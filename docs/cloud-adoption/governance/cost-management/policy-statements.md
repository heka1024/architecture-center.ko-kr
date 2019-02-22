---
title: 'CAF: 비용 관리 샘플 정책 설명'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 1/4/2019
description: 비용 관리 샘플 정책 설명
author: BrianBlanchard
ms.openlocfilehash: 1c8b94ae5285fa26cdf9760892beaced2487af8b
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55902056"
---
# <a name="cost-management-sample-policy-statements"></a><span data-ttu-id="13323-103">비용 관리 샘플 정책 설명</span><span class="sxs-lookup"><span data-stu-id="13323-103">Cost Management sample policy statements</span></span>

<span data-ttu-id="13323-104">개별 클라우드 정책 설명은 위험 평가 프로세스 중에 파악된 특정 위험을 해결하기 위한 지침입니다.</span><span class="sxs-lookup"><span data-stu-id="13323-104">Individual cloud policy statements are guidelines for addressing specific risks identified during your risk assessment process.</span></span> <span data-ttu-id="13323-105">이러한 설명은 위험 및 위험을 해결하기 위한 계획의 간략한 요약을 제공해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="13323-105">These statements should provide a concise summary of risks and plans to deal with them.</span></span> <span data-ttu-id="13323-106">각 설명 정의에는 다음 정보가 포함되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="13323-106">Each statement definition should include these pieces of information:</span></span>

- <span data-ttu-id="13323-107">비즈니스 위험 - 이 정책이 처리할 위험의 요약입니다.</span><span class="sxs-lookup"><span data-stu-id="13323-107">Business risk - A summary of the risk this policy will address.</span></span>
- <span data-ttu-id="13323-108">정책 설명 - 정책 요구 사항의 명확한 요약 설명입니다.</span><span class="sxs-lookup"><span data-stu-id="13323-108">Policy statement - A clear summary explanation of the policy requirements.</span></span>
- <span data-ttu-id="13323-109">디자인 옵션 - IT 팀과 개발자가 정책 구현 시에 사용할 수 있는 실행 가능한 권장 사항, 사양 또는 기타 지침입니다.</span><span class="sxs-lookup"><span data-stu-id="13323-109">Design options - Actionable recommendations, specifications, or other guidance that IT teams and developers can use when implementing the policy.</span></span>

<span data-ttu-id="13323-110">여러 비용 관련 일반 비즈니스 위험을 해결하는 다음 샘플 정책 설명은 사용자 조직의 요구를 해결하는 실제 정책 설명의 초안을 작성할 때 참조할 수 있도록 예제로 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="13323-110">The following sample policy statements address a number of common cost-related business risks, and are provided as examples for you to reference when drafting actual policy statements addressing your own organization's needs.</span></span> <span data-ttu-id="13323-111">특정 위험을 해결하기 위해 반드시 이러한 예제를 사용해야 하는 것은 아니며, 확인된 한 가지 위험을 해결할 수 있도록 여러 정책 옵션이 제공될 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="13323-111">These examples are not meant to be proscriptive, and there are potentially several policy options for dealing with any single identified risk.</span></span> <span data-ttu-id="13323-112">실무 팀 및 IT 팀과 긴밀하게 협력하여 특정 비용 관련 위험에 가장 적합한 정책 솔루션을 선택하세요.</span><span class="sxs-lookup"><span data-stu-id="13323-112">Work closely with business and IT teams to identify the best policy solutions for your particular cost-related risks.</span></span>  

## <a name="future-proofing"></a><span data-ttu-id="13323-113">향후 상황 예측</span><span class="sxs-lookup"><span data-stu-id="13323-113">Future-proofing</span></span>

<span data-ttu-id="13323-114">**비즈니스 위험**.</span><span class="sxs-lookup"><span data-stu-id="13323-114">**Business risk**.</span></span> <span data-ttu-id="13323-115">거버넌스 팀의 비용 관리 분야 투자가 보장되지 않는 현재 기준입니다.</span><span class="sxs-lookup"><span data-stu-id="13323-115">Current criteria that don't warrant an investment in a Cost Management discipline from the governance team.</span></span> <span data-ttu-id="13323-116">그러나 이후에는 투자가 진행될 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="13323-116">However, you anticipate such an investment in the future.</span></span>

<span data-ttu-id="13323-117">**정책 설명**.</span><span class="sxs-lookup"><span data-stu-id="13323-117">**Policy statement**.</span></span> <span data-ttu-id="13323-118">클라우드에 배포된 모든 자산을 청구 단위, 애플리케이션/워크로드와 연결해야 하며 명명 표준을 준수해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="13323-118">You should associate all assets deployed to the cloud with a billing unit, application/workload, and meet naming standards.</span></span> <span data-ttu-id="13323-119">이 정책을 적용하면 향후 비용 관리 작업의 효율성을 높일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="13323-119">This policy will ensure that future Cost Management efforts will be effective.</span></span>

<span data-ttu-id="13323-120">**디자인 옵션**.</span><span class="sxs-lookup"><span data-stu-id="13323-120">**Design options**.</span></span> <span data-ttu-id="13323-121">향후 상황 예측의 기초 설정과 관련된 자세한 내용은 CAF 지침의 일부로 포함된 [실행 가능한 디자인 가이드](../journeys/overview.md)에서 거버넌스 MVP 작성 관련 설명을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="13323-121">For information on establishing a future-proof foundation, see the discussions related to creating a governance MVP in the [actionable design guides](../journeys/overview.md) included as part of the CAF guidance.</span></span>

## <a name="budget-overruns"></a><span data-ttu-id="13323-122">예산 초과</span><span class="sxs-lookup"><span data-stu-id="13323-122">Budget overruns</span></span>

<span data-ttu-id="13323-123">**비즈니스 위험:** 셀프 서비스 배포에서는 초과 지출 위험이 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="13323-123">**Business risk:** Self-service deployment creates a risk of overspending.</span></span>

<span data-ttu-id="13323-124">**정책 설명:** 모든 클라우드 배포는 승인된 예산 및 예산 제한 메커니즘이 포함된 청구 단위에 할당해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="13323-124">**Policy statement:** Any cloud deployment must be allocated to a billing unit with approved budget and a mechanism for budgetary limits.</span></span>

<span data-ttu-id="13323-125">**디자인 옵션:** Azure에서는 [Azure Cost Management](/azure/cost-management/manage-budgets)를 통해 예산을 제어할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="13323-125">**Design options:** In Azure, budget can be controlled with [Azure Cost Management](/azure/cost-management/manage-budgets)</span></span>

## <a name="underutilization"></a><span data-ttu-id="13323-126">미달 사용</span><span class="sxs-lookup"><span data-stu-id="13323-126">Underutilization</span></span>

<span data-ttu-id="13323-127">**비즈니스 위험:** 회사에서 클라우드 서비스 비용을 선불 결제했거나 일정량의 서비스를 사용하는 연간 약정에 가입했습니다.</span><span class="sxs-lookup"><span data-stu-id="13323-127">**Business risk:** The company has prepaid for cloud services or has made an annual commitment to spend a specific amount.</span></span> <span data-ttu-id="13323-128">이 경우 합의된 사용량을 사용하지 않아 투자 금액이 손실될 위험이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="13323-128">There is a risk that the agreed upon amount won't be used, resulting in a lost investment.</span></span>

<span data-ttu-id="13323-129">**정책 설명:** 클라우드 예산이 할당된 각 청구 단위의 사용량을 매년 확인해 예산을 설정하고, 분기별로 확인해 예산을 조정하고, 매월 확인해 계획한 지출과 실제 지출을 비교 검토할 시간을 할당합니다.</span><span class="sxs-lookup"><span data-stu-id="13323-129">**Policy statement:** Each billing unit with an allocated cloud budget will meet annually to set budgets, quarterly to adjust budgets, and monthly to allocate time for reviewing planned versus actual spending.</span></span> <span data-ttu-id="13323-130">매달 청구 책임자와 함께 계획한 지출과 실제 지출의 편차가 20%를 초과하는 클라우드 서비스 관련 논의를 진행합니다.</span><span class="sxs-lookup"><span data-stu-id="13323-130">Discuss any deviations greater than 20% with the billing unit leader monthly.</span></span> <span data-ttu-id="13323-131">추적용으로 모든 자산을 청구 단위에 할당합니다.</span><span class="sxs-lookup"><span data-stu-id="13323-131">For tracking purposes, assign all assets to a billing unit.</span></span>

<span data-ttu-id="13323-132">**디자인 옵션:**</span><span class="sxs-lookup"><span data-stu-id="13323-132">**Design options:**</span></span>

- <span data-ttu-id="13323-133">Azure에서는 [Azure Cost Management](/azure/cost-management/quick-acm-cost-analysis)를 통해 계획한 지출과 실제 지출을 관리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="13323-133">In Azure, planned versus actual spending can be managed via [Azure Cost Management](/azure/cost-management/quick-acm-cost-analysis)</span></span>
- <span data-ttu-id="13323-134">다양한 옵션을 사용해 청구 단위를 기준으로 리소스를 그룹화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="13323-134">There are several options for grouping resources by billing unit.</span></span> <span data-ttu-id="13323-135">Azure에서는 거버넌스 팀과 함께 [리소스 일관성 모델](../../decision-guides/resource-consistency/overview.md)을 선택하여 모든 자산에 적용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="13323-135">In Azure, a [resource consistency model](../../decision-guides/resource-consistency/overview.md) should be chosen in conjunction with the governance team and applied to all assets.</span></span>

## <a name="overprovisioned-assets"></a><span data-ttu-id="13323-136">초과 프로비전된 자산</span><span class="sxs-lookup"><span data-stu-id="13323-136">Overprovisioned assets</span></span>

<span data-ttu-id="13323-137">**비즈니스 위험:** 기존 온-프레미스 데이터 센터에서는 장기적인 확장을 위한 추가 용량 계획을 통해 자산을 배포하는 방식이 일반적으로 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="13323-137">**Business risk:** In traditional on-premises datacenters, it is common practice to deploy assets with extra capacity planning for growth in the distant future.</span></span> <span data-ttu-id="13323-138">클라우드는 기존 장비에 비해 더 빠르게 크기를 조정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="13323-138">The cloud can scale more quickly than traditional equipment.</span></span> <span data-ttu-id="13323-139">클라우드의 자산 역시 기술적 용량에 따라 가격이 책정됩니다.</span><span class="sxs-lookup"><span data-stu-id="13323-139">Assets in the cloud are also priced based on the technical capacity.</span></span> <span data-ttu-id="13323-140">이전의 온-프레미스 방식으로 인해 클라우드 지출이 불필요하게 증가할 위험이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="13323-140">There is a risk of the old on-premises practice artificially inflating cloud spending.</span></span>

<span data-ttu-id="13323-141">**정책 설명:** 클라우드에 배포된 모든 자산은 사용률을 모니터링하고 사용률이 50%를 초과하는 용량을 보고할 수 있는 프로그램에 등록해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="13323-141">**Policy statement:** Any asset deployed to the cloud must be enrolled in a program that can monitor utilization and report any capacity in excess of 50% of utilization.</span></span> <span data-ttu-id="13323-142">또한 클라우드에 배포된 모든 자산은 논리적으로 그룹화하거나 태그를 지정해야 합니다. 그래야 거버넌스 팀 구성원이 초과 프로비전된 자산 최적화와 관련한 작업을 워크로드 소유자에게 요청할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="13323-142">Any asset deployed to the cloud must be grouped or tagged in a logical manner, so governance team members can engage the workload owner regarding any optimization of overprovisioned assets.</span></span>

<span data-ttu-id="13323-143">**디자인 옵션:**</span><span class="sxs-lookup"><span data-stu-id="13323-143">**Design options:**</span></span>

- <span data-ttu-id="13323-144">Azure에서는 [Azure Advisor](/azure/advisor/advisor-cost-recommendations)가 최적화 권장 사항을 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="13323-144">In Azure, [Azure Advisor](/azure/advisor/advisor-cost-recommendations) can provide optimization recommendations.</span></span>
- <span data-ttu-id="13323-145">다양한 옵션을 사용해 청구 단위를 기준으로 리소스를 그룹화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="13323-145">There are several options for grouping resources by billing unit.</span></span> <span data-ttu-id="13323-146">Azure에서는 거버넌스 팀과 함께 [리소스 일관성 모델](../../decision-guides/resource-consistency/overview.md)을 선택하여 모든 자산에 적용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="13323-146">In Azure, a [resource consistency model](../../decision-guides/resource-consistency/overview.md) should be chosen in conjunction with the governance team and applied to all assets.</span></span>

## <a name="overoptimization"></a><span data-ttu-id="13323-147">과도한 최적화</span><span class="sxs-lookup"><span data-stu-id="13323-147">Overoptimization</span></span>

<span data-ttu-id="13323-148">**비즈니스 위험:** 비용 관리를 효율적으로 수행하더라도 새로운 위험이 실제로 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="13323-148">**Business risk:** Effective cost management can actually create new risks.</span></span> <span data-ttu-id="13323-149">지출 최적화는 시스템 성능을 저하시키는 작업이기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="13323-149">Optimization of spending is inverse to system performance.</span></span> <span data-ttu-id="13323-150">비용을 줄이는 경우 지출이 지나치게 긴축되어 사용자 환경의 품질이 저하될 위험이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="13323-150">When reducing costs, there is a risk of overtightening spending and producing poor user experiences.</span></span>

<span data-ttu-id="13323-151">**정책 설명:** 그룹화나 태그 지정을 통해 고객 환경에 직접 영향을 주는 모든 자산을 직접 확인해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="13323-151">**Policy statement:** Any asset that directly affects customer experiences must be identified through grouping or tagging.</span></span> <span data-ttu-id="13323-152">고객 환경에 영향을 주는 모든 자산을 최적화하기 전에 클라우드 거버넌스 팀이 지난 90일 이내의 사용률 추세를 기준으로 하여 최적화를 조정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="13323-152">Before optimizing any asset that affects customer experience, the Cloud Governance team must adjust optimization based on no fewer than 90 days of utilization trends.</span></span> <span data-ttu-id="13323-153">자산을 최적화할 때 고려한 특정 계절이나 이벤트 관련 버스트를 문서로 작성합니다.</span><span class="sxs-lookup"><span data-stu-id="13323-153">Document any seasonal or event driven bursts considered when optimizing assets.</span></span>

<span data-ttu-id="13323-154">**디자인 옵션:**</span><span class="sxs-lookup"><span data-stu-id="13323-154">**Design options:**</span></span>

- <span data-ttu-id="13323-155">Azure에서는 [Azure Monitor의 인사이트 기능](/azure/azure-monitor/insights/vminsights-performance)을 통해 시스템 사용률을 분석할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="13323-155">In Azure, [Azure Monitor's insights features](/azure/azure-monitor/insights/vminsights-performance) can help with analysis of system utilization.</span></span>
- <span data-ttu-id="13323-156">다양한 옵션을 사용해 역할을 기준으로 리소스를 그룹화하고 태그를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="13323-156">There are several options for grouping and tagging resources based on roles.</span></span> <span data-ttu-id="13323-157">Azure에서는 거버넌스 팀과 함께 [리소스 일관성 모델](../../decision-guides/resource-consistency/overview.md)을 선택하여 모든 자산에 적용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="13323-157">In Azure, you should choose a [resource consistency model](../../decision-guides/resource-consistency/overview.md) in conjunction with the governance team and apply this to all assets.</span></span>

## <a name="next-steps"></a><span data-ttu-id="13323-158">다음 단계</span><span class="sxs-lookup"><span data-stu-id="13323-158">Next steps</span></span>

<span data-ttu-id="13323-159">이 문서에서 설명한 샘플을 토대로 하여 특정 비즈니스 위험을 해결하고 클라우드 채택 계획에 적합한 정책을 개발합니다.</span><span class="sxs-lookup"><span data-stu-id="13323-159">Use the samples mentioned in this article as a starting point to develop policies that address specific business risks that align with your cloud adoption plans.</span></span>

<span data-ttu-id="13323-160">비용 관리 관련 사용자 지정 정책 설명 개발을 시작하려면 [비용 관리 템플릿](template.md)을 다운로드하세요.</span><span class="sxs-lookup"><span data-stu-id="13323-160">To begin developing your own custom policy statements related to Cost Management, download the [Cost Management template](template.md).</span></span>

<span data-ttu-id="13323-161">이 분야를 신속하게 채택하려면 사용자 환경에 가장 가깝게 일치하는 [실행 가능한 거버넌스 경험](../journeys/overview.md)을 선택한 다음</span><span class="sxs-lookup"><span data-stu-id="13323-161">To accelerate adoption of this discipline, choose the [Actionable Governance Journey](../journeys/overview.md) that most closely aligns with your environment.</span></span> <span data-ttu-id="13323-162">디자인을 수정하여 회사의 구체적인 정책 결정 사항을 통합합니다.</span><span class="sxs-lookup"><span data-stu-id="13323-162">Then modify the design to incorporate your specific corporate policy decisions.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="13323-163">실행 가능한 거버넌스 경험</span><span class="sxs-lookup"><span data-stu-id="13323-163">Actionable Governance Journeys</span></span>](../journeys/overview.md)
