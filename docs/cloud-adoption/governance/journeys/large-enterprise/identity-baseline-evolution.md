---
title: 'CAF: 대기업 – ID 기준 개선'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 대기업 – ID 기준 개선
author: BrianBlanchard
ms.openlocfilehash: efda14819bfbc70632dc9bb8b4c6c5aca96004c0
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901575"
---
# <a name="large-enterprise-identity-baseline-evolution"></a><span data-ttu-id="91a30-103">대기업: ID 기준 진화</span><span class="sxs-lookup"><span data-stu-id="91a30-103">Large enterprise: Identity Baseline evolution</span></span>

<span data-ttu-id="91a30-104">이 문서에서는 거버넌스 MVP에 ID 기준 컨트롤을 추가하여 이야기를 전개합니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-104">This article evolves the narrative by adding Identity Baseline controls to the governance MVP.</span></span>

## <a name="evolution-of-the-narrative"></a><span data-ttu-id="91a30-105">설명 개선</span><span class="sxs-lookup"><span data-stu-id="91a30-105">Evolution of the narrative</span></span>

<span data-ttu-id="91a30-106">두 데이터 센터의 클라우드 마이그레이션에 대한 비즈니스 타당성을 CFO가 승인했습니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-106">The business justification for the cloud migration of the two datacenters was approved by the CFO.</span></span> <span data-ttu-id="91a30-107">기술적인 실행 가능성을 연구하는 과정에서 여러 장애물이 발견되었습니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-107">During the technical feasibility study, several roadblocks were discovered:</span></span>

- <span data-ttu-id="91a30-108">보호되는 데이터 및 중요 업무용 애플리케이션은 두 데이터 센터에서 25%의 워크로드를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-108">Protected data and mission-critical applications represent 25% of the workloads in the two datacenters.</span></span> <span data-ttu-id="91a30-109">PII 및 중요 업무용 애플리케이션과 관련된 현재 거버넌스 정책이 현대화될 때까지 어느 것도 제거할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-109">Neither can be eliminated until the current governance policies regarding PII and mission-critical applications have been modernized.</span></span>
- <span data-ttu-id="91a30-110">이러한 데이터 센터의 자산 중 7%가 클라우드와 호환되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-110">7% of the assets in those datacenters are not cloud-compatible.</span></span> <span data-ttu-id="91a30-111">자산은 데이터 센터 계약이 종료되기 전에 대체 데이터 센터로 이동될 예정입니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-111">They will be moved to an alternate datacenter before termination of the datacenter contract.</span></span>
- <span data-ttu-id="91a30-112">데이터 센터(750대의 가상 머신)의 자산 중 15%에는 레거시 인증 또는 타사 다단계 인증에 대한 종속성을 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-112">15% of the assets in the datacenter (750 virtual machines) have a dependency on legacy authentication or third-party multi-factor authentication.</span></span>
- <span data-ttu-id="91a30-113">기존 데이터 센터와 Azure를 연결하는 VPN 연결은 데이터 센터를 사용 중지하기 위한 2년의 타임라인 이내에 자산 볼륨을 마이그레이션할 만큼 충분한 데이터 전송 속도 또는 대기 시간을 제공하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-113">The VPN connection that connects existing datacenters and Azure does not offer sufficient data transmission speeds or latency to migrate the volume of assets within the two-year timeline to retire the datacenter.</span></span>

<span data-ttu-id="91a30-114">처음 두 장애물은 동시에 완화됩니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-114">The first two roadblocks are being mitigated in parallel.</span></span> <span data-ttu-id="91a30-115">이 문서에서는 세 번째 및 네 번째 장애물의 해결 방법에 대해 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-115">This article will address the resolution of the third and fourth roadblocks.</span></span>

### <a name="evolution-of-the-cloud-governance-team"></a><span data-ttu-id="91a30-116">클라우드 거버넌스 팀의 발전 상황</span><span class="sxs-lookup"><span data-stu-id="91a30-116">Evolution of the Cloud Governance team</span></span>

<span data-ttu-id="91a30-117">클라우드 거버넌스 팀은 확장되고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-117">The Cloud Governance team is expanding.</span></span> <span data-ttu-id="91a30-118">ID 관리와 관련된 추가 지원의 필요성을 감안할 때 ID 기준 팀의 시스템 관리자는 기존 팀원이 변동 내용을 알 수 있도록 주간 회의에 참여합니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-118">Given the need for additional support regarding identity management, a systems administrator from the Identity Baseline team now participates in a weekly meeting to keep the existing team members aware of changes.</span></span>

### <a name="evolution-of-the-current-state"></a><span data-ttu-id="91a30-119">현재 상태의 발전</span><span class="sxs-lookup"><span data-stu-id="91a30-119">Evolution of the current state</span></span>

<span data-ttu-id="91a30-120">IT 팀은 CIO 및 CFO에게서 두 데이터 센터 사용 중지 계획을 추진하도록 승인을 받았습니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-120">The IT team has approval to move forward with the CIO and CFO's plans to retire two datacenters.</span></span> <span data-ttu-id="91a30-121">그러나 IT 팀은 해당 데이터 센터의 자산 중 750(15%)대는 클라우드가 아닌 다른 환경으로 이동해야 한다는 점을 우려하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-121">However, IT is concerned that 750 (15%) of the assets in those datacenters will have to be moved somewhere other than the cloud.</span></span>

### <a name="evolution-of-the-future-state"></a><span data-ttu-id="91a30-122">향후 상태의 발전</span><span class="sxs-lookup"><span data-stu-id="91a30-122">Evolution of the future state</span></span>

<span data-ttu-id="91a30-123">새로운 향후 상태 계획에는 레거시 인증 요건이 있는 750대의 가상 머신을 마이그레이션하기 위한 보다 견고한 ID 기준 솔루션이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-123">The new future state plans require a more robust Identity Baseline solution to migrate the 750 virtual machines with legacy authentication requirements.</span></span> <span data-ttu-id="91a30-124">이 두 데이터 센터 외에도 다른 데이터 센터에서 유사한 비율의 자산이 이 문제의 영향을 받을 것으로 예상됩니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-124">Beyond these two datacenters, similar percentages of assets in other datacenters are expected to be affected by this challenge.</span></span>
<span data-ttu-id="91a30-125">향후 상태에는 클라우드 공급자로부터 회사의 MPLS/임대 회선 솔루션으로의 연결도 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-125">The future state now also requires a connection from the cloud provider to the company’s MPLS/leased-line solution.</span></span>

<span data-ttu-id="91a30-126">현재와 미래의 상태가 변경되면 새로운 정책 설명이 요구되는 새로운 위험에 노출됩니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-126">The changes to current and future state expose new risks that will require new policy statements.</span></span>

## <a name="evolution-of-tangible-risks"></a><span data-ttu-id="91a30-127">실질적인 위험의 심화</span><span class="sxs-lookup"><span data-stu-id="91a30-127">Evolution of tangible risks</span></span>

<span data-ttu-id="91a30-128">**마이그레이션 중 비즈니스 중단**.</span><span class="sxs-lookup"><span data-stu-id="91a30-128">**Business interruption during migration**.</span></span> <span data-ttu-id="91a30-129">클라우드로 마이그레이션하면 관리할 수 있는 시간 제한적인 위험이 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-129">Migration to the cloud creates a controlled, time-bound risk that can be managed.</span></span> <span data-ttu-id="91a30-130">노후된 하드웨어를 세계의 다른 곳으로 이동하는 것은 훨씬 더 위험합니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-130">Moving aging hardware to another part of the world is much higher risk.</span></span> <span data-ttu-id="91a30-131">비즈니스 운영이 중단되는 것을 피하려면 완화 전략이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-131">A mitigation strategy is needed to avoid interruptions to business operations.</span></span>

<span data-ttu-id="91a30-132">**기존 ID 종속성**.</span><span class="sxs-lookup"><span data-stu-id="91a30-132">**Existing identity dependencies**.</span></span> <span data-ttu-id="91a30-133">기존 인증 및 ID 서비스에 대한 종속성으로 인해 일부 워크로드의 클라우드로 마이그레이션이 지연되거나 진행되지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-133">Dependencies on existing authentication and identity services may delay or prevent the migration of some workloads to the cloud.</span></span> <span data-ttu-id="91a30-134">두 데이터 센터를 제 시간에 반환하지 않으면 수백만 달러의 데이터 센터 임대 비용이 부과됩니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-134">Failure to return the two datacenters on time will incur millions of dollars in datacenter lease fees.</span></span>

<span data-ttu-id="91a30-135">이 비즈니스 위험은 다음과 같은 몇 가지 기술적 위험으로 확장될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-135">This business risk can be expanded into a few technical risks:</span></span>

- <span data-ttu-id="91a30-136">클라우드에서는 레거시 인증을 사용할 수 없으므로 일부 애플리케이션의 배포가 제한될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-136">Legacy authentication might not be available in the cloud, limiting deployment of some applications.</span></span>
- <span data-ttu-id="91a30-137">클라우드에서는 현재 타사 MFA 솔루션을 사용할 수 없으므로 일부 애플리케이션의 배포가 제한될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-137">The current third-party MFA solution might not be available in the cloud, limiting deployment of some applications.</span></span>
- <span data-ttu-id="91a30-138">도구를 변경하거나 이동하면 중단이 발생하고 비용이 추가될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-138">Retooling or moving either could create outages and add costs.</span></span>
- <span data-ttu-id="91a30-139">VPN의 속도와 안정성이 마이그레이션을 방해할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-139">The speed and stability of the VPN might impede migration.</span></span>
- <span data-ttu-id="91a30-140">클라우드로 들어오는 트래픽이 글로벌 네트워크의 다른 부분에서 보안 문제를 일으킬 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-140">Traffic coming into the cloud could cause security issues in other parts of the global network.</span></span>

## <a name="evolution-of-the-policy-statements"></a><span data-ttu-id="91a30-141">정책 설명 개선</span><span class="sxs-lookup"><span data-stu-id="91a30-141">Evolution of the policy statements</span></span>

<span data-ttu-id="91a30-142">정책을 다음과 같이 변경하면 새로운 위험을 완화하고 구현 과정을 안내할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-142">The following changes to policy will help mitigate the new risks and guide implementation.</span></span>

1. <span data-ttu-id="91a30-143">선택한 클라우드 공급자는 레거시 방법을 통한 인증 수단을 제공해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-143">The chosen cloud provider must offer a means of authenticating via legacy methods.</span></span>
2. <span data-ttu-id="91a30-144">선택한 클라우드 공급자는 현재 타사 MFA 솔루션을 이용한 인증 수단을 제공해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-144">The chosen cloud provider must offer a means of authentication with the current third-party MFA solution.</span></span>
3. <span data-ttu-id="91a30-145">클라우드 공급자와 회사의 통신 공급자 간에 고속 비공개 연결을 설정하여 클라우드 공급자를 데이터 센터의 글로벌 네트워크에 연결해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-145">A high-speed private connection should be established between the cloud provider and the company’s telco provider, connecting the cloud provider to the global network of datacenters.</span></span>
4. <span data-ttu-id="91a30-146">충분한 보안 요구 사항이 설정될 때까지 인바운드 공용 트래픽은 클라우드에 호스트되는 회사 자산에 액세스할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-146">Until sufficient security requirements are established, no inbound public traffic may access company assets hosted in the cloud.</span></span> <span data-ttu-id="91a30-147">글로벌 WAN 외부의 소스로부터 발생하는 모든 포트는 차단됩니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-147">All ports are blocked from any source outside of the global WAN.</span></span>

## <a name="evolution-of-the-best-practices"></a><span data-ttu-id="91a30-148">모범 사례 개선</span><span class="sxs-lookup"><span data-stu-id="91a30-148">Evolution of the best practices</span></span>

<span data-ttu-id="91a30-149">거버넌스 MVP 디자인을 개선하여 가상 머신에서 Active Directory의 구현 및 새로운 Azure 정책을 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-149">The governance MVP design evolves to include new Azure policies and an implementation of Active Directory on a virtual machine.</span></span> <span data-ttu-id="91a30-150">이 두 가지 디자인 변경을 통해 새 기업 정책 설명을 충족하게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-150">Together, these two design changes fulfill the new corporate policy statements.</span></span>

<span data-ttu-id="91a30-151">새 모범 사례는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-151">Here are the new best practices:</span></span>

1. <span data-ttu-id="91a30-152">DMZ 청사진: DMZ의 온-프레미스 쪽은 다음 솔루션과 온-프레미스 Active Directory 서버 간의 통신을 허용하도록 구성되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-152">DMZ blueprint: The on-premises side of the DMZ should be configured to allow communication between the following solution and the on-premises Active Directory servers.</span></span> <span data-ttu-id="91a30-153">이 모범 사례는 DMZ에서 네트워크 경계를 넘는 Active Directory Domain Services를 사용하도록 요구합니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-153">This best practice requires a DMZ to enable Active Directory Domain Services across network boundaries.</span></span>
2. <span data-ttu-id="91a30-154">Azure Resource Manager 템플릿:</span><span class="sxs-lookup"><span data-stu-id="91a30-154">Azure Resource Manager templates:</span></span>
    1. <span data-ttu-id="91a30-155">외부 트래픽을 차단하고 내부 트래픽을 허용하도록 NSG를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-155">Define an NSG to block external traffic and whitelist internal traffic.</span></span>
    1. <span data-ttu-id="91a30-156">두 개의 AD 가상 머신을 골든 이미지 기반의 부하 분산된 쌍으로 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-156">Deploy two AD virtual machines in a load balanced pair based on a golden image.</span></span> <span data-ttu-id="91a30-157">처음 부팅 시 해당 이미지는 PowerShell 스크립트를 실행하여 도메인에 가입하고 도메인 서비스에 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-157">On first boot, that image runs a PowerShell script to join the domain and register with domain services.</span></span> <span data-ttu-id="91a30-158">자세한 내용은 [AD DS(Active Directory Domain Services)를 Azure로 확장](../../../../reference-architectures/identity/adds-extend-domain.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="91a30-158">For more information, see [Extend Active Directory Domain Services (AD DS) to Azure](../../../../reference-architectures/identity/adds-extend-domain.md).</span></span>
3. <span data-ttu-id="91a30-159">Azure Policy: 모든 리소스에 NSG를 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-159">Azure Policy: Apply the NSG to all resources.</span></span>
4. <span data-ttu-id="91a30-160">Azure blueprint</span><span class="sxs-lookup"><span data-stu-id="91a30-160">Azure blueprint</span></span>
    1. <span data-ttu-id="91a30-161">`active-directory-virtual-machines`라는 청사진을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-161">Create a blueprint named `active-directory-virtual-machines`.</span></span>
    1. <span data-ttu-id="91a30-162">청사진에 각 AD 템플릿 및 정책을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-162">Add each of the AD templates and policies to the blueprint.</span></span>
    1. <span data-ttu-id="91a30-163">해당 관리 그룹에 청사진을 게시합니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-163">Publish the blueprint to any applicable management group.</span></span>
    1. <span data-ttu-id="91a30-164">레거시 또는 타사 MFA 인증을 요구하는 모든 구독에 청사진을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-164">Apply the blueprint to any subscription requiring legacy or third-party MFA authentication.</span></span>
    1. <span data-ttu-id="91a30-165">이제 Azure에서 실행되는 AD 인스턴스를 온-프레미스 AD 솔루션의 확장으로 사용할 수 있으므로 기존 Active Directory 기능을 통해 기존 MFA 도구와 통합하고 클레임 기반 인증을 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-165">The instance of AD running in Azure can now be used as an extension of the on-premises AD solution, allowing it to integrate with the existing MFA tool and provide claims-based authentication, both through existing Active Directory functionality.</span></span>

## <a name="conclusion"></a><span data-ttu-id="91a30-166">결론</span><span class="sxs-lookup"><span data-stu-id="91a30-166">Conclusion</span></span>

<span data-ttu-id="91a30-167">이러한 변경 사항을 거버넌스 MVP에 추가하면 이 문서에 나오는 대부분의 위험을 완화할 수 있으며 각 클라우드 도입 팀은 이 장애물을 신속하게 통과할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-167">Adding these changes to the governance MVP helps mitigate many of the risks in this article, allowing each cloud adoption team to quickly move past this roadblock.</span></span>

## <a name="next-steps"></a><span data-ttu-id="91a30-168">다음 단계</span><span class="sxs-lookup"><span data-stu-id="91a30-168">Next steps</span></span>

<span data-ttu-id="91a30-169">클라우드 채택 과정이 개선되어 비즈니스 가치를 추가적으로 제공하는 과정에서 위험 및 클라우드 거버넌스 요구도 발전합니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-169">As cloud adoption evolves and delivers additional business value, risks and cloud governance needs will also evolve.</span></span> <span data-ttu-id="91a30-170">다음은 발생할 수 있는 몇 가지 변화입니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-170">The following are a few evolutions that may occur.</span></span> <span data-ttu-id="91a30-171">이 과정에 있는 가상의 회사에서 다음 트리거는 클라우드 도입 계획에 보호되는 데이터를 포함시키는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-171">For the fictional company in this journey, the next trigger is the inclusion of protected data in the cloud adoption plan.</span></span> <span data-ttu-id="91a30-172">이 변경에는 추가 보안 컨트롤이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="91a30-172">This change will require additional security controls.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="91a30-173">보안 기준 개선</span><span class="sxs-lookup"><span data-stu-id="91a30-173">Security Baseline evolution</span></span>](./security-baseline-evolution.md)
