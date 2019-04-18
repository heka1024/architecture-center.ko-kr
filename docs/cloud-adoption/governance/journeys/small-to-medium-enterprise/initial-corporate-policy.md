---
title: 'CAF: 소규모 및 중간 enterprise-거 버 넌 스 전략 뒤 초기 회사 정책'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 소규모 및 중간 enterprise-거 버 넌 스 전략 뒤 초기 회사 정책
author: BrianBlanchard
ms.openlocfilehash: 2133145c9933ad450ea3cc55ecd68b8a667df783
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59640025"
---
# <a name="small-to-medium-enterprise-initial-corporate-policy-behind-the-governance-strategy"></a>중소기업: 거버넌스 전략의 기반이 되는 초기 기업 정책

아래에서 설명하는 기업 정책은 거버넌스 경험이 시작되는 지점인 초기 거버넌스 위치를 정의합니다. 이 문서에서는 초기 단계의 위험, 초기 정책 설명 및 정책 설명 적용을 위한 초기 프로세스의 정의를 제시합니다.

> [!NOTE]
>기업 정책은 기술 문서는 아니지만 여러 기술 관련 사항을 결정하는 데 기준으로 사용됩니다. [개요](./overview.md)에 설명되어 있는 거버넌스 MVP는 이 정책에서 최종적으로 파생됩니다. 따라서 조직은 거버넌스 MVP를 구현하기 전에 고유한 목표와 비즈니스 위험을 기준으로 하여 기업 정책을 개발해야 합니다.

## <a name="cloud-governance-team"></a>클라우드 거버넌스 팀

이 문서의 설명에서 클라우드 거버넌스 팀은 거버넌스의 필요성을 인지한 시스템 관리자 두 명으로 구성됩니다. 이 두 관리자는 '클라우드 보유자' 직책을 맡아 앞으로 몇 달 동안 회사의 클라우드 현재 상태 거버넌스를 정리하는 작업을 인계받아 진행할 예정입니다. 이후 거버넌스 경험이 개선됨에 따라 이 직함은 변경될 가능성이 높습니다.

[!INCLUDE [business-risk](../../../../../includes/cloud-adoption/governance/business-risks.md)]

## <a name="tolerance-indicators"></a>허용 범위 지표

현재 위험 허용 범위는 높고 클라우드 거버넌스 투자 의향은 낮은 상태입니다. 따라서 허용 범위 지표는 시간과 에너지를 더 투자하도록 하는 조기 경보 시스템 역할을 합니다. 지표에서 다음과 같은 수치가 관찰되면 거버넌스 전략을 개선해야 합니다.

- 비용 관리: 클라우드에 대한 배포 범위가 자산 100개를 초과하거나 월 지출 비용이 미화 1,000달러를 초과합니다.
- 보안 기준: 정의된 클라우드 채택 계획에 보호된 데이터가 포함됩니다.
- 리소스 일관성: 정의된 클라우드 채택 계획에 중요 업무용 애플리케이션이 포함됩니다.

[!INCLUDE [policy-statements](../../../../../includes/cloud-adoption/governance/policy-statements.md)]

## <a name="next-steps"></a>다음 단계

클라우드 거버넌스 팀은 이 기업 정책을 통해 클라우드 채택의 기반이 되는 거버넌스 MVP 구현을 준비할 수 있습니다. 다음 단계에서 이 MVP를 구현합니다.

> [!div class="nextstepaction"]
> [모범 사례 설명](./best-practice-explained.md)