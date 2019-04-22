---
title: 'CAF: 대규모 엔터프라이즈 – 모범 사례 설명'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 대규모 엔터프라이즈 – 모범 사례 설명
author: BrianBlanchard
ms.openlocfilehash: 2d52797f1c3541fab1c97d97d0438210d2e66f79
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59068993"
---
# <a name="large-enterprise-best-practice-explained"></a><span data-ttu-id="78c7f-103">대기업: 설명된 모범 사례</span><span class="sxs-lookup"><span data-stu-id="78c7f-103">Large enterprise: Best practice explained</span></span>

<span data-ttu-id="78c7f-104">거버넌스 과정은 일련의 초기 [회사 정책](./initial-corporate-policy.md) 세트에서 시작됩니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-104">The governance journey starts with a set of initial [corporate policies](./initial-corporate-policy.md).</span></span> <span data-ttu-id="78c7f-105">이러한 정책은 [모범 사례](./overview.md)를 반영하는 거버넌스 MVP(최소 기능 제품)를 만드는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-105">These policies are used to establish a minimum viable product (MVP) for governance that reflects [best practices](./overview.md).</span></span>

<span data-ttu-id="78c7f-106">이 문서에서는 거버넌스 MVP를 만드는 데 필요한 개략적인 전략을 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-106">In this article, we discuss the high-level strategies that are required to create a governance MVP.</span></span> <span data-ttu-id="78c7f-107">거버넌스 MVP의 핵심은 [배포 가속](../../deployment-acceleration/overview.md) 분야에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-107">The core of the governance MVP is the [Deployment Acceleration](../../deployment-acceleration/overview.md) discipline.</span></span> <span data-ttu-id="78c7f-108">이 단계에서 적용되는 도구 및 패턴은 향후 거버넌스의 확장에 필요한 점층적 개선을 가능하게 할 것입니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-108">The tools and patterns applied at this stage will enable the incremental evolutions needed to expand governance in the future.</span></span>

## <a name="governance-mvp-cloud-adoption-foundation"></a><span data-ttu-id="78c7f-109">거버넌스 MVP(클라우드 도입 기반)</span><span class="sxs-lookup"><span data-stu-id="78c7f-109">Governance MVP (Cloud Adoption Foundation)</span></span>

<span data-ttu-id="78c7f-110">몇 가지 간단한 원칙과 클라우드 기반 거버넌스 도구를 통해 거버넌스 및 기업 정책의 빠른 도입이 가능해집니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-110">Rapid adoption of governance and corporate policy is achievable, thanks to a few simple principles and cloud-based governance tooling.</span></span> <span data-ttu-id="78c7f-111">이것이 모든 거버넌스 프로세스에서 접근할 세 가지 거버넌스 분야 중 첫 번째입니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-111">These are the first of the three governance disciplines to approach in any governance process.</span></span> <span data-ttu-id="78c7f-112">이 문서에서는 각 분야를 자세히 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-112">Each will be expanded upon in this article.</span></span>

<span data-ttu-id="78c7f-113">도입부를 만들기 위해, 이 문서에서는 거버넌스 MVP를 만드는 데 필요한 ID 기준, 보안 기준, 배포 가속화의 바탕이 되는 개괄적인 전략을 논의하며, 이는 모든 채택의 토대가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-113">To establish the starting point, this article will discuss the high-level strategies behind Identity Baseline, Security Baseline, and Deployment Acceleration that are required to create a governance MVP, which will serve as the foundation for all adoption.</span></span>

![점층적 거버넌스 MVP의 예](../../../_images/governance/governance-mvp.png)

## <a name="implementation-process"></a><span data-ttu-id="78c7f-115">구현 프로세스</span><span class="sxs-lookup"><span data-stu-id="78c7f-115">Implementation process</span></span>

<span data-ttu-id="78c7f-116">거버넌스 MVP를 구현할 때는 ID, 보안, 네트워킹을 고려해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-116">The implementation of the governance MVP has dependencies on Identity, Security, and Networking.</span></span> <span data-ttu-id="78c7f-117">이러한 항목이 거버넌스의 구현에 미치는 영향을 파악하고 나면 클라우드 거버넌스 팀은 거버넌스의 몇 가지 측면을 결정합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-117">Once the dependencies are resolved, the Cloud Governance team will decide a few aspects of governance.</span></span> <span data-ttu-id="78c7f-118">클라우드 거버넌스 팀 및 지원 팀이 결정한 사항은 단일 적용 자산 패키지를 통해 구현됩니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-118">The decisions from the Cloud Governance team and from supporting teams will be implemented through a single package of enforcement assets.</span></span>

![점층적 거버넌스 MVP의 예](../../../_images/governance/governance-mvp-implementation-flow.png)

<span data-ttu-id="78c7f-120">이 구현은 다음과 같은 간단한 검사 목록을 통해서도 설명할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-120">This implementation can also be described using a simple checklist:</span></span>

1. <span data-ttu-id="78c7f-121">핵심 종속성에 관한 결정을 요청합니다. ID, 네트워크 및 암호화가 핵심 종속성에 해당합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-121">Solicit decisions regarding core dependencies: Identity, Network, and Encryption.</span></span>
2. <span data-ttu-id="78c7f-122">회사 정책 적용 중에 사용할 패턴을 결정합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-122">Determine the pattern to be used during corporate policy enforcement.</span></span>
3. <span data-ttu-id="78c7f-123">리소스에 일관성, 리소스 태그 및 로깅 및 보고 분야에 대 한 적절 한 거 버 넌 스 패턴을 결정 합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-123">Determine the appropriate governance patterns for the Resource Consistency, Resource Tagging, and Logging and Reporting disciplines.</span></span>
4. <span data-ttu-id="78c7f-124">종속 결정 및 거버넌스 결정을 적용하기 위해 선택한 정책 적용 패턴에 맞는 거버넌스 도구를 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-124">Implement the governance tools aligned to the chosen policy enforcement pattern to apply the dependent decisions and governance decisions.</span></span>

[!INCLUDE [implementation-process](../../../../../includes/cloud-adoption/governance/implementation-process.md)]

## <a name="application-of-governance-defined-patterns"></a><span data-ttu-id="78c7f-125">거버넌스 정의 패턴의 적용</span><span class="sxs-lookup"><span data-stu-id="78c7f-125">Application of governance-defined patterns</span></span>

<span data-ttu-id="78c7f-126">클라우드 거버넌스 팀은 다음 의사 결정 및 구현을 담당합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-126">The Cloud Governance team will be responsible for the following decisions and implementations.</span></span> <span data-ttu-id="78c7f-127">대다수 팀은 다른 팀의 의견과 조언을 들어야 하지만 클라우드 거버넌스 팀은 의사 결정과 구현을 모두 자체 처리할 가능성이 높습니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-127">Many will require inputs from other teams, but the Cloud Governance team is likely to own both the decision and implementation.</span></span> <span data-ttu-id="78c7f-128">다음 섹션에서는 이 사용 사례에서 이루어진 의사 결정과 각 결정의 세부 정보를 간략히 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-128">The following sections outline the decisions made for this use case and details of each decision.</span></span>

### <a name="subscription-model"></a><span data-ttu-id="78c7f-129">구독 모델</span><span class="sxs-lookup"><span data-stu-id="78c7f-129">Subscription Model</span></span>

<span data-ttu-id="78c7f-130">Azure 구독에 **혼합** 패턴을 선택했습니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-130">The **Mixed** pattern has been chosen for Azure subscriptions.</span></span>

- <span data-ttu-id="78c7f-131">Azure 리소스의 새 요청이 발생하면 각 운영 지역의 주요 사업부에 "부서"가 설립되어져야 합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-131">As new requests for Azure resources arise, a "Department" should be established for each major business unit in each operating geography.</span></span> <span data-ttu-id="78c7f-132">각 부서 안의 각 애플리케이션 원형에는 "구독"이 만들어져야 합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-132">Within each of the Departments, "Subscriptions" should be created for each application archetype.</span></span>
- <span data-ttu-id="78c7f-133">애플리케이션 원형은 요구 사항이 유사한 애플리케이션을 그룹화하는 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-133">An application archetype is a means of grouping applications with similar needs.</span></span> <span data-ttu-id="78c7f-134">일반적인 예제는 다음을 포함합니다. 데이터를 보호하는 애플리케이션, 관리되는 애플리케이션(예: HIPAA 또는 FedRAMP), 저위험 애플리케이션, 온-프레미스 종속성/SAP/Azure의 기타 메인프레임이 있는 애플리케이션, 온-프레미스 SAP 또는 메인프레임을 확장하는 애플리케이션.</span><span class="sxs-lookup"><span data-stu-id="78c7f-134">Common examples include: Applications with protected data, governed applications (such as HIPAA or FedRAMP), low-risk applications, applications with on-premises dependencies, SAP or other mainframes in Azure, or applications that extend on-premises SAP or mainframes.</span></span> <span data-ttu-id="78c7f-135">각 조직에는 업무를 지원하는 애플리케이션의 유형 및 데이터 분류에 기반한 고유한 요구 사항이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-135">Each organization has unique needs based on data classifications and the types of applications that support the business.</span></span> <span data-ttu-id="78c7f-136">디지털 자산의 종속성 매핑은 조직에서 애플리케이션 원형을 정의하는 데 도움이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-136">Dependency mapping of the digital estate can help define the application archetypes in an organization.</span></span>
- <span data-ttu-id="78c7f-137">공통 명명 규칙은 글머리 기호가 붙은 위의 두 항목에 따라 구독 디자인 과정에서 합의되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-137">A common naming convention should be agreed upon as part of the subscription design, based on the above two bullets.</span></span>

### <a name="resource-consistency"></a><span data-ttu-id="78c7f-138">리소스 일관성</span><span class="sxs-lookup"><span data-stu-id="78c7f-138">Resource Consistency</span></span>

<span data-ttu-id="78c7f-139">**계층적 일관성**이 리소스 일관성 패턴으로 선택되었습니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-139">**Hierarchical Consistency** has been chosen as a Resource Consistency pattern.</span></span>

- <span data-ttu-id="78c7f-140">각 애플리케이션의 리소스 그룹을 만들어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-140">Resource groups should be created for each application.</span></span> <span data-ttu-id="78c7f-141">각 애플리케이션 원형의 관리 그룹을 만들어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-141">Management groups should be created for each application archetype.</span></span> <span data-ttu-id="78c7f-142">관련 관리 그룹의 모든 구독에 Azure Policy를 적용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-142">Azure Policy should be applied to all subscriptions in the associated management group.</span></span>
- <span data-ttu-id="78c7f-143">배포 프로세스의 일환으로, 모든 자산의 리소스 일관성 템플릿을 소스 제어에 저장해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-143">As part of the deployment process, Resource Consistency templates for all assets should be stored in source control.</span></span>
- <span data-ttu-id="78c7f-144">각 리소스 그룹을 특정 워크로드나 애플리케이션에 맞춰 조정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-144">Each resource group should align to a specific workload or application.</span></span>
- <span data-ttu-id="78c7f-145">정의한 Azure 관리 그룹 계층 구조는 중첩된 그룹을 사용하여 청구 책임 및 애플리케이션 소유권을 나타내야 합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-145">The Azure management group hierarchy defined should represent billing responsibility and application ownership using nested groups.</span></span>
- <span data-ttu-id="78c7f-146">Azure Policy 구현이 광범위하므로 팀의 확정 시간을 초과하고, 당장은 큰 가치를 제공하지 못할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-146">Extensive implementation of Azure Policy could exceed the team’s time commitments and may not provide much value at this point.</span></span> <span data-ttu-id="78c7f-147">그러나 초기의 일부 클라우드 거버넌스 정책 문을 시행하기 위해서는 간단한 기본 정책을 만들어 처음 몇 개의 리소스 그룹에 적용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-147">However, a simple default policy should be created and applied to each resource group to enforce the first few cloud governance policy statements.</span></span> <span data-ttu-id="78c7f-148">이는 특정 거버넌스 요구 사항의 구현 방식을 정의하는 용도로 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-148">This serves to define the implementation of specific governance requirements.</span></span> <span data-ttu-id="78c7f-149">그런 다음, 이러한 구현을 모든 배포 자산에 걸쳐 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-149">Those implementations can then be applied across all deployed assets.</span></span>

### <a name="resource-tagging"></a><span data-ttu-id="78c7f-150">리소스 태그 지정</span><span class="sxs-lookup"><span data-stu-id="78c7f-150">Resource Tagging</span></span>

<span data-ttu-id="78c7f-151">리소스 태그 지정을 위해 **계정** 패턴을 선택했습니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-151">The **Accounting** pattern has been chosen for resource tagging.</span></span>

- <span data-ttu-id="78c7f-152">배포된 자산에는 다음에 대한 가치로 태그가 지정되어져야 합니다. 부서/청구 단위, 지리, 데이터 분류, 중요성, SLA, 환경, 애플리케이션 원형, 애플리케이션 및 애플리케이션 소유자에 대한 가치가 그것입니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-152">Deployed assets should be tagged with values for the following: Department/Billing Unit, Geography, Data Classification, Criticality, SLA, Environment, Application Archetype, Application, and Application Owner.</span></span>
- <span data-ttu-id="78c7f-153">배포된 자산과 연결된 Azure 관리 그룹 및 구독과 함께 이러한 차치는 거버넌스, 운영, 보안 의사 결정에 영향을 미칩니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-153">These values along with the Azure management group and subscription associated with a deployed asset will drive governance, operations, and security decisions.</span></span>

### <a name="logging-and-reporting"></a><span data-ttu-id="78c7f-154">로깅 및 보고</span><span class="sxs-lookup"><span data-stu-id="78c7f-154">Logging and reporting</span></span>

<span data-ttu-id="78c7f-155">현재, 로그, 보고를 위한 **하이브리드** 패턴이 권장되기는 하지만, 모든 개발 팀이 해당 패턴을 반드시 사용해야 하는 것은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-155">At this point, a **Hybrid** pattern for log and reporting is suggested but not required of any development team.</span></span>

- <span data-ttu-id="78c7f-156">로깅 및 보고 목적으로 수집될 특정 데이터 요소와 관련해서 현재 설정된 거버넌스 요구 사항은 없습니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-156">No governance requirements are currently set regarding the specific data points to be collected for logging or reporting purposes.</span></span> <span data-ttu-id="78c7f-157">이 문서의 내용은 이러한 가상의 상황에 국한되므로 반대의 경우를 고려하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-157">This is specific to this fictional narrative and should be considered an antipattern.</span></span> <span data-ttu-id="78c7f-158">로깅 표준은 가능한 한 빨리 결정하고 적용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-158">Logging standards should be determined and enforced as soon as possible.</span></span>
- <span data-ttu-id="78c7f-159">보호된 데이터 또는 중요 업무용 워크로드를 릴리스하기 전에 추가 분석이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-159">Additional analysis is required before the release of any protected data or mission-critical workloads.</span></span>
- <span data-ttu-id="78c7f-160">보호된 데이터 또는 중요 업무용 워크로드를 지원하기 전에 먼저 로깅에 사용되는 작업 영역에 대한 액세스 권한을 기존의 온-프레미스 운영 모니터링 솔루션에 부여해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-160">Prior to supporting protected data or mission-critical workloads, the existing on-premises operational monitoring solution must be granted access to the workspace used for logging.</span></span> <span data-ttu-id="78c7f-161">정의된 SLA에 따라 애플리케이션을 지원해야 할 경우 애플리케이션은 해당 테넌트의 사용과 관련된 보안 및 로깅 요구 사항을 충족해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-161">Applications are required to meet security and logging requirements associated with the use of that tenant, if the application is to be supported with a defined SLA.</span></span>

## <a name="evolution-of-governance-processes"></a><span data-ttu-id="78c7f-162">거버넌스 프로세스의 진화</span><span class="sxs-lookup"><span data-stu-id="78c7f-162">Evolution of governance processes</span></span>

<span data-ttu-id="78c7f-163">자동화 도구로 제어할 수 없거나 제어해서는 안 되는 정책 문이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-163">Some of the policy statements cannot or should not be controlled by automated tooling.</span></span> <span data-ttu-id="78c7f-164">IT 보안 및 온-프레미스 ID 기준 팀이 정기적으로 관리해야 하는 정책도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-164">Other policies will require periodic effort from IT Security and on-premises Identity Baseline teams.</span></span> <span data-ttu-id="78c7f-165">클라우드 거버넌스 팀은 다음 프로세스를 감독하여 마지막 8개 정책 문을 구현해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-165">The Cloud Governance team will need to oversee the following processes to implement the last eight policy statements:</span></span>

<span data-ttu-id="78c7f-166">**회사 정책 변경**: 클라우드 거버넌스 팀은 새 정책을 채택하도록 거버넌스 MVP 디자인을 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-166">**Corporate Policy Changes**: The Cloud Governance team will make changes to the governance MVP design to adopt the new policies.</span></span> <span data-ttu-id="78c7f-167">거버넌스 MVP의 가치는 새 정책의 자동 적용이 가능하다는 점에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-167">The value of the governance MVP is that it will allow for the automatic enforcement of the new policies.</span></span>

<span data-ttu-id="78c7f-168">**채택 가속화**: 클라우드 거버넌스 팀은 여러 팀의 배포 스크립트를 검토해왔습니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-168">**Adoption Acceleration**: The Cloud Governance team has been reviewing deployment scripts across multiple teams.</span></span> <span data-ttu-id="78c7f-169">또한 배포 템플릿으로 사용되는 스크립트 세트를 유지 관리해왔습니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-169">They've maintained a set of scripts that serve as deployment templates.</span></span> <span data-ttu-id="78c7f-170">클라우드 채택 팀과 DevOps 팀은 이러한 템플릿을 사용하여 배포를 더 빠르게 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-170">Those templates can be used by the cloud adoption teams and DevOps teams to more quickly define deployments.</span></span> <span data-ttu-id="78c7f-171">각 스크립트에는 거버넌스 정책을 적용하는 데 필요한 요구 사항이 있으므로 클라우드 채택 엔지니어의 추가 작업은 필요 없습니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-171">Each script contains the requirements for enforcing governance policies, and additional effort from cloud adoption engineers is not needed.</span></span> <span data-ttu-id="78c7f-172">이러한 스크립트의 큐레이터로서 이들은 정책 변경을 좀 더 빠르게 구현할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-172">As the curators of these scripts, they can implement policy changes more quickly.</span></span> <span data-ttu-id="78c7f-173">또한 채택을 가속화하는 역할을 합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-173">Additionally, they are viewed as accelerators of adoption.</span></span> <span data-ttu-id="78c7f-174">이를 통해 엄격한 적용 규정 없이도 일관된 배포를 진행할 수 있게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-174">This ensures consistent deployments without strictly enforcing adherence.</span></span>

<span data-ttu-id="78c7f-175">**엔지니어 교육**: 클라우드 거버넌스 팀은 격월로 교육 세션을 제공하고, 엔지니어를 대상으로 2개의 비디오를 제작했습니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-175">**Engineer Training**: The Cloud Governance team offers bi-monthly training sessions and has created two videos for engineers.</span></span> <span data-ttu-id="78c7f-176">두 가지 리소스 모두 엔지니어들이 거버넌스 문화와 배포 수행 방법을 빠르게 숙지하게 돕습니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-176">Both resources help engineers get up to speed quickly on the governance culture and how deployments are performed.</span></span> <span data-ttu-id="78c7f-177">팀은 프로덕션 및 비프로덕션 배포 간의 차이점을 보여 주는 교육 자산을 추가하여 엔지니어들이 새 정책이 채택에 미치는 영향을 이해할 수 있게 합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-177">The team is adding training assets to demonstrate the difference between production and non-production deployments, which helps engineers understand how the new policies affect adoption.</span></span> <span data-ttu-id="78c7f-178">이를 통해 엄격한 적용 규정 없이도 일관된 배포를 진행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-178">This ensures consistent deployments without strictly enforcing adherence.</span></span>

<span data-ttu-id="78c7f-179">**배포 계획**: 보호 된 데이터를 포함 하는 모든 자산을 배포 하기 전에 클라우드 거 버 넌 스 팀이 거 버 넌 스 맞춤의 유효성을 검사 하는 배포 스크립트를 검토할 책임이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-179">**Deployment Planning**: Before deploying any asset containing protected data, the Cloud Governance team will be responsible for reviewing deployment scripts to validate governance alignment.</span></span> <span data-ttu-id="78c7f-180">이전에 배포가 승인된 기존 팀이 있다면 프로그래밍 방식 도구를 사용하여 감사합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-180">Existing teams with previously approved deployments will be audited using programmatic tooling.</span></span>

<span data-ttu-id="78c7f-181">**월별 감사 및 보고**: 매월, 클라우드 거버넌스 팀은 모든 클라우드 배포를 감사하여 정책에 잘 맞는지 지속적으로 검증합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-181">**Monthly Audit and Reporting**: Each month, the Cloud Governance team runs an audit of all cloud deployments to validate continued alignment to policy.</span></span> <span data-ttu-id="78c7f-182">차이점이 발견되면 문서로 정리한 후 클라우드 채택 팀과 공유합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-182">When deviations are discovered, they are documented and shared with the cloud adoption teams.</span></span> <span data-ttu-id="78c7f-183">비즈니스 중단이나 데이터 누수 위험이 없는 경우 정책이 자동으로 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-183">When enforcement doesn't risk a business interruption or data leak, the policies are automatically enforced.</span></span> <span data-ttu-id="78c7f-184">감사가 끝나면 클라우드 거버넌스 팀은 클라우드 전략 팀과 각 클라우드 도입 팀에 대한 보고서를 정리하여 전반적인 정책 준수 상황을 전달합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-184">At the end of the audit, the Cloud Governance team compiles a report for the Cloud Strategy team and each cloud adoption team to communicate overall adherence to policy.</span></span> <span data-ttu-id="78c7f-185">또한 이 보고서는 감사 및 법적 목적으로 저장됩니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-185">The report is also stored for auditing and legal purposes.</span></span>

<span data-ttu-id="78c7f-186">**분기별 정책 검토**: 분기마다 클라우드 거버넌스 팀과 클라우드 전략 팀은 감사 결과를 검토하고 회사 정책 변경을 제안합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-186">**Quarterly Policy Review**: Each quarter, the Cloud Governance team and Cloud Strategy team to review audit results and suggest changes to corporate policy.</span></span> <span data-ttu-id="78c7f-187">이러한 제안 사항의 대부분은 사용 패턴의 지속적인 개선과 관찰을 통해 얻어집니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-187">Many of those suggestions are the result of continuous improvements and the observation of usage patterns.</span></span> <span data-ttu-id="78c7f-188">승인된 정책 변경 내용은 후속 감사 주기 동안 거버넌스 도구에 통합됩니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-188">Approved policy changes are integrated into governance tooling during subsequent audit cycles.</span></span>

## <a name="alternative-patterns"></a><span data-ttu-id="78c7f-189">대체 패턴</span><span class="sxs-lookup"><span data-stu-id="78c7f-189">Alternative patterns</span></span>

<span data-ttu-id="78c7f-190">이 거버넌스 과정에서 선택한 패턴이 읽는 사람의 요구와 맞지 않으면 각 패턴의 대체 패턴을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-190">If any of the patterns chosen in this governance journey don't align with the reader's requirements, alternatives to each pattern are available:</span></span>

- [<span data-ttu-id="78c7f-191">암호화 패턴</span><span class="sxs-lookup"><span data-stu-id="78c7f-191">Encryption patterns</span></span>](../../../decision-guides/encryption/overview.md)
- [<span data-ttu-id="78c7f-192">ID 패턴</span><span class="sxs-lookup"><span data-stu-id="78c7f-192">Identity patterns</span></span>](../../../decision-guides/identity/overview.md)
- [<span data-ttu-id="78c7f-193">로깅 및 보고 패턴</span><span class="sxs-lookup"><span data-stu-id="78c7f-193">Logging and Reporting patterns</span></span>](../../../decision-guides/log-and-report/overview.md)
- [<span data-ttu-id="78c7f-194">정책 적용 패턴</span><span class="sxs-lookup"><span data-stu-id="78c7f-194">Policy Enforcement patterns</span></span>](../../../decision-guides/policy-enforcement/overview.md)
- [<span data-ttu-id="78c7f-195">리소스 일관성 패턴</span><span class="sxs-lookup"><span data-stu-id="78c7f-195">Resource Consistency patterns</span></span>](../../../decision-guides/resource-consistency/overview.md)
- [<span data-ttu-id="78c7f-196">리소스 태그 지정 패턴</span><span class="sxs-lookup"><span data-stu-id="78c7f-196">Resource Tagging patterns</span></span>](../../../decision-guides/resource-tagging/overview.md)
- [<span data-ttu-id="78c7f-197">소프트웨어 정의 네트워크 패턴</span><span class="sxs-lookup"><span data-stu-id="78c7f-197">Software Defined Network patterns</span></span>](../../../decision-guides/software-defined-network/overview.md)
- [<span data-ttu-id="78c7f-198">구독 디자인 패턴</span><span class="sxs-lookup"><span data-stu-id="78c7f-198">Subscription Design patterns</span></span>](../../../decision-guides/subscriptions/overview.md)

## <a name="next-steps"></a><span data-ttu-id="78c7f-199">다음 단계</span><span class="sxs-lookup"><span data-stu-id="78c7f-199">Next steps</span></span>

<span data-ttu-id="78c7f-200">이 지침을 구현하고 나면 각 클라우드 채택 팀이 견고한 거버넌스를 토대로 작업을 진행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-200">Once this guidance is implemented, each cloud adoption team can proceed with a solid governance foundation.</span></span> <span data-ttu-id="78c7f-201">클라우드 거버넌스 팀은 회사 정책 및 거버넌스 분야를 지속적으로 업데이트하기 위해 노력합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-201">The Cloud Governance team will work in parallel to continually update the corporate policies and governance disciplines.</span></span>

<span data-ttu-id="78c7f-202">두 팀은 허용 오차 지표를 사용하여 클라우드 채택을 계속 지원하는 데 필요한 차기 발전 방향을 파악합니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-202">Both teams will use the tolerance indicators to identify the next evolution needed to continue supporting cloud adoption.</span></span> <span data-ttu-id="78c7f-203">이 과정에서 회사는 레거시 또는 타사 MFA(Multi-Factor Authentication) 요구 사항에 따라 애플리케이션을 지원하도록 거버넌스 기준을 발전시키게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="78c7f-203">The next step for the company in this journey is to evolve their governance baseline to support applications with legacy or third-party multifactor authentication (MFA) requirements.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="78c7f-204">ID 기준 진화</span><span class="sxs-lookup"><span data-stu-id="78c7f-204">Identity Baseline evolution</span></span>](./identity-baseline-evolution.md)