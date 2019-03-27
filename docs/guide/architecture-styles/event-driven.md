---
title: 이벤트 기반 아키텍처 스타일
titleSuffix: Azure Application Architecture Guide
description: Azure에서 이벤트 기반 아키텍처와 IoT 아키텍처의 혜택, 과제 및 모범 사례를 설명합니다.
author: MikeWasson
ms.date: 08/30/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: seojan19
ms.openlocfilehash: b83a919e4ccd41d20b508b10604365a0b4ca90af
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58245644"
---
# <a name="event-driven-architecture-style"></a><span data-ttu-id="bcadf-103">이벤트 기반 아키텍처 스타일</span><span class="sxs-lookup"><span data-stu-id="bcadf-103">Event-driven architecture style</span></span>

<span data-ttu-id="bcadf-104">이벤트 기반 아키텍처는 이벤트 스트림을 생성하는 **이벤트 생산자**와 이벤트를 수신 대기하는 **이벤트 소비자**로 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-104">An event-driven architecture consists of **event producers** that generate a stream of events, and **event consumers** that listen for the events.</span></span>

![이벤트 기반 아키텍처 스타일의 다이어그램](./images/event-driven.svg)

<span data-ttu-id="bcadf-106">이벤트는 거의 실시간으로 전달되므로 이벤트가 발생하는 즉시 소비자가 이벤트에 응답할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-106">Events are delivered in near real time, so consumers can respond immediately to events as they occur.</span></span> <span data-ttu-id="bcadf-107">생산자와 소비자가 분리되므로 생산자는 수신 대기 중인 소비자를 알 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-107">Producers are decoupled from consumers &mdash; a producer doesn't know which consumers are listening.</span></span> <span data-ttu-id="bcadf-108">소비자도 서로 분리되며 각 소비자에게 모든 이벤트가 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-108">Consumers are also decoupled from each other, and every consumer sees all of the events.</span></span> <span data-ttu-id="bcadf-109">이는 소비자가 큐에서 메시지를 끌어오고 오류가 없다고 가정할 경우 메시지가 한 번만 처리되는 [경쟁 소비자][competing-consumers] 패턴과 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-109">This differs from a [Competing Consumers][competing-consumers] pattern, where consumers pull messages from a queue and a message is processed just once (assuming no errors).</span></span> <span data-ttu-id="bcadf-110">IoT와 같은 일부 시스템에서는 매우 높은 볼륨으로 이벤트를 수집해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-110">In some systems, such as IoT, events must be ingested at very high volumes.</span></span>

<span data-ttu-id="bcadf-111">이벤트 기반 아키텍처는 게시자/구독자 모델 또는 이벤트 스트림 모델을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-111">An event driven architecture can use a pub/sub model or an event stream model.</span></span>

- <span data-ttu-id="bcadf-112">**게시자/구독자**: 메시징 인프라에서 구독을 추적합니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-112">**Pub/sub**: The messaging infrastructure keeps track of subscriptions.</span></span> <span data-ttu-id="bcadf-113">이벤트가 게시되면 각 구독자에게 이벤트를 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-113">When an event is published, it sends the event to each subscriber.</span></span> <span data-ttu-id="bcadf-114">이벤트를 받은 후에는 재생할 수 없으며 새 구독자에게 이벤트가 표시되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-114">After an event is received, it cannot be replayed, and new subscribers do not see the event.</span></span>

- <span data-ttu-id="bcadf-115">**이벤트 스트리밍**: 이벤트가 로그에 기록됩니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-115">**Event streaming**: Events are written to a log.</span></span> <span data-ttu-id="bcadf-116">이벤트가 파티션 내에 엄격하게 정렬되며 지속 가능합니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-116">Events are strictly ordered (within a partition) and durable.</span></span> <span data-ttu-id="bcadf-117">클라이언트는 스트림을 구독하지 않고, 대신 스트림의 일부에서 읽을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-117">Clients don't subscribe to the stream, instead a client can read from any part of the stream.</span></span> <span data-ttu-id="bcadf-118">클라이언트가 스트림에서 해당 위치를 진행해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-118">The client is responsible for advancing its position in the stream.</span></span> <span data-ttu-id="bcadf-119">즉, 클라이언트가 언제든지 연결하고 이벤트를 재생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-119">That means a client can join at any time, and can replay events.</span></span>

<span data-ttu-id="bcadf-120">소비자 측면에서 다음과 같은 몇 가지 일반적인 변형이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-120">On the consumer side, there are some common variations:</span></span>

- <span data-ttu-id="bcadf-121">**단순 이벤트 처리**.</span><span class="sxs-lookup"><span data-stu-id="bcadf-121">**Simple event processing**.</span></span> <span data-ttu-id="bcadf-122">이벤트가 즉시 소비자에서 작업을 트리거합니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-122">An event immediately triggers an action in the consumer.</span></span> <span data-ttu-id="bcadf-123">예를 들어 서비스 버스 토픽에 메시지를 게시할 때마다 함수가 실행되도록 서비스 버스 트리거와 함께 Azure Functions를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-123">For example, you could use Azure Functions with a Service Bus trigger, so that a function executes whenever a message is published to a Service Bus topic.</span></span>

- <span data-ttu-id="bcadf-124">**복합 이벤트 처리**.</span><span class="sxs-lookup"><span data-stu-id="bcadf-124">**Complex event processing**.</span></span> <span data-ttu-id="bcadf-125">소비자가 Azure Stream Analytics 또는 Apache Storm과 같은 기술을 사용하여 이벤트 데이터에서 패턴을 찾으면서 일련의 이벤트를 처리합니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-125">A consumer processes a series of events, looking for patterns in the event data, using a technology such as Azure Stream Analytics or Apache Storm.</span></span> <span data-ttu-id="bcadf-126">예를 들어 포함된 디바이스에서 일정 기간 동안 읽은 값을 집계하고 이동 평균이 특정 임계값을 초과하면 알림을 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-126">For example, you could aggregate readings from an embedded device over a time window, and generate a notification if the moving average crosses a certain threshold.</span></span>

- <span data-ttu-id="bcadf-127">**이벤트 스트림 처리**.</span><span class="sxs-lookup"><span data-stu-id="bcadf-127">**Event stream processing**.</span></span> <span data-ttu-id="bcadf-128">Azure IoT Hub 또는 Apache Kafka와 같은 데이터 스트리밍 플랫폼을 파이프라인으로 사용하여 이벤트를 수집하고 스트림 프로세서에 공급합니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-128">Use a data streaming platform, such as Azure IoT Hub or Apache Kafka, as a pipeline to ingest events and feed them to stream processors.</span></span> <span data-ttu-id="bcadf-129">스트림 프로세서는 스트림을 처리하거나 변환합니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-129">The stream processors act to process or transform the stream.</span></span> <span data-ttu-id="bcadf-130">애플리케이션의 각 하위 시스템에 사용되는 여러 스트림 프로세서가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-130">There may be multiple stream processors for different subsystems of the application.</span></span> <span data-ttu-id="bcadf-131">이 접근 방법은 IoT 워크로드에 적합합니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-131">This approach is a good fit for IoT workloads.</span></span>

<span data-ttu-id="bcadf-132">이벤트의 소스가 IoT 솔루션의 물리적 디바이스와 같이 시스템 외부에 있을 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-132">The source of the events may be external to the system, such as physical devices in an IoT solution.</span></span> <span data-ttu-id="bcadf-133">이 경우 시스템이 데이터 원본에 필요한 볼륨 및 처리량으로 데이터를 수집할 수 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-133">In that case, the system must be able to ingest the data at the volume and throughput that is required by the data source.</span></span>

<span data-ttu-id="bcadf-134">위의 논리 다이어그램에서 각 소비자 유형은 단일 상자로 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-134">In the logical diagram above, each type of consumer is shown as a single box.</span></span> <span data-ttu-id="bcadf-135">실제로는 소비자가 시스템의 단일 실패 지점이 되지 않도록 한 소비자의 여러 인스턴스가 있는 것이 일반적입니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-135">In practice, it's common to have multiple instances of a consumer, to avoid having the consumer become a single point of failure in system.</span></span> <span data-ttu-id="bcadf-136">이벤트의 볼륨 및 빈도를 처리하기 위해 여러 인스턴스가 필요할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-136">Multiple instances might also be necessary to handle the volume and frequency of events.</span></span> <span data-ttu-id="bcadf-137">또한 단일 소비자가 여러 스레드의 이벤트를 처리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-137">Also, a single consumer might process events on multiple threads.</span></span> <span data-ttu-id="bcadf-138">이벤트가 순서대로 처리되어야 하거나 정확히 한 번 처리되어야 하는 경우 이것이 문제가 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-138">This can create challenges if events must be processed in order, or require exactly-once semantics.</span></span> <span data-ttu-id="bcadf-139">[조정 최소화][minimize-coordination]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="bcadf-139">See [Minimize Coordination][minimize-coordination].</span></span>

## <a name="when-to-use-this-architecture"></a><span data-ttu-id="bcadf-140">이 아키텍처를 사용하는 경우</span><span class="sxs-lookup"><span data-stu-id="bcadf-140">When to use this architecture</span></span>

- <span data-ttu-id="bcadf-141">여러 하위 시스템이 동일한 이벤트를 처리해야 하는 경우.</span><span class="sxs-lookup"><span data-stu-id="bcadf-141">Multiple subsystems must process the same events.</span></span>
- <span data-ttu-id="bcadf-142">최소 시간 지연의 실시간 처리.</span><span class="sxs-lookup"><span data-stu-id="bcadf-142">Real-time processing with minimum time lag.</span></span>
- <span data-ttu-id="bcadf-143">패턴 일치 또는 일정 기간의 집계와 같은 복합 이벤트 처리.</span><span class="sxs-lookup"><span data-stu-id="bcadf-143">Complex event processing, such as pattern matching or aggregation over time windows.</span></span>
- <span data-ttu-id="bcadf-144">높은 볼륨 및 높은 데이터 개발속도(예: IoT).</span><span class="sxs-lookup"><span data-stu-id="bcadf-144">High volume and high velocity of data, such as IoT.</span></span>

## <a name="benefits"></a><span data-ttu-id="bcadf-145">이점</span><span class="sxs-lookup"><span data-stu-id="bcadf-145">Benefits</span></span>

- <span data-ttu-id="bcadf-146">생산자와 소비자가 분리됩니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-146">Producers and consumers are decoupled.</span></span>
- <span data-ttu-id="bcadf-147">지점 간 통합이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-147">No point-to point-integrations.</span></span> <span data-ttu-id="bcadf-148">시스템에 새 소비자를 쉽게 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-148">It's easy to add new consumers to the system.</span></span>
- <span data-ttu-id="bcadf-149">이벤트가 도착하는 즉시 소비자가 이벤트에 응답할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-149">Consumers can respond to events immediately as they arrive.</span></span>
- <span data-ttu-id="bcadf-150">확장성이 있고 배포 가능합니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-150">Highly scalable and distributed.</span></span>
- <span data-ttu-id="bcadf-151">하위 시스템에서 이벤트 스트림을 독립적으로 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-151">Subsystems have independent views of the event stream.</span></span>

## <a name="challenges"></a><span data-ttu-id="bcadf-152">과제</span><span class="sxs-lookup"><span data-stu-id="bcadf-152">Challenges</span></span>

- <span data-ttu-id="bcadf-153">배달 보장.</span><span class="sxs-lookup"><span data-stu-id="bcadf-153">Guaranteed delivery.</span></span> <span data-ttu-id="bcadf-154">일부 시스템, 특히 IoT 시나리오에서는 이벤트가 배달되도록 보장하는 것이 중요합니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-154">In some systems, especially in IoT scenarios, it's crucial to guarantee that events are delivered.</span></span>
- <span data-ttu-id="bcadf-155">이벤트를 순서대로 또는 한 번만 처리.</span><span class="sxs-lookup"><span data-stu-id="bcadf-155">Processing events in order or exactly once.</span></span> <span data-ttu-id="bcadf-156">복원 및 확장성을 위해 일반적으로 각 소비자 유형이 여러 인스턴스에서 실행됩니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-156">Each consumer type typically runs in multiple instances, for resiliency and scalability.</span></span> <span data-ttu-id="bcadf-157">이벤트가 소비자 유형 내에서 순서대로 처리되어야 하거나 처리 논리가 비멱등적인 경우 이것이 문제가 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bcadf-157">This can create a challenge if the events must be processed in order (within a consumer type), or if the processing logic is not idempotent.</span></span>

 <!-- links -->

[competing-consumers]: ../../patterns/competing-consumers.md
[minimize-coordination]: ../design-principles/minimize-coordination.md
