---
title: 'CAF: 디지털 자산 합리화'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
description: 디지털 자산을 평가하여 클라우드에서 호스트하는 최적의 방법을 결정합니다.
author: BrianBlanchard
ms.date: 12/10/2018
ms.topic: guide
ms.openlocfilehash: 02189c9edcbfea0a55fe69a53bf610e85470a4d0
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55897204"
---
# <a name="rationalize-the-digital-estate"></a>디지털 자산 합리화

클라우드 합리화는 자산을 클라우드에서 호스트하는 가장 좋은 방법을 결정하기 위해 자산을 평가하는 프로세스입니다. [방법](approach.md)이 결정되고 [인벤토리](inventory.md)가 집계되면 클라우드 합리화를 시작할 수 있습니다. [합리화의 5가지 R](5-rs-of-rationalization.md)에서는 가장 일반적인 합리화 옵션을 설명합니다.

## <a name="traditional-view-of-rationalization"></a>기존 합리화 보기

복잡한 의사 결정 트리 같은 기존의 합리화 프로세스를 시각화하면 합리화를 쉽게 이해할 수 있습니다. 디지털 자산의 각 자산은 5개 답변(5R) 중 하나로 이어지는 프로세스를 통해 공급됩니다. 소규모 자산의 경우 이 프로세스가 잘 작동합니다. 대규모 자산의 경우 그렇게 효과적이지는 않으며 상당한 지연이 발생할 수 있습니다. 프로세스를 살펴보면서 그 이유를 알아보겠습니다. 그 후 보다 효율적인 모델을 제공해 드리겠습니다.

**인벤토리**. 기존 모델을 사용하는 완전한 합리화를 완성하려면 애플리케이션, 소프트웨어, 하드웨어, 운영 체제 및 시스템 성능 메트릭을 포함하여 철저한 자산 인벤토리가 필요합니다.

**정량적 분석**. 의사 결정 트리에서 정량적 질문은 의사 결정의 첫 번째 레이어를 결정합니다. 일반적인 질문: 자산이 현재 사용되고 있습니까? 만약 그렇다면, 자산이 최적화되고 적절한 크기로 조정되어 있습니까?? 자산 간에 어떤 종속성이 있습니까? 이러한 질문은 인벤토리 분류에 필수적입니다.

**정성적 분석**. 의사 결정의 그 다음 세트에서는 정성적 분석의 형태로 사람의 지능이 필요합니다. 종종 이러한 질문은 솔루션에만 해당되며 비즈니스 이해 관계자와 고급 사용자만 답변할 수 있습니다. 이러한 의사 결정 과정에서 일반적으로 시간이 지연되고, 일정이 상당히 지체됩니다. 이 분석에는 일반적으로 애플리케이션당 40&ndash;80 FTE 시간이 소요됩니다. 정성적 분석 질문 목록을 작성하는 방법에 대한 지침은 [디지털 자산 계획에 대한 접근 방법](approach.md)을 참조하세요.

**합리화 의사 결정**. 정성적 및 정량적 데이터가 숙련된 합리화 팀의 손에 들어가면 명확한 의사 결정이 탄생합니다. 하지만 아쉽게도 숙련된 합리화 팀을 고용하기에는 몸값이 너무 비싸고 교육하기에는 수개월이 걸립니다.

## <a name="rationalization-at-enterprise-scale"></a>엔터프라이즈급 규모의 합리화

이 작업이 너무 오래 걸리고 VM 50대의 디지털 자산을 처리하기에는 벅차다고 생각된다면 VM 수천대와 애플리케이션 수백 개가 있는 환경에서 비즈니스를 변환하려면 얼마나 많은 수고가 필요할지 상상해 보세요. 사람이 직접 한다면 1,500시간 이상의 FTE와 9개월 이상의 계획 기간이 소요될 것입니다.

전체 합리화는 최종 상태이자 추구해야 할 올바른 방향이지만, 투입 시간 및 에너지 대비 ROI는 좀처럼 높지 않습니다.

재무 의사 결정에 있어서 합리화가 필수인 경우 클라우드 합리화를 전문으로 하는 전문 서비스 조직에 맡겨 프로세스를 빠르게 끝내는 방안을 고려해 볼 수 있습니다. 그렇다 하더라도, 전체 합리화는 시간과 비용이 많이 들기 때문에 변환 또는 비즈니스 결과를 얻는 시간이 지연될 수 있습니다.

이 문서의 나머지 부분에서는 증분 합리화라고도 하는 대안을 설명합니다.

## <a name="incremental-rationalization"></a>증분 합리화

대규모 디지털 자산의 전체 합리화는 위험이 높으며 복잡성 때문에 시간이 지연될 수 있습니다. 증분 방식은 의사 결정이 지연되면 장애물의 위험을 줄여야 하는 부담이 기업에 가중된다는 전제를 깔고 있습니다. 시간이 지남에 따라 이 방법은 프로세스를 개발하는 유기적인 모델과 합리화 의사 결정에 필요한 경험을 효과적으로 만들어 냅니다.

### <a name="inventory-reduce-discovery-data-points"></a>인벤토리: 검색 데이터 지점 줄이기

전체 디지털 자산의 인벤토리를 정확하게 실시간으로 유지하기 위해 시간, 에너지, 비용을 투자하는 조직은 거의 없습니다. 손실, 도난, 새로 고침 주기 및 직원 온보딩은 종종 최종 사용자 디바이스의 구체적인 자산 추적을 정당화합니다. 그러나 기존의 온-프레미스 데이터 센터에서 서버 및 애플리케이션 인벤토리를 정확하게 유지하더라도 ROI(투자 수익률)가 낮은 경우가 자주 있습니다. 대부분의 IT 조직은 데이터 센터에 고정된 자산의 사용 현황을 추적하는 것보다 더 시급한 다른 문제를 해결해야 합니다.

클라우드 변환에서는 인벤토리와 운영 비용 사이에 직접적인 상관 관계가 있습니다. 적절한 계획을 세우려면 정확한 인벤토리 데이터가 필요합니다. 아쉽게도 현재 환경 검사 옵션은 전체 인벤토리를 검사하고 분류해야 하므로 의사 결정이 수주 또는 수개월까지 지연될 수 있습니다. 다행스럽게도 데이터 수집 속도를 높이는 몇 가지 요령이 있습니다.

지연에 대해 가장 많이 언급되는 것은 에이전트 기반 검색입니다. 기존의 합리화에 필요한 강력한 데이터는 종종 각 자산에서 실행되는 에이전트에서만 수집할 수 있는 데이터에 의존합니다. 이러한 에이전트 종속성은 종종 진행 속도를 저하시키는 원인이 됩니다. 보안, 작업 및 관리 함수에 피드백을 요구할 수 있기 때문입니다.

증분 합리화 프로세스에서는 초기 검색에 에이전트 없는 솔루션을 사용하여 초기 의사 결정 속도를 높일 수 있습니다. 환경의 복잡성 수준에 따라 에이전트 기반 솔루션이 계속 필요한 경우도 있겠지만, 비즈니스 변화의 중요 경로에서 에이전트 기반 솔루션을 제거할 수 있습니다.

### <a name="quantitative-analysis-streamline-decisions"></a>정량적 분석: 의사 결정 간소화

인벤토리 검색 방법과 상관 없이 정량적 분석은 다양한 초기 의사 결정 및 가정을 실현할 수 있습니다. 첫 번째 워크로드를 식별하려고 시도하거나 합리화의 목표가 대략적인 비용 비교인 경우에 특히 그렇습니다. 증분 합리화 프로세스에서 클라우드 전략 및 클라우드 도입 팀은 [합리화의 5R](5-rs-of-rationalization.md)을 두 가지 간결한 의사 결정으로 제한하고 정량적 요인만 적용하여 분석을 간소화하고 변화에 필요한 초기 데이터의 양을 줄입니다.

예를 들어 조직이 클라우드로 IaaS 마이그레이션하는 과정에 있으면 대부분의 워크로드가 사용 중지되거나 다시 호스트될 것으로 가정할 수 있습니다.

### <a name="qualitative-analysis-temporary-assumptions"></a>정성적 분석: 일시적 가정

잠재적 결과의 수를 줄이면 자산의 미래 상태에 대한 초기 의사 결정에 쉽게 도달할 수 있습니다. 옵션 수가 감소하면 초기 단계에서 기업에 해야 하는 질문 수도 감소합니다.

위의 예제를 계속 이어서 말씀드리면, 옵션을 다시 호스트 또는 사용 중지로 제한하면 초기 합리화 과정에서 기업에 한 가지 질문, 즉, 사용 중지 여부만 확인하면 됩니다.

"분석 결과에 따르면, 이 자산을 적극적으로 활용하는 사용자가 없습니다. 정확한 결과입니까 아니면 우리가 놓친 부분이 있습니까?" 이러한 이진 질문은 일반적으로 정성적 분석을 통해 훨씬 쉽게 실행할 수 있습니다.

이렇게 간소화된 접근 방식은 기준, 재무 계획, 전략 및 방향을 제시합니다. 이후 작업에서 각 자산이 추가 합리화 및 정성적 분석을 거치면서 다른 옵션을 평가하게 됩니다. 이 초기 합리화에서 만들어진 모든 가정은 구현 전에 테스트를 거칩니다.

## <a name="challenging-assumptions"></a>가정 검증

이전 섹션의 결과는 가정으로 채워진 대략적인 합리화입니다. 다음으로, 일부 가정을 검증할 차례입니다.

### <a name="retiring-assets"></a>자산 사용 중지

기존의 온-프레미스 환경에서는 사용되지 않은 소규모 자산을 호스팅해도 연간 비용에 큰 영향을 주는 경우가 거의 없습니다. 몇 가지 예외를 제외하고, 자산의 정리 및 사용 중지로 얻는 비용 절감 효과보다 실제 자산을 분석하고 사용 중지하는 데 드는 FTE 활동이 더 많습니다.

하지만 클라우드 회계 모델로 이동할 경우 자산을 사용 중지하면 연간 운영 비용 및 사전 마이그레이션 활동을 대폭 줄일 수 있습니다.

조직에서 정량적 분석을 완료한 후 디지털 자산의 20% 이상을 사용 중지하는 경우는 별로 없습니다. 이러한 작업을 결정하기 전에 추가적인 정성적 분석을 권장합니다. 확인되면 자산을 사용 중지하여 클라우드 마이그레이션에서 처음으로 만족스러운 ROI를 얻을 수 있습니다. 이것은 여러 상황에서 가장 큰 비용 절감 요소 중 하나입니다. 이처럼 클라우드 전략 팀은 마이그레이션 프로세스의 빌드 단계와 동시에 자산의 유효성 검사 및 사용 중지를 감독하여 초기에 재무적 성과를 얻는 것이 좋습니다.

### <a name="program-adjustments"></a>프로그램 조정

기업이 한 가지 변환 과정만 시작하는 경우는 거의 없습니다. 비용 절감, 시장 성장, 새로운 매출원 간의 선택은 거의 대부분 이진 의사 결정이 아닙니다. 따라서 클라우드 전략 팀은 변환 활동과 동시에 IT와 협력하여 주요 변환 과정의 범위 밖에 있는 자산을 식별하는 것이 좋습니다.

다음은 이 문서에서 사용되는 IaaS 마이그레이션 예제입니다.

- DevOps 팀에 이미 배포 자동화에 포함된 자산을 식별하여 핵심 마이그레이션 계획에서 빼 달라고 요청합니다.

- 데이터 및 R&D 팀에 새로운 매출원을 개척하는 자산을 식별하여 핵심 마이그레이션 계획에서 빼 달라고 요청합니다.

이 프로그램 중심의 정성적 분석은 신속하게 실행할 수 있으며 여러 마이그레이션 백로그 간에 조정됩니다.

일부 자산은 한동안 자산을 다시 호스트하고, 초기 마이그레이션이 끝나면 이후 합리화 과정에서 단계적으로 처리해야 할 수도 있습니다.

## <a name="selecting-the-first-workload"></a>첫 번째 워크로드 선택

테스트 및 학습의 핵심은 첫 번째 워크로드를 구현하는 것입니다. 성장 마인드셋을 보여주고 함양하는 첫 번째 기회입니다.

### <a name="business-criteria"></a>비즈니스 조건

클라우드 전략 팀의 사업부 멤버가 지원하는 워크로드를 식별하여 비즈니스 투명성을 확보합니다. 가급적이면 팀과 관련이 깊고 클라우드로 전환해야 하는 강력한 동기를 부여하는 워크로드를 선택하는 것이 좋습니다.

### <a name="technical-criteria"></a>기술 조건

종속성이 가장 적고 자산의 일부 그룹으로 이동할 수 있는 워크로드를 선택합니다. 유효성 검사를 쉽게 하려면 테스트 경로가 정의된 워크로드를 선택하는 것이 좋습니다.

첫 번째 워크로드는 운영 또는 거버넌스 용량이 없는 실험 환경에 종종 배포됩니다. 보안 데이터와 상호 작용하지 않는 워크로드를 선택하는 것이 매우 중요합니다.

### <a name="qualitative-analysis"></a>정성적 분석

클라우드 도입 및 클라우드 전략 팀은 서로 협력하여 소규모 워크로드를 분석할 수 있습니다. 이렇게 하면 통제된 기회 속에서 정성적 분석 기준을 만들고 테스트할 수 있습니다. 모집단이 작으면 영향을 받는 사용자를 대상으로 설문 조사를 실시하여 일주일 내에 자세한 정성적 분석을 완료할 수 있습니다. 일반적인 정성적 분석 요인의 경우 [합리화의 5가지 R](5-rs-of-rationalization.md)에서 특정 합리화 대상을 참조하세요.

### <a name="migration"></a>마이그레이션

클라우드 도입 팀은 지속적인 합리화를 진행하는 동시에 다음과 같은 주요 영역에서 소규모 워크로드를 마이그레이션하여 학습을 확장할 수 있습니다.

- 클라우드 공급 기업의 플랫폼에 대한 기술을 강화합니다.
- 장기적 비전에 필요한 핵심 서비스(및 Azure 표준)를 정의합니다.
- 변환의 이후 과정에서 필요한 작업의 발전 방향을 정확하게 이해합니다.
- 내재된 비즈니스 위험과 이러한 위험에 대한 기업의 허용 한도를 이해합니다.
- 기업의 위험 허용 한도에 따라 기준선 또는 MVP(minimum viable product)를 설정합니다.

## <a name="release-planning"></a>릴리스 계획

클라우드 도입 팀이 첫 번째 워크로드를 마이그레이션 또는 구현하는 동안 클라우드 전략 팀은 나머지 애플리케이션/워크로드의 우선 순위를 지정할 수 있습니다.

### <a name="power-of-ten"></a>10의 제곱

기존의 합리화 방식은 쓸데없는 일에 시간을 낭비합니다. 다행스럽게도 종종 모든 애플리케이션에 대한 계획이 없어도 변환 과정을 시작할 수 있습니다. 증분 모델에서는 10의 제곱이 좋은 시작점입니다. 이 모델에서는 클라우드 전략 팀이 맨 처음에 마이그레이션할 10개 애플리케이션을 선택합니다. 이 10개 워크로드에는 단순 워크로드와 복합 워크로드가 포함되어야 합니다.

### <a name="building-the-first-backlogs"></a>첫 번째 백로그를 작성

클라우드 도입 팀과 클라우드 전략 팀은 공동으로 정성적 분석을 실시하여 처음 10개 워크로드를 선택할 수 있습니다. 이렇게 하면 우선 순위가 가장 높은 마이그레이션 백로그와 우선 순위가 가장 높은 릴리스 백로그가 만들어집니다. 이 방법을 사용하면 팀이 같은 과정을 반복할 수 있으며 적절한 정성적 분석 프로세스를 만들기에 충분한 시간이 제공됩니다.

### <a name="maturing-the-process"></a>프로세스 성숙

두 팀이 정성적 분석 조건에 동의하면 각 반복 사이에 평가 작업을 수행할 수 있습니다. 평가 기준에 대한 합의에 도달하려면 일반적으로 2&ndash;3개 릴리스가 필요합니다.

평가가 마이그레이션의 증분 실행 프로세스로 이동되 면 클라우드 도입 팀은 평가 및 아키텍처를 더 빠르게 반복할 수 있습니다. 이 단계에서는 클라우드 전략 팀 역시 추상화되어 시간 낭비를 줄일 수 있습니다. 또한 이렇게 하면 클라우드 전략 팀은 아직 특정 릴리스에 포함되지 않은 애플리케이션의 우선 순위를 지정하는 일에 집중할 수 있으며, 따라서 변화하는 시장 상황에 보다 적극적으로 대응할 수 있습니다.

우선 순위가 지정된 모든 애플리케이션을 즉시 마이그레이션할 수 있는 것은 아닙니다. 팀이 심층적인 정성적 분석을 통해 백로그 우선 순위를 다시 지정해야 하는 비즈니스 이벤트 및 종속성을 발견하면 시퀀싱이 변할 수 있습니다. 워크로드 몇 개를 그룹화하는 릴리스도 있고, 단일 워크로드만 포함하는 릴리스도 있습니다.

클라우드 도입 팀은 완전한 워크로드 마이그레이션을 생성하지 않는 반복을 실행할 가능성이 있습니다. 워크로드가 작을수록 종속성이 적고, 워크로드가 단일 스프린트 또는 반복에 적합할 가능성이 높습니다. 이러한 이유로, 릴리스 백로그에 포함되는 첫 번째 애플리케이션의 크기가 작고 외부 종속성이 없는 것이 좋습니다.

## <a name="end-state"></a>최종 상태

시간이 지나면 클라우드 도입 팀과 클라우드 전략 팀이 완전한 전체 인벤토리 합리화를 완료할 것입니다. 그러나 이 증분 방법에서는 팀이 합리화 프로세스에서 지속적으로 속도를 높일 수 있습니다. 또한 사전 분석 활동 없이 변환 과정을 통해 실질적인 비즈니스 결과를 금방 얻을 수 있습니다.

경우에 따라 재무 모델이 너무 엄격하여 추가 합리화 없이는 의사 결정이 불가능할 수 있습니다. 이 경우 보다 전통적인 합리화 방법이 필요할 수 있습니다.

## <a name="next-steps"></a>다음 단계

합리화 활동의 결과는 선택한 변환의 영향을 받게 될 모든 자산의 우선 순위가 지정된 백로그입니다. 이 백로그를 클라우드 서비스의 비용 모델 기초로 사용할 수 있습니다.

> [!div class="nextstepaction"]
> [비용 모델을 디지털 자산에 맞춰 조정](calculate.md)