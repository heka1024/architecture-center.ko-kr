---
title: 손상 방지 레이어 패턴
titleSuffix: Cloud Design Patterns
description: 현대식 애플리케이션과 레거시 시스템 사이에 외관 또는 어댑터 레이어를 구현합니다.
keywords: 디자인 패턴
author: dragon119
ms.date: 06/23/2017
ms.custom: seodec18
ms.openlocfilehash: d1023140deea4a2714c762945935d0838136e508
ms.sourcegitcommit: 680c9cef945dff6fee5e66b38e24f07804510fa9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54011279"
---
# <a name="anti-corruption-layer-pattern"></a><span data-ttu-id="491ee-104">손상 방지 레이어 패턴</span><span class="sxs-lookup"><span data-stu-id="491ee-104">Anti-Corruption Layer pattern</span></span>

<span data-ttu-id="491ee-105">동일한 의미 체계를 공유하지 않는 다른 하위 시스템 간에 외관 또는 어댑터 레이어를 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-105">Implement a façade or adapter layer between different subsystems that don't share the same semantics.</span></span> <span data-ttu-id="491ee-106">이 레이어는 하나의 하위 시스템의 요청을 다른 하위 시스템으로 변환합니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-106">This layer translates requests that one subsystem makes to the other subsystem.</span></span> <span data-ttu-id="491ee-107">이 패턴을 사용하여 애플리케이션의 디자인이 외부 하위 시스템에 대한 종속성으로 제한되지 않도록 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-107">Use this pattern to ensure that an application's design is not limited by dependencies on outside subsystems.</span></span> <span data-ttu-id="491ee-108">이 패턴은 *도메인 중심 디자인*에서 Eric Evans가 처음으로 설명했습니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-108">This pattern was first described by Eric Evans in *Domain-Driven Design*.</span></span>

## <a name="context-and-problem"></a><span data-ttu-id="491ee-109">컨텍스트 및 문제점</span><span class="sxs-lookup"><span data-stu-id="491ee-109">Context and problem</span></span>

<span data-ttu-id="491ee-110">대부분의 애플리케이션은 일부 데이터 또는 기능을 위해 다른 시스템에 의존합니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-110">Most applications rely on other systems for some data or functionality.</span></span> <span data-ttu-id="491ee-111">예를 들어 레거시 애플리케이션을 최신 시스템으로 마이그레이션해도 기존 레거시 리소스가 여전히 필요할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-111">For example, when a legacy application is migrated to a modern system, it may still need existing legacy resources.</span></span> <span data-ttu-id="491ee-112">새로운 기능은 레거시 시스템을 호출할 수 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-112">New features must be able to call the legacy system.</span></span> <span data-ttu-id="491ee-113">시간이 지나면서 더 큰 애플리케이션의 다른 기능을 최신 시스템으로 이동하게 되는 점진적 마이그레이션에서 특히 그렇습니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-113">This is especially true of gradual migrations, where different features of a larger application are moved to a modern system over time.</span></span>

<span data-ttu-id="491ee-114">종종 이러한 레거시 시스템에서 복잡한 데이터 스키마 또는 사용되지 않는 API와 같은 품질 문제가 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-114">Often these legacy systems suffer from quality issues such as convoluted data schemas or obsolete APIs.</span></span> <span data-ttu-id="491ee-115">레거시 시스템에서 사용되는 기능 및 기술은 최신 시스템의 경우와 크게 다를 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-115">The features and technologies used in legacy systems can vary widely from more modern systems.</span></span> <span data-ttu-id="491ee-116">레거시 시스템과 상호 작용하기 위해 새 애플리케이션은 오래된 인프라, 프로토콜, 데이터 모델, API 또는 최신 애플리케이션에 추가되지 않은 기타 기능을 지원해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-116">To interoperate with the legacy system, the new application may need to support outdated infrastructure, protocols, data models, APIs, or other features that you wouldn't otherwise put into a modern application.</span></span>

<span data-ttu-id="491ee-117">새 시스템과 레거시 시스템 간에 액세스를 유지 관리하면 새 시스템이 적어도 레거시 시스템 API의 일부 또는 기타 의미 체계를 강제로 준수할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-117">Maintaining access between new and legacy systems can force the new system to adhere to at least some of the legacy system's APIs or other semantics.</span></span> <span data-ttu-id="491ee-118">품질 문제가 있는 레거시 기능을 지원하면 완전하게 디자인된 최신 애플리케이션에 “손상”을 줄 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-118">When these legacy features have quality issues, supporting them "corrupts" what might otherwise be a cleanly designed modern application.</span></span>

<span data-ttu-id="491ee-119">레거시 시스템뿐 아니라 개발 팀이 제어하지 않는 모든 외부 시스템에서 비슷한 문제가 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-119">Similar issues can arise with any external system that your development team doesn't control, not just legacy systems.</span></span>

## <a name="solution"></a><span data-ttu-id="491ee-120">해결 방법</span><span class="sxs-lookup"><span data-stu-id="491ee-120">Solution</span></span>

<span data-ttu-id="491ee-121">손상 방지 레이어를 다른 하위 시스템 사이에 배치하여 격리합니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-121">Isolate the different subsystems by placing an anti-corruption layer between them.</span></span> <span data-ttu-id="491ee-122">이 레이어는 두 시스템 간의 통신을 변환하여, 다른 응용 프로그램이 해당 디자인 및 기술 방식을 손상시키지 않도록 하여 해당 시스템을 변경되지 않은 상태로 유지하도록 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-122">This layer translates communications between the two systems, allowing one system to remain unchanged while the other can avoid compromising its design and technological approach.</span></span>

![손상 방지 레이어 패턴의 다이어그램](./_images/anti-corruption-layer.png)

<span data-ttu-id="491ee-124">위의 다이어그램에는 두 개의 하위 시스템을 사용한 애플리케이션이 나와 있습니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-124">The diagram above shows an application with two subsystems.</span></span> <span data-ttu-id="491ee-125">하위 시스템 A는 손상 방지 레이어를 통해 하위 시스템 B를 호출합니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-125">Subsystem A calls to subsystem B through an anti-corruption layer.</span></span> <span data-ttu-id="491ee-126">하위 시스템 A와 손상 방지 레이어 간 통신은 항상 데이터 모델 및 하위 시스템 A의 아키텍처를 사용합니다. 손상 방지 레이어에서 하위 시스템 B로의 호출은 해당 하위 시스템의 데이터 모델 또는 메서드에 아키텍처를 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-126">Communication between subsystem A and the anti-corruption layer always uses the data model and architecture of subsystem A. Calls from the anti-corruption layer to subsystem B conform to that subsystem's data model or methods.</span></span> <span data-ttu-id="491ee-127">손상 방지 레이어는 두 시스템 사이의 변환에 필요한 모든 논리를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-127">The anti-corruption layer contains all of the logic necessary to translate between the two systems.</span></span> <span data-ttu-id="491ee-128">이러한 레이어는 애플리케이션 내의 구성 요소로 또는 독립적인 서비스로 구현할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-128">The layer can be implemented as a component within the application or as an independent service.</span></span>

## <a name="issues-and-considerations"></a><span data-ttu-id="491ee-129">문제 및 고려 사항</span><span class="sxs-lookup"><span data-stu-id="491ee-129">Issues and considerations</span></span>

- <span data-ttu-id="491ee-130">손상 방지 레이어가 있는 경우 두 시스템 간에 수행되는 호출이 지연될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-130">The anti-corruption layer may add latency to calls made between the two systems.</span></span>
- <span data-ttu-id="491ee-131">손상 방지 레이어는 관리 및 유지해야 하는 추가 서비스를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-131">The anti-corruption layer adds an additional service that must be managed and maintained.</span></span>
- <span data-ttu-id="491ee-132">손상 방지 레이어의 크기를 조정하는 방식을 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-132">Consider how your anti-corruption layer will scale.</span></span>
- <span data-ttu-id="491ee-133">둘 이상의 손상 방지 레이어가 필요한지 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-133">Consider whether you need more than one anti-corruption layer.</span></span> <span data-ttu-id="491ee-134">다양한 기술 또는 언어를 사용하여 기능을 여러 서비스로 분해하려고 하거나, 손상 방지 레이어를 분할할 다른 이유가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-134">You may want to decompose functionality into multiple services using different technologies or languages, or there may be other reasons to partition the anti-corruption layer.</span></span>
- <span data-ttu-id="491ee-135">다른 애플리케이션 또는 서비스와의 관계를 고려해서 손상 방지 레이어를 관리하는 방법을 결정합니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-135">Consider how the anti-corruption layer will be managed in relation with your other applications or services.</span></span> <span data-ttu-id="491ee-136">손상 방지 레이어가 모니터링, 릴리스 및 구성 프로세스에 어떻게 통합될 수 있을까요?</span><span class="sxs-lookup"><span data-stu-id="491ee-136">How will it be integrated into your monitoring, release, and configuration processes?</span></span>
- <span data-ttu-id="491ee-137">트랜잭션 및 데이터 일관성이 유지 관리되고 모니터링할 수 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-137">Make sure transaction and data consistency are maintained and can be monitored.</span></span>
- <span data-ttu-id="491ee-138">손상 방지 레이어가 다른 하위 시스템 간의 모든 통신을 처리해야 하는지 또는 기능의 하위 집합만 처리하면 되는지를 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-138">Consider whether the anti-corruption layer needs to handle all communication between different subsystems, or just a subset of features.</span></span>
- <span data-ttu-id="491ee-139">손상 방지 레이어가 애플리케이션 마이그레이션 전략의 일부인 경우 모든 레거시 기능이 마이그레이션된 후에 영구적인지 또는 사용 중지되는지를 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-139">If the anti-corruption layer is part of an application migration strategy, consider whether it will be permanent, or will be retired after all legacy functionality has been migrated.</span></span>

## <a name="when-to-use-this-pattern"></a><span data-ttu-id="491ee-140">이 패턴을 사용해야 하는 경우</span><span class="sxs-lookup"><span data-stu-id="491ee-140">When to use this pattern</span></span>

<span data-ttu-id="491ee-141">다음 경우에 이 패턴을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-141">Use this pattern when:</span></span>

- <span data-ttu-id="491ee-142">마이그레이션이 여러 단계를 통해 진행되도록 계획되어 있지만 새 시스템과 레거시시 스템 간의 통합을 유지 관리해야 하는 경우</span><span class="sxs-lookup"><span data-stu-id="491ee-142">A migration is planned to happen over multiple stages, but integration between new and legacy systems needs to be maintained.</span></span>
- <span data-ttu-id="491ee-143">두 개 또는 그 이상의 하위 시스템이 다른 의미 체계를 갖지만 여전히 통신해야 하는 경우</span><span class="sxs-lookup"><span data-stu-id="491ee-143">Two or more subsystems have different semantics, but still need to communicate.</span></span>

<span data-ttu-id="491ee-144">새 시스템과 레거시 시스템 간의 큰 의미 체계 차이가 없는 경우에는 이 패턴이 적합하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="491ee-144">This pattern may not be suitable if there are no significant semantic differences between new and legacy systems.</span></span>

## <a name="related-guidance"></a><span data-ttu-id="491ee-145">관련 지침</span><span class="sxs-lookup"><span data-stu-id="491ee-145">Related guidance</span></span>

- [<span data-ttu-id="491ee-146">스트랭글러 패턴</span><span class="sxs-lookup"><span data-stu-id="491ee-146">Strangler pattern</span></span>](./strangler.md)
