---
title: 'CAF: 대기업 – 다중 클라우드 개선'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 대기업 – 다중 클라우드 개선
author: BrianBlanchard
ms.openlocfilehash: 5ef29aa523c04ff93b2d4f983482f94654a4a039
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901808"
---
# <a name="large-enterprise-multi-cloud-evolution"></a>대기업: 다중 클라우드 진화

## <a name="evolution-of-the-narrative"></a>이야기 전개

Microsoft는 고객들이 특정 용도로 여러 클라우드를 도입한다는 점을 잘 알고 있습니다. 이 경험에서 소개하는 가상의 기업도 마찬가지입니다. 이 고객은 Azure 도입 경험을 진행하는 과정에서 우수한 사업 실적을 기록하여 기존 사업을 보완하는 소규모 기업을 인수하게 되었습니다. 이 기업은 다른 클라우드 공급자의 서비스를 통해 모든 IT 작업을 실행하고 있습니다.

이 문서에서는 새 조직 통합 시의 클라우드 변경 방식에 대해 설명합니다. 이 설명에서는 회사가 이 고객 경험에 요약된 각 거버넌스 개선 단계를 완료했다고 가정합니다.

### <a name="evolution-of-the-current-state"></a>현재 상태의 전개

이 이야기의 앞 부분에서는 클라우드 지출이 회사의 고정 경상비 중 일부로 자리함에 따라 회사가 비용 관리 및 비용 모니터링을 구현하기 시작했습니다.

이후로 거버넌스에 영향을 줄 수 있는 몇 가지 변화가 있었습니다.

- Active Directory의 온-프레미스 인스턴스를 통해 ID를 제어합니다. Azure Active Directory로의 복제를 통해 하이브리드 ID를 손쉽게 사용할 수 있게 되었습니다.
- 대개 Azure Monitor 및 관련 자동화 기능을 통해 IT 운영 또는 클라우드 운영을 관리합니다.
- Azure Vault 인스턴스를 통해 재해 복구/비즈니스 연속성을 제어합니다.
- Azure Security Center를 사용하여 보안 위반과 공격을 모니터링합니다.
- 클라우드 거버넌스 모니터링에는 Azure Security Center 및 Azure Monitor가 둘 다 사용됩니다.
- Azure Blueprints, Azure Policy 및 관리 그룹을 사용하여 정책 준수를 자동화합니다.

### <a name="evolution-of-the-future-state"></a>향후 상태의 전개

개선 과정의 목표는 가능한 모든 영역에서 인수 대상 회사를 기존 운영 분야에 통합하는 것입니다.

## <a name="evolution-of-tangible-risks"></a>실질적인 위험의 전개

**기업 인수 비용**: 새 기업 인수를 통한 수익 창출 효과는 약 5년 이내에 나타날 것으로 전망됩니다. 하지만 수익 창출 속도가 느려서 이사회에서는 인수 비용을 최대한 제어하고자 합니다. 이로 인해 비용 제어와 기술 통합이 상충할 위험이 있습니다.

이러한 비즈니스 위험은 다음과 같은 몇 가지 기술적 위험으로 확장될 수 있습니다.

- 클라우드 완화가 추가적인 인수 비용을 초래할 위험이 있습니다.
- 새로운 환경이 제대로 관리되지 않거나 정책 위반을 초래할 위험도 있습니다.

## <a name="evolution-of-the-policy-statements"></a>정책 설명의 전개

정책을 다음과 같이 변경하면 새로운 위험을 완화하고 구현 과정을 안내할 수 있습니다.

1. 기존의 운영 관리 및 보안 모니터링 도구를 통해 보조 클라우드의 모든 자산을 모니터링해야 합니다.
2. 모든 조직 구성 단위를 기존 ID 공급자에 통합해야 합니다.
3. 기본 ID 공급자가 보조 클라우드의 자산에 대한 인증을 제어해야 합니다.

## <a name="evolution-of-the-best-practices"></a>모범 사례의 전개

이 문서 섹션에서는 Azure Cost Management 구현 및 새로운 Azure 정책을 포함하도록 거버넌스 MVP 디자인을 개선합니다. 이 두 가지 디자인 변경을 통해 새로운 기업 정책 설명을 처리하게 됩니다.

1. 네트워크 연결 - 네트워킹 및 IT 보안 실행, 거버넌스 지원
    1. MPLS/임대 회선 공급자에서 새 클라우드로의 연결을 추가하면 네트워크가 통합됩니다. 라우팅 테이블과 방화벽 구성을 추가하면 환경 간의 액세스 및 트래픽을 제어할 수 있습니다.
2. ID 공급자를 통합합니다. 보조 클라우드에서 호스트되는 워크로드에 따라 다양한 옵션을 통해 ID 공급자를 통합할 수 있습니다. 다음은 몇 가지 예제입니다.
    1. OAuth 2를 사용하여 인증하는 애플리케이션의 경우에는 보조 클라우드의 Active Directory에 속한 사용자를 기존 Azure AD 테넌트로 복제하면 됩니다.
    2. 다른 한편으로 두 온-프레미스 ID 공급자 간의 페더레이션을 통해 사용자가 새 Active Directory 도메인을 Azure에 복제할 수 있게 됩니다.
3. Azure Site Recovery에 자산 추가
    1. Azure Site Recovery는 원래 하이브리드/다중 클라우드 도구로 구축되었습니다.
    2. 온-프레미스 자산을 보호하는 데 사용한 것과 같은 Azure Site Recovery 프로세스를 통해 보조 클라우드의 가상 머신을 보호할 수도 있습니다.
4. Azure Cost Management에 자산 추가
    1. Azure Cost Management는 원래 다중 클라우드 도구로 구축되었습니다.
    2. 일부 클라우드 공급자의 경우에는 보조 클라우드의 가상 머신이 Azure Cost Management와 호환될 수도 있습니다. 추가 비용이 발생할 수 있습니다.
5. Azure Monitor에 자산 추가
    1. Azure Monitor는 원래 하이브리드 클라우드 도구로 구축되었습니다.
    2. 보조 클라우드의 가상 머신이 Azure Monitor 에이전트와 호환될 수도 있으며, 그러면 운영 모니터링을 위해 가상 머신을 Azure Monitor에 포함할 수 있습니다.
6. 거버넌스 적용 도구
    1. 거버넌스 적용 도구는 클라우드별로 다릅니다.
    2. 거버넌스 과정에서 수립된 기업 정책은 그렇지 않습니다. 그러므로 구현은 클라우드별로 다를 수도 있지만 보조 공급자에도 정책 설명을 적용할 수 있습니다.

다중 클라우드 도입 범위가 확대되면 위에서 설명한 디자인 개선 과정이 계속 진행되어 디자인이 완성됩니다.

## <a name="next-steps"></a>다음 단계

많은 대기업에서 클라우드 거버넌스의 분야가 도입 차단 요소가 될 수 있습니다. 다음 문서에서는 클라우드에서의 장기적 성공을 위해 거버넌스를 팀 스포츠로 고려하여 확인해 봅니다.

> [!div class="nextstepaction"]
> [거버넌스의 여러 계층](./multiple-layers-of-governance.md)
