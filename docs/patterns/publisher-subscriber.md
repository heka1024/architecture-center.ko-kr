---
title: 게시자-구독자 패턴
description: 애플리케이션이 여러 관심있는 소비자에게 이벤트를 비동기식으로 알릴 수 있습니다.
keywords: 디자인 패턴
author: alexbuckgit
ms.date: 12/07/2018
ms.topic: design-pattern
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.openlocfilehash: 9b931337f7f0e5dc58f83701271c7d3491af5bfd
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58248748"
---
# <a name="publisher-subscriber-pattern"></a><span data-ttu-id="026cf-104">게시자-구독자 패턴</span><span class="sxs-lookup"><span data-stu-id="026cf-104">Publisher-Subscriber pattern</span></span>

<span data-ttu-id="026cf-105">애플리케이선이 발신자를 수신자에게 연결하지 않고 여러 관심있는 소비자에게 이벤트를 비동기식으로 알릴 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-105">Enable an application to announce events to multiple interested consumers aynchronously, without coupling the senders to the receivers.</span></span>

<span data-ttu-id="026cf-106">**또 다른 명칭**: 게시/구독 메시지</span><span class="sxs-lookup"><span data-stu-id="026cf-106">**Also called**: Pub/sub messaging</span></span>

## <a name="context-and-problem"></a><span data-ttu-id="026cf-107">컨텍스트 및 문제점</span><span class="sxs-lookup"><span data-stu-id="026cf-107">Context and problem</span></span>

<span data-ttu-id="026cf-108">클라우드 기반 및 분산 애플리케이션에서는 이벤트가 발생하면 종종 시스템 구성 요소가 다른 구성 요소에 정보를 제공해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-108">In cloud-based and distributed applications, components of the system often need to provide information to other components as events happen.</span></span>

<span data-ttu-id="026cf-109">비동기 메시지는 발신자를 소비자로부터 분리하고, 발신자가 응답을 기다리지 않게 해주는 효과적인 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-109">Asynchronous messaging is an effective way to decouple senders from consumers, and avoid blocking the sender to wait for a response.</span></span> <span data-ttu-id="026cf-110">그러나 각 소비자에게 전용 메시지 큐를 사용하면 여러 소비자에 맞게 효과적으로 크기가 조정되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-110">However, using a dedicated message queue for each consumer does not effectively scale to many consumers.</span></span> <span data-ttu-id="026cf-111">또한 일부 소비자는 특정 정보에만 관심이 있을 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-111">Also, some of the consumers might be interested in only a subset of the information.</span></span> <span data-ttu-id="026cf-112">발신자가 소비자의 ID를 모르더라도 관심이 있는 모든 소비자에게 이벤트를 알리려면 어떻게 해야 할까요?</span><span class="sxs-lookup"><span data-stu-id="026cf-112">How can the sender announce events to all interested consumers without knowing their identities?</span></span>

## <a name="solution"></a><span data-ttu-id="026cf-113">해결 방법</span><span class="sxs-lookup"><span data-stu-id="026cf-113">Solution</span></span>

<span data-ttu-id="026cf-114">다음을 포함하는 비동기 메시지 하위 시스템을 도입합니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-114">Introduce an asynchronous messaging subsystem that includes the following:</span></span>

- <span data-ttu-id="026cf-115">발신자가 사용하는 입력 메시지 채널.</span><span class="sxs-lookup"><span data-stu-id="026cf-115">An input messaging channel used by the sender.</span></span> <span data-ttu-id="026cf-116">발신자는 알려진 메시지 형식을 사용하여 이벤트를 메시지에 패키징하고, 입력 채널을 통해 이러한 메시지를 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-116">The sender packages events into messages, using a known message format, and sends these messages via the input channel.</span></span> <span data-ttu-id="026cf-117">이 패턴의 발신자를 *게시자*라고도 합니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-117">The sender in this pattern is also called the *publisher*.</span></span>

  > [!NOTE]
  > <span data-ttu-id="026cf-118">*메시지*는 데이터 패킷입니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-118">A *message* is a packet of data.</span></span> <span data-ttu-id="026cf-119">*이벤트*는 발생한 변경 내용 또는 작업에 대해 다른 구성 요소에 알리는 메시지입니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-119">An *event* is a message that notifies other components about a change or an action that has taken place.</span></span>

- <span data-ttu-id="026cf-120">소비자마다 출력 메시지 채널이 하나씩 있습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-120">One output messaging channel per consumer.</span></span> <span data-ttu-id="026cf-121">소비자를 *구독자*라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-121">The consumers are known as *subscribers*.</span></span>

- <span data-ttu-id="026cf-122">입력 채널의 각 메시지를 해당 메시지에 관심이 있는 모든 구독자의 출력 채널로 복사하는 메커니즘입니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-122">A mechanism for copying each message from the input channel to the output channels for all subscribers interested in that message.</span></span> <span data-ttu-id="026cf-123">이 작업은 일반적으로 메시지 브로커 또는 이벤트 버스 같은 중간자가 처리합니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-123">This operation is typically handled by a intermediary such as a message broker or event bus.</span></span>

<span data-ttu-id="026cf-124">다음 다이어그램은 이 패턴의 논리적 구성 요소를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-124">The following diagram shows the logical components of this pattern:</span></span>

![메시지 브로커를 사용하는 게시-구독 패턴](./_images/publish-subscribe.png)
 
<span data-ttu-id="026cf-126">게시/구독 메시지에는 다음과 같은 혜택이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-126">Pub/sub messaging has the following benefits:</span></span>

- <span data-ttu-id="026cf-127">여전히 통신이 필요한 하위 시스템을 분리합니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-127">It decouples subsystems that still need to communicate.</span></span> <span data-ttu-id="026cf-128">하위 시스템을 독립적으로 관리할 수 있으며, 하나 이상의 수신기가 오프라인이어도 메시지를 올바르게 관리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-128">Subsystems can be managed independently, and messages can be properly managed even if one or more receivers are offline.</span></span>

- <span data-ttu-id="026cf-129">확장성이 향상되고 발신자의 응답성이 개선됩니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-129">It increases scalability and improves responsiveness of the sender.</span></span> <span data-ttu-id="026cf-130">발신자는 신속하게 단일 메시지를 입력 채널로 보낸 다음, 핵심 처리 업무로 돌아갈 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-130">The sender can quickly send a single message to the input channel, then return to its core processing responsibilities.</span></span> <span data-ttu-id="026cf-131">메시지 인프라는 관심이 있는 구독자에게 메시지를 전달하는 책임을 맡습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-131">The messaging infrastructure is responsible for ensuring messages are delivered to interested subscribers.</span></span>

- <span data-ttu-id="026cf-132">안정성을 향상시킵니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-132">It improves reliability.</span></span> <span data-ttu-id="026cf-133">비동기 메시지는 증가된 부하 하에서도 애플리케이션이 계속해서 원활하게 실행되고 일시적 오류를 보다 효과적으로 처리할 수 있게 도와줍니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-133">Asynchronous messaging helps applications continue to run smoothly under increased loads and handle intermittent failures more effectively.</span></span>

- <span data-ttu-id="026cf-134">지연된 또는 예약된 처리를 허용합니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-134">It allows for deferred or scheduled processing.</span></span> <span data-ttu-id="026cf-135">구독자는 사용량이 적은 시간까지 메시지 선택을 미룰 수 있으며, 특정 일정에 따라 메시지를 라우팅 또는 처리할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-135">Subscribers can wait to pick up messages until off-peak hours, or messages can be routed or processed according to a specific schedule.</span></span>

- <span data-ttu-id="026cf-136">여러 플랫폼, 프로그래밍 언어 또는 통신 프로토콜을 사용하여 시스템을 간단하게 통합할 수 있을 뿐 아니라 온-프레미스 시스템과 클라우드에서 실행 중인 애플리케이션 간의 통합도 가능합니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-136">It enables simpler integration between systems using different platforms, programming languages, or communication protocols, as well as between on-premises systems and applications running in the cloud.</span></span>

- <span data-ttu-id="026cf-137">기업 전체에서 비동기 워크플로를 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-137">It facilitates asynchronous workflows across an enterprise.</span></span>

- <span data-ttu-id="026cf-138">테스트 가능성이 향상됩니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-138">It improves testability.</span></span> <span data-ttu-id="026cf-139">전체 통합 테스트 전략의 일부로 채널을 모니터링하고 메시지를 검사 또는 기록할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-139">Channels can be monitored and messages can be inspected or logged as part of an overall integration test strategy.</span></span>

- <span data-ttu-id="026cf-140">애플리케이션의 문제를 분리합니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-140">It provides separation of concerns for your applications.</span></span> <span data-ttu-id="026cf-141">각 애플리케이션은 핵심 기능에 집중할 수 있고, 메시지 인프라는 메시지를 여러 소비자에게 안정적으로 라우팅하는 데 필요한 모든 것을 처리합니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-141">Each application can focus on its core capabilities, while the messaging infrastructure handles everything required to reliably route messages to multiple consumers.</span></span> 

## <a name="issues-and-considerations"></a><span data-ttu-id="026cf-142">문제 및 고려 사항</span><span class="sxs-lookup"><span data-stu-id="026cf-142">Issues and considerations</span></span>

<span data-ttu-id="026cf-143">이 패턴을 구현할 방법을 결정할 때 다음 사항을 고려하세요.</span><span class="sxs-lookup"><span data-stu-id="026cf-143">Consider the following points when deciding how to implement this pattern:</span></span>

- <span data-ttu-id="026cf-144">**기존 기술.**</span><span class="sxs-lookup"><span data-stu-id="026cf-144">**Existing technologies.**</span></span> <span data-ttu-id="026cf-145">게시-구독 모델을 지원하는 시중의 메시지 제품 및 서비스를 사용할 것을 강력하게 권장합니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-145">It is strongly recommended to use available messaging products and services that support a publish-subscribe model, rather than building your own.</span></span> <span data-ttu-id="026cf-146">Azure에서는 [Service Bus](/azure/service-bus-messaging/) 또는 [Event Grid](/azure/event-grid/)를 사용하는 방안을 고려해 보세요.</span><span class="sxs-lookup"><span data-stu-id="026cf-146">In Azure, consider using [Service Bus](/azure/service-bus-messaging/) or [Event Grid](/azure/event-grid/).</span></span> <span data-ttu-id="026cf-147">게시/구독 메시지에 사용할 수 있는 다른 기술로는 Redis, RabbitMQ 및 Apache Kafka가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-147">Other technologies that can be used for pub/sub messaging include Redis, RabbitMQ, and Apache Kafka.</span></span>

- <span data-ttu-id="026cf-148">**구독 처리.**</span><span class="sxs-lookup"><span data-stu-id="026cf-148">**Subscription handling.**</span></span> <span data-ttu-id="026cf-149">메시지 인프라는 소비자가 사용 가능한 채널에서 구독 또는 구독 취소할 수 있는 메커니즘을 제공해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-149">The messaging infrastructure must provide mechanisms that consumers can use to subscribe to or unsubscribe from available channels.</span></span>

- <span data-ttu-id="026cf-150">**보안.**</span><span class="sxs-lookup"><span data-stu-id="026cf-150">**Security.**</span></span> <span data-ttu-id="026cf-151">권한이 없는 사용자 또는 애플리케이션이 도청할 수 없도록 보안 정책을 통해 모든 메시지 채널에 대한 연결을 제한해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-151">Connecting to any message channel must be restricted by security policy to prevent eavesdropping by unauthorized users or applications.</span></span>

- <span data-ttu-id="026cf-152">**메시지 하위 집합.**</span><span class="sxs-lookup"><span data-stu-id="026cf-152">**Subsets of messages.**</span></span> <span data-ttu-id="026cf-153">구독자는 일반적으로 게시자가 배포하는 메시지 하위 집합에만 관심이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-153">Subscribers are usually only interested in subset of the messages distributed by a publisher.</span></span> <span data-ttu-id="026cf-154">메시지 서비스는 종종 구독자가 다음 방법을 통해 수신 메시지의 범위를 좁힐 수 있도록 허용합니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-154">Messaging services often allow subscribers to narrow the set of messages received by:</span></span>

  - <span data-ttu-id="026cf-155">**토픽.**</span><span class="sxs-lookup"><span data-stu-id="026cf-155">**Topics.**</span></span> <span data-ttu-id="026cf-156">각 토픽에는 전용 출력 채널이 있으며, 각 소비자는 모든 관련 토픽을 구독할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-156">Each topic has a dedicated output channel, and each consumer can subscribe to all relevant topics.</span></span>
  - <span data-ttu-id="026cf-157">**콘텐츠 필터링.**</span><span class="sxs-lookup"><span data-stu-id="026cf-157">**Content filtering.**</span></span> <span data-ttu-id="026cf-158">각 메시지의 콘텐츠에 따라 메시지를 검사하고 분산합니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-158">Messages are inspected and distributed based on the content of each message.</span></span> <span data-ttu-id="026cf-159">각 구독자는 관심이 있는 콘텐츠를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-159">Each subscriber can specify the content it is interested in.</span></span>

- <span data-ttu-id="026cf-160">**와일드카드 구독자.**</span><span class="sxs-lookup"><span data-stu-id="026cf-160">**Wildcard subscribers.**</span></span> <span data-ttu-id="026cf-161">구독자가 와일드카드를 통해 여러 토픽을 구독하는 것을 허용하는 방안을 고려해 보세요.</span><span class="sxs-lookup"><span data-stu-id="026cf-161">Consider allowing subscribers to subscribe to multiple topics via wildcards.</span></span>

- <span data-ttu-id="026cf-162">**양방향 통신.**</span><span class="sxs-lookup"><span data-stu-id="026cf-162">**Bi-directional communication.**</span></span> <span data-ttu-id="026cf-163">게시-구독 시스템의 채널은 단방향으로 취급됩니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-163">The channels in a publish-subscribe system are treated as unidirectional.</span></span> <span data-ttu-id="026cf-164">특정 구독자가 게시자에게 확인 메시지를 보내거나 상태 정보를 전달해야 하는 경우 [요청/회신 패턴](http://www.enterpriseintegrationpatterns.com/patterns/messaging/RequestReply.html)을 고려해 보세요.</span><span class="sxs-lookup"><span data-stu-id="026cf-164">If a specific subscriber needs to send acknowledgement or communicate status back to the publisher, consider using the [Request/Reply Pattern](http://www.enterpriseintegrationpatterns.com/patterns/messaging/RequestReply.html).</span></span> <span data-ttu-id="026cf-165">이 패턴은 구독자에게 메시지를 보내는 데 한 채널을 사용하고, 게시자와 통신하는 데 별도의 회신 채널을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-165">This pattern uses one channel to send a message to the subscriber, and a separate reply channel for communicating back to the publisher.</span></span>

- <span data-ttu-id="026cf-166">**메시지 정렬.**</span><span class="sxs-lookup"><span data-stu-id="026cf-166">**Message ordering.**</span></span> <span data-ttu-id="026cf-167">소비자 인스턴스가 메시지를 수신하는 순서는 확정되지 않으며 메시지가 만들어진 순서를 반드시 반영하는 것은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-167">The order in which consumer instances receive messages isn't guaranteed, and doesn't necessarily reflect the order in which the messages were created.</span></span> <span data-ttu-id="026cf-168">메시지 처리 순서에 대한 의존성을 제거할 수 있도록 메시지 처리가 멱등적인 시스템을 디자인하세요.</span><span class="sxs-lookup"><span data-stu-id="026cf-168">Design the system to ensure that message processing is idempotent to help eliminate any dependency on the order of message handling.</span></span>

- <span data-ttu-id="026cf-169">**메시지 우선 순위.**</span><span class="sxs-lookup"><span data-stu-id="026cf-169">**Message priority.**</span></span> <span data-ttu-id="026cf-170">일부 솔루션은 메시지를 특정 순서대로 처리해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-170">Some solutions may require that messages are processed in a specific order.</span></span> <span data-ttu-id="026cf-171">[우선 순위 큐 패턴](priority-queue.md)은 특정 메시지를 다른 메시지보다 먼저 전달하는 메커니즘을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-171">The [Priority Queue pattern](priority-queue.md) provides a mechanism for ensuring specific messages are delivered before others.</span></span>

- <span data-ttu-id="026cf-172">**포이즌 메시지.**</span><span class="sxs-lookup"><span data-stu-id="026cf-172">**Poison messages.**</span></span> <span data-ttu-id="026cf-173">잘못된 형식의 메시지 또는 사용할 수 없는 리소스에 액세스를 요구하는 작업은 서비스 인스턴스의 실패를 초래할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-173">A malformed message, or a task that requires access to resources that aren't available, can cause a service instance to fail.</span></span> <span data-ttu-id="026cf-174">시스템에서는 메시지가 큐로 반환되는 것을 막아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-174">The system should prevent such messages being returned to the queue.</span></span> <span data-ttu-id="026cf-175">그 대신, 필요한 경우 이러한 메시지를 분석할 수 있도록 메시지 세부 정보를 캡처하여 다른 곳에 저장하세요.</span><span class="sxs-lookup"><span data-stu-id="026cf-175">Instead, capture and store the details of these messages elsewhere so that they can be analyzed if necessary.</span></span>

- <span data-ttu-id="026cf-176">**반복 메시지.**</span><span class="sxs-lookup"><span data-stu-id="026cf-176">**Repeated messages.**</span></span> <span data-ttu-id="026cf-177">같은 메시지를 여러 번 보낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-177">The same message might be sent more than once.</span></span> <span data-ttu-id="026cf-178">예를 들어 발신자가 메시지를 게시한 후 실패할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-178">For example, the sender might fail after posting a message.</span></span> <span data-ttu-id="026cf-179">그 후 발신자의 새 인스턴스가 시작되어 메시지를 반복할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-179">Then a new instance of the sender might start up and repeat the message.</span></span> <span data-ttu-id="026cf-180">메시지 인프라는 메시지를 최대 1회(at-most-once) 제공할 수 있도록 메시지 ID를 기반으로 중복 메시지 감지 및 제거(중복 제거라고도 함)를 구현해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-180">The messaging infrastructure should implement duplicate message detection and removal (also known as de-duping) based on message IDs in order to provide at-most-once delivery of messages.</span></span>

- <span data-ttu-id="026cf-181">**메시지 만료.**</span><span class="sxs-lookup"><span data-stu-id="026cf-181">**Message expiration.**</span></span> <span data-ttu-id="026cf-182">메시지 수명이 제한되어 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-182">A message might have a limited lifetime.</span></span> <span data-ttu-id="026cf-183">이 기간 내에 처리되지 않는 메시지는 더 이상 관련이 없으므로 삭제해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-183">If it isn't processed within this period, it might no longer be relevant and should be discarded.</span></span> <span data-ttu-id="026cf-184">발신자는 메시지의 데이터 중 일부로 만료 시간을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-184">A sender can specify an experiation time as part of the data in the message.</span></span> <span data-ttu-id="026cf-185">수신자는 이 정보를 검사한 후 메시지와 연결된 비즈니스 논리를 수행할 것인지 결정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-185">A receiver can examine this information before deciding whether to perform the business logic associated with the message.</span></span>

- <span data-ttu-id="026cf-186">**메시지 예약.**</span><span class="sxs-lookup"><span data-stu-id="026cf-186">**Message scheduling.**</span></span> <span data-ttu-id="026cf-187">메시지에 일시적으로 엠바고가 내려져 특정 날짜 및 시간까지 처리하면 안 되는 경우가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-187">A message might be temporarily embargoed and should not be processed until a specific date and time.</span></span> <span data-ttu-id="026cf-188">이 시간까지는 수신자에게 메시지를 제공하면 안 됩니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-188">The message should not be available to a receiver until this time.</span></span>

## <a name="when-to-use-this-pattern"></a><span data-ttu-id="026cf-189">이 패턴을 사용해야 하는 경우</span><span class="sxs-lookup"><span data-stu-id="026cf-189">When to use this pattern</span></span>

<span data-ttu-id="026cf-190">다음 경우에 이 패턴을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-190">Use this pattern when:</span></span>

- <span data-ttu-id="026cf-191">애플리케이션이 수많은 소비자에게 정보를 브로드캐스트해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-191">An application needs to broadcast information to a significant number of consumers.</span></span>

- <span data-ttu-id="026cf-192">애플리케이션이 독립적으로 개발된 하나 이상의 애플리케이션 또는 서비스와 통신해야 하며, 이러한 애플리케이션 또는 서비스는 다양한 플랫폼, 프로그래밍 언어 및 통신 프로토콜을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-192">An application needs to communicate with one or more independently-developed applications or services, which may use different platforms, programming languages, and communication protocols.</span></span>

- <span data-ttu-id="026cf-193">애플리케이션이 소비자의 실시간 응답을 요구하지 않고도 소비자에게 정보를 보낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-193">An application can send information to consumers without requiring real-time responses from the consumers.</span></span>

- <span data-ttu-id="026cf-194">통합되는 시스템은 데이터에 대한 결과적 일관성 모델을 지원하도록 설계됩니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-194">The systems being integrated are designed to support an eventual consistency model for their data.</span></span>

- <span data-ttu-id="026cf-195">애플리케이션이 여러 소비자에게 정보를 전달해야 하며, 소비자의 가용성 요구 사항 또는 가동 시간 일정이 발신자와 다를 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-195">An application needs to communicate information to multiple consumers, which may have different availability requirements or uptime schedules than the sender.</span></span>

<span data-ttu-id="026cf-196">다음의 경우에는 이 패턴이 유용하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-196">This pattern might not be useful when:</span></span>

- <span data-ttu-id="026cf-197">애플리케이션의 소비자가 소수이며 소비자가 요구하는 정보가 생산 애플리케이션과 매우 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-197">An application has only a few consumers who need significantly different information from the producing application.</span></span>

- <span data-ttu-id="026cf-198">애플리케이션이 소비자와 거의 실시간으로 상호 작용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-198">An application requires near real-time interaction with consumers.</span></span>

## <a name="example"></a><span data-ttu-id="026cf-199">예</span><span class="sxs-lookup"><span data-stu-id="026cf-199">Example</span></span>

<span data-ttu-id="026cf-200">다음 다이어그램은 Service Bus를 사용하여 워크플로를 조정하고 Event Grid를 사용하여 하위 시스템에 발생하는 이벤트에 대해 알리는 엔터프라이즈 통합 아키텍처를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-200">The following diagram shows an enterprise integration architecture that uses Service Bus to coordinate workflows, and Event Grid notify subsystems of events that occur.</span></span> <span data-ttu-id="026cf-201">자세한 내용은 [Azure에서 메시지 큐 및 이벤트를 사용하여 엔터프라이즈 통합](../reference-architectures/enterprise-integration/queues-events.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="026cf-201">For more information, see [Enterprise integration on Azure using message queues and events](../reference-architectures/enterprise-integration/queues-events.md).</span></span>

![엔터프라이즈 통합 아키텍처](../reference-architectures/enterprise-integration/_images/enterprise-integration-queues-events.png)

## <a name="related-patterns-and-guidance"></a><span data-ttu-id="026cf-203">관련 패턴 및 지침</span><span class="sxs-lookup"><span data-stu-id="026cf-203">Related patterns and guidance</span></span>

<span data-ttu-id="026cf-204">이 패턴을 구현할 때 다음 패턴 및 지침도 관련이 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-204">The following patterns and guidance might be relevant when implementing this pattern:</span></span>

- <span data-ttu-id="026cf-205">[메시지를 전송하는 Azure 서비스 중에서 선택](/azure/event-grid/compare-messaging-services)</span><span class="sxs-lookup"><span data-stu-id="026cf-205">[Choose between Azure services that deliver messages](/azure/event-grid/compare-messaging-services).</span></span>

- <span data-ttu-id="026cf-206">[이벤트 기반 아키텍처 스타일](../guide/architecture-styles/event-driven.md)은 게시/구독 메시지를 사용하는 아키텍처 스타일입니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-206">The [Event-driven architecture style](../guide/architecture-styles/event-driven.md) is an architecture style that uses pub/sub messaging.</span></span>

- <span data-ttu-id="026cf-207">[비동기 메시징 입문](https://msdn.microsoft.com/library/dn589781.aspx).</span><span class="sxs-lookup"><span data-stu-id="026cf-207">[Asynchronous Messaging Primer](https://msdn.microsoft.com/library/dn589781.aspx).</span></span> <span data-ttu-id="026cf-208">메시지 큐는 비동기 통신 메커니즘입니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-208">Message queues are an asynchronous communications mechanism.</span></span> <span data-ttu-id="026cf-209">소비자 서비스가 회신을 애플리케이션에 전송해야 하는 경우 특정 형태의 응답 메시징을 구현해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-209">If a consumer service needs to send a reply to an application, it might be necessary to implement some form of response messaging.</span></span> <span data-ttu-id="026cf-210">비동기 메시징 입문서에서는 메시지 큐를 사용하여 요청/회신 메시징을 구현하는 방법에 대한 정보를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-210">The Asynchronous Messaging Primer provides information on how to implement request/reply messaging using message queues.</span></span>

- <span data-ttu-id="026cf-211">[관찰자 패턴](https://en.wikipedia.org/wiki/Observer_pattern).</span><span class="sxs-lookup"><span data-stu-id="026cf-211">[Observer Pattern](https://en.wikipedia.org/wiki/Observer_pattern).</span></span> <span data-ttu-id="026cf-212">게시-구독 패턴은 비동기 메시지를 통해 관찰자를 주체로부터 분리하여 관찰자 패턴을 기반으로 구현됩니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-212">The Publish-Subscribe pattern builds on the Observer pattern by decoupling subjects from observers via asynchronous messaging.</span></span>

- <span data-ttu-id="026cf-213">[메시지 브로커 패턴](https://en.wikipedia.org/wiki/Message_broker).</span><span class="sxs-lookup"><span data-stu-id="026cf-213">[Message Broker Pattern](https://en.wikipedia.org/wiki/Message_broker).</span></span> <span data-ttu-id="026cf-214">게시-구독 모델을 지원하는 많은 메시지 하위 시스템은 메시지 브로커를 통해 구현됩니다.</span><span class="sxs-lookup"><span data-stu-id="026cf-214">Many messaging subsystems that support a publish-subscribe modek are implemented via a message broker.</span></span>
