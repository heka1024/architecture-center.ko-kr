---
title: 'CAF: 보안 기준 동기 부여 및 비즈니스 위험'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 보안 기준 동기 부여 및 비즈니스 위험
author: BrianBlanchard
ms.openlocfilehash: 8407ed358e5862e466176096ee6a82ad792027cb
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58247314"
---
# <a name="security-baseline-motivations-and-business-risks"></a><span data-ttu-id="2c2d6-103">보안 기준 동기 부여 및 비즈니스 위험</span><span class="sxs-lookup"><span data-stu-id="2c2d6-103">Security Baseline motivations and business risks</span></span>

<span data-ttu-id="2c2d6-104">이 문서에서는 고객이 일반적으로 클라우드 거버넌스 전략 내에 보안 기준 분야를 도입하는 이유를 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="2c2d6-104">This article discusses the reasons that customers typically adopt a Security Baseline discipline within a cloud governance strategy.</span></span> <span data-ttu-id="2c2d6-105">또한 정책 설명을 추진할 수 있는 잠재적인 비즈니스 위험의 몇 가지 예도 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="2c2d6-105">It also provides a few examples of potential business risks that can drive policy statements.</span></span>

<!-- markdownlint-disable MD026 -->

## <a name="is-a-security-baseline-relevant"></a><span data-ttu-id="2c2d6-106">보안 기준이 적절합니까?</span><span class="sxs-lookup"><span data-stu-id="2c2d6-106">Is a Security Baseline relevant?</span></span>

<span data-ttu-id="2c2d6-107">보안은 모든 IT 조직의 주요 관심사입니다.</span><span class="sxs-lookup"><span data-stu-id="2c2d6-107">Security is a key concern for any IT organization.</span></span> <span data-ttu-id="2c2d6-108">클라우드 배포 시 워크로드가 기존의 온-프레미스 데이터 센터에 호스팅될 때와 동일한 보안 위험에 직면합니다.</span><span class="sxs-lookup"><span data-stu-id="2c2d6-108">Cloud deployments face many of the same security risks as workloads hosted in traditional on-premises datacenters.</span></span> <span data-ttu-id="2c2d6-109">그러나 워크로드를 저장하고 실행할 물리적 하드웨어의 직접 소유권이 없다는 공용 클라우드 플랫폼의 속성 상, 클라우드 보안을 위해 자체 정책 및 프로세스가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="2c2d6-109">However, the nature of public cloud platforms, with a lack of direct ownership of the physical hardware storing and running your workloads, means cloud security requires its own policy and processes.</span></span>

<span data-ttu-id="2c2d6-110">클라우드 보안 거버넌스가 기존의 보안 정책과 구별되는 주요 특징 중 하나는 리소스를 간편하게 만들 수 있다는 것이며, 배포 전에 보안을 고려하지 않으면 취약성이 더 늘어날 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2c2d6-110">One of the primary things that set cloud security governance apart from traditional security policy is the ease with which resources can be created, potentially adding vulnerabilities if security isn't considered before deployment.</span></span> <span data-ttu-id="2c2d6-111">또한 클라우드 기반 네트워크 토폴로지를 신속하게 변경할 수 있도록 [SDN(소프트웨어 정의 네트워킹)](../../decision-guides/software-defined-network/overview.md) 같은 기술이 제공하는 유연성을 통해 전체적인 네트워크 공격 노출 영역을 예측할 수 없는 방식으로 쉽게 수정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2c2d6-111">The flexibility that technologies like [software defined networking (SDN)](../../decision-guides/software-defined-network/overview.md) provide for rapidly changing your cloud-based network topology can also easily modify your overall network attack surface in unforeseen ways.</span></span> <span data-ttu-id="2c2d6-112">클라우드 플랫폼 역시 온-프레미스 환경에서는 불가능할 수 있는 방식으로 보안을 강화하는 도구와 기능을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="2c2d6-112">Cloud platforms also provide tools and features that can improve your security capabilities in ways not always possible in on-premises environments.</span></span>

<span data-ttu-id="2c2d6-113">고객이 보안 정책 및 프로세스에 투자하는 금액은 클라우드 배포의 특성에 따라 크게 달라집니다.</span><span class="sxs-lookup"><span data-stu-id="2c2d6-113">The amount you invest into security policy and processes will depend a great deal on the nature of your cloud deployment.</span></span> <span data-ttu-id="2c2d6-114">초기 배포 시에는 가장 기본적인 보안 정책만 적용하면 되지만, 중요 업무용 워크로드는 복잡하고 광범위한 보안 요구 사항을 동반할 것입니다.</span><span class="sxs-lookup"><span data-stu-id="2c2d6-114">Initial test deployments may only need the most basic of security policies in place, while a mission-critical workload will entail addressing complex and extensive security needs.</span></span> <span data-ttu-id="2c2d6-115">모든 배포는 일정 수준에서 분야의 개입이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="2c2d6-115">All deployments will need to engage with the discipline at some level.</span></span>

<span data-ttu-id="2c2d6-116">보안 기준 분야는 보안 위험으로부터 클라우드 배포를 보호하기 위해 적용할 수 있는 회사 정책 및 수동 프로세스를 다룹니다.</span><span class="sxs-lookup"><span data-stu-id="2c2d6-116">The Security Baseline discipline covers the corporate policies and manual processes that you can put in place to protect your cloud deployment against security risks.</span></span>

> [!NOTE]
><span data-ttu-id="2c2d6-117">보안 기준의 컨텍스트에서 [ID 기준](../identity-baseline/overview.md) 및 액세스 제어와의 관련성을 이해하는 것이 중요하지만, [클라우드 거버넌스의 5개 분야](../overview.md)에서는 [ID 기준](../identity-baseline/overview.md)을 보안 기준과 분리된 자체 분야로 간주합니다.</span><span class="sxs-lookup"><span data-stu-id="2c2d6-117">While it is important to understand [Identity Baseline](../identity-baseline/overview.md) in the context of Security Baseline and how that relates to Access Control, the [Five Disciplines of Cloud Governance](../overview.md) calls out [Identity Baseline](../identity-baseline/overview.md) as its own discipline, separate from Security Baseline.</span></span>

## <a name="business-risk"></a><span data-ttu-id="2c2d6-118">비즈니스 위험</span><span class="sxs-lookup"><span data-stu-id="2c2d6-118">Business risk</span></span>

<span data-ttu-id="2c2d6-119">보안 기준 분야는 핵심 보안 관련 비즈니스 위험을 해결하려고 시도합니다.</span><span class="sxs-lookup"><span data-stu-id="2c2d6-119">The Security Baseline discipline attempts to address core security-related business risks.</span></span> <span data-ttu-id="2c2d6-120">클라우드 배포를 계획하고 구현할 때 회사와 협력하여 이러한 위험을 식별하고 관련성을 모니터링하세요.</span><span class="sxs-lookup"><span data-stu-id="2c2d6-120">Work with your business to identify these risks and monitor each of them for relevance as you plan for and implement your cloud deployments.</span></span>

<span data-ttu-id="2c2d6-121">위험은 조직마다 다르지만, 다음 항목을 일반적인 보안 관련 위험으로 사용하여 클라우드 거버넌스 팀 내에서 토론을 시작하는 시작점으로 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2c2d6-121">Risks will differ between organization, but the following serve as common security-related risks that you can use as a starting point for discussions within your Cloud Governance team:</span></span>

- <span data-ttu-id="2c2d6-122">**데이터 보안 위반**.</span><span class="sxs-lookup"><span data-stu-id="2c2d6-122">**Data breach**.</span></span> <span data-ttu-id="2c2d6-123">클라우드에 호스팅된 중요 데이터가 실수로 노출 또는 손실되면 고객을 잃게 되거나 계약 문제 또는 법적 책임 문제가 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2c2d6-123">Inadvertent exposure or loss of sensitive cloud-hosted data can lead to losing customers, contractual issues, or legal consequences.</span></span>
- <span data-ttu-id="2c2d6-124">**서비스 중단**.</span><span class="sxs-lookup"><span data-stu-id="2c2d6-124">**Service disruption**.</span></span> <span data-ttu-id="2c2d6-125">안전하지 않은 인프라 때문에 발생하는 가동 중단 및 기타 성능 문제는 정상 작동을 방해하며 생산성 또는 비즈니스 손실로 이어질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2c2d6-125">Outages and other performance issues due to insecure infrastructure interrupts normal operations and can result in lost productivity or lost business.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2c2d6-126">다음 단계</span><span class="sxs-lookup"><span data-stu-id="2c2d6-126">Next steps</span></span>

<span data-ttu-id="2c2d6-127">[클라우드 관리 템플릿](./template.md)을 사용하여 현재 클라우드 도입 계획으로 인해 발생할 가능성이 있는 비즈니스 위험을 문서화합니다.</span><span class="sxs-lookup"><span data-stu-id="2c2d6-127">Using the [Cloud Management Template](./template.md), document business risks that are likely to be introduced by the current cloud adoption plan.</span></span>

<span data-ttu-id="2c2d6-128">현실적인 비즈니스 위험이 이해되었으면 다음 단계는 회사의 위험 허용 범위와 이러한 허용 범위를 모니터링할 지표와 주요 메트릭을 문서화하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="2c2d6-128">Once an understanding of realistic business risks is established, the next step is to document the business's tolerance for risk and the indicators and key metrics to monitor that tolerance.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="2c2d6-129">지표, 메트릭 및 위험 허용 범위 이해</span><span class="sxs-lookup"><span data-stu-id="2c2d6-129">Understand indicators, metrics, and risk tolerance</span></span>](./metrics-tolerance.md)
