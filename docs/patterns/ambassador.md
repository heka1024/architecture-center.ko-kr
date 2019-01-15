---
title: 특사 패턴
titleSuffix: Cloud Design Patterns
description: 소비자 서비스 또는 애플리케이션을 대신하여 네트워크 요청을 전송하는 도우미 서비스를 만듭니다.
keywords: 디자인 패턴
author: dragon119
ms.date: 06/23/2017
ms.custom: seodec18
ms.openlocfilehash: f03bfa0b45494ac1428aeee5cc6c413d5607ba79
ms.sourcegitcommit: 680c9cef945dff6fee5e66b38e24f07804510fa9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54009783"
---
# <a name="ambassador-pattern"></a><span data-ttu-id="59d0b-104">특사 패턴</span><span class="sxs-lookup"><span data-stu-id="59d0b-104">Ambassador pattern</span></span>

<span data-ttu-id="59d0b-105">소비자 서비스 또는 애플리케이션을 대신하여 네트워크 요청을 전송하는 도우미 서비스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-105">Create helper services that send network requests on behalf of a consumer service or application.</span></span> <span data-ttu-id="59d0b-106">특사 서비스는 클라이언트와 함께 배치하는 Out of Process 프록시로 간주할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-106">An ambassador service can be thought of as an out-of-process proxy that is co-located with the client.</span></span>

<span data-ttu-id="59d0b-107">이 패턴은 모니터링, 로깅, 라우팅, 보안(예: TLS) 및 언어 독립적 방식의 [복원력 패턴][resiliency-patterns] 등의 일반적인 클라이언트 연결 작업의 오프로딩에 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-107">This pattern can be useful for offloading common client connectivity tasks such as monitoring, logging, routing, security (such as TLS), and [resiliency patterns][resiliency-patterns] in a language agnostic way.</span></span> <span data-ttu-id="59d0b-108">레거시 애플리케이션 또는 수정하기 어려운 다른 애플리케이션에서 네트워킹 기능을 확장하기 위해 이 패턴을 사용하는 경우가 많습니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-108">It is often used with legacy applications, or other applications that are difficult to modify, in order to extend their networking capabilities.</span></span> <span data-ttu-id="59d0b-109">특수 팀을 통해 이러한 기능을 구현할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-109">It can also enable a specialized team to implement those features.</span></span>

## <a name="context-and-problem"></a><span data-ttu-id="59d0b-110">컨텍스트 및 문제점</span><span class="sxs-lookup"><span data-stu-id="59d0b-110">Context and problem</span></span>

<span data-ttu-id="59d0b-111">복원력 있는 클라우드 기반 애플리케이션을 사용하려면 [회로 차단](./circuit-breaker.md), 라우팅, 측정 및 모니터링 등의 기능과 네트워크 관련 구성을 업데이트하는 기능이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-111">Resilient cloud-based applications require features such as [circuit breaking](./circuit-breaker.md), routing, metering and monitoring, and the ability to make network-related configuration updates.</span></span> <span data-ttu-id="59d0b-112">이러한 기능을 추가하기 위해 레거시 애플리케이션 또는 기존 코드 라이브러리를 업데이트하기는 어렵거나 불가능할 수 있습니다. 개발 팀에서 더 이상 코드를 쉽게 수정하거나 유지 관리할 수 없기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-112">It may be difficult or impossible to update legacy applications or existing code libraries to add these features, because the code is no longer maintained or can't be easily modified by the development team.</span></span>

<span data-ttu-id="59d0b-113">네트워크 호출을 위해 연결, 인증 및 권한 부여에 대한 상당한 구성이 필요할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-113">Network calls may also require substantial configuration for connection, authentication, and authorization.</span></span> <span data-ttu-id="59d0b-114">여러 언어 및 프레임워크를 사용하여 작성된 여러 애플리케이션에서 이러한 호출을 사용할 경우 이러한 인스턴스 각각에 대해 호출을 구성해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-114">If these calls are used across multiple applications, built using multiple languages and frameworks, the calls must be configured for each of these instances.</span></span> <span data-ttu-id="59d0b-115">또한 조직 내의 중앙 팀에서 네트워크 및 보안 기능을 관리해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-115">In addition, network and security functionality may need to be managed by a central team within your organization.</span></span> <span data-ttu-id="59d0b-116">대규모 코드 기반에서는 해당 팀이 친숙하지 않은 애플리케이션 코드를 업데이트하는 것이 위험할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-116">With a large code base, it can be risky for that team to update application code they aren't familiar with.</span></span>

## <a name="solution"></a><span data-ttu-id="59d0b-117">해결 방법</span><span class="sxs-lookup"><span data-stu-id="59d0b-117">Solution</span></span>

<span data-ttu-id="59d0b-118">클라이언트 프레임워크 및 라이브러리를 애플리케이션과 외부 서비스 간의 프록시 역할을 하는 외부 프로세스에 배치합니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-118">Put client frameworks and libraries into an external process that acts as a proxy between your application and external services.</span></span> <span data-ttu-id="59d0b-119">라우팅, 복원력, 보안 기능에 대한 제어를 허용하고 모든 호스트 관련 액세스 제한을 방지하기 위해 애플리케이션과 동일한 호스트 환경에서 프록시를 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-119">Deploy the proxy on the same host environment as your application to allow control over routing, resiliency, security features, and to avoid any host-related access restrictions.</span></span> <span data-ttu-id="59d0b-120">또한 특사 패턴을 사용하여 계측을 확장하고 표준화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-120">You can also use the ambassador pattern to standardize and extend instrumentation.</span></span> <span data-ttu-id="59d0b-121">프록시는 대기 시간 또는 리소스 사용량과 같은 성능 메트릭을 모니터링할 수 있으며 이 모니터링은 애플리케이션과 동일한 호스트 환경에서 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-121">The proxy can monitor performance metrics such as latency or resource usage, and this monitoring happens in the same host environment as the application.</span></span>

![특사 패턴의 다이어그램](./_images/ambassador.png)

<span data-ttu-id="59d0b-123">특사에 오프로드되는 기능은 애플리케이션과 독립적으로 관리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-123">Features that are offloaded to the ambassador can be managed independently of the application.</span></span> <span data-ttu-id="59d0b-124">애플리케이션의 레거시 기능을 방해하지 않고 특사를 업데이트 및 수정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-124">You can update and modify the ambassador without disturbing the application's legacy functionality.</span></span> <span data-ttu-id="59d0b-125">또한 별도의 특수 팀에서 특사로 이동된 보안, 네트워킹 또는 인증 기능을 구현하고 유지 관리할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-125">It also allows for separate, specialized teams to implement and maintain security, networking, or authentication features that have been moved to the ambassador.</span></span>

<span data-ttu-id="59d0b-126">특사 서비스를 [사이드카](./sidecar.md)로 배포하여 사용 중인 애플리케이션 또는 서비스의 수명 주기와 함께 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-126">Ambassador services can be deployed as a [sidecar](./sidecar.md) to accompany the lifecycle of a consuming application or service.</span></span> <span data-ttu-id="59d0b-127">또는 특사는 일반 호스트의 여러 개별 프로세스에서 공유되는 경우 디먼 또는 Windows 서비스로 배포될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-127">Alternatively, if an ambassador is shared by multiple separate processes on a common host, it can be deployed as a daemon or Windows service.</span></span> <span data-ttu-id="59d0b-128">사용 중인 서비스가 컨테이너화된 경우 통신을 위해 구성된 적절한 링크를 사용하여 동일한 호스트에 특사를 별도의 컨테이너로 만들어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-128">If the consuming service is containerized, the ambassador should be created as a separate container on the same host, with the appropriate links configured for communication.</span></span>

## <a name="issues-and-considerations"></a><span data-ttu-id="59d0b-129">문제 및 고려 사항</span><span class="sxs-lookup"><span data-stu-id="59d0b-129">Issues and considerations</span></span>

- <span data-ttu-id="59d0b-130">프록시는 약간의 대기 시간 오버헤드를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-130">The proxy adds some latency overhead.</span></span> <span data-ttu-id="59d0b-131">애플리케이션에서 직접 호출하는 클라이언트 라이브러리가 더 좋은 방법인지 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-131">Consider whether a client library, invoked directly by the application, is a better approach.</span></span>
- <span data-ttu-id="59d0b-132">일반화된 기능을 프록시에 포함할 때 미칠 수 있는 영향을 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-132">Consider the possible impact of including generalized features in the proxy.</span></span> <span data-ttu-id="59d0b-133">예를 들어 특사는 재시도를 처리할 수 있지만 모든 작업이 idempotent가 아닌 이상 안전하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-133">For example, the ambassador could handle retries, but that might not be safe unless all operations are idempotent.</span></span>
- <span data-ttu-id="59d0b-134">클라이언트와 프록시 간에 일부 컨텍스트를 전달할 수 있도록 하는 메커니즘을 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-134">Consider a mechanism to allow the client to pass some context to the proxy, as well as back to the client.</span></span> <span data-ttu-id="59d0b-135">예를 들어 HTTP 요청 헤더를 포함하여 재시도를 취소하거나 최대 재시도 횟수를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-135">For example, include HTTP request headers to opt out of retry or specify the maximum number of times to retry.</span></span>
- <span data-ttu-id="59d0b-136">프록시를 패키지하고 배포하는 방법을 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-136">Consider how you will package and deploy the proxy.</span></span>
- <span data-ttu-id="59d0b-137">모든 클라이언트에 단일 공유 인스턴스를 사용할지 또는 각 클라이언트에 하나의 인스턴스를 사용할지 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-137">Consider whether to use a single shared instance for all clients or an instance for each client.</span></span>

## <a name="when-to-use-this-pattern"></a><span data-ttu-id="59d0b-138">이 패턴을 사용해야 하는 경우</span><span class="sxs-lookup"><span data-stu-id="59d0b-138">When to use this pattern</span></span>

<span data-ttu-id="59d0b-139">다음 경우에 이 패턴을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-139">Use this pattern when you:</span></span>

- <span data-ttu-id="59d0b-140">여러 언어 또는 프레임워크에 대한 클라이언트 연결 기능의 공통 집합을 빌드해야 하는 경우</span><span class="sxs-lookup"><span data-stu-id="59d0b-140">Need to build a common set of client connectivity features for multiple languages or frameworks.</span></span>
- <span data-ttu-id="59d0b-141">인프라 개발자 또는 더 많은 다른 특수 팀에 교차하는 클라이언트 연결 문제를 오프로드해야 하는 경우</span><span class="sxs-lookup"><span data-stu-id="59d0b-141">Need to offload cross-cutting client connectivity concerns to infrastructure developers or other more specialized teams.</span></span>
- <span data-ttu-id="59d0b-142">레거시 애플리케이션 또는 수정하기 어려운 애플리케이션에서 클라우드 또는 클러스터 연결 요구 사항을 지원해야 하는 경우</span><span class="sxs-lookup"><span data-stu-id="59d0b-142">Need to support cloud or cluster connectivity requirements in a legacy application or an application that is difficult to modify.</span></span>

<span data-ttu-id="59d0b-143">다음 경우에는 이 패턴이 적합하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-143">This pattern may not be suitable:</span></span>

- <span data-ttu-id="59d0b-144">네트워크 요청 대기 시간이 중요한 경우.</span><span class="sxs-lookup"><span data-stu-id="59d0b-144">When network request latency is critical.</span></span> <span data-ttu-id="59d0b-145">프록시에서 최소 수준이더라도 약간의 오버헤드가 발생하고 이는 일부 경우에 애플리케이션에 영향을 미칠 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-145">A proxy will introduce some overhead, although minimal, and in some cases this may affect the application.</span></span>
- <span data-ttu-id="59d0b-146">단일 언어로 클라이언트 연결 기능을 사용하는 경우.</span><span class="sxs-lookup"><span data-stu-id="59d0b-146">When client connectivity features are consumed by a single language.</span></span> <span data-ttu-id="59d0b-147">이 경우 개발팀에 패키지로 배포되는 클라이언트 라이브러리가 더 나은 옵션이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-147">In that case, a better option might be a client library that is distributed to the development teams as a package.</span></span>
- <span data-ttu-id="59d0b-148">연결 기능이 일반화될 수 없고 클라이언트 애플리케이션과 심층적 통합이 필요한 경우</span><span class="sxs-lookup"><span data-stu-id="59d0b-148">When connectivity features cannot be generalized and require deeper integration with the client application.</span></span>

## <a name="example"></a><span data-ttu-id="59d0b-149">예</span><span class="sxs-lookup"><span data-stu-id="59d0b-149">Example</span></span>

<span data-ttu-id="59d0b-150">다음 다이어그램에서는 특사 프록시를 통해 원격 서비스에 요청하는 애플리케이션을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-150">The following diagram shows an application making a request to a remote service via an ambassador proxy.</span></span> <span data-ttu-id="59d0b-151">특사는 라우팅, 회로 차단 및 로깅을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-151">The ambassador provides routing, circuit breaking, and logging.</span></span> <span data-ttu-id="59d0b-152">원격 서비스를 호출한 후 클라이언트 애플리케이션에 대한 응답을 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="59d0b-152">It calls the remote service and then returns the response to the client application:</span></span>

![특사 패턴의 예제](./_images/ambassador-example.png)

## <a name="related-guidance"></a><span data-ttu-id="59d0b-154">관련 지침</span><span class="sxs-lookup"><span data-stu-id="59d0b-154">Related guidance</span></span>

- [<span data-ttu-id="59d0b-155">사이드카 패턴</span><span class="sxs-lookup"><span data-stu-id="59d0b-155">Sidecar pattern</span></span>](./sidecar.md)

<!-- links -->

[resiliency-patterns]: ./category/resiliency.md
