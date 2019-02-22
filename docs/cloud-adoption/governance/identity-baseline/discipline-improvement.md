---
title: 'CAF: ID 기준 분야 개선'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: ID 기준 분야 개선
author: BrianBlanchard
ms.openlocfilehash: c96a638af549782fec22b2068c9b4943df4b943a
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901317"
---
# <a name="identity-baseline-discipline-improvement"></a><span data-ttu-id="5836a-103">ID 기준 분야 개선</span><span class="sxs-lookup"><span data-stu-id="5836a-103">Identity Baseline discipline improvement</span></span>

<span data-ttu-id="5836a-104">ID 기준 분야는 애플리케이션 또는 워크로드를 호스트하는 클라우드 공급 기업에 관계 없이 사용자 ID의 일관성 및 연속성을 보장하는 정책을 설정하는 방법에 집중합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-104">The Identity Baseline discipline focuses on ways of establishing policies that ensure consistency and continuity of user identities regardless of the cloud provider that hosts the application or workload.</span></span> <span data-ttu-id="5836a-105">5개 분야의 클라우드 거버넌스 내에서 ID 기준은 [하이브리드 ID 전략](../../decision-guides/identity/overview.md), ID 리포지토리의 평가 및 확장, Single Sign-On 구현(동일한 로그온), 무단 사용 또는 악의적인 행위자에 대한 감사 및 모니터링에 관한 결정을 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-105">Within the Five Disciplines of Cloud Governance, Identity Baseline includes decisions regarding the [Hybrid Identity Strategy](../../decision-guides/identity/overview.md), evaluation and extension of identity repositories, implementation of single sign-on (same sign-on), auditing and monitoring for unauthorized use or malicious actors.</span></span> <span data-ttu-id="5836a-106">경우에 따라 이 기준에는 여러 ID 공급 기업을 현대화 및 통합하는 결정도 포함될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-106">In some cases, it may also involve decisions to modernize, consolidate, or integrate multiple identity providers.</span></span>

<span data-ttu-id="5836a-107">이 문서에서는 ID 기준 분야를 더욱 효율적으로 개발하고 완성도를 높이기 위해 회사가 수행할 수 있는 몇 가지 작업을 간략하게 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-107">This article outlines some potential tasks your company can engage in to better develop and mature the Identity Baseline discipline.</span></span> <span data-ttu-id="5836a-108">이러한 작업은 클라우드 솔루션을 구현하는 과정의 계획, 빌드, 도입, 운영 단계로 구분한 다음, [클라우드 거버넌스에 대한 증분 방식](../journeys/overview.md#an-incremental-approach-to-cloud-governance)의 개발을 허용하는 과정에서 이러한 단계를 반복적으로 수행하게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-108">These tasks can be broken down into planning, building, adopting, and operating phases of implementing a cloud solution, which are then iterated on allowing the development of an [incremental approach to cloud governance](../journeys/overview.md#an-incremental-approach-to-cloud-governance).</span></span>

![도입의 4단계](../../_images/adoption-phases.png)

<span data-ttu-id="5836a-110">*그림 1. 클라우드 거버넌스의 증분 방식 도입 단계.*</span><span class="sxs-lookup"><span data-stu-id="5836a-110">*Figure 1. Adoption phases of the incremental approach to cloud governance.*</span></span>

<span data-ttu-id="5836a-111">한 문서에서 모든 비즈니스의 요구 사항을 설명할 수는 없습니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-111">It's impossible for any one document to account for the requirements of all businesses.</span></span> <span data-ttu-id="5836a-112">따라서 이 문서에서는 거버넌스 완성 프로세스의 각 단계에서 권장되는 최소 활동과 수행할 가능성이 있는 활동의 예를 간략하게 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-112">As such, this article outlines suggested minimum and potential example activities for each phase of the governance maturation process.</span></span> <span data-ttu-id="5836a-113">이러한 활동의 초기 목표는 [정책 MVP](../journeys/overview.md#an-incremental-approach-to-cloud-governance)를 빌드하고 증분 방식 정책 개선을 위한 프레임워크를 설정하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-113">The initial objective of these activities is to help you build a [Policy MVP](../journeys/overview.md#an-incremental-approach-to-cloud-governance) and establish a framework for incremental policy evolution.</span></span> <span data-ttu-id="5836a-114">클라우드 거버넌스 팀은 ID 기준 거버넌스 기능을 개선하기 위해 이러한 활동에 투자할 비용을 결정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-114">Your Cloud Governance team will need to decide how much to invest in these activities to improve your Identity Baseline governance capabilities.</span></span>

> [!CAUTION]
> <span data-ttu-id="5836a-115">이 문서에서 설명하는 최소 활동과 수행할 가능성이 있는 활동은 특정 기업 정책 또는 타사 규정 준수 요구 사항을 기준으로 작성된 것이 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-115">Neither the minimum or potential activities outlined in this article are aligned to specific corporate policies or third party compliance requirements.</span></span> <span data-ttu-id="5836a-116">이 지침은 클라우드 거버넌스 모델을 사용하여 두 요구 사항을 모두 충족하기 위한 논의를 원활하게 진행할 수 있도록 설계되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-116">This guidance is designed to help facilitate the conversations that will lead to alignment of both requirements with a cloud governance model.</span></span>

## <a name="planning-and-readiness"></a><span data-ttu-id="5836a-117">계획 및 준비</span><span class="sxs-lookup"><span data-stu-id="5836a-117">Planning and readiness</span></span>

<span data-ttu-id="5836a-118">이 거버넌스 완성 과정 단계에서는 서로 분리되어 있는 사업 결과와 실행 가능한 전략을 연결합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-118">This phase of governance maturity bridges the divide between business outcomes and actionable strategies.</span></span> <span data-ttu-id="5836a-119">이 프로세스에서는 리더십 팀이 특정 메트릭을 정의하여 디지털 자산에 매핑한 다음, 전반적인 마이그레이션 작업 계획을 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-119">During this process, the leadership team defines specific metrics, maps those metrics to the digital estate, and begins planning the overall migration effort.</span></span>

<span data-ttu-id="5836a-120">**최소 권장 활동:**</span><span class="sxs-lookup"><span data-stu-id="5836a-120">**Minimum suggested activities:**</span></span>

* <span data-ttu-id="5836a-121">[ID 도구 체인](toolchain.md) 옵션을 평가하고 조직에 적합한 하이브리드 전략을 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-121">Evaluate your [Identity toolchain](toolchain.md) options and implement a hybrid strategy that is appropriate to your organization.</span></span>
* <span data-ttu-id="5836a-122">초안 아키텍처 지침 문서를 개발하여 주요 관련자에게 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-122">Develop a draft Architecture Guidelines document and distribute to key stakeholders.</span></span>
* <span data-ttu-id="5836a-123">아키텍처 지침 개발에서 영향을 받은 직원과 팀을 교육하고 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-123">Educate and involve the people and teams affected by the development of architecture guidelines.</span></span>

<span data-ttu-id="5836a-124">**수행할 가능성이 있는 활동:**</span><span class="sxs-lookup"><span data-stu-id="5836a-124">**Potential activities:**</span></span>

* <span data-ttu-id="5836a-125">클라우드에서 ID 및 액세스 관리를 제어하는 역할 및 할당을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-125">Define roles and assignments that will govern identity and access management in the cloud.</span></span>
* <span data-ttu-id="5836a-126">온-프레미스 그룹을 정의하여 해당 클라우드 기반 역할에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-126">Define your on-premises groups and map to corresponding cloud-based roles.</span></span>
* <span data-ttu-id="5836a-127">인벤토리 ID 공급 기업(사용자 지정 애플리케이션에서 사용되는 데이터베이스 기반 ID 포함).</span><span class="sxs-lookup"><span data-stu-id="5836a-127">Inventory identity providers (including database-driven identities used by custom applications).</span></span>
* <span data-ttu-id="5836a-128">중복된 ID 공급 기업의 통합 옵션을 고려하여 전반적인 ID 솔루션을 단순화합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-128">Consider options for consolidation or integration of identity providers where duplication exists, to simplify the overall identity solution.</span></span>
* <span data-ttu-id="5836a-129">기존 ID 공급 기업의 하이브리드 호환성을 평가합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-129">Evaluate hybrid compatibility of existing identity providers.</span></span>
* <span data-ttu-id="5836a-130">하이브리드 호환성이 없는 ID 공급 기업의 경우 통합 옵션 또는 대체 옵션을 평가합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-130">For identity providers that are not hybrid compatible, evaluate consolidation or replacement options.</span></span>

## <a name="build-and-pre-deployment"></a><span data-ttu-id="5836a-131">빌드 및 배포 전 단계</span><span class="sxs-lookup"><span data-stu-id="5836a-131">Build and pre-deployment</span></span>

<span data-ttu-id="5836a-132">여러 기술적 및 비기술적 필수 조건은 환경을 성공적으로 마이그레이션해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-132">A number of technical and nontechnical prerequisites are required to successfully migrate an environment.</span></span> <span data-ttu-id="5836a-133">이 프로세스에서는 결정, 준비 및 마이그레이션을 진행하는 핵심 인프라에 중점을 둡니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-133">This process focuses on the decisions, readiness, and core infrastructure that proceeds a migration.</span></span>

<span data-ttu-id="5836a-134">**최소 권장 활동:**</span><span class="sxs-lookup"><span data-stu-id="5836a-134">**Minimum suggested activities:**</span></span>

* <span data-ttu-id="5836a-135">[ID 도구 체인](toolchain.md)을 구현하기 전에 파일럿 테스트를 고려하여 도구 체인이 사용자 경험을 최대한 간소화하는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-135">Consider a pilot test before implementing your [Identity toolchain](toolchain.md), making sure it simplifies the user experience as much as possible.</span></span>
* <span data-ttu-id="5836a-136">파일럿 테스트에서 배포 전 단계로 피드백을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-136">Apply feedback from pilot tests into the pre-deployment.</span></span> <span data-ttu-id="5836a-137">결과가 허용될 때까지 반복합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-137">Repeat until results are acceptable.</span></span>
* <span data-ttu-id="5836a-138">배포 및 사용자 도입 계획을 포함하여 아키텍처 지침 문서를 업데이트하여 주요 관련자에게 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-138">Update the Architecture Guidelines document to include deployment and user adoption plans, and distribute to key stakeholders.</span></span>
* <span data-ttu-id="5836a-139">얼리어답터 프로그램을 설정하고 제한된 수의 사용자에게 롤아웃하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-139">Consider establishing an early adopter program and rolling out to a limited number of users.</span></span>
* <span data-ttu-id="5836a-140">아키텍처 지침에서 영향을 가장 많이 받는 직원과 팀을 대상으로 지속적인 교육을 진행합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-140">Continue to educate the people and teams most affected by the architecture guidelines.</span></span>

<span data-ttu-id="5836a-141">**수행할 가능성이 있는 활동:**</span><span class="sxs-lookup"><span data-stu-id="5836a-141">**Potential activities:**</span></span>

* <span data-ttu-id="5836a-142">논리적, 물리적 아키텍처를 평가하고 [하이브리드 ID 전략](../../decision-guides/identity/overview.md)을 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-142">Evaluate your logical and physical architecture and determine a [Hybrid Identity Strategy](../../decision-guides/identity/overview.md).</span></span>
* <span data-ttu-id="5836a-143">로그인 ID 할당과 같은 ID 액세스 관리 정책을 매핑하고, Azure AD의 적절한 인증 방법을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-143">Map identity access management policies, such as login ID assignments, and choose the appropriate authentication method for Azure AD.</span></span>
  * <span data-ttu-id="5836a-144">페더레이션한 경우 관리 계정에 대한 테넌트 제한을 사용하도록 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-144">If federated, enable tenant restrictions for administrative accounts.</span></span>
* <span data-ttu-id="5836a-145">온-프레미스 및 클라우드 디렉터리를 통합합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-145">Integrate your on-premises and cloud directories.</span></span>
* <span data-ttu-id="5836a-146">다음 액세스 모델을 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-146">Consider using the following access models:</span></span>
  * <span data-ttu-id="5836a-147">[최소 권한 액세스](/windows-server/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models) 모델</span><span class="sxs-lookup"><span data-stu-id="5836a-147">[Least Privilege Access](/windows-server/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models) model</span></span>
  * <span data-ttu-id="5836a-148">[권한 있는 ID 기준](/azure/active-directory/privileged-identity-management/pim-configure) 액세스 모델</span><span class="sxs-lookup"><span data-stu-id="5836a-148">[Privileged Identity Baseline](/azure/active-directory/privileged-identity-management/pim-configure) access model</span></span>
* <span data-ttu-id="5836a-149">통합 전 세부 정보를 모두 완료하고 [ID 모범 사례](/azure/security/azure-security-identity-management-best-practices)를 검토합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-149">Finalize all pre-integration details and review [Identity Best Practices](/azure/security/azure-security-identity-management-best-practices).</span></span>
  * <span data-ttu-id="5836a-150">단일 ID, SSO(Single Sign-On) 또는 원활한 SSO를 사용하도록 설정</span><span class="sxs-lookup"><span data-stu-id="5836a-150">Enable single identity, single sign-on (SSO), or seamless SSO</span></span>
  * <span data-ttu-id="5836a-151">관리자에 대해 MFA(Multi-Factor Authentication) 구성</span><span class="sxs-lookup"><span data-stu-id="5836a-151">Configure multi-factor authentication (MFA) for administrators</span></span>
  * <span data-ttu-id="5836a-152">필요한 경우 ID 공급 기업 통합</span><span class="sxs-lookup"><span data-stu-id="5836a-152">Consolidate or integrate identity providers, where necessary</span></span>
  * <span data-ttu-id="5836a-153">ID 관리를 중앙 집중화하는 데 필요한 도구 구현</span><span class="sxs-lookup"><span data-stu-id="5836a-153">Implement tooling necessary to centralize management of identities</span></span>
  * <span data-ttu-id="5836a-154">JIT(Just-In-Time) 액세스 및 역할 변경 경고 사용하도록 설정</span><span class="sxs-lookup"><span data-stu-id="5836a-154">Enable just-in-time (JIT) access and role change alerting</span></span>
  * <span data-ttu-id="5836a-155">기본 제공 역할에 할당하기 위한 키 관리 활동의 위험 분석 수행</span><span class="sxs-lookup"><span data-stu-id="5836a-155">Conduct a risk analysis of key admin activities for assigning to built-in roles</span></span>
  * <span data-ttu-id="5836a-156">더 강력한 모든 사용자 인증의 롤아웃 업데이트 고려</span><span class="sxs-lookup"><span data-stu-id="5836a-156">Consider an updated rollout of stronger authentication for all users</span></span>
  * <span data-ttu-id="5836a-157">추가 관리 역할에 대해 JIT(시간이 제한된 활성화 사용)의 PIM(권한 있는 ID 기준)을 사용하도록 설정</span><span class="sxs-lookup"><span data-stu-id="5836a-157">Enable Privileged Identity Baseline (PIM) for JIT (using time-limited activation) for additional administrative roles</span></span>
  * <span data-ttu-id="5836a-158">글로벌 관리자 계정에서 사용자 계정 분리(관리자가 실수로 이메일을 열거나, 해당 글로벌 관리자 계정과 연결된 프로그램을 실행하지 않아야 함)</span><span class="sxs-lookup"><span data-stu-id="5836a-158">Separate user accounts from Global admin accounts (to make sure that administrators do not inadvertently open emails or run programs associated with their Global admin accounts)</span></span>

## <a name="adopt-and-migrate"></a><span data-ttu-id="5836a-159">도입 및 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="5836a-159">Adopt and migrate</span></span>

<span data-ttu-id="5836a-160">마이그레이션은 기존 디지털 자산의 애플리케이션이나 워크로드 이동, 테스트 및 도입에 중점을 두는 증분 방식 프로세스입니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-160">Migration is an incremental process that focuses on the movement, testing, and adoption of applications or workloads in an existing digital estate.</span></span>

<span data-ttu-id="5836a-161">**최소 권장 활동:**</span><span class="sxs-lookup"><span data-stu-id="5836a-161">**Minimum suggested activities:**</span></span>

* <span data-ttu-id="5836a-162">[ID 도구 체인](toolchain.md)을 개발 환경에서 프로덕션 환경으로 마이그레이션합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-162">Migrate your [Identity toolchain](toolchain.md) from development to production.</span></span>
* <span data-ttu-id="5836a-163">아키텍처 지침 문서를 업데이트하여 주요 관계자에게 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-163">Update the Architecture Guidelines document and distribute to key stakeholders.</span></span>
* <span data-ttu-id="5836a-164">교육 자료와 설명서, 인식 정보, 인센티브 및 기타 프로그램을 개발하여 사용자 도입을 촉진할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-164">Develop educational materials and documentation, awareness communications, incentives, and other programs to help drive user adoption.</span></span>

<span data-ttu-id="5836a-165">**수행할 가능성이 있는 활동:**</span><span class="sxs-lookup"><span data-stu-id="5836a-165">**Potential activities:**</span></span>

* <span data-ttu-id="5836a-166">빌드/배포 전 단계에서 정의한 모범 사례가 적절하게 실행되는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-166">Validate that the best practices defined during the Build / Pre-deployment phases are properly executed.</span></span>
* <span data-ttu-id="5836a-167">[하이브리드 ID 전략](../../decision-guides/identity/overview.md)을 확인하고 또는 구체화합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-167">Validate and/or refine your [Hybrid Identity Strategy](../../decision-guides/identity/overview.md).</span></span>
* <span data-ttu-id="5836a-168">릴리스 전에 각 애플리케이션 또는 워크로드가 ID 전략에 적합한지 계속 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-168">Ensure that each application or workload continues to align with the identity strategy before release.</span></span>
* <span data-ttu-id="5836a-169">SSO(Single Sign-On) 및 원활한 SSO가 애플리케이션에 대해 예상 대로 작동하는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-169">Validate that single sign-on (SSO) and seamless SSO is working as expected for your applications.</span></span>
* <span data-ttu-id="5836a-170">가능하면 대체 ID 저장소의 수를 줄이거나 제거합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-170">Reduce or eliminate the number of alternative identity stores, when possible.</span></span>
* <span data-ttu-id="5836a-171">모든 앱 내 또는 데이터베이스 내 ID 저장소의 필요성을 조사합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-171">Scrutinize the need for any in-app or in-database identity stores.</span></span> <span data-ttu-id="5836a-172">적절한 ID 공급 기업(자사 또는 타사) 범위에 속하지 않는 ID는 애플리케이션 및 사용자에게 위험이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-172">Identities that fall outside of a proper identity provider (first-party or third-party) can represent risk to the application and the users.</span></span>
* <span data-ttu-id="5836a-173">[온-프레미스 페더레이션된 애플리케이션](/azure/active-directory/active-directory-device-registration-on-premises-setup)에 대한 조건부 액세스를 사용하도록 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-173">Enable conditional access for [on-premises federated applications](/azure/active-directory/active-directory-device-registration-on-premises-setup).</span></span>
* <span data-ttu-id="5836a-174">지역 간 동기화를 사용하여 여러 허브의 글로벌 지역에 ID를 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-174">Distribute identity across global regions in multiple hubs with synchronization between regions.</span></span>
* <span data-ttu-id="5836a-175">중앙 RBAC(역할 기반 액세스 제어) 페더레이션을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-175">Establish central role-based access control (RBAC) federation.</span></span>

## <a name="operate-and-post-implementation"></a><span data-ttu-id="5836a-176">작동 및 구현 후 작업</span><span class="sxs-lookup"><span data-stu-id="5836a-176">Operate and post-implementation</span></span>

<span data-ttu-id="5836a-177">변환이 완료되면 애플리케이션이나 워크로드의 원래 수명 주기 동안 거버넌스와 관련 작업이 실행되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-177">Once the transformation is complete, governance and operations must live on for the natural lifecycle of an application or workload.</span></span> <span data-ttu-id="5836a-178">이 거버넌스 완성 과정 단계에서는 일반적으로 솔루션이 구현되고 변환 주기가 안정화되기 시작한 후에 진행되는 활동을 중점적으로 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-178">This phase of governance maturity focuses on the activities that commonly come after the solution is implemented and the transformation cycle begins to stabilize.</span></span>

<span data-ttu-id="5836a-179">**최소 권장 활동:**</span><span class="sxs-lookup"><span data-stu-id="5836a-179">**Minimum suggested activities:**</span></span>

* <span data-ttu-id="5836a-180">계속해서 바뀌는 조직의 ID 요구 변경 내용에 따라 [ID기준 도구 체인](toolchain.md)을 사용자 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-180">Customize your [Identity Baseline toolchain](toolchain.md) based on changes to your organization’s changing identity needs.</span></span>
* <span data-ttu-id="5836a-181">악의적인 위협 가능성에 대한 경고를 받을 수 있는 알림과 보고서를 자동화합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-181">Automate notifications and reports to alert you of potential malicious threats.</span></span>
* <span data-ttu-id="5836a-182">시스템 사용량 및 사용자 도입 진행률을 모니터링하고 보고합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-182">Monitor and report on system usage and user adoption progress.</span></span>
* <span data-ttu-id="5836a-183">배포 후 메트릭을 보고하여 관련자에게 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-183">Report on post-deployment metrics and distribute to stakeholders.</span></span>
* <span data-ttu-id="5836a-184">아키텍처 지침을 구체화하여 향후 도입 프로세스를 안내합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-184">Refine the Architecture Guidelines to guide future adoption processes.</span></span>
* <span data-ttu-id="5836a-185">정기적으로 영향을 받는 팀과 통신하고 지속적으로 교육을 진행하여 아키텍처 지침을 계속 준수해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-185">Communicate and continually educate the affected teams on a periodic basis to ensure ongoing adherence to architecture guidelines.</span></span>

<span data-ttu-id="5836a-186">**수행할 가능성이 있는 활동:**</span><span class="sxs-lookup"><span data-stu-id="5836a-186">**Potential activities:**</span></span>

* <span data-ttu-id="5836a-187">ID 정책 및 준수 사례를 정기적으로 감사합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-187">Conduct periodic audits of identity policies and adherence practices.</span></span>
* <span data-ttu-id="5836a-188">악의적인 행위자 및 데이터 위반 특히, 잠재적인 관리자 계정 인수 같은 ID 사기와 관련된 행위를 정기적으로 검사합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-188">Scan for malicious actors and data breaches regularly, particularly those related to identity fraud, such as potential admin account takeovers.</span></span>
* <span data-ttu-id="5836a-189">모니터링 및 보고 도구를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-189">Configure a monitoring and reporting tool.</span></span>
* <span data-ttu-id="5836a-190">사기 예방 및 보안 시스템과 더욱 긴밀하게 통합하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-190">Consider integrating more closely with security and fraud-prevention systems.</span></span>
* <span data-ttu-id="5836a-191">권한 상승된 사용자 또는 역할에 대한 액세스 권한을 정기적으로 검토합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-191">Regularly review access rights for elevated users or roles.</span></span>
  * <span data-ttu-id="5836a-192">관리자 권한을 활성화할 자격이 있는 모든 사용자를 식별합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-192">Identify every user who is eligible to activate admin privilege.</span></span>
* <span data-ttu-id="5836a-193">온보딩, 오프보딩 및 자격 증명 업데이트 프로세스를 검토합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-193">Review on-boarding, off-boarding, and credential update processes.</span></span>
* <span data-ttu-id="5836a-194">높은 수준의 자동화 및 IAM(ID 액세스 관리) 모듈 간의 통신을 조사합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-194">Investigate increasing levels of automation and communication between identity access management (IAM) modules.</span></span>
* <span data-ttu-id="5836a-195">개발 보안 작업(DevSecOps) 방법을 구현하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-195">Consider implementing a development security operations (DevSecOps) approach.</span></span>
* <span data-ttu-id="5836a-196">영향 분석을 수행하여 비용, 보안 및 사용자 도입의 결과를 측정합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-196">Carry out an impact analysis to gauge results on costs, security, and user adoption.</span></span>
* <span data-ttu-id="5836a-197">시스템에서 생성된 메트릭의 변경 내용을 보여주는 영향 보고서를 정기적으로 생성하고 [하이브리드 ID 전략](../../decision-guides/identity/overview.md)의 비즈니스 영향을 예측합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-197">Periodically produce an impact report that shows the changes in metrics created by the system and estimate the business impacts of the [Hybrid Identity Strategy](../../decision-guides/identity/overview.md).</span></span>
* <span data-ttu-id="5836a-198">[Azure Security Center](/azure/security-center/security-center-intro)에서 권장되는 통합 모니터링을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-198">Establish integrated monitoring recommended by the [Azure Security Center](/azure/security-center/security-center-intro).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5836a-199">다음 단계</span><span class="sxs-lookup"><span data-stu-id="5836a-199">Next steps</span></span>

<span data-ttu-id="5836a-200">이제 클라우드 ID 거버넌스의 개념을 이해했으므로 Azure 플랫폼에서 ID 기준 거버넌스 분야를 개발할 때 필요한 Azure 도구 및 기능을 식별하는 [ID 기준 도구 체인](toolchain.md)을 조사합니다.</span><span class="sxs-lookup"><span data-stu-id="5836a-200">Now that you understand the concept of cloud identity governance, examine the [Identity Baseline toolchain](toolchain.md) to identify Azure tools and features that you'll need when developing the Identity Baseline governance discipline on the Azure platform.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="5836a-201">Azure용 ID 기준 도구 체인</span><span class="sxs-lookup"><span data-stu-id="5836a-201">Identity Baseline toolchain for Azure</span></span>](toolchain.md)