---
title: 'CAF: Cost Management 메트릭, 지표 및 위험 허용 범위'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 클라우드 거버넌스와 관련된 Cost Management에 대해 설명합니다.
author: BrianBlanchard
ms.openlocfilehash: 76e6b1b32dd862322f6cafd9aa63c6c4f79f4f5d
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58241734"
---
# <a name="cost-management-metrics-indicators-and-risk-tolerance"></a>Cost Management 메트릭, 지표 및 위험 허용 범위

이 문서는 Cost Management와 관련된 비즈니스 위험 허용 범위를 정량화하는 데 도움을 주기 위한 것입니다. 메트릭과 지표를 정의하면 Cost Management 분야를 완성하는 데 투자하기 위한 비즈니스 사례를 만들 수 있습니다.

## <a name="metrics"></a>메트릭

Cost Management는 일반적으로 비용과 관련된 메트릭에 중점을 둡니다. 위험 분석의 일환으로 클라우드 기반 워크로드에 대한 현재 및 계획된 지출과 관련된 데이터를 수집하여 직면한 위험의 정도 및 클라우드 채택 전략에 대한 비용 거버넌스 투자의 중요도를 결정할 수 있습니다.

보안 기준 분야 내에서 위험 허용 범위를 평가하는 데 도움이 되도록 수집해야 하는 유용한 메트릭의 예는 다음과 같습니다.

- 연간 지출: 클라우드 공급자가 제공하는 서비스에 대한 연간 총 비용
- 월간 지출: 클라우드 공급자가 제공하는 서비스에 대한 월간 총 비용
- 예상 대 실제 비율: 예상 지출과 실제 지출을 비교한 비율(월간 또는 연간)
- 채택 속도(MOM) 비율: 월별 클라우드 비용의 델타 비율
- 누적 비용: 해당 월의 시작에서 누적된 일별 총 지출
- 지출 추세: 예산 대비 지출 추세

## <a name="risk-tolerance-indicators"></a>위험 허용 범위 지표

개발/테스트 또는 실험의 첫 번째 워크로드와 같은 초기 배포 시 Cost Management는 상대적으로 위험이 낮습니다. 자산이 더 많이 배포됨에 따라 위험이 커지고 비즈니스의 위험 허용 범위가 줄어들 가능성이 있습니다. 또한 클라우드에 자산을 구성하거나 배포할 수 있는 기능이 제공되는 클라우드 채택 팀이 늘어남에 따라 위험이 커지고 허용 범위가 줄어듭니다. 반대로, Cost Management 분야가 성장하면 클라우드 채택 단계에 있는 사용자가 더 혁신적인 새 기술을 배포하게 됩니다.

클라우드 채택의 초기 단계에서는 비즈니스 팀과 협력하여 위험 허용 범위 기준을 결정합니다. 기준이 확정되면 Cost Management 분야에 대한 투자를 트리거하는 기준을 결정해야 합니다. 이러한 기준은 각 조직마다 다를 수 있습니다.

[비즈니스 위험](./business-risks.md)이 식별되면 비즈니스 팀과 협력하여 이러한 위험이 증가될 수 있는 트리거를 식별하는 데 사용할 수 있는 벤치마크를 확인합니다. 위에서 언급한 것과 같은 메트릭을 위험 기준 허용 범위와 비교하여 Cost Management에 더 많이 투자해야 한다는 비즈니스의 필요성을 나타내는 몇 가지 예는 다음과 같습니다.

- 약정 기반(가장 일반적): 올해 $X,000,000를 클라우드 공급업체에 지출하기로 약정한 회사입니다. 비즈니스에서 지출 목표가 20%를 초과하지 않도록 하고, 해당 약정의 90% 이상을 사용할 수 있도록 하기 위해 Cost Management 분야가 필요합니다.
- 백분율 트리거: 프로덕션 시스템에 안정적인 클라우드 지출을 확보한 회사입니다. 해당 X%를 초과하여 변경되는 경우 Cost Management 분야에 투자하는 것이 좋습니다.
- 오버프로비저닝된 트리거: 배포한 솔루션이 오버프로비전되고 있다고 생각하는 회사입니다. Cost Management는 프로비전과 자산 활용의 적절한 맞춤을 보여줄 수 있을 때까지의 최우선 투자입니다.
- 월간 지출 트리거: 매월 $x,000를 초과하여 지출하는 회사는 상당한 비용으로 간주됩니다. 지정된 월에 지출이 해당 금액을 초과하면 Cost Management에 투자해야 합니다.
- 연간 지출 트리거: 클라우드 실험에 매년 $X,000를 지출할 수 있는 IT R&D 예산을 확보한 회사입니다. 클라우드에서 프로덕션 워크로드를 실행할 수 있지만, 예산이 해당 금액을 초과하지 않으면 여전히 실험 솔루션으로 간주됩니다. 초과되면 예산을 프로덕션 투자처럼 처리하고 지출을 면밀히 관리해야 합니다.
- OpEx(운영 비용) 초과(비일반적): 회사에서 운영 비용이 크게 초과하고 있고, 개발/테스트 워크로드를 배포하기 전에 Cost Management 제어가 필요합니다.

## <a name="next-steps"></a>다음 단계

[클라우드 관리 템플릿](./template.md)을 사용하여 현재 클라우드 채택 계획에 부합하는 메트릭 및 허용 범위 지표를 문서화합니다.

위험 및 허용 범위를 기반으로 하여 Cost Management 정책 준수를 제어하고 전달하는 프로세스를 설정합니다.

> [!div class="nextstepaction"]
> [정책 준수 프로세스 설정](compliance-processes.md)
