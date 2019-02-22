---
title: 'CAF: 대규모 엔터프라이즈 거버넌스 경험'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 대규모 엔터프라이즈 거버넌스 경험
author: BrianBlanchard
ms.openlocfilehash: 689b600524c3be6c505ade8c5aaa447d772c6e35
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901333"
---
# <a name="caf-large-enterprise-governance-journey"></a>CAF: 대규모 엔터프라이즈 거버넌스 경험

## <a name="best-practice-overview"></a>모범 사례 개요

이 거버넌스 경험은 거버넌스 성숙도의 여러 단계를 통해 가상 회사의 경험을 따릅니다. 실제 고객 경험에 따릅니다. 제안된 모범 사례는 가상의 회사의 제약 조건 및 요구에 따릅니다.

빠른 시작점으로 이 개요는 모범 사례에 따라 거버넌스에 대한 MVP(실행 가능한 최소 제품)을 정의합니다. 또한 새 비즈니스 또는 기술 위험이 나타나는 경우 추가적인 모범 사례를 추가하는 일부 거버넌스 개선에 대한 링크도 제공합니다.

> [!WARNING]
> 이러한 MVP는 일련의 가정 세트를 기반으로 하는 기본 시작 지점입니다. 이 최소한의 모범 사례 세트도 고유한 비즈니스 위험과 위험 허용 오차에 따라 결정되는 회사 정책을 기반으로 합니다. 이러한 가정이 사용자에게 적용되는지 확인하려면 이 문서 다음에 나오는 [긴 내러티브](./narrative.md)를 참조하세요.

### <a name="governance-best-practice"></a>거버넌스 모범 사례

이 모범 사례는 조직이 여러 Azure 구독에 빠르고 일관되게 거버넌스 보호책을 추가하는 데 사용할 수 있는 기반으로 사용됩니다.

### <a name="resource-organization"></a>리소스 조직

다음 다이어그램에서는 리소스를 구성하기 위한 거버넌스 MVP 계층 구조를 보여줍니다.

![리소스 조직 다이어그램](../../../_images/governance/resource-organization.png)

모든 애플리케이션은 관리 그룹, 구독 및 리소스 그룹 계층 구조의 적절한 영역에 배포되어야 합니다. 배포를 계획하는 동안 클라우드 거버넌스 팀은 계층 구조에서 필요한 노드를 만들어 클라우드 도입 팀의 역량을 강화합니다.

1. 지리 및 환경(프로덕션, 비프로덕션)을 반영하는 자세한 계층 구조가 있는 각 사업부에 대한 관리 그룹입니다.
2. 사업부, 지리, 환경 및 "애플리케이션 분류"의 각 고유 조합에 대한 구독입니다.
3. 각 애플리케이션에 대한 별도의 리소스 그룹입니다.
4. 일관된 명명법이 그룹화 계층 구조의 각 수준에서 적용되어야 합니다.

![대규모 엔터프라이즈의 리소스 조직 다이어그램](../../../_images/governance/large-enterprise-resource-organization.png)

이러한 패턴은 불필요하게 계층 구조를 복잡하게 만들지 않고도 증가의 여지를 제공합니다.

[!INCLUDE [governance-of-resources](../../../../../includes/cloud-adoption/governance/governance-of-resources.md)]

## <a name="governance-evolutions"></a>거버넌스 개선

이 MVP가 배포된 후 거버넌스의 추가 계층은 환경에 신속하게 통합될 수 있습니다. 특정 비즈니스 요구 사항을 충족하기 위해 MVP를 발전시키는 몇 가지 방법은 다음과 같습니다.

- [보호된 데이터의 보안 기준](./security-baseline-evolution.md)
- [중요 업무용 애플리케이션의 리소스 일관성](./resource-consistency-evolution.md)
- [비용 관리 제어](./cost-management-evolution.md)
- [다중 클라우드 개선 제어](./multi-cloud-evolution.md)

<!-- markdownlint-disable MD026 -->

## <a name="what-does-this-best-practice-do"></a>이 모범 사례가 하는 것은 무엇인가요?

MVP에서 [배포 가속](../../deployment-acceleration/overview.md) 분야의 사례 및 도구가 신속하게 회사 정책을 적용하기 위해 설정됩니다. 특히 MVP는 Azure Blueprints, Azure Policy 및 Azure 관리 그룹을 사용하여 이 가상의 회사의 내러티브에 정의된 대로 몇 가지 기본 회사 정책을 적용합니다. 해당 회사 정책은 ID 및 보안에 대한 아주 작은 기준을 설정하려면 Azure Resource Manager 템플릿 및 Azure Policy를 사용하여 적용합니다.

![증분 거버넌스 MVP의 예제](../../../_images/governance/governance-mvp.png)

## <a name="evolving-the-best-practice"></a>모범 사례 발전

시간이 지남에 따라 이 거버넌스 MVP는 거버넌스 사례를 발전시키는 데 사용됩니다. 도입이 진전됨에 따라 비즈니스 위험도 증가합니다. CAF 거버넌스 모델 내의 여러 분야는 해당 위험을 완화하기 위해 발전됩니다. 이 시리즈의 뒷부분에 나오는 문서에서는 가상 회사에 영향을 주는 회사 정책의 개선을 설명합니다. 이러한 개선은 세 가지 분야에서 이뤄집니다.

- 내러티브에서 마이그레이션 종속성이 발전하는 경우의 ID 기준
- 도입이 크기 조정하는 경우의 Cost Management입니다.
- 보호된 데이터가 배포되는 경우의 보안 기준입니다.
- IT 운영 팀이 중요 업무용 워크로드 지원을 시작하는 경우의 리소스 일관성입니다.

![증분 거버넌스 MVP의 예제](../../../_images/governance/governance-evolution-large.png)

## <a name="next-steps"></a>다음 단계

이제 거버넌스 MVP에 익숙해졌고 다음의 거버넌스 개선을 이해했으므로 추가 컨텍스트에 대한 지원 내러티브를 참조하세요.

> [!div class="nextstepaction"]
> [지원 내러티브 참조](./narrative.md)
