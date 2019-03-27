---
title: CQRS 아키텍처 스타일
titleSuffix: Azure Application Architecture Guide
description: CQRS 아키텍처의 이점, 과제 및 모범 사례를 설명합니다.
author: MikeWasson
ms.date: 08/30/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: seojan19
ms.openlocfilehash: 35f37afa60f943f410f1fbd46c789c0b2c66e36e
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58246204"
---
# <a name="cqrs-architecture-style"></a><span data-ttu-id="992d2-103">CQRS 아키텍처 스타일</span><span class="sxs-lookup"><span data-stu-id="992d2-103">CQRS architecture style</span></span>

<span data-ttu-id="992d2-104">CQRS(명령 및 쿼리 책임 분리)는 쓰기 작업에서 읽기 작업을 구분하는 아키텍처 스타일입니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-104">Command and Query Responsibility Segregation (CQRS) is an architecture style that separates read operations from write operations.</span></span>

![CQRS 아키텍처 스타일의 논리 다이어그램](./images/cqrs-logical.svg)

<span data-ttu-id="992d2-106">기존 아키텍처에서 데이터베이스를 쿼리하고 업데이트하는 데 동일한 데이터 모델을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-106">In traditional architectures, the same data model is used to query and update a database.</span></span> <span data-ttu-id="992d2-107">그러면 간단하고 기본적인 CRUD 작업에 적합합니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-107">That's simple and works well for basic CRUD operations.</span></span> <span data-ttu-id="992d2-108">그러나 더 복잡한 애플리케이션에서는 이 방법을 사용하기 어려울 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-108">In more complex applications, however, this approach can become unwieldy.</span></span> <span data-ttu-id="992d2-109">예를 들어 애플리케이션은 읽기 쪽에서 다른 쿼리를 수행할 수 있습니다. 그러면 모양이 다른 DTO(데이터 전송 개체)를 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-109">For example, on the read side, the application may perform many different queries, returning data transfer objects (DTOs) with different shapes.</span></span> <span data-ttu-id="992d2-110">개체 매핑이 복잡해 질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-110">Object mapping can become complicated.</span></span> <span data-ttu-id="992d2-111">모델은 쓰기 쪽에서 복잡한 유효성 검사 및 비즈니스 논리를 구현할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-111">On the write side, the model may implement complex validation and business logic.</span></span> <span data-ttu-id="992d2-112">따라서 너무 많은 작업을 수행하는 과도하게 복잡한 모델이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-112">As a result, you can end up with an overly complex model that does too much.</span></span>

<span data-ttu-id="992d2-113">읽기 및 쓰기 워크로드는 매우 다른 성능 및 확장성 요구 사항을 포함하여 비대칭이라는 점이 또 다른 잠재적인 문제입니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-113">Another potential problem is that read and write workloads are often asymmetrical, with very different performance and scale requirements.</span></span>

<span data-ttu-id="992d2-114">CQRS는 읽기 및 쓰기를 구분된 모델로 구분하여 이러한 문제를 해결합니다. 이 때 데이터를 업데이트하는 **명령** 및 데이터를 읽는 **쿼리**를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-114">CQRS addresses these problems by separating reads and writes into separate models, using **commands** to update data, and **queries** to read data.</span></span>

- <span data-ttu-id="992d2-115">명령은 데이터 중심이 아닌 작업을 기반으로 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-115">Commands should be task based, rather than data centric.</span></span> <span data-ttu-id="992d2-116">("ReservationStatus를 Reserved로 설정"하지 않고 "호텔 객실 예약")명령은 동기적으로 처리되지 않고 비동기 처리를 위해 큐에 배치될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-116">("Book hotel room," not "set ReservationStatus to Reserved.") Commands may be placed on a queue for asynchronous processing, rather than being processed synchronously.</span></span>

- <span data-ttu-id="992d2-117">쿼리는 데이터베이스를 수정하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-117">Queries never modify the database.</span></span> <span data-ttu-id="992d2-118">쿼리는 도메인 정보를 캡슐화하지 않는 DTO를 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-118">A query returns a DTO that does not encapsulate any domain knowledge.</span></span>

<span data-ttu-id="992d2-119">더 높은 격리 수준을 위해 쓰기 데이터에서 읽기 데이터를 물리적으로 구분할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-119">For greater isolation, you can physically separate the read data from the write data.</span></span> <span data-ttu-id="992d2-120">이 경우에 읽기 데이터베이스는 쿼리에 대해 최적화된 고유한 데이터 스키마를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-120">In that case, the read database can use its own data schema that is optimized for queries.</span></span> <span data-ttu-id="992d2-121">예를 들어 복잡한 조인이나 복잡한 O/RM 매핑을 방지하기 위해 데이터의 [구체화된 뷰][materialized-view]를 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-121">For example, it can store a [materialized view][materialized-view] of the data, in order to avoid complex joins or complex O/RM mappings.</span></span> <span data-ttu-id="992d2-122">다른 유형의 데이터 저장소도 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-122">It might even use a different type of data store.</span></span> <span data-ttu-id="992d2-123">예를 들어 쓰기 데이터베이스가 관계형일 수 있는 반면 읽기 데이터베이스는 문서 데이터베이스입니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-123">For example, the write database might be relational, while the read database is a document database.</span></span>

<span data-ttu-id="992d2-124">별도 읽기 및 쓰기 데이터베이스를 사용하는 경우 동기화를 유지해야 합니다. 일반적으로 데이터베이스를 업데이트할 때마다 쓰기 모델에서 이벤트를 게시함으로써 수행됩니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-124">If separate read and write databases are used, they must be kept in sync. Typically this is accomplished by having the write model publish an event whenever it updates the database.</span></span> <span data-ttu-id="992d2-125">데이터베이스를 업데이트하고 이벤트를 게시하는 작업은 단일 트랜잭션에서 이루어져야 합니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-125">Updating the database and publishing the event must occur in a single transaction.</span></span>

<span data-ttu-id="992d2-126">CQRS의 일부 구현에서는 [이벤트 소싱 패턴][event-sourcing]을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-126">Some implementations of CQRS use the [Event Sourcing pattern][event-sourcing].</span></span> <span data-ttu-id="992d2-127">이러한 패턴에서 애플리케이션 상태는 이벤트의 시퀀스로 저장됩니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-127">With this pattern, application state is stored as a sequence of events.</span></span> <span data-ttu-id="992d2-128">각 이벤트는 데이터에 대한 변경 집합을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-128">Each event represents a set of changes to the data.</span></span> <span data-ttu-id="992d2-129">현재 상태는 이벤트를 재생함으로써 구축됩니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-129">The current state is constructed by replaying the events.</span></span> <span data-ttu-id="992d2-130">CQRS 컨텍스트에서 이벤트 소싱의 이점 중 하나는 다른 구성 요소 &mdash;를 알리는 데 동일한 이벤트를 사용할 수 있다는 점입니다. 특히 읽기 모델에 알립니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-130">In a CQRS context, one benefit of Event Sourcing is that the same events can be used to notify other components &mdash; in particular, to notify the read model.</span></span> <span data-ttu-id="992d2-131">읽기 모델은 현재 상태의 스냅숏을 만드는 데 이벤트를 사용합니다. 이것이 쿼리에 보다 효과적입니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-131">The read model uses the events to create a snapshot of the current state, which is more efficient for queries.</span></span> <span data-ttu-id="992d2-132">그러나 이벤트 소싱은 디자인에 복잡성을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-132">However, Event Sourcing adds complexity to the design.</span></span>

![CQRS 이벤트](./images/cqrs-events.svg)

## <a name="when-to-use-this-architecture"></a><span data-ttu-id="992d2-134">이 아키텍처를 사용하는 경우</span><span class="sxs-lookup"><span data-stu-id="992d2-134">When to use this architecture</span></span>

<span data-ttu-id="992d2-135">읽기 및 쓰기 워크로드가 비대칭인 경우에 특히 많은 사용자가 동일한 데이터에 액세스하는 공동 작업 도메인에 CQRS를 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-135">Consider CQRS for collaborative domains where many users access the same data, especially when the read and write workloads are asymmetrical.</span></span>

<span data-ttu-id="992d2-136">CQRS는 전체 시스템에 적용되는 최상위 아키텍처가 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-136">CQRS is not a top-level architecture that applies to an entire system.</span></span> <span data-ttu-id="992d2-137">읽기 및 쓰기를 구분하는 명확한 값이 있는 해당 하위 시스템에만 CQRS를 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-137">Apply CQRS only to those subsystems where there is clear value in separating reads and writes.</span></span> <span data-ttu-id="992d2-138">그렇지 않으면 아무 이점 없이 복잡성만 추가하게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-138">Otherwise, you are creating additional complexity for no benefit.</span></span>

## <a name="benefits"></a><span data-ttu-id="992d2-139">이점</span><span class="sxs-lookup"><span data-stu-id="992d2-139">Benefits</span></span>

- <span data-ttu-id="992d2-140">**독립적인 크기 조정**</span><span class="sxs-lookup"><span data-stu-id="992d2-140">**Independently scaling**.</span></span> <span data-ttu-id="992d2-141">CQRS를 통해 읽기 및 쓰기 워크로드를 독립적으로 확장하고 더 적은 수의 잠금 경합이 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-141">CQRS allows the read and write workloads to scale independently, and may result in fewer lock contentions.</span></span>
- <span data-ttu-id="992d2-142">**최적화된 데이터 스키마**.</span><span class="sxs-lookup"><span data-stu-id="992d2-142">**Optimized data schemas**.</span></span> <span data-ttu-id="992d2-143">읽기 쪽에서는 쿼리에 최적화된 스키마를 사용하는 반면 쓰기 쪽에서는 업데이트에 최적화된 스키마를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-143">The read side can use a schema that is optimized for queries, while the write side uses a schema that is optimized for updates.</span></span>
- <span data-ttu-id="992d2-144">**보안**.</span><span class="sxs-lookup"><span data-stu-id="992d2-144">**Security**.</span></span> <span data-ttu-id="992d2-145">올바른 도메인 엔터티만 데이터에서 쓰기를 수행할 수 있는지 쉽게 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-145">It's easier to ensure that only the right domain entities are performing writes on the data.</span></span>
- <span data-ttu-id="992d2-146">**문제 구분**</span><span class="sxs-lookup"><span data-stu-id="992d2-146">**Separation of concerns**.</span></span> <span data-ttu-id="992d2-147">읽기 및 쓰기 쪽을 구분하면 유지 가능하고 유연한 모델을 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-147">Segregating the read and write sides can result in models that are more maintainable and flexible.</span></span> <span data-ttu-id="992d2-148">대부분의 복잡한 비즈니스 논리는 쓰기 모델로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-148">Most of the complex business logic goes into the write model.</span></span> <span data-ttu-id="992d2-149">읽기 모델은 상대적으로 간단할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-149">The read model can be relatively simple.</span></span>
- <span data-ttu-id="992d2-150">**단순한 쿼리**</span><span class="sxs-lookup"><span data-stu-id="992d2-150">**Simpler queries**.</span></span> <span data-ttu-id="992d2-151">읽기 데이터베이스에서 구체화된 뷰를 저장하여 쿼리할 때 애플리케이션은 복잡한 조인을 방지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-151">By storing a materialized view in the read database, the application can avoid complex joins when querying.</span></span>

## <a name="challenges"></a><span data-ttu-id="992d2-152">과제</span><span class="sxs-lookup"><span data-stu-id="992d2-152">Challenges</span></span>

- <span data-ttu-id="992d2-153">**복잡성**.</span><span class="sxs-lookup"><span data-stu-id="992d2-153">**Complexity**.</span></span> <span data-ttu-id="992d2-154">CQRS의 기본 개념은 간단합니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-154">The basic idea of CQRS is simple.</span></span> <span data-ttu-id="992d2-155">하지만 이벤트 소싱 패턴을 포함하는 경우에 특히 더 복잡한 애플리케이션 디자인을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-155">But it can lead to a more complex application design, especially if they include the Event Sourcing pattern.</span></span>

- <span data-ttu-id="992d2-156">**메시징**</span><span class="sxs-lookup"><span data-stu-id="992d2-156">**Messaging**.</span></span> <span data-ttu-id="992d2-157">CQRS에 메시징이 필요하지 않지만 명령을 처리하고 업데이트 이벤트를 게시하는 데 공통적으로 메시징을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-157">Although CQRS does not require messaging, it's common to use messaging to process commands and publish update events.</span></span> <span data-ttu-id="992d2-158">이 경우에 애플리케이션은 메시지 오류 또는 중복 메시지를 처리해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-158">In that case, the application must handle message failures or duplicate messages.</span></span>

- <span data-ttu-id="992d2-159">**결과적 일관성**</span><span class="sxs-lookup"><span data-stu-id="992d2-159">**Eventual consistency**.</span></span> <span data-ttu-id="992d2-160">읽기 및 쓰기 데이터베이스를 구분하는 경우 읽기 데이터는 기한이 경과되었을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-160">If you separate the read and write databases, the read data may be stale.</span></span>

## <a name="best-practices"></a><span data-ttu-id="992d2-161">모범 사례</span><span class="sxs-lookup"><span data-stu-id="992d2-161">Best practices</span></span>

- <span data-ttu-id="992d2-162">CQRS 구현에 대한 자세한 내용은 [CQRS 패턴][cqrs-pattern]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="992d2-162">For more information about implementing CQRS, see the [CQRS pattern][cqrs-pattern].</span></span>

- <span data-ttu-id="992d2-163">[이벤트 소싱][event-sourcing] 패턴을 사용하여 업데이트 충돌을 방지하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-163">Consider using the [Event Sourcing][event-sourcing] pattern to avoid update conflicts.</span></span>

- <span data-ttu-id="992d2-164">쿼리의 스키마를 최적화하기 위해 읽기 모델에 [구체화된 뷰 패턴][materialized-view]을 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-164">Consider using the [Materialized View pattern][materialized-view] for the read model, to optimize the schema for queries.</span></span>

## <a name="cqrs-in-microservices"></a><span data-ttu-id="992d2-165">마이크로 서비스의 CQRS</span><span class="sxs-lookup"><span data-stu-id="992d2-165">CQRS in microservices</span></span>

<span data-ttu-id="992d2-166">CQRS는 [마이크로 서비스 아키텍처][microservices]에서 특히 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-166">CQRS can be especially useful in a [microservices architecture][microservices].</span></span> <span data-ttu-id="992d2-167">마이크로 서비스의 원리 중 하나는 서비스가 다른 서비스의 데이터 저장소에 직접 액세스할 수 없다는 점입니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-167">One of the principles of microservices is that a service cannot directly access another service's data store.</span></span>

![마이크로서비스에 대한 잘못된 방법의 다이어그램](./images/cqrs-microservices-wrong.png)

<span data-ttu-id="992d2-169">다음 다이어그램에서 서비스 A는 데이터 저장소에 기록하고 서비스 B는 데이터의 구체화된 뷰를 유지합니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-169">In the following diagram, Service A writes to a data store, and Service B keeps a materialized view of the data.</span></span> <span data-ttu-id="992d2-170">서비스 A는 데이터 저장소에 기록할 때마다 이벤트를 게시합니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-170">Service A publishes an event whenever it writes to the data store.</span></span> <span data-ttu-id="992d2-171">서비스 B는 이벤트를 구독합니다.</span><span class="sxs-lookup"><span data-stu-id="992d2-171">Service B subscribes to the event.</span></span>

![마이크로서비스에 대한 올바른 방법의 다이어그램](./images/cqrs-microservices-right.png)

<!-- links -->

[cqrs-pattern]: ../../patterns/cqrs.md
[event-sourcing]: ../../patterns/event-sourcing.md
[materialized-view]: ../../patterns/materialized-view.md
[microservices]: ./microservices.md
