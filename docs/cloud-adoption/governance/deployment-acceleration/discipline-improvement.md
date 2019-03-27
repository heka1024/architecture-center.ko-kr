---
title: 'CAF: 신속한 배포 분야의 개선 사항'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 신속한 배포 분야의 개선 사항
author: alexbuckgit
ms.openlocfilehash: 98192c777d8866efb01544737e8cabea6354c4d7
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58243724"
---
# <a name="deployment-acceleration-discipline-improvement"></a><span data-ttu-id="a313c-103">신속한 배포 분야의 개선 사항</span><span class="sxs-lookup"><span data-stu-id="a313c-103">Deployment Acceleration discipline improvement</span></span>

<span data-ttu-id="a313c-104">신속한 배포 분야에서는 리소스를 일관성 있게 반복적으로 배포 및 구성하고, 수명 주기 전반에 걸쳐 리소스가 규정을 준수하는 상태로 유지되도록 하는 정책을 중점적으로 수립합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-104">The Deployment Acceleration discipline focuses on establishing policies that ensure that resources are deployed and configured consistently and repeatably, and remain in compliance throughout their lifecycle.</span></span> <span data-ttu-id="a313c-105">클라우드 거버넌스의 5가지 분야 중 신속한 배포에는 배포 자동화, 배포 아티팩트의 원본 제어, 배포된 리소스를 모니터링해 적절한 상태 유지, 그리고 규정 준수 문제 감사와 관련된 결정이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-105">Within the Five Disciplines of Cloud Governance, Deployment Acceleration includes decisions regarding automating deployments, source-controlling deployment artifacts, monitoring deployed resources to maintain desired state, and auditing any compliance issues.</span></span>

<span data-ttu-id="a313c-106">이 문서에서는 신속한 배포 분야를 더욱 효율적으로 개발하고 완성도를 높이기 위해 수행할 수 있는 몇 가지 작업을 간략하게 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-106">This article outlines some potential tasks your company can engage in to better develop and mature the Deployment Acceleration discipline.</span></span> <span data-ttu-id="a313c-107">이러한 작업은 클라우드 솔루션 구현 과정의 계획/빌드/채택/운영 단계로 구분할 수 있습니다. [증분 방식 클라우드 거버넌스](../journeys/overview.md#an-incremental-approach-to-cloud-governance) 개발을 허용하는 과정에서 이러한 단계를 반복적으로 수행하게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-107">These tasks can be broken down into planning, building, adopting, and operating phases of implementing a cloud solution, which are then iterated on allowing the development of an [incremental approach to cloud governance](../journeys/overview.md#an-incremental-approach-to-cloud-governance).</span></span>

![도입의 4단계](../../_images/adoption-phases.png)

<span data-ttu-id="a313c-109">*그림 1. 클라우드 거버넌스의 증분 방식 도입 단계.*</span><span class="sxs-lookup"><span data-stu-id="a313c-109">*Figure 1. Adoption phases of the incremental approach to cloud governance.*</span></span>

<span data-ttu-id="a313c-110">한 문서에서 모든 비즈니스의 요구 사항을 설명할 수는 없습니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-110">It's impossible for any one document to account for the requirements of all businesses.</span></span> <span data-ttu-id="a313c-111">따라서 이 문서에서는 거버넌스 완성 프로세스의 각 단계에서 권장되는 최소 활동과 수행할 가능성이 있는 활동의 예를 간략하게 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-111">As such, this article outlines suggested minimum and potential example activities for each phase of the governance maturation process.</span></span> <span data-ttu-id="a313c-112">이러한 활동의 초기 목표는 [정책 MVP](../journeys/overview.md#an-incremental-approach-to-cloud-governance)를 빌드하고 증분 방식 정책 개선을 위한 프레임워크를 설정하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-112">The initial objective of these activities is to help you build a [Policy MVP](../journeys/overview.md#an-incremental-approach-to-cloud-governance) and establish a framework for incremental policy evolution.</span></span> <span data-ttu-id="a313c-113">클라우드 거버넌스 팀은 ID 기준 거버넌스 기능을 개선하기 위해 이러한 활동에 투자할 비용을 결정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-113">Your Cloud Governance team will need to decide how much to invest in these activities to improve your Identity Baseline governance capabilities.</span></span>

> [!CAUTION]
> <span data-ttu-id="a313c-114">이 문서에서 설명하는 최소 활동과 수행할 가능성이 있는 활동은 특정 기업 정책 또는 타사 규정 준수 요구 사항을 기준으로 작성된 것이 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-114">Neither the minimum or potential activities outlined in this article are aligned to specific corporate policies or third party compliance requirements.</span></span> <span data-ttu-id="a313c-115">이 지침은 클라우드 거버넌스 모델을 사용하여 두 요구 사항을 모두 충족하기 위한 논의를 원활하게 진행할 수 있도록 설계되었습니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-115">This guidance is designed to help facilitate the conversations that will lead to alignment of both requirements with a cloud governance model.</span></span>

## <a name="planning-and-readiness"></a><span data-ttu-id="a313c-116">계획 및 준비</span><span class="sxs-lookup"><span data-stu-id="a313c-116">Planning and readiness</span></span>

<span data-ttu-id="a313c-117">이 거버넌스 완성 과정 단계에서는 서로 분리되어 있는 사업 결과와 실행 가능한 전략을 연결합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-117">This phase of governance maturity bridges the divide between business outcomes and actionable strategies.</span></span> <span data-ttu-id="a313c-118">이 프로세스에서는 리더십 팀이 특정 메트릭을 정의하여 디지털 자산에 매핑한 다음, 전반적인 마이그레이션 작업 계획을 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-118">During this process, the leadership team defines specific metrics, maps those metrics to the digital estate, and begins planning the overall migration effort.</span></span>

<span data-ttu-id="a313c-119">**최소 권장 활동:**</span><span class="sxs-lookup"><span data-stu-id="a313c-119">**Minimum suggested activities:**</span></span>

- <span data-ttu-id="a313c-120">[신속한 배포 도구 체인](toolchain.md) 옵션을 평가하고 조직에 적합한 하이브리드 전략을 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-120">Evaluate your [Deployment Acceleration toolchain](toolchain.md) options and implement a hybrid strategy that is appropriate to your organization.</span></span>
- <span data-ttu-id="a313c-121">초안 아키텍처 지침 문서를 개발하여 주요 관련자에게 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-121">Develop a draft Architecture Guidelines document and distribute to key stakeholders.</span></span>
- <span data-ttu-id="a313c-122">아키텍처 지침 개발의 영향을 받는 직원과 팀을 개발 과정에 포함하고 교육을 진행합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-122">Educate and involve the people and teams affected by the development of Architecture Guidelines.</span></span>
- <span data-ttu-id="a313c-123">개발 팀과 IT 담당자가 DevSecOps 원칙과 전략, 그리고 신속한 개발 분야에서 완전 자동화된 배포의 중요성을 이해할 수 있도록 교육을 진행합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-123">Train development teams and IT staff to understand DevSecOps principles and strategies and the importance of fully automated deployments in the Deployment Acceleration Discipline.</span></span>

<span data-ttu-id="a313c-124">**수행할 가능성이 있는 활동:**</span><span class="sxs-lookup"><span data-stu-id="a313c-124">**Potential activities:**</span></span>

- <span data-ttu-id="a313c-125">클라우드에서 신속한 배포를 제어하는 역할과 할당을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-125">Define roles and assignments that will govern Deployment Acceleration in the cloud.</span></span>

## <a name="build-and-pre-deployment"></a><span data-ttu-id="a313c-126">빌드 및 배포 전 단계</span><span class="sxs-lookup"><span data-stu-id="a313c-126">Build and pre-deployment</span></span>

<span data-ttu-id="a313c-127">**최소 권장 활동:**</span><span class="sxs-lookup"><span data-stu-id="a313c-127">**Minimum suggested activities:**</span></span>

- <span data-ttu-id="a313c-128">새 클라우드 기반 애플리케이션의 경우 개발 프로세스 초기에 완전 자동화된 배포를 도입합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-128">For new cloud-based applications, introduce fully automated deployments early in the development process.</span></span> <span data-ttu-id="a313c-129">이 배포에 투자를 하면 테스트 프로세스의 안정성을 높이고 개발/QA/프로덕션 환경 전반에서 일관성을 유지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-129">This investment will improve the reliability of your testing processes and ensure consistency across your development, QA, and production environments.</span></span>
- <span data-ttu-id="a313c-130">GitHub 또는 Azure DevOps와 같은 원본 제어 플랫폼을 사용하여 배포 템플릿 또는 구성 스크립트와 같은 모든 배포 아티팩트를 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-130">Store all deployment artifacts such as deployment templates or configuration scripts using a source-control platform such as GitHub or Azure DevOps.</span></span>
- <span data-ttu-id="a313c-131">[신속한 배포 도구 체인](toolchain.md)을 구현하기 전에 파일럿 테스트를 진행하여 배포를 최대한 원활하게 수행할 수 있음을 확인할지 여부를 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-131">Consider a pilot test before implementing your [Deployment Acceleration toolchain](toolchain.md), making sure it streamlines your deployments as much as possible.</span></span> <span data-ttu-id="a313c-132">배포 전 단계에서 진행한 파일럿 테스트의 피드백을 적용합니다(필요에 따라 테스트 반복).</span><span class="sxs-lookup"><span data-stu-id="a313c-132">Apply feedback from pilot tests during the pre-deployment phase, repeating as needed.</span></span>
- <span data-ttu-id="a313c-133">애플리케이션의 논리적/물리적 아키텍처를 평가하고 애플리케이션 리소스의 배포를 자동화하거나 다른 클라우드 기반 리소스를 사용해 아키텍처의 각 부분을 개선할 수 있는 기회를 파악합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-133">Evaluate the logical and physical architecture of your applications, and identify opportunities to automate the deployment of application resources or improve portions of the architecture using other cloud-based resources.</span></span>
- <span data-ttu-id="a313c-134">배포 및 사용자 채틱 계획을 포함하여 아키텍처 지침 문서를 업데이트한 다음 주요 관련자에게 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-134">Update the Architecture Guidelines document to include deployment and user adoption plans, and distribute to key stakeholders.</span></span>
- <span data-ttu-id="a313c-135">아키텍처 지침의 영향을 가장 많이 받는 직원과 팀을 대상으로 지속적인 교육을 진행합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-135">Continue to educate the people and teams most affected by the architecture guidelines.</span></span>

<span data-ttu-id="a313c-136">**수행할 가능성이 있는 활동:**</span><span class="sxs-lookup"><span data-stu-id="a313c-136">**Potential activities:**</span></span>

- <span data-ttu-id="a313c-137">개발/QA/프로덕션 환경 전반에 걸쳐 애플리케이션에 대한 업데이트 릴리스를 완전히 관리하기 위한 CI/CD(연속 통합 및 지속적인 배포) 파이프라인을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-137">Define a continuous integration and continuous deployment (CI/CD) pipeline to fully manage releasing updates to your application through your development, QA, and production environments.</span></span>

## <a name="adopt-and-migrate"></a><span data-ttu-id="a313c-138">채택 및 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="a313c-138">Adopt and migrate</span></span>

<span data-ttu-id="a313c-139">마이그레이션은 기존 디지털 자산의 애플리케이션이나 워크로드 이동, 테스트 및 도입에 중점을 두는 증분 방식 프로세스입니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-139">Migration is an incremental process that focuses on the movement, testing, and adoption of applications or workloads in an existing digital estate.</span></span>

<span data-ttu-id="a313c-140">**최소 권장 활동:**</span><span class="sxs-lookup"><span data-stu-id="a313c-140">**Minimum suggested activities:**</span></span>

- <span data-ttu-id="a313c-141">[신속한 배포 도구 체인](toolchain.md)을 개발 환경에서 프로덕션 환경으로 마이그레이션합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-141">Migrate your [Deployment Acceleration toolchain](toolchain.md) from development to production.</span></span>
- <span data-ttu-id="a313c-142">아키텍처 지침 문서를 업데이트하여 주요 관련자에게 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-142">Update the Architecture Guidelines document and distribute to key stakeholders.</span></span>
- <span data-ttu-id="a313c-143">개발자의 IT 팀이 채택을 원활하게 진행할 수 있도록 교육 자료와 설명서, 인식 정보, 인센티브 및 기타 프로그램을 개발합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-143">Develop educational materials and documentation, awareness communications, incentives, and other programs to help drive developer and IT adoption.</span></span>

<span data-ttu-id="a313c-144">**수행할 가능성이 있는 활동:**</span><span class="sxs-lookup"><span data-stu-id="a313c-144">**Potential activities:**</span></span>

- <span data-ttu-id="a313c-145">빌드 및 배포 전 단계에서 정의한 모범 사례가 적절하게 실행되는지 유효성을 검사합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-145">Validate that the best practices defined during the build and pre-deployment phases are properly executed.</span></span>
- <span data-ttu-id="a313c-146">릴리스 전에 각 애플리케이션 또는 워크로드가 신속한 배포 전략에 적합한지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-146">Ensure that each application or workload aligns with the Deployment Acceleration strategy before release.</span></span>

## <a name="operate-and-post-implementation"></a><span data-ttu-id="a313c-147">작동 및 구현 후 작업</span><span class="sxs-lookup"><span data-stu-id="a313c-147">Operate and post-implementation</span></span>

<span data-ttu-id="a313c-148">변환이 완료되면 애플리케이션이나 워크로드의 원래 수명 주기 동안 거버넌스와 관련 작업이 실행되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-148">Once the transformation is complete, governance and operations must live on for the natural lifecycle of an application or workload.</span></span> <span data-ttu-id="a313c-149">이 거버넌스 완성 과정 단계에서는 일반적으로 솔루션이 구현되고 변환 주기가 안정화되기 시작한 후에 진행되는 활동을 중점적으로 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-149">This phase of governance maturity focuses on the activities that commonly come after the solution is implemented and the transformation cycle begins to stabilize.</span></span>

<span data-ttu-id="a313c-150">**최소 권장 활동:**</span><span class="sxs-lookup"><span data-stu-id="a313c-150">**Minimum suggested activities:**</span></span>

- <span data-ttu-id="a313c-151">계속해서 바뀌는 조직의 ID 요구 변경 내용에 따라 [신속한 배포 도구 체인](toolchain.md)을 사용자 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-151">Customize your [Deployment Acceleration toolchain](toolchain.md) based on changes to your organization’s changing identity needs.</span></span>
- <span data-ttu-id="a313c-152">구성 문제 가능성이나 악의적인 위협에 대한 경고를 받을 수 있도록 알림과 보고서를 자동화합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-152">Automate notifications and reports to alert you of potential configuration issues or malicious threats.</span></span>
- <span data-ttu-id="a313c-153">애플리케이션 및 리소스 사용량을 모니터링하고 보고합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-153">Monitor and report on application and resource usage.</span></span>
- <span data-ttu-id="a313c-154">배포 후 메트릭을 보고하고 관련자에게 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-154">Report on post-deployment metrics and distribute to stakeholders.</span></span>
- <span data-ttu-id="a313c-155">향후 채택 프로세스에서 참조할 수 있도록 아키텍처 지침을 수정합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-155">Revise the Architecture Guidelines to guide future adoption processes.</span></span>
- <span data-ttu-id="a313c-156">아키텍처 지침을 지속적으로 준수할 수 있도록 관련 직원/팀과 계속 정기적으로 정보를 교환하고 교육을 진행합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-156">Continue to communicate with and train the affected people and teams on a regular basis to ensure ongoing adherence to Architecture Guidelines.</span></span>

<span data-ttu-id="a313c-157">**수행할 가능성이 있는 활동:**</span><span class="sxs-lookup"><span data-stu-id="a313c-157">**Potential activities:**</span></span>

- <span data-ttu-id="a313c-158">원하는 상태 구성 모니터링 및 보고 도구를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-158">Configure a desired state configuration monitoring and reporting tool.</span></span>
- <span data-ttu-id="a313c-159">구성 도구 및 스크립트를 정기적으로 검토하여 프로세스를 개선하고 일반적인 문제를 파악합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-159">Regularly review configuration tools and scripts to improve processes and identify common issues.</span></span>
- <span data-ttu-id="a313c-160">개발, 운영 및 보안 팀과 협의하여 DevSecOps 사례를 완성하고 작업 효율성을 떨어뜨리는 조직의 사일로를 없앱니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-160">Work with development, operations, and security teams to help mature DevSecOps practices and break down organizational silos that lead to inefficiencies.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a313c-161">다음 단계</span><span class="sxs-lookup"><span data-stu-id="a313c-161">Next steps</span></span>

<span data-ttu-id="a313c-162">이제 클라우드 ID 거버넌스의 개념을 이해했으므로 Azure 플랫폼에서 ID 기준 거버넌스 분야를 개발할 때 필요한 Azure 도구 및 기능을 식별하는 [ID 기준 도구 체인](toolchain.md)을 조사합니다.</span><span class="sxs-lookup"><span data-stu-id="a313c-162">Now that you understand the concept of cloud identity governance, examine the [Identity Baseline toolchain](toolchain.md) to identify Azure tools and features that you'll need when developing the Identity Baseline governance discipline on the Azure platform.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="a313c-163">Azure용 ID 기준 도구 체인</span><span class="sxs-lookup"><span data-stu-id="a313c-163">Identity Baseline toolchain for Azure</span></span>](toolchain.md)
