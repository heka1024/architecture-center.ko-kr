---
title: 'CAF: 보안 기준 샘플 정책 설명'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 보안 기준 샘플 정책 설명
author: BrianBlanchard
ms.openlocfilehash: ac40e022f8dff0c3021c04cd9d6ae42d28afaf1f
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901381"
---
# <a name="security-baseline-sample-policy-statements"></a>보안 기준 샘플 정책 설명

개별 클라우드 정책 설명은 위험 평가 프로세스 중에 파악된 특정 위험을 해결하기 위한 지침입니다. 이러한 설명은 위험의 간략한 요약과 위험을 해결하기 위한 계획을 제공해야 합니다. 각 설명 정의에는 다음 정보가 포함되어야 합니다.

- 기술적 위험 - 이 정책이 처리할 위험의 요약입니다.
- 정책 설명 - 정책 요구 사항의 명확한 요약 설명입니다.
- 기술 옵션 - IT 팀과 개발자가 정책 구현 시에 사용할 수 있는 실행 가능한 권장 사항, 사양 또는 기타 지침입니다.

여러 보안 관련 일반 비즈니스 위험을 해결하는 다음 샘플 정책 설명은 사용자 조직의 요구를 해결하는 실제 정책 설명의 초안을 작성할 때 참조할 수 있도록 예제로 제공됩니다. 특정 위험을 해결하기 위해 반드시 이러한 예제를 사용해야 하는 것은 아니며, 확인된 한 가지 위험을 해결할 수 있도록 여러 정책 옵션이 제공될 수도 있습니다. 실무 팀, 보안 팀 및 IT 팀과 긴밀하게 협력하여 특정 보안 위험에 가장 적합한 정책 솔루션을 선택하세요.  

## <a name="asset-classification"></a>자산 분류

**기술적 위험**: 중요한 데이터를 포함하는 자산 또는 중요 업무용 자산으로 정확하게 확인되지 않은 자산은 충분히 보호되지 않을 수 있으므로 데이터가 누수되거나 업무가 중단될 가능성이 있습니다.

**정책 설명**: 배포된 모든 자산은 중요도 및 데이터 분류를 기준으로 범주를 지정해야 합니다. 자산을 클라우드로 배포하기 전에 클라우드 거버넌스 팀과 애플리케이션 소유자가 분류를 검토해야 합니다.

**사용 가능한 디자인 옵션**: [리소스 태그 지정 표준](../../decision-guides/resource-tagging/overview.md)을 정하고 IT 담당자가 [Azure 리소스 태그](/azure/azure-resource-manager/resource-group-using-tags)를 사용해 배포된 모든 리소스에 해당 표준을 일관되게 적용하도록 합니다.

## <a name="data-encryption"></a>데이터 암호화.

**기술적 위험**: 보호된 데이터가 저장 중에 노출될 위험이 있습니다.

**정책 설명**: 보호된 데이터는 미사용 시 모두 암호화해야 합니다.

**사용 가능한 디자인 옵션**: [Azure 암호화 개요](/azure/security/security-azure-encryption-overview) 문서에 나와 있는 Azure 플랫폼에서 미사용 데이터 암호화를 수행하는 방법의 설명을 참조하세요.  

## <a name="network-isolation"></a>네트워크 격리

**기술적 위험**: 네트워크와 네트워크 내 서브넷 간의 연결로 인해 데이터 누수 또는 중요 업무용 서비스 중단을 야기할 수 있는 취약성이 발생할 수 있습니다.

**정책 설명**: 보호된 데이터를 포함하는 네트워크 서브넷은 다른 서브넷에서 격리해야 합니다. 보호된 데이터 서브넷 간의 네트워크 트래픽은 정기적으로 감사해야 합니다.

**사용 가능한 디자인 옵션**: Azure에서 [Azure Virtual Network](/azure/virtual-network/virtual-networks-overview)를 통해 네트워크 및 서브넷 격리를 관리합니다.

## <a name="secure-external-access"></a>외부 액세스 보호

**기술적 위험**: 공용 인터넷에서 워크로드 액세스를 허용하면 침입 위험이 발생하여 데이터가 무단 노출되거나 업무가 중단될 수 있습니다.

**정책 설명**: 공용 인터넷이나 데이터 센터를 통해서는 보호된 데이터를 포함하는 서브넷에 직접 액세스할 수 없습니다. 이러한 서브넷에 액세스하려면 중간 서브넷을 거쳐야 합니다. 즉, 해당 서브넷에 액세스할 때는 항상 패킷 검사 및 차단 기능을 실행할 수 있는 방화벽 솔루션을 거쳐야 합니다.

**사용 가능한 디자인 옵션**: Azure에서 [공용 인터넷과 클라우드 기반 네트워크 간에 DMZ](/azure/architecture/reference-architectures/dmz/secure-vnet-dmz)를 배포하여 공용 엔드포인트를 보호합니다.

## <a name="ddos-protection"></a>DDoS 보호

**기술적 위험**: DDoS(배포된 서비스 거부) 공격으로 인해 업무가 중단될 수 있습니다.

**정책 설명**: 공개적으로 액세스할 수 있는 모든 네트워크 엔드포인트에 자동화된 DDoS 완화 메커니즘을 배포합니다.

**사용 가능한 디자인 옵션**: [Azure DDoS Protection](/azure/virtual-network/ddos-protection-overview)을 사용하여 DDoS 공격으로 인한 업무 중단을 최소화합니다.

## <a name="secure-on-premises-connectivity"></a>온-프레미스 연결 보안 유지

**기술적 위험**: 공용 인터넷을 통해 클라우드 네트워크와 온-프레미스 간에 전송되는 암호화되지 않은 트래픽은 가로채기에 취약하므로 데이터 노출 위험이 발생합니다.

**정책 설명**: 온-프레미스와 클라우드 네트워크 간의 모든 연결은 암호화된 보안 VPN 연결이나 전용 개인 WAN 링크를 통해 설정해야 합니다.

**사용 가능한 디자인 옵션**: Azure에서 ExpressRoute 또는 Azure VPN을 사용하여 온-프레미스와 클라우드 네트워크 간의 개인 연결을 설정합니다.

## <a name="network-monitoring-and-enforcement"></a>네트워크 모니터링 및 적용

**기술적 위험**: 네트워크 구성을 변경하면 새로운 취약성과 데이터 노출 위험이 발생할 수 있습니다.

**정책 설명**: 거버넌스 도구는 보안 기준 팀이 정의한 네트워크 구성 요구 사항을 감사하고 적용해야 합니다.

**사용 가능한 디자인 옵션**: Azure에서는 [Azure Network Watcher](/azure/network-watcher/network-watcher-monitoring-overview)를 사용하여 네트워크 활동을 모니터링할 수 있으며 [Azure Security Center](/azure/security-center/security-center-network-recommendations)를 사용하여 보안 취약성을 파악할 수 있습니다. Azure Policy에서는 보안 팀이 정의한 제한에 따라 네트워크 리소스 및 리소스 구성 정책을 제한할 수 있습니다.

## <a name="security-review"></a>보안 검토

**기술적 위험**: 새로운 보안 위협과 공격 유형이 계속 나타나므로 클라우드 리소스 노출 또는 제공 중단 위험이 증가합니다.

**정책 설명**: 보안 팀은 클라우드 배포에 영향을 줄 수 있는 추세 및 익스플로잇 가능성을 정기적으로 검토하여 클라우드에서 사용되는 보안 기준 도구에 업데이트를 제공해야 합니다.

**사용 가능한 디자인 옵션**: 관련 IT 및 거버넌스 팀 구성원들이 참여하는 정기 보안 검토 회의를 진행합니다. 기존 보안 데이터와 메트릭을 검토해 현재 정책 및 보안 기준 도구에서 부족한 부분을 확인한 다음 새로운 위험을 완화할 수 있도록 정책을 업데이트합니다.

## <a name="next-steps"></a>다음 단계

이 문서에서 설명한 샘플을 토대로 하여 특정 보안 위험을 해결하며 클라우드 채택 계획에 적합한 정책을 개발합니다.

보안 기준 관련 사용자 지정 정책 설명 개발을 시작하려면 [보안 기준 템플릿](template.md)을 다운로드하세요.

이러한 원칙을 신속하게 채택하려면 사용자 환경에 가장 가깝게 일치하는 [실행 가능한 거버넌스 경험](../journeys/overview.md)을 선택한 다음 디자인을 수정하여 회사의 구체적인 정책 결정 사항을 통합합니다.

> [!div class="nextstepaction"]
> [실행 가능한 거버넌스 경험](../journeys/overview.md)
