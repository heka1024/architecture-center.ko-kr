---
title: 'CAF: 클라우드 전환을 위한 금융 모델 만들기'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
description: 클라우드 전환을 위한 금융 모델을 만드는 방법입니다.
author: BrianBlanchard
ms.date: 12/10/2018
ms.topic: guide
ms.openlocfilehash: 1f3ed8a84b84ba577ad5e5db8b1becd318dc04a3
ms.sourcegitcommit: 0a8a60d782facc294f7f78ec0e9033e3ee16bf4a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068874"
---
# <a name="create-a-financial-model-for-cloud-transformation"></a><span data-ttu-id="93167-103">클라우드 전환을 위한 금융 모델 만들기</span><span class="sxs-lookup"><span data-stu-id="93167-103">Create a financial model for cloud transformation</span></span>

<span data-ttu-id="93167-104">클라우드 전환의 전체 비즈니스 가치를 정확히 나타내는 금융 모델을 만드는 과정은 복잡할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-104">Creating a financial model that accurately represents the full business value of any cloud transformation can be complicated.</span></span> <span data-ttu-id="93167-105">금융 모델 및 비즈니스 근거는 조직마다 다른 경우가 많습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-105">Financial models and business justifications tend to be different from one organization to the next.</span></span> <span data-ttu-id="93167-106">이 문서에서는 금융 모델을 만들 때 일반적으로 누락되는 몇 가지 요소와 수식을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="93167-106">This article establishes some formulas and points out a few things that are commonly missed when creating a financial model.</span></span>

## <a name="return-on-investment-roi"></a><span data-ttu-id="93167-107">투자 수익률(ROI)</span><span class="sxs-lookup"><span data-stu-id="93167-107">Return on investment (ROI)</span></span>

<span data-ttu-id="93167-108">투자 수익률(ROI)은 임원진이나 이사회에서 중요한 기준으로 삼는 경우가 많습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-108">Return on investment (ROI) is often an important criteria with the C-Suite or the board.</span></span> <span data-ttu-id="93167-109">ROI는 제한된 자본 리소스를 투자하는 데 사용되는 다양한 방법을 비교하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="93167-109">ROI is used to compare different ways to invest limited capital resources.</span></span> <span data-ttu-id="93167-110">ROI 수식은 매우 간단합니다.</span><span class="sxs-lookup"><span data-stu-id="93167-110">The formula for ROI is fairly simple.</span></span> <span data-ttu-id="93167-111">각 수식 입력을 만드는 데 필요한 세부 정보는 간단하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-111">The details required to create each input to the formula may not be as simple.</span></span> <span data-ttu-id="93167-112">기본적으로, ROI는 초기 투자에서 창출된 수익의 규모입니다.</span><span class="sxs-lookup"><span data-stu-id="93167-112">Essentially, ROI is the amount of return produced from an initial investment.</span></span> <span data-ttu-id="93167-113">일반적으로 백분율로 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="93167-113">Usually it is represented as a percentage:</span></span>

![투자 수익률(ROI) = (투자 수익 – 투자 비용) / 투자 비용](../_images/formula-roi.png)

<!-- markdownlint-disable MD036 -->
<!--*ROI = (Gain from Investment &minus; Initial Investment) / Initial Investment*-->
<!-- markdownlint-enable MD036 -->

<span data-ttu-id="93167-115">다음 섹션에서는 초기 투자 및 투자 수익(소득)을 계산하는 데 필요한 데이터를 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-115">In the next sections, we will walk through the data needed to calculate the initial investment and the gain from investment (earnings).</span></span>

## <a name="calculating-initial-investment"></a><span data-ttu-id="93167-116">초기 투자 계산</span><span class="sxs-lookup"><span data-stu-id="93167-116">Calculating initial investment</span></span>

<span data-ttu-id="93167-117">초기 투자는 전환을 완료하는 데 필요한 자본 지출(CapEx)과 운영 지출(OpEx)을 말합니다.</span><span class="sxs-lookup"><span data-stu-id="93167-117">Initial investment is the capital expenditure (CapEx) and operating expenditure (OpEx) required to complete a transformation.</span></span> <span data-ttu-id="93167-118">회계 모델과 CFO의 기호에 따라 비용의 분류가 달라질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-118">The classification of costs can vary depending on accounting models and CFO preference.</span></span> <span data-ttu-id="93167-119">그러나 이 범주에는 전환할 전문가 서비스, 전환 중에만 사용되는 소프트웨어 라이선스, 전환 중 클라우드 서비스 비용, 전환 중 샐러리맨 비용 등이 포함될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-119">However, this category would include things like: Professional services to transform, software licenses that are used solely during the transformation, cost of cloud services during the transformation, and potentially the cost of the salaried employees during the transformation.</span></span>

<span data-ttu-id="93167-120">이러한 비용을 합산하여 초기 투자의 예상치를 계산하세요.</span><span class="sxs-lookup"><span data-stu-id="93167-120">Add these costs together to create an estimate of the initial investment.</span></span>

## <a name="calculating-the-gain-from-investment"></a><span data-ttu-id="93167-121">투자 수익 계산</span><span class="sxs-lookup"><span data-stu-id="93167-121">Calculating the gain from investment</span></span>

<span data-ttu-id="93167-122">투자 수익을 계산할 때는 비즈니스 성과 및 관련 기술 변경 사항과 관련이 높은 두 번째 계산 수식이 필요한 경우가 많습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-122">Gain from investment often requires a second formula for calculation, which is very specific to the business outcomes and associated technical changes.</span></span> <span data-ttu-id="93167-123">수익은 비용 절감액을 계산하는 것만큼 단순하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-123">Earnings are not as simple as calculating reduction in costs.</span></span>

<span data-ttu-id="93167-124">수익을 계산하려면 두 가지 변수가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="93167-124">To calculate earnings, two variables are required:</span></span>

![투자 수익 = 수익 델타 + 비용 델타](../_images/formula-gain-from-investment.png)

<!-- markdownlint-disable MD036 -->
<!--*Gain from Investment = Revenue Deltas + Cost Deltas*-->
<!-- markdownlint-enable MD036 -->

<span data-ttu-id="93167-126">각 항목에 대한 설명은 아래와 같습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-126">Each is described below.</span></span>

## <a name="revenue-delta"></a><span data-ttu-id="93167-127">수익 델타</span><span class="sxs-lookup"><span data-stu-id="93167-127">Revenue delta</span></span>

<span data-ttu-id="93167-128">수익 델타는 비즈니스와 함께 예측해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="93167-128">Revenue delta should be forecasted in partnership with the business.</span></span> <span data-ttu-id="93167-129">비즈니스 이해 관계자가 수익 영향에 동의하면 이를 사용하여 수익 상태를 개선할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-129">Once the business stakeholders agree on a revenue impact, that can be used to improve the earning position.</span></span>

## <a name="cost-deltas"></a><span data-ttu-id="93167-130">비용 델타</span><span class="sxs-lookup"><span data-stu-id="93167-130">Cost deltas</span></span>

<span data-ttu-id="93167-131">비용 델타는 전환의 결과로 얻게 될 증가량 또는 감소량입니다.</span><span class="sxs-lookup"><span data-stu-id="93167-131">Cost deltas are the amount of increase or decrease that will come as a result of the transformation.</span></span> <span data-ttu-id="93167-132">델타 비용에 영향을 줄 수 있는 독립 변수의 여러 가지가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-132">There are a number of independent variables that can affect cost deltas.</span></span> <span data-ttu-id="93167-133">수익은 주로 자본 비용 절감, 비용 방지, 비용 회피, 운영 비용 절감 및 감가상각 축소와 같은 하드 비용을 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="93167-133">Earnings are largely based on hard costs like capital expense reductions, cost avoidance, operational cost reductions, and depreciation reductions.</span></span> <span data-ttu-id="93167-134">다음 섹션은 고려해야 할 비용 델타의 예입니다.</span><span class="sxs-lookup"><span data-stu-id="93167-134">The following sections are examples of cost deltas to consider.</span></span>

### <a name="depreciation-reductions-or-acceleration"></a><span data-ttu-id="93167-135">감가상각 축소 또는 가속</span><span class="sxs-lookup"><span data-stu-id="93167-135">Depreciation reductions or acceleration</span></span>

<span data-ttu-id="93167-136">감가상각에 대한 지침은 CFO 또는 재무팀에게 문의하세요.</span><span class="sxs-lookup"><span data-stu-id="93167-136">For guidance on depreciation, speak with the CFO or finance team.</span></span> <span data-ttu-id="93167-137">다음은 감가상각 항목에 대한 일반적인 참고 자료로 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-137">The following is meant to serve as a general reference on the topic of depreciation.</span></span>

<span data-ttu-id="93167-138">자산 구입을 위해 자본을 투자한 경우 해당 투자금을 재무 또는 세무 용도로 사용하여 자산의 예상 수명 동안 지속적인 혜택을 창출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-138">When capital is invested in the acquisition of an asset, that investment could be used for financial or tax purposes to produce ongoing benefits over the expected lifespan of the asset.</span></span> <span data-ttu-id="93167-139">일부 회사에서는 감가상각을 긍정적인 절세 혜택으로 간주합니다.</span><span class="sxs-lookup"><span data-stu-id="93167-139">Some companies see depreciation as a positive tax advantage.</span></span> <span data-ttu-id="93167-140">다른 회사에서는 이를 연간 IT 예산에서 기인한 기타 반복 지출과 유사한 확정된 지속적 지출로 간주합니다.</span><span class="sxs-lookup"><span data-stu-id="93167-140">Others see it as committed, ongoing expense similar to other recurring expenses attributed to the annual IT budget.</span></span>

<span data-ttu-id="93167-141">경리과에 문의하여 감가상각 제거가 가능하고, 비용 델타에 긍정적인 기여를 하는지 확인하세요.</span><span class="sxs-lookup"><span data-stu-id="93167-141">Speak with the finance office to see if elimination of depreciation is possible, and if it would make a positive contribution to cost deltas.</span></span>

### <a name="physical-asset-recovery"></a><span data-ttu-id="93167-142">물리적 자산 회수</span><span class="sxs-lookup"><span data-stu-id="93167-142">Physical asset recovery</span></span>

<span data-ttu-id="93167-143">경우에 따라 사용 중지된 자산을 판매하여 재원을 확보할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-143">In some cases, retired assets can be sold as a source of revenue.</span></span> <span data-ttu-id="93167-144">이러한 수익은 편의상 비용 절감액으로 묶이는 경우가 많습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-144">Often, this revenue is lumped into cost reduction for simplicity.</span></span> <span data-ttu-id="93167-145">그러나 실제로 수익 증가로 볼 수 있으며 이에 따라 세금이 부과될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-145">However, it's truly an increase in revenue and may be taxed as such.</span></span> <span data-ttu-id="93167-146">경리과에 문의하여 이러한 옵션의 실용성과 결과 수익을 고려하는 방법을 파악하세요.</span><span class="sxs-lookup"><span data-stu-id="93167-146">Speak with the finance office to understand the viability of this option and how to account for the resulting revenue.</span></span>

### <a name="operational-cost-reductions"></a><span data-ttu-id="93167-147">운영 비용 절감</span><span class="sxs-lookup"><span data-stu-id="93167-147">Operational cost reductions</span></span>

<span data-ttu-id="93167-148">비즈니스를 운영하는 데 필요한 반복 지출을 OpEx(운영 비용)라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="93167-148">Recurring expenses required to operate the business are often referred to as operational expenses (OpEx).</span></span> <span data-ttu-id="93167-149">OpEx는 매우 광범위한 범주입니다.</span><span class="sxs-lookup"><span data-stu-id="93167-149">OpEx is a very broad category.</span></span> <span data-ttu-id="93167-150">대부분의 회계 모델에는 소프트웨어 라이선스, 호스팅 비용, 전자 청구서, 부동산 임대, 냉방비, 운영에 필요한 임시직, 장비 임대, 교체 부품, 유지 관리 계약, 수리 서비스, BC/DR(Business Continuity/Disaster Recovery) 서비스 및 기타 자본 비용 승인이 필요 없는 여러 경비가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="93167-150">In most accounting models, it would include software licensing, hosting expenses, electric bills, real estate rentals, cooling expenses, temporary staff required for operations, equipment rentals, replacement parts, maintenance contracts, repair services, Business Continuity/Disaster Recovery (BC/DR) services, and a number of other expenses that don't require capital expense approvals.</span></span>

<span data-ttu-id="93167-151">이 범주에 작업 변환을 고려 하는 경우 가장 큰 수익 영역 중 하나입니다.</span><span class="sxs-lookup"><span data-stu-id="93167-151">This category is one of the largest earnings areas when considering an Operational Transformation.</span></span> <span data-ttu-id="93167-152">이 목록을 철저히 작성하는 데 투자하는 시간은 절대 시간 낭비가 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="93167-152">Time invested in making this list exhaustive is seldom wasted.</span></span> <span data-ttu-id="93167-153">CIO와 경리과에 질문하여 모든 운영 비용이 고려되었는지 확인하세요.</span><span class="sxs-lookup"><span data-stu-id="93167-153">Ask questions of the CIO and finance team to ensure all operational costs are accounted for.</span></span>

### <a name="cost-avoidance"></a><span data-ttu-id="93167-154">비용 회피</span><span class="sxs-lookup"><span data-stu-id="93167-154">Cost avoidance</span></span>

<span data-ttu-id="93167-155">OpEx(운영 비용)가 예상되지만 아직 승인된 예산에 없을 경우 비용 절감액 범주에 적절하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-155">When an operational expense (OpEx) is expected, but not yet in an approved budget, it may not fit into a cost reduction category.</span></span> <span data-ttu-id="93167-156">예를 들어 내년에 VMWare 및 Microsoft 라이선스를 다시 협상하고 비용을 지불해야 하는 경우 아직 완전한 자격을 갖춘 비용은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="93167-156">For instance, if VMWare and Microsoft licenses need to be renegotiated and paid next year, they aren't fully qualified costs yet.</span></span> <span data-ttu-id="93167-157">이러한 예상 비용 절감액은 비용 델타 계산을 위해 운영 비용으로 취급됩니다.</span><span class="sxs-lookup"><span data-stu-id="93167-157">Reductions in those expected costs would be treated like operational costs for the sake of cost delta calculations.</span></span> <span data-ttu-id="93167-158">하지만 협상 및 예산 승인이 완료될 때까지는 비공식적으로 "비용 회피"라고 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="93167-158">Informally, however, they should be referred to as "cost avoidance," until negotiation and budget approval is complete.</span></span>

### <a name="soft-cost-reductions"></a><span data-ttu-id="93167-159">소프트 비용 절감</span><span class="sxs-lookup"><span data-stu-id="93167-159">Soft cost reductions</span></span>

<span data-ttu-id="93167-160">일부 회사에서는 운영 복잡성 감소 또는 데이터 센터 운영에 필요한 정규직 감소와 같은 소프트 비용도 포함될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-160">In some companies, soft costs such as reductions in operational complexity or reduction in full-time staff to operate a datacenter could also be included.</span></span> <span data-ttu-id="93167-161">그러나 소프트 비용을 포함하는 것은 문제의 소지가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-161">However, including soft costs can be ill-advised.</span></span> <span data-ttu-id="93167-162">소프트 비용을 포함하면 감소한 비용이 실질적인 비용 절감액과 같다는 검증되지 않은 가정을 불러올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-162">Including soft costs inserts an undocumented assumption that the reduction in costs will equate to tangible cost savings.</span></span> <span data-ttu-id="93167-163">기술 프로젝트는 실질적인 소프트 비용 회수가 거의 발생하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-163">Technology projects seldom result in actual soft cost recovery.</span></span>

### <a name="headcount-reductions"></a><span data-ttu-id="93167-164">인원수 축소</span><span class="sxs-lookup"><span data-stu-id="93167-164">Headcount reductions</span></span>

<span data-ttu-id="93167-165">직원이 절약한 시간은 소프트 비용 절감에 포함되는 경우가 많습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-165">Time savings for staff are often included under soft cost reduction.</span></span> <span data-ttu-id="93167-166">이러한 시간 절약이 IT 급여 또는 파견의 실질적 감소에 해당하는 경우 인원수 감소와 별도로 계산될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-166">When those time savings map to actual reduction of IT salary or staffing, it could be calculated separately as a headcount reduction.</span></span>

<span data-ttu-id="93167-167">즉, 온-프레미스에서 필요한 기술은 일반적으로 클라우드에서 필요한(또는 그 이상의) 유사 기술 세트에 해당합니다.</span><span class="sxs-lookup"><span data-stu-id="93167-167">That said, the skills needed on-premises generally map to a similar (or higher level) set of skills needed in the cloud.</span></span> <span data-ttu-id="93167-168">즉, 직원들은 일반적으로 클라우드 마이그레이션 후 일시 해고되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-168">That means people generally don't get laid off after a cloud migration.</span></span>

<span data-ttu-id="93167-169">타사 또는 MSP(관리되는 공급자)에서 운영 능력을 제공하는 경우는 예외입니다.</span><span class="sxs-lookup"><span data-stu-id="93167-169">An exception is when operational capacity is provided by a third party or managed services provider (MSP).</span></span> <span data-ttu-id="93167-170">타사에서 IT 시스템을 관리하는 경우 운영 비용이 클라우드 네이티브 솔루션 또는 클라우드 네이티브 MSP로 바뀔 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-170">If IT systems are managed by a third party, the costs to operate could be replaced by a cloud-native solution or cloud-native MSP.</span></span> <span data-ttu-id="93167-171">클라우드 네이티브 MSP는 더 저렴한 비용으로 더 효율적인 운영을 할 가능성이 높습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-171">A cloud native MSP is likely to operate more efficiently and potentially at a lower cost.</span></span> <span data-ttu-id="93167-172">이 경우 운영 비용 절감액은 하드 비용 계산에 속합니다.</span><span class="sxs-lookup"><span data-stu-id="93167-172">If that's the case, operational cost reductions belong in the hard cost calculations.</span></span>

### <a name="capital-expense-reductions-or-avoidance"></a><span data-ttu-id="93167-173">자본 비용 절감 또는 회피</span><span class="sxs-lookup"><span data-stu-id="93167-173">Capital expense reductions or avoidance</span></span>

<span data-ttu-id="93167-174">CapEx(자본 비용)는 운영 비용과 약간 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="93167-174">Capital expenses (CapEx) are slightly different that operational expenses.</span></span> <span data-ttu-id="93167-175">일반적으로 이 범주는 갱신 주기 또는 데이터 센터 확장에 좌우됩니다.</span><span class="sxs-lookup"><span data-stu-id="93167-175">Generally, this category is driven by refresh cycles or datacenter expansion.</span></span> <span data-ttu-id="93167-176">데이터 센터 확장의 예로 빅 데이터 솔루션 또는 데이터 웨어하우스를 호스트하며, 일반적으로 CapEx 범주에 속하는 새로운 고성능 클러스터가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-176">An example of a datacenter expansion would be a new high-performance cluster to host a Big Data solution or data warehouse, and would generally fit into a CapEx category.</span></span> <span data-ttu-id="93167-177">기본 갱신 주기가 더 일반적입니다.</span><span class="sxs-lookup"><span data-stu-id="93167-177">More common are the basic refresh cycles.</span></span> <span data-ttu-id="93167-178">일부 회사에서는 주기를 새로 고침 하는 엄격한 하드웨어가 의미 자산 사용 중지 및 일반 주기 (일반적으로 3, 5, 8 년 마다) 중에 대체 됩니다.</span><span class="sxs-lookup"><span data-stu-id="93167-178">Some companies have rigid hardware refresh cycles, meaning assets are retired and replaced on a regular cycle (usually every three, five, or eight years).</span></span> <span data-ttu-id="93167-179">이러한 주기는 자산 임대 주기 또는 장비의 예상 수명과 일치하는 경우가 많습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-179">These cycles often coincide with asset lease cycles or forecasted lifespan of equipment.</span></span> <span data-ttu-id="93167-180">갱신 주기가 도래하면 IT에서 CapEx를 인출하여 새 장비를 구입합니다.</span><span class="sxs-lookup"><span data-stu-id="93167-180">When a refresh cycle hits, IT draws CapEx to acquire new equipment.</span></span>

<span data-ttu-id="93167-181">갱신 주기가 승인되고 예산이 책정되면 클라우드 전환을 통해 비용을 회피할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-181">If a refresh cycle is approved and budgeted, the Cloud Transformation could help eliminate that cost.</span></span> <span data-ttu-id="93167-182">갱신 주기가 계획되었으나 아직 승인되지 않은 경우 클라우드 전환에서 CapEx 비용 회피를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93167-182">If a refresh cycle is planned but not yet approved, the Cloud Transformation could create a CapEx cost avoidance.</span></span> <span data-ttu-id="93167-183">두 시나리오는 모두 비용 델타에 추가됩니다.</span><span class="sxs-lookup"><span data-stu-id="93167-183">Both scenarios would be added to the cost delta.</span></span>

## <a name="next-steps"></a><span data-ttu-id="93167-184">다음 단계</span><span class="sxs-lookup"><span data-stu-id="93167-184">Next steps</span></span>

<span data-ttu-id="93167-185">클라우드 전환의 컨텍스트에서 몇 가지 회계 결과의 예에 대해 알아봅니다.</span><span class="sxs-lookup"><span data-stu-id="93167-185">Read some example fiscal outcomes in the context of a cloud transformation.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="93167-186">회계 결과의 예</span><span class="sxs-lookup"><span data-stu-id="93167-186">Examples of fiscal outcomes</span></span>](./business-outcomes/fiscal-outcomes.md)