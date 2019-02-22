---
title: 'CAF: 디자인 가이드를 정책에 맞게 조정합니다.'
description: 디자인 가이드를 어떻게 정책에 맞게 조정하나요?
author: BrianBlanchard
ms.date: 01/04/2019
ms.openlocfilehash: 77a35597585e5f58967ea79d3c7b0fa17b6ab80e
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901298"
---
<!---
I've established policies. How to help developers adopt these policies?
Draft an architecture design guide.

[Aspirational statement] If you're using Azure, you can use one of ours as a starting point. The choose one of the following 6 as a starting point and mold it to fit your policies.
--->

<!-- markdownlint-disable MD026 -->

# <a name="how-do-you-align-design-guides-with-policy"></a>디자인 가이드를 어떻게 정책에 맞게 조정하나요?

[식별된 위험](understanding-business-risk.md)에 따라 [클라우드 정책을 정의](define-policy.md)한 후에는 IT 직원 및 개발자가 참조하는 정책에 부합하는 실행 가능한 지침을 생성해야 합니다. 클라우드 거버넌스 설계 가이드의 기초를 마련하면 5가지 거버넌스 분야 각각에 대해 생성한 정책 설명에 따라 구체적인 구조, 기술 및 프로세스를 지정할 수 있습니다.

클라우드 거버넌스 설계 가이드는 정책 요구 사항을 가장 잘 충족하는 클라우드 배포의 핵심 인프라 구성 요소 각각에 대한 아키텍처 옵션 및 디자인 패턴을 수립해야 합니다. 이와 함께, 각각의 디자인 의사 결정을 지원할 기술, 도구 및 프로세스에 대한 간략한 설명을 제공해야 합니다.

위험 분석 및 정책 설명이 어느 정도는 클라우드 플랫폼에 구애되겠지만, 디자인 가이드는 IT 및 개발 팀에서 원하는 플랫폼별 구현 정보를 제공해야 합니다. 디자인을 결정하고 지침을 제공할 때 선택한 플랫폼의 아키텍처, 도구 및 기능에 집중해야 합니다.

클라우드 디자인 가이드는 각 인프라 구성 요소와 관련된 기술 세부 정보 중 일부를 고려해야 하지만, 광범위한 기술 문서 또는 사양서가 아닙니다. 가이드는 모든 정책 설명을 다뤄야 하며, 직원이 쉽게 이해하고 참조할 수 있는 쉬운 형식으로 디자인 의사 결정을 명확하게 설명해야 합니다.

<!-- markdownlint-enable MD033 -->

## <a name="using-the-actionable-governance-journeys"></a>실행 가능한 거버넌스 여정 사용

클라우드 도입에 Azure 플랫폼을 사용할 계획인 경우 CAF는 CAF 거버넌스 모델의 증분 방식을 설명하는 [거버넌스 여정](../journeys/overview.md)을 제공합니다. 이러한 서술식 여정에서는 비즈니스 위험, 내결함성 요구 사항, 거버넌스 MVP(최소 실행 가능 제품)를 만드는 데 사용된 정책 설명을 포함하여 다양한 일반 도입 시나리오를 다룹니다. 이러한 여정은 Azure에서 제공하는 클라우드 도입 프로세스의 실제 고객 경험을 합성하여 나타냅니다.

클라우드를 도입하는 고객마다 고유한 목표, 우선 순위 과제가 있겠지만, 이러한 샘플은 정책을 지침으로 변환할 수 있는 유용한 템플릿을 제공해야 합니다. 현재 상황과 가장 비슷한 시나리오를 시작점으로 선택하고, 구체적인 정책 요구 사항에 맞게 조정하세요.

## <a name="next-steps"></a>다음 단계

디자인 지침을 준비한 후에는 정책 준수를 위한 정책 준수 프로세스를 수립합니다.

> [!div class="nextstepaction"]
> [정책 준수 프로세스](processes.md)