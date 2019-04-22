---
title: 'CAF: 비용 관리 정책 준수 프로세스'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 비용 관리 정책 준수 프로세스
author: BrianBlanchard
ms.openlocfilehash: 5fd007546589020f376107382c54c391cc5d05ad
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901330"
---
# <a name="cost-management-policy-compliance-processes"></a><span data-ttu-id="18ce2-103">비용 관리 정책 준수 프로세스</span><span class="sxs-lookup"><span data-stu-id="18ce2-103">Cost Management policy compliance processes</span></span>

<span data-ttu-id="18ce2-104">이 문서에서는 [비용 관리](./overview.md) 거버넌스 분야를 지원하는 프로세스를 만드는 방식을 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-104">This article discusses an approach to creating processes that support a [Cost Management](./overview.md) governance discipline.</span></span> <span data-ttu-id="18ce2-105">클라우드 비용을 효율적으로 제어하려면 먼저 정책 준수를 지원할 수 있는 반복 수동 프로세스를 만들어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-105">Effective governance of cloud costs starts with recurring manual processes designed to support policy compliance.</span></span> <span data-ttu-id="18ce2-106">이렇게 하려면 클라우드 거버넌스 팀과 실무 팀의 관련자가 정기적으로 정책을 검토/업데이트하고 정책 준수 여부를 확인해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-106">This requires regular involvement of the Cloud Governance team and interested business stakeholders to review and update policy and ensure policy compliance.</span></span> <span data-ttu-id="18ce2-107">또한 도구를 사용해 진행 중인 대부분의 모니터링 및 적용 프로세스를 자동화하거나 보충하면 거버넌스의 오버헤드를 줄이고 정책 미준수 상황에 더욱 빠르게 대응할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-107">In addition, many ongoing monitoring and enforcement processes can be automated or supplemented with tooling to reduce the overhead of governance and allow for faster response to policy deviation.</span></span>

## <a name="planning-review-and-reporting-processes"></a><span data-ttu-id="18ce2-108">계획, 검토 및 보고 프로세스</span><span class="sxs-lookup"><span data-stu-id="18ce2-108">Planning, review, and reporting processes</span></span>

<span data-ttu-id="18ce2-109">클라우드에서 제공되는 비용 관리 도구는 지원 대상 프로세스와 정책에 사용하는 경우에만 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-109">The best Cost Management tools in the cloud are only as good as the processes and policies that they support.</span></span> <span data-ttu-id="18ce2-110">아래에는 비용 관리 분야에서 일반적으로 수행되는 예제 프로세스 세트가 나와 있습니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-110">The following is a set of example processes commonly involved in the Cost Management discipline.</span></span> <span data-ttu-id="18ce2-111">비용 거버넌스 지침에 따라 업무 팀의 피드백 및 업무 변경 내용을 기준으로 비용 정책을 계속 업데이트할 수 있는 프로세스를 계획할 때 이러한 예제를 참조할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-111">Use these examples as a starting point when planning the processes that will allow you to continue to update cost policy based on business change and feedback from the business teams subject to cost governance guidance.</span></span>

<span data-ttu-id="18ce2-112">**초기 위험 평가 및 계획**: 비용 관리 분야를 초기 채택할 때 클라우드 비용과 관련된 주요 비즈니스 위험 및 허용 범위를 파악합니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-112">**Initial risk assessment and planning**: As part of your initial adoption of the Cost Management discipline, identify your core business risks and tolerances related to cloud costs.</span></span> <span data-ttu-id="18ce2-113">이 정보를 사용해 업무 팀 구성원들과 예산 및 비용 관련 위험을 논의하고 이러한 위험을 완화하는 기준 정책 세트를 개발해 초기 거버넌스 전략을 수립합니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-113">Use this information to discuss budget and cost-related risks with members of your business teams and develop a baseline set of policies for mitigating these risks to establish your initial governance strategy.</span></span>

<span data-ttu-id="18ce2-114">**배포 계획:** 모든 자산을 배포하기 전에 예상 클라우드 할당량을 기준으로 하여 예산을 예측합니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-114">**Deployment planning:** Prior to deployment of any asset, establish a forecasted budget based on expected cloud allocation.</span></span> <span data-ttu-id="18ce2-115">그리고 배포 소유권 및 회계 정보를 문서로 작성합니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-115">Ensure that ownership and accounting information for the deployment is documented.</span></span>  

<span data-ttu-id="18ce2-116">**연간 계획:** 매년 배포된 모든 자산 및 배포할 자산에 대한 롤업 분석을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-116">**Annual planning:** On an annual basis, perform a roll-up analysis on all deployed and to-be-deployed assets.</span></span> <span data-ttu-id="18ce2-117">그런 다음 셀프 서비스 방식의 채택이 가능하도록 사업부, 부서, 팀 및 기타 적절한 부문별로 예산을 조정합니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-117">Align budgets by business units, departments, teams, and other appropriate divisions to empower self-service adoption.</span></span> <span data-ttu-id="18ce2-118">그리고 각 청구 단위의 리더가 예산 및 및 지출 추적 방식을 알고 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-118">Ensure that the leader of each billing unit is aware of the budget and how to track spending.</span></span>

<span data-ttu-id="18ce2-119">또한 이 단계에서 최대한 할인을 받을 수 있도록 사전 약정이나 사전 구매를 진행합니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-119">This is the time to make a precommitment or prepurchase to maximize discounting.</span></span> <span data-ttu-id="18ce2-120">연말 할인 옵션을 통해 더 많은 할인을 받을 수 있도록 클라우드 공급업체의 회계 연도에 맞춰 예산을 책정하면 효율적입니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-120">It is wise to align annual budgeting with the cloud vendor's fiscal year to further capitalize on year-end discount options.</span></span>

<span data-ttu-id="18ce2-121">**분기별 계획:** 분기별로 각 청구 단위 리더와 함께 예산을 검토해 예측한 예산과 실제 지출액을 비교합니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-121">**Quarterly planning:** On a quarterly basis, review budgets with each billing unit leader to align forecast and actual spending.</span></span> <span data-ttu-id="18ce2-122">계획이 변경되었거나 예기치 않은 지출 패턴이 확인된 경우에는 예산을 적절하게 조정하여 다시 할당합니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-122">If there are changes to the plan or unexpected spending patterns, align and reallocate the budget.</span></span>

<span data-ttu-id="18ce2-123">또한 이 분기별 계획 프로세스에서 현재 또는 향후 사업 계획과 관련한 현재 클라우드 거버넌스 팀 구성원들의 지식 격차를 평가하면 효율적입니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-123">This quarterly planning process is also a good time to evaluate the current membership of your Cloud Governance team for knowledge gaps related to current or future business plans.</span></span> <span data-ttu-id="18ce2-124">관련 담당자와 워크로드 소유자가 임시 고문 또는 팀의 영구 구성원으로 검토와 계획에 참여하도록 초대합니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-124">Invite relevant staff and workload owners to participate in reviews and planning as either temporary advisors or permanent members of your team.</span></span>

<span data-ttu-id="18ce2-125">**교육과 학습**: 실무자와 IT 담당자가 최신 비용 관리 정책 요구 사항을 파악할 수 있도록 2개월마다 학습 세션을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-125">**Education and Training**: On a bi-monthly basis, offer training sessions to make sure business and IT staff are up-to-date on the latest Cost Management policy requirements.</span></span> <span data-ttu-id="18ce2-126">이 프로세스의 일환으로 설명서, 지침 또는 기타 학습 자산을 검토하고 최신 기업 정책 설명과 일치하도록 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-126">As part of this process review and update any documentation, guidance, or other training assets to ensure they are in sync with the latest corporate policy statements.</span></span>

<span data-ttu-id="18ce2-127">**월별 보고:** 매월 예측 예산과 실제 지출액의 비교 수치를 보고합니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-127">**Monthly reporting:** On a monthly basis, report actual spending against forecast.</span></span> <span data-ttu-id="18ce2-128">예상치 못한 차이가 확인되면 청구 담당 리더에게 알립니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-128">Notify billing leaders of any unexpected deviations.</span></span>

<span data-ttu-id="18ce2-129">이러한 기본 프로세스를 진행하면 예산을 조정하고 비용 관리 분야의 기준을 마련할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-129">These basic processes will help align spending and establish a foundation for the Cost Management discipline.</span></span>

## <a name="ongoing-monitoring-processes"></a><span data-ttu-id="18ce2-130">지속적인 모니터링 프로세스</span><span class="sxs-lookup"><span data-stu-id="18ce2-130">Ongoing monitoring processes</span></span>

<span data-ttu-id="18ce2-131">과거, 현재 및 향후의 계획된 클라우드 관련 지출을 얼마나 명확하게 파악할 수 있는지에 따라 비용 관리 거버넌스 전략의 성패가 좌우된다고 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-131">A successful Cost Management governance strategy depends on visibility into the past, current, and planned future cloud-related spending.</span></span> <span data-ttu-id="18ce2-132">기존 비용 관련 메트릭과 데이터를 분석할 수 없다면 위험 변경을 파악하거나 위험 허용 범위 위반 사항을 검색할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-132">Without the ability to analyze the relevant metrics and data of your existing costs, you cannot identify changes in your risks or detect violations of your risk tolerances.</span></span> <span data-ttu-id="18ce2-133">위에서 설명한 지속적인 거버넌스 프로세스를 수행하려면 품질 데이터가 필요합니다. 이 데이터가 있어야 변화하는 비즈니스 요구 사항과 클라우드 사용량에 따라 인프라를 더 효율적으로 보호하기 위해 정책을 수정할 수 있기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-133">The ongoing governance processes discussed above require quality data to ensure policy can be modified to better protect your infrastructure against changing business requirements and cloud usage.</span></span>

<span data-ttu-id="18ce2-134">IT 팀이 클라우드 지출과 사용량을 모니터링해 예상 비용에서 계획되지 않은 차이가 발생했는지를 파악하기 위한 자동화된 시스템을 구현했는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-134">Ensure that your IT teams have implemented automated systems for monitoring your cloud spending and usage for unplanned deviations from expected costs.</span></span> <span data-ttu-id="18ce2-135">그리고 정책 위반 기능성을 즉시 검색하고 완화할 수 있는 보고 및 경고 시스템을 구축합니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-135">Establish reporting and alerting systems to ensure prompt detection and mitigation of potential policy violations.</span></span>

## <a name="compliance-violation-triggers-and-enforcement-actions"></a><span data-ttu-id="18ce2-136">규정 준수 위반 트리거 및 적용 작업</span><span class="sxs-lookup"><span data-stu-id="18ce2-136">Compliance violation triggers and enforcement actions</span></span>

<span data-ttu-id="18ce2-137">위반 사항이 검색되면 정책을 다시 준수하는 상태가 되도록 정책 적용 조치를 취해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-137">When violations are detected, you should take enforcement actions to realign with policy.</span></span> <span data-ttu-id="18ce2-138">[Azure용 비용 관리 도구 체인](toolchain.md)에 요약되어 있는 도구를 사용하면 대부분의 위반 트리거를 자동화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-138">You can automate most violation triggers using the tools outlined in the [Cost Management toolchain for Azure](toolchain.md).</span></span>

<span data-ttu-id="18ce2-139">트리거의 예는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-139">The following are examples of triggers:</span></span>

* <span data-ttu-id="18ce2-140">월별 예산 편차: 예측 예산 대 실제 지출 비율이 20%를 초과하는 월별 지출 편차가 확인되면 청구 단위 리더와 협의하여</span><span class="sxs-lookup"><span data-stu-id="18ce2-140">Monthly budget deviations: Discuss any deviations in monthly spending that exceed 20% forecast-versus-actual ratio with the billing unit leader.</span></span> <span data-ttu-id="18ce2-141">해결 방법과 예측 변경 내용을 기록합니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-141">Record resolutions and changes in forecast.</span></span>
* <span data-ttu-id="18ce2-142">채택 속도: 구독 수준에서 편차가 20%를 초과하면 청구 단위 리더와의 검토 작업이 트리거됩니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-142">Pace of adoption: Any deviation at a subscription level exceeding 20% will trigger a review with billing unit leader.</span></span> <span data-ttu-id="18ce2-143">이 작업에서 해결 방법과 예측 변경 내용을 기록합니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-143">Record resolutions and changes in forecast.</span></span>

## <a name="next-steps"></a><span data-ttu-id="18ce2-144">다음 단계</span><span class="sxs-lookup"><span data-stu-id="18ce2-144">Next steps</span></span>

<span data-ttu-id="18ce2-145">[클라우드 관리 템플릿](./template.md)을 사용하여 현재 클라우드 채택 계획에 적합한 프로세스와 트리거를 문서로 작성합니다.</span><span class="sxs-lookup"><span data-stu-id="18ce2-145">Using the [Cloud Management template](./template.md), document the processes and triggers that align to the current cloud adoption plan.</span></span>

<span data-ttu-id="18ce2-146">채택 계획에 따라 클라우드 관리 정책을 실행하는 지침은 비용 관리 분야 개선 관련 문서를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="18ce2-146">For guidance on executing cloud management policies in alignment with adoption plans, see the article on Cost Management discipline improvement.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="18ce2-147">비용 관리 분야 개선</span><span class="sxs-lookup"><span data-stu-id="18ce2-147">Cost Management discipline improvement</span></span>](./discipline-improvement.md)