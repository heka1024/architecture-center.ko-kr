---
title: 'CAF: 리소스 일관성 동기 및 비즈니스 위험'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 리소스 일관성 동기 및 비즈니스 위험
author: alexbuckgit
ms.openlocfilehash: 19e0d761e4afa3473099bde2edc960c8b9eadb79
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58242464"
---
# <a name="resource-consistency-motivations-and-business-risks"></a><span data-ttu-id="ca5ab-103">리소스 일관성 동기 및 비즈니스 위험</span><span class="sxs-lookup"><span data-stu-id="ca5ab-103">Resource Consistency motivations and business risks</span></span>

<span data-ttu-id="ca5ab-104">이 문서에서는 고객이 일반적으로 클라우드 거버넌스 전략 내에서 리소스 일관성 분야를 채택하는 이유에 대해 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="ca5ab-104">This article discusses the reasons that customers typically adopt a Resource Consistency discipline within a cloud governance strategy.</span></span> <span data-ttu-id="ca5ab-105">또한 정책 설명을 추진할 수 있는 잠재적인 비즈니스 위험의 몇 가지 예도 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="ca5ab-105">It also provides a few examples of potential business risks that can drive policy statements.</span></span>

<!-- markdownlint-disable MD026 -->

## <a name="is-resource-consistency-relevant"></a><span data-ttu-id="ca5ab-106">리소스 일관성과 관련이 있나요?</span><span class="sxs-lookup"><span data-stu-id="ca5ab-106">Is Resource Consistency relevant?</span></span>

<span data-ttu-id="ca5ab-107">리소스와 워크로드를 배포하는 경우 클라우드는 대부분의 기존 온-프레미스 데이터 센터에 비해 향상된 민첩성과 유연성을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="ca5ab-107">When it comes to deploying resources and workloads, the cloud offers increased agility and flexibility over most traditional on-premises datacenters.</span></span> <span data-ttu-id="ca5ab-108">그러나 이러한 잠재적인 클라우드 기반 이점은 클라우드 채택의 성공을 심각하게 위협할 수 있는 잠재적인 관리 단점도 함께 수반하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca5ab-108">However, these potential cloud-based advantages also come paired with potential management drawbacks that can seriously jeopardize the success of your cloud adoption.</span></span> <span data-ttu-id="ca5ab-109">어떤 자산을 배포했습니까?</span><span class="sxs-lookup"><span data-stu-id="ca5ab-109">What assets have you deployed?</span></span> <span data-ttu-id="ca5ab-110">어떤 팀에서 어떤 자산을 소유합니까?</span><span class="sxs-lookup"><span data-stu-id="ca5ab-110">What teams own what assets?</span></span> <span data-ttu-id="ca5ab-111">워크로드를 지원하기 위한 충분한 리소스가 있습니까?</span><span class="sxs-lookup"><span data-stu-id="ca5ab-111">Do you have enough resources supporting a workload?</span></span> <span data-ttu-id="ca5ab-112">워크로드가 정상인지 어떻게 알 수 있습니까?</span><span class="sxs-lookup"><span data-stu-id="ca5ab-112">How do you know if workloads are healthy?</span></span>

<span data-ttu-id="ca5ab-113">리소스 일관성은 리소스를 일관되게 반복적으로 배포, 업데이트 및 구성하고, 서비스 중단을 최소화하고, 최대한 짧은 시간 내에 문제를 해결하도록 하는 데 매우 중요합니다.</span><span class="sxs-lookup"><span data-stu-id="ca5ab-113">Resource Consistency is crucial to ensure that resources are deployed, updated, and configured consistently and repeatably, and that service disruptions are minimized and remedied in as little time as possible.</span></span>

<span data-ttu-id="ca5ab-114">리소스 일관성 분야는 클라우드 배포의 운영 측면과 관련된 비즈니스 위험을 식별하고 완화하는 것과 관련이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca5ab-114">The Resource Consistency discipline is concerned with identifying and mitigating business risks related to the operational aspects of your cloud deployment.</span></span> <span data-ttu-id="ca5ab-115">리소스 일관성에는 애플리케이션, 워크로드 및 자산 성능에 대한 모니터링이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca5ab-115">Resource Consistency includes monitoring of applications, workloads, and asset performance.</span></span> <span data-ttu-id="ca5ab-116">또한 크기 조정 요구 사항을 충족시키고, 성능 SLA(서비스 수준 계약) 위반을 수정하며, 자동화된 업데이트 관리를 통해 성능 SLA 위반을 사전에 방지하는 데 필요한 작업도 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca5ab-116">It also includes the tasks required to meet scale demands, remediate performance Service Level Agreement (SLA) violations, and proactively avoid performance SLA violations through automated remediation.</span></span>

<span data-ttu-id="ca5ab-117">초기 테스트 배포에는 리소스 일관성 요구 사항을 지원하기 위해 몇 가지 형식적인 명명 및 태그 지정 표준을 채택하는 것 이외의 많은 작업이 필요하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca5ab-117">Initial test deployments may not require much beyond adopting some cursory naming and tagging standards to support your Resource Consistency needs.</span></span> <span data-ttu-id="ca5ab-118">클라우드 채택의 완성도가 높아지고 더 복잡한 중요 업무용 자산을 배포함에 따라 리소스 일관성 분야에 대한 투자의 필요성이 빠르게 증가합니다.</span><span class="sxs-lookup"><span data-stu-id="ca5ab-118">As your cloud adoption matures and you deploy more complicated and mission-critical assets, the need to invest in the Resource Consistency discipline increases rapidly.</span></span>

## <a name="business-risk"></a><span data-ttu-id="ca5ab-119">비즈니스 위험</span><span class="sxs-lookup"><span data-stu-id="ca5ab-119">Business risk</span></span>

<span data-ttu-id="ca5ab-120">리소스 일관성 분야는 핵심 운영 비즈니스 위험을 처리하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="ca5ab-120">The Resource Consistency discipline attempts to address core operational business risks.</span></span> <span data-ttu-id="ca5ab-121">비즈니스 및 IT 팀과 협력하여 이러한 위험을 식별하고, 클라우드 배포를 계획하고 구현할 때 각 위험에 대한 관련성을 모니터링합니다.</span><span class="sxs-lookup"><span data-stu-id="ca5ab-121">Work with your business and IT teams to identify these risks and monitor each of them for relevance as you plan for and implement your cloud deployments.</span></span>

<span data-ttu-id="ca5ab-122">조직마다 위험이 다르지만, 클라우드 거버넌스 팀 내 토론의 시작 지점으로 사용할 수 있는 일반적인 위험은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="ca5ab-122">Risks will differ between organization, but the following serve as common risks that you can use as a starting point for discussions within your Cloud Governance team:</span></span>

- <span data-ttu-id="ca5ab-123">**불필요한 운영 비용**.</span><span class="sxs-lookup"><span data-stu-id="ca5ab-123">**Unnecessary operational cost**.</span></span> <span data-ttu-id="ca5ab-124">쓸모없거나 사용되지 않는 리소스 또는 수요가 적은 시간에 오버프로비저닝된 리소스는 불필요한 운영 비용을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="ca5ab-124">Obsolete or unused resources, or resources that are overprovisioned during times of low demand, add unnecessary operational costs.</span></span>
- <span data-ttu-id="ca5ab-125">**언더프로비저닝된 리소스**.</span><span class="sxs-lookup"><span data-stu-id="ca5ab-125">**Underprovisioned resources**.</span></span> <span data-ttu-id="ca5ab-126">예상 수요보다 높은 리소스는 초과 수요로 인해 클라우드 리소스에 과부하가 발생하여 비즈니스를 중단시킬 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca5ab-126">Resources that experience higher than anticipated demand can result in business disruption as cloud resources are overwhelmed by demand.</span></span>
- <span data-ttu-id="ca5ab-127">**관리 비효율성**.</span><span class="sxs-lookup"><span data-stu-id="ca5ab-127">**Management inefficiencies**.</span></span> <span data-ttu-id="ca5ab-128">리소스와 관련된 일관된 메타데이터 명명 및 태그 지정이 부족하면 IT 직원이 관리 작업에 필요한 리소스를 찾거나 자산과 관련된 소유권 및 계정 정보를 파악하는 데 어려움을 겪을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca5ab-128">Lack of consistent naming and tagging metadata associated with resources can lead to IT staff having difficulty finding resources for management tasks or identifying ownership and accounting information related to assets.</span></span> <span data-ttu-id="ca5ab-129">이로 인해 비용이 증가하고 서비스 중단 또는 다른 운영 문제에 대한 IT 응답성이 저하될 수 있는 관리 비효율성이 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="ca5ab-129">This results in management inefficiencies that can increase cost and slow IT responsiveness to service disruption or other operational issues.</span></span>
- <span data-ttu-id="ca5ab-130">**비즈니스 중단**.</span><span class="sxs-lookup"><span data-stu-id="ca5ab-130">**Business Interruption**.</span></span> <span data-ttu-id="ca5ab-131">확정된 조직의 SLA(서비스 수준 계약)를 위반하는 서비스 중단으로 인해 비즈니스 또는 기타 재무적 효과가 손실될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca5ab-131">Service disruptions that result in violations of your organization's established Service Level Agreements (SLAs) can result in loss of business or other financial impacts to your company.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ca5ab-132">다음 단계</span><span class="sxs-lookup"><span data-stu-id="ca5ab-132">Next steps</span></span>

<span data-ttu-id="ca5ab-133">[클라우드 관리 템플릿](./template.md)을 사용하여 현재 클라우드 도입 계획으로 인해 발생할 가능성이 있는 비즈니스 위험을 문서화합니다.</span><span class="sxs-lookup"><span data-stu-id="ca5ab-133">Using the [Cloud Management Template](./template.md), document business risks that are likely to be introduced by the current cloud adoption plan.</span></span>

<span data-ttu-id="ca5ab-134">현실적인 비즈니스 위험이 이해되었으면 다음 단계는 회사의 위험 허용 범위와 이러한 허용 범위를 모니터링할 지표와 주요 메트릭을 문서화하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="ca5ab-134">Once an understanding of realistic business risks is established, the next step is to document the business's tolerance for risk and the indicators and key metrics to monitor that tolerance.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ca5ab-135">지표, 메트릭 및 위험 허용 범위 이해</span><span class="sxs-lookup"><span data-stu-id="ca5ab-135">Understand indicators, metrics, and risk tolerance</span></span>](./metrics-tolerance.md)
