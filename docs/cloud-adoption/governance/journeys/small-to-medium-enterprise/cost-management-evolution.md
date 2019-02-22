---
title: 'CAF: 중소기업 – Cost Management 진화  '
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 2/11/2019
description: 중소기업 설명 – Cost Management 진화
author: BrianBlanchard
ms.openlocfilehash: ca070bfca3efbf9548faa8cf28a1adffefd4a33a
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901285"
---
# <a name="small-to-medium-enterprise-cost-management-evolution"></a><span data-ttu-id="4fc38-103">중소기업: Cost Management 진화</span><span class="sxs-lookup"><span data-stu-id="4fc38-103">Small-to-medium enterprise: Cost Management evolution</span></span>

<span data-ttu-id="4fc38-104">이 문서에서는 거버넌스 MVP에 비용 컨트롤을 추가하여 서술식 여정을 발전시킵니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-104">This article evolves the narrative by adding cost controls to the governance MVP.</span></span>

## <a name="evolution-of-the-narrative"></a><span data-ttu-id="4fc38-105">서술식 여정의 발전</span><span class="sxs-lookup"><span data-stu-id="4fc38-105">Evolution of the narrative</span></span>

<span data-ttu-id="4fc38-106">도입 규모가 거버넌스 MVP에 정의된 비용 허용 오차 지표를 초과하여 증가했습니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-106">Adoption has grown beyond the cost tolerance indicator defined in the governance MVP.</span></span> <span data-ttu-id="4fc38-107">"DR" 데이터 센터의 마이그레이션과 부합하므로 이는 좋은 현상입니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-107">This is a good thing, as it corresponds with migrations from the "DR" datacenter.</span></span> <span data-ttu-id="4fc38-108">이제 지출이 증가하면 클라우드 거버넌스 팀이 시간을 투자해야 하는 합당한 명분이 생겼습니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-108">The increase in spending now justifies an investment of time from the Cloud Governance team.</span></span>

### <a name="evolution-of-the-current-state"></a><span data-ttu-id="4fc38-109">현재 상태의 발전</span><span class="sxs-lookup"><span data-stu-id="4fc38-109">Evolution of the current state</span></span>

<span data-ttu-id="4fc38-110">이 서술식 여정의 이전 단계에서, IT는 DR 데이터 센터를 100% 사용 중지했습니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-110">In the previous phase of this narrative, IT had retired 100% of the DR datacenter.</span></span> <span data-ttu-id="4fc38-111">애플리케이션 개발 및 BI 팀은 프로덕션 트래픽을 처리할 준비가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-111">The application development and BI teams were ready for production traffic.</span></span>

<span data-ttu-id="4fc38-112">그 후로 거버넌스에 영향을 주는 몇 가지 변화가 있었습니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-112">Since then, some things have changed that will affect governance:</span></span>

- <span data-ttu-id="4fc38-113">마이그레이션 팀이 프로덕션 데이터 센터에서 VM을 마이그레이션하기 시작했습니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-113">The migration team has begun migrating VMs out of the production datacenter.</span></span>
- <span data-ttu-id="4fc38-114">애플리케이션 개발 팀은 CI/CD 파이프라인을 통해 적극적으로 프로덕션 애플리케이션을 클라우드로 푸시하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-114">The application development teams is actively pushing production applications to the cloud through CI/CD pipelines.</span></span> <span data-ttu-id="4fc38-115">이러한 애플리케이션은 사용자의 요구에 대응하여 크기를 조정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-115">Those applications can reactively scale with user demands.</span></span>
- <span data-ttu-id="4fc38-116">IT 내부의 비즈니스 인텔리전스 팀은 클라우드에 다양한 예측 분석 도구를 제공해 왔습니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-116">The business intelligence team within IT has delivered a number of predictive analytics tools in the cloud.</span></span> <span data-ttu-id="4fc38-117">클라우드에 집계되는 데이터 볼륨이 계속 증가합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-117">the volumes of data aggregated in the cloud continues to grow.</span></span>
- <span data-ttu-id="4fc38-118">이러한 증가는 커밋된 비즈니스 결과를 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-118">All of this growth supports committed business outcomes.</span></span> <span data-ttu-id="4fc38-119">그러나 비용이 급격하게 늘어나기 시작했습니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-119">However, costs have begun to mushroom.</span></span> <span data-ttu-id="4fc38-120">추정 예산이 예상보다 빠르게 늘어나고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-120">Projected budgets are growing faster than expected.</span></span> <span data-ttu-id="4fc38-121">CFO는 향상된 비용 관리 방법이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-121">The CFO needs improved approaches to managing costs.</span></span>

### <a name="evolution-of-the-future-state"></a><span data-ttu-id="4fc38-122">향후 상태의 발전</span><span class="sxs-lookup"><span data-stu-id="4fc38-122">Evolution of the future state</span></span>

<span data-ttu-id="4fc38-123">비용 모니터링 및 보고 기능이 클라우드 솔루션에 추가됩니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-123">Cost monitoring and reporting is to be added to the cloud solution.</span></span> <span data-ttu-id="4fc38-124">IT는 계속해서 비용 클리어링 하우스 역할을 합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-124">IT is still serving as a cost clearing house.</span></span> <span data-ttu-id="4fc38-125">다시 말해서, 클라우드 서비스 요금 결제는 계속해서 IT 조달로 처리됩니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-125">This means that payment for cloud services continues to come from IT procurement.</span></span> <span data-ttu-id="4fc38-126">그러나 보고는 클라우드 비용을 소비하는 직무의 직접 운영 비용으로 처리해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-126">However, reporting should tie direct operational expenses to the functions that are consuming the cloud costs.</span></span> <span data-ttu-id="4fc38-127">이 모델을 클라우드 계정에 대한 "쇼백" 모델이라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-127">This model is referred to as "Show Back" model to cloud accounting.</span></span>

<span data-ttu-id="4fc38-128">현재와 미래의 상태가 변하면 새로운 위험에 노출되고, 그에 따라 새로운 정책 설명이 필요하게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-128">The changes to current and future state expose new risks that will require new policy statements.</span></span>

## <a name="evolution-of-tangible-risks"></a><span data-ttu-id="4fc38-129">실질적인 위험의 심화</span><span class="sxs-lookup"><span data-stu-id="4fc38-129">Evolution of tangible risks</span></span>

<span data-ttu-id="4fc38-130">**예산 제어**: 셀프 서비스 기능으로 인해 새 플랫폼에서 예상치 않게 과도한 비용이 발생할 수 있는 위험이 내재되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-130">**Budget control**: There is an inherent risk that self-service capabilities will result in excessive and unexpected costs on the new platform.</span></span> <span data-ttu-id="4fc38-131">계획한 예산과 계속 맞춰 나가려면 비용을 모니터링하고 비용 위험을 지속적으로 완화하는 거버넌스 프로세스가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-131">Governance processes for monitoring costs and mitigating ongoing cost risks must be in place to ensure continued alignment with the planned budget.</span></span>

<span data-ttu-id="4fc38-132">이 비즈니스 위험은 다음과 같은 몇 가지 기술적 위험으로 확장될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-132">This business risk can be expanded into a few technical risks:</span></span>

- <span data-ttu-id="4fc38-133">실제 비용이 계획을 초과할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-133">Actual costs might exceed the plan.</span></span>
- <span data-ttu-id="4fc38-134">비즈니스 조건이 변합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-134">Business conditions change.</span></span> <span data-ttu-id="4fc38-135">이렇게 되면 비즈니스 직무에서 예상보다 많은 클라우드 서비스를 소비해야 하므로 이례적인 지출이 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-135">When they do, there will be cases when a business function needs to consume more cloud services than expected, leading to spending anomalies.</span></span> <span data-ttu-id="4fc38-136">이 추가 지출이 계획에 필요한 조정 지출이 아닌 초과 지출로 간주될 위험이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-136">There is a risk that this extra spending will be considered overages, as opposed to a necessary adjustment to the plan.</span></span>
- <span data-ttu-id="4fc38-137">시스템이 오버프로비저닝되어 초과 지출이 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-137">Systems could be overprovisioned, resulting in excess spending.</span></span>

## <a name="evolution-of-the-policy-statements"></a><span data-ttu-id="4fc38-138">정책 설명의 개선</span><span class="sxs-lookup"><span data-stu-id="4fc38-138">Evolution of the policy statements</span></span>

<span data-ttu-id="4fc38-139">정책을 다음과 같이 변경하면 새로운 위험을 완화하고 구현 과정을 안내할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-139">The following changes to policy will help mitigate the new risks and guide implementation.</span></span>

1. <span data-ttu-id="4fc38-140">거버넌스 팀은 모든 클라우드 비용을 매주 계획과 비교하여 모니터링해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-140">All cloud costs should be monitored against plan on a weekly basis by the governance team.</span></span> <span data-ttu-id="4fc38-141">클라우드 비용과 계획 사이의 편차에 대한 보고서를 IT 책임자 및 재무 담당자와 매월 공유해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-141">Reporting on deviations between cloud costs and plan is to be shared with IT leadership and finance monthly.</span></span> <span data-ttu-id="4fc38-142">모든 클라우드 비용 및 계획 업데이트를 IT 책임자 및 재무 담당자가 매월 검토해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-142">All cloud costs and plan updates should be reviewed with IT leadership and finance monthly.</span></span>
2. <span data-ttu-id="4fc38-143">책임 소재를 명확히 하기 위해 모든 비용을 비즈니스 직무에 할당해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-143">All costs must be allocated to a business function for accountability purposes.</span></span>
3. <span data-ttu-id="4fc38-144">클라우드 자산을 지속적으로 모니터링하여 최적화가 필요한지 확인해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-144">Cloud assets should be continually monitored for optimization opportunities.</span></span>
4. <span data-ttu-id="4fc38-145">클라우드 거버넌스 도구는 자산 크기 조정 옵션을 승인된 구성 목록으로 제한해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-145">Cloud Governance tooling must limit Asset sizing options to an approved list of configurations.</span></span> <span data-ttu-id="4fc38-146">도구는 비용 모니터링 솔루션을 통해 모든 자산을 검색하고 추적할 수 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-146">The tooling must ensure that all assets are discoverable and tracked by the cost monitoring solution.</span></span>
5. <span data-ttu-id="4fc38-147">배포를 계획하는 동안 프로덕션 워크로드 호스팅과 관련된 필수 클라우드 리소스를 문서화해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-147">During deployment planning, any required cloud resources associated with the hosting of production workloads should be documented.</span></span> <span data-ttu-id="4fc38-148">이 문서는 예산을 재정비하고 비싼 옵션을 사용하지 못하도록 추가 자동화를 준비하는 데 도움이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-148">This documentation will help refine budgets and prepare additional automation to prevent the use of more expensive options.</span></span> <span data-ttu-id="4fc38-149">이 과정에서 클라우드 공급 기업이 제공하는 다양한 할인 도구(예: 예약 인스턴스 또는 라이선스 비용 절감)를 고려해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-149">During this process consideration should be given to different discounting tools offered by the cloud provider, such as reserved instances or license cost reductions.</span></span>
6. <span data-ttu-id="4fc38-150">모든 애플리케이션 소유자는 클라우드 비용을 보다 잘 제어하기 위해 워크로드를 최적화하는 교육에 참석해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-150">All application owners are required to attend trained on practices for optimizing workloads to better control cloud costs.</span></span>

## <a name="evolution-of-the-best-practices"></a><span data-ttu-id="4fc38-151">모범 사례 개선</span><span class="sxs-lookup"><span data-stu-id="4fc38-151">Evolution of the best practices</span></span>

<span data-ttu-id="4fc38-152">이 문서 섹션에서는 Azure Cost Management 구현 및 새로운 Azure 정책을 포함하도록 거버넌스 MVP 디자인을 개선합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-152">This section of the article will evolve the governance MVP design to include new Azure policies and an implementation of Azure Cost Management.</span></span> <span data-ttu-id="4fc38-153">이 두 가지 디자인 변경을 통해 새로운 기업 정책 설명을 처리하게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-153">Together, these two design changes will fulfill the new corporate policy statements.</span></span>

1. <span data-ttu-id="4fc38-154">Azure Cost Management 구현</span><span class="sxs-lookup"><span data-stu-id="4fc38-154">Implement Azure Cost Management</span></span>
    1. <span data-ttu-id="4fc38-155">구독 패턴 및 리소스 일관성 분야에 맞게 적절한 액세스 범위를 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-155">Establish the right scope of access to align with the subscription pattern and the Resource Consistency discipline.</span></span> <span data-ttu-id="4fc38-156">이전 문서에 정의된 거버넌스 MVP에 맞게 조정한다고 가정하면, 간략한 보고를 수행하는 클라우드 거버넌스 팀에는 **등록 계정 범위** 액세스 권한이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-156">Assuming alignment with the governance MVP defined in prior articles, this requires **Enrollment Account Scope** access for the Cloud Governance team executing on high-level reporting.</span></span> <span data-ttu-id="4fc38-157">거버넌스 이외의 팀에는 **리소스 그룹 범위** 액세스 권한이 필요할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-157">Additional teams outside of governance may require **Resource Group Scope** access.</span></span>
    2. <span data-ttu-id="4fc38-158">Azure Cost Management에서 예산을 수립합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-158">Establish a budget in Azure Cost Management.</span></span>
    3. <span data-ttu-id="4fc38-159">초기 추천을 검토하고 이행합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-159">Review and act on initial recommendations.</span></span> <span data-ttu-id="4fc38-160">보고를 지원하는 반복 프로세스를 갖춥니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-160">Have a recurring process to support reporting.</span></span>
    4. <span data-ttu-id="4fc38-161">초기에 그리고 반복적으로 Azure Cost Management 보고를 구성하고 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-161">Configure and execute Azure Cost Management Reporting, both initial and recurring.</span></span>
2. <span data-ttu-id="4fc38-162">Azure Policy 업데이트</span><span class="sxs-lookup"><span data-stu-id="4fc38-162">Update Azure Policy</span></span>
    1. <span data-ttu-id="4fc38-163">태그 지정, 관리 그룹, 구독 및 리소스 그룹 값을 감사하여 편차가 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-163">Audit the tagging, management group, subscription, and resource group values to identify any deviation.</span></span>
    2. <span data-ttu-id="4fc38-164">SKU 크기 옵션을 설정하여 배포 계획 설명서에 나열된 SKU로 배포를 제한합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-164">Establish SKU size options to limit deployments to SKUs listed in deployment planning documentation.</span></span>

## <a name="conclusion"></a><span data-ttu-id="4fc38-165">결론</span><span class="sxs-lookup"><span data-stu-id="4fc38-165">Conclusion</span></span>

<span data-ttu-id="4fc38-166">이러한 프로세스와 변경 사항을 거버넌스 MVP에 추가하면 비용 거버넌스와 연관된 많은 위험을 완화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-166">Adding these processes and changes to the governance MVP helps mitigate many of the risks associated with cost governance.</span></span> <span data-ttu-id="4fc38-167">이 모든 것이 합쳐져서 비용을 제어하는 데 필요한 가시성, 책임성 및 최적화를 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-167">Together, they create the visibility, accountability, and optimization needed to control costs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4fc38-168">다음 단계</span><span class="sxs-lookup"><span data-stu-id="4fc38-168">Next steps</span></span>

<span data-ttu-id="4fc38-169">클라우드 도입 과정이 계속 발전하고 비즈니스 가치가 추가되면서 위험 및 클라우드 거버넌스 요구도 함께 발전합니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-169">As cloud adoption continues to evolve and deliver additional business value, risks and cloud governance needs will also evolve.</span></span> <span data-ttu-id="4fc38-170">이 여정에 등장하는 가상의 회사가 그 다음으로 수행할 단계는 이 거버넌스 투자를 사용하여 여러 클라우드를 관리하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="4fc38-170">For the fictional company in this journey, the next step is using this governance investment to manage multiple clouds.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="4fc38-171">다중 클라우드 발전</span><span class="sxs-lookup"><span data-stu-id="4fc38-171">Multi-cloud evolution</span></span>](./multi-cloud-evolution.md)
