---
title: 'CAF: CISO 준비'
description: CISO가 클라우드 사용을 준비하는 방법
author: BrianBlanchard
ms.date: 10/03/2018
ms.openlocfilehash: a4535163990797decdacdacdcb6a33f0118366e9
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58238351"
---
# <a name="ciso-cloud-readiness-guide"></a>CISO 클라우드 준비 가이드

Azure CAF(클라우드 채택 프레임워크)와 같은 Microsoft 지침은 이 문서를 통해 지원되는 수많은 기업의 고유한 보안 제약 조건을 안내하거나 결정하기 위해 제공되는 것이 아닙니다. 즉, 클라우드로 이전할 때는 클라우드 기술이 CISO(Chief Information Security Officer/Chief Information Security Office)의 역할을 대신하는 것이 아니라 클라우드 기술을 통해 CISO와 CISO의 담당 부서가 클라우드에 대해 철저하게 파악하고 더욱 긴밀하게 통합된다고 할 수 있습니다. 이 가이드에서는 독자가 CISO 프로세스에 대해 잘 알고 있으며 클라우드 변환을 진행할 수 있도록 해당 프로세스를 현대화하는 방법을 모색 중이라고 가정합니다.

클라우드를 채택하면 기존의 IT 환경에서는 고려 대상에도 포함되지 않았던 서비스를 사용할 수 있게 됩니다. 일반적으로는 애플리케이션 개발 팀이나 기존에는 프로덕션 배포를 진행해 본 적이 없는 기타 IT 팀이 셀프 서비스 또는 자동화된 배포를 실행합니다. 그리고 개별 직원들이 셀프 서비스 기능을 활용할 수 있는 조직도 있습니다. 따라서 온-프레미스 환경에서는 필요하지 않았던 새로운 보안 요구 사항이 적용될 수 있습니다. 또한 중앙 집중식 보안은 적용하기가 더 까다롭기 때문에 실무 팀과 IT 팀이 공동으로 보안을 관리하는 경우가 많습니다. 이 문서에서는 CISO가 해당 방식을 활용할 수 있도록 준비하고 증분 방식 거버넌스에 참여하는 데 도움이 되는 정보를 제공합니다.

## <a name="how-can-the-ciso-prepare-for-the-cloud"></a>CISO가 클라우드 사용을 준비하는 방법

대다수 정책과 마찬가지로 조직 내의 보안 및 거버넌스 정책도 유기적으로 확장되는 경향이 있습니다. 보안 인시던트가 발생하면 조직에서는 해당 인시던트를 사용자에게 알리고 반복적인 인시던트 발생 가능성을 줄이기 위해 정책을 작성합니다. 이러한 방식은 자연스럽기는 하지만, '정책 블로트'가 발생하며 기술에 대한 의존도가 높아집니다. 클라우드 변환 경험에서는 정책을 현대화하고 재설정할 수 있는 고유한 기회가 제공됩니다. CISO는 변환 경험을 준비하면서 [정책 검토](./what-is-a-cloud-policy-review.md)의 기본 관련자 역할을 맡아 중요한 가치를 즉시 제시할 수 있습니다.

이러한 검토에서 CISO의 역할은 기존 정책/규정 준수의 제약 조건과 클라우드 공급자의 개선된 보안 상태 간에 적절한 균형을 유지하는 것입니다. 이 검토의 진행 상황은 여러 가지 형식으로 측정할 수 있는데, 클라우드 공급자에게 오프로드해도 안전한 보안 정책의 수를 기준으로 측정하는 경우가 많습니다.

**보안 위험의 이전**: 기업은 서비스를 IaaS(Infrastructure as a Service) 호스팅 모델로 이동하는 과정에서 하드웨어 프로비전과 관련한 직접적인 위험은 많지 않다고 가정합니다. 하지만 해당 위험은 제거되는 것이 아니라 클라우드 공급업체로 이전되는 것입니다. 클라우드 공급업체의 하드웨어 프로비전 방식에서 안전하게 반복 가능한 프로세스를 통해 위험이 동일한 수준으로 완화되어야 하드웨어 프로비전 실행의 위험이 기업 정책에서 제거된다고 할 수 있습니다. 그러면 기업 정책을 해당 프로세스의 유효성 검사를 위한 새로운 정책으로 대체할 수 있습니다. 이 경우 실행 위험이 재당할되므로 전반적인 보안 위험은 감소하게 됩니다.

PaaS(Platform as a Service) 또는 SaaS(Software as a Service) 모델을 통합하기 위해 솔루션을 스택의 상위 위치로 이동하는 과정에서 위험을 추가로 완화/이전/대체할 수 있습니다. 위험이 클라우드 공급자에게 안전하게 이전되면 보안 정책 또는 기타 규정 준수 정책을 실행/모니터링/적용하는 비용도 안전하게 줄일 수 있습니다.

**성장을 우선적으로 고려하는 사고방식**: 기업과 기술 구현업체 모두 변화에 대한 두려움을 가질 수 있습니다. 하지만 CISO가 조직에서 성장을 우선적으로 고려하는 사고방식으로의 전환을 주도하면 변화 과정에서 자연스럽게 발생하는 두려움이 안전 및 정책 규정 준수에 대한 관심으로 바뀌게 됩니다. 즉, 성장을 우선적으로 고려하는 사고방식으로 [정책 검토](./what-is-a-cloud-policy-review.md), 변환 경험, 심지어는 단순한 구현 검토에라도 참여하는 팀은 공정하면서도 효율적으로 관리 가능한 위험 프로필을 그대로 유지하면서 클라우드로 빠르게 이전할 수 있습니다.

## <a name="resources-for-the-chief-information-security-officer"></a>Chief Information Security Officer용 리소스

성장을 우선적으로 고려하는 사고방식으로 [정책 검토](./what-is-a-cloud-policy-review.md)를 진행하려면 클라우드에 대한 지식이 반드시 필요합니다. CISO는 다음 리소스를 통해 Microsoft Azure 플랫폼의 보안 상태를 더욱 자세하게 파악할 수 있습니다.

보안 플랫폼 리소스:

* [보안 개발 주기, 내부 감사](https://www.microsoft.com/sdl/)
* [필수 보안 교육, 백그라운드 검사](https://downloads.cloudsecurityalliance.org/star/self-assessment/StandardResponsetoRequestforInformationWindowsAzureSecurityPrivacy.docx)
* [침투 테스트, 침입 검색, DDoS, 감사 및 로깅](https://www.microsoft.com/trustcenter/Security/AuditingAndLogging)
* [최첨단 데이터 센터](https://www.microsoft.com/cloud-platform/global-datacenters), 물리적 보안, [보안 네트워크](/azure/security/security-network-overview)
* [클라우드에서 Microsoft Azure의 보안 대응(PDF)](https://aka.ms/SecurityResponsePaper)

개인 정보 및 컨트롤:

* [항상 사용자 데이터 관리](https://www.microsoft.com/trustcenter/Privacy/You-own-your-data)(영문)
* [데이터 위치에서 제어](https://www.microsoft.com/trustcenter/Privacy/Where-your-data-is-located)
* [조건부 데이터 액세스 제공](https://www.microsoft.com/trustcenter/Privacy/Who-can-access-your-data-and-on-what-terms)
* [사법 기관에 대한 응답](https://www.microsoft.com/trustcenter/Privacy/Responding-to-govt-agency-requests-for-customer-data)(영문)
* [엄격한 개인 정보 보호 표준](https://www.microsoft.com/TrustCenter/Privacy/We-set-and-adhere-to-stringent-standards)

규정 준수:

* [보안 센터](https://www.microsoft.com/trustcenter/default.aspx)
* [일반 컨트롤 허브](https://www.microsoft.com/trustcenter/Common-Controls-Hub)(영문)
* [Cloud Services 실사 검사 목록](https://www.microsoft.com/trustcenter/Compliance/Due-Diligence-Checklist)(영문)
* [서비스별, 지역별 및 산업별 규정 준수](https://www.microsoft.com/trustcenter/Compliance/default.aspx)

투명성:

* [Microsoft Azure 서비스에서 고객 데이터를 보호하는 방법](https://www.microsoft.com/trustcenter/Transparency/default.aspx)(영문)
* [Microsoft에서 Azure 서비스의 데이터 위치를 관리하는 방법](https://azuredatacentermap.azurewebsites.net/)
* [데이터에 누가 그리고 어떤 조건으로 액세스할 수 있는가](https://www.microsoft.com/trustcenter/Privacy/Who-can-access-your-data-and-on-what-terms)
* [Microsoft Azure 서비스에서 고객 데이터를 보호하는 방법](https://www.microsoft.com/trustcenter/Transparency/default.aspx)(영문)
* [Azure 서비스, 투명성 허브에 대한 인증 검토](https://www.microsoft.com/trustcenter/Compliance/default.aspx)

## <a name="next-steps"></a>다음 단계

거버넌스 전략의 첫 번째 작업 단계는 [정책 검토](./what-is-a-cloud-policy-review.md)입니다. 정책 검토 과정에서 [정책 및 규정 준수](./overview.md)를 가이드로 유용하게 활용할 수 있습니다.

> [!div class="nextstepaction"]
> [정책 검토 준비](./what-is-a-cloud-policy-review.md)