---
title: 'CAF: 중소기업 거버넌스 과정'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 중소기업 거버넌스 과정
author: BrianBlanchard
ms.openlocfilehash: a3e078845038a12977e7be5affbf22708411069f
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58245164"
---
# <a name="small-to-medium-enterprise-governance-journey"></a><span data-ttu-id="ca6ad-103">중소기업 거버넌스 과정</span><span class="sxs-lookup"><span data-stu-id="ca6ad-103">Small-to-medium enterprise governance journey</span></span>

## <a name="best-practice-overview"></a><span data-ttu-id="ca6ad-104">모범 사례 개요</span><span class="sxs-lookup"><span data-stu-id="ca6ad-104">Best practice overview</span></span>

<span data-ttu-id="ca6ad-105">이 거버넌스 과정은 거버넌스 성숙도의 여러 단계를 거치면서 가상 회사의 경험을 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-105">This governance journey follows the experiences of a fictional company through various stages of governance maturity.</span></span> <span data-ttu-id="ca6ad-106">실제 고객 경험을 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-106">It is based on real customer journeys.</span></span> <span data-ttu-id="ca6ad-107">제안된 모범 사례는 가상의 회사의 제약 조건 및 요구를 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-107">The suggested best practices are based on the constraints and needs of the fictional company.</span></span>

<span data-ttu-id="ca6ad-108">빠른 시작 지점인, 이 개요에서는 모범 사례를 기반으로 거버넌스에 대한 MVP(최소 실행 가능 제품)를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-108">As a quick starting point, this overview defines a minimum viable product (MVP) for governance based on best practices.</span></span> <span data-ttu-id="ca6ad-109">또한 새로운 비즈니스 또는 기술 위험이 드러나면서 모범 사례를 추가하는 거버넌스 개선에 대한 링크도 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-109">It also provides links to some governance evolutions that add further best practices as new business or technical risks emerge.</span></span>

> [!WARNING]
> <span data-ttu-id="ca6ad-110">이러한 MVP는 일련의 가정 세트를 기반으로 하는 기본 시작 지점입니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-110">This MVP is a baseline starting point, based on a set of assumptions.</span></span> <span data-ttu-id="ca6ad-111">최소한의 모범 사례도 위험 허용 오차 및 고유의 비즈니스 위험으로 인한 기업 정책을 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-111">Even this minimal set of best practices is based on corporate policies driven by unique business risks and risk tolerances.</span></span> <span data-ttu-id="ca6ad-112">이러한 가정이 적용 가능한지 확인하려면 이 문서에 나오는 [긴 이야기](./narrative.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-112">To see if these assumptions apply to you, read the [longer narrative](./narrative.md) that follows this article.</span></span>

## <a name="governance-best-practice"></a><span data-ttu-id="ca6ad-113">거버넌스 모범 사례</span><span class="sxs-lookup"><span data-stu-id="ca6ad-113">Governance best practice</span></span>

<span data-ttu-id="ca6ad-114">이 모범 사례는 조직이 여러 Azure 구독에 거버넌스 보호책을 빠르고 일관성 있게 추가하는 데 사용할 수 있는 토대의 역할을 합니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-114">This best practice serves as a foundation that an organization can use to quickly and consistently add governance guardrails across multiple Azure subscriptions.</span></span>

### <a name="resource-organization"></a><span data-ttu-id="ca6ad-115">리소스 조직</span><span class="sxs-lookup"><span data-stu-id="ca6ad-115">Resource organization</span></span>

<span data-ttu-id="ca6ad-116">다음 다이어그램은 조직 리소스에 대한 거버넌스 MVP 계층 구조를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-116">The following diagram shows the governance MVP hierarchy for organizing resources.</span></span>

![리소스 조직 다이어그램](../../../_images/governance/resource-organization.png)

<span data-ttu-id="ca6ad-118">모든 애플리케이션은 관리 그룹, 구독 및 리소스 그룹 계층 구조의 적절한 영역에 배포되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-118">Every application should be deployed in the proper area of the management group, subscription, and resource group hierarchy.</span></span> <span data-ttu-id="ca6ad-119">배포를 계획하는 동안 클라우드 거버넌스 팀은 클라우드 도입 팀의 역량을 강화하기 위해 계층 구조에 필요한 노드를 만들게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-119">During deployment planning, the Cloud Governance team will create the necessary nodes in the hierarchy to empower the cloud adoption teams.</span></span>  

1. <span data-ttu-id="ca6ad-120">환경 유형별 관리 그룹입니다(예: 프로덕션, 개발 및 테스트).</span><span class="sxs-lookup"><span data-stu-id="ca6ad-120">A management group for each type of environment (such as Production, Development, and Test).</span></span>
2. <span data-ttu-id="ca6ad-121">각 "애플리케이션 분류"에 대한 구독입니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-121">A subscription for each "application categorization".</span></span>
3. <span data-ttu-id="ca6ad-122">각 애플리케이션에 대한 별도의 리소스 그룹입니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-122">A separate resource group for each application.</span></span>
4. <span data-ttu-id="ca6ad-123">그룹화 계층 구조의 각 수준에는 일관된 명명법이 적용되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-123">Consistent nomenclature should be applied at each level of this grouping hierarchy.</span></span>

<span data-ttu-id="ca6ad-124">다음은 사용 중인 패턴의 예입니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-124">Here is an example of this pattern in use:</span></span>

![미드마켓 회사 리소스 조직의 예](../../../_images/governance/mid-market-resource-organization.png)

<span data-ttu-id="ca6ad-126">이러한 패턴은 계층 구조를 쓸데없이 복잡하게 만들지 않으면서 성장의 여지를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-126">These patterns provide room for growth without complicating the hierarchy unnecessarily.</span></span>

[!INCLUDE [governance-of-resources](../../../../../includes/cloud-adoption/governance/governance-of-resources.md)]

## <a name="governance-evolutions"></a><span data-ttu-id="ca6ad-127">거버넌스 개선</span><span class="sxs-lookup"><span data-stu-id="ca6ad-127">Governance evolutions</span></span>

<span data-ttu-id="ca6ad-128">이 MVP가 배포된 후 추가적인 거버넌스 계층을 환경에 신속하게 통합할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-128">Once this MVP has been deployed, additional layers of governance can be quickly incorporated into the environment.</span></span> <span data-ttu-id="ca6ad-129">다음은 특정 비즈니스 요구 사항에 맞게 MVP를 개선하는 몇 가지 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-129">Here are some ways to evolve the MVP to meet specific business needs:</span></span>

- [<span data-ttu-id="ca6ad-130">보호된 데이터에 대한 보안 기준</span><span class="sxs-lookup"><span data-stu-id="ca6ad-130">Security Baseline for protected data</span></span>](./security-baseline-evolution.md)
- [<span data-ttu-id="ca6ad-131">중요 업무용 애플리케이션에 대한 리소스 구성</span><span class="sxs-lookup"><span data-stu-id="ca6ad-131">Resource configurations for mission-critical applications</span></span>](./resource-consistency-evolution.md)
- [<span data-ttu-id="ca6ad-132">Cost Management에 대한 제어</span><span class="sxs-lookup"><span data-stu-id="ca6ad-132">Controls for Cost Management</span></span>](./cost-management-evolution.md)
- [<span data-ttu-id="ca6ad-133">다중 클라우드 개선을 위한 제어</span><span class="sxs-lookup"><span data-stu-id="ca6ad-133">Controls for multi-cloud evolution</span></span>](./multi-cloud-evolution.md)

<!-- markdownlint-disable MD026 -->

## <a name="what-does-this-best-practice-do"></a><span data-ttu-id="ca6ad-134">이 모범 사례로 무엇을 할 수 있습니까?</span><span class="sxs-lookup"><span data-stu-id="ca6ad-134">What does this best practice do?</span></span>

<span data-ttu-id="ca6ad-135">MVP에는, 기업 정책을 신속히 적용할 수 있도록 [배포 가속화](../../deployment-acceleration/overview.md) 분야의 사례와 도구가 설정되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-135">In the MVP, practices and tools from the [Deployment Acceleration](../../deployment-acceleration/overview.md) discipline are established to quickly apply corporate policy.</span></span> <span data-ttu-id="ca6ad-136">특히, MVP는 가상의 회사에 대한 이야기에 정의된 대로 Azure Blueprints, Azure Policy 및 Azure 관리 그룹을 사용하여 몇 가지 기본적인 회사 정책을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-136">In particular, the MVP uses Azure Blueprints, Azure Policy, and Azure management groups to apply a few basic corporate policies, as defined in the narrative for this fictional company.</span></span> <span data-ttu-id="ca6ad-137">이러한 회사 정책은 ID와 보안에 대한 매우 작은 기준을 설정하기 위해 Resource Manager 템플릿과 Azure 정책을 사용하여 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-137">Those corporate policies are applied using Resource Manager templates and Azure policies to establish a very small baseline for identity and security.</span></span>

![증분 거버넌스 MVP의 예](../../../_images/governance/governance-mvp.png)

## <a name="evolving-the-best-practice"></a><span data-ttu-id="ca6ad-139">발전하는 모범 사례</span><span class="sxs-lookup"><span data-stu-id="ca6ad-139">Evolving the best practice</span></span>

<span data-ttu-id="ca6ad-140">시간이 지나면서, 거버넌스 MVP는 거버넌스 사례를 발전시키는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-140">Over time, this governance MVP will be used to evolve the governance practices.</span></span> <span data-ttu-id="ca6ad-141">도입이 진행되면서 비즈니스 위험도 증가합니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-141">As adoption advances, business risk grows.</span></span> <span data-ttu-id="ca6ad-142">이러한 위험을 완화하기 위해 CAF 거버넌스 모델 내에서 다양한 분야가 개선됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-142">Various disciplines within the CAF governance model will evolve to mitigate those risks.</span></span> <span data-ttu-id="ca6ad-143">이 시리즈의 뒷부분에 나오는 문서에서는 가상의 회사에 영향을 주는 회사 정책의 발전에 대해 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-143">Later articles in this series discuss the evolution of corporate policy affecting the fictional company.</span></span> <span data-ttu-id="ca6ad-144">이러한 발전은 세 가지 분야에서 진행됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-144">These evolutions happen across three disciplines:</span></span>

- <span data-ttu-id="ca6ad-145">Cost Management는 도입 규모 변화와 관련됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-145">Cost Management, as adoption scales.</span></span>
- <span data-ttu-id="ca6ad-146">보안 기준은 보호된 데이터 배포와 관련이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-146">Security Baseline, as protected data is deployed.</span></span>
- <span data-ttu-id="ca6ad-147">리소스 일관성은 IT 운영을 통해 중요 업무용 워크로드를 지원하는 것과 관련이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-147">Resource Consistency, as IT Operations begins supporting mission-critical workloads.</span></span>

![증분 거버넌스 MVP의 예](../../../_images/governance/governance-evolution.png)

## <a name="next-steps"></a><span data-ttu-id="ca6ad-149">다음 단계</span><span class="sxs-lookup"><span data-stu-id="ca6ad-149">Next steps</span></span>

<span data-ttu-id="ca6ad-150">거버넌스 MPV를 이해했고 따라야 할 거버넌스 개선에 대한 아이디어를 얻었으면, 추가적인 컨텍스트를 뒷받침하는 이야기를 읽어보십시오.</span><span class="sxs-lookup"><span data-stu-id="ca6ad-150">Now that you’re familiar with the governance MVP and have an idea of the governance evolutions to follow, read the supporting narrative for additional context.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ca6ad-151">뒷받침하는 이야기 읽어보기</span><span class="sxs-lookup"><span data-stu-id="ca6ad-151">Read the supporting narrative</span></span>](./narrative.md)
