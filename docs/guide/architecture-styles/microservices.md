---
title: 마이크로 서비스 아키텍처 스타일
description: Azure에서 마이크로 서비스 아키텍처의 혜택, 과제 및 모범 사례를 설명합니다.
author: MikeWasson
ms.date: 11/13/2018
ms.openlocfilehash: 4e5d50f829323829c953977257e690354566ebf6
ms.sourcegitcommit: 19a517a2fb70768b3edb9a7c3c37197baa61d9b5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52295534"
---
# <a name="microservices-architecture-style"></a><span data-ttu-id="4202f-103">마이크로 서비스 아키텍처 스타일</span><span class="sxs-lookup"><span data-stu-id="4202f-103">Microservices architecture style</span></span>

<span data-ttu-id="4202f-104">마이크로 서비스 아키텍처는 작은 자율 서비스 컬렉션으로 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-104">A microservices architecture consists of a collection of small, autonomous services.</span></span> <span data-ttu-id="4202f-105">각 서비스는 자체 포함되며 단일 비즈니스 기능을 구현해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-105">Each service is self-contained and should implement a single business capability.</span></span> 

![](./images/microservices-logical.svg)
 
<span data-ttu-id="4202f-106">일부 측면에서 마이크로 서비스는 SOA(서비스 지향 아키텍처)의 자연스러운 발전이지만 마이크로 서비스와 SOA 간에는 차이가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-106">In some ways, microservices are the natural evolution of service oriented architectures (SOA), but there are differences between microservices and SOA.</span></span> <span data-ttu-id="4202f-107">다음은 마이크로 서비스를 정의하는 몇 가지 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-107">Here are some defining characteristics of a microservice:</span></span>

- <span data-ttu-id="4202f-108">마이크로 서비스 아키텍처에서 서비스는 작고, 독립적이며, 느슨하게 결합되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-108">In a microservices architecture, services are small, independent, and loosely coupled.</span></span>

- <span data-ttu-id="4202f-109">각 서비스는 작은 개발 팀이 관리할 수 있는 개별 코드베이스입니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-109">Each service is a separate codebase, which can be managed by a small development team.</span></span>

- <span data-ttu-id="4202f-110">서비스를 독립적으로 배포할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-110">Services can be deployed independently.</span></span> <span data-ttu-id="4202f-111">팀이 전체 응용 프로그램을 다시 빌드한 후 재배치하지 않고도 기존 서비스를 업데이트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-111">A team can update an existing service without rebuilding and redeploying the entire application.</span></span>

- <span data-ttu-id="4202f-112">서비스가 해당 데이터 또는 외부 상태를 유지해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-112">Services are responsible for persisting their own data or external state.</span></span> <span data-ttu-id="4202f-113">이는 별도의 데이터 레이어가 데이터 지속성을 처리하는 기존 모델과의 차이점입니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-113">This differs from the traditional model, where a separate data layer handles data persistence.</span></span>

- <span data-ttu-id="4202f-114">서비스가 잘 정의된 API를 사용하여 서로 통신합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-114">Services communicate with each other by using well-defined APIs.</span></span> <span data-ttu-id="4202f-115">각 서비스의 내부 구현 세부 정보는 다른 서비스에서 숨겨집니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-115">Internal implementation details of each service are hidden from other services.</span></span>

- <span data-ttu-id="4202f-116">서비스가 동일한 기술 스택, 라이브러리 또는 프레임워크를 공유할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-116">Services don't need to share the same technology stack, libraries, or frameworks.</span></span>

<span data-ttu-id="4202f-117">서비스 자체 외에도 다음과 같은 몇 가지 다른 구성 요소가 기존 마이크로 서비스 아키텍처에 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-117">Besides for the services themselves, some other components appear in a typical microservices architecture:</span></span>

<span data-ttu-id="4202f-118">**관리**.</span><span class="sxs-lookup"><span data-stu-id="4202f-118">**Management**.</span></span> <span data-ttu-id="4202f-119">관리 구성 요소는 노드에 서비스 배치, 실패 식별, 노드 간에 서비스 부하 조정 등의 작업을 담당합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-119">The management component is responsible for placing services on nodes, identifying failures, rebalancing services across nodes, and so forth.</span></span>  

<span data-ttu-id="4202f-120">**서비스 검색**.</span><span class="sxs-lookup"><span data-stu-id="4202f-120">**Service Discovery**.</span></span>  <span data-ttu-id="4202f-121">서비스 목록과 서비스 목록이 배치되는 노드를 유지 관리합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-121">Maintains a list of services and which nodes they are located on.</span></span> <span data-ttu-id="4202f-122">서비스 조회를 통해 서비스 엔드포인트를 찾을 수 있게 합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-122">Enables service lookup to find the endpoint for a service.</span></span> 

<span data-ttu-id="4202f-123">**API 게이트웨이**.</span><span class="sxs-lookup"><span data-stu-id="4202f-123">**API Gateway**.</span></span> <span data-ttu-id="4202f-124">API 게이트웨이는 클라이언트의 진입점입니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-124">The API gateway is the entry point for clients.</span></span> <span data-ttu-id="4202f-125">클라이언트는 서비스를 직접 호출하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-125">Clients don't call services directly.</span></span> <span data-ttu-id="4202f-126">대신, 호출을 백 엔드의 적절한 서비스에 전달하는 API 게이트웨이를 호출합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-126">Instead, they call the API gateway, which forwards the call to the appropriate services on the back end.</span></span> <span data-ttu-id="4202f-127">API 게이트웨이는 여러 서비스의 응답을 집계하고 집계된 응답을 반환할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-127">The API gateway might aggregate the responses from several services and return the aggregated response.</span></span> 

<span data-ttu-id="4202f-128">API 게이트웨이를 사용할 경우의 장점은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-128">The advantages of using an API gateway include:</span></span>

- <span data-ttu-id="4202f-129">클라이언트와 서비스가 분리됩니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-129">It decouples clients from services.</span></span> <span data-ttu-id="4202f-130">모든 클라이언트를 업데이트하지 않고도 서비스 버전을 관리하거나 서비스를 리팩터링할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-130">Services can be versioned or refactored without needing to update all of the clients.</span></span>

-  <span data-ttu-id="4202f-131">서비스가 웹 우호적이 아닌 AMQP 등의 메시징 프로토콜을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-131">Services can use messaging protocols that are not web friendly, such as AMQP.</span></span>

- <span data-ttu-id="4202f-132">API 게이트웨이는 인증, 로깅, SSL 종료, 부하 분산 등의 다른 교차 기능을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-132">The API Gateway can perform other cross-cutting functions such as authentication, logging, SSL termination, and load balancing.</span></span>

## <a name="when-to-use-this-architecture"></a><span data-ttu-id="4202f-133">이 아키텍처를 사용하는 경우</span><span class="sxs-lookup"><span data-stu-id="4202f-133">When to use this architecture</span></span>

<span data-ttu-id="4202f-134">다음과 같은 경우 이 아키텍처 스타일을 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-134">Consider this architecture style for:</span></span>

- <span data-ttu-id="4202f-135">높은 릴리스 개발속도가 필요한 대규모 응용 프로그램.</span><span class="sxs-lookup"><span data-stu-id="4202f-135">Large applications that require a high release velocity.</span></span>

- <span data-ttu-id="4202f-136">고확장성이 필요한 복합 응용 프로그램.</span><span class="sxs-lookup"><span data-stu-id="4202f-136">Complex applications that need to be highly scalable.</span></span>

- <span data-ttu-id="4202f-137">풍부한 도메인이나 많은 하위 도메인이 있는 응용 프로그램.</span><span class="sxs-lookup"><span data-stu-id="4202f-137">Applications with rich domains or many subdomains.</span></span>

- <span data-ttu-id="4202f-138">소규모 개발 팀으로 구성된 조직.</span><span class="sxs-lookup"><span data-stu-id="4202f-138">An organization that consists of small development teams.</span></span>


## <a name="benefits"></a><span data-ttu-id="4202f-139">이점</span><span class="sxs-lookup"><span data-stu-id="4202f-139">Benefits</span></span> 

- <span data-ttu-id="4202f-140">**독립 배포**.</span><span class="sxs-lookup"><span data-stu-id="4202f-140">**Independent deployments**.</span></span> <span data-ttu-id="4202f-141">전체 응용 프로그램을 다시 배포하지 않고 서비스를 업데이트할 수 있고, 문제가 발생하면 업데이트를 롤백 또는 롤포워드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-141">You can update a service without redeploying the entire application, and roll back or roll forward an update if something goes wrong.</span></span> <span data-ttu-id="4202f-142">버그 수정 및 기능 릴리스 관리가 더 용이해지고 위험이 감소합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-142">Bug fixes and feature releases are more manageable and less risky.</span></span>

- <span data-ttu-id="4202f-143">**독립 개발**.</span><span class="sxs-lookup"><span data-stu-id="4202f-143">**Independent development**.</span></span> <span data-ttu-id="4202f-144">단일 개발 팀이 서비스를 빌드, 테스트 및 배포할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-144">A single development team can build, test, and deploy a service.</span></span> <span data-ttu-id="4202f-145">그 결과, 연속적인 혁신과 더 빠른 릴리스 주기가 가능합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-145">The result is continuous innovation and a faster release cadence.</span></span> 

- <span data-ttu-id="4202f-146">**집중화된 소규모 팀**.</span><span class="sxs-lookup"><span data-stu-id="4202f-146">**Small, focused teams**.</span></span> <span data-ttu-id="4202f-147">팀이 한 서비스에 집중할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-147">Teams can focus on one service.</span></span> <span data-ttu-id="4202f-148">각 서비스의 범위가 작을수록 코드베이스 이해가 더 용이해지며 새로운 팀 구성원이 더 쉽게 이용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-148">The smaller scope of each service makes the code base easier to understand, and it's easier for new team members to ramp up.</span></span>

- <span data-ttu-id="4202f-149">**결함 격리**.</span><span class="sxs-lookup"><span data-stu-id="4202f-149">**Fault isolation**.</span></span> <span data-ttu-id="4202f-150">한 서비스가 다운되더라도 전체 응용 프로그램 작동은 중단되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-150">If a service goes down, it won't take out the entire application.</span></span> <span data-ttu-id="4202f-151">그러나 저절로 복원되는 것은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-151">However, that doesn't mean you get resiliency for free.</span></span> <span data-ttu-id="4202f-152">복원 모범 사례 및 디자인 패턴을 따라야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-152">You still need to follow resiliency best practices and design patterns.</span></span> <span data-ttu-id="4202f-153">[Azure용 복원 응용 프로그램 디자인][resiliency-overview]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="4202f-153">See [Designing resilient applications for Azure][resiliency-overview].</span></span>

- <span data-ttu-id="4202f-154">**혼합 기술 스택**.</span><span class="sxs-lookup"><span data-stu-id="4202f-154">**Mixed technology stacks**.</span></span> <span data-ttu-id="4202f-155">팀이 해당 서비스에 가장 적합한 기술을 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-155">Teams can pick the technology that best fits their service.</span></span> 

- <span data-ttu-id="4202f-156">**세분화된 크기 조정**.</span><span class="sxs-lookup"><span data-stu-id="4202f-156">**Granular scaling**.</span></span> <span data-ttu-id="4202f-157">서비스를 독립적으로 확장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-157">Services can be scaled independently.</span></span> <span data-ttu-id="4202f-158">이와 동시에, VM당 서비스 수준이 높을수록 VM 리소스가 완전히 활용됨을 의미합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-158">At the same time, the higher density of services per VM means that VM resources are fully utilized.</span></span> <span data-ttu-id="4202f-159">배치 제약 조건을 사용하여 서비스를 VM 프로필(높은 CPU, 높은 메모리 등)에 맞출 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-159">Using placement constraints, a services can be matched to a VM profile (high CPU, high memory, and so on).</span></span>

## <a name="challenges"></a><span data-ttu-id="4202f-160">과제</span><span class="sxs-lookup"><span data-stu-id="4202f-160">Challenges</span></span>

- <span data-ttu-id="4202f-161">**복잡성**.</span><span class="sxs-lookup"><span data-stu-id="4202f-161">**Complexity**.</span></span> <span data-ttu-id="4202f-162">마이크로 서비스 응용 프로그램에는 동등한 모놀리식 응용 프로그램보다 작동 부분이 더 많습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-162">A microservices application has more moving parts than the equivalent monolithic application.</span></span> <span data-ttu-id="4202f-163">각 서비스는 더 단순하지만 전체 시스템이 더 복잡합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-163">Each service is simpler, but the entire system as a whole is more complex.</span></span>

- <span data-ttu-id="4202f-164">**개발 및 테스트**.</span><span class="sxs-lookup"><span data-stu-id="4202f-164">**Development and test**.</span></span> <span data-ttu-id="4202f-165">서비스 종속성에 대한 개발 시 다른 접근 방법이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-165">Developing against service dependencies requires a different approach.</span></span> <span data-ttu-id="4202f-166">기존 도구는 서비스 종속성 작업에 맞게 설계되지 않았을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-166">Existing tools are not necessarily designed to work with service dependencies.</span></span> <span data-ttu-id="4202f-167">서비스 경계를 벗어난 리팩터링은 어려울 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-167">Refactoring across service boundaries can be difficult.</span></span> <span data-ttu-id="4202f-168">특히 응용 프로그램이 빠르게 발전하는 경우 서비스 종속성을 테스트하기도 어렵습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-168">It is also challenging to test service dependencies, especially when the application is evolving quickly.</span></span>

- <span data-ttu-id="4202f-169">**통제 부족**.</span><span class="sxs-lookup"><span data-stu-id="4202f-169">**Lack of governance**.</span></span> <span data-ttu-id="4202f-170">마이크로 서비스 빌드에 대한 분산 접근 방법에는 장점이 있지만 문제가 발생할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-170">The decentralized approach to building microservices has advantages, but it can also lead to problems.</span></span> <span data-ttu-id="4202f-171">언어와 프레임워크가 너무 많아서 응용 프로그램 유지 관리가 어려워질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-171">You may end up with so many different languages and frameworks that the application becomes hard to maintain.</span></span> <span data-ttu-id="4202f-172">팀의 유연성을 지나치게 제한하지 않고 몇 가지 프로젝트 전체 표준을 적용하는 것이 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-172">It may be useful to put some project-wide standards in place, without overly restricting teams' flexibility.</span></span> <span data-ttu-id="4202f-173">특히 로깅과 같은 교차 기능에 해당합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-173">This especially applies to cross-cutting functionality such as logging.</span></span>

- <span data-ttu-id="4202f-174">**네트워크 정체 및 대기 시간**.</span><span class="sxs-lookup"><span data-stu-id="4202f-174">**Network congestion and latency**.</span></span> <span data-ttu-id="4202f-175">다수의 작고 세분화된 서비스를 사용하면 서비스 간 통신이 증가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-175">The use of many small, granular services can result in more interservice communication.</span></span> <span data-ttu-id="4202f-176">또한 서비스 종속성 체인이 너무 길어질 경우(서비스 A가 B를 호출하고, B가 C를 호출하고...) 추가 대기 시간이 문제가 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-176">Also, if the chain of service dependencies gets too long (service A calls B, which calls C...), the additional latency can become a problem.</span></span> <span data-ttu-id="4202f-177">API를 신중하게 디자인해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-177">You will need to design APIs carefully.</span></span> <span data-ttu-id="4202f-178">통신량이 과도한 API를 피하고, 직렬화 형식을 고려하고, 비동기 통신 패턴을 사용할 영역을 찾아보세요.</span><span class="sxs-lookup"><span data-stu-id="4202f-178">Avoid overly chatty APIs, think about serialization formats, and look for places to use asynchronous communication patterns.</span></span>

- <span data-ttu-id="4202f-179">**데이터 무결성**.</span><span class="sxs-lookup"><span data-stu-id="4202f-179">**Data integrity**.</span></span> <span data-ttu-id="4202f-180">각 마이크로 서비스가 자체 데이터 지속성을 담당합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-180">With each microservice responsible for its own data persistence.</span></span> <span data-ttu-id="4202f-181">그 결과, 데이터 일관성이 과제가 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-181">As a result, data consistency can be a challenge.</span></span> <span data-ttu-id="4202f-182">가능한 경우 결과적 일관성을 수용합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-182">Embrace eventual consistency where possible.</span></span>

- <span data-ttu-id="4202f-183">**관리**.</span><span class="sxs-lookup"><span data-stu-id="4202f-183">**Management**.</span></span> <span data-ttu-id="4202f-184">마이크로 서비스에 성공하려면 성숙한 DevOps 문화가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-184">To be successful with microservices requires a mature DevOps culture.</span></span> <span data-ttu-id="4202f-185">전체 서비스의 상관관계 로깅이 까다로울 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-185">Correlated logging across services can be challenging.</span></span> <span data-ttu-id="4202f-186">일반적으로 로깅은 단일 사용자 작업에 대한 여러 서비스 호출을 상호 연결해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-186">Typically, logging must correlate multiple service calls for a single user operation.</span></span>

- <span data-ttu-id="4202f-187">**버전 관리**.</span><span class="sxs-lookup"><span data-stu-id="4202f-187">**Versioning**.</span></span> <span data-ttu-id="4202f-188">서비스 업데이트로 인해 종속된 서비스가 손상되지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-188">Updates to a service must not break services that depend on it.</span></span> <span data-ttu-id="4202f-189">언제든지 여러 서비스가 업데이트될 수 있으므로 신중하게 디자인하지 않으면 이전 버전 또는 이후 버전과의 호환성 문제가 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-189">Multiple services could be updated at any given time, so without careful design, you might have problems with backward or forward compatibility.</span></span>

- <span data-ttu-id="4202f-190">**기능**.</span><span class="sxs-lookup"><span data-stu-id="4202f-190">**Skillset**.</span></span> <span data-ttu-id="4202f-191">마이크로 서비스는 고도로 분산된 시스템입니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-191">Microservices are highly distributed systems.</span></span> <span data-ttu-id="4202f-192">팀이 성공을 위한 기술과 경험을 가지고 있는지 신중하게 평가합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-192">Carefully evaluate whether the team has the skills and experience to be successful.</span></span>

## <a name="best-practices"></a><span data-ttu-id="4202f-193">모범 사례</span><span class="sxs-lookup"><span data-stu-id="4202f-193">Best practices</span></span>

- <span data-ttu-id="4202f-194">비즈니스 도메인을 중심으로 서비스를 모델링합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-194">Model services around the business domain.</span></span> 

- <span data-ttu-id="4202f-195">모든 것을 분산합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-195">Decentralize everything.</span></span> <span data-ttu-id="4202f-196">개별 팀이 서비스 디자인 및 빌드를 담당합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-196">Individual teams are responsible for designing and building services.</span></span> <span data-ttu-id="4202f-197">코드 또는 데이터 스키마를 공유하지 않도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-197">Avoid sharing code or data schemas.</span></span> 

- <span data-ttu-id="4202f-198">데이터 저장소가 데이터를 소유하는 서비스의 개인용이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-198">Data storage should be private to the service that owns the data.</span></span> <span data-ttu-id="4202f-199">각 서비스 및 데이터 형식에 가장 적합한 저장소를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-199">Use the best storage for each service and data type.</span></span> 

- <span data-ttu-id="4202f-200">서비스가 잘 디자인된 API를 통해 통신합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-200">Services communicate through well-designed APIs.</span></span> <span data-ttu-id="4202f-201">구현 세부 정보가 누출되지 않도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-201">Avoid leaking implementation details.</span></span> <span data-ttu-id="4202f-202">API는 서비스의 내부 구현이 아니라 도메인을 모델링해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-202">APIs should model the domain, not the internal implementation of the service.</span></span>

- <span data-ttu-id="4202f-203">서비스 간의 결합을 피합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-203">Avoid coupling between services.</span></span> <span data-ttu-id="4202f-204">결합의 원인에는 공유 데이터베이스 스키마, 엄격한 통신 프로토콜 등이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-204">Causes of coupling include shared database schemas and rigid communication protocols.</span></span>

- <span data-ttu-id="4202f-205">인증, SSL 종료 등의 교차 문제를 게이트웨이에 오프로드합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-205">Offload cross-cutting concerns, such as authentication and SSL termination, to the gateway.</span></span>

- <span data-ttu-id="4202f-206">도메인 정보를 게이트웨이에서 숨깁니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-206">Keep domain knowledge out of the gateway.</span></span> <span data-ttu-id="4202f-207">게이트웨이는 비즈니스 규칙 또는 도메인 논리를 몰라도 클라이언트 요청을 처리하고 라우트해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-207">The gateway should handle and route client requests without any knowledge of the business rules or domain logic.</span></span> <span data-ttu-id="4202f-208">그러지 않으면 게이트웨이가 종속성이 되며 서비스 간에 결합이 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-208">Otherwise, the gateway becomes a dependency and can cause coupling between services.</span></span>

- <span data-ttu-id="4202f-209">서비스에 느슨한 결합 및 높은 기능 응집력이 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-209">Services should have loose coupling and high functional cohesion.</span></span> <span data-ttu-id="4202f-210">함께 변경될 가능성이 큰 기능은 함께 패키지하고 배포해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-210">Functions that are likely to change together should be packaged and deployed together.</span></span> <span data-ttu-id="4202f-211">개별 서비스에 상주할 경우, 한 서비스가 변경되면 다른 서비스를 업데이트해야 하므로 해당 서비스가 긴밀하게 결합됩니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-211">If they reside in separate services, those services end up being tightly coupled, because a change in one service will require updating the other service.</span></span> <span data-ttu-id="4202f-212">두 서비스 간의 과도한 통신량은 긴밀한 결합과 낮은 응집력의 증상일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-212">Overly chatty communication between two services may be a symptom of tight coupling and low cohesion.</span></span> 

- <span data-ttu-id="4202f-213">실패를 격리합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-213">Isolate failures.</span></span> <span data-ttu-id="4202f-214">복원 전략을 사용하여 한 서비스 내의 실패가 계단식으로 연속되지 않도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="4202f-214">Use resiliency strategies to prevent failures within a service from cascading.</span></span> <span data-ttu-id="4202f-215">[복원력 패턴][resiliency-patterns] 및 [복원력 있는 응용 프로그램 디자인][resiliency-overview]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="4202f-215">See [Resiliency patterns][resiliency-patterns] and [Designing resilient applications][resiliency-overview].</span></span>

## <a name="next-steps"></a><span data-ttu-id="4202f-216">다음 단계</span><span class="sxs-lookup"><span data-stu-id="4202f-216">Next steps</span></span>

<span data-ttu-id="4202f-217">Azure에서 마이크로 서비스 아키텍처를 빌드하는 방법에 대한 자세한 지침은 [Azure에서 마이크로 서비스 설계, 구축 및 운영](../../microservices/index.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="4202f-217">For detailed guidance about building a microservices architecture on Azure, see [Designing, building, and operating microservices on Azure](../../microservices/index.md).</span></span>


<!-- links -->

[resiliency-overview]: ../../resiliency/index.md
[resiliency-patterns]: ../../patterns/category/resiliency.md



