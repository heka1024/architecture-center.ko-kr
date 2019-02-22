---
title: 'CAF: 보안 기준 동기 부여 및 비즈니스 위험'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 1/8/2019
description: 보안 기준 동기 부여 및 비즈니스 위험
author: BrianBlanchard
ms.openlocfilehash: d57b79b37aa257d07df0168a7fee53ec8ecd1526
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901245"
---
# <a name="security-baseline-motivations-and-business-risks"></a>보안 기준 동기 부여 및 비즈니스 위험

이 문서에서는 고객이 일반적으로 클라우드 거버넌스 전략 내에 보안 기준 분야를 도입하는 이유를 설명합니다. 또한 정책 설명을 추진할 수 있는 잠재적인 비즈니스 위험의 몇 가지 예도 제공합니다.

<!-- markdownlint-disable MD026 -->

## <a name="is-a-security-baseline-relevant"></a>보안 기준이 적절합니까?

보안은 모든 IT 조직의 주요 관심사입니다. 클라우드 배포 시 워크로드가 기존의 온-프레미스 데이터 센터에 호스팅될 때와 동일한 보안 위험에 직면합니다. 그러나 워크로드를 저장하고 실행할 물리적 하드웨어의 직접 소유권이 없다는 공용 클라우드 플랫폼의 속성 상, 클라우드 보안을 위해 자체 정책 및 프로세스가 필요합니다.

클라우드 보안 거버넌스가 기존의 보안 정책과 구별되는 주요 특징 중 하나는 리소스를 간편하게 만들 수 있다는 것이며, 배포 전에 보안을 고려하지 않으면 취약성이 더 늘어날 수 있습니다. 또한 클라우드 기반 네트워크 토폴로지를 신속하게 변경할 수 있도록 [SDN(소프트웨어 정의 네트워킹)](../../decision-guides/software-defined-network/overview.md) 같은 기술이 제공하는 유연성을 통해 전체적인 네트워크 공격 노출 영역을 예측할 수 없는 방식으로 쉽게 수정할 수 있습니다. 클라우드 플랫폼 역시 온-프레미스 환경에서는 불가능할 수 있는 방식으로 보안을 강화하는 도구와 기능을 제공합니다.

고객이 보안 정책 및 프로세스에 투자하는 금액은 클라우드 배포의 특성에 따라 크게 달라집니다. 초기 배포 시에는 가장 기본적인 보안 정책만 적용하면 되지만, 중요 업무용 워크로드는 복잡하고 광범위한 보안 요구 사항을 동반할 것입니다. 모든 배포는 일정 수준에서 분야의 개입이 필요합니다.

보안 기준 분야는 보안 위험으로부터 클라우드 배포를 보호하기 위해 적용할 수 있는 회사 정책 및 수동 프로세스를 다룹니다.

> [!NOTE]
>보안 기준의 컨텍스트에서 [ID 기준](../identity-baseline/overview.md) 및 액세스 제어와의 관련성을 이해하는 것이 중요하지만, [클라우드 거버넌스의 5개 분야](../overview.md)에서는 [ID 기준](../identity-baseline/overview.md)을 보안 기준과 분리된 자체 분야로 간주합니다.

## <a name="business-risk"></a>비즈니스 위험

보안 기준 분야는 핵심 보안 관련 비즈니스 위험을 해결하려고 시도합니다. 클라우드 배포를 계획하고 구현할 때 회사와 협력하여 이러한 위험을 식별하고 관련성을 모니터링하세요.

위험은 조직마다 다르지만, 다음 항목을 일반적인 보안 관련 위험으로 사용하여 클라우드 거버넌스 팀 내에서 토론을 시작하는 시작점으로 사용할 수 있습니다.

- **데이터 보안 위반**. 클라우드에 호스팅된 중요 데이터가 실수로 노출 또는 손실되면 고객을 잃게 되거나 계약 문제 또는 법적 책임 문제가 발생할 수 있습니다.
- **서비스 중단**. 안전하지 않은 인프라 때문에 발생하는 가동 중단 및 기타 성능 문제는 정상 작동을 방해하며 생산성 또는 비즈니스 손실로 이어질 수 있습니다.

## <a name="next-steps"></a>다음 단계

[클라우드 관리 템플릿](./template.md)을 사용하여 현재 클라우드 도입 계획으로 인해 발생할 가능성이 있는 비즈니스 위험을 문서화합니다.

현실적인 비즈니스 위험을 이해했다면, 그 다음 단계는 회사의 위험 허용 범위와 허용 범위를 모니터링할 지표 및 핵심 메트릭을 문서화하는 것입니다.

> [!div class="nextstepaction"]
> [지표, 메트릭 및 위험 허용 범위의 이해](./metrics-tolerance.md)
