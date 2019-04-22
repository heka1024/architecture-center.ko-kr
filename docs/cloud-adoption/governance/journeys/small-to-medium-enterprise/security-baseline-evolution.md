---
title: 'CAF: 소규모 및 중간 enterprise – 보안 기준선 발전'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 설명은 소규모 및 중간 enterprise – 보안 기준선 발전
author: BrianBlanchard
ms.openlocfilehash: bb26fa2f0d21bda6b1af1213bca817136b0c963f
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59640246"
---
# <a name="caf-small-to-medium-enterprise-security-baseline-evolution"></a><span data-ttu-id="7016c-103">CAF: 중소 엔터프라이즈: 보안 기준 진화</span><span class="sxs-lookup"><span data-stu-id="7016c-103">CAF: Small-to-medium enterprise: Security Baseline evolution</span></span>

<span data-ttu-id="7016c-104">이 문서에서는 보호된 데이터를 클라우드로 이동할 때 적용 가능한 보안 컨트롤을 추가하여 내러티브를 전개합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-104">This article evolves the narrative by adding security controls that support moving protected data to the cloud.</span></span>

## <a name="evolution-of-the-narrative"></a><span data-ttu-id="7016c-105">내러티브의 전개</span><span class="sxs-lookup"><span data-stu-id="7016c-105">Evolution of the narrative</span></span>

<span data-ttu-id="7016c-106">IT 및 비즈니스 리더십은 IT, 앱 개발 및 BI 팀에 의한 초기 단계 실험의 결과에 만족합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-106">IT and business leadership have been happy with results from early stage experimentation by the IT, App Development, and BI teams.</span></span> <span data-ttu-id="7016c-107">이러한 실험에서 실질적인 비즈니스 가치를 이끌어내려면 해당 팀은 보호된 데이터를 솔루션에 통합할 수 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-107">To realize tangible business values from these experiments, those teams must be allowed to integrate protected data into solutions.</span></span> <span data-ttu-id="7016c-108">이런 요구 사항은 회사 정책의 변경을 촉발시키지만 보호된 데이터를 클라우드로 이동할 수 있으려면 클라우드 거버넌스 구현의 개선이 추가로 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-108">This triggers changes to corporate policy, but also requires an evolution of the cloud governance implementations before protected data can land in the cloud.</span></span>

### <a name="evolution-of-the-cloud-governance-team"></a><span data-ttu-id="7016c-109">클라우드 거버넌스 팀의 개선</span><span class="sxs-lookup"><span data-stu-id="7016c-109">Evolution of the Cloud Governance team</span></span>

<span data-ttu-id="7016c-110">변화 하는 내 러 티브 및 지금 제공 되는 지원의 효과 지정 클라우드 거 버 넌 스 팀은 이제 볼 다르게 합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-110">Given the effect of the changing narrative and support provided so far, the Cloud Governance team is now viewed differently.</span></span> <span data-ttu-id="7016c-111">이 팀을 시작한 두 시스템 관리자는 이제 숙련된 클라우드 설계자로 보일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-111">The two system administrators who started the team are now viewed as experienced cloud architects.</span></span> <span data-ttu-id="7016c-112">이 내러티브가 전개되면서 두 관리자에 대한 인식은 클라우드 보유자에서 클라우드 보호자 역할로 바뀝니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-112">As this narrative develops, the perception of them will shift from being Cloud Custodians to more of a Cloud Guardian role.</span></span>

<span data-ttu-id="7016c-113">미묘한 차이지만 이 차이는 거버넌스 중심의 IT 문화를 조성할 경우 중요합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-113">While the difference is subtle, it’s an important distinction when building a governance- focused IT culture.</span></span> <span data-ttu-id="7016c-114">클라우드 보유자는 혁신적인 클라우드 설계자가 만든 문제를 정리합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-114">A Cloud Custodian cleans up the messes made by innovative cloud architects.</span></span> <span data-ttu-id="7016c-115">이 두 역할은 자연스럽게 충돌하며 서로 목표가 반대됩니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-115">The two roles have natural friction and opposing objectives.</span></span> <span data-ttu-id="7016c-116">반면, 클라우드 보호자는 클라우드를 안전하게 유지할 수 있으므로 다른 클라우드 설계자는 거의 문제를 일으키지 않고 더 신속하게 이동할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-116">On the other hand, a Cloud Guardian helps keep the cloud safe, so other cloud architects can move more quickly, with less messes.</span></span> <span data-ttu-id="7016c-117">또한 클라우드 보호를 5 개 분야의 클라우드 거 버 넌 스 defender 뿐만 아니라 혁신 액셀러레이터 쉽게 배포 및 채택 가속화 하는 템플릿 만들기에 관련 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-117">Additionally, a Cloud Guardian is involved in creating templates that accelerate deployment and adoption, making them an innovation accelerator as well as a defender of the Five Disciplines of Cloud Governance.</span></span>

### <a name="evolution-of-the-current-state"></a><span data-ttu-id="7016c-118">현재 상태의 발전</span><span class="sxs-lookup"><span data-stu-id="7016c-118">Evolution of the current state</span></span>

<span data-ttu-id="7016c-119">이 내러티브의 초반에 애플리케이션 개발 팀은 여전히 개발/테스트 직무를 수행하며 일하고 있었으며, BI 팀은 실험 단계에 머물러 있었습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-119">At the start of this narrative, the application development teams were still working in a dev/test capacity, and the BI team was still in the experimental phase.</span></span> <span data-ttu-id="7016c-120">IT는 Prod 및 DR이라는 호스팅된 인프라 환경 2개에서 운영됐습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-120">IT operated two hosted infrastructure environments, named Prod and DR.</span></span>

<span data-ttu-id="7016c-121">그 이후로 거버넌스에 영향을 주는 다음의 몇 가지 사항이 변경되었습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-121">Since then, some things have changed that will affect governance:</span></span>

- <span data-ttu-id="7016c-122">애플리케이션 개발 팀이 개선된 사용자 환경으로 클라우드 네이티브 애플리케이션을 배포하기 위해 CI/CD 파이프라인을 구현했습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-122">The application development team has implemented a CI/CD pipeline to deploy a cloud native application with an improved user experience.</span></span> <span data-ttu-id="7016c-123">해당 앱은 아직 보호된 데이터와 상호 작용하지 않으므로 프로덕션 준비가 완료된 것은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-123">That app doesn’t yet interact with protected data, so it is not production ready.</span></span>
- <span data-ttu-id="7016c-124">IT 팀 내의 비즈니스 인텔리전스 팀이 물류, 인벤토리 및 타사의 클라우드에서 데이터를 적극적으로 큐레이션합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-124">The Business Intelligence team within IT actively curates data in the cloud from logistics, inventory, and third party.</span></span> <span data-ttu-id="7016c-125">이 데이터는 비즈니스 프로세스를 생성할 수 있는 새로운 예측을 추진하는 데 사용되고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-125">This data is being used to drive new predictions, which could shape business processes.</span></span> <span data-ttu-id="7016c-126">그러나 고객 및 금융 데이터를 데이터 플랫폼에 통합할 때까지는 이러한 예측 및 인사이트가 실행 가능하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-126">However, those predictions and insights are not actionable until customer and financial data can be integrated into the data platform.</span></span>
- <span data-ttu-id="7016c-127">IT 팀이 DR 데이터 센터의 사용을 중지하는 CIO 및 CFO의 계획을 진행하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-127">The IT team is progressing on the CIO and CFO's plans to retire the DR datacenter.</span></span> <span data-ttu-id="7016c-128">DR 데이터 센터의 2,000개 자산 중 1,000개를 초과하는 자산이 사용 중지되거나 마이그레이션되었습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-128">More than 1,000 of the 2,000 assets in the DR datacenter have been retired or migrated.</span></span>
- <span data-ttu-id="7016c-129">PII 및 금융 데이터에 관해 느슨하게 정의된 정책이 업데이트되었습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-129">The loosely defined policies regarding PII and financial data have been modernized.</span></span> <span data-ttu-id="7016c-130">그러나 새 회사 정책은 관련 보안 및 거버넌스 정책의 구현 여부에 따르게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-130">However, the new corporate policies are contingent on the implementation of related security and governance policies.</span></span> <span data-ttu-id="7016c-131">관련 팀의 작업은 계속 중단 상태입니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-131">Teams are still stalled.</span></span>

### <a name="evolution-of-the-future-state"></a><span data-ttu-id="7016c-132">향후 상태의 개선</span><span class="sxs-lookup"><span data-stu-id="7016c-132">Evolution of the future state</span></span>

<span data-ttu-id="7016c-133">App Dev 팀과 BI 팀의 초기 실험 결과, 고객 경험 및 데이터 기반 결정 과정이 개선될 가능성이 있는 것으로 나타났습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-133">Early experiments by the App Dev and BI teams show potential improvements in customer experiences and data-driven decisions.</span></span> <span data-ttu-id="7016c-134">이 두 팀은 이러한 솔루션을 프로덕션 환경으로 배포하여 향후 18개월 동안 클라우드 도입 범위를 확장하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-134">Both teams want to expand adoption of the cloud over the next 18 months by deploying those solutions to production.</span></span>

<span data-ttu-id="7016c-135">남은 6개월 동안 클라우드 거버넌스 팀은 보안 및 거버넌스 요구 사항을 구현하여 클라우드 도입 팀이 해당 데이터 센터의 보호된 데이터를 마이그레이션할 수 있게 합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-135">During the remaining six months, the Cloud Governance team will implement security and governance requirements to allow the cloud adoption teams to migrate the protected data in those datacenters.</span></span>

<span data-ttu-id="7016c-136">현재와 미래의 상태가 변경되면 새로운 정책 설명이 필요한 새로운 위험이 생겨납니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-136">The changes to current and future state expose new risks that require new policy statements.</span></span>

## <a name="evolution-of-tangible-risks"></a><span data-ttu-id="7016c-137">실질적인 위험의 개선</span><span class="sxs-lookup"><span data-stu-id="7016c-137">Evolution of tangible risks</span></span>

<span data-ttu-id="7016c-138">**데이터 침해**: 새 데이터 플랫폼을 도입할 경우 잠재적인 데이터 침해 관련 책임이 기본적으로 증가하게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-138">**Data Breach**: When adopting any new data platform, there is an inherent increase in liabilities related to potential data breaches.</span></span> <span data-ttu-id="7016c-139">이로 인해 클라우드 기술을 채택하는 기술자가 이 위험을 줄일 수 있는 솔루션을 구현할 책임이 커졌습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-139">Technicians adopting cloud technologies have increased responsibilities to implement solutions that can decrease this risk.</span></span> <span data-ttu-id="7016c-140">기술자가 이러한 책임을 이행하도록 하려면 엄격한 보안 및 거버넌스 전략을 구현해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-140">A robust security and governance strategy must be implemented to ensure those technicians fulfill those responsibilities.</span></span>

<span data-ttu-id="7016c-141">이 비즈니스 위험은 다음과 같은 몇 가지 기술적 위험으로 확장될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-141">This business risk can be expanded into a few technical risks:</span></span>

- <span data-ttu-id="7016c-142">중요 업무용 응용 프로그램 또는 보호 된 데이터를 실수로 배포 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-142">Mission-critical applications or protected data might be deployed unintentionally.</span></span>
- <span data-ttu-id="7016c-143">암호화를 부적절하게 결정하여 보호된 데이터가 스토리지 중에 노출될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-143">Protected data might be exposed during storage due to poor encryption decisions.</span></span>
- <span data-ttu-id="7016c-144">권한이 없는 사용자가 보호된 데이터에 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-144">Unauthorized users might access protected data.</span></span>
- <span data-ttu-id="7016c-145">외부 침입자가 보호된 데이터에 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-145">External intrusion might result in access to protected data.</span></span>
- <span data-ttu-id="7016c-146">외부 침입 또는 서비스 거부 공격으로 인해 비즈니스가 중단될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-146">External intrusion or denial of service attacks might cause a business interruption.</span></span>
- <span data-ttu-id="7016c-147">조직 또는 고용 상태가 변경되어 보호된 데이터에 무단 액세스가 허용될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-147">Organization or employment changes might allow for unauthorized access to protected data.</span></span>
- <span data-ttu-id="7016c-148">새로운 악성 공격으로 인해 새로운 침입 또는 액세스 기회가 생길 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-148">New exploits could create new intrusion or access opportunities.</span></span>
- <span data-ttu-id="7016c-149">일관성 없는 배포 프로세스로 인해 보안 허점이 발생하여 데이터가 유출되거나 중단될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-149">Inconsistent deployment processes might result in security gaps, which could lead to data leaks or interruptions.</span></span>
- <span data-ttu-id="7016c-150">구성 드리프트 또는 패치 누락으로 인해 의도하지 않은 보안 허점이 발생하여 데이터가 유출되거나 중단될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-150">Configuration drift or missed patches might result in unintended security gaps, which could lead to data leaks or interruptions.</span></span>

## <a name="evolution-of-the-policy-statements"></a><span data-ttu-id="7016c-151">정책 설명 개선</span><span class="sxs-lookup"><span data-stu-id="7016c-151">Evolution of the policy statements</span></span>

<span data-ttu-id="7016c-152">정책을 다음과 같이 변경하면 새로운 위험을 완화하고 구현 과정을 안내할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-152">The following changes to policy will help mitigate the new risks and guide implementation.</span></span> <span data-ttu-id="7016c-153">목록은 길어 보이지만 이러한 정책을 도입하는 과정은 보기보다 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-153">The list looks long, but adopting these policies may be easier than it appears.</span></span>

1. <span data-ttu-id="7016c-154">배포된 모든 자산은 중요도 및 데이터 분류를 기준으로 분류되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-154">All deployed assets must be categorized by criticality and data classification.</span></span> <span data-ttu-id="7016c-155">자산을 클라우드로 배포하기 전에 클라우드 거버넌스 팀과 애플리케이션 소유자가 분류를 검토해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-155">Classifications are to be reviewed by the Cloud Governance team and the application owner before deployment to the cloud.</span></span>
2. <span data-ttu-id="7016c-156">보호된 데이터를 저장하거나 액세스하는 애플리케이션은 이렇게 하지 않는 애플리케이션과 다른 방식으로 관리해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-156">Applications that store or access protected data are to be managed differently than those that don’t.</span></span> <span data-ttu-id="7016c-157">최소한 의도치 않게 보호된 데이터에 액세스하지 않도록 해당 애플리케이션을 분할해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-157">At a minimum, they should be segmented to avoid unintended access of protected data.</span></span>
3. <span data-ttu-id="7016c-158">보호된 데이터는 미사용 시 모두 암호화해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-158">All protected data must be encrypted when at rest.</span></span>
4. <span data-ttu-id="7016c-159">보호된 데이터를 포함하는 모든 세그먼트에서 권한이 상승된 사용 권한은 예외로 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-159">Elevated permissions in any segment containing protected data should be an exception.</span></span> <span data-ttu-id="7016c-160">이러한 모든 예외는 클라우드 거버넌스 팀에서 기록하며, 정기적으로 감사를 받습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-160">Any such exceptions will be recorded with the Cloud Governance team and audited regularly.</span></span>
5. <span data-ttu-id="7016c-161">보호된 데이터를 포함하는 네트워크 서브넷은 기타 모든 서브넷에서 격리해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-161">Network subnets containing protected data must be isolated from any other subnets.</span></span> <span data-ttu-id="7016c-162">보호된 데이터 서브넷 간의 네트워크 트래픽은 정기적으로 감사합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-162">Network traffic between protected data subnets will be audited regularly.</span></span>
6. <span data-ttu-id="7016c-163">공용 인터넷이나 데이터 센터를 통해 보호된 데이터를 포함하는 서브넷에 직접 액세스할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-163">No subnet containing protected data can be directly accessed over the public internet or across datacenters.</span></span> <span data-ttu-id="7016c-164">해당 서브넷에 액세스하려면 중간 서브넷을 통해 라우팅되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-164">Access to those subnets must be routed through intermediate subnets.</span></span> <span data-ttu-id="7016c-165">해당 서브넷에 대한 모든 액세스는 패킷 검사 및 차단 기능을 수행할 수 있는 방화벽 솔루션을 거쳐야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-165">All access into those subnets must come through a firewall solution that can perform packet scanning and blocking functions.</span></span>
7. <span data-ttu-id="7016c-166">보안 관리 팀에서 정의한 네트워크 구성 요구 사항을 거버넌스 도구를 통해 감사하고 적용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-166">Governance tooling must audit and enforce network configuration requirements defined by the security management team.</span></span>
8. <span data-ttu-id="7016c-167">거버넌스 도구는 VM 배포를 승인된 이미지로만 제한해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-167">Governance tooling must limit VM deployment to approved images only.</span></span>
9. <span data-ttu-id="7016c-168">가능한 경우 항상 노드 구성 관리에서 모든 게스트 운영 체제의 구성에 정책 요구 사항을 적용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-168">Whenever possible, node configuration management should apply policy requirements to the configuration of any guest operating system.</span></span>
10. <span data-ttu-id="7016c-169">거버넌스 도구는 모든 배포된 자산에 자동 업데이트가 사용되도록 적용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-169">Governance tooling must enforce that automatic updates are enabled on all deployed assets.</span></span> <span data-ttu-id="7016c-170">위반은 운영 관리 팀에서 검토하고 작업 정책에 따라 수정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-170">Violations must be reviewed with operational management teams and remediated in accordance with operations policies.</span></span> <span data-ttu-id="7016c-171">자동으로 업데이트되지 않는 자산은 IT 운영 팀에서 소유한 프로세스에 포함돼야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-171">Assets that are not automatically updated must be included in processes owned by IT Operations.</span></span>
11. <span data-ttu-id="7016c-172">모든 중요 업무용 애플리케이션 또는 보호된 데이터용으로 새 구독이나 관리 그룹을 만들 경우 청사진이 적절하게 할당되도록 클라우드 거버넌스 팀의 검토가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-172">Creation of new subscriptions or management groups for any mission-critical applications or protected data will require a review from the Cloud Governance team, to ensure that the proper blueprint is assigned.</span></span>
12. <span data-ttu-id="7016c-173">중요 업무용 앱이나 보호된 데이터를 포함하는 모든 관리 그룹 또는 구독에는 최소 권한 액세스 모델이 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-173">A least-privilege access model will be applied to any management group or subscription that contains mission-critical apps or protected data.</span></span>
13. <span data-ttu-id="7016c-174">보안 팀에서 클라우드 배포에 영향을 줄 수 있는 추세 및 악용을 정기적으로 검토하여 클라우드에서 사용되는 보안 관리 도구에 업데이트를 제공해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-174">Trends and exploits that could affect cloud deployments should be reviewed regularly by the security team to provide updates to security management tooling used in the cloud.</span></span>
14. <span data-ttu-id="7016c-175">배포된 자산에 거버넌스를 지속적으로 적용하려면 클라우드 거버넌스 팀에서 배포 도구를 승인해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-175">Deployment tooling must be approved by the Cloud Governance team to ensure ongoing governance of deployed assets.</span></span>
15. <span data-ttu-id="7016c-176">정기적인 검토 및 감사를 위해서는 클라우드 거버넌스 팀이 액세스할 수 있는 중앙 리포지토리에서 배포 스크립트를 유지 관리해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-176">Deployment scripts must be maintained in a central repository accessible by the Cloud Governance team for periodic review and auditing.</span></span>
16. <span data-ttu-id="7016c-177">모든 자산에서 일관성을 유지하려면 거버넌스 프로세스에 배포 시점의 감사 내용과 정기적인 주기로 실행되는 감사 내용이 포함되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-177">Governance processes must include audits at the point of deployment and at regular cycles to ensure consistency across all assets.</span></span>
17. <span data-ttu-id="7016c-178">고객 인증이 필요한 모든 애플리케이션을 배포할 경우 내부 사용자용 기본 ID 공급 기업과 호환되는 승인된 ID 공급 기업을 사용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-178">Deployment of any applications that require customer authentication must use an approved identity provider that is compatible with the primary identity provider for internal users.</span></span>
18. <span data-ttu-id="7016c-179">클라우드 거버넌스 프로세스에는 ID 관리 팀의 분기별 검토가 포함돼야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-179">Cloud governance processes must include quarterly reviews with identity management teams.</span></span> <span data-ttu-id="7016c-180">이러한 검토를 통해 클라우드 자산 구성으로 방지해야 되는 악의적인 행위자 또는 사용 패턴을 쉽게 파악할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-180">These reviews can help identify malicious actors or usage patterns that should be prevented by cloud asset configuration.</span></span>

## <a name="evolution-of-the-best-practices"></a><span data-ttu-id="7016c-181">모범 사례 개선</span><span class="sxs-lookup"><span data-stu-id="7016c-181">Evolution of the best practices</span></span>

<span data-ttu-id="7016c-182">거버넌스 MVP 디자인을 개선하여 Azure Cost Management의 구현 및 새로운 Azure 정책을 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-182">The governance MVP design will evolve to include new Azure policies and an implementation of Azure Cost Management.</span></span> <span data-ttu-id="7016c-183">이러한 두 가지 디자인 변경을 통해 새 회사 정책 설명이 실현됩니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-183">Together, these two design changes will fulfill the new corporate policy statements.</span></span>

1. <span data-ttu-id="7016c-184">네트워킹 팀 및 IT 보안 팀에서는 네트워크 요구 사항을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-184">The Networking and IT Security teams will define network requirements.</span></span> <span data-ttu-id="7016c-185">클라우드 거버넌스 팀은 대화를 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-185">The Cloud Governance team will support the conversation.</span></span>
2. <span data-ttu-id="7016c-186">ID 팀 및 IT 보안 팀에서는 요구 사항을 정의하고 로컬 Active Directory 구현에 필요한 모든 변경을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-186">The Identity and IT Security teams will define identity requirements and make any necessary changes to local Active Directory implementation.</span></span> <span data-ttu-id="7016c-187">클라우드 거버넌스 팀에서는 변경 내용을 검토합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-187">The Cloud Governance team will review changes.</span></span>
3. <span data-ttu-id="7016c-188">모든 관련 Azure Resource Manager 템플릿과 스크립팅된 구성을 저장하고 버전을 지정하려면 Azure DevOps에서 리포지토리를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-188">Create a repository in Azure DevOps to store and version all relevant Azure Resource Manager templates and scripted configurations.</span></span>
4. <span data-ttu-id="7016c-189">Azure Security Center 구현:</span><span class="sxs-lookup"><span data-stu-id="7016c-189">Azure Security Center implementation:</span></span>
    1. <span data-ttu-id="7016c-190">보호된 데이터 분류를 포함하는 모든 관리 그룹용으로 Azure Security Center를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-190">Configure Azure Security Center for any management group that contains protected data classifications.</span></span>
    2. <span data-ttu-id="7016c-191">패치가 규정을 준수하도록 하려면 기본적으로 자동 프로비저닝을 사용으로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-191">Set automatic provisioning to on by default to ensure patching compliance.</span></span>
    3. <span data-ttu-id="7016c-192">OS 보안 구성을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-192">Establish OS security configurations.</span></span> <span data-ttu-id="7016c-193">IT 보안 팀에서는 구성을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-193">The IT Security team will define the configuration.</span></span>
    4. <span data-ttu-id="7016c-194">Security Center를 처음 사용할 경우 IT 보안 팀을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-194">Support the IT Security team in the initial use of Security Center.</span></span> <span data-ttu-id="7016c-195">Security Center를 IT 보안 팀으로 전환하여 사용한다고 해도 액세스를 유지하여 거버넌스를 지속적으로 개선합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-195">Transition the use of Security Center to the IT Security team, but maintain access for the purpose of continually improving governance.</span></span>
    5. <span data-ttu-id="7016c-196">구독 내의 Security Center 구성에 필요한 변경 내용을 반영하는 Resource Manager 템플릿을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-196">Create a Resource Manager template that reflects the changes required for Security Center configuration within a subscription.</span></span>
5. <span data-ttu-id="7016c-197">모든 구독에 대해 Azure 정책을 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-197">Update Azure policies for all subscriptions:</span></span>
    1. <span data-ttu-id="7016c-198">모든 관리 그룹과 구독에서 중요도 및 데이터 분류를 감사하고 적용하여 보호된 데이터 분류를 통해 모든 구독을 파악합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-198">Audit and enforce the criticality and data classification across all management groups and subscriptions, to identify any subscriptions with protected data classifications.</span></span>
    2. <span data-ttu-id="7016c-199">승인된 이미지 사용만 감사하고 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-199">Audit and enforce the use of approved images only.</span></span>
6. <span data-ttu-id="7016c-200">보호된 데이터 분류를 포함하는 모든 구독에 대해 Azure 정책을 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-200">Update Azure policies for all subscriptions that contains protected data classifications:</span></span>
    1. <span data-ttu-id="7016c-201">표준 Azure RBAC 역할의 사용만 감사하고 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-201">Audit and enforce the use of standard Azure RBAC roles only.</span></span>
    2. <span data-ttu-id="7016c-202">개별 노드에서 미사용 상태의 모든 스토리지 계정과 파일에 대해 암호화를 감사하고 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-202">Audit and enforce encryption for all storage accounts and files at rest on individual nodes.</span></span>
    3. <span data-ttu-id="7016c-203">모든 NIC 및 서브넷에 NSG의 적용을 감사하고 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-203">Audit and enforce the application of an NSG to all NICs and subnets.</span></span> <span data-ttu-id="7016c-204">네트워킹 팀 및 IT 보안 팀에서는 NSG를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-204">The Networking and IT Security teams will define the NSG.</span></span>
    4. <span data-ttu-id="7016c-205">네트워크 인터페이스별 승인된 네트워크 서브넷 및 vNet 사용을 감사하고 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-205">Audit and enforce the use of approved network subnet and vNet per network interface.</span></span>
    5. <span data-ttu-id="7016c-206">사용자 정의 라우팅 테이블의 제한을 감사하고 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-206">Audit and enforce the limitation of user-defined routing tables.</span></span>
    6. <span data-ttu-id="7016c-207">다음과 같이 게스트 구성에 대해 기본 제공 정책을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-207">Apply the Built-in Policies for Guest Configuration as follows:</span></span>
        1. <span data-ttu-id="7016c-208">Windows 웹 서버가 보안 통신 프로토콜을 사용 중인지 감사</span><span class="sxs-lookup"><span data-stu-id="7016c-208">Audit that Windows web servers are using secure communication protocols</span></span>
        2. <span data-ttu-id="7016c-209">암호 보안 설정이 Linux 및 Windows 머신 내부에 제대로 설정되었는지 감사</span><span class="sxs-lookup"><span data-stu-id="7016c-209">Audit that password security settings are set correctly inside Linux and Windows machines</span></span>
7. <span data-ttu-id="7016c-210">방화벽 구성:</span><span class="sxs-lookup"><span data-stu-id="7016c-210">Firewall configuration:</span></span>
    1. <span data-ttu-id="7016c-211">필요한 보안 요구 사항을 충족하는 Azure Firewall의 구성을 식별합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-211">Identify a configuration of Azure Firewall that meets necessary security requirements.</span></span> <span data-ttu-id="7016c-212">또는 Azure와 호환되는 호환 가능한 타사 어플라이언스를 식별합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-212">Alternatively, identify a compatible third-party appliance that is compatible with Azure.</span></span>
    2. <span data-ttu-id="7016c-213">Resource Manager 템플릿을 만들어 필요한 구성으로 방화벽을 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-213">Create a Resource Manager template to deploy the firewall with required configurations.</span></span>
8. <span data-ttu-id="7016c-214">Azure 청사진:</span><span class="sxs-lookup"><span data-stu-id="7016c-214">Azure blueprint:</span></span>
    1. <span data-ttu-id="7016c-215">`protected-data`라는 새 청사진을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-215">Create a new blueprint named `protected-data`.</span></span>
    2. <span data-ttu-id="7016c-216">청사진에 방화벽 및 Azure Security Center 템플릿을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-216">Add the firewall and Azure Security Center templates to the blueprint.</span></span>
    3. <span data-ttu-id="7016c-217">보호된 데이터 구독용으로 새 정책을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-217">Add the new policies for protected data subscriptions.</span></span>
    4. <span data-ttu-id="7016c-218">현재 보호된 데이터를 호스트하려는 모든 관리 그룹에 청사진을 게시합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-218">Publish the blueprint to any management group which current plans on hosting protected data.</span></span>
    5. <span data-ttu-id="7016c-219">영향을 받는 각 구독에 기존 청사진 외에 새 청사진을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-219">Apply the new blueprint to each affected subscription, in addition to existing blueprints.</span></span>

## <a name="conclusion"></a><span data-ttu-id="7016c-220">결론</span><span class="sxs-lookup"><span data-stu-id="7016c-220">Conclusion</span></span>

<span data-ttu-id="7016c-221">위에 언급된 프로세스와 변경 내용을 거버넌스 MVP에 추가하면 보안 거버넌스와 연관된 대부분의 위험을 완화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-221">Adding the above processes and changes to the governance MVP will help to mitigate many of the risks associated with security governance.</span></span> <span data-ttu-id="7016c-222">이러한 프로세스와 변경 내용은 데이터를 보호하는 데 필요한 네트워크, ID 및 보안 모니터링 도구를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-222">Together, they add the network, identity, and security monitoring tools needed to protect data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7016c-223">다음 단계</span><span class="sxs-lookup"><span data-stu-id="7016c-223">Next Steps</span></span>

<span data-ttu-id="7016c-224">클라우드 도입 과정이 계속 개선되어 비즈니스 가치를 추가적으로 제공하므로 위험 및 클라우드 거버넌스 요구도 함께 발전합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-224">As cloud adoption continues to evolve and deliver additional business value, risks and cloud governance needs also evolve.</span></span> <span data-ttu-id="7016c-225">이 경험을 진행하는 가상의 기업이 다음으로 수행하는 단계는 중요 업무용 워크로드를 지원하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-225">For the fictional company in this journey, the next step is to support mission-critical workloads.</span></span> <span data-ttu-id="7016c-226">이 시점에서 리소스 일관성 컨트롤이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="7016c-226">This is the point when Resource Consistency controls are needed.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="7016c-227">리소스 일관성 개선</span><span class="sxs-lookup"><span data-stu-id="7016c-227">Resource Consistency evolution</span></span>](./resource-consistency-evolution.md)