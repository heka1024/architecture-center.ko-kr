---
title: 'CAF: 대규모 엔터프라이즈-다중 클라우드 발전'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 대규모 엔터프라이즈-다중 클라우드 발전
author: BrianBlanchard
ms.openlocfilehash: 62a2fdd6e340c96494354f4f0cf2f78ab572c251
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59641147"
---
# <a name="large-enterprise-multi-cloud-evolution"></a><span data-ttu-id="dad67-103">대기업: 다중 클라우드 진화</span><span class="sxs-lookup"><span data-stu-id="dad67-103">Large enterprise: Multi-cloud evolution</span></span>

## <a name="evolution-of-the-narrative"></a><span data-ttu-id="dad67-104">이야기 전개</span><span class="sxs-lookup"><span data-stu-id="dad67-104">Evolution of the narrative</span></span>

<span data-ttu-id="dad67-105">Microsoft는 고객들이 특정 용도로 여러 클라우드를 도입한다는 점을 잘 알고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-105">Microsoft recognizes that customers are adopting multiple clouds for specific purposes.</span></span> <span data-ttu-id="dad67-106">이 경험에서 소개하는 가상의 기업도 마찬가지입니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-106">The fictional company in this journey is no exception.</span></span> <span data-ttu-id="dad67-107">이 고객은 Azure 도입 경험을 진행하는 과정에서 우수한 사업 실적을 기록하여 기존 사업을 보완하는 소규모 기업을 인수하게 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-107">In parallel to the Azure adoption journey, the business success has led to the acquisition of a small, but complementary business.</span></span> <span data-ttu-id="dad67-108">이 기업은 다른 클라우드 공급자의 서비스를 통해 모든 IT 작업을 실행하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-108">That business is running all of their IT operations on a different cloud provider.</span></span>

<span data-ttu-id="dad67-109">이 문서에서는 새 조직 통합 시의 클라우드 변경 방식에 대해 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-109">This article describes how things change when integrating the new organization.</span></span> <span data-ttu-id="dad67-110">이 설명에서는 회사가 이 고객 경험에 요약된 각 거버넌스 개선 단계를 완료했다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-110">For purposes of the narrative, we assume this company has completed each of the governance evolutions outlined in this customer journey.</span></span>

### <a name="evolution-of-the-current-state"></a><span data-ttu-id="dad67-111">현재 상태의 전개</span><span class="sxs-lookup"><span data-stu-id="dad67-111">Evolution of the current state</span></span>

<span data-ttu-id="dad67-112">이 이야기의 앞 부분에서는 클라우드 지출이 회사의 고정 경상비 중 일부로 자리함에 따라 회사가 비용 관리 및 비용 모니터링을 구현하기 시작했습니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-112">In the previous phase of this narrative, the company had begun to implement cost controls and cost monitoring, as cloud spending becomes part of the company's regular operational expenses.</span></span>

<span data-ttu-id="dad67-113">이후로 거버넌스에 영향을 줄 수 있는 몇 가지 변화가 있었습니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-113">Since then, some things have changed that will affect governance:</span></span>

- <span data-ttu-id="dad67-114">Active Directory의 온-프레미스 인스턴스를 통해 ID를 제어합니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-114">Identity is controlled by an on-premises instance of Active Directory.</span></span> <span data-ttu-id="dad67-115">Azure Active Directory로의 복제를 통해 하이브리드 ID를 손쉽게 사용할 수 있게 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-115">Hybrid Identity is facilitated through replication to Azure Active Directory.</span></span>
- <span data-ttu-id="dad67-116">대개 Azure Monitor 및 관련 자동화 기능을 통해 IT 운영 또는 클라우드 운영을 관리합니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-116">IT Operations or Cloud Operations are largely managed by Azure Monitor and related automations.</span></span>
- <span data-ttu-id="dad67-117">Azure Vault 인스턴스를 통해 재해 복구/비즈니스 연속성을 제어합니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-117">Disaster Recovery / Business Continuity is controlled by Azure Vault instances.</span></span>
- <span data-ttu-id="dad67-118">Azure Security Center를 사용하여 보안 위반과 공격을 모니터링합니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-118">Azure Security Center is used to monitor security violations and attacks.</span></span>
- <span data-ttu-id="dad67-119">클라우드 거버넌스 모니터링에는 Azure Security Center 및 Azure Monitor가 둘 다 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-119">Azure Security Center and Azure Monitor are both used to monitor governance of the cloud.</span></span>
- <span data-ttu-id="dad67-120">Azure Blueprints, Azure Policy 및 관리 그룹을 사용하여 정책 준수를 자동화합니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-120">Azure Blueprints, Azure Policy, and management groups are used to automate compliance to policy.</span></span>

### <a name="evolution-of-the-future-state"></a><span data-ttu-id="dad67-121">향후 상태의 전개</span><span class="sxs-lookup"><span data-stu-id="dad67-121">Evolution of the future state</span></span>

<span data-ttu-id="dad67-122">개선 과정의 목표는 가능한 모든 영역에서 인수 대상 회사를 기존 운영 분야에 통합하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-122">The goal is to integrate the acquisition company into existing operations wherever possible.</span></span>

## <a name="evolution-of-tangible-risks"></a><span data-ttu-id="dad67-123">실질적인 위험의 전개</span><span class="sxs-lookup"><span data-stu-id="dad67-123">Evolution of tangible risks</span></span>

<span data-ttu-id="dad67-124">**기업 인수 비용**: 새 기업 인수를 통한 수익 창출 효과는 약 5년 이내에 나타날 것으로 전망됩니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-124">**Business Acquisition Cost**: Acquisition of the new business is slated to be profitable in approximately five years.</span></span> <span data-ttu-id="dad67-125">하지만 수익 창출 속도가 느려서 이사회에서는 인수 비용을 최대한 제어하고자 합니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-125">Because of the slow rate of return, the board wants to control acquisition costs, as much as possible.</span></span> <span data-ttu-id="dad67-126">이로 인해 비용 제어와 기술 통합이 상충할 위험이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-126">There is a risk of cost control and technical integration conflicting with one another.</span></span>

<span data-ttu-id="dad67-127">이러한 비즈니스 위험은 다음과 같은 몇 가지 기술적 위험으로 확장될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-127">This business risk can be expanded into a few technical risks</span></span>

- <span data-ttu-id="dad67-128">클라우드 완화가 추가적인 인수 비용을 초래할 위험이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-128">There is risk of cloud migration producing additional acquisition costs.</span></span>
- <span data-ttu-id="dad67-129">새로운 환경이 제대로 관리되지 않거나 정책 위반을 초래할 위험도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-129">There is also a risk of the new environment not being properly governed or resulting in policy violations.</span></span>

## <a name="evolution-of-the-policy-statements"></a><span data-ttu-id="dad67-130">정책 설명의 전개</span><span class="sxs-lookup"><span data-stu-id="dad67-130">Evolution of the policy statements</span></span>

<span data-ttu-id="dad67-131">정책을 다음과 같이 변경하면 새로운 위험을 완화하고 구현 과정을 안내할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-131">The following changes to policy will help mitigate the new risks and guide implementation.</span></span>

1. <span data-ttu-id="dad67-132">기존의 운영 관리 및 보안 모니터링 도구를 통해 보조 클라우드의 모든 자산을 모니터링해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-132">All assets in a secondary cloud must be monitored through existing operational management and security monitoring tools.</span></span>
2. <span data-ttu-id="dad67-133">모든 조직 구성 단위를 기존 ID 공급자에 통합해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-133">All organizational units must be integrated into the existing identity provider.</span></span>
3. <span data-ttu-id="dad67-134">기본 ID 공급자가 보조 클라우드의 자산에 대한 인증을 제어해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-134">The primary identity provider should govern authentication to assets in the secondary cloud.</span></span>

## <a name="evolution-of-the-best-practices"></a><span data-ttu-id="dad67-135">모범 사례의 전개</span><span class="sxs-lookup"><span data-stu-id="dad67-135">Evolution of the best practices</span></span>

<span data-ttu-id="dad67-136">이 문서 섹션에서는 Azure Cost Management 구현 및 새로운 Azure 정책을 포함하도록 거버넌스 MVP 디자인을 개선합니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-136">This section of the article will evolve the governance MVP design to include new Azure policies and an implementation of Azure Cost Management.</span></span> <span data-ttu-id="dad67-137">이 두 가지 디자인 변경을 통해 새로운 기업 정책 설명을 처리하게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-137">Together, these two design changes will fulfill the new corporate policy statements.</span></span>

1. <span data-ttu-id="dad67-138">네트워크 연결 - 네트워킹 및 IT 보안 실행, 거버넌스 지원</span><span class="sxs-lookup"><span data-stu-id="dad67-138">Connect the networks - Executed by Networking and IT Security, supported by governance</span></span>
    1. <span data-ttu-id="dad67-139">MPLS/임대 회선 공급자에서 새 클라우드로의 연결을 추가하면 네트워크가 통합됩니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-139">Adding a connection from the MPLS/Leased line provider to the new cloud will integrate networks.</span></span> <span data-ttu-id="dad67-140">라우팅 테이블과 방화벽 구성을 추가하면 환경 간의 액세스 및 트래픽을 제어할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-140">Adding routing tables and firewall configurations will control access and traffic between the environments.</span></span>
2. <span data-ttu-id="dad67-141">ID 공급자를 통합합니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-141">Consolidate Identity Providers.</span></span> <span data-ttu-id="dad67-142">보조 클라우드에서 호스트되는 워크로드에 따라 다양한 옵션을 통해 ID 공급자를 통합할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-142">Depending on the workloads being hosted in the secondary cloud, there are a variety of options to identity provider consolidation.</span></span> <span data-ttu-id="dad67-143">다음은 몇 가지 예제입니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-143">The following are a few examples:</span></span>
    1. <span data-ttu-id="dad67-144">OAuth 2를 사용하여 인증하는 애플리케이션의 경우에는 보조 클라우드의 Active Directory에 속한 사용자를 기존 Azure AD 테넌트로 복제하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-144">For applications that authenticate using OAuth 2, users in the Active Directory in the secondary cloud could simply be replicated to the existing Azure AD tenant.</span></span>
    2. <span data-ttu-id="dad67-145">다른 한편으로 두 온-프레미스 ID 공급자 간의 페더레이션을 통해 사용자가 새 Active Directory 도메인을 Azure에 복제할 수 있게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-145">On the other extreme, federation between the two on-premises identity providers, would allow users from the new Active Directory domains to be replicated to Azure.</span></span>
3. <span data-ttu-id="dad67-146">Azure Site Recovery에 자산 추가</span><span class="sxs-lookup"><span data-stu-id="dad67-146">Add assets to Azure Site Recovery</span></span>
    1. <span data-ttu-id="dad67-147">Azure Site Recovery는 원래 하이브리드/다중 클라우드 도구로 구축되었습니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-147">Azure Site Recovery was built as a hybrid/multi-cloud tool from the beginning.</span></span>
    2. <span data-ttu-id="dad67-148">온-프레미스 자산을 보호하는 데 사용한 것과 같은 Azure Site Recovery 프로세스를 통해 보조 클라우드의 가상 머신을 보호할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-148">Virtual machines in the secondary cloud might be able to be protected by the same Azure Site Recovery processes used to protect on-premises assets.</span></span>
4. <span data-ttu-id="dad67-149">Azure Cost Management에 자산 추가</span><span class="sxs-lookup"><span data-stu-id="dad67-149">Add assets to Azure Cost Management</span></span>
    1. <span data-ttu-id="dad67-150">Azure Cost Management는 원래 다중 클라우드 도구로 구축되었습니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-150">Azure Cost Management was built as a multi-cloud tool from the beginning.</span></span>
    2. <span data-ttu-id="dad67-151">일부 클라우드 공급자의 경우에는 보조 클라우드의 가상 머신이 Azure Cost Management와 호환될 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-151">Virtual machines in the secondary cloud might be compatible with Azure Cost Management for some cloud providers.</span></span> <span data-ttu-id="dad67-152">추가 비용이 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-152">Additional costs may apply.</span></span>
5. <span data-ttu-id="dad67-153">Azure Monitor에 자산 추가</span><span class="sxs-lookup"><span data-stu-id="dad67-153">Add assets to Azure Monitor</span></span>
    1. <span data-ttu-id="dad67-154">Azure Monitor는 원래 하이브리드 클라우드 도구로 구축되었습니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-154">Azure Monitor was built as a hybrid cloud tool from the beginning.</span></span>
    2. <span data-ttu-id="dad67-155">보조 클라우드의 가상 머신이 Azure Monitor 에이전트와 호환될 수도 있으며, 그러면 운영 모니터링을 위해 가상 머신을 Azure Monitor에 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-155">Virtual machines in the secondary cloud might be compatible with Azure Monitor agents, allowing them to be included in Azure Monitor for operational monitoring.</span></span>
6. <span data-ttu-id="dad67-156">거버넌스 적용 도구</span><span class="sxs-lookup"><span data-stu-id="dad67-156">Governance enforcement tools</span></span>
    1. <span data-ttu-id="dad67-157">거버넌스 적용 도구는 클라우드별로 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-157">Governance enforcement is cloud-specific.</span></span>
    2. <span data-ttu-id="dad67-158">거버넌스 과정에서 수립된 기업 정책은 그렇지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-158">The corporate policies established in the governance journey are not.</span></span> <span data-ttu-id="dad67-159">그러므로 구현은 클라우드별로 다를 수도 있지만 보조 공급자에도 정책 설명을 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-159">While the implementation may vary from cloud to cloud, the policy statements can be applied to the secondary provider.</span></span>

<span data-ttu-id="dad67-160">다중 클라우드 도입 범위가 확대되면 위에서 설명한 디자인 개선 과정이 계속 진행되어 디자인이 완성됩니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-160">As multi-cloud adoption grows, the design evolution above will continue to mature.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dad67-161">다음 단계</span><span class="sxs-lookup"><span data-stu-id="dad67-161">Next steps</span></span>

<span data-ttu-id="dad67-162">많은 대기업에는 다섯 가지 분야의 클라우드 거 버 넌 스 블 도입 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-162">In many large enterprises, the Five Disciplines of Cloud Governance can be blockers to adoption.</span></span> <span data-ttu-id="dad67-163">다음 기술 자료 문서에 클라우드에서 장기적인 성공을 확인 하려면 몇 가지 추가 생각에 거 버 넌 스 팀 스포츠와 같습니다.</span><span class="sxs-lookup"><span data-stu-id="dad67-163">The next article has some additional thoughts on making governance a team sport to help ensure long-term success in the cloud.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="dad67-164">거버넌스의 여러 계층</span><span class="sxs-lookup"><span data-stu-id="dad67-164">Multiple layers of governance</span></span>](./multiple-layers-of-governance.md)
