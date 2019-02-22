---
title: 'CAF: 보안 기준 분야 향상'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 보안 기준 분야 향상
author: BrianBlanchard
ms.openlocfilehash: 28a971f56c9f8ada1d184bdc1cb3dbb9a17c3507
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901903"
---
# <a name="security-baseline-discipline-improvement"></a><span data-ttu-id="882b7-103">보안 기준 분야 향상</span><span class="sxs-lookup"><span data-stu-id="882b7-103">Security Baseline discipline improvement</span></span>

<span data-ttu-id="882b7-104">보안 기준 분야는 네트워크, 자산 및 클라우드 공급자 솔루션에 상주할 데이터(가장 중요)를 보호하는 정책을 수립하는 방법에 중점을 둡니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-104">The Security Baseline discipline focuses on ways of establishing policies that protect the network, assets, and most importantly the data that will reside on a Cloud Provider's solution.</span></span> <span data-ttu-id="882b7-105">5개 클라우드 거버넌스 분야의 보안 기준에는 디지털 자산 및 데이터에 대한 분류가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-105">Within the five disciplines of Cloud Governance, Security Baseline includes classification of the digital estate and data.</span></span> <span data-ttu-id="882b7-106">또한 데이터, 자산 및 네트워크의 보안과 관련된 위험, 비즈니스 허용 범위 및 완화 전략에 대한 설명서도 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-106">It also includes documentation of risks, business tolerance, and mitigation strategies associated with the security of the data, assets, and network.</span></span> <span data-ttu-id="882b7-107">기술적 관점에서 볼 때 [암호화](../../decision-guides/encryption/overview.md), [네트워크 요구 사항](../../decision-guides/software-defined-network/overview.md), [하이브리드 ID 전략](../../decision-guides/identity/overview.md) 및 클라우드 보안 기준 정책을 개발하는 데 사용된 [프로세스](compliance-processes.md)와 관련된 의사 결정에의 참여도 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-107">From a technical perspective, this also includes involvement in decisions regarding [encryption](../../decision-guides/encryption/overview.md), [network requirements](../../decision-guides/software-defined-network/overview.md), [hybrid identity strategies](../../decision-guides/identity/overview.md), and the [processes](compliance-processes.md) used to develop cloud Security Baseline policies.</span></span>

<span data-ttu-id="882b7-108">이 문서에서는 보안 기준 분야를 더 효율적으로 개발하고 완성도를 높이기 위해 회사에서 참여할 수 있는 몇 가지 잠재적 작업에 대해 간략하게 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-108">This article outlines some potential tasks your company can engage in to better develop and mature the Security Baseline discipline.</span></span> <span data-ttu-id="882b7-109">이러한 작업은 클라우드 솔루션 구현의 계획, 구축, 채택 및 운영 단계로 나눌 수 있으며, [클라우드 거버넌스에 대한 점진적 접근 방식](../journeys/overview.md#an-incremental-approach-to-cloud-governance)을 개발하는 과정에서 반복됩니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-109">These tasks can be broken down into planning, building, adopting, and operating phases of implementing a cloud solution, which are then iterated on allowing the development of an [incremental approach to cloud governance](../journeys/overview.md#an-incremental-approach-to-cloud-governance).</span></span>

![채택의 4가지 단계](../../_images/adoption-phases.png)

<span data-ttu-id="882b7-111">*그림 1. 클라우드 거버넌스에 대한 점진적 접근 방식의 채택 단계*</span><span class="sxs-lookup"><span data-stu-id="882b7-111">*Figure 1. Adoption phases of the incremental approach to cloud governance.*</span></span>

<span data-ttu-id="882b7-112">한 문서에서 모든 비즈니스의 요구 사항을 설명할 수는 없습니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-112">It's impossible for any one document to account for the requirements of all businesses.</span></span> <span data-ttu-id="882b7-113">따라서 이 문서에서는 거버넌스 완성 프로세스의 각 단계에 제안되는 최소 활동과 잠재적인 활동 예에 대해 간략하게 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-113">As such, this article outlines suggested minimum and potential example activities for each phase of the governance maturation process.</span></span> <span data-ttu-id="882b7-114">이러한 활동의 초기 목표는 [정책 MVP](../journeys/overview.md#an-incremental-approach-to-cloud-governance)를 구축하고 점진적 정책 전개를 위한 프레임워크를 설정하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-114">The initial objective of these activities is to help you build a [Policy MVP](../journeys/overview.md#an-incremental-approach-to-cloud-governance) and establish a framework for incremental policy evolution.</span></span> <span data-ttu-id="882b7-115">클라우드 거버넌스 팀에서 보안 기준 거버넌스 기능을 향상시키기 위해 이러한 활동에 투자할 비용을 결정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-115">Your Cloud Governance team will need to decide how much to invest in these activities to improve your Security Baseline governance capabilities.</span></span>

> [!CAUTION]
> <span data-ttu-id="882b7-116">이 문서에서 설명하는 최소 활동과 잠재적 활동은 모두 특정 회사 정책 또는 타사 규정 준수 요구 사항에 부합하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-116">Neither the minimum or potential activities outlined in this article are aligned to specific corporate policies or third party compliance requirements.</span></span> <span data-ttu-id="882b7-117">이 지침은 클라우드 거버넌스 모델을 두 가지 요구 사항 모두에 맞게 조정하기 위한 대화를 쉽게 진행할 수 있도록 설계되었습니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-117">This guidance is designed to help facilitate the conversations that will lead to alignment of both requirements with a cloud governance model.</span></span>

## <a name="planning-and-readiness"></a><span data-ttu-id="882b7-118">계획 및 준비</span><span class="sxs-lookup"><span data-stu-id="882b7-118">Planning and readiness</span></span>

<span data-ttu-id="882b7-119">이 거버넌스 성숙 단계는 비즈니스 결과와 실행 가능한 전략 간의 간극을 좁혀줍니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-119">This phase of governance maturity bridges the divide between business outcomes and actionable strategies.</span></span> <span data-ttu-id="882b7-120">이 프로세스에서는 리더쉽 팀에서 특정 메트릭을 정의하고, 이러한 메트릭을 디지털 자산에 매핑하며, 전체 마이그레이션 작업의 계획 수립을 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-120">During this process, the leadership team defines specific metrics, maps those metrics to the digital estate, and begins planning the overall migration effort.</span></span>

<span data-ttu-id="882b7-121">**최소 제안 작업:**</span><span class="sxs-lookup"><span data-stu-id="882b7-121">**Minimum suggested activities:**</span></span>

- <span data-ttu-id="882b7-122">[보안 기준 도구 체인](toolchain.md) 옵션을 평가합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-122">Evaluate your [Security Baseline toolchain](toolchain.md) options.</span></span>
- <span data-ttu-id="882b7-123">아키텍처 지침 문서 초안을 개발하여 주요 이해 관계자에게 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-123">Develop a draft Architecture Guidelines document and distribute to key stakeholders.</span></span>
- <span data-ttu-id="882b7-124">아키텍처 지침 개발의 영향을 받는 사용자와 팀을 교육하고 참여시킵니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-124">Educate and involve the people and teams affected by the development of architecture guidelines.</span></span>
- <span data-ttu-id="882b7-125">우선 순위가 지정된 보안 작업을 마이그레이션 백로그에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-125">Add prioritized security tasks to your migration backlog.</span></span>

<span data-ttu-id="882b7-126">**잠재적 활동:**</span><span class="sxs-lookup"><span data-stu-id="882b7-126">**Potential activities:**</span></span>

- <span data-ttu-id="882b7-127">데이터 분류 스키마를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-127">Define a data classification schema.</span></span>
- <span data-ttu-id="882b7-128">디지털 자산 계획 프로세스를 수행하여 비즈니스 프로세스와 지원 작업을 강화하는 현재 IT 자산의 인벤토리를 작성합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-128">Conduct a digital estate planning process to inventory the current IT assets powering your business processes and supporting operations.</span></span>
- <span data-ttu-id="882b7-129">[정책 검토](../../governance/policy-compliance/what-is-a-cloud-policy-review.md)를 수행하여 기존의 회사 IT 보안 정책을 현대화하는 프로세스를 시작하고, 알려진 위험을 처리하는 MVP 정책을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-129">Conduct a [policy review](../../governance/policy-compliance/what-is-a-cloud-policy-review.md) to begin the process of modernizing existing corporate IT security policies, and define MVP policies addressing known risks.</span></span>
- <span data-ttu-id="882b7-130">클라우드 플랫폼의 보안 지침을 검토합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-130">Review your cloud platform's security guidelines.</span></span> <span data-ttu-id="882b7-131">Azure의 경우 이러한 지침은 [Microsoft Service Trust 플랫폼](https://www.microsoft.com/trustcenter/stp/default.aspx)에서 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-131">For Azure these can be found in the [Microsoft Service Trust Platform](https://www.microsoft.com/trustcenter/stp/default.aspx).</span></span>
- <span data-ttu-id="882b7-132">보안 기준 정책에 [보안 개발 수명 주기](https://www.microsoft.com/securityengineering/sdl/)가 포함되어 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-132">Determine whether your Security Baseline policy includes a [Security Development Lifecycle](https://www.microsoft.com/securityengineering/sdl/).</span></span>
- <span data-ttu-id="882b7-133">다음 1~3개의 릴리스에 따라 네트워크, 데이터 및 자산 관련 비즈니스 위험을 평가하고, 이러한 위험에 대한 조직의 허용 범위를 측정합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-133">Evaluate network, data, and asset-related business risks based on the next one to three releases, and gauge your organization's tolerance for those risks.</span></span>
- <span data-ttu-id="882b7-134">Microsoft의 [top trends in cybersecurity](https://www.microsoft.com/security/operations/security-intelligence-report)(주요 사이버 보안 추세) 보고서를 검토하여 현재 보안 상황에 대한 개요를 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-134">Review Microsoft's [top trends in cybersecurity](https://www.microsoft.com/security/operations/security-intelligence-report) report to get an overview of the current security landscape.</span></span>
- <span data-ttu-id="882b7-135">조직에서 [Security DevOps](https://www.microsoft.com/en-us/securityengineering/devsecops)(보안 DevOps) 역할을 개발하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-135">Consider developing a [Security DevOps](https://www.microsoft.com/en-us/securityengineering/devsecops) role in your organization.</span></span>

<!-- "en-us" location is required for the URL above. -->

## <a name="build-and-pre-deployment"></a><span data-ttu-id="882b7-136">빌드 및 배포 전</span><span class="sxs-lookup"><span data-stu-id="882b7-136">Build and pre-deployment</span></span>

<span data-ttu-id="882b7-137">환경을 성공적으로 마이그레이션하려면 여러 가지 기술 및 비기술적 필수 구성 요소가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-137">A number of technical and nontechnical prerequisites are required to successful migrate an environment.</span></span> <span data-ttu-id="882b7-138">이 프로세스는 의사 결정, 준비 및 마이그레이션을 진행하는 핵심 인프라에 중점을 둡니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-138">This process focuses on the decisions, readiness, and core infrastructure that proceeds a migration.</span></span>

<span data-ttu-id="882b7-139">**최소 제안 작업:**</span><span class="sxs-lookup"><span data-stu-id="882b7-139">**Minimum suggested activities:**</span></span>

- <span data-ttu-id="882b7-140">배포 전 단계에서 롤아웃하여 [보안 기준 도구 체인](toolchain.md)을 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-140">Implement your [Security Baseline toolchain](toolchain.md) by rolling out in a pre-deployment phase.</span></span>
- <span data-ttu-id="882b7-141">아키텍처 지침 문서를 업데이트하고 주요 이해 관계자에게 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-141">Update the Architecture Guidelines document and distribute to key stakeholders.</span></span>
- <span data-ttu-id="882b7-142">보안 작업을 우선 순위가 지정된 마이그레이션 백로그에 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-142">Implement security tasks on your prioritized migration backlog.</span></span>
- <span data-ttu-id="882b7-143">교육 자료와 설명서, 인식 커뮤니케이션, 인센티브 및 다른 프로그램을 개발하여 사용자의 채택을 촉진합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-143">Develop educational materials and documentation, awareness communications, incentives, and other programs to help drive user adoption.</span></span>

<span data-ttu-id="882b7-144">**잠재적 활동:**</span><span class="sxs-lookup"><span data-stu-id="882b7-144">**Potential activities:**</span></span>

- <span data-ttu-id="882b7-145">클라우드 호스팅 데이터에 대한 조직의 [암호화](../../decision-guides/encryption/overview.md) 전략을 결정합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-145">Determine your organization's [encryption](../../decision-guides/encryption/overview.md) strategy for cloud-hosted data.</span></span>
- <span data-ttu-id="882b7-146">클라우드 배포의 [ID](../../decision-guides/identity/overview.md) 전략을 평가합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-146">Evaluate your cloud deployment's [identity](../../decision-guides/identity/overview.md) strategy.</span></span> <span data-ttu-id="882b7-147">클라우드 기반 ID 솔루션이 온-프레미스 ID 공급자와 공존하거나 통합되는 방법을 결정합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-147">Determine how your cloud-based identity solution will coexist or integrate with on-premises identity providers.</span></span>
- <span data-ttu-id="882b7-148">가상화된 네트워킹 기능을 안전하게 유지할 수 있도록 [SDN(소프트웨어 정의 네트워킹)](../../decision-guides/software-defined-network/overview.md) 설계에 대한 네트워크 경계 정책을 결정합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-148">Determine network boundary policies for your [Software Defined Networking (SDN)](../../decision-guides/software-defined-network/overview.md) design to ensure secure virtualized networking capabilities.</span></span>
- <span data-ttu-id="882b7-149">조직의 [최소 권한 액세스](/azure/active-directory/users-groups-roles/roles-delegate-by-task) 정책을 평가하고, 작업 기반 역할을 사용하여 특정 리소스에 대한 액세스를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-149">Evaluate your organization's [least privilege access](/azure/active-directory/users-groups-roles/roles-delegate-by-task) policies, and use task-based roles to provide access to specific resources.</span></span>
- <span data-ttu-id="882b7-150">보안 및 모니터링 메커니즘을 모든 클라우드 서비스 및 가상 머신에 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-150">Apply security and monitoring mechanisms to for all cloud services and virtual machines.</span></span>
- <span data-ttu-id="882b7-151">가능한 경우 [보안 정책](../../decision-guides/policy-enforcement/overview.md)을 자동화합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-151">Automate [security policies](../../decision-guides/policy-enforcement/overview.md) where possible.</span></span>
- <span data-ttu-id="882b7-152">보안 기준 정책을 검토하고, [보안 개발 수명 주기](https://www.microsoft.com/securityengineering/sdl/)에 요약된 것과 같은 모범 사례 지침에 따라 계획을 수정해야 하는지 여부를 결정합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-152">Review your Security Baseline policy and determine if you need to modify your plans according to best practices guidance such as those outlined in the [Security Development Lifecycle](https://www.microsoft.com/securityengineering/sdl/).</span></span>

## <a name="adopt-and-migrate"></a><span data-ttu-id="882b7-153">채택 및 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="882b7-153">Adopt and migrate</span></span>

<span data-ttu-id="882b7-154">마이그레이션은 기존 디지털 자산에서 애플리케이션 또는 워크로드의 이동, 테스트 및 채택에 중점을 둔 점진적 프로세스입니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-154">Migration is an incremental process that focuses on the movement, testing, and adoption of applications or workloads in an existing digital estate.</span></span>

<span data-ttu-id="882b7-155">**최소 제안 작업:**</span><span class="sxs-lookup"><span data-stu-id="882b7-155">**Minimum suggested activities:**</span></span>

- <span data-ttu-id="882b7-156">[보안 기준 도구 체인](toolchain.md)을 배포 전 환경에서 프로덕션 환경으로 마이그레이션합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-156">Migrate your [Security Baseline toolchain](toolchain.md) from pre-deployment to production.</span></span>
- <span data-ttu-id="882b7-157">아키텍처 지침 문서를 업데이트하고 주요 이해 관계자에게 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-157">Update the Architecture Guidelines document and distribute to key stakeholders.</span></span>
- <span data-ttu-id="882b7-158">교육 자료와 설명서, 인식 커뮤니케이션, 인센티브 및 다른 프로그램을 개발하여 사용자의 채택을 촉진합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-158">Develop educational materials and documentation, awareness communications, incentives, and other programs to help drive user adoption</span></span>

<span data-ttu-id="882b7-159">**잠재적 활동:**</span><span class="sxs-lookup"><span data-stu-id="882b7-159">**Potential activities:**</span></span>

- <span data-ttu-id="882b7-160">최신 보안 기준과 위협 정보를 검토하여 새 비즈니스 위험을 식별합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-160">Review the latest Security Baseline and threat information to identify any new business risks.</span></span>
- <span data-ttu-id="882b7-161">발생할 수 있는 새 보안 위험을 처리할 수 있는 조직의 허용 범위를 측정합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-161">Gauge your organization's tolerance to handle new security risks that may arise.</span></span>
- <span data-ttu-id="882b7-162">정책의 편차를 식별하고 수정을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-162">Identify deviations from policy, and enforce corrections.</span></span>
- <span data-ttu-id="882b7-163">보안 및 액세스 제어 자동화를 조정하여 정책 준수를 최대한 보장합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-163">Adjust security and access control automation to ensure maximum policy compliance.</span></span>  
- <span data-ttu-id="882b7-164">빌드/배포 전 단계에서 정의된 모범 사례가 제대로 실행되는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-164">Validate that the best practices defined during the Build / Pre-deployment phases are properly executed.</span></span>
- <span data-ttu-id="882b7-165">최소 권한 액세스 정책을 검토하고, 액세스 제어를 조정하여 보안을 최대화합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-165">Review your least privilege access policies and adjust access controls to maximize security.</span></span>
- <span data-ttu-id="882b7-166">워크로드에 대해 보안 기준 도구 체인을 테스트하여 취약성을 식별하고 해결합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-166">Test your Security Baseline toolchain against your workloads to identify and resolve any vulnerabilities.</span></span>

## <a name="operate-and-post-implementation"></a><span data-ttu-id="882b7-167">운영 및 구현 후</span><span class="sxs-lookup"><span data-stu-id="882b7-167">Operate and post-implementation</span></span>

<span data-ttu-id="882b7-168">전환이 완료되면 애플리케이션 또는 워크로드의 원래 수명 주기 동안 거버넌스와 운영이 지속되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-168">Once the transformation is complete, governance and operations must live on for the natural lifecycle of an application or workload.</span></span> <span data-ttu-id="882b7-169">이 거버넌스 성숙 단계는 솔루션을 구현하고 전환 주기의 안정화를 시작한 후에 일반적으로 수행되는 활동에 중점을 둡니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-169">This phase of governance maturity focuses on the activities that commonly come after the solution is implemented and the transformation cycle begins to stabilize.</span></span>

<span data-ttu-id="882b7-170">**최소 제안 작업:**</span><span class="sxs-lookup"><span data-stu-id="882b7-170">**Minimum suggested activities:**</span></span>

- <span data-ttu-id="882b7-171">[보안 기준 도구 체인](toolchain.md)을 유효성 검사 및/또는 구체화합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-171">Validate and/or refine your [Security Baseline toolchain](toolchain.md).</span></span>
- <span data-ttu-id="882b7-172">알림 및 보고서를 사용자 지정하여 잠재적인 보안 문제를 경고합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-172">Customize notifications and reports to alert you of potential security issues.</span></span>
- <span data-ttu-id="882b7-173">향후 채택 프로세스를 안내하기 위한 아키텍처 지침을 구체화합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-173">Refine the Architecture Guidelines to guide future adoption processes.</span></span>
- <span data-ttu-id="882b7-174">영향을 받는 팀에 주기적으로 전달하고 교육하여 아키텍처 지침을 지속적으로 준수하도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-174">Communicate and educate the affected teams periodically to ensure ongoing adherence to architecture guidelines.</span></span>

<span data-ttu-id="882b7-175">**잠재적 활동:**</span><span class="sxs-lookup"><span data-stu-id="882b7-175">**Potential activities:**</span></span>

- <span data-ttu-id="882b7-176">워크로드에 대한 패턴과 동작을 검색하고, 모니터링 및 보고 도구를 구성하여 비정상적인 활동, 액세스 또는 리소스 사용을 식별하고 알려줍니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-176">Discover patterns and behavior for your workloads and configure your monitoring and reporting tools to identify and notify you of any abnormal activity, access, or resource usage.</span></span>
- <span data-ttu-id="882b7-177">모니터링 및 보고 정책을 지속적으로 업데이트하여 최신 취약성, 악용 및 공격을 탐지합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-177">Continuously update your monitoring and reporting policies to detect the latest vulnerabilities, exploits, and attacks.</span></span>
- <span data-ttu-id="882b7-178">무단 액세스를 빠르게 중지하고 공격자에 의해 손상되었을 수 있는 리소스를 사용하지 않도록 설정하는 절차를 마련합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-178">Have procedures in place to quickly stop unauthorized access and disable resources that may have been compromised by an attacker.</span></span>
- <span data-ttu-id="882b7-179">가능한 경우 최신 보안 모범 사례를 정기적으로 검토하고, 보안 정책, 자동화 및 모니터링 기능에 추천 사항을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-179">Regularly review the latest security best practices and apply recommendations to your security policy, automation, and monitoring capabilities where possible.</span></span>

## <a name="next-steps"></a><span data-ttu-id="882b7-180">다음 단계</span><span class="sxs-lookup"><span data-stu-id="882b7-180">Next steps</span></span>

<span data-ttu-id="882b7-181">이제 클라우드 보안 거버넌스의 개념을 이해했으므로 [Microsoft에서 제공하는 Azure에 대한 보안 및 모범 사례 지침](azure-security-guidance.md)에 대해 자세히 알아봅니다.</span><span class="sxs-lookup"><span data-stu-id="882b7-181">Now that you understand the concept of cloud security governance, move on to learn more about [what security and best practices guidance Microsoft provides](azure-security-guidance.md) for Azure.</span></span>

> [!div class="nextstepaction"]
> <span data-ttu-id="882b7-182">[Azure 보안 지침에 자세한 정보](azure-security-guidance.md)
> [Azure 보안 소개](/azure/security/azure-security)
> [로깅, 보고 및 모니터링에 자세한 정보](../../decision-guides/log-and-report/overview.md)</span><span class="sxs-lookup"><span data-stu-id="882b7-182">[Learn about security guidance for Azure](azure-security-guidance.md)
[Introduction to Azure Security](/azure/security/azure-security)
[Learn about logging, reporting, and monitoring](../../decision-guides/log-and-report/overview.md)</span></span>
