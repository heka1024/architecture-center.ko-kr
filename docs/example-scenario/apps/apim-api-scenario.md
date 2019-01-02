---
title: Azure의 API 기반 아키텍처에 레거시 웹 애플리케이션 마이그레이션
description: Azure API Management를 사용하여 레거시 웹 애플리케이션을 현대화합니다.
author: begim
ms.date: 09/13/2018
ms.custom: fasttrack
ms.openlocfilehash: ea063653b4962e42cbec7f9d98c16e22e987efd1
ms.sourcegitcommit: a0e8d11543751d681953717f6e78173e597ae207
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/06/2018
ms.locfileid: "53004711"
---
# <a name="migrating-a-legacy-web-application-to-an-api-based-architecture-on-azure"></a>Azure의 API 기반 아키텍처에 레거시 웹 애플리케이션 마이그레이션

여행 업계의 한 전자상거래 회사에서 레거시 브라우저 기반 소프트웨어 스택을 현대화하려고 합니다. 기존 스택은 대부분 모놀리식이고, 최근 프로젝트에 [SOAP 기반 HTTP 서비스][soap]가 일부 존재합니다. 이 회사는 내부에서 개발한 지적 재산권의 일부를 수익 창출에 이용하여 추가 수익원을 만들고자 합니다.

프로젝트의 목표는 기술적인 문제를 해결하고, 지속적인 유지 관리를 개선하고, 회귀 버그를 줄여 기능 개발 속도를 높이는 것입니다. 프로젝트에서는 위험을 피하기 위해 반복 프로세스를 사용하고, 일부 단계가 병렬로 수행됩니다.

* 개발 팀은 VM에 호스트되는 관계형 데이터베이스로 구성된 애플리케이션 백 엔드를 현대화할 예정입니다.
* 사내 개발 팀은 새 HTTP API를 통해 공개될 새 비즈니스 기능을 개발할 예정입니다.
* 계약 개발 팀은 Azure에 호스트되는 새로운 브라우저 기반 UI를 빌드할 예정입니다.

새 애플리케이션 기능은 단계별로 제공될 예정입니다. 이 기능은 현재 전자상거래 비즈니스를 구동하는 기존의 브라우저 기반 클라이언트-서버 UI 기능(온-프레미스에 호스트됨)을 점진적으로 대체할 것입니다.

관리 팀은 불필요한 현대화를 원하지 않습니다. 범위 및 비용도 계속 제어할 수 있기를 원합니다. 이렇게 하기 위해, 기존 HTTP SOAP 서비스를 유지하기로 결정했습니다. 또한 기존 UI 변경을 최소화하려 합니다. [Azure APIM(API Management)][apim]을 활용하여 여러 프로젝트 요구 사항 및 제약 조건을 해결할 수 있습니다.

## <a name="architecture"></a>아키텍처

![아키텍처 다이어그램][architecture]

새 UI는 Azure에 PaaS(Platform as a Service)로 호스트되며, 기존 및 새 HTTP API를 모두 사용합니다. 이러한 API는 보다 우수한 성능, 보다 간편한 통합, 향후 확장성을 제공하도록 잘 설계된 인터페이스 집합을 제공합니다.

### <a name="components-and-security"></a>구성 요소 및 보안

1. 기존 온-프레미스 웹 애플리케이션은 계속해서 기존 온-프레미스 웹 서비스를 직접 사용할 것입니다.
2. 기존 웹앱에서 기존 HTTP 서비스로 보내는 호출은 변경되지 않습니다. 이러한 호출은 회사 네트워크 내부용입니다.
3. 인바운드 호출은 Azure에서 기존 내부 서비스로 수행됩니다.
    * 보안 팀은 [보안 전송(HTTPS/SSL)을 사용하여][apim-ssl] APIM 인스턴스의 트래픽이 회사 방화벽을 통과하여 기존 온-프레미스 서비스로 이동하는 것을 허용합니다.
    * 운영 팀은 APIM 인스턴스에서 서비스로 오는 인바운드 호출만 허용할 것입니다. 이 요구 사항은 회사 네트워크 경계 내에서 [APIM 인스턴스의 IP 주소를 허용 목록에 추가][apim-whitelist-ip]하여 충족됩니다.
    * 새 모듈은 온-프레미스 HTTP 서비스 요청 파이프라인에 구성되며(**오직** 외부에서 시작된 연결에 따라 작동하도록), [APIM이 제공하는 인증서][apim-mutualcert-auth]의 유효성을 검사합니다.
1. 새 API의 특징:
    * APIM 인스턴스를 통해서만 표시되며, API 외관을 제공합니다. 새 API는 직접 액세스할 수 없습니다.
    * [Azure PaaS Web API 앱][azure-api-apps]으로 개발 및 게시됩니다.
    * [APIM VIP][apim-faq-vip]만 수락하도록 허용 목록에 추가됩니다([웹앱 설정][azure-appservice-ip-restrict]을 통해).
    * 보안 전송/SSL이 켜진 상태로 Azure 웹앱에 호스트됩니다.
    * 권한 부여가 설정되며, Azure Active Directory 및 OAuth2를 사용하여 [Azure App Service를 통해 제공][azure-appservice-auth]됩니다.
2. 새 웹 브라우저 기반 애플리케이션은 기존 HTTP API와 새 API **모두**를 위한 Azure API Management 인스턴스를 사용합니다.

APIM 인스턴스는 레거시 HTTP 서비스를 새 API 계약에 매핑하도록 구성됩니다. 이렇게 하면 새 UI가 레거시 서비스/API 및 새 API 집합과의 통합을 인식하지 못합니다. 나중에 프로젝트 팀이 점진적으로 기능을 새 API로 이식하고 원래 서비스를 사용 중지할 것입니다. 이러한 변경 내용은 APIM 구성 내에서 처리되며, 프런트 엔드 UI는 영향을 받지 않으므로 다시 개발할 필요가 없습니다.

### <a name="alternatives"></a>대안

* 조직에서 레거시 애플리케이션을 호스트하는 VM을 포함하여 인프라 전체를 Azure로 이동하려는 경우 APIM는 주소 지정이 가능한 HTTP 엔드포인트의 외관으로 작동할 수 있으므로 여전히 유용한 옵션입니다.
* 고객이 기존 엔드포인트를 비공개로 유지하여 공개하지 않기로 결정한 경우 고객의 API Management 인스턴스를 [Azure VNet(Virtual Network)][azure-vnet]에 연결할 수 있습니다.
  * 배포된 Azure Virtual Network에 연결된 [Azure 리프트 앤 시프트 시나리오][azure-vm-lift-shift]에서, 고객은 사설 IP 주소를 통해 백 엔드 서비스의 주소를 직접 지정할 수 있습니다.
  * 온-프레미스 시나리오에서 API Management 인스턴스는 [Azure VPN 게이트웨이 및 사이트 간 IPSec VPN 연결][azure-vpn] 또는 [ExpressRoute][azure-er]를 통해 내부 서비스에 비공개로 연결하여 이 [하이브리드 Azure 및 온-프레미스 시나리오][azure-hybrid]를 구현할 수 있습니다.
* 내부 모드에서 API Management 인스턴스를 배포하여 API Management 인스턴스를 비공개로 유지할 수 있습니다. 그런 다음, 배포에 [Azure Application Gateway][azure-appgw]를 사용하여 일부 API에 공개 액세스를 설정하고 나머지는 내부로 유지할 수 있습니다. 자세한 내용은 [내부 모드에서 VNET에 APIM 연결][apim-vnet-internal]을 참조하세요.

> [!NOTE]
> API Management를 VNET에 연결하는 일반적인 방법은 [여기를 참조][apim-vnet]하세요.

### <a name="availability-and-scalability"></a>가용성 및 확장성

* 가격 책정 계층을 선택하고 단위를 추가하여 Azure API Management를 [규모 확장][apim-scaleout]할 수 있습니다.
* [자동 크기 조정을 통해 자동으로][apim-autoscale] 크기가 조정되기도 합니다.
* [여러 지역에 배포][apim-multi-regions]하면 장애 조치(failover) 옵션을 사용할 수 있으며 [프리미엄 계층][apim-pricing]에서 수행할 수 있습니다.
* [Azure Application Insights와 통합][azure-apim-ai]하는 방안을 고려해 보세요. 이렇게 하면 모니터링용 [Azure Monitor][azure-mon]을 통해 메트릭이 표시됩니다.

## <a name="deployment"></a>배포

시작하려면 [포털에서 Azure API Management 인스턴스를 만듭니다.][apim-create]

또는 해당 사용 사례와 일치하는 기존 Azure Resource Manager [빠른 시작 템플릿][azure-quickstart-templates-apim]에서 선택할 수도 있습니다.

## <a name="pricing"></a>가격

API Management는 개발자, 기본, 표준 및 프리미엄의 네 가지 계층으로 제공됩니다. 계층 간 차이점은 [Azure API Management 가격 책정 지침][apim-pricing]에서 확인할 수 있습니다.

고객은 단위를 추가하고 제거하여 API Management 크기를 조정할 수 있습니다. 각 단위의 용량은 해당 계층에 따라 다릅니다.

> [!NOTE]
> 개발자 계층은 평가판 API Management 기능의 평가에 사용할 수 있습니다. 개발자 계층은 프로덕션에 사용할 수 없습니다.

예상 비용을 살펴보고 배포 요구 사항에 맞게 사용자 지정하려면 [Azue 가격 책정 계산기][pricing-calculator]에서 배율 단위 및 App Service 인스턴스의 수를 수정하면 됩니다.

## <a name="related-resources"></a>관련 리소스

방대한 Azure API Management [설명서 및 참조 문서][apim]를 검토하세요.


<!-- links -->
[architecture]: ./media/architecture-apim-api-scenario.png
[apim-create]: /azure/api-management/get-started-create-service-instance
[apim-git]: /azure/api-management/api-management-configuration-repository-git
[apim-multi-regions]: /azure/api-management/api-management-howto-deploy-multi-region
[apim-autoscale]: /azure/api-management/api-management-howto-autoscale
[apim-scaleout]: /azure/api-management/upgrade-and-scale
[azure-apim-ai]: /azure/api-management/api-management-howto-app-insights
[azure-ai]: /azure/application-insights/
[azure-mon]: /azure/monitoring-and-diagnostics/monitoring-overview
[azure-appgw]: /azure/application-gateway/application-gateway-introduction
[apim-vnet-internal]: /azure/api-management/api-management-howto-integrate-internal-vnet-appgateway
[apim-vnet]: /azure/api-management/api-management-using-with-vnet
[azure-hybrid]: /azure/architecture/reference-architectures/hybrid-networking/
[azure-er]: /azure/expressroute/expressroute-introduction
[azure-vpn]: /azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal
[azure-vnet]: /azure/virtual-network/virtual-networks-overview
[azure-appservice-auth]: /azure/app-service/app-service-authentication-overview#identity-providers
[apim-faq-vip]: /azure/api-management/api-management-faq#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules
[azure-appservice-ip-restrict]: /azure/app-service/app-service-ip-restrictions
[azure-api-apps]: /azure/app-service/
[apim-ssl]: /azure/api-management/api-management-howto-manage-protocols-ciphers
[apim-mutualcert-auth]: /azure/api-management/api-management-howto-mutual-certificates
[apim-whitelist-ip]: /azure/api-management/api-management-faq#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules
[anti-corruption-layer-pattern]: /azure/architecture/patterns/anti-corruption-layer
[apim]: /azure/api-management/api-management-key-concepts
[apim-api-design-guidance]: /azure/architecture/best-practices/api-design
[visualstudio-youtube-solid-design]: https://youtu.be/agkWYPUcLpg
[azure-vm-lift-shift]: https://azure.microsoft.com/resources/azure-virtual-datacenter-lift-and-shift-guide/
[standard-pricing-calc]: https://azure.com/e/
[premium-pricing-calc]: https://azure.com/e/
[apim-pricing]: https://azure.microsoft.com/pricing/details/api-management/
[azure-quickstart-templates-apim]: https://azure.microsoft.com/resources/templates/?term=API+Management&pageNumber=1
[soap]: https://en.wikipedia.org/wiki/SOAP
[pricing-calculator]: https://azure.com/e/0e916a861fac464db61342d378cc0bd6
