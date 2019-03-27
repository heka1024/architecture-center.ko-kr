---
title: 'CAF: 리소스 일관성 분야 개선'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 리소스 일관성 분야 개선
author: BrianBlanchard
ms.openlocfilehash: bc81b894d46266c52291c53dba5532ab2ab6b860
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58241574"
---
# <a name="resource-consistency-discipline-improvement"></a><span data-ttu-id="e81c6-103">리소스 일관성 분야 개선</span><span class="sxs-lookup"><span data-stu-id="e81c6-103">Resource Consistency discipline improvement</span></span>

<span data-ttu-id="e81c6-104">리소스 일관성 분야에서는 환경, 애플리케이션 또는 워크로드의 작업 관리와 관련된 정책을 설정하는 방법에 중점을 둡니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-104">The Resource Consistency discipline focuses on ways of establishing policies related to the operational management of an environment, application, or workload.</span></span> <span data-ttu-id="e81c6-105">5개 클라우드 거버넌스 분야 내에서 리소스 일관성에는 애플리케이션, 워크로드 및 자산 성능에 대한 모니터링이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-105">Within the five disciplines of Cloud Governance, Resource Consistency includes monitoring of applications, workload, and asset performance.</span></span> <span data-ttu-id="e81c6-106">또한 리소스 일관성에는 확장 요구 사항을 충족하고, 성능 SLA(서비스 수준 약정) 위반을 수정하고, 자동 수정을 통해 SLA 위반을 사전에 방지하는 데 필요한 작업도 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-106">It also includes the tasks required to meet scale demands, remediate performance Service Level Agreement (SLA) violations, and proactively avoid SLA violations through automated remediation.</span></span>

<span data-ttu-id="e81c6-107">이 문서에서는 리소스 일관성 분야를 더욱 효율적으로 개발하고 완성도를 높이기 위해 회사가 수행할 수 있는 몇 가지 작업을 간략하게 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-107">This article outlines some potential tasks your company can engage in to better develop and mature the Resource Consistency discipline.</span></span> <span data-ttu-id="e81c6-108">이러한 작업은 클라우드 솔루션을 구현하는 과정의 계획, 빌드, 도입, 운영 단계로 구분한 다음, [클라우드 거버넌스에 대한 증분 방식](../journeys/overview.md#an-incremental-approach-to-cloud-governance)의 개발을 허용하는 과정에서 이러한 단계를 반복적으로 수행하게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-108">These tasks can be broken down into planning, building, adopting, and operating phases of implementing a cloud solution, which are then iterated on allowing the development of an [incremental approach to cloud governance](../journeys/overview.md#an-incremental-approach-to-cloud-governance).</span></span>

![도입의 4단계](../../_images/adoption-phases.png)

<span data-ttu-id="e81c6-110">*그림 1. 클라우드 거버넌스의 증분 방식 도입 단계.*</span><span class="sxs-lookup"><span data-stu-id="e81c6-110">*Figure 1. Adoption phases of the incremental approach to cloud governance.*</span></span>

<span data-ttu-id="e81c6-111">한 문서에서 모든 비즈니스의 요구 사항을 설명할 수는 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-111">It's impossible for any one document to account for the requirements of all businesses.</span></span> <span data-ttu-id="e81c6-112">따라서 이 문서에서는 거버넌스 완성 프로세스의 각 단계에서 권장되는 최소 활동과 수행할 가능성이 있는 활동의 예를 간략하게 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-112">As such, this article outlines suggested minimum and potential example activities for each phase of the governance maturation process.</span></span> <span data-ttu-id="e81c6-113">이러한 활동의 초기 목표는 [정책 MVP](../journeys/overview.md#an-incremental-approach-to-cloud-governance)를 빌드하고 증분 방식 정책 개선을 위한 프레임워크를 설정하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-113">The initial objective of these activities is to help you build a [Policy MVP](../journeys/overview.md#an-incremental-approach-to-cloud-governance) and establish a framework for incremental policy evolution.</span></span> <span data-ttu-id="e81c6-114">클라우드 거버넌스 팀은 리소스 일관성 거버넌스 기능을 개선하려면 이러한 활동에 투자할 비용을 결정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-114">Your Cloud Governance team will need to decide how much to invest in these activities to improve your Resource Consistency governance capabilities.</span></span>

> [!CAUTION]
> <span data-ttu-id="e81c6-115">이 문서에서 설명하는 최소 활동과 수행할 가능성이 있는 활동은 특정 기업 정책 또는 타사 규정 준수 요구 사항을 기준으로 작성된 것이 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-115">Neither the minimum or potential activities outlined in this article are aligned to specific corporate policies or third party compliance requirements.</span></span> <span data-ttu-id="e81c6-116">이 지침은 클라우드 거버넌스 모델을 사용하여 두 요구 사항을 모두 충족하기 위한 논의를 원활하게 진행할 수 있도록 설계되었습니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-116">This guidance is designed to help facilitate the conversations that will lead to alignment of both requirements with a cloud governance model.</span></span>

## <a name="planning-and-readiness"></a><span data-ttu-id="e81c6-117">계획 및 준비</span><span class="sxs-lookup"><span data-stu-id="e81c6-117">Planning and readiness</span></span>

<span data-ttu-id="e81c6-118">이 거버넌스 완성 과정 단계에서는 서로 분리되어 있는 사업 결과와 실행 가능한 전략을 연결합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-118">This phase of governance maturity bridges the divide between business outcomes and actionable strategies.</span></span> <span data-ttu-id="e81c6-119">이 프로세스에서는 리더십 팀이 특정 메트릭을 정의하여 디지털 자산에 매핑한 다음, 전반적인 마이그레이션 작업 계획을 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-119">During this process, the leadership team defines specific metrics, maps those metrics to the digital estate, and begins planning the overall migration effort.</span></span>

<span data-ttu-id="e81c6-120">**최소 권장 활동:**</span><span class="sxs-lookup"><span data-stu-id="e81c6-120">**Minimum suggested activities:**</span></span>

* <span data-ttu-id="e81c6-121">[리소스 일관성 도구 체인](toolchain.md) 옵션을 평가합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-121">Evaluate your [Resource Consistency toolchain](toolchain.md) options.</span></span>
* <span data-ttu-id="e81c6-122">클라우드 전략에 대한 라이선스 요구 사항을 이해합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-122">Understand the licensing requirements for your cloud strategy.</span></span>
* <span data-ttu-id="e81c6-123">초안 아키텍처 지침 문서를 개발하여 주요 관련자에게 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-123">Develop a draft Architecture Guidelines document and distribute to key stakeholders.</span></span>
* <span data-ttu-id="e81c6-124">사용하는 Resource Manager에 익숙해져서 솔루션에 대한 모든 리소스를 그룹으로 배포, 관리 및 모니터링합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-124">Become familiar with the resource manager you use to deploy, manage, and monitor all the resources for your solution as a group.</span></span>
* <span data-ttu-id="e81c6-125">아키텍처 지침 개발에서 영향을 받은 직원과 팀을 교육하고 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-125">Educate and involve the people and teams affected by the development of architecture guidelines.</span></span>
* <span data-ttu-id="e81c6-126">우선 순위가 지정된 리소스 배포 작업을 마이그레이션 백로그에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-126">Add prioritized resource deployment tasks to your migration backlog.</span></span>

<span data-ttu-id="e81c6-127">**수행할 가능성이 있는 활동:**</span><span class="sxs-lookup"><span data-stu-id="e81c6-127">**Potential activities:**</span></span>

* <span data-ttu-id="e81c6-128">비즈니스 이해 관계자 및/또는 클라우드 전략 팀을 사용하여 전체 사업부 및 조직 내에서 원하는 클라우드 계정 관리 방식 및 비용 계정 사례를 이해합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-128">Work with the business stakeholders and/or your cloud strategy team to understand the desired cloud accounting approach and cost accounting practices within your business units and organization as a whole.</span></span>
* <span data-ttu-id="e81c6-129">[모니터링 및 정책 적용](compliance-processes.md) 요구 사항을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-129">Define your [monitoring and policy enforcement](compliance-processes.md) requirements.</span></span>
* <span data-ttu-id="e81c6-130">수정 정책 및 SLA 요구 사항을 정의하려면 가동 중단 비용과 비즈니스 가치를 검사합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-130">Examine the business value and cost of outage to define remediation policy and SLA requirements.</span></span>
* <span data-ttu-id="e81c6-131">리소스에 대한 [간단한 워크로드](./governance-simple-workload.md) 또는 [여러 팀](./governance-multiple-teams.md)의 거버넌스 전략을 배포할지 여부를 결정합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-131">Determine whether you'll deploy a [simple workload](./governance-simple-workload.md) or [multi-team](./governance-multiple-teams.md) governance strategy for your resources.</span></span>
* <span data-ttu-id="e81c6-132">계획된 워크로드의 확장성 요구 사항을 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-132">Determine scalability requirements for your planned workloads.</span></span>

## <a name="build-and-pre-deployment"></a><span data-ttu-id="e81c6-133">빌드 및 배포 전 단계</span><span class="sxs-lookup"><span data-stu-id="e81c6-133">Build and pre-deployment</span></span>

<span data-ttu-id="e81c6-134">환경을 성공적으로 마이그레이션하려면 여러 가지 기술 및 비기술적 필수 구성 요소가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-134">A number of technical and nontechnical prerequisites are required to successful migrate an environment.</span></span> <span data-ttu-id="e81c6-135">이 프로세스는 의사 결정, 준비 및 마이그레이션을 진행하는 핵심 인프라에 중점을 둡니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-135">This process focuses on the decisions, readiness, and core infrastructure that proceeds a migration.</span></span>

<span data-ttu-id="e81c6-136">**최소 권장 활동:**</span><span class="sxs-lookup"><span data-stu-id="e81c6-136">**Minimum suggested activities:**</span></span>

* <span data-ttu-id="e81c6-137">[리소스 일관성 도구 체인](toolchain.md)을 배포 전 단계에서 롤아웃하여 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-137">Implement your [Resource Consistency toolchain](toolchain.md) by rolling out in a pre-deployment phase.</span></span>
* <span data-ttu-id="e81c6-138">아키텍처 지침 문서를 업데이트하여 주요 관계자에게 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-138">Update the Architecture Guidelines document and distribute to key stakeholders.</span></span>
* <span data-ttu-id="e81c6-139">우선 순위가 지정된 마이그레이션 백로그에서 리소스 배포 작업을 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-139">Implement resource deployment tasks on your prioritized migration backlog.</span></span>
* <span data-ttu-id="e81c6-140">교육 자료와 설명서, 인식 정보, 인센티브 및 기타 프로그램을 개발하여 사용자 도입을 촉진할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-140">Develop educational materials and documentation, awareness communications, incentives, and other programs to help drive user adoption.</span></span>

<span data-ttu-id="e81c6-141">**수행할 가능성이 있는 활동:**</span><span class="sxs-lookup"><span data-stu-id="e81c6-141">**Potential activities:**</span></span>

* <span data-ttu-id="e81c6-142">[구독 디자인 전략](../../decision-guides/subscriptions/overview.md)을 결정하고, 조직 및 워크로드 요구에 가장 잘 맞는 구독 패턴을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-142">Decide on a [subscription design strategy](../../decision-guides/subscriptions/overview.md), choosing the subscription patterns that best fit your organization and workload needs.</span></span>
* <span data-ttu-id="e81c6-143">[리소스 일관성 전략](../../decision-guides/resource-consistency/overview.md)을 사용하여 시간에 따라 아키텍처 지침을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-143">Use a [resource consistency](../../decision-guides/resource-consistency/overview.md) strategy to enforce architecture guidelines over time.</span></span>
* <span data-ttu-id="e81c6-144">리소스가 조직 및 계정 요구 사항에 맞도록 [리소스 이름 지정 및 태그 지정 표준](../../decision-guides/resource-tagging/overview.md)을 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-144">Implement [resource naming, and tagging standards](../../decision-guides/resource-tagging/overview.md) for your resources to match your organizational and accounting requirements.</span></span>
* <span data-ttu-id="e81c6-145">자동 관리 지정 시간 거버넌스를 만들려면 배포 템플릿 및 자동화를 사용하여 리소스 및 리소스 그룹을 배포하는 경우 일반적인 구성 및 일관된 그룹화 구조를 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-145">To create proactive point-in-time governance, use deployment templates and automation to enforce common configurations and a consistent grouping structure when deploying resources and resource groups.</span></span>
* <span data-ttu-id="e81c6-146">기본적으로 사용자가 사용 권한이 없는 최소 권한의 사용 권한 모델을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-146">Establish a least privilege permissions model, where users have no permissions by default.</span></span>
* <span data-ttu-id="e81c6-147">각 워크로드 및 계정을 소유하는 조직의 구성원 및 이러한 리소스를 유지 관리하거나 수정하려면 액세스해야 하는 구성원을 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-147">Determine who in your organization owns each workload and account, and who will need to access to maintain or modify these resources.</span></span> <span data-ttu-id="e81c6-148">이러한 요구 사항에 맞는 클라우드 역할 및 책임을 정의하고, 액세스 제어에 대한 기준으로 이러한 역할을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-148">Define cloud roles and responsibilities that match these needs and use these roles as the basis for access control.</span></span>
* <span data-ttu-id="e81c6-149">리소스 간 종속성을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-149">Define dependencies between resources.</span></span>
* <span data-ttu-id="e81c6-150">자동화된 리소스 크기 조정을 구현하여 계획 단계에서 정의된 요구 사항에 맞춥니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-150">Implement automated resource scaling to match requirements defined in the Plan stage.</span></span>
* <span data-ttu-id="e81c6-151">수신된 서비스의 품질을 측정하려면 액세스 성능을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-151">Conduct access performance to measure the quality of services received.</span></span>
* <span data-ttu-id="e81c6-152">구성 설정 및 리소스 생성 규칙을 사용하여 SLA 적용을 관리하려면 [정책](/azure/governance/policy/overview)을 배포하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-152">Consider deploying [policy](/azure/governance/policy/overview) to manage SLA enforcement using configuration settings and resource creation rules.</span></span>

## <a name="adopt-and-migrate"></a><span data-ttu-id="e81c6-153">도입 및 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="e81c6-153">Adopt and migrate</span></span>

<span data-ttu-id="e81c6-154">마이그레이션은 기존 디지털 자산의 애플리케이션이나 워크로드 이동, 테스트 및 도입에 중점을 두는 증분 방식 프로세스입니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-154">Migration is an incremental process that focuses on the movement, testing, and adoption of applications or workloads in an existing digital estate.</span></span>

<span data-ttu-id="e81c6-155">**최소 권장 활동:**</span><span class="sxs-lookup"><span data-stu-id="e81c6-155">**Minimum suggested activities:**</span></span>

* <span data-ttu-id="e81c6-156">[리소스 일관성 도구 체인](toolchain.md)을 배포 전 환경에서 프로덕션 환경으로 마이그레이션합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-156">Migrate your [Resource Consistency toolchain](toolchain.md) from pre-deployment to production.</span></span>
* <span data-ttu-id="e81c6-157">아키텍처 지침 문서를 업데이트하여 주요 관계자에게 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-157">Update the Architecture Guidelines document and distribute to key stakeholders.</span></span>
* <span data-ttu-id="e81c6-158">교육 자료와 설명서, 인식 정보, 인센티브 및 기타 프로그램을 개발하여 사용자 도입을 촉진할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-158">Develop educational materials and documentation, awareness communications, incentives, and other programs to help drive user adoption.</span></span>
* <span data-ttu-id="e81c6-159">기존 자동화된 재구성 스크립트나 도구를 마이그레이션하여 정의된 SLA 요구 사항을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-159">Migrate any existing automated remediation scripts or tools to support defined SLA requirements.</span></span>

<span data-ttu-id="e81c6-160">**수행할 가능성이 있는 활동:**</span><span class="sxs-lookup"><span data-stu-id="e81c6-160">**Potential activities:**</span></span>

* <span data-ttu-id="e81c6-161">모니터링 및 보고 데이터를 완료하고 테스트합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-161">Complete and test monitoring and reporting data.</span></span> <span data-ttu-id="e81c6-162">선택한 온-프레미스, 클라우드 게이트웨이 또는 하이브리드 솔루션을 사용하여.</span><span class="sxs-lookup"><span data-stu-id="e81c6-162">with your chosen on-premises, cloud gateway, or hybrid solution.</span></span>
* <span data-ttu-id="e81c6-163">리소스에 대한 SLA 또는 관리 정책을 변경해야 하는지를 결정합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-163">Determine if changes need to be made to SLA or management policy for resources.</span></span>
* <span data-ttu-id="e81c6-164">클라우드 자산에서 리소스를 효율적으로 검색하려면 쿼리 기능을 구현하여 작업을 개선합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-164">Improve operations tasks by implementing query capabilities to efficiently find resource across your cloud estate.</span></span>
* <span data-ttu-id="e81c6-165">리소스를 비즈니스 요구 사항 및 거버넌스 요구 사항 변경에 맞춥니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-165">Align resources to changing business needs and governance requirements.</span></span>
* <span data-ttu-id="e81c6-166">가상 머신, 가상 네트워크 및 스토리지 계정이 각 릴리스 기간 동안 실제 리소스 액세스 요구 사항을 반영하고 필요에 따라 조정되는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-166">Ensure that your virtual machines, virtual networks, and storage accounts reflect actual resource access needs during each release, and adjust as necessary.</span></span>
* <span data-ttu-id="e81c6-167">자동화된 리소스의 크기 조정이 액세스 요구 사항을 충족하는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-167">Verify automated scaling of resources meets access requirements.</span></span>
* <span data-ttu-id="e81c6-168">리소스, 리소스 그룹 및 Azure 구독에 대한 사용자 액세스를 검토하고, 필요에 따라 액세스 제어를 조정합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-168">Review user access to resources, resource groups, and Azure subscriptions, and adjust access controls as necessary.</span></span>
* <span data-ttu-id="e81c6-169">리소스 액세스 계획의 변화를 모니터링하고, 관계자와 함께 추가 로그오프가 필요한지 여부의 유효성을 검사합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-169">Monitor changes in resource access plans and validate with stakeholders if additional sign-offs are needed.</span></span>
* <span data-ttu-id="e81c6-170">실제 비용을 반영하도록 아키텍처 지침 문서의 변경 내용을 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-170">Update changes to the Architecture Guidelines document to reflect actual costs.</span></span>
* <span data-ttu-id="e81c6-171">사업부의 P&L에 따라 명확한 재무 조정이 조직에 필요한지 여부를 결정합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-171">Determine whether your organization requires clearer financial alignment to P&Ls for business units.</span></span>
* <span data-ttu-id="e81c6-172">글로벌 조직에서는 SLA 규정 준수 또는 주권 요구 사항을 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-172">For global organizations, implement your SLA compliance or sovereignty requirements.</span></span>
* <span data-ttu-id="e81c6-173">클라우드 집계의 경우 클라우드 공급 기업에 게이트웨이 솔루션을 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-173">For cloud aggregation, deploy a gateway solution to a cloud provider.</span></span>
* <span data-ttu-id="e81c6-174">하이브리드 또는 게이트웨이 옵션을 허용하지 않는 도구에서는 모니터링과 작동 모니터링 도구를 긴밀하게 결합합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-174">For tools that don't allow for hybrid or gateway options, tightly couple monitoring with an operational monitoring tool.</span></span>

## <a name="operate-and-post-implementation"></a><span data-ttu-id="e81c6-175">작동 및 구현 후 작업</span><span class="sxs-lookup"><span data-stu-id="e81c6-175">Operate and post-implementation</span></span>

<span data-ttu-id="e81c6-176">변환이 완료되면 애플리케이션이나 워크로드의 원래 수명 주기 동안 거버넌스와 관련 작업이 실행되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-176">Once the transformation is complete, governance and operations must live on for the natural lifecycle of an application or workload.</span></span> <span data-ttu-id="e81c6-177">이 거버넌스 완성 과정 단계에서는 일반적으로 솔루션이 구현되고 변환 주기가 안정화되기 시작한 후에 진행되는 활동을 중점적으로 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-177">This phase of governance maturity focuses on the activities that commonly come after the solution is implemented and the transformation cycle begins to stabilize.</span></span>

<span data-ttu-id="e81c6-178">**최소 권장 활동:**</span><span class="sxs-lookup"><span data-stu-id="e81c6-178">**Minimum suggested activities:**</span></span>

* <span data-ttu-id="e81c6-179">조직의 변화하는 Cost Management 요구 사항에 대한 업데이트에 따라 [리소스 일관성 도구 체인](toolchain.md)을 사용자 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-179">Customize your [Resource Consistency toolchain](toolchain.md) based on updates to your organization’s changing Cost Management needs.</span></span>
* <span data-ttu-id="e81c6-180">실제 리소스 사용량을 반영하도록 알림과 보고서를 자동화하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-180">Consider automating any notifications and reports to reflect actual resource usage.</span></span>
* <span data-ttu-id="e81c6-181">향후 도입 프로세스에서 참조할 수 있도록 아키텍처 지침을 구체적으로 작성합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-181">Refine Architecture Guidelines to guide future adoption processes.</span></span>
* <span data-ttu-id="e81c6-182">아키텍처 지침을 계속 준수할 수 있도록 영향을 받는 팀을 대상으로 정기 교육을 진행합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-182">Educate affected teams periodically to ensure ongoing adherence to the architecture guidelines.</span></span>

<span data-ttu-id="e81c6-183">**수행할 가능성이 있는 활동:**</span><span class="sxs-lookup"><span data-stu-id="e81c6-183">**Potential activities:**</span></span>

* <span data-ttu-id="e81c6-184">실제 리소스 변경 내용을 반영하도록 분기별로 계획을 조정합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-184">Adjust plans quarterly to reflect changes to actual resources.</span></span>
* <span data-ttu-id="e81c6-185">향후 배포 시 거버넌스 요구 사항을 자동으로 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-185">Automatically apply and enforce governance requirements during future deployments.</span></span>
* <span data-ttu-id="e81c6-186">사용률이 낮은 리소스를 평가하여 계속 사용할 가치가 있는지를 결정합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-186">Evaluate underused resources and determine if they're worth continuing.</span></span>
* <span data-ttu-id="e81c6-187">계획적 리소스 사용량 및 실제 리소스 사용량 간의 불일치 및 잘못된 부분을 검색합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-187">Detect misalignments and anomalies between planned and actual resource usage.</span></span>
* <span data-ttu-id="e81c6-188">클라우드 도입 팀과 클라우드 전략 팀이 이러한 잘못된 부분을 파악하여 해결할 수 있도록 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-188">Assist the cloud adoption teams and the Cloud Strategy team in understanding and resolving these anomalies.</span></span>
* <span data-ttu-id="e81c6-189">청구 및 SLA에 대한 리소스 일관성을 변경해야 하는지를 결정합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-189">Determine if changes need to be made to Resource Consistency for billing and SLAs.</span></span>
* <span data-ttu-id="e81c6-190">온-프레미스, 클라우드 게이트웨이 또는 하이브리드 솔루션에 조정이 필요한지 여부를 결정하는 로깅 및 모니터링 도구를 평가합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-190">Evaluate logging and monitoring tools to determine whether your on-premises, cloud gateway, or hybrid solution needs adjusting.</span></span>
* <span data-ttu-id="e81c6-191">사업부 및 지리적으로 분산된 그룹에서는 중앙 집중식 정책을 더 잘 적용하고 SLA 요구 사항을 충족하려면 조직이 추가 클라우드 관리 기능(예: [Azure 관리 그룹](/azure/governance/management-groups/))을 사용하는 것이 좋은지를 결정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e81c6-191">For business units and geographically distributed groups, determine if your organization should consider using additional cloud management features (for example [Azure management groups](/azure/governance/management-groups/)) to better apply centralized policy and meet SLA requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e81c6-192">다음 단계</span><span class="sxs-lookup"><span data-stu-id="e81c6-192">Next steps</span></span>

<span data-ttu-id="e81c6-193">이제 클라우드 리소스 거버넌스의 개념을 이해했으므로 [단일 워크로드](governance-simple-workload.md) 또는 [여러 팀](governance-multiple-teams.md)에 대해 거버넌스 모델을 디자인하는 방법을 알아보기 위한 준비 과정으로 Azure에서 [리소스 액세스를 관리하는 방법](azure-resource-access.md)에 대해 자세히 알아보세요.</span><span class="sxs-lookup"><span data-stu-id="e81c6-193">Now that you understand the concept of cloud resource governance, move on to learn more about [how resource access is managed](azure-resource-access.md) in Azure in preparation for learning how to design a governance model for a [simple workload](governance-simple-workload.md) or for [multiple teams](governance-multiple-teams.md).</span></span>

> [!div class="nextstepaction"]
> <span data-ttu-id="e81c6-194">[Azure의 리소스 액세스에 대해 알아보기](azure-resource-access.md)
> [Azure의 SLA에 대해 알아보기](https://azure.microsoft.com/support/legal/sla/)
> [로깅, 보고 및 모니터링에 대해 알아보기](../../decision-guides/log-and-report/overview.md)</span><span class="sxs-lookup"><span data-stu-id="e81c6-194">[Learn about resource access in Azure](azure-resource-access.md)
[Learn about SLAs for Azure](https://azure.microsoft.com/support/legal/sla/)
[Learn about logging, reporting, and monitoring](../../decision-guides/log-and-report/overview.md)</span></span>
