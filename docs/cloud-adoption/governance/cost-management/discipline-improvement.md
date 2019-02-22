---
title: 'CAF: 비용 관리 분야 개선'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 비용 관리 분야 개선
author: BrianBlanchard
ms.openlocfilehash: 34975d195a95b1a85ada96efe8c76a6138385ec1
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55902160"
---
# <a name="cost-management-discipline-improvement"></a><span data-ttu-id="11639-103">비용 관리 분야 개선</span><span class="sxs-lookup"><span data-stu-id="11639-103">Cost Management discipline improvement</span></span>

<span data-ttu-id="11639-104">비용 관리 분야에서는 클라우드 기반 워크로드를 호스트할 때 발생하는 경비와 관련된 주요 비즈니스 위험을 해결하고자 합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-104">The Cost Management discipline attempts to address core business risks related to expenses incurred when hosting cloud-based workloads.</span></span> <span data-ttu-id="11639-105">클라우드 거버넌스의 5가지 분야 중 비용 관리 분야에서는 계획된 비용 주기를 생성하고 유지 관리하기 위해 클라우드 리소스의 비용과 사용량을 제어합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-105">Within the five disciplines of Cloud Governance, Cost Management is involved in controlling cost and usage of cloud resources with the goal of creating and maintaining a planned cost cycle.</span></span>

<span data-ttu-id="11639-106">이 문서에서는 회사에서 비용 관리 분야를 개발하고 완성하기 위해 수행할 수 있는 작업을 간략하게 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-106">This article outlines potential tasks your company perform to develop and mature your Cost Management discipline.</span></span> <span data-ttu-id="11639-107">이러한 작업은 클라우드 솔루션 구현 과정의 계획/빌드/채택/운영 단계로 구분할 수 있습니다. [증분 방식 클라우드 거버넌스](../journeys/overview.md#an-incremental-approach-to-cloud-governance) 개발을 허용하는 과정에서 이러한 단계를 반복적으로 수행하게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="11639-107">These tasks can be broken down into planning, building, adopting, and operating phases of implementing a cloud solution, which are then iterated on allowing the development of an [incremental approach to cloud governance](../journeys/overview.md#an-incremental-approach-to-cloud-governance).</span></span>

![채택의 4단계](../../_images/adoption-phases.png)

<span data-ttu-id="11639-109">*그림 1. 증분 방식 클라우드 거버넌스의 채택 단계*</span><span class="sxs-lookup"><span data-stu-id="11639-109">*Figure 1. Adoption phases of the incremental approach to cloud governance.*</span></span>

<span data-ttu-id="11639-110">한 문서에서 모든 기업의 요구 사항을 설명할 수는 없습니다.</span><span class="sxs-lookup"><span data-stu-id="11639-110">No single document can account for the requirements of all businesses.</span></span> <span data-ttu-id="11639-111">따라서 이 문서에서는 거버넌스 완성 프로세스의 각 단계에서 권장되는 최소 활동과 수행할 가능성이 있는 활동의 예를 간략하게 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-111">As such, this article outlines suggested minimum and potential example activities for each phase of the governance maturation process.</span></span> <span data-ttu-id="11639-112">이러한 활동의 초기 목표는 [정책 MVP](../journeys/overview.md#an-incremental-approach-to-cloud-governance)를 작성하고 증분 방식 정책 개선을 위한 프레임워크를 설정하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="11639-112">The initial objective of these activities is to help you build a [Policy MVP](../journeys/overview.md#an-incremental-approach-to-cloud-governance) and establish a framework for incremental policy evolution.</span></span> <span data-ttu-id="11639-113">클라우드 거버넌스 팀은 비용 관리 거버넌스 기능을 개선하기 위해 이러한 활동에 투자할 비용을 결정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-113">Your Cloud Governance team will need to decide how much to invest in these activities to improve your Cost Management governance capabilities.</span></span>

> [!CAUTION]
> <span data-ttu-id="11639-114">이 문서에서 설명하는 최소 활동과 수행할 가능성이 있는 활동은 특정 기업 정책 또는 타사 규정 준수 요구 사항을 기준으로 작성된 것이 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="11639-114">Neither the minimum or potential activities outlined in this article are aligned to specific corporate policies or third party compliance requirements.</span></span> <span data-ttu-id="11639-115">이 지침은 클라우드 거버넌스 모델을 사용하여 두 요구 사항을 모두 충족하기 위한 논의를 원활하게 진행하는 데 활용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="11639-115">This guidance is designed to help facilitate the conversations that will lead to alignment of both requirements with a cloud governance model.</span></span>

## <a name="planning-and-readiness"></a><span data-ttu-id="11639-116">계획 및 준비</span><span class="sxs-lookup"><span data-stu-id="11639-116">Planning and readiness</span></span>

<span data-ttu-id="11639-117">거버넌스 완성 과정의 이 단계에서는 서로 분리되어 있는 사업 결과와 실행 가능한 전략을 연결합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-117">This phase of governance maturity bridges the divide between business outcomes and actionable strategies.</span></span> <span data-ttu-id="11639-118">이 프로세스에서는 리더쉽 팀이 특정 메트릭을 정의하여 디지털 자산에 매핑한 다음 전반적인 마이그레이션 작업 계획을 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-118">During this process, the leadership team defines specific metrics, maps those metrics to the digital estate, and begins planning the overall migration effort.</span></span>

<span data-ttu-id="11639-119">**최소 권장 활동:**</span><span class="sxs-lookup"><span data-stu-id="11639-119">**Minimum suggested activities:**</span></span>

* <span data-ttu-id="11639-120">[비용 관리 도구 체인](toolchain.md) 옵션을 평가합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-120">Evaluate your [Cost Management toolchain](toolchain.md) options.</span></span>
* <span data-ttu-id="11639-121">초안 아키텍처 지침 문서를 개발하여 주요 관련자에게 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-121">Develop a draft Architecture Guidelines document and distribute to key stakeholders.</span></span>
* <span data-ttu-id="11639-122">아키텍처 지침 개발의 영향을 받는 직원과 팀을 개발 과정에 포함하고 교육을 진행합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-122">Educate and involve the people and teams affected by the development of Architecture Guidelines.</span></span>

<span data-ttu-id="11639-123">**수행할 가능성이 있는 활동:**</span><span class="sxs-lookup"><span data-stu-id="11639-123">**Potential activities:**</span></span>

* <span data-ttu-id="11639-124">클라우드 전략의 비즈니스 타당성을 지원하는 예산을 결정합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-124">Ensure budgetary decisions that support the business justification for your cloud strategy.</span></span>
* <span data-ttu-id="11639-125">적절한 자금 할당을 보고하는 데 사용하는 학습 메트릭의 유효성을 검사합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-125">Validate learning metrics that you use to report on the successful allocation of funding.</span></span>
* <span data-ttu-id="11639-126">클라우드 비용을 설명하는 방식에 영향을 주는 적절한 클라우드 회계 모델을 파악합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-126">Understand the desired cloud accounting model that affects how cloud costs should be accounted for.</span></span>
* <span data-ttu-id="11639-127">디지털 자산 계획을 숙지하고 예상 비용이 정확한지 유효성을 검사합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-127">Become familiar with the digital estate plan and validate accurate costing expectations.</span></span>
* <span data-ttu-id="11639-128">구매 옵션을 평가해 종량제와 기업계약 구매를 통한 사전 약정 중 더 효율적인 옵션을 결정합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-128">Evaluate buying options to determine if it's better to "pay as you go" or to make a precommitment by purchasing an Enterprise Agreement.</span></span>
* <span data-ttu-id="11639-129">계획된 예산에 맞게 비즈니스 목표를 결정하고 필요에 따라 예산 계획을 조정합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-129">Align business goals with planned budgets and adjust budgetary plans as necessary.</span></span>
* <span data-ttu-id="11639-130">각 비용 주기 종료 시에 기술/업무 관련자에게 알림을 제공하는 목표 및 예산 보고 메커니즘을 개발합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-130">Develop a goals and budget reporting mechanism to notify technical and business stakeholders at the end of each cost cycle.</span></span>

## <a name="build-and-pre-deployment"></a><span data-ttu-id="11639-131">빌드 및 배포 전 단계</span><span class="sxs-lookup"><span data-stu-id="11639-131">Build and pre-deployment</span></span>

<span data-ttu-id="11639-132">환경을 적절하게 마이그레이션하려면 여러 가지 기술적/비기술적 필수 조건을 충족해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-132">A number of technical and nontechnical prerequisites are required to successfully migrate an environment.</span></span> <span data-ttu-id="11639-133">이 프로세스에서는 마이그레이션 결정, 준비 및 마이그레이션을 진행하는 핵심 인프라 관련 사항을 중점적으로 결정합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-133">This process focuses on the decisions, readiness, and core infrastructure that proceeds a migration.</span></span>

<span data-ttu-id="11639-134">**최소 권장 활동:**</span><span class="sxs-lookup"><span data-stu-id="11639-134">**Minimum suggested activities:**</span></span>

* <span data-ttu-id="11639-135">[비용 관리 도구 체인](toolchain.md)을 배포 전 단계에서 출시하여 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-135">Implement your [Cost Management toolchain](toolchain.md) by rolling out in a pre-deployment phase.</span></span>
* <span data-ttu-id="11639-136">아키텍처 지침 문서를 업데이트하여 주요 관련자에게 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-136">Update the Architecture Guidelines document and distribute to key stakeholders.</span></span>
* <span data-ttu-id="11639-137">교육 자료와 설명서, 인식 정보, 인센티브 및 기타 프로그램을 개발하여 사용자 도입을 촉진할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="11639-137">Develop educational materials and documentation, awareness communications, incentives, and other programs to help drive user adoption.</span></span>
* <span data-ttu-id="11639-138">구매 요구 사항이 예산 및 목표에 적합한지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-138">Determine if your purchase requirements align with your budgets and goals.</span></span>

<span data-ttu-id="11639-139">**수행할 가능성이 있는 활동:**</span><span class="sxs-lookup"><span data-stu-id="11639-139">**Potential activities:**</span></span>

* <span data-ttu-id="11639-140">핵심 소유권 모델을 정의하는 [구독 전략](../../decision-guides/subscriptions/overview.md)에 맞는 예산 계획을 세웁니다.</span><span class="sxs-lookup"><span data-stu-id="11639-140">Align your budgetary plans with the [Subscription Strategy](../../decision-guides/subscriptions/overview.md) that defines your core ownership model.</span></span>
* <span data-ttu-id="11639-141">[리소스 일관성 전략](../../decision-guides/resource-consistency/overview.md)을 활용하여 장기적으로 아키텍처 및 비용 지침을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-141">Leverage the [Resource Consistency Strategy](../../decision-guides/resource-consistency/overview.md) to enforce architecture and cost guidelines over time.</span></span>
* <span data-ttu-id="11639-142">채택 및 마이그레이션 계획에 영향을 주는 변칙적인 비용이 발생하는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-142">Determine if there are any cost anomalies that affect your adoption and migration plans.</span></span>

## <a name="adopt-and-migrate"></a><span data-ttu-id="11639-143">채택 및 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="11639-143">Adopt and migrate</span></span>

<span data-ttu-id="11639-144">마이그레이션은 기존 디지털 자산의 애플리케이션이나 워크로드 이동, 테스트 및 채택을 중점적으로 수행하는 증분 방식 프로세스입니다.</span><span class="sxs-lookup"><span data-stu-id="11639-144">Migration is an incremental process that focuses on the movement, testing, and adoption of applications or workloads in an existing digital estate.</span></span>

<span data-ttu-id="11639-145">**최소 권장 활동:**</span><span class="sxs-lookup"><span data-stu-id="11639-145">**Minimum suggested activities:**</span></span>

* <span data-ttu-id="11639-146">[비용 관리 도구 체인](toolchain.md)을 배포 전 환경에서 프로덕션 환경으로 마이그레이션합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-146">Migrate your [Cost Management toolchain](toolchain.md) from pre-deployment to production.</span></span>
* <span data-ttu-id="11639-147">아키텍처 지침 문서를 업데이트하여 주요 관련자에게 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-147">Update the Architecture Guidelines document and distribute to key stakeholders.</span></span>
* <span data-ttu-id="11639-148">교육 자료와 설명서, 인식 정보, 인센티브 및 기타 프로그램을 개발하여 사용자 도입을 촉진할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="11639-148">Develop educational materials and documentation, awareness communications, incentives, and other programs to help drive user adoption.</span></span>

<span data-ttu-id="11639-149">**수행할 가능성이 있는 활동:**</span><span class="sxs-lookup"><span data-stu-id="11639-149">**Potential activities:**</span></span>

* <span data-ttu-id="11639-150">클라우드 회계 모델을 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-150">Implement your Cloud Accounting Model.</span></span>
* <span data-ttu-id="11639-151">각 릴리스 중의 실제 지출이 예산에 반영되었는지 확인하고 필요에 따라 예산을 조정합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-151">Ensure that your budgets reflect your actual spending during each release and adjust as necessary.</span></span>
* <span data-ttu-id="11639-152">예산 계획의 변화를 모니터링하고 관계자와 함께 추가 확인이 필요한지 여부의 유효성을 검사합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-152">Monitor changes in budgetary plans and validate with stakeholders if additional sign-offs are needed.</span></span>
* <span data-ttu-id="11639-153">실제 비용을 반영하도록 아키텍처 지침 문서 변경 내용을 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-153">Update changes to the Architecture Guidelines document to reflect actual costs.</span></span>

## <a name="operate-and-post-implementation"></a><span data-ttu-id="11639-154">작동 및 구현 후 작업</span><span class="sxs-lookup"><span data-stu-id="11639-154">Operate and post-implementation</span></span>

<span data-ttu-id="11639-155">변환이 완료되면 애플리케이션이나 워크로드의 원래 수명 주기 동안 거버넌스와 관련 작업이 실행되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-155">Once the transformation is complete, governance and operations must live on for the natural lifecycle of an application or workload.</span></span> <span data-ttu-id="11639-156">거버넌스 완성 과정의 이 단계에서는 일반적으로 솔루션이 구현되고 변환 주기가 안정화되기 시작한 후에 진행되는 활동을 중점적으로 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-156">This phase of governance maturity focuses on the activities that commonly come after the solution is implemented and the transformation cycle begins to stabilize.</span></span>

<span data-ttu-id="11639-157">**최소 권장 활동:**</span><span class="sxs-lookup"><span data-stu-id="11639-157">**Minimum suggested activities:**</span></span>

* <span data-ttu-id="11639-158">계속해서 바뀌는 조직의 비용 관리 요구 변경 내용에 따라 [비용 관리 도구 체인](toolchain.md)을 사용자 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-158">Customize your [Cost Management toolchain](toolchain.md) based on changes in your organization’s cost management needs.</span></span>
* <span data-ttu-id="11639-159">실제 지출을 반영하도록 알림과 보고서의 자동화를 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-159">Consider automating any notifications and reports to reflect actual spending.</span></span>
* <span data-ttu-id="11639-160">향후 채택 프로세스에서 참조할 수 있도록 아키텍처 지침을 구체적으로 작성합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-160">Refine Architecture Guidelines to guide future adoption processes.</span></span>
* <span data-ttu-id="11639-161">아키텍처 지침을 계속 준수할 수 있도록 영향을 받는 팀을 대상으로 정기 교육을 진행합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-161">Educate affected teams on a periodic basis to ensure ongoing adherence to the Architecture Guidelines.</span></span>

<span data-ttu-id="11639-162">**수행할 가능성이 있는 활동:**</span><span class="sxs-lookup"><span data-stu-id="11639-162">**Potential activities:**</span></span>

* <span data-ttu-id="11639-163">분기별 클라우드 비즈니스 검토를 실행하여 기업에 제공되는 가치와 관련 비용을 공개합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-163">Execute a quarterly cloud business review to communicate value delivered to the business and associated costs.</span></span>
* <span data-ttu-id="11639-164">실제 지출 변경 내용을 반영하도록 분기별로 계획을 조정합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-164">Adjust plans quarterly to reflect changes to actual spending.</span></span>
* <span data-ttu-id="11639-165">사업부 구독의 손익 수준에 맞게 예산을 결정합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-165">Determine financial alignment to P&Ls for business unit subscriptions.</span></span>
* <span data-ttu-id="11639-166">매월 관계자 가치 및 비용 보고 방법을 분석합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-166">Analyze stakeholder value and cost reporting methods on a monthly basis.</span></span>
* <span data-ttu-id="11639-167">사용률이 낮은 자산을 교정하고 계속 사용할 가치가 있는지를 결정합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-167">Remediate underused assets and determine if they're worth continuing.</span></span>
* <span data-ttu-id="11639-168">계획과 실제 지출 간의 불일치 사항과 변칙적인 지출을 검색합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-168">Detect misalignments and anomalies between the plan and actual spending.</span></span>
* <span data-ttu-id="11639-169">클라우드 채택 팀과 클라우드 전략 팀이 그러한 변칙적인 지출을 파악하여 해결할 수 있도록 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="11639-169">Assist the cloud adoption teams and the Cloud Strategy team with understanding and resolving these anomalies.</span></span>

## <a name="next-steps"></a><span data-ttu-id="11639-170">다음 단계</span><span class="sxs-lookup"><span data-stu-id="11639-170">Next steps</span></span>

<span data-ttu-id="11639-171">이제 클라우드 ID 거버넌스의 개념을 이해했으므로 [비용 관리 도구 체인](toolchain.md)에서 Azure 플랫폼에서 비용 관리 거버넌스 분야를 개발할 때 필요한 Azure 도구 및 기능을 살펴보세요.</span><span class="sxs-lookup"><span data-stu-id="11639-171">Now that you understand the concept of cloud identity governance, examine the [Cost Management toolchain](toolchain.md) to identify Azure tools and features that you'll need when developing the Cost Management governance discipline on the Azure platform.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="11639-172">Azure용 비용 관리 도구 체인</span><span class="sxs-lookup"><span data-stu-id="11639-172">Cost Management toolchain for Azure</span></span>](toolchain.md)