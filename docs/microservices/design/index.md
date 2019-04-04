---
title: Azure Kubernetes Service에 대한 마이크로서비스 참조 구현
description: 이 참조 구현은 마이크로서비스 아키텍처의 모범 사례를 보여줍니다.
author: MikeWasson
ms.date: 02/26/2019
ms.topic: guide
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: microservices
ms.openlocfilehash: 15e9aa16c0e2cfccecbfb84d217c275cc99a66fd
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58344413"
---
# <a name="designing-a-microservices-architecture"></a><span data-ttu-id="639ee-103">마이크로서비스 아키텍처 디자인</span><span class="sxs-lookup"><span data-stu-id="639ee-103">Designing a microservices architecture</span></span>

<span data-ttu-id="639ee-104">마이크로 서비스는 복원력이 있고, 확장성이 뛰어나며, 독립적으로 배포할 수 있고, 신속하게 진화할 수 있는 클라우드 애플리케이션을 구축하는 데 널리 사용되는 아키텍처 스타일이 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="639ee-104">Microservices have become a popular architectural style for building cloud applications that are resilient, highly scalable, independently deployable, and able to evolve quickly.</span></span> <span data-ttu-id="639ee-105">하지만, 마이크로 서비스가 단순히 업계 유행어로만 남지 않으려면 애플리케이션을 설계하고 구축하는 데 있어 다른 접근 방식이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="639ee-105">To be more than just a buzzword, however, microservices require a different approach to designing and building applications.</span></span>

<span data-ttu-id="639ee-106">이어지는 본 문서에서는 Azure에서 마이크로 서비스 아키텍처를 구축하고 실행하는 방법을 살펴봅니다.</span><span class="sxs-lookup"><span data-stu-id="639ee-106">In this set of articles, we explore how to build and run a microservices architecture on Azure.</span></span> <span data-ttu-id="639ee-107">다룰 주제는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="639ee-107">Topics include:</span></span>

- [<span data-ttu-id="639ee-108">서비스 간 통신</span><span class="sxs-lookup"><span data-stu-id="639ee-108">Interservice communication</span></span>](./interservice-communication.md)
- [<span data-ttu-id="639ee-109">API 디자인</span><span class="sxs-lookup"><span data-stu-id="639ee-109">API design</span></span>](./api-design.md)
- [<span data-ttu-id="639ee-110">API 게이트웨이</span><span class="sxs-lookup"><span data-stu-id="639ee-110">API gateways</span></span>](./gateway.md)
- [<span data-ttu-id="639ee-111">데이터 고려 사항</span><span class="sxs-lookup"><span data-stu-id="639ee-111">Data considerations</span></span>](./data-considerations.md)
- [<span data-ttu-id="639ee-112">디자인 패턴</span><span class="sxs-lookup"><span data-stu-id="639ee-112">Design patterns</span></span>](./patterns.md)

## <a name="prerequisites"></a><span data-ttu-id="639ee-113">필수 조건</span><span class="sxs-lookup"><span data-stu-id="639ee-113">Prerequisites</span></span>

<span data-ttu-id="639ee-114">위의 문서를 읽기 전에, 다음 문서를 먼저 읽어봅니다.</span><span class="sxs-lookup"><span data-stu-id="639ee-114">Before reading these articles, you might start with the following:</span></span>

- <span data-ttu-id="639ee-115">[마이크로서비스 아키텍처 소개](../introduction.md)</span><span class="sxs-lookup"><span data-stu-id="639ee-115">[Introduction to microservices architectures](../introduction.md).</span></span> <span data-ttu-id="639ee-116">마이크로서비스의 장점 및 과제가 무엇이며 이 아키텍처 스타일을 언제 사용하는지 이해합니다.</span><span class="sxs-lookup"><span data-stu-id="639ee-116">Understand the benefits and challenges of microservices, and when to use this style of architecture.</span></span>
- <span data-ttu-id="639ee-117">[도메인 분석을 사용하여 마이크로서비스 모델링](../model/domain-analysis.md)</span><span class="sxs-lookup"><span data-stu-id="639ee-117">[Using domain analysis to model microservices](../model/domain-analysis.md).</span></span> <span data-ttu-id="639ee-118">마이크로서비스를 모델링하는 도메인 기반 접근 방식을 알아봅니다.</span><span class="sxs-lookup"><span data-stu-id="639ee-118">Learn a domain-driven approach to modeling microservices.</span></span>

## <a name="reference-implementation"></a><span data-ttu-id="639ee-119">참조 구현</span><span class="sxs-lookup"><span data-stu-id="639ee-119">Reference implementation</span></span>

<span data-ttu-id="639ee-120">마이크로서비스 아키텍처의 모범 사례를 보여드리기 위해 드론 배달 애플리케이션이라는 참조 구현을 만들었습니다.</span><span class="sxs-lookup"><span data-stu-id="639ee-120">To illustrate best practices for a microservices architecture, we created a reference implementation that we call the Drone Delivery application.</span></span> <span data-ttu-id="639ee-121">이 구현은 AKS(Azure Kubernetes Service)를 사용하여 Kubernetes에서 실행됩니다.</span><span class="sxs-lookup"><span data-stu-id="639ee-121">This implementation runs on Kubernetes using Azure Kubernetes Service (AKS).</span></span> <span data-ttu-id="639ee-122">참조 구현은 [GitHub][drone-ri]에서 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="639ee-122">You can find the reference implementation on [GitHub][drone-ri].</span></span>

![드론 배달 애플리케이션의 아키텍처](../images/drone-delivery.png)

## <a name="scenario"></a><span data-ttu-id="639ee-124">시나리오</span><span class="sxs-lookup"><span data-stu-id="639ee-124">Scenario</span></span>

<span data-ttu-id="639ee-125">Fabrikam, Inc.라는 회사가 드론 배달 서비스를 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="639ee-125">Fabrikam, Inc. is starting a drone delivery service.</span></span> <span data-ttu-id="639ee-126">이 회사는 다수의 드론 항공기를 관리합니다.</span><span class="sxs-lookup"><span data-stu-id="639ee-126">The company manages a fleet of drone aircraft.</span></span> <span data-ttu-id="639ee-127">기업은 서비스에 등록하고 사용자는 배달을 위해 드론이 상품을 픽업하도록 요청할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="639ee-127">Businesses register with the service, and users can request a drone to pick up goods for delivery.</span></span> <span data-ttu-id="639ee-128">사용자가 픽업을 예약하면 백 엔드 시스템이 드론을 할당하고 사용자에게 예상 배달 시간을 알립니다.</span><span class="sxs-lookup"><span data-stu-id="639ee-128">When a customer schedules a pickup, a backend system assigns a drone and notifies the user with an estimated delivery time.</span></span> <span data-ttu-id="639ee-129">배달이 진행되는 동안 지속적으로 업데이트되는 ETA를 통해 고객은 드론의 위치를 추적할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="639ee-129">While the delivery is in progress, the customer can track the location of the drone, with a continuously updated ETA.</span></span>

<span data-ttu-id="639ee-130">이 시나리오에는 상당히 복잡한 도메인이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="639ee-130">This scenario involves a fairly complicated domain.</span></span> <span data-ttu-id="639ee-131">비즈니스 우려 사항으로는 드론 예약, 패키지 추적, 사용자 계정 관리, 기록 데이터 저장 및 분석 등이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="639ee-131">Some of the business concerns include scheduling drones, tracking packages, managing user accounts, and storing and analyzing historical data.</span></span> <span data-ttu-id="639ee-132">또한 Fabrikam은 빠르게 시장에 진입 및 반복하며 새로운 기능을 추가하고자 합니다.</span><span class="sxs-lookup"><span data-stu-id="639ee-132">Moreover, Fabrikam wants to get to market quickly and then iterate quickly, adding new functionality and capabilities.</span></span> <span data-ttu-id="639ee-133">애플리케이션은 SLO(서비스 수준 목표)가 높은 클라우드 규모에서 작동해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="639ee-133">The application needs to operate at cloud scale, with a high service level objective (SLO).</span></span> <span data-ttu-id="639ee-134">또한 시스템의 각 부분에서 데이터 저장소 및 쿼리에 대한 요구 사항이 완전히 달라야 합니다.</span><span class="sxs-lookup"><span data-stu-id="639ee-134">Fabrikam also expects that different parts of the system will have very different requirements for data storage and querying.</span></span> <span data-ttu-id="639ee-135">이러한 모든 사항을 고려한 끝에 Fabrikam은 드론 배달 애플리케이션에 마이크로 서비스 아키텍처를 사용하기로 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="639ee-135">All of these considerations lead Fabrikam to choose a microservices architecture for the Drone Delivery application.</span></span>

> [!NOTE]
> <span data-ttu-id="639ee-136">마이크로 서비스 아키텍처와 다른 아키텍처 스타일 중에서 선택을 해야 하는 경우 도움이 필요하면 [Azure 애플리케이션 아키텍처 가이드](../../guide/index.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="639ee-136">For help in choosing between a microservices architecture and other architectural styles, see the [Azure Application Architecture Guide](../../guide/index.md).</span></span>

<span data-ttu-id="639ee-137">제공된 참조 구현에는 [AKS(Azure Kubernetes Service)](/azure/aks/)와 Kubernetes가 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="639ee-137">Our reference implementation uses Kubernetes with [Azure Kubernetes Service](/azure/aks/) (AKS).</span></span> <span data-ttu-id="639ee-138">단, [Azure Service Fabric](/azure/service-fabric/)을 비롯한 모든 컨테이너 오케스트레이터에는 높은 수준의 아키텍처 결정과 도전 과제가 많이 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="639ee-138">However, many of the high-level architectural decisions and challenges will apply to any container orchestrator, including [Azure Service Fabric](/azure/service-fabric/).</span></span>

<!-- links -->

[drone-ri]: https://github.com/mspnp/microservices-reference-implementation

## <a name="next-steps"></a><span data-ttu-id="639ee-139">다음 단계</span><span class="sxs-lookup"><span data-stu-id="639ee-139">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="639ee-140">컴퓨팅 옵션 선택</span><span class="sxs-lookup"><span data-stu-id="639ee-140">Choose a compute option</span></span>](./compute-options.md)