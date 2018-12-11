---
title: Azure Integration Services를 사용한 엔터프라이즈 통합
description: 이 아키텍처 참조에서는 Azure Logic Apps 및 Azure API Management를 사용하여 간단한 엔터프라이즈 통합 패턴을 구현하는 방법을 보여줍니다.
services: logic-apps
author: mattfarm
ms.author: mattfarm
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 12/03/2018
ms.openlocfilehash: c8aa3f8b736fabd1a6701778f22a7eec9bf46ee7
ms.sourcegitcommit: e7e0e0282fa93f0063da3b57128ade395a9c1ef9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52919115"
---
# <a name="basic-enterprise-integration-on-azure"></a>Azure에서 기본 엔터프라이즈 통합

이 참조 아키텍처는 [Azure Integration Services][integration-services]를 사용하여 엔터프라이즈 백 엔드 시스템에 대한 호출을 오케스트레이션합니다. 백 엔드 시스템에는 SaaS(Software as a Service) 시스템, Azure 서비스 및 기업의 기존 웹 서비스가 포함될 수 있습니다.

Azure Integration Services는 응용 프로그램과 데이터를 통합하기 위한 서비스 컬렉션입니다. 이 아키텍처는 워크플로를 오케스트레이션하는 [Logic Apps][logic-apps] 및 API 카탈로그를 만드는 [API Management][apim] 서비스를 사용합니다. 이 아키텍처는 백 엔드 서비스에 대한 비동기 호출로 인해 워크플로가 트리거되는 기본 통합 시나리오에 충분합니다. [큐 및 이벤트](./queues-events.md)를 사용하는 더 정교한 아키텍처는 이러한 기본 카키텍처를 기반으로 합니다. 

![아키텍처 다이어그램 - 간단한 엔터프라이즈 통합](./_images/simple-enterprise-integration.png)

## <a name="architecture"></a>아키텍처

이 아키텍처의 구성 요소는 다음과 같습니다.

- **백 엔드 시스템**. 다이어그램의 오른쪽에는 기업에서 배포한 또는 사용하는 다양한 백 엔드 시스템이 나와 있습니다. SaaS 시스템, 다른 Azure 서비스, REST 또는 SOAP 엔드포인트를 공개하는 웹 서비스 등이 여기에 포함됩니다.

- **Azure Logic Apps**. [Logic Apps][logic-apps]는 응용 프로그램, 데이터 및 서비스를 통합하는 엔터프라이즈 워크플로를 빌드하기 위한 서버리스 플랫폼입니다. 이 아키텍처는 논리 앱이 HTTP 요청에 의해 트리거됩니다. 워크플로를 중첩하여 더 복잡한 오케스트레이션도 가능합니다. Logic Apps는 일반적으로 사용되는 서비스와 통합되는 [커넥터][logic-apps-connectors]입니다. Logic Apps는 수백 개의 커넥터를 제공하며, 사용자 지정 커넥터를 만들 수도 있습니다.

- **Azure API Management**. [API Management][apim]는 HTTP API 카탈로그를 게시하여 재사용 및 검색 기능의 수준을 올리는 관리 서비스입니다. API Management는 두 가지 관련 구성 요소로 구성됩니다.

    - **API 게이트웨이**. API 게이트웨이는 HTTP 호출을 받아서 백 엔드로 라우팅합니다. 

    - **개발자 포털**. Azure API Management의 각 인스턴스는 [개발자 포털][apim-dev-portal]에 대한 액세스를 제공합니다. 개발자는 이 포털을 통해 API 호출과 관련된 문서 및 샘플 코드에 액세스할 수 있습니다. 개발자 포털에서 API를 테스트할 수도 있습니다.

    이 아키텍처에는 [논리 앱을 API로 가져와서][apim-logic-app] 복합 API를 빌드합니다. WSDL 사양에서 [OpenAPI(Swagger) 사양 가져오기][apim-openapi] 또는 [SOAP API 가져오기][apim-soap]를 통해 기존 웹 서비스를 가져올 수도 있습니다. 

    API 게이트웨이를 사용하여 백 엔드에서 프런트 엔드 클라이언트를 분리할 수 있습니다. 예를 들어 URL을 다시 작성하거나, 요청이 백 엔드에 도달하기 전에 변환할 수 있습니다. 또한 인증, CORS(원본 간 리소스 공유) 지원, 응답 캐싱 등 여러 교차 편집 문제를 처리합니다.

- **Azure DNS**. [Azure DNS][dns]는 DNS 도메인에 대한 호스팅 서비스입니다. Azure DNS는 Microsoft Azure 인프라를 사용하여 이름 확인을 제공합니다. Azure에서 도메인을 호스팅하면 다른 Azure 서비스에 사용하는 것과 동일한 자격 증명, API, 도구 및 결제(청구) 정보를 사용하여 DNS 레코드를 관리할 수 있습니다. 사용자 지정 도메인 이름(예: contoso.com)을 사용하려면 사용자 지정 도메인 이름을 IP 주소에 매핑하는 DNS 레코드를 만듭니다. 자세한 내용은 [API Management에서 사용자 지정 도메인 이름 구성][apim-domain]을 참조하세요.

- **Azure AD(Azure Active Directory)**. [Azure AD][aad]를 사용하여 API 게이트웨이를 호출하는 클라이언트를 인증합니다. Azure AD는 OIDC(OpenID Connect) 프로토콜을 지원합니다. 클라이언트는 Azure AD에서 액세스 토큰을 획득하고, API 게이트웨이는 [토큰의 유효성을 검사][apim-jwt]하여 요청에 권한을 부여합니다. API Management 표준 또는 프리미엄 계층을 사용하는 경우 Azure AD로 개발자 포털에 대한 액세스를 보호할 수도 있습니다.

## <a name="recommendations"></a>권장 사항

여러분의 요구 사항이 여기에 제시된 일반 아키텍처와 다를 수 있습니다. 이 섹션의 권장 사항을 시작점으로 사용합니다.

### <a name="api-management"></a>API Management

API Management 기본, 표준 또는 프리미엄 계층을 사용합니다. 이러한 계층은 프로덕션 SLA(서비스 수준 계약)를 제공하고 Azure 지역 내에서 규모 확장을 지원합니다. API Management의 처리량은 *단위*로 측정됩니다. 가격 책정 계층마다 확장 가능한 최대 규모가 있습니다. 프리미엄 계층은 여러 Azure 지역에 걸친 규모 확장도 지원합니다. 기능 집합 및 필요한 처리량의 수준을 기준으로 계층을 선택합니다. 자세한 내용은 [API Management 가격 책정][apim-pricing] 및 [Azure API Management 인스턴스의 용량][apim-capacity]을 참조하세요.

각 Azure API Management 인스턴스는 기본 도메인 이름을 가지며, 이 이름은 `azure-api.net`의 하위 도메인입니다(예: `contoso.azure-api.net`). 조직에 [사용자 지정 도메인][apim-domain]을 구성하는 방안을 고려해 보세요.

### <a name="logic-apps"></a>Logic Apps 

Logic Apps는 짧은 응답 대기 시간이 필요하지 않은 시나리오에 가장 적합합니다(예: 비동기 또는 반장기 실행 API 호출). 사용자 인터페이스를 차단하는 호출과 같이 짧은 대기 시간이 필요한 경우 다른 기술을 사용하세요. 예를 들어, Azure App Service에 배포되는 Azure Functions 또는 Web API를 사용합니다. API 사용자에게 API를 향하도록 API Management를 사용합니다.

### <a name="region"></a>지역

네트워크 대기 시간을 최소화할 수 있도록, API Management와 Logic Apps를 동일한 지역에 프로비전합니다. 일반적으로 사용자(또는 백 엔드 서비스)와 가장 가까운 지역을 선택합니다.

리소스 그룹에도 지역이 있습니다. 이 지역은 배포 메타 데이터를 저장할 위치와 배포 템플릿을 실행할 위치를 지정합니다. 배포 중 가용성을 향상 시키려면 리소스 그룹과 리소스를 같은 지역에 배치합니다.

## <a name="scalability-considerations"></a>확장성 고려 사항

상황에 따라 [캐싱 정책][apim-caching]을 추가하여 API Management의 확장성을 높일 수 있습니다. 또한 캐싱은 백 엔드 서비스의 로드를 줄이는 데 도움이 됩니다.

더 많은 용량을 제공하기 위해 Azure 지역 내에서 Azure API Management 기본, 표준 및 프리미엄 계층을 확장할 수 있습니다. **메트릭** 메뉴에서 서비스의 사용량을 분석하려면 **용량 메트릭** 옵션을 선택한 다음, 적절하게 확장 또는 축소합니다. 업그레이드 또는 크기 조정 프로세스를 적용하는 데는 15~45분 정도 소요될 수 있습니다.

API Management 서비스 크기 조정에 대한 권장 사항:

- 크기를 조정하는 경우 트래픽 패턴을 고려합니다. 더욱 일시적인 트래픽 패턴을 사용하는 고객은 더 많은 용량이 필요합니다.

- 66%보다 큰 일관된 용량은 확장(스케일 업)이 필요할 수 있습니다.

- 20% 아래의 일관된 용량은 축소(스케일 다운)가 필요할 수 있습니다.

- 프로덕션 환경에서 부하를 사용하도록 설정하기 전에 항상 대표적인 부하를 사용하여 API Management 서비스의 부하를 테스트합니다.

프리미엄 계층을 사용하면 API Management 인스턴스를 여러 Azure 지역에 걸쳐 확장할 수 있습니다. 이렇게 하면 API Management의 SLA를 높일 수 있으며, 여러 지역에서 사용자와 가까운 서비스를 프로비전할 수 있습니다.


Logic Apps 서버리스 모델은 관리자가 서비스 확장성을 계획할 필요가 없음을 의미합니다. 서비스가 수요를 충족하도록 자동으로 확장됩니다.

## <a name="availability-considerations"></a>가용성 고려 사항

각 서비스의 SLA를 검토합니다.

- [API Management SLA][apim-sla]
- [Logic Apps SLA][logic-apps-sla]

프리미엄 계층을 사용하여 두 개 이상의 지역에 API Management를 배포하면 더 높은 SLA를 얻을 수 있습니다. [API Management 가격 책정][apim-pricing]을 참조하세요.

### <a name="backups"></a>Backup

API Management 구성을 주기적으로 [백업][apim-backup]합니다. 서비스가 배포된 곳과 다른 위치 또는 Azure 지역에 백업 파일을 저장합니다. [RTO][rto]에 따라 재해 복구 전략을 선택합니다.

* 재해 복구 이벤트에서 새 API Management 인스턴스를 프로비전하고, 백업을 새 인스턴스로 복원하고, DNS 레코드를 다시 지정합니다.

* 다른 Azure 지역에서 API Management 서비스의 패시브 인스턴스를 유지합니다. 주기적으로 백업을 해당 인스턴스에 복원하여 액티브 서비스와 동기화를 유지합니다. 재해 복구 이벤트 중 서비스를 복원하려면 DNS 레코드만 다시 지정하면 됩니다. 이 방법은 패시브 인스턴스 비용이 발생하므로 비용이 증가하지만, 복구 시간이 단축됩니다. 

논리 앱의 경우 코드로 구성(configuration-as-code) 방법으로 백업 및 복원하는 것이 좋습니다. 논리 앱은 서버리스이므로 Azure Resource Manager 템플릿에서 신속하게 다시 만들 수 있습니다. 원본 제어에 템플릿을 저장하고, 템플릿을 CI/CD(지속적인 통합/지속적인 배포) 프로세스와 통합합니다. 재해 복구 이벤트 발생 시, 템플릿을 새 지역에 배포합니다.

다른 지역에 논리 앱을 배포하는 경우 API Management에서 구성을 업데이트합니다. 기본 PowerShell 스크립트를 사용하여 API의 **백 엔드** 속성을 업데이트할 수 있습니다.

## <a name="manageability-considerations"></a>관리 효율성 고려 사항

프로덕션, 개발 및 테스트 환경에 대해 별도의 리소스 그룹을 만듭니다. 별도의 리소스 그룹을 만들면 배포 관리, 테스트 배포 삭제, 액세스 권한 할당 등이 더 간단해집니다.

리소스 그룹에 리소스를 할당할 때는 다음 요소를 고려합니다.

* **수명 주기**. 일반적으로 수명 주기가 같은 리소스를 동일한 리소스 그룹에 배치합니다.

* **액세스**. RBAC([역할 기반 액세스 제어][rbac])를 사용하여 그룹의 리소스에 액세스 정책을 적용할 수 있습니다.

* **청구**. 리소스 그룹에 대한 롤업 비용을 확인할 수 있습니다.

* **API Management 가격 책정 계층**. 개발 및 테스트 환경에는 개발자 계층을 사용합니다. 사전 프로덕션 기간에 비용을 최소화하려면 프로덕션 환경의 복제본을 배포하고 테스트를 실행한 다음, 종료합니다.

### <a name="deployment"></a>배포

[Azure Resource Manager 템플릿][arm]을 사용하여 Azure 리소스를 배포합니다. 템플릿을 사용하면 PowerShell 또는 Azure CLI를 통해 배포를 쉽게 자동화할 수 있습니다.

API Management 및 모든 개별 논리 앱을 별도의 자체 Resource Manager 템플릿에 배치합니다. 별도의 템플릿을 사용하면, 소스 제어 시스템에 리소스를 저장할 수 있습니다. 템플릿을 CI/CD 프로세스의 일부로 함께 또는 개별적으로 배포할 수 있습니다.

### <a name="versions"></a>버전

논리 앱의 구성을 변경하거나 Resource Manager 템플릿을 통해 업데이트를 배포할 때마다 Azure는 해당 버전의 복사본을 유지하고 실행 기록이 있는 모든 버전을 유지합니다. 이러한 버전을 사용하여 기록 변경 내용을 추적하거나 버전을 논리 앱의 현재 구성으로 승격할 수 있습니다. 예를 들어 논리 앱을 이전 버전으로 롤백할 수 있습니다.

API Management는 각각 별개이지만 상호 보완 관계인 두 가지 버전 관리 개념을 지원합니다.

* *버전*을 통해 API 소비자는 요구 사항에 맞는 API 버전을 선택할 수 있습니다(예: v1, v2, 베타 또는 프로덕션).

* *수정 버전*을 통해 API 관리자는 API에서 호환성이 손상되지 않는 변경 작업을 수행하고 변경 내용을 배포할 수 있으며, 변경 로그를 통해 API 소비자에게 변경 관련 정보를 제공할 수 있습니다.

개발 환경에서 수정을 만들고 Resource Manager 템플릿을 사용하여 다른 환경에서 해당 변경 내용을 배포할 수 있습니다. 자세한 내용은 [여러 버전의 API 게시][apim-versions]를 참조하세요.

또한 변경 내용을 최신 버전으로 만들어서 사용자에게 제공하기 전에 수정 버전을 사용하여 미리 API를 테스트할 수 있습니다. 그러나 부하 테스트 또는 통합 테스트에는 이 방법을 권장하지 않습니다. 대신에 별도의 테스트 또는 사전 프로덕션 환경을 사용합니다.

## <a name="diagnostics-and-monitoring"></a>진단 및 모니터링

API Management와 Logic Apps 둘 다 [Azure Monitor][monitor]를 사용하여 운영을 모니터링합니다. Azure Monitor는 각 서비스에 구성된 메트릭을 기반으로 하여 정보를 제공하고 기본적으로 활성화됩니다. 자세한 내용은 다음을 참조하세요.

- [게시된 API 모니터링][apim-monitor]
- [상태 모니터링, 진단 로깅 설정, Azure Logic Apps에 대한 경고 설정][logic-apps-monitor]

또한 각 서비스에는 다음 옵션이 있습니다.

* 보다 심층적인 분석 및 대시보드 구성이 가능하도록, Logic Apps 로그를 [Azure Log Analytics][logic-apps-log-analytics]로 보낼 수 있습니다.

* DevOps 모니터링이 가능하도록, API Management에 대해 Azure Application Insights를 구성할 수 있습니다.

* API Management는 [사용자 지정 API 분석용 Power BI 솔루션 템플릿][apim-pbi]을 지원합니다. 자체 분석 솔루션을 만들기 위해 이 솔루션 템플릿을 사용할 수 있습니다. 비즈니스 사용자의 경우 Power BI를 통해 보고서를 사용할 수 있습니다.

## <a name="security-considerations"></a>보안 고려 사항

이 목록은 모든 보안 모범 사례를 완벽하게 설명하지는 않지만, 이 아키텍처에 한정적으로 적용되는 몇 가지 보안 고려 사항이 있습니다.

* Azure API Management 서비스는 고정된 공용 IP 주소를 사용합니다. API Management의 IP 주소로만 Logic Apps 엔드포인트를 호출할 수 있도록 액세스가 제한됩니다. 자세한 내용은 [받는 IP 주소 제한][logic-apps-restrict-ip]을 참조하세요.

* 사용자에게 적절한 액세스 수준이 있는지 확인하려면 RBAC(역할 기반 액세스 제어)를 사용합니다.

* OAuth 또는 OpenID Connect를 사용하여 API Management에서 공용 API 엔드포인트를 보호합니다. 공용 API 엔드포인트를 보호하려면 ID 공급자를 구성하고, JWT(JSON Web Token) 유효성 검사 정책을 추가합니다. 자세한 내용은 [Azure Active Directory 및 API Management에서 OAuth 2.0을 사용하여 API 보호][apim-oauth]를 참조하세요.

* 상호 인증서를 사용하여 API Management에서 백 엔드 서비스에 연결합니다.

* API Management API에 HTTPS를 적용합니다.

### <a name="storing-secrets"></a>비밀 저장

암호, 액세스 키 또는 연결 문자열을 소스 제어로 체크 인하지 마세요. 이러한 값이 필요한 경우 적절한 기술을 사용하여 이러한 값을 보호하고 배포합니다. 

커넥터 내에서 만들 수 없는 중요한 값이 논리 앱에 필요한 경우 이러한 값을 Azure Key Vault에 저장하고 Resource Manager 템플릿에서 참조합니다. 각 환경에 대한 배포 템플릿 매개 변수 및 매개 변수 파일을 사용합니다. 자세한 내용은 [워크플로 내에서 매개 변수 및 입력 보안][logic-apps-secure]을 참조하세요.

API Management는 *명명된 값* 또는 *속성*이라는 개체를 사용하여 비밀을 관리합니다. 이러한 개체는 API Management 정책을 통해 액세스할 수 있는 값을 안전하게 저장합니다. 자세한 내용은 [Azure API Management 정책에 명명된 값을 사용하는 방법][apim-properties]을 참조하세요.

## <a name="cost-considerations"></a>비용 고려 사항

실행되고 있는 모든 API Management 인스턴스에 대한 요금이 청구됩니다. 시스템을 강화했는데 그 정도 성능 수준이 항상 필요한 것은 아닌 경우 수동으로 규모 축소하거나 [자동 크기 조정][apim-autoscale]을 구성합니다.

Logic Apps는 [서버리스](/azure/logic-apps/logic-apps-serverless-overview) 모델을 사용합니다. 청구는 작업 및 커넥터 실행에 따라 계산됩니다. 자세한 내용은 [Logic Apps 가격 책정](https://azure.microsoft.com/pricing/details/logic-apps/)을 참조하세요. 현재 Logic Apps에 대한 계층 고려 사항은 없습니다.

## <a name="next-steps"></a>다음 단계

큰 안정성과 확장성을 위해 백 엔드 시스템을 분리하는 메시지 큐 및 이벤트를 사용합니다. 이 패턴은 이 시리즈의 다음 참조 아키텍처에 나와 있습니다([메시지 큐 및 이벤트를 사용한 엔터프라이즈 통합](./queues-events.md)).

<!-- links -->

[aad]: /azure/active-directory
[apim]: /azure/api-management
[apim-autoscale]: /azure/api-management/api-management-howto-autoscale
[apim-backup]: /azure/api-management/api-management-howto-disaster-recovery-backup-restore
[apim-caching]: /azure/api-management/api-management-howto-cache
[apim-capacity]: /azure/api-management/api-management-capacity
[apim-dev-portal]: /azure/api-management/api-management-key-concepts#a-namedeveloper-portal-a-developer-portal
[apim-domain]: /azure/api-management/configure-custom-domain
[apim-jwt]: /azure/api-management/policies/authorize-request-based-on-jwt-claims
[apim-logic-app]: /azure/api-management/import-logic-app-as-api
[apim-monitor]: /azure/api-management/api-management-howto-use-azure-monitor
[apim-oauth]: /azure/api-management/api-management-howto-protect-backend-with-aad
[apim-openapi]: /azure/api-management/import-api-from-oas
[apim-pbi]: http://aka.ms/apimpbi
[apim-pricing]: https://azure.microsoft.com/pricing/details/api-management/
[apim-properties]: /azure/api-management/api-management-howto-properties
[apim-sla]: https://azure.microsoft.com/support/legal/sla/api-management/
[apim-soap]: /azure/api-management/import-soap-api
[apim-versions]: /azure/api-management/api-management-get-started-publish-versions
[arm]: /azure/azure-resource-manager/resource-group-authoring-templates
[dns]: /azure/dns/
[integration-services]: https://azure.microsoft.com/product-categories/integration/
[logic-apps]: /azure/logic-apps/logic-apps-overview
[logic-apps-connectors]: /azure/connectors/apis-list
[logic-apps-log-analytics]: /azure/logic-apps/logic-apps-monitor-your-logic-apps-oms
[logic-apps-monitor]: /azure/logic-apps/logic-apps-monitor-your-logic-apps
[logic-apps-restrict-ip]: /azure/logic-apps/logic-apps-securing-a-logic-app#restrict-incoming-ip-addresses
[logic-apps-secure]: /azure/logic-apps/logic-apps-securing-a-logic-app#secure-parameters-and-inputs-within-a-workflow
[logic-apps-sla]: https://azure.microsoft.com/support/legal/sla/logic-apps
[monitor]: /azure/azure-monitor/overview
[rbac]: /azure/role-based-access-control/overview
[rto]: ../../resiliency/index.md#rto-and-rpo