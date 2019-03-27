---
title: 'CAF: Cost Management 동기 부여 및 비즈니스 위험'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Cost Management 동기 부여 및 비즈니스 위험
author: BrianBlanchard
ms.openlocfilehash: b9bf31a796ea1a7530c6a668a231d74b9b765509
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58247474"
---
# <a name="cost-management-motivations-and-business-risks"></a><span data-ttu-id="d9279-103">Cost Management 동기 부여 및 비즈니스 위험</span><span class="sxs-lookup"><span data-stu-id="d9279-103">Cost Management motivations and business risks</span></span>

<span data-ttu-id="d9279-104">이 문서에서는 고객이 일반적으로 클라우드 거버넌스 전략 내에 Cost Management 분야를 도입하는 이유를 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="d9279-104">This article discusses the reasons that customers typically adopt a Cost Management discipline within a cloud governance strategy.</span></span> <span data-ttu-id="d9279-105">정책 설명이 필요한 비즈니스 위험의 몇 가지 예제도 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="d9279-105">It also provides a few examples of business risks that drive policy statements.</span></span>

<!-- markdownlint-disable MD026 -->

## <a name="is-cost-management-relevant"></a><span data-ttu-id="d9279-106">Cost Management와 관련이 있습니까?</span><span class="sxs-lookup"><span data-stu-id="d9279-106">Is Cost Management relevant?</span></span>

<span data-ttu-id="d9279-107">비용 관리의 관점에서 볼 때, 클라우드 도입은 패러다임 전환을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="d9279-107">In terms of cost governance, cloud adoption creates a paradigm shift.</span></span> <span data-ttu-id="d9279-108">기존의 온-프레미스 환경에서 비용 관리는 새로 고침 주기, 데이터 센터 구입, 호스트 갱신 및 반복되는 유지 관리 문제를 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="d9279-108">Management of cost in a traditional on-premises world is based on refresh cycles, datacenter acquisitions, host renewals, and recurring maintenance issues.</span></span> <span data-ttu-id="d9279-109">이러한 비용을 예측, 계획 및 세분화하여 연간 자본 지출 예산에 맞출 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d9279-109">You can forecast, plan, and refine each of these costs to align with annual capital expenditure budgets.</span></span>

<span data-ttu-id="d9279-110">클라우드 솔루션의 경우, 많은 기업이 Cost Management에 사후 대응적인 방식을 취하는 경향이 더 많습니다.</span><span class="sxs-lookup"><span data-stu-id="d9279-110">For cloud solutions, many businesses tend to take a more reactive approach to Cost Management.</span></span> <span data-ttu-id="d9279-111">많은 경우, 기업은 일정량의 클라우드 서비스를 미리 구매하거나 사용을 약속합니다.</span><span class="sxs-lookup"><span data-stu-id="d9279-111">In many cases, businesses will prepurchase, or commit to use, a set amount of cloud services.</span></span> <span data-ttu-id="d9279-112">이 모델은 기업이 특정 클라우드 공급 업체에 대한 지출을 계획하는 정도에 따라 할인이 극대화되면 사전 계획적인 비용 주기에 대한 인식이 생성된다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="d9279-112">This model assumes that maximizing discounts, based on how much the business plans on spending with a specific cloud vendor, creates the perception of a proactive, planned cost cycle.</span></span> <span data-ttu-id="d9279-113">단, 기업이 성숙한 Cost Management 분야도 구현해야만 이러한 인식이 현실화됩니다.</span><span class="sxs-lookup"><span data-stu-id="d9279-113">However, that perception will only become a reality if the business also implements mature Cost Management disciplines.</span></span>

<span data-ttu-id="d9279-114">클라우드는 기존의 온-프레미스 데이터 센터에서는 볼 수 없었던 셀프 서비스 기능을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="d9279-114">The cloud offers self-service capabilities that were previously unheard of in traditional on-premises datacenters.</span></span> <span data-ttu-id="d9279-115">이러한 새로운 기능을 통해 기업은 보다 민첩하고 제한이 덜하며 새로운 기술을 채택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d9279-115">These new capabilities empower businesses to be more agile, less restrictive, and more open to adopt new technologies.</span></span> <span data-ttu-id="d9279-116">단, 셀프 서비스에는 최종 사용자가 무의식적으로 할당된 예산을 초과할 수 있다는 단점이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d9279-116">However, the downside of self-service is that end users can unknowingly exceed allocated budgets.</span></span> <span data-ttu-id="d9279-117">반대로, 동일한 사용자가 계획 변경으로 인해 예기치 않게 예측된 양의 클라우드 서비스를 사용하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d9279-117">Conversely, the same users can experience a change in plans and unexpectedly not use the amount of cloud services forecasted.</span></span> <span data-ttu-id="d9279-118">어느 방향이든 이러한 변화의 가능성은 거버넌스 팀 내의 Cost Management 분야에 대한 투자를 정당화합니다.</span><span class="sxs-lookup"><span data-stu-id="d9279-118">The potential of shift in either direction justifies investment in a Cost Management discipline within the governance team.</span></span>

## <a name="business-risk"></a><span data-ttu-id="d9279-119">비즈니스 위험</span><span class="sxs-lookup"><span data-stu-id="d9279-119">Business risk</span></span>

<span data-ttu-id="d9279-120">Cost Management 분야에서는 클라우드 기반 워크로드를 호스트할 때 발생하는 비용과 관련된 주요 비즈니스 위험을 해결하고자 합니다.</span><span class="sxs-lookup"><span data-stu-id="d9279-120">The Cost Management discipline attempts to address core business risks related to expenses incurred when hosting cloud-based workloads.</span></span> <span data-ttu-id="d9279-121">클라우드 배포를 계획하고 구현할 때 회사와 협력하여 이러한 위험을 식별하고 관련성을 모니터링하세요.</span><span class="sxs-lookup"><span data-stu-id="d9279-121">Work with your business to identify these risks and monitor each of them for relevance as you plan for and implement your cloud deployments.</span></span>

<span data-ttu-id="d9279-122">위험은 조직마다 다르지만, 다음 항목은 클라우드 거버넌스 팀 내에서 토론의 시작점으로 사용할 수 있는 일반적인 비용 관련 위험 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="d9279-122">Risks will differ between organization, but the following serve as common cost-related risks that you can use as a starting point for discussions within your Cloud Governance team:</span></span>

- <span data-ttu-id="d9279-123">**예산 제어**.</span><span class="sxs-lookup"><span data-stu-id="d9279-123">**Budget control**.</span></span> <span data-ttu-id="d9279-124">예산을 제어하지 않으면 클라우드 공급 업체에 과도한 지출이 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d9279-124">Not controlling budget can lead to excessive spending with a cloud vendor.</span></span>
- <span data-ttu-id="d9279-125">**사용률 손실**.</span><span class="sxs-lookup"><span data-stu-id="d9279-125">**Utilization loss**.</span></span> <span data-ttu-id="d9279-126">사전 구매나 사전 위탁이 사용되지 않으면 투자가 낭비될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d9279-126">Prepurchases or precommitments that are not used can result in wasted investments.</span></span>
- <span data-ttu-id="d9279-127">**이례적인 지출**: 어느 쪽이든 예상치 못한 급등이 발생하면 부적절한 사용을 나타내는 지표가 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d9279-127">**Spending anomalies**: Unexpected spikes in either direction can be indicators of improper usage.</span></span>
- <span data-ttu-id="d9279-128">**오버프로비저닝된 자산**.</span><span class="sxs-lookup"><span data-stu-id="d9279-128">**Overprovisioned assets**.</span></span> <span data-ttu-id="d9279-129">애플리케이션이나 VM(가상 머신)의 수요를 초과하는 구성으로 자산을 배포하면 비용이 증가하고 낭비가 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d9279-129">When assets are deployed in a configuration that exceed the needs of an application or virtual machine (VM), they can increase costs and create waste.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d9279-130">다음 단계</span><span class="sxs-lookup"><span data-stu-id="d9279-130">Next steps</span></span>

<span data-ttu-id="d9279-131">[클라우드 관리 템플릿](./template.md)을 사용하여 현재 클라우드 도입 계획으로 인해 발생할 가능성이 있는 비즈니스 위험을 문서화합니다.</span><span class="sxs-lookup"><span data-stu-id="d9279-131">Using the [Cloud Management Template](./template.md), document business risks that are likely to be introduced by the current cloud adoption plan.</span></span>

<span data-ttu-id="d9279-132">현실적인 비즈니스 위험이 이해되었으면 다음 단계는 회사의 위험 허용 범위와 이러한 허용 범위를 모니터링할 지표와 주요 메트릭을 문서화하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="d9279-132">Once an understanding of realistic business risks is established, the next step is to document the business's tolerance for risk and the indicators and key metrics to monitor that tolerance.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d9279-133">지표, 메트릭 및 위험 허용 범위 이해</span><span class="sxs-lookup"><span data-stu-id="d9279-133">Understand indicators, metrics, and risk tolerance</span></span>](./metrics-tolerance.md)
