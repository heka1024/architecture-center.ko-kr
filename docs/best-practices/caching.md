---
title: 캐싱 지침
titleSuffix: Best practices for cloud applications
description: 성능 및 확장성을 향상하기 위한 캐시에 대한 지침입니다.
author: dragon119
ms.date: 05/24/2017
ms.custom: seodec18
ms.openlocfilehash: b7f720b9e08b0316f9967a19e1b93069aa04e55f
ms.sourcegitcommit: 4ba3304eebaa8c493c3e5307bdd9d723cd90b655
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53307472"
---
# <a name="caching"></a><span data-ttu-id="3c1b9-103">구성</span><span class="sxs-lookup"><span data-stu-id="3c1b9-103">Caching</span></span>

<span data-ttu-id="3c1b9-104">캐싱은 시스템의 성능 및 확장성을 개선하는 데 목표를 두는 일반적인 기술입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-104">Caching is a common technique that aims to improve the performance and scalability of a system.</span></span> <span data-ttu-id="3c1b9-105">이를 위해 자주 액세스하는 데이터를 애플리케이션에 가까이 있는 빠른 스토리지에 일시적으로 복사합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-105">It does this by temporarily copying frequently accessed data to fast storage that's located close to the application.</span></span> <span data-ttu-id="3c1b9-106">이 빠른 데이터 스토리지가 원래 원본보다 애플리케이션에 가까이 위치하는 경우 캐싱은 데이터를 보다 신속하게 이용하여 클라이언트 애플리케이션에 대한 응답 시간을 훨씬 향상할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-106">If this fast data storage is located closer to the application than the original source, then caching can significantly improve response times for client applications by serving data more quickly.</span></span>

<span data-ttu-id="3c1b9-107">캐싱은 클라이언트 인스턴스가 동일한 데이터를 반복해서 읽을 때 특히 다음 조건이 모두 원래 데이터 저장소에 적용되는 경우에 가장 효과적입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-107">Caching is most effective when a client instance repeatedly reads the same data, especially if all the following conditions apply to the original data store:</span></span>

- <span data-ttu-id="3c1b9-108">상대적으로 정적 상태로 유지됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-108">It remains relatively static.</span></span>
- <span data-ttu-id="3c1b9-109">캐시 속도와 비교하여 느립니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-109">It's slow compared to the speed of the cache.</span></span>
- <span data-ttu-id="3c1b9-110">높은 수준의 경합이 발생하기 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-110">It's subject to a high level of contention.</span></span>
- <span data-ttu-id="3c1b9-111">네트워크 대기 시간이 액세스를 느려지게 할 수 있는 경우 멀리 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-111">It's far away when network latency can cause access to be slow.</span></span>

## <a name="caching-in-distributed-applications"></a><span data-ttu-id="3c1b9-112">분산된 애플리케이션에서 캐싱</span><span class="sxs-lookup"><span data-stu-id="3c1b9-112">Caching in distributed applications</span></span>

<span data-ttu-id="3c1b9-113">분산된 애플리케이션은 데이터를 캐시할 때 일반적으로 다음 전략 중 하나 또는 모두를 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-113">Distributed applications typically implement either or both of the following strategies when caching data:</span></span>

- <span data-ttu-id="3c1b9-114">데이터가 애플리케이션 또는 서비스의 인스턴스를 실행하는 컴퓨터에서 로컬로 유지되는 전용 캐시를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-114">Using a private cache, where data is held locally on the computer that's running an instance of an application or service.</span></span>
- <span data-ttu-id="3c1b9-115">여러 프로세스 및/또는 컴퓨터가 액세스할 수 있는 일반적인 소스로 제공하는 공유 캐시를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-115">Using a shared cache, serving as a common source which can be accessed by multiple processes and/or machines.</span></span>

<span data-ttu-id="3c1b9-116">두 경우 모두 캐싱이 클라이언트 쪽 및/또는 서버 쪽에서 수행될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-116">In both cases, caching can be performed client-side and/or server-side.</span></span> <span data-ttu-id="3c1b9-117">클라이언트 쪽 캐싱은 웹 브라우저 또는 데스크톱 애플리케이션과 같이 시스템의 사용자 인터페이스를 제공하는 프로세스에서 수행됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-117">Client-side caching is done by the process that provides the user interface for a system, such as a web browser or desktop application.</span></span> <span data-ttu-id="3c1b9-118">서버 쪽 캐싱은 원격으로 실행되는 비즈니스 서비스를 제공하는 프로세스에서 수행됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-118">Server-side caching is done by the process that provides the business services that are running remotely.</span></span>

### <a name="private-caching"></a><span data-ttu-id="3c1b9-119">전용 캐싱</span><span class="sxs-lookup"><span data-stu-id="3c1b9-119">Private caching</span></span>

<span data-ttu-id="3c1b9-120">캐시의 가장 기본적인 형식은 메모리 내 저장소입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-120">The most basic type of cache is an in-memory store.</span></span> <span data-ttu-id="3c1b9-121">이는 단일 프로세스의 주소 공간에 보유하고 해당 프로세스에서 실행되는 코드에서 직접 액세스합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-121">It's held in the address space of a single process and accessed directly by the code that runs in that process.</span></span> <span data-ttu-id="3c1b9-122">이 유형의 캐시는 액세스가 매우 빠릅니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-122">This type of cache is very quick to access.</span></span> <span data-ttu-id="3c1b9-123">캐시의 크기가 일반적으로 프로세스를 호스트하는 컴퓨터에서 사용 가능한 메모리의 볼륨으로 제한되므로 적당한 양의 정적 데이터를 저장하기 위해 매우 효과적인 수단을 제공할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-123">It can also provide an extremely effective means for storing modest amounts of static data, since the size of a cache is typically constrained by the volume of memory that's available on the machine hosting the process.</span></span>

<span data-ttu-id="3c1b9-124">메모리에서 물리적으로 가능한 것 보다 많은 정보를 캐시하는 경우 로컬 파일 시스템에 캐시된 데이터를 작성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-124">If you need to cache more information than is physically possible in memory, you can write cached data to the local file system.</span></span> <span data-ttu-id="3c1b9-125">이를 통한 액세스는 데이터 메모리에 유지된 데이터보다 느리지만 여전히 네트워크를 통해 데이터를 검색할 때보다 빠르고 안정적입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-125">This will be slower to access than data that's held in-memory, but should still be faster and more reliable than retrieving data across a network.</span></span>

<span data-ttu-id="3c1b9-126">이 모델을 사용하는 애플리케이션의 여러 인스턴스가 동시에 실행되는 경우 각 애플리케이션 인스턴스가 데이터의 자체 복사본을 유지하는 자체 독립적인 캐시 데이터를 보유합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-126">If you have multiple instances of an application that uses this model running concurrently, each application instance has its own independent cache holding its own copy of the data.</span></span>

<span data-ttu-id="3c1b9-127">캐시는 과거 어떤 시점에 있던 원래 데이터의 스냅숏으로 생각할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-127">Think of a cache as a snapshot of the original data at some point in the past.</span></span> <span data-ttu-id="3c1b9-128">이 데이터가 정적이지 않으면 다른 애플리케이션 인스턴스가 캐시에 서로 다른 버전의 데이터를 유지할 가능성이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-128">If this data is not static, it is likely that different application instances hold different versions of the data in their caches.</span></span> <span data-ttu-id="3c1b9-129">따라서 이러한 인스턴스가 수행한 동일한 쿼리는 그림 1에 나와있는 것처럼 서로 다른 결과를 반환할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-129">Therefore, the same query performed by these instances can return different results, as shown in Figure 1.</span></span>

![애플리케이션의 서로 다른 인스턴스에서 메모리 내 캐시를 사용](./images/caching/Figure1.png)

<span data-ttu-id="3c1b9-131">*그림 1: 애플리케이션의 서로 다른 인스턴스에서 메모리 내 캐시 사용.*</span><span class="sxs-lookup"><span data-stu-id="3c1b9-131">*Figure 1: Using an in-memory cache in different instances of an application.*</span></span>

### <a name="shared-caching"></a><span data-ttu-id="3c1b9-132">공유 캐싱</span><span class="sxs-lookup"><span data-stu-id="3c1b9-132">Shared caching</span></span>

<span data-ttu-id="3c1b9-133">메모리 내 캐싱으로 발생하는 각 캐시 데이터가 달라지는 문제는 공유 캐시를 사용하면 완화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-133">Using a shared cache can help alleviate concerns that data might differ in each cache, which can occur with in-memory caching.</span></span> <span data-ttu-id="3c1b9-134">공유 캐싱을 통해 다른 애플리케이션 인스턴스가 캐시된 데이터의 동일한 보기를 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-134">Shared caching ensures that different application instances see the same view of cached data.</span></span> <span data-ttu-id="3c1b9-135">이를 위해 그림 2와 같이 일반적으로 별도 서비스의 일부로 호스트된 별도 위치에 있는 캐시를 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-135">It does this by locating the cache in a separate location, typically hosted as part of a separate service, as shown in Figure 2.</span></span>

![공유 캐시 사용](./images/caching/Figure2.png)

<span data-ttu-id="3c1b9-137">*그림 2: 공유 캐시 사용.*</span><span class="sxs-lookup"><span data-stu-id="3c1b9-137">*Figure 2: Using a shared cache.*</span></span>

<span data-ttu-id="3c1b9-138">공유 캐싱 방법을 사용하는 중요한 이점은 확장성을 제공하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-138">An important benefit of the shared caching approach is the scalability it provides.</span></span> <span data-ttu-id="3c1b9-139">많은 공유 캐시 서비스는 클러스터의 서버를 사용하여 구현되며 투명한 방식으로 클러스터 전체에서 데이터를 배포하는 소프트웨어를 활용합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-139">Many shared cache services are implemented by using a cluster of servers, and utilize software that distributes the data across the cluster in a transparent manner.</span></span> <span data-ttu-id="3c1b9-140">애플리케이션 인스턴스가 캐시 서비스에 요청을 간단히 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-140">An application instance simply sends a request to the cache service.</span></span> <span data-ttu-id="3c1b9-141">기본 인프라가 클러스터에서 캐시된 데이터의 위치를 결정합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-141">The underlying infrastructure is responsible for determining the location of the cached data in the cluster.</span></span> <span data-ttu-id="3c1b9-142">더 많은 서버를 추가하여 캐시를 쉽게 확장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-142">You can easily scale the cache by adding more servers.</span></span>

<span data-ttu-id="3c1b9-143">공유 캐싱 접근 방법에는 두 가지 주요 약점이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-143">There are two main disadvantages of the shared caching approach:</span></span>

- <span data-ttu-id="3c1b9-144">캐시는 더 이상 각 애플리케이션 인스턴스에 로컬로 유지되지 않기 때문에 액세스가 느립니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-144">The cache is slower to access because it is no longer held locally to each application instance.</span></span>
- <span data-ttu-id="3c1b9-145">별도 캐시 서비스를 구현하는 요구 사항이 솔루션에 복잡성을 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-145">The requirement to implement a separate cache service might add complexity to the solution.</span></span>

## <a name="considerations-for-using-caching"></a><span data-ttu-id="3c1b9-146">캐싱 사용에 대한 고려 사항</span><span class="sxs-lookup"><span data-stu-id="3c1b9-146">Considerations for using caching</span></span>

<span data-ttu-id="3c1b9-147">다음 섹션에서는 캐시 설계 및 사용에 대한 고려 사항을 자세히 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-147">The following sections describe in more detail the considerations for designing and using a cache.</span></span>

### <a name="decide-when-to-cache-data"></a><span data-ttu-id="3c1b9-148">데이터를 캐시하는 시점을 결정합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-148">Decide when to cache data</span></span>

<span data-ttu-id="3c1b9-149">캐싱은 성능, 확장성 및 가용성을 크게 향상할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-149">Caching can dramatically improve performance, scalability, and availability.</span></span> <span data-ttu-id="3c1b9-150">보유한 데이터가 많을수록 그리고 이 데이터에 액세스해야 하는 사용자 수가 많을수록 캐싱의 이점은 더 커집니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-150">The more data that you have and the larger the number of users that need to access this data, the greater the benefits of caching become.</span></span> <span data-ttu-id="3c1b9-151">이는 캐싱이 원래 데이터 저장소의 많은 양의 동시 요청 처리와 관련된 대기 시간 및 경합을 줄이기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-151">That's because caching reduces the latency and contention that's associated with handling large volumes of concurrent requests in the original data store.</span></span>

<span data-ttu-id="3c1b9-152">예를 들어 데이터베이스는 제한된 수의 동시 연결을 지원할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-152">For example, a database might support a limited number of concurrent connections.</span></span> <span data-ttu-id="3c1b9-153">하지만 기본 데이터베이스가 아닌 공유 캐시에서 데이터를 검색하면 사용 가능한 연결 수를 현재 모두 사용한 경우에도 클라이언트 애플리케이션에서 이 데이터에 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-153">Retrieving data from a shared cache, however, rather than the underlying database, makes it possible for a client application to access this data even if the number of available connections is currently exhausted.</span></span> <span data-ttu-id="3c1b9-154">또한 데이터베이스를 사용할 수 없게 된 경우 클라이언트 애플리케이션은 캐시에 저장된 데이터를 사용하여 계속할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-154">Additionally, if the database becomes unavailable, client applications might be able to continue by using the data that's held in the cache.</span></span>

<span data-ttu-id="3c1b9-155">자주 읽지만 자주 수정하지 않는 데이터(예: 쓰기 작업보다 읽기 작업의 비율이 더 높은 데이터)를 캐시하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-155">Consider caching data that is read frequently but modified infrequently (for example, data that has a higher proportion of read operations than write operations).</span></span> <span data-ttu-id="3c1b9-156">그러나 캐시를 중요한 정보의 신뢰할 수 있는 저장소로는 사용하지 않는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-156">However, we don't recommend that you use the cache as the authoritative store of critical information.</span></span> <span data-ttu-id="3c1b9-157">대신 애플리케이션에서 손실을 수용할 수 없는 모든 변경 내용은 항상 영구 데이터 저장소에 저장되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-157">Instead, ensure that all changes that your application cannot afford to lose are always saved to a persistent data store.</span></span> <span data-ttu-id="3c1b9-158">이렇게 하면 캐시를 사용할 수 없는 경우 애플리케이션이 데이터 저장소를 사용하여 작업을 계속할 수 있으며 중요한 정보는 손실되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-158">This means that if the cache is unavailable, your application can still continue to operate by using the data store, and you won't lose important information.</span></span>

### <a name="determine-how-to-cache-data-effectively"></a><span data-ttu-id="3c1b9-159">데이터를 효과적으로 캐시하는 방법 결정</span><span class="sxs-lookup"><span data-stu-id="3c1b9-159">Determine how to cache data effectively</span></span>

<span data-ttu-id="3c1b9-160">효과적으로 캐시를 사용하는 열쇠는 캐시에 가장 적합한 데이터를 결정하고 적절한 시점에 캐싱하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-160">The key to using a cache effectively lies in determining the most appropriate data to cache, and caching it at the appropriate time.</span></span> <span data-ttu-id="3c1b9-161">데이터가 처음으로 애플리케이션에서 검색되었을 때 필요에 따라 캐시에 데이터를 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-161">The data can be added to the cache on demand the first time it is retrieved by an application.</span></span> <span data-ttu-id="3c1b9-162">즉, 애플리케이션이 데이터 저장소에서 한 번만 데이터를 가져와야 하며 이후 액세스는 캐시를 사용하여 충족할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-162">This means that the application needs to fetch the data only once from the data store, and that subsequent access can be satisfied by using the cache.</span></span>

<span data-ttu-id="3c1b9-163">또는 일반적으로 애플리케이션을 시작할 때 캐시가 사전에 부분적으로 또는 완전히 데이터로 채워질 수 있습니다(시딩이라고 알려진 방법).</span><span class="sxs-lookup"><span data-stu-id="3c1b9-163">Alternatively, a cache can be partially or fully populated with data in advance, typically when the application starts (an approach known as seeding).</span></span> <span data-ttu-id="3c1b9-164">그러나 이 방법은 애플리케이션을 실행하기 시작할 때 데이터 저장소에 갑자기 높은 부하를 부과하여 큰 캐시에 대한 시드를 구현하는 데 권장하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-164">However, it might not be advisable to implement seeding for a large cache because this approach can impose a sudden, high load on the original data store when the application starts running.</span></span>

<span data-ttu-id="3c1b9-165">보통, 사용 패턴에 대한 분석을 실행하면 완전히 또는 부분적으로 캐시를 미리 채울지 결정하고 캐시가 될 데이터를 선택하는 데 도움이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-165">Often an analysis of usage patterns can help you decide whether to fully or partially prepopulate a cache, and to choose the data to cache.</span></span> <span data-ttu-id="3c1b9-166">예를 들어 애플리케이션을 일주일에 한 번만 사용하는 고객이 아니라 정기적으로(보통 매일) 사용하는 고객에 대해 정적 사용자 프로필 데이터와 함께 캐시를 시드하는 것이 유용할 것입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-166">For example, it can be useful to seed the cache with the static user profile data for customers who use the application regularly (perhaps every day), but not for customers who use the application only once a week.</span></span>

<span data-ttu-id="3c1b9-167">캐싱은 일반적으로 변경할 수 없거나 자주 변경되지 않는 데이터와 잘 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-167">Caching typically works well with data that is immutable or that changes infrequently.</span></span> <span data-ttu-id="3c1b9-168">예로 전자 상거래 애플리케이션에서 제품 및 가격 정보와 같은 참조 정보 또는 생성 비용이 많이 드는 공유 정적 리소스를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-168">Examples include reference information such as product and pricing information in an e-commerce application, or shared static resources that are costly to construct.</span></span> <span data-ttu-id="3c1b9-169">이 데이터의 일부 또는 전부를 애플리케이션 시작 시 로드하여 리소스에 대한 수요를 최소화하고 성능을 향상할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-169">Some or all of this data can be loaded into the cache at application startup to minimize demand on resources and to improve performance.</span></span> <span data-ttu-id="3c1b9-170">또한 백그라운드 프로세스가 주기적으로 최신 상태를 확인하기 위해 캐시의 참조 데이터를 업데이트하거나 참조 데이터가 변경되는 경우 새로 고치는 것이 적합할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-170">It might also be appropriate to have a background process that periodically updates reference data in the cache to ensure it is up to date, or that refreshes the cache when reference data changes.</span></span>

<span data-ttu-id="3c1b9-171">이 고려 사항에 일부 예외는 있지만 캐싱이 동적 데이터에서 유용하지 않습니다(자세한 내용은 이 문서의 뒷부분에 나오는 매우 동적인 데이터 캐시 섹션을 참조).</span><span class="sxs-lookup"><span data-stu-id="3c1b9-171">Caching is less useful for dynamic data, although there are some exceptions to this consideration (see the section Cache highly dynamic data later in this article for more information).</span></span> <span data-ttu-id="3c1b9-172">원래 데이터를 정기적으로 변경하면 캐시된 정보가 매우 빠르게 유효하지 않게 되거나 원래 데이터 저장소와 캐시를 동기화하는 오버 헤드에서 캐싱 효율성이 떨어집니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-172">When the original data changes regularly, either the cached information becomes stale very quickly or the overhead of synchronizing the cache with the original data store reduces the effectiveness of caching.</span></span>

<span data-ttu-id="3c1b9-173">캐시는 엔터티에 대한 완전한 데이터를 포함할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-173">Note that a cache does not have to include the complete data for an entity.</span></span> <span data-ttu-id="3c1b9-174">예를 들어 데이터 항목이 이름, 주소 및 계정 잔액을 가진 은행 고객과 같은 다중값 개체를 나타내는 경우 다른 요소(예: 계정 잔액)가 보다 동적인 반면 이러한 요소 중 일부는 정적(예: 이름, 주소)으로 남아 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-174">For example, if a data item represents a multivalued object such as a bank customer with a name, address, and account balance, some of these elements might remain static (such as the name and address), while others (such as the account balance) might be more dynamic.</span></span> <span data-ttu-id="3c1b9-175">이러한 상황에서 필요하다면 데이터의 정적 부분을 캐시하고 남아 있는 정보만 검색(또는 계산)하는 것이 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-175">In these situations, it can be useful to cache the static portions of the data and retrieve (or calculate) only the remaining information when it is required.</span></span>

<span data-ttu-id="3c1b9-176">캐시에 대해 미리 채우기, 요청 로딩 또는 둘의 조합 중에 적절한 것을 확인하기 위해 성능 테스트 및 사용 현황 분석을 수행하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-176">We recommend that you carry out performance testing and usage analysis to determine whether pre-population or on-demand loading of the cache, or a combination of both, is appropriate.</span></span> <span data-ttu-id="3c1b9-177">결정은 변동성 및 데이터의 사용 패턴을 기반으로 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-177">The decision should be based on the volatility and usage pattern of the data.</span></span> <span data-ttu-id="3c1b9-178">캐시 사용률 및 성능 분석은 과도한 로드를 발생하며 확장성이 뛰어나야 하는 애플리케이션에서 특히 중요합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-178">Cache utilization and performance analysis is particularly important in applications that encounter heavy loads and must be highly scalable.</span></span> <span data-ttu-id="3c1b9-179">예를 들어 확장성이 뛰어난 시나리오에서 사용량이 많은 시간에 데이터 저장소에서 부하를 줄이도록 캐시를 시드하는 것이 합리적일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-179">For example, in highly scalable scenarios it might make sense to seed the cache to reduce the load on the data store at peak times.</span></span>

<span data-ttu-id="3c1b9-180">애플리케이션이 실행될 때 계산 반복을 피하기 위해 캐싱도 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-180">Caching can also be used to avoid repeating computations while the application is running.</span></span> <span data-ttu-id="3c1b9-181">작업이 데이터를 변환하거나 복잡 한 계산을 수행하는 경우 캐시에 작업의 결과를 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-181">If an operation transforms data or performs a complicated calculation, it can save the results of the operation in the cache.</span></span> <span data-ttu-id="3c1b9-182">같은 계산을 이후에 수행해야 하는 경우 애플리케이션이 캐시에서 결과를 간단히 검색할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-182">If the same calculation is required afterward, the application can simply retrieve the results from the cache.</span></span>

<span data-ttu-id="3c1b9-183">애플리케이션이 캐시에 저장된 데이터를 수정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-183">An application can modify data that's held in a cache.</span></span> <span data-ttu-id="3c1b9-184">그러나 캐시는 언제든지 사라질 수 있는 임시 데이터 저장소라고 생각하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-184">However, we recommend thinking of the cache as a transient data store that could disappear at any time.</span></span> <span data-ttu-id="3c1b9-185">캐시에만 중요한 데이터를 저장하지 말고 원래 데이터 저장소에도 정보를 유지해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-185">Do not store valuable data in the cache only; make sure that you maintain the information in the original data store as well.</span></span> <span data-ttu-id="3c1b9-186">즉, 캐시를 사용할 수 없게 되는 경우 데이터 손실 가능성을 최소화합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-186">This means that if the cache becomes unavailable, you minimize the chance of losing data.</span></span>

### <a name="cache-highly-dynamic-data"></a><span data-ttu-id="3c1b9-187">매우 동적인 데이터 캐싱</span><span class="sxs-lookup"><span data-stu-id="3c1b9-187">Cache highly dynamic data</span></span>

<span data-ttu-id="3c1b9-188">영구 데이터 저장소에 빠르게 변경되는 정보를 저장하는 경우 시스템에 오버헤드가 부과될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-188">When you store rapidly-changing information in a persistent data store, it can impose an overhead on the system.</span></span> <span data-ttu-id="3c1b9-189">예를 들어 상태 또는 일부 다른 측정을 지속적으로 보고하는 장치를 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-189">For example, consider a device that continually reports status or some other measurement.</span></span> <span data-ttu-id="3c1b9-190">애플리케이션이 캐시된 정보가 거의 항상 오래된 기준으로 이 데이터를 캐시하지 않도록 선택하면 데이터 저장소에서 이 정보를 저장하고 검색하는 경우 동일하게 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-190">If an application chooses not to cache this data on the basis that the cached information will nearly always be outdated, then the same consideration could be true when storing and retrieving this information from the data store.</span></span> <span data-ttu-id="3c1b9-191">이 데이터를 저장하고 가져오는 데 걸리는 시간에 변경되었을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-191">In the time it takes to save and fetch this data, it might have changed.</span></span>

<span data-ttu-id="3c1b9-192">이와 같은 상황에서 영구 데이터 저장소 대신 캐시에 동적 정보를 직접 저장하는 것의 이점을 고려하세요.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-192">In a situation such as this, consider the benefits of storing the dynamic information directly in the cache instead of in the persistent data store.</span></span> <span data-ttu-id="3c1b9-193">데이터가 중요하지 않고 감사를 필요로 하지 않으면 필요에 따른 변경이 손실된 경우 중요하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-193">If the data is non-critical and does not require auditing, then it doesn't matter if the occasional change is lost.</span></span>

### <a name="manage-data-expiration-in-a-cache"></a><span data-ttu-id="3c1b9-194">캐시 데이터의 만료 관리</span><span class="sxs-lookup"><span data-stu-id="3c1b9-194">Manage data expiration in a cache</span></span>

<span data-ttu-id="3c1b9-195">대부분의 경우 캐시에 저장된 데이터는 원래 데이터 저장소에 저장된 데이터의 복사본입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-195">In most cases, data that's held in a cache is a copy of data that's held in the original data store.</span></span> <span data-ttu-id="3c1b9-196">원래 데이터 저장소의 데이터는 캐시된 이후 변경될 수 있으며 이는 캐시된 데이터의 생산성을 떨어 뜨리게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-196">The data in the original data store might change after it was cached, causing the cached data to become stale.</span></span> <span data-ttu-id="3c1b9-197">많은 캐싱 시스템을 사용하면 캐시의 데이터를 만료하고 데이터가 오래된 기간을 줄이도록 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-197">Many caching systems enable you to configure the cache to expire data and reduce the period for which data may be out of date.</span></span>

<span data-ttu-id="3c1b9-198">캐시된 데이터가 만료되면 캐시에서 제거되고 애플리케이션은 원래 데이터 저장소에서 데이터를 검색해야 합니다(새로 가져온 정보를 다시 캐시에 저장할 수 있음).</span><span class="sxs-lookup"><span data-stu-id="3c1b9-198">When cached data expires, it's removed from the cache, and the application must retrieve the data from the original data store (it can put the newly-fetched information back into cache).</span></span> <span data-ttu-id="3c1b9-199">캐시를 구성할 때 기본 만료 정책을 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-199">You can set a default expiration policy when you configure the cache.</span></span> <span data-ttu-id="3c1b9-200">많은 캐시 서비스에서 개별 개체를 프로그래밍 방식으로 캐시에 저장할 때 개별 개체에 대한 만료 기간을 규정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-200">In many cache services, you can also stipulate the expiration period for individual objects when you store them programmatically in the cache.</span></span> <span data-ttu-id="3c1b9-201">일부 캐시를 사용하여 만료 기간을 절대값으로 지정하거나 항목이 지정된 시간 내에 액세스되지 않으면 캐시에서 제거되도록 하는 상대값으로 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-201">Some caches enable you to specify the expiration period as an absolute value, or as a sliding value that causes the item to be removed from the cache if it is not accessed within the specified time.</span></span> <span data-ttu-id="3c1b9-202">이 설정은 지정된 개체에 대한 모든 캐시 만료 정책보다 우선합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-202">This setting overrides any cache-wide expiration policy, but only for the specified objects.</span></span>

> [!NOTE]
> <span data-ttu-id="3c1b9-203">캐시와 신중하게 포함된 개체에 대한 만료 기간을 고려해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-203">Consider the expiration period for the cache and the objects that it contains carefully.</span></span> <span data-ttu-id="3c1b9-204">너무 짧게 만들면 개체는 너무 빨리 만료되고 캐시를 사용하는 이점은 줄어듭니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-204">If you make it too short, objects will expire too quickly and you will reduce the benefits of using the cache.</span></span> <span data-ttu-id="3c1b9-205">너무 기간을 길게 만들면 데이터가 오래될 위험이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-205">If you make the period too long, you risk the data becoming stale.</span></span>

<span data-ttu-id="3c1b9-206">또한 데이터가 오랜 시간 동안 상주하도록 허용되면 캐시를 채우는 것이 가능합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-206">It's also possible that the cache might fill up if data is allowed to remain resident for a long time.</span></span> <span data-ttu-id="3c1b9-207">이 경우 캐시에 새 항목을 추가하는 모든 요청이 제거라는 프로세스에서 일부 항목을 강제로 제거할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-207">In this case, any requests to add new items to the cache might cause some items to be forcibly removed in a process known as eviction.</span></span> <span data-ttu-id="3c1b9-208">캐시 서비스는 가장 최근 사용(LRU)을 기초로 데이터를 제거하지만 일반적으로 이 정책을 재정의하고 항목을 제거하지 않도록 방지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-208">Cache services typically evict data on a least-recently-used (LRU) basis, but you can usually override this policy and prevent items from being evicted.</span></span> <span data-ttu-id="3c1b9-209">그러나 이 접근 방식을 사용하면 캐시에서 사용할 수 있는 메모리를 초과할 위험이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-209">However, if you adopt this approach, you risk exceeding the memory that's available in the cache.</span></span> <span data-ttu-id="3c1b9-210">애플리케이션이 캐시에 항목을 추가하려 하면 예외가 발생하여 실패합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-210">An application that attempts to add an item to the cache will fail with an exception.</span></span>

<span data-ttu-id="3c1b9-211">일부 캐싱 구현은 추가적인 제거 정책을 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-211">Some caching implementations might provide additional eviction policies.</span></span> <span data-ttu-id="3c1b9-212">여러 가지 유형의 제거 정책이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-212">There are several types of eviction policies.</span></span> <span data-ttu-id="3c1b9-213">내용은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-213">These include:</span></span>

- <span data-ttu-id="3c1b9-214">가장 최근에 사용한 데이터 제거 정책(데이터가 다시 필요하지 않다고 예상)</span><span class="sxs-lookup"><span data-stu-id="3c1b9-214">A most-recently-used policy (in the expectation that the data will not be required again).</span></span>
- <span data-ttu-id="3c1b9-215">선입선출 제거 정책(오래된 데이터를 먼저 제거)</span><span class="sxs-lookup"><span data-stu-id="3c1b9-215">A first-in-first-out policy (oldest data is evicted first).</span></span>
- <span data-ttu-id="3c1b9-216">트리거된 이벤트 기반의 명시적 제거 정책(예: 수정 중인 데이터)</span><span class="sxs-lookup"><span data-stu-id="3c1b9-216">An explicit removal policy based on a triggered event (such as the data being modified).</span></span>

### <a name="invalidate-data-in-a-client-side-cache"></a><span data-ttu-id="3c1b9-217">클라이언트 쪽 캐시에서 데이터 무효화</span><span class="sxs-lookup"><span data-stu-id="3c1b9-217">Invalidate data in a client-side cache</span></span>

<span data-ttu-id="3c1b9-218">클라이언트 쪽 캐시에 저장된 데이터는 일반적으로 클라이언트에 데이터를 제공하는 서비스의 외부로 간주됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-218">Data that's held in a client-side cache is generally considered to be outside the auspices of the service that provides the data to the client.</span></span> <span data-ttu-id="3c1b9-219">서비스에서 클라이언트가 클라이언트 쪽 캐시에서 정보를 추가하거나 제거하도록 직접 강제할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-219">A service cannot directly force a client to add or remove information from a client-side cache.</span></span>

<span data-ttu-id="3c1b9-220">즉, 잘못 구성된 캐시를 사용하는 클라이언트가 오래된 정보를 계속 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-220">This means that it's possible for a client that uses a poorly configured cache to continue using outdated information.</span></span> <span data-ttu-id="3c1b9-221">예를 들어 캐시의 만료 정책이 제대로 구현되지 않은 경우 클라이언트는 원래 데이터 원본의 정보가 변경되었을 때 로컬로 캐시된 오래된 정보를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-221">For example, if the expiration policies of the cache aren't properly implemented, a client might use outdated information that's cached locally when the information in the original data source has changed.</span></span>

<span data-ttu-id="3c1b9-222">HTTP 연결을 통해 데이터를 제공하는 웹 애플리케이션을 작성하는 경우 웹 클라이언트(예: 브라우저 또는 웹 프록시)가 가장 최근의 정보를 가져오도록 암시적으로 강요할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-222">If you are building a web application that serves data over an HTTP connection, you can implicitly force a web client (such as a browser or web proxy) to fetch the most recent information.</span></span> <span data-ttu-id="3c1b9-223">리소스가 해당 리소스의 URI에서 변경에 의해 업데이트된 경우 이렇게 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-223">You can do this if a resource is updated by a change in the URI of that resource.</span></span> <span data-ttu-id="3c1b9-224">웹 클라이언트는 일반적으로 리소스의 URI를 클라이언트 쪽 캐시에서 키로 사용하므로 URI가 변경되면 웹 클라이언트는 모든 이전에 캐시된 버전의 리소스를 무시하고 대신 새 버전을 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-224">Web clients typically use the URI of a resource as the key in the client-side cache, so if the URI changes, the web client ignores any previously cached versions of a resource and fetches the new version instead.</span></span>

## <a name="managing-concurrency-in-a-cache"></a><span data-ttu-id="3c1b9-225">캐시에서 동시성 관리</span><span class="sxs-lookup"><span data-stu-id="3c1b9-225">Managing concurrency in a cache</span></span>

<span data-ttu-id="3c1b9-226">캐시는 종종 애플리케이션의 여러 인스턴스에서 공유하도록 설계됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-226">Caches are often designed to be shared by multiple instances of an application.</span></span> <span data-ttu-id="3c1b9-227">각 애플리케이션 인스턴스가 캐시에서 데이터를 읽고 수정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-227">Each application instance can read and modify data in the cache.</span></span> <span data-ttu-id="3c1b9-228">따라서 모든 공유 데이터 저장소와 함께 발생하는 동일한 동시성 문제는 캐시에도 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-228">Consequently, the same concurrency issues that arise with any shared data store also apply to a cache.</span></span> <span data-ttu-id="3c1b9-229">애플리케이션이 캐시에 보유하는 데이터를 수정해야 하는 경우 애플리케이션의 한 인스턴스가 만드는 업데이트가 다른 인스턴스가 만든 변경을 덮어쓰지 않도록 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-229">In a situation where an application needs to modify data that's held in the cache, you might need to ensure that updates made by one instance of the application do not overwrite the changes made by another instance.</span></span>

<span data-ttu-id="3c1b9-230">데이터의 특성 및 충돌 가능성에 따라 동시성에 대한 두 방법 중 하나를 채택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-230">Depending on the nature of the data and the likelihood of collisions, you can adopt one of two approaches to concurrency:</span></span>

- <span data-ttu-id="3c1b9-231">**낙관적**.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-231">**Optimistic**.</span></span> <span data-ttu-id="3c1b9-232">애플리케이션을 업데이트하기 바로 전에 캐시에서 데이터가 검색된 이후 변경되지 않았는지를 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-232">Immediately prior to updating the data, the application checks to see whether the data in the cache has changed since it was retrieved.</span></span> <span data-ttu-id="3c1b9-233">데이터가 여전히 동일한 경우 변경이 가능합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-233">If the data is still the same, the change can be made.</span></span> <span data-ttu-id="3c1b9-234">그렇지 않으면 애플리케이션에서 업데이트 여부를 결정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-234">Otherwise, the application has to decide whether to update it.</span></span> <span data-ttu-id="3c1b9-235">(이를 결정하는 비즈니스 논리는 애플리케이션에 특정됨) 이 방법은 업데이트가 드물거나 충돌이 발생할 가능성이 없는 상황에 적합합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-235">(The business logic that drives this decision will be application-specific.) This approach is suitable for situations where updates are infrequent, or where collisions are unlikely to occur.</span></span>
- <span data-ttu-id="3c1b9-236">**비관적**.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-236">**Pessimistic**.</span></span> <span data-ttu-id="3c1b9-237">애플리케이션이 데이터를 검색할 때 다른 인스턴스가 데이터를 변경하지 못하도록 캐시에서 잠급니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-237">When it retrieves the data, the application locks it in the cache to prevent another instance from changing it.</span></span> <span data-ttu-id="3c1b9-238">이렇게 하면 충돌이 발생할 수 없지만 동일한 데이터를 처리해야 하는 다른 인스턴스를 차단할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-238">This process ensures that collisions cannot occur, but they can also block other instances that need to process the same data.</span></span> <span data-ttu-id="3c1b9-239">비관적 동시성은 솔루션의 확장성에 영향을 줄 수 있으며 단기 실행 작업에 대해서만 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-239">Pessimistic concurrency can affect the scalability of a solution and is recommended only for short-lived operations.</span></span> <span data-ttu-id="3c1b9-240">특히 애플리케이션이 캐시에 여러 항목을 업데이트하고 이러한 변경 내용이 일관되게 적용되었는지 확인해야 하는 경우 이 방법이 충돌 가능성이 더 높은 상황에 적절할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-240">This approach might be appropriate for situations where collisions are more likely, especially if an application updates multiple items in the cache and must ensure that these changes are applied consistently.</span></span>

### <a name="implement-high-availability-and-scalability-and-improve-performance"></a><span data-ttu-id="3c1b9-241">고가용성 및 확장성 구현과 성능 향상</span><span class="sxs-lookup"><span data-stu-id="3c1b9-241">Implement high availability and scalability, and improve performance</span></span>

<span data-ttu-id="3c1b9-242">데이터의 기본 리포지토리로 캐시를 사용하지 않도록 합니다. 이는 캐시가 채워지면 원래 데이터 저장소의 역할입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-242">Avoid using a cache as the primary repository of data; this is the role of the original data store from which the cache is populated.</span></span> <span data-ttu-id="3c1b9-243">원래 데이터 저장소는 데이터의 지 속성 보장을 담당합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-243">The original data store is responsible for ensuring the persistence of the data.</span></span>

<span data-ttu-id="3c1b9-244">공유 캐시 서비스의 가용성에 대한 높은 종속성을 솔루션에 도입하지 않도록 주의하십시오.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-244">Be careful not to introduce critical dependencies on the availability of a shared cache service into your solutions.</span></span> <span data-ttu-id="3c1b9-245">공유 캐시를 제공하는 서비스를 사용할 수 없는 경우 애플리케이션이 계속 작동될 수 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-245">An application should be able to continue functioning if the service that provides the shared cache is unavailable.</span></span> <span data-ttu-id="3c1b9-246">캐시 서비스가 다시 시작되기를 기다리는 동안 애플리케이션이 중단되거나 실패하지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-246">The application should not hang or fail while waiting for the cache service to resume.</span></span>

<span data-ttu-id="3c1b9-247">따라서 애플리케이션은 캐시 서비스의 가용성을 검색하여 준비하고 캐시에 액세스할 수 없는 경우 원래 데이터 저장소로 대체해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-247">Therefore, the application must be prepared to detect the availability of the cache service and fall back to the original data store if the cache is inaccessible.</span></span> <span data-ttu-id="3c1b9-248">[회로 차단기 패턴](../patterns/circuit-breaker.md)은 시나리오를 처리하는데 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-248">The [Circuit-Breaker pattern](../patterns/circuit-breaker.md) is useful for handling this scenario.</span></span> <span data-ttu-id="3c1b9-249">[캐시 배제 패턴](../patterns/cache-aside.md)과 같은 전략을 따라 캐시를 제공하는 서비스를 복구할 수 있으며, 원래 데이터 저장소에서 데이터를 읽을 때 서비스를 사용할 수 있게 되면 캐시를 다시 채울 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-249">The service that provides the cache can be recovered, and once it becomes available, the cache can be repopulated as data is read from the original data store, following a strategy such as the [Cache-aside pattern](../patterns/cache-aside.md).</span></span>

<span data-ttu-id="3c1b9-250">그러나 캐시를 일시적으로 사용할 수 없을 때 애플리케이션이 다시 원래 데이터 저장소로 되돌아가는 경우 시스템의 확장성에 영향을 미칠 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-250">However, there might be a scalability impact on the system if the application falls back to the original data store when the cache is temporarily unavailable.</span></span> <span data-ttu-id="3c1b9-251">데이터 저장소가 복구 중인 동안 원래 데이터 저장소에 데이터 요청이 너무 많아 시간이 초과되고 연결되지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-251">While the data store is being recovered, the original data store could be swamped with requests for data, resulting in timeouts and failed connections.</span></span>

<span data-ttu-id="3c1b9-252">모든 애플리케이션 인스턴스가 액세스하는 공유 캐시와 함께 애플리케이션의 각 인스턴스에 로컬, 개인 캐시를 구현하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-252">Consider implementing a local, private cache in each instance of an application, together with the shared cache that all application instances access.</span></span> <span data-ttu-id="3c1b9-253">애플리케이션이 항목을 검색하는 경우 먼저 로컬 캐시에서 다음 공유 캐시에서 마지막으로 원래 데이터 저장소에서 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-253">When the application retrieves an item, it can check first in its local cache, then in the shared cache, and finally in the original data store.</span></span> <span data-ttu-id="3c1b9-254">공유 캐시 또는 데이터베이스(공유 캐시를 사용할 수 없는 경우)의 데이터를 사용하여 로컬 캐시를 채울 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-254">The local cache can be populated using the data in either the shared cache, or in the database if the shared cache is unavailable.</span></span>

<span data-ttu-id="3c1b9-255">이 방법은 로컬 캐시가 공유 캐시와 관련하여 너무 부실해지지 않도록 신중하게 구성해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-255">This approach requires careful configuration to prevent the local cache from becoming too stale with respect to the shared cache.</span></span> <span data-ttu-id="3c1b9-256">그러나 공유 캐시에 연결할 수 없는 경우 로컬 캐시가 버퍼 역할을 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-256">However, the local cache acts as a buffer if the shared cache is unreachable.</span></span> <span data-ttu-id="3c1b9-257">그림 3이 이 구조를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-257">Figure 3 shows this structure.</span></span>

![공유 캐시와 함께 로컬 비공개 캐시 사용](./images/caching/Caching3.png)

<span data-ttu-id="3c1b9-259">*그림 3: 공유 캐시와 함께 로컬 비공개 캐시 사용.*</span><span class="sxs-lookup"><span data-stu-id="3c1b9-259">*Figure 3: Using a local private cache with a shared cache.*</span></span>

<span data-ttu-id="3c1b9-260">상대적으로 수명이 긴 데이터를 보유하는 대형 캐시를 지원하려면 캐시를 사용할 수 없게 된 경우 일부 캐시 서비스가 자동 장애 조치를 구현하는 고가용성 옵션을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-260">To support large caches that hold relatively long-lived data, some cache services provide a high-availability option that implements automatic failover if the cache becomes unavailable.</span></span> <span data-ttu-id="3c1b9-261">이 방법은 일반적으로 주 캐시 서버에 저장된 캐시된 데이터를 보조 캐시 서버로 복제하고 주 서버에 오류가 발생하거나 연결이 손실되는 경우 보조 서버로 전환하는 기능을 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-261">This approach typically involves replicating the cached data that's stored on a primary cache server to a secondary cache server, and switching to the secondary server if the primary server fails or connectivity is lost.</span></span>

<span data-ttu-id="3c1b9-262">여러 대상에 쓰기와 연결된 대기 시간을 줄이려면 주 서버의 캐시에 데이터를 쓸 때 보조 서버에 복제가 비동기적으로 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-262">To reduce the latency that's associated with writing to multiple destinations, the replication to the secondary server might occur asynchronously when data is written to the cache on the primary server.</span></span> <span data-ttu-id="3c1b9-263">이 방법은 일부 캐시된 정보에 오류가 발생하여 손실될 가능성으로 이어지지만 캐시의 전체 크기에 비해 이 데이터의 비율이 작을 것입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-263">This approach leads to the possibility that some cached information might be lost in the event of a failure, but the proportion of this data shuld be small compared to the overall size of the cache.</span></span>

<span data-ttu-id="3c1b9-264">공유 캐시가 크면 노드에 캐시된 데이터를 분할하여 경합 가능성을 줄이고 확장성을 개선하는 데 도움이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-264">If a shared cache is large, it might be beneficial to partition the cached data across nodes to reduce the chances of contention and improve scalability.</span></span> <span data-ttu-id="3c1b9-265">많은 공유 캐시는 노드를 동적으로 추가(및 제거)하고 파티션에 데이터 균형을 다시 조정하는 기능을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-265">Many shared caches support the ability to dynamically add (and remove) nodes and rebalance the data across partitions.</span></span> <span data-ttu-id="3c1b9-266">이 방법은 노드 컬렉션이 클라이언트 애플리케이션에 원활한 단일 캐시로 표시되는 클러스터링을 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-266">This approach might involve clustering, in which the collection of nodes is presented to client applications as a seamless, single cache.</span></span> <span data-ttu-id="3c1b9-267">하지만 내부적으로 데이터는 부하를 균등하게 분산하는 미리 정의된 배포 전략에 따라 노드 간에 분산됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-267">Internally, however, the data is dispersed between nodes following a predefined distribution strategy that balances the load evenly.</span></span> <span data-ttu-id="3c1b9-268">가능한 분할 전략에 대한 자세한 내용은 [데이터 분할 지침](https://msdn.microsoft.com/library/dn589795.aspx)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-268">For more information about possible partitioning strategies, see [Data partitioning guidance](https://msdn.microsoft.com/library/dn589795.aspx).</span></span>

<span data-ttu-id="3c1b9-269">또한 클러스터링을 사용하면 캐시의 가용성을 높일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-269">Clustering can also increase the availability of the cache.</span></span> <span data-ttu-id="3c1b9-270">노드가 실패해도 캐시의 나머지는 계속 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-270">If a node fails, the remainder of the cache is still accessible.</span></span> <span data-ttu-id="3c1b9-271">클러스터링은 복제 및 장애 조치(Failover)와 함께 자주 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-271">Clustering is frequently used in conjunction with replication and failover.</span></span> <span data-ttu-id="3c1b9-272">각 노드는 복제할 수 있으며 노드가 실패하면 복제본이 신속하게 온라인 상태로 전환될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-272">Each node can be replicated, and the replica can be quickly brought online if the node fails.</span></span>

<span data-ttu-id="3c1b9-273">많은 읽기 및 쓰기 작업은 단일 데이터 값이나 개체를 포함할 가능성이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-273">Many read and write operations are likely to involve single data values or objects.</span></span> <span data-ttu-id="3c1b9-274">그러나 필요할 때 많은 양의 데이터를 신속하게 저장하거나 검색해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-274">However, at times it might be necessary to store or retrieve large volumes of data quickly.</span></span> <span data-ttu-id="3c1b9-275">예를 들어 캐시를 시드하면 수백 또는 수천 개의 항목을 캐시에 작성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-275">For example, seeding a cache could involve writing hundreds or thousands of items to the cache.</span></span> <span data-ttu-id="3c1b9-276">또한 애플리케이션은 동일한 요청의 일부로 캐시에서 많은 수의 관련 항목을 검색해야 할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-276">An application might also need to retrieve a large number of related items from the cache as part of the same request.</span></span>

<span data-ttu-id="3c1b9-277">많은 대규모 캐시는 이러한 목적을 위해 배치 작업을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-277">Many large-scale caches provide batch operations for these purposes.</span></span> <span data-ttu-id="3c1b9-278">이를 통해 클라이언트 애플리케이션에서 대량의 항목을 단일 요청으로 패키지할 수 있고 많은 수의 작은 요청을 수행하는 것과 관련된 오버헤드를 줄일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-278">This enables a client application to package up a large volume of items into a single request and reduces the overhead that's associated with performing a large number of small requests.</span></span>

## <a name="caching-and-eventual-consistency"></a><span data-ttu-id="3c1b9-279">캐싱 및 결과적 일관성</span><span class="sxs-lookup"><span data-stu-id="3c1b9-279">Caching and eventual consistency</span></span>

<span data-ttu-id="3c1b9-280">캐시 배제 패턴이 작동하려면 캐시를 채우는 애플리케이션의 인스턴스가 최근의 일관된 버전의 데이터에 액세스해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-280">For the cache-aside pattern to work, the instance of the application that populates the cache must have access to the most recent and consistent version of the data.</span></span> <span data-ttu-id="3c1b9-281">결과적인 일관성(예; 복제된 데이터 저장소)을 구현하는 시스템에서는 이 경우가 아닐 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-281">In a system that implements eventual consistency (such as a replicated data store) this might not be the case.</span></span>

<span data-ttu-id="3c1b9-282">애플리케이션의 한 인스턴스는 데이터 항목을 수정하고 해당 항목의 캐시된 버전을 무효화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-282">One instance of an application could modify a data item and invalidate the cached version of that item.</span></span> <span data-ttu-id="3c1b9-283">애플리케이션의 또 다른 인스턴스는 캐시 누락을 일으키는 캐시에서 이 항목 읽기를 시도하여 데이터 저장소에서 데이터를 읽고 캐시에 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-283">Another instance of the application might attempt to read this item from a cache, which causes a cache-miss, so it reads the data from the data store and adds it to the cache.</span></span> <span data-ttu-id="3c1b9-284">그러나 데이터 저장소가 다른 복제본과 완벽하게 동기화되지 않은 경우 애플리케이션 인스턴스는 이전 값과 함께 캐시를 읽고 채울 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-284">However, if the data store has not been fully synchronized with the other replicas, the application instance could read and populate the cache with the old value.</span></span>

<span data-ttu-id="3c1b9-285">데이터 일관성을 처리하는 방법에 대한 자세한 내용은 [데이터 일관성 입문서](https://msdn.microsoft.com/library/dn589800.aspx)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-285">For more information about handling data consistency, see the [Data consistency primer](https://msdn.microsoft.com/library/dn589800.aspx).</span></span>

### <a name="protect-cached-data"></a><span data-ttu-id="3c1b9-286">캐시된 데이터 보호</span><span class="sxs-lookup"><span data-stu-id="3c1b9-286">Protect cached data</span></span>

<span data-ttu-id="3c1b9-287">사용한 캐시 서비스와 관련 없이 무단 액세스에서 캐시에 보관된 데이터를 보호하는 방법을 고려하세요.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-287">Irrespective of the cache service you use, consider how to protect the data that's held in the cache from unauthorized access.</span></span> <span data-ttu-id="3c1b9-288">두 개의 주요 관심사가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-288">There are two main concerns:</span></span>

- <span data-ttu-id="3c1b9-289">캐시 데이터의 개인 정보 보호</span><span class="sxs-lookup"><span data-stu-id="3c1b9-289">The privacy of the data in the cache.</span></span>
- <span data-ttu-id="3c1b9-290">캐시와 캐시를 사용하는 애플리케이션 간에 흐르는 데이터의 개인 정보 보호</span><span class="sxs-lookup"><span data-stu-id="3c1b9-290">The privacy of data as it flows between the cache and the application that's using the cache.</span></span>

<span data-ttu-id="3c1b9-291">캐시의 데이터를 보호하기 위해 캐시 서비스에서 애플리케이션이 다음을 지정하도록 요구하는 인증 메커니즘을 구현할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-291">To protect data in the cache, the cache service might implement an authentication mechanism that requires that applications specify the following:</span></span>

- <span data-ttu-id="3c1b9-292">캐시 데이터에 액세스할 수 있는 ID</span><span class="sxs-lookup"><span data-stu-id="3c1b9-292">Which identities can access data in the cache.</span></span>
- <span data-ttu-id="3c1b9-293">이러한 ID가 수행할 수 있는 작업(읽기 및 쓰기)</span><span class="sxs-lookup"><span data-stu-id="3c1b9-293">Which operations (read and write) that these identities are allowed to perform.</span></span>

<span data-ttu-id="3c1b9-294">읽기 및 쓰기 데이터와 관련된 오버헤드를 줄이려면 ID에 캐시의 읽기 및/또는 쓰기 권한을 부여한 후에 ID가 캐시에서 모든 데이터를 사용할 수 있게 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-294">To reduce overhead that's associated with reading and writing data, after an identity has been granted write and/or read access to the cache, that identity can use any data in the cache.</span></span>

<span data-ttu-id="3c1b9-295">캐시된 데이터의 하위 집합에 대한 액세스를 제한해야 하는 경우 다음 중 하나를 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-295">If you need to restrict access to subsets of the cached data, you can do one of the following:</span></span>

- <span data-ttu-id="3c1b9-296">다양한 캐시 서버를 사용하여 캐시를 파티션으로 분할하고 사용하도록 해야 하는 파티션에 대한 ID에 액세스 권한을 부여합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-296">Split the cache into partitions (by using different cache servers) and only grant access to identities for the partitions that they should be allowed to use.</span></span>
- <span data-ttu-id="3c1b9-297">다른 키를 사용하여 각 하위 집합에서 데이터를 암호화하고 각 하위 집합에 액세스해야 하는 ID에만 암호화 키를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-297">Encrypt the data in each subset by using different keys, and provide the encryption keys only to identities that should have access to each subset.</span></span> <span data-ttu-id="3c1b9-298">클라이언트 애플리케이션은 캐시에서 모든 데이터를 검색할 수 있지만 키를 가진 경우 데이터를 해독할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-298">A client application might still be able to retrieve all of the data in the cache, but it will only be able to decrypt the data for which it has the keys.</span></span>

<span data-ttu-id="3c1b9-299">또한 데이터가 캐시 안팎으로 이동할 때 해당 데이터를 보호해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-299">You must also protect the data as it flows in and out of the cache.</span></span> <span data-ttu-id="3c1b9-300">이를 위해 클라이언트 애플리케이션에서 캐시에 연결하는 데 사용하는 네트워크 인프라에서 제공하는 보안 기능을 사용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-300">To do this, you depend on the security features provided by the network infrastructure that client applications use to connect to the cache.</span></span> <span data-ttu-id="3c1b9-301">클라이언트 애플리케이션을 호스트하는 같은 조직 내의 온사이트 서버를 사용하여 캐시가 구현되면 추가 단계를 수행하기 위해 네트워크 자체의 격리가 필요하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-301">If the cache is implemented using an on-site server within the same organization that hosts the client applications, then the isolation of the network itself might not require you to take additional steps.</span></span> <span data-ttu-id="3c1b9-302">캐시가 원격에 위치하고 공용 네트워크를 통해 TCP 또는 HTTP 연결(인터넷과 같은)이 필요한 경우 SSL을 구현하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-302">If the cache is located remotely and requires a TCP or HTTP connection over a public network (such as the Internet), consider implementing SSL.</span></span>

## <a name="considerations-for-implementing-caching-in-azure"></a><span data-ttu-id="3c1b9-303">Azure에서 캐싱을 구현하기 위한 고려 사항</span><span class="sxs-lookup"><span data-stu-id="3c1b9-303">Considerations for implementing caching in Azure</span></span>

<span data-ttu-id="3c1b9-304">[Azure Redis Cache](/azure/redis-cache/)는 Azure 데이터 센터에서 서비스로 실행되는 오픈 소스 Redis 캐시를 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-304">[Azure Redis Cache](/azure/redis-cache/) is an implementation of the open source Redis cache that runs as a service in an Azure datacenter.</span></span> <span data-ttu-id="3c1b9-305">애플리케이션이 웹사이트, 클라우드 서비스로 구현되거나 Azure 가상 머신 내에서 구현되거나 무관히 모든 Azure 애플리케이션에서 액세스할 수 있는 캐싱 서비스를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-305">It provides a caching service that can be accessed from any Azure application, whether the application is implemented as a cloud service, a website, or inside an Azure virtual machine.</span></span> <span data-ttu-id="3c1b9-306">적절한 액세스 키가 있는 클라이언트 애플리케이션이 캐시를 공유할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-306">Caches can be shared by client applications that have the appropriate access key.</span></span>

<span data-ttu-id="3c1b9-307">Azure Redis Cache는 가용성, 확장성 및 보안을 제공하는 고성능 캐싱 솔루션입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-307">Azure Redis Cache is a high-performance caching solution that provides availability, scalability and security.</span></span> <span data-ttu-id="3c1b9-308">일반적으로 하나 이상의 전용 컴퓨터에 분산된 서비스로 실행됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-308">It typically runs as a service spread across one or more dedicated machines.</span></span> <span data-ttu-id="3c1b9-309">빠른 액세스를 보장하기 위해 메모리에 가능한 한 많은 정보를 저장하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-309">It attempts to store as much information as it can in memory to ensure fast access.</span></span> <span data-ttu-id="3c1b9-310">이 아키텍처는 느린 I/O 작업을 수행할 필요를 줄여 짧은 대기 시간과 높은 처리량을 제공하도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-310">This architecture is intended to provide low latency and high throughput by reducing the need to perform slow I/O operations.</span></span>

<span data-ttu-id="3c1b9-311">Azure Redis Cache는 클라이언트 애플리케이션에서 사용되는 많고 다양한 API와 호환됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-311">Azure Redis Cache is compatible with many of the various APIs that are used by client applications.</span></span> <span data-ttu-id="3c1b9-312">온-프레미스를 실행하는 Azure Redis Cache를 이미 사용하는 기존 애플리케이션이 있는 경우 Azure Redis Cache는 클라우드에서 캐시에 빠른 마이그레이션 경로를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-312">If you have existing applications that already use Azure Redis Cache running on-premises, the Azure Redis Cache provides a quick migration path to caching in the cloud.</span></span>

### <a name="features-of-redis"></a><span data-ttu-id="3c1b9-313">Redis의 기능</span><span class="sxs-lookup"><span data-stu-id="3c1b9-313">Features of Redis</span></span>

<span data-ttu-id="3c1b9-314">Redis는 간단한 캐시 서버 이상입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-314">Redis is more than a simple cache server.</span></span> <span data-ttu-id="3c1b9-315">여러 일반적인 시나리오를 지원하는 광범위한 명령 집합과 함께 분산된 메모리 내 데이터베이스를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-315">It provides a distributed in-memory database with an extensive command set that supports many common scenarios.</span></span> <span data-ttu-id="3c1b9-316">이러한 내용은 이 문서의 뒷부분, Redis 캐싱 사용 섹션에 설명되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-316">These are described later in this document, in the section Using Redis caching.</span></span> <span data-ttu-id="3c1b9-317">이 섹션은 Redis를 제공하는 주요 기능 중 일부를 요약합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-317">This section summarizes some of the key features that Redis provides.</span></span>

### <a name="redis-as-an-in-memory-database"></a><span data-ttu-id="3c1b9-318">메모리 내 데이터베이스인 Redis</span><span class="sxs-lookup"><span data-stu-id="3c1b9-318">Redis as an in-memory database</span></span>

<span data-ttu-id="3c1b9-319">Redis는 읽기 및 쓰기 작업을 둘 다 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-319">Redis supports both read and write operations.</span></span> <span data-ttu-id="3c1b9-320">Redis에서 쓰기는 로컬 스냅숏 파일 또는 추가 전용 로그 파일 중 하나에 주기적으로 저장되어 시스템 오류로부터 보호될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-320">In Redis, writes can be protected from system failure either by being stored periodically in a local snapshot file or in an append-only log file.</span></span> <span data-ttu-id="3c1b9-321">많은 캐시(일시적인 데이터 저장소로 고려해야 함)는 이와 다른 경우입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-321">This is not the case in many caches (which should be considered transitory data stores).</span></span>

<span data-ttu-id="3c1b9-322">모든 쓰기는 비동기적이며 클라이언트가 데이터를 읽고 쓰는 것을 차단하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-322">All writes are asynchronous and do not block clients from reading and writing data.</span></span> <span data-ttu-id="3c1b9-323">Redis이 실행되기 시작하는 경우 스냅숏 또는 로그 파일에서 데이터를 읽고 메모리 내 캐시를 생성하는데 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-323">When Redis starts running, it reads the data from the snapshot or log file and uses it to construct the in-memory cache.</span></span> <span data-ttu-id="3c1b9-324">자세한 내용은 Redis 웹 사이트에서 [Redis 지속성](https://redis.io/topics/persistence)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-324">For more information, see [Redis persistence](https://redis.io/topics/persistence) on the Redis website.</span></span>

> [!NOTE]
> <span data-ttu-id="3c1b9-325">Redis는 치명적인 오류가 발생한 경우 모든 쓰기가 반드시 저장된다고 할 수 없지만 최악의 경우 몇 초 분량의 데이터만 손실될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-325">Redis does not guarantee that all writes will be saved in the event of a catastrophic failure, but at worst you might lose only a few seconds worth of data.</span></span> <span data-ttu-id="3c1b9-326">캐시는 신뢰할 수 있는 데이터 소스의 역할을 하며 캐시를 사용하여 중요한 데이터를 적절한 데이터 저장소에 성공적으로 저장하도록 하는 것이 애플리케이션의 책임입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-326">Remember that a cache is not intended to act as an authoritative data source, and it is the responsibility of the applications using the cache to ensure that critical data is saved successfully to an appropriate data store.</span></span> <span data-ttu-id="3c1b9-327">자세한 내용은 [캐시 배제 패턴](../patterns/cache-aside.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-327">For more information, see the [Cache-aside pattern](../patterns/cache-aside.md).</span></span>

#### <a name="redis-data-types"></a><span data-ttu-id="3c1b9-328">Redis 데이터 형식</span><span class="sxs-lookup"><span data-stu-id="3c1b9-328">Redis data types</span></span>

<span data-ttu-id="3c1b9-329">Redis는 해시, 목록 및 집합 같은 단순 형식 또는 복잡한 데이터 구조를 포함할 수 있는 키-값 저장소를 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-329">Redis is a key-value store, where values can contain simple types or complex data structures such as hashes, lists, and sets.</span></span> <span data-ttu-id="3c1b9-330">이러한 데이터 유형에 대한 소규모 작업 집합을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-330">It supports a set of atomic operations on these data types.</span></span> <span data-ttu-id="3c1b9-331">키는 영구적이거나 라이브가 제한된 시간으로 태그되어 그 시점에 키 및 해당 값이 자동으로 캐시에서 제거됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-331">Keys can be permanent or tagged with a limited time-to-live, at which point the key and its corresponding value are automatically removed from the cache.</span></span> <span data-ttu-id="3c1b9-332">Redis 키와 값에 대한 자세한 내용은 Redis 웹 사이트에서 [Redis 데이터 형식 및 추상화 소개](https://redis.io/topics/data-types-intro) 페이지를 방문하세요.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-332">For more information about Redis keys and values, visit the page [An introduction to Redis data types and abstractions](https://redis.io/topics/data-types-intro) on the Redis website.</span></span>

#### <a name="redis-replication-and-clustering"></a><span data-ttu-id="3c1b9-333">Redis 복제 및 클러스터링</span><span class="sxs-lookup"><span data-stu-id="3c1b9-333">Redis replication and clustering</span></span>

<span data-ttu-id="3c1b9-334">Redis는 가용성을 보장하고 처리량을 유지하도록 마스터/하위 복제를 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-334">Redis supports master/subordinate replication to help ensure availability and maintain throughput.</span></span> <span data-ttu-id="3c1b9-335">Redis 마스터 노드에 대한 쓰기 작업이 하나 이상의 하위 노드에 복제됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-335">Write operations to a Redis master node are replicated to one or more subordinate nodes.</span></span> <span data-ttu-id="3c1b9-336">읽기 작업은 마스터 또는 하위 항목 중 하나에서 제공될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-336">Read operations can be served by the master or any of the subordinates.</span></span>

<span data-ttu-id="3c1b9-337">네트워크 분할이 발생하는 경우 연결이 다시 설정될 때 부하 직원은 데이터를 제공하고 그런 다음 투명하게 마스터와 재동기화합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-337">In the event of a network partition, subordinates can continue to serve data and then transparently resynchronize with the master when the connection is reestablished.</span></span> <span data-ttu-id="3c1b9-338">자세한 내용은 Redis 웹 사이트에서 [복제](https://redis.io/topics/replication) 페이지를 방문하십시오.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-338">For further details, visit the [Replication](https://redis.io/topics/replication) page on the Redis website.</span></span>

<span data-ttu-id="3c1b9-339">또한 Redis는 클러스터링을 제공하여 데이터를 서버의 분할로 투명하게 분할하고 로드를 분산시킬 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-339">Redis also provides clustering, which enables you to transparently partition data into shards across servers and spread the load.</span></span> <span data-ttu-id="3c1b9-340">이 기능은 캐시 크기의 증가에 따라 새 Redis 서버가 추가될 수 있고 데이터가 다시 분할되므로 확장성을 향상합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-340">This feature improves scalability, because new Redis servers can be added and the data repartitioned as the size of the cache increases.</span></span>

<span data-ttu-id="3c1b9-341">또한 클러스터의 각 서버는 마스터/하위 복제를 사용하여 복제할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-341">Furthermore, each server in the cluster can be replicated by using master/subordinate replication.</span></span> <span data-ttu-id="3c1b9-342">이렇게 하면 클러스터의 각 노드에 걸친 가용성을 보장합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-342">This ensures availability across each node in the cluster.</span></span> <span data-ttu-id="3c1b9-343">클러스터링 및 분할에 대한 자세한 내용은 Redis 웹 사이트에서 [Redis 클러스터 자습서 페이지](https://redis.io/topics/cluster-tutorial)를 방문하세요.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-343">For more information about clustering and sharding, visit the [Redis cluster tutorial page](https://redis.io/topics/cluster-tutorial) on the Redis website.</span></span>

### <a name="redis-memory-use"></a><span data-ttu-id="3c1b9-344">Redis 메모리 사용</span><span class="sxs-lookup"><span data-stu-id="3c1b9-344">Redis memory use</span></span>

<span data-ttu-id="3c1b9-345">Redis 캐시는 호스트 컴퓨터에서 사용할 수 있는 리소스에 따라 한정된 크기가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-345">A Redis cache has a finite size that depends on the resources available on the host computer.</span></span> <span data-ttu-id="3c1b9-346">Redis 서버를 구성할 때 사용할 수 있는 메모리의 최대 크기를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-346">When you configure a Redis server, you can specify the maximum amount of memory it can use.</span></span> <span data-ttu-id="3c1b9-347">또한 Redis 캐시에 있는 키에 만료 시간이 있도록 구성할 수 있으며 그 이후 캐시에서 자동으로 제거됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-347">You can also configure a key in a Redis cache to have an expiration time, after which it is automatically removed from the cache.</span></span> <span data-ttu-id="3c1b9-348">이 기능은 메모리 내 캐시가 오래되거나 유효하지 않은 데이터로 채워지지 않도록 방지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-348">This feature can help prevent the in-memory cache from filling with old or stale data.</span></span>

<span data-ttu-id="3c1b9-349">메모리가 차면 Redis는 정책의 수에 따라서 자동으로 키와 값을 제거할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-349">As memory fills up, Redis can automatically evict keys and their values by following a number of policies.</span></span> <span data-ttu-id="3c1b9-350">기본값은 LRU(오래 전에 사용한 항목)이지만 임의로 키 제거 또는 제거 해제와 같은 기타 정책을 선택할 수 있습니다(이 경우 캐시가 가득 차면 여기에 항목을 추가하는 시도는 실패함).</span><span class="sxs-lookup"><span data-stu-id="3c1b9-350">The default is LRU (least recently used), but you can also select other policies such as evicting keys at random or turning off eviction altogether (in which, case attempts to add items to the cache fail if it is full).</span></span> <span data-ttu-id="3c1b9-351">자세한 정보는 [Redis를 LRU 캐시로 사용](https://redis.io/topics/lru-cache) 페이지를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-351">The page [Using Redis as an LRU cache](https://redis.io/topics/lru-cache) provides more information.</span></span>

### <a name="redis-transactions-and-batches"></a><span data-ttu-id="3c1b9-352">Redis 트랜잭션 및 배치</span><span class="sxs-lookup"><span data-stu-id="3c1b9-352">Redis transactions and batches</span></span>

<span data-ttu-id="3c1b9-353">Redis는 클라이언트 애플리케이션을 활성화하여 캐시의 데이터를 원자성 트랜잭션으로 읽고 쓰는 일련의 작업을 제출합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-353">Redis enables a client application to submit a series of operations that read and write data in the cache as an atomic transaction.</span></span> <span data-ttu-id="3c1b9-354">트랜잭션에서 모든 명령은 순차적으로 실행되도록 보장되고 다른 동시 클라이언트가 발급한 어떤 명령도 이들 간에 섞이지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-354">All the commands in the transaction are guaranteed to run sequentially, and no commands issued by other concurrent clients will be interwoven between them.</span></span>

<span data-ttu-id="3c1b9-355">그러나 관계형 데이터베이스가 이것을 수행하는 경우 true 트랜잭션이 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-355">However, these are not true transactions as a relational database would perform them.</span></span> <span data-ttu-id="3c1b9-356">트랜잭션 처리는 두 단계로 구성됩니다. 첫 번째는 명령이 대기할 때이고 두 번째는 명령이 실행될 때입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-356">Transaction processing consists of two stages--the first is when the commands are queued, and the second is when the commands are run.</span></span> <span data-ttu-id="3c1b9-357">명령 큐 단계 동안 클라이언트가 트랜잭션을 구성하는 명령을 전송합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-357">During the command queuing stage, the commands that comprise the transaction are submitted by the client.</span></span> <span data-ttu-id="3c1b9-358">이 시점에서 일종의 오류가 발생하는 경우(구문 오류 또는 잘못된 매개 변수 개수 등) Redis는 전체 트랜잭션 처리를 거부하고 삭제합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-358">If some sort of error occurs at this point (such as a syntax error, or the wrong number of parameters) then Redis refuses to process the entire transaction and discards it.</span></span>

<span data-ttu-id="3c1b9-359">실행 단계 동안 Redis는 큐에 대기 중인 각 명령을 차례로 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-359">During the run phase, Redis performs each queued command in sequence.</span></span> <span data-ttu-id="3c1b9-360">이 단계 동안 명령이 실패한 경우 Redis는 큐에 대기 중인 다음 명령을 사용하여 계속되고 이미 실행된 명령의 효과를 롤백하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-360">If a command fails during this phase, Redis continues with the next queued command and does not roll back the effects of any commands that have already been run.</span></span> <span data-ttu-id="3c1b9-361">이 간단해진 트랜잭션의 형태가 성능을 유지하고 경합으로 발생하는 성능 문제를 방지합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-361">This simplified form of transaction helps to maintain performance and avoid performance problems that are caused by contention.</span></span>

<span data-ttu-id="3c1b9-362">Redis는 낙관적 잠금이라는 형태를 구현하여 일관성 유지를 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-362">Redis does implement a form of optimistic locking to assist in maintaining consistency.</span></span> <span data-ttu-id="3c1b9-363">Redis를 이용한 트랜잭션 및 잠금에 대한 자세한 정보는 Redis 웹 사이트에서 [트랜잭션 페이지](https://redis.io/topics/transactions)를 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-363">For detailed information about transactions and locking with Redis, visit the [Transactions page](https://redis.io/topics/transactions) on the Redis website.</span></span>

<span data-ttu-id="3c1b9-364">또한 Redis는 요청의 비 트랜잭션 배치를 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-364">Redis also supports non-transactional batching of requests.</span></span> <span data-ttu-id="3c1b9-365">Redis 서버에 명령을 보내기 위해 클라이언트가 사용하는 Redis 프로토콜을 사용하면 클라이언트가 동일한 요청의 일부로써 일련의 작업을 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-365">The Redis protocol that clients use to send commands to a Redis server enables a client to send a series of operations as part of the same request.</span></span> <span data-ttu-id="3c1b9-366">이는 네트워크에서 패킷 조각화를 줄일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-366">This can help to reduce packet fragmentation on the network.</span></span> <span data-ttu-id="3c1b9-367">배치가 처리될 때 각 명령이 수행됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-367">When the batch is processed, each command is performed.</span></span> <span data-ttu-id="3c1b9-368">이러한 명령 중 하나라도 형식이 잘못된 경우 거부되지만(트랜잭션으로 발생하지 않은) 나머지 명령은 수행됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-368">If any of these commands are malformed, they will be rejected (which doesn't happen with a transaction), but the remaining commands will be performed.</span></span> <span data-ttu-id="3c1b9-369">또한 배치의 명령이 처리되는 순서에 대해서 보장할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-369">There is also no guarantee about the order in which the commands in the batch will be processed.</span></span>

### <a name="redis-security"></a><span data-ttu-id="3c1b9-370">Redis 보안</span><span class="sxs-lookup"><span data-stu-id="3c1b9-370">Redis security</span></span>

<span data-ttu-id="3c1b9-371">Redis는 빠른 데이터 액세스 제공에 집중되었으며 신뢰할 수 있는 클라이언트만 액세스할 수 있는 신뢰할 수 있는 환경 내에서 실행되도록 설계되었습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-371">Redis is focused purely on providing fast access to data, and is designed to run inside a trusted environment that can be accessed only by trusted clients.</span></span> <span data-ttu-id="3c1b9-372">Redis는 암호 인증을 기반으로 하는 제한된 보안 모델을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-372">Redis supports a limited security model based on password authentication.</span></span> <span data-ttu-id="3c1b9-373">(권장하지 않지만 인증을 완전히 제거할 수는 있음)</span><span class="sxs-lookup"><span data-stu-id="3c1b9-373">(It is possible to remove authentication completely, although we don't recommend this.)</span></span>

<span data-ttu-id="3c1b9-374">모든 인증된 클라이언트는 같은 전역 암호를 공유하고 동일한 리소스에 액세스합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-374">All authenticated clients share the same global password and have access to the same resources.</span></span> <span data-ttu-id="3c1b9-375">더 포괄적인 로그인 보안이 필요한 경우 Redis 서버 앞에 사용자 고유의 보안 계층을 구현해야 하며 모든 클라이언트 요청이 이 추가 계층을 통해 전달되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-375">If you need more comprehensive sign-in security, you must implement your own security layer in front of the Redis server, and all client requests should pass through this additional layer.</span></span> <span data-ttu-id="3c1b9-376">Redis는 신뢰할 수 없는 또는 인증되지 않은 클라이언트에 직접 노출되지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-376">Redis should not be directly exposed to untrusted or unauthenticated clients.</span></span>

<span data-ttu-id="3c1b9-377">명령을 사용하지 않도록 하거나 이름을 바꾸고 권한 있는 클라이언트만 새 이름으로 제공하여 액세스를 제한할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-377">You can restrict access to commands by disabling them or renaming them (and by providing only privileged clients with the new names).</span></span>

<span data-ttu-id="3c1b9-378">Redis는 모든 형태의 데이터 암호화를 직접 지원하지 않으므로 모든 인코딩을 클라이언트 애플리케이션이 수행해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-378">Redis does not directly support any form of data encryption, so all encoding must be performed by client applications.</span></span> <span data-ttu-id="3c1b9-379">또한 Redis는 어떤 형태의 전송 보안도 제공하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-379">Additionally, Redis does not provide any form of transport security.</span></span> <span data-ttu-id="3c1b9-380">네트워크를 통해 흐르는 데이터를 보호해야 하는 경우 SSL 프록시를 구현하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-380">If you need to protect data as it flows across the network, we recommend implementing an SSL proxy.</span></span>

<span data-ttu-id="3c1b9-381">자세한 내용은 Redis 웹 사이트에서 [Redis 보안](https://redis.io/topics/security) 페이지를 방문하세요.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-381">For more information, visit the [Redis security](https://redis.io/topics/security) page on the Redis website.</span></span>

> [!NOTE]
> <span data-ttu-id="3c1b9-382">Azure Redis Cache는 클라이언트가 연결하는 자체 보안 계층을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-382">Azure Redis Cache provides its own security layer through which clients connect.</span></span> <span data-ttu-id="3c1b9-383">기본 Redis 서버는 공용 네트워크에 노출되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-383">The underlying Redis servers are not exposed to the public network.</span></span>

### <a name="azure-redis-cache"></a><span data-ttu-id="3c1b9-384">Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="3c1b9-384">Azure Redis cache</span></span>

<span data-ttu-id="3c1b9-385">Azure Redis Cache는 Azure 데이터 센터에서 호스트되는 Redis 서버에 대한 액세스를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-385">Azure Redis Cache provides access to Redis servers that are hosted at an Azure datacenter.</span></span> <span data-ttu-id="3c1b9-386">액세스 제어 및 보안을 제공하는 외관의 역할을 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-386">It acts as a façade that provides access control and security.</span></span> <span data-ttu-id="3c1b9-387">Azure 포털을 사용하여 캐시를 프로비전할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-387">You can provision a cache by using the Azure portal.</span></span>

<span data-ttu-id="3c1b9-388">포털에서 다양한 미리 정의된 구성을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-388">The portal provides a number of predefined configurations.</span></span> <span data-ttu-id="3c1b9-389">이 구성은 SSL 통신(개인) 및 99.9%의 가용성 SLA와 마스터/하위 복제를 지원하는 전용 서비스로 실행하는 53GB 캐시에서 공유 하드웨어에서 실행하는 복제(가용성을 보장하지 않음)를 지원하지 않는 250MB 캐시까지의 범위에 이릅니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-389">These range from a 53 GB cache running as a dedicated service that supports SSL communications (for privacy) and master/subordinate replication with an SLA of 99.9% availability, down to a 250 MB cache without replication (no availability guarantees) running on shared hardware.</span></span>

<span data-ttu-id="3c1b9-390">Azure 포털을 사용하여 캐시의 제거 정책을 구성할 수도 있고 제공된 역할에 사용자를 추가하여 캐시에 액세스를 제어합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-390">Using the Azure portal, you can also configure the eviction policy of the cache, and control access to the cache by adding users to the roles provided.</span></span> <span data-ttu-id="3c1b9-391">멤버가 수행할 수 있는 작업이 정의된 역할에는 소유자, 기여자 및 독자가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-391">These roles, which define the operations that members can perform, include Owner, Contributor, and Reader.</span></span> <span data-ttu-id="3c1b9-392">예를 들어 소유자 역할의 멤버는 캐시(보안 포함) 및 해당 콘텐츠를 완전히 제어하고 기여자 역할의 멤버는 캐시에서 정보를 읽고 쓸 수 있고 읽기 역할의 멤버는 캐시에서 데이터를 검색할 수만 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-392">For example, members of the Owner role have complete control over the cache (including security) and its contents, members of the Contributor role can read and write information in the cache, and members of the Reader role can only retrieve data from the cache.</span></span>

<span data-ttu-id="3c1b9-393">대부분의 관리 작업은 Azure 포털을 통해 수행됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-393">Most administrative tasks are performed through the Azure portal.</span></span> <span data-ttu-id="3c1b9-394">이러한 이유로 Redis의 표준 버전에서 사용할 수 있는 관리 명령의 대부분은 사용할 수 없습니다. 여기에는 프로그래밍 방식으로 구성을 수정하고 Redis 서버를 종료하며 디스크에 추가적인 종속 장치를 구성하거나 강제로 데이터를 저장하는 기능이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-394">For this reason, many of the administrative commands that are available in the standard version of Redis are not available, including the ability to modify the configuration programmatically, shut down the Redis server, configure additional subordinates, or forcibly save data to disk.</span></span>

<span data-ttu-id="3c1b9-395">Azure 포털은 캐시의 성능을 모니터링할 수 있도록 편리한 그래픽을 포함하여 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-395">The Azure portal includes a convenient graphical display that enables you to monitor the performance of the cache.</span></span> <span data-ttu-id="3c1b9-396">예를 들어 수행되는 연결 수, 수행되는 요청 수, 읽기 및 쓰기 볼륨, 캐시 적중 수 대 캐시 누락 수를 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-396">For example, you can view the number of connections being made, the number of requests being performed, the volume of reads and writes, and the number of cache hits versus cache misses.</span></span> <span data-ttu-id="3c1b9-397">이 정보를 사용하여 캐시의 효과를 확인하고 필요하면 다른 구성으로 전환하거나 제거 정책을 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-397">Using this information, you can determine the effectiveness of the cache and if necessary, switch to a different configuration or change the eviction policy.</span></span>

<span data-ttu-id="3c1b9-398">또한 하나 이상의 중요한 메트릭이 예상되는 범위를 벗어나면 관리자에게 전자 메일 메시지를 보내는 경고를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-398">Additionally, you can create alerts that send email messages to an administrator if one or more critical metrics fall outside of an expected range.</span></span> <span data-ttu-id="3c1b9-399">예를 들어 캐시 누락 수가 지난 시간에서 지정된 값을 초과하는 경우 캐시가 너무 작거나 데이터가 너무 빨리 제거되고 있다는 의미이므로 관리자에게 알리는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-399">For example, you might want to alert an administrator if the number of cache misses exceeds a specified value in the last hour, because it means the cache might be too small or data might be being evicted too quickly.</span></span>

<span data-ttu-id="3c1b9-400">또한 CPU, 메모리 및 캐시용 네트워크 사용량을 모니터링할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-400">You can also monitor the CPU, memory, and network usage for the cache.</span></span>

<span data-ttu-id="3c1b9-401">Azure Redis Cache 만들기 및 구성 방법을 보여주는 자세한 내용 및 예제는 Azure 블로그의 [Azure Redis Cache 살펴보기](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) 페이지를 방문하십시오.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-401">For further information and examples showing how to create and configure an Azure Redis Cache, visit the page [Lap around Azure Redis Cache](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) on the Azure blog.</span></span>

## <a name="caching-session-state-and-html-output"></a><span data-ttu-id="3c1b9-402">캐싱 세션 상태 및 HTML 출력</span><span class="sxs-lookup"><span data-stu-id="3c1b9-402">Caching session state and HTML output</span></span>

<span data-ttu-id="3c1b9-403">Azure 웹 역할을 사용하여 실행하는 ASP.NET 웹 애플리케이션을 구축하면 Azure Redis Cache에서 세션 상태 정보 및 HTML 출력을 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-403">If you're building ASP.NET web applications that run by using Azure web roles, you can save session state information and HTML output in an Azure Redis Cache.</span></span> <span data-ttu-id="3c1b9-404">Azure Redis Cache용 세션 상태 제공자를 사용하면 ASP.NET 웹 애플리케이션의 다른 인스턴스 간에 세션 정보를 공유할 수 있고 클라이언트-서버 선호도를 사용할 수 없는 경우 웹 팜의 경우에 매우 유용합니다. 메모리 내 캐싱 세션 데이터는 적절하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-404">The session state provider for Azure Redis Cache enables you to share session information between different instances of an ASP.NET web application, and is very useful in web farm situations where client-server affinity is not available and caching session data in-memory would not be appropriate.</span></span>

<span data-ttu-id="3c1b9-405">Azure Redis Cache와 세션 상태 제공자를 사용하면 다음을 포함하여 다양한 이점이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-405">Using the session state provider with Azure Redis Cache delivers several benefits, including:</span></span>

- <span data-ttu-id="3c1b9-406">ASP.NET 웹 애플리케이션의 많은 인스턴스와 세션 상태를 공유합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-406">Sharing session state with a large number of instances of ASP.NET web applications.</span></span>
- <span data-ttu-id="3c1b9-407">향상된 확장성을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-407">Providing improved scalability.</span></span>
- <span data-ttu-id="3c1b9-408">여러 판독기와 단일 작성기용 동일한 세션 상태 데이터에 제어된 동시 액세스를 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-408">Supporting controlled, concurrent access to the same session state data for multiple readers and a single writer.</span></span>
- <span data-ttu-id="3c1b9-409">압축을 사용하여 메모리를 절약하고 네트워크 성능을 향상시킵니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-409">Using compression to save memory and improve network performance.</span></span>

<span data-ttu-id="3c1b9-410">자세한 내용은 [Azure Redis Cache에 대한 ASP.NET 세션 상태 제공자](/azure/redis-cache/cache-aspnet-session-state-provider/)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-410">For more information, see [ASP.NET session state provider for Azure Redis Cache](/azure/redis-cache/cache-aspnet-session-state-provider/).</span></span>

> [!NOTE]
> <span data-ttu-id="3c1b9-411">Azure 환경 외부에서 실행되는 ASP.NET 애플리케이션과 함께 Azure Redis Cache에 세션 상태 제공자를 사용하지 마세요.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-411">Do not use the session state provider for Azure Redis Cache with ASP.NET applications that run outside of the Azure environment.</span></span> <span data-ttu-id="3c1b9-412">Azure 외부에서 캐시에 액세스하는 대기 시간은 데이터를 캐시하는 성능 혜택을 제거할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-412">The latency of accessing the cache from outside of Azure can eliminate the performance benefits of caching data.</span></span>

<span data-ttu-id="3c1b9-413">마찬가지로, Azure Redis Cache용 출력 캐시 공급자를 사용하면 ASP.NET 웹 애플리케이션이 생성한 HTTP 응답을 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-413">Similarly, the output cache provider for Azure Redis Cache enables you to save the HTTP responses generated by an ASP.NET web application.</span></span> <span data-ttu-id="3c1b9-414">Azure Redis Cache와 출력 캐시 공급자를 사용하면 복잡한 HTML 출력을 렌더링하는 애플리케이션의 응답 시간을 향상시킬 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-414">Using the output cache provider with Azure Redis Cache can improve the response times of applications that render complex HTML output.</span></span> <span data-ttu-id="3c1b9-415">유사한 응답을 생성하는 애플리케이션 인스턴스는 이 HTML 출력을 새로 생성하기 보다 캐시에서 공유 출력 조각을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-415">Application instances that generate similar responses can make use of the shared output fragments in the cache rather than generating this HTML output afresh.</span></span> <span data-ttu-id="3c1b9-416">자세한 내용은 [Azure Redis Cache에 대한 ASP.NET 출력 캐시 공급자](/azure/redis-cache/cache-aspnet-output-cache-provider/)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-416">For more information, see [ASP.NET output cache provider for Azure Redis Cache](/azure/redis-cache/cache-aspnet-output-cache-provider/).</span></span>

## <a name="building-a-custom-redis-cache"></a><span data-ttu-id="3c1b9-417">사용자 지정 Redis 캐시 빌드</span><span class="sxs-lookup"><span data-stu-id="3c1b9-417">Building a custom Redis cache</span></span>

<span data-ttu-id="3c1b9-418">Azure Redis Cache는 기본 Redis 서버에 외관의 역할을 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-418">Azure Redis Cache acts as a façade to the underlying Redis servers.</span></span> <span data-ttu-id="3c1b9-419">Azure Redis Cache가 다루지 않는 고급 구성이 필요한 경우(예: 53GB보다 더 큰 캐시) Azure 가상 머신을 사용하여 사용자 고유의 Redis 서버를 빌드 및 호스트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-419">If you require an advanced configuration that is not covered by the Azure Redis cache (such as a cache bigger than 53 GB) you can build and host your own Redis servers by using Azure virtual machines.</span></span>

<span data-ttu-id="3c1b9-420">복제를 구현하려는 경우 마스터 및 하위 노드 역할을 수행할 여러 VM을 만들어야 할 수 있기 때문에 이는 잠재적으로 복잡한 프로세스입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-420">This is a potentially complex process because you might need to create several VMs to act as master and subordinate nodes if you want to implement replication.</span></span> <span data-ttu-id="3c1b9-421">또한 클러스터를 만들려는 경우 여러 마스터와 하위 서버가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-421">Furthermore, if you wish to create a cluster, then you need multiple masters and subordinate servers.</span></span> <span data-ttu-id="3c1b9-422">높은 수준의 가용성과 확장성을 제공하는 최소한의 클러스터된 복제 토폴로지는 세 쌍의 마스터/하위 서버로 구성된 6개 이상의 VM을 구성합니다(클러스터에 3개 이상의 마스터 노드를 포함해야 함).</span><span class="sxs-lookup"><span data-stu-id="3c1b9-422">A minimal clustered replication topology that provides a high degree of availability and scalability comprises at least six VMs organized as three pairs of master/subordinate servers (a cluster must contain at least three master nodes).</span></span>

<span data-ttu-id="3c1b9-423">각 마스터/하위 쌍은 대기 시간을 최소화하기 위해 서로 가까이 배치되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-423">Each master/subordinate pair should be located close together to minimize latency.</span></span> <span data-ttu-id="3c1b9-424">그러나 이를 사용할 가능성이 큰 애플리케이션에 가까운 캐시된 데이터를 찾으려는 경우 다른 지역에 있는 다른 Azure 데이터 센터에서 쌍의 각 집합을 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-424">However, each set of pairs can be running in different Azure datacenters located in different regions, if you wish to locate cached data close to the applications that are most likely to use it.</span></span> <span data-ttu-id="3c1b9-425">빌드 및 Azure VM으로 실행되는 Redis 노드를 빌드 및 구성하는 방법의 예제를 보려면 [Azure의 CentOS Linux VM에서 Redis 실행](https://blogs.msdn.microsoft.com/tconte/2012/06/08/running-redis-on-a-centos-linux-vm-in-windows-azure/)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-425">For an example of building and configuring a Redis node running as an Azure VM, see [Running Redis on a CentOS Linux VM in Azure](https://blogs.msdn.microsoft.com/tconte/2012/06/08/running-redis-on-a-centos-linux-vm-in-windows-azure/).</span></span>

> [!NOTE]
> <span data-ttu-id="3c1b9-426">이러한 방식으로 사용자 고유의 Redis 캐시를 구현하는 경우 서비스 모니터링, 관리 및 보안에 책임이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-426">Please note that if you implement your own Redis cache in this way, you are responsible for monitoring, managing, and securing the service.</span></span>

## <a name="partitioning-a-redis-cache"></a><span data-ttu-id="3c1b9-427">Redis 캐시 분할</span><span class="sxs-lookup"><span data-stu-id="3c1b9-427">Partitioning a Redis cache</span></span>

<span data-ttu-id="3c1b9-428">캐시 분할은 여러 컴퓨터에서 캐시를 나누어는 것이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-428">Partitioning the cache involves splitting the cache across multiple computers.</span></span> <span data-ttu-id="3c1b9-429">이 구조는 단일 캐시 서버를 사용하며 다음을 포함하여 여러 장점이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-429">This structure gives you several advantages over using a single cache server, including:</span></span>

- <span data-ttu-id="3c1b9-430">훨씬 큰 캐시를 만들면 단일 서버에 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-430">Creating a cache that is much bigger than can be stored on a single server.</span></span>
- <span data-ttu-id="3c1b9-431">가용성 향상하여 서버에 걸쳐 데이터를 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-431">Distributing data across servers, improving availability.</span></span> <span data-ttu-id="3c1b9-432">하나의 서버가 실패하거나 액세스할 수 없게 되면 보유한 데이터는 사용할 수 없지만 나머지 서버에 있는 데이터에는 여전히 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-432">If one server fails or becomes inaccessible, the data that it holds is unavailable, but the data on the remaining servers can still be accessed.</span></span> <span data-ttu-id="3c1b9-433">캐시의 경우에는 캐시된 데이터가 데이터베이스에 저장된 데이터의 임시 복사본일 뿐이므로 중요하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-433">For a cache, this is not crucial because the cached data is only a transient copy of the data that's held in a database.</span></span> <span data-ttu-id="3c1b9-434">액세스할 수 없게 되는 서버의 캐시된 데이터는 대신 다른 서버에서 캐시될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-434">Cached data on a server that becomes inaccessible can be cached on a different server instead.</span></span>
- <span data-ttu-id="3c1b9-435">성능 및 확장성을 향상하여 서버에 걸쳐 부하를 분산합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-435">Spreading the load across servers, thereby improving performance and scalability.</span></span>
- <span data-ttu-id="3c1b9-436">지리 위치 데이터가 액세스하는 사용자에게 가까우므로 대기 시간이 감소합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-436">Geolocating data close to the users that access it, thus reducing latency.</span></span>

<span data-ttu-id="3c1b9-437">캐시에 대해 분할의 가장 일반적인 형태가 분할됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-437">For a cache, the most common form of partitioning is sharding.</span></span> <span data-ttu-id="3c1b9-438">이 전략에서 각 파티션(또는 분할된 데이터베이스)은 그 자체로 Redis 캐시입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-438">In this strategy, each partition (or shard) is a Redis cache in its own right.</span></span> <span data-ttu-id="3c1b9-439">데이터는 분할 논리를 사용하여 특정 파티션으로 전송하여 데이터를 분산하는 데 다양한 방식을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-439">Data is directed to a specific partition by using sharding logic, which can use a variety of approaches to distribute the data.</span></span> <span data-ttu-id="3c1b9-440">[분할 패턴](../patterns/sharding.md)은 분할을 구현하는 방법에 대한 자세한 정보를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-440">The [Sharding pattern](../patterns/sharding.md) provides more information about implementing sharding.</span></span>

<span data-ttu-id="3c1b9-441">Redis 캐시에서 분할을 구현하려면 다음 방법 중 하나를 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-441">To implement partitioning in a Redis cache, you can take one of the following approaches:</span></span>

- <span data-ttu-id="3c1b9-442">*서버측 쿼리 라우팅입니다.*</span><span class="sxs-lookup"><span data-stu-id="3c1b9-442">*Server-side query routing.*</span></span> <span data-ttu-id="3c1b9-443">이 기술에서 클라이언트 애플리케이션은 캐시를 구성하는 Redis 서버 중 하나에 요청을 보냅니다(아마도 가장 가까운 서버).</span><span class="sxs-lookup"><span data-stu-id="3c1b9-443">In this technique, a client application sends a request to any of the Redis servers that comprise the cache (probably the closest server).</span></span> <span data-ttu-id="3c1b9-444">각 Redis 서버는 보유한 분할을 설명하는 메타데이터를 저장하며 또한 다른 서버에 위치한 분할에 대한 정보를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-444">Each Redis server stores metadata that describes the partition that it holds, and also contains information about which partitions are located on other servers.</span></span> <span data-ttu-id="3c1b9-445">Redis 서버는 클라이언트 요청을 검사합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-445">The Redis server examines the client request.</span></span> <span data-ttu-id="3c1b9-446">로컬로 해결할 수 있는 경우 요청된 작업을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-446">If it can be resolved locally, it will perform the requested operation.</span></span> <span data-ttu-id="3c1b9-447">그렇지 않으면 적합한 서버로 요청을 전달합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-447">Otherwise it will forward the request on to the appropriate server.</span></span> <span data-ttu-id="3c1b9-448">이 모델은 Redis 클러스터링이 구현하고 Redis 웹 사이트의 [Redis 클러스터 자습서](https://redis.io/topics/cluster-tutorial) 페이지에서 자세히 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-448">This model is implemented by Redis clustering, and is described in more detail on the [Redis cluster tutorial](https://redis.io/topics/cluster-tutorial) page on the Redis website.</span></span> <span data-ttu-id="3c1b9-449">Redis 클러스터링은 클라이언트 애플리케이션에 대해 투명하며, 클라이언트를 다시 구성하지 않고도 Redis 서버를 클러스터 및 다시 분할된 데이터에 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-449">Redis clustering is transparent to client applications, and additional Redis servers can be added to the cluster (and the data re-partitioned) without requiring that you reconfigure the clients.</span></span>
- <span data-ttu-id="3c1b9-450">*클라이언트측 분할입니다.*</span><span class="sxs-lookup"><span data-stu-id="3c1b9-450">*Client-side partitioning.*</span></span> <span data-ttu-id="3c1b9-451">이 모델에서 클라이언트 애플리케이션은 적절한 Redis 서버에 요청을 라우팅하는 논리(아마도 라이브러리 형식으로)를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-451">In this model, the client application contains logic (possibly in the form of a library) that routes requests to the appropriate Redis server.</span></span> <span data-ttu-id="3c1b9-452">이 방법은 Azure Redis Cache와 함께 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-452">This approach can be used with Azure Redis Cache.</span></span> <span data-ttu-id="3c1b9-453">여러 Azure Redis Cache(각 데이터 파티션에 하나)를 만들고 올바른 캐시에 요청을 라우트하는 클라이언트 쪽 논리를 구현할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-453">Create multiple Azure Redis Caches (one for each data partition) and implement the client-side logic that routes the requests to the correct cache.</span></span> <span data-ttu-id="3c1b9-454">분할 체계가 변경되면(예를 들어 추가로 Azure Redis Cache를 생성하는 경우) 클라이언트 애플리케이션을 다시 구성해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-454">If the partitioning scheme changes (if additional Azure Redis Caches are created, for example), client applications might need to be reconfigured.</span></span>
- <span data-ttu-id="3c1b9-455">*프록시 지원 분할입니다.*</span><span class="sxs-lookup"><span data-stu-id="3c1b9-455">*Proxy-assisted partitioning.*</span></span> <span data-ttu-id="3c1b9-456">이 스키마에서 클라이언트 애플리케이션은 송신 데이터를 분할하는 방법을 이해하는 중간 프록시 서비스에 요청을 보낸 다음 Redis 서버에 적절한 요청을 라우팅합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-456">In this scheme, client applications send requests to an intermediary proxy service which understands how the data is partitioned and then routes the request to the appropriate Redis server.</span></span> <span data-ttu-id="3c1b9-457">또한 이 방법은 Azure Redis Cache와 함께 사용할 수 있습니다. 프록시 서비스는 Azure 클라우드 서비스로 구현할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-457">This approach can also be used with Azure Redis Cache; the proxy service can be implemented as an Azure cloud service.</span></span> <span data-ttu-id="3c1b9-458">이 접근 방법을 사용하려면 서비스를 구현하기 위해 추가적으로 복잡한 수준이 필요하고 요청도 클라이언트 쪽 분할을 사용하는 것보다 오래 걸릴 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-458">This approach requires an additional level of complexity to implement the service, and requests might take longer to perform than using client-side partitioning.</span></span>

<span data-ttu-id="3c1b9-459">웹사이트는 Redis의 [분할: 여러 Redis 인스턴스 간에 데이터를 분할하는 방법](https://redis.io/topics/partitioning) 페이지는 Redis와 분할을 구현하는 추가 정보를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-459">The page [Partitioning: how to split data among multiple Redis instances](https://redis.io/topics/partitioning) on the Redis website provides further information about implementing partitioning with Redis.</span></span>

### <a name="implement-redis-cache-client-applications"></a><span data-ttu-id="3c1b9-460">Redis 캐시 클라이언트 애플리케이션 구현</span><span class="sxs-lookup"><span data-stu-id="3c1b9-460">Implement Redis cache client applications</span></span>

<span data-ttu-id="3c1b9-461">Redis는 다양한 프로그래밍 언어로 작성된 클라이언트 애플리케이션을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-461">Redis supports client applications written in numerous programming languages.</span></span> <span data-ttu-id="3c1b9-462">.NET Framework를 사용하여 새 애플리케이션을 빌드하면 StackExchange.Redis 클라이언트 라이브러리를 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-462">If you are building new applications by using the .NET Framework, the recommended approach is to use the StackExchange.Redis client library.</span></span> <span data-ttu-id="3c1b9-463">이 라이브러리는 Redis 서버에 연결, 명령 전송 및 응답 수신에 대한 세부 정보를 끌어내는 .NET Framework 개체 모델을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-463">This library provides a .NET Framework object model that abstracts the details for connecting to a Redis server, sending commands, and receiving responses.</span></span> <span data-ttu-id="3c1b9-464">Visual Studio에서 NuGet 패키지로 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-464">It is available in Visual Studio as a NuGet package.</span></span> <span data-ttu-id="3c1b9-465">Azure Redis Cache 또는 VM에서 호스트되는 사용자 지정 Redis 캐시에 연결하려면 이 동일한 라이브러리를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-465">You can use this same library to connect to an Azure Redis Cache, or a custom Redis cache hosted on a VM.</span></span>

<span data-ttu-id="3c1b9-466">Redis 서버에 연결하려면 `ConnectionMultiplexer` 클래스의 정적 `Connect` 메서드를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-466">To connect to a Redis server you use the static `Connect` method of the `ConnectionMultiplexer` class.</span></span> <span data-ttu-id="3c1b9-467">이 메서드가 만드는 연결은 클라이언트 애플리케이션의 전체 수명 동안 사용할 수 있도록 디자인되었으며 같은 연결이 여러 동시 스레드에서 사용될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-467">The connection that this method creates is designed to be used throughout the lifetime of the client application, and the same connection can be used by multiple concurrent threads.</span></span> <span data-ttu-id="3c1b9-468">성능을 저하시킬 수 있으므로 Redis 작업을 수행할 때마다 다시 연결하고 끊지 마세요.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-468">Do not reconnect and disconnect each time you perform a Redis operation because this can degrade performance.</span></span>

<span data-ttu-id="3c1b9-469">Redis 호스트 및 암호의 주소와 같은 연결 매개 변수를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-469">You can specify the connection parameters, such as the address of the Redis host and the password.</span></span> <span data-ttu-id="3c1b9-470">Azure Redis Cache를 사용하면 암호는 Azure 관리 포털을 사용하여 Azure Redis Cache용으로 생성된 기본 또는 보조 키입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-470">If you are using Azure Redis Cache, the password is either the primary or secondary key that is generated for Azure Redis Cache by using the Azure Management portal.</span></span>

<span data-ttu-id="3c1b9-471">Redis 서버에 연결한 후 캐시의 역할을  하는 Redis 데이터베이스에 핸들을 가져올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-471">After you have connected to the Redis server, you can obtain a handle on the Redis database that acts as the cache.</span></span> <span data-ttu-id="3c1b9-472">Redis 연결이 이를 수행하는 `GetDatabase` 메서드를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-472">The Redis connection provides the `GetDatabase` method to do this.</span></span> <span data-ttu-id="3c1b9-473">그런 다음 `StringGet` 및 `StringSet` 메서드를 사용하여 캐시에서 항목을 검색하고 캐시에 데이터를 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-473">You can then retrieve items from the cache and store data in the cache by using the `StringGet` and `StringSet` methods.</span></span> <span data-ttu-id="3c1b9-474">이러한 메서드는 키를 매개 변수로 예상하고 일치하는 값을 갖는 캐시에서 항목을 반환하거나(`StringGet`) 이 키로 캐시에 항목을 추가합니다(`StringSet`).</span><span class="sxs-lookup"><span data-stu-id="3c1b9-474">These methods expect a key as a parameter, and return the item either in the cache that has a matching value (`StringGet`) or add the item to the cache with this key (`StringSet`).</span></span>

<span data-ttu-id="3c1b9-475">Redis 서버 위치에 따라 요청이 서버에 전송되고 응답이 클라이언트에 반환되는 동안 많은 작업이 약간의 대기 시간을 발생시킬 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-475">Depending on the location of the Redis server, many operations might incur some latency while a request is transmitted to the server and a response is returned to the client.</span></span> <span data-ttu-id="3c1b9-476">StackExchange 라이브러리는 클라이언트 애플리케이션이 응답을 하도록 노출하는 많은 매서드의 비동기 버전을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-476">The StackExchange library provides asynchronous versions of many of the methods that it exposes to help client applications remain responsive.</span></span> <span data-ttu-id="3c1b9-477">이러한 메서드는 .NET Framework에서 [작업 기반 비동기 패턴](/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-477">These methods support the [Task-based Asynchronous pattern](/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap) in the .NET Framework.</span></span>

<span data-ttu-id="3c1b9-478">다음 코드 조각에 `RetrieveItem`이라는 메서드가 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-478">The following code snippet shows a method named `RetrieveItem`.</span></span> <span data-ttu-id="3c1b9-479">Redis 및 StackExchange 라이브러리를 기반으로 한 캐시 배제 패턴의 구현 예를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-479">It illustrates an implementation of the cache-aside pattern based on Redis and the StackExchange library.</span></span> <span data-ttu-id="3c1b9-480">`StringGetAsync` 메서드를 호출하여 메서드가 문자열 키 값을 사용하고 Redis 캐시에서 해당 항목을 검색하려고 시도합니다(`StringGet`의 비동기 버전).</span><span class="sxs-lookup"><span data-stu-id="3c1b9-480">The method takes a string key value and attempts to retrieve the corresponding item from the Redis cache by calling the `StringGetAsync` method (the asynchronous version of `StringGet`).</span></span>

<span data-ttu-id="3c1b9-481">항목이 없는 경우 `GetItemFromDataSourceAsync` 메서드(StackExchange 라이브러리의 일부가 아니고 로컬 메서드임)를 사용하여 기본 데이터 원본에서 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-481">If the item is not found, it is fetched from the underlying data source using the `GetItemFromDataSourceAsync` method (which is a local method and not part of the StackExchange library).</span></span> <span data-ttu-id="3c1b9-482">그런 다음 `StringSetAsync` 메서드를 사용하여 캐시에 추가되어 다음에 더 신속히 검색할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-482">It's then added to the cache by using the `StringSetAsync` method so it can be retrieved more quickly next time.</span></span>

```csharp
// Connect to the Azure Redis cache
ConfigurationOptions config = new ConfigurationOptions();
config.EndPoints.Add("<your DNS name>.redis.cache.windows.net");
config.Password = "<Redis cache key from management portal>";
ConnectionMultiplexer redisHostConnection = ConnectionMultiplexer.Connect(config);
IDatabase cache = redisHostConnection.GetDatabase();
...
private async Task<string> RetrieveItem(string itemKey)
{
    // Attempt to retrieve the item from the Redis cache
    string itemValue = await cache.StringGetAsync(itemKey);

    // If the value returned is null, the item was not found in the cache
    // So retrieve the item from the data source and add it to the cache
    if (itemValue == null)
    {
        itemValue = await GetItemFromDataSourceAsync(itemKey);
        await cache.StringSetAsync(itemKey, itemValue);
    }

    // Return the item
    return itemValue;
}
```

<span data-ttu-id="3c1b9-483">`StringGet` 및 `StringSet` 메서드는 문자열 값을 검색하거나 저장하는 데 제한되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-483">The `StringGet` and `StringSet` methods are not restricted to retrieving or storing string values.</span></span> <span data-ttu-id="3c1b9-484">두 메서드에서 바이트 배열로 직렬화된 모든 항목을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-484">They can take any item that is serialized as an array of bytes.</span></span> <span data-ttu-id="3c1b9-485">.NET 개체를 저장해야 하는 경우 바이트 스트림으로 직렬화하고 캐시에 쓰기 위해 `StringSet` 메서드를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-485">If you need to save a .NET object, you can serialize it as a byte stream and use the `StringSet` method to write it to the cache.</span></span>

<span data-ttu-id="3c1b9-486">마찬가지로, `StringGet` 메서드를 사용하여 캐시에서 개체를 읽을 수 있으며 .NET 개체로 역직렬화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-486">Similarly, you can read an object from the cache by using the `StringGet` method and deserializing it as a .NET object.</span></span> <span data-ttu-id="3c1b9-487">다음 코드는 IDatabase 인터페이스용 확장 메서드 집합(Redis 연결의 `GetDatabase` 메서드는 `IDatabase` 개체를 반환함) 및 `BlogPost` 개체를 캐시로 읽고 쓰기 위해 이러한 메서드를 사용하는 일부 샘플 코드를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-487">The following code shows a set of extension methods for the IDatabase interface (the `GetDatabase` method of a Redis connection returns an `IDatabase` object),  and some sample code that uses these methods to read and write a `BlogPost` object to the cache:</span></span>

```csharp
public static class RedisCacheExtensions
{
    public static async Task<T> GetAsync<T>(this IDatabase cache, string key)
    {
        return Deserialize<T>(await cache.StringGetAsync(key));
    }

    public static async Task<object> GetAsync(this IDatabase cache, string key)
    {
        return Deserialize<object>(await cache.StringGetAsync(key));
    }

    public static async Task SetAsync(this IDatabase cache, string key, object value)
    {
        await cache.StringSetAsync(key, Serialize(value));
    }

    static byte[] Serialize(object o)
    {
        byte[] objectDataAsStream = null;

        if (o != null)
        {
            BinaryFormatter binaryFormatter = new BinaryFormatter();
            using (MemoryStream memoryStream = new MemoryStream())
            {
                binaryFormatter.Serialize(memoryStream, o);
                objectDataAsStream = memoryStream.ToArray();
            }
        }

        return objectDataAsStream;
    }

    static T Deserialize<T>(byte[] stream)
    {
        T result = default(T);

        if (stream != null)
        {
            BinaryFormatter binaryFormatter = new BinaryFormatter();
            using (MemoryStream memoryStream = new MemoryStream(stream))
            {
                result = (T)binaryFormatter.Deserialize(memoryStream);
            }
        }

        return result;
    }
}
```

<span data-ttu-id="3c1b9-488">다음 코드는 캐시 배제 패턴을 따라 직렬화 가능한 `BlogPost` 개체를 읽고 쓰는 데 이러한 확장 메서드를 사용하는 `RetrieveBlogPost`라는 메서드를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-488">The following code illustrates a method named `RetrieveBlogPost` that uses these extension methods to read and write a serializable `BlogPost` object to the cache following the cache-aside pattern:</span></span>

```csharp
// The BlogPost type
[Serializable]
public class BlogPost
{
    private HashSet<string> tags;

    public BlogPost(int id, string title, int score, IEnumerable<string> tags)
    {
        this.Id = id;
        this.Title = title;
        this.Score = score;
        this.tags = new HashSet<string>(tags);
    }

    public int Id { get; set; }
    public string Title { get; set; }
    public int Score { get; set; }
    public ICollection<string> Tags => this.tags;
}
...
private async Task<BlogPost> RetrieveBlogPost(string blogPostKey)
{
    BlogPost blogPost = await cache.GetAsync<BlogPost>(blogPostKey);
    if (blogPost == null)
    {
        blogPost = await GetBlogPostFromDataSourceAsync(blogPostKey);
        await cache.SetAsync(blogPostKey, blogPost);
    }

    return blogPost;
}
```

<span data-ttu-id="3c1b9-489">클라이언트 애플리케이션이 여러 비동기 요청을 보내면 Redis는 명령 파이프라인을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-489">Redis supports command pipelining if a client application sends multiple asynchronous requests.</span></span> <span data-ttu-id="3c1b9-490">Redis는 엄격한 시퀀스의 명령을 수신 및 응답하기 보다 동일한 연결을 사용하여 요청을 멀티플렉싱할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-490">Redis can multiplex the requests using the same connection rather than receiving and responding to commands in a strict sequence.</span></span>

<span data-ttu-id="3c1b9-491">이 방법은 네트워크를 보다 효율적으로 만들어 대기 시간을 줄일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-491">This approach helps to reduce latency by making more efficient use of the network.</span></span> <span data-ttu-id="3c1b9-492">다음 코드 조각은 동시에 두 고객의 세부 정보를 검색하는 예를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-492">The following code snippet shows an example that retrieves the details of two customers concurrently.</span></span> <span data-ttu-id="3c1b9-493">코드는 결과를 수신하려고 대기하기 전에 두 요청을 제출하고 다른 처리(표시되지 않음)를 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-493">The code submits two requests and then performs some other processing (not shown) before waiting to receive the results.</span></span> <span data-ttu-id="3c1b9-494">캐시 개체의 `Wait` 메서드는 .NET Framework `Task.Wait` 메서드와 비슷합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-494">The `Wait` method of the cache object is similar to the .NET Framework `Task.Wait` method:</span></span>

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
var task1 = cache.StringGetAsync("customer:1");
var task2 = cache.StringGetAsync("customer:2");
...
var customer1 = cache.Wait(task1);
var customer2 = cache.Wait(task2);
```

<span data-ttu-id="3c1b9-495">Azure Redis Cache를 수행할 수 있는 클라이언트 애플리케이션 작성 방법에 대한 자세한 내용은 [Azure Redis Cache 설명서](https://azure.microsoft.com/documentation/services/cache/)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-495">For additional information on writing client applications that can the Azure Redis Cache, see [Azure Redis Cache documentation](https://azure.microsoft.com/documentation/services/cache/).</span></span> <span data-ttu-id="3c1b9-496">[StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Basics.md)에서도 자세한 내용을 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-496">More information is also available at [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Basics.md).</span></span>

<span data-ttu-id="3c1b9-497">동일한 웹 사이트의 [파이프라인 및 멀티플렉서](https://stackexchange.github.io/StackExchange.Redis/PipelinesMultiplexers) 페이지에서 Redis와 StackExchange 라이브러리를 통한 파이프라인 및 비동기 작업에 대한 자세한 정보를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-497">The page [Pipelines and multiplexers](https://stackexchange.github.io/StackExchange.Redis/PipelinesMultiplexers) on the same website provides more information about asynchronous operations and pipelining with Redis and the StackExchange library.</span></span> 

## <a name="using-redis-caching"></a><span data-ttu-id="3c1b9-498">Redis 캐싱 사용</span><span class="sxs-lookup"><span data-stu-id="3c1b9-498">Using Redis caching</span></span>

<span data-ttu-id="3c1b9-499">캐싱 문제에 대한 Redis의 가장 간단한 용도는 키-값 쌍이며 여기서 값은 이진 데이터를 포함할 수 있는 해석되지 않은 임의 길이의 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-499">The simplest use of Redis for caching concerns is key-value pairs where the value is an uninterpreted string of arbitrary length that can contain any binary data.</span></span> <span data-ttu-id="3c1b9-500">(기본적으로 문자열로 처리할 수 있는 바이트 배열임)</span><span class="sxs-lookup"><span data-stu-id="3c1b9-500">(It is essentially an array of bytes that can be treated as a string).</span></span> <span data-ttu-id="3c1b9-501">이 문서의 앞부분에 있는 Redis 캐시 클라이언트 애플리케이션 구현 섹션에서 이 시나리오를 설명했습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-501">This scenario was illustrated in the section Implement Redis Cache client applications earlier in this article.</span></span>

<span data-ttu-id="3c1b9-502">키에는 해석되지 않은 데이터도 포함되므로 모든 이진 정보를 키로 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-502">Note that keys also contain uninterpreted data, so you can use any binary information as the key.</span></span> <span data-ttu-id="3c1b9-503">그러나 키가 길수록 차지하는 저장 공간이 더 많아지고 조회 작업을 수행하는 데 더 오래 걸립니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-503">The longer the key is, however, the more space it will take to store, and the longer it will take to perform lookup operations.</span></span> <span data-ttu-id="3c1b9-504">유용성 및 용이한 유지 관리를 위해 keyspace를 신중하게 디자인하고 의미는 있지만 자세한 정보를 표시하지 않은 키를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-504">For usability and ease of maintenance, design your keyspace carefully and use meaningful (but not verbose) keys.</span></span>

<span data-ttu-id="3c1b9-505">예를 들어 "customer:100"과 같은 구조화된 키를 사용하여 단순히 "100"이 아닌 ID 100인 고객에 대한 키를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-505">For example, use structured keys such as "customer:100" to represent the key for the customer with ID 100 rather than simply "100".</span></span> <span data-ttu-id="3c1b9-506">이 체계를 사용하면 다른 데이터 형식을 저장하는 값을 쉽게 구분할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-506">This scheme enables you to easily distinguish between values that store different data types.</span></span> <span data-ttu-id="3c1b9-507">예를 들어 "orders:100" 키를 사용하여 ID 100으로 순서에 대한 키를 나타낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-507">For example, you could also use the key "orders:100" to represent the key for the order with ID 100.</span></span>

<span data-ttu-id="3c1b9-508">1차원 이진 문자열 외에도 Redis 키-값 쌍의 값은 목록, 집합(정렬 및 정렬되지 않은) 및 해시를 포함한 더 구조화된 정보를 보유할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-508">Apart from one-dimensional binary strings, a value in a Redis key-value pair can also hold more structured information, including lists, sets (sorted and unsorted), and hashes.</span></span> <span data-ttu-id="3c1b9-509">Redis는 이러한 형식을 조작할 수 있는 포괄적인 명령 집합을 제공하고 이 명령 중 다수는 StackExchange와 같은 클라이언트 라이브러리를 통해 .NET Framework 애플리케이션에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-509">Redis provides a comprehensive command set that can manipulate these types, and many of these commands are available to .NET Framework applications through a client library such as StackExchange.</span></span> <span data-ttu-id="3c1b9-510">Redis에 웹 사이트의 [Redis 데이터 형식 및 추상화 소개](https://redis.io/topics/data-types-intro) 페이지가 이를 조작하는데 사용할 수 있는 유형 및 명령의 자세한 개요를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-510">The page [An introduction to Redis data types and abstractions](https://redis.io/topics/data-types-intro) on the Redis website provides a more detailed overview of these types and the commands that you can use to manipulate them.</span></span>

<span data-ttu-id="3c1b9-511">이 섹션은 이러한 데이터 형식 및 명령에 대한 일부 일반적인 사용 사례를 요약합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-511">This section summarizes some common use cases for these data types and commands.</span></span>

### <a name="perform-atomic-and-batch-operations"></a><span data-ttu-id="3c1b9-512">원자성 및 배치 작업 수행</span><span class="sxs-lookup"><span data-stu-id="3c1b9-512">Perform atomic and batch operations</span></span>

<span data-ttu-id="3c1b9-513">Redis는 문자열 값에 대한 일련의 원자성 get-and-set 작업을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-513">Redis supports a series of atomic get-and-set operations on string values.</span></span> <span data-ttu-id="3c1b9-514">별도 `GET` 및 `SET` 명령을 사용하는 경우 이러한 작업은 발생할 수 있는 경합 충격을 제거합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-514">These operations remove the possible race hazards that might occur when using separate `GET` and `SET` commands.</span></span> <span data-ttu-id="3c1b9-515">사용할 수 있는 작업은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-515">The operations that are available include:</span></span>

- <span data-ttu-id="3c1b9-516">정수 숫자 데이터 값에 원자성 증가 및 감소 작업을 수행하는 `INCR`, `INCRBY`, `DECR` 및 `DECRBY`입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-516">`INCR`, `INCRBY`, `DECR`, and `DECRBY`, which perform atomic increment and decrement operations on integer numeric data values.</span></span> <span data-ttu-id="3c1b9-517">StackExchange 라이브러리는 `IDatabase.StringIncrementAsync` 및 `IDatabase.StringDecrementAsync` 메서드의 오버로드된 버전을 제공하여 이러한 작업을 수행하고 캐시에 저장된 결과 값을 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-517">The StackExchange library provides overloaded versions of the `IDatabase.StringIncrementAsync` and `IDatabase.StringDecrementAsync` methods to perform these operations and return the resulting value that is stored in the cache.</span></span> <span data-ttu-id="3c1b9-518">다음 코드 조각에서는 이러한 메서드를 사용하는 방법을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-518">The following code snippet illustrates how to use these methods:</span></span>

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  await cache.StringSetAsync("data:counter", 99);
  ...
  long oldValue = await cache.StringIncrementAsync("data:counter");
  // Increment by 1 (the default)
  // oldValue should be 100
  
  long newValue = await cache.StringDecrementAsync("data:counter", 50);
  // Decrement by 50
  // newValue should be 50
  ```

- <span data-ttu-id="3c1b9-519">키와 연결된 값을 검색하고 새 값으로 변경하는 `GETSET`입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-519">`GETSET`, which retrieves the value that's associated with a key and changes it to a new value.</span></span> <span data-ttu-id="3c1b9-520">StackExchange 라이브러리를 사용하면 이 작업을 `IDatabase.StringGetSetAsync` 방법을 통해 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-520">The StackExchange library makes this operation available through the `IDatabase.StringGetSetAsync` method.</span></span> <span data-ttu-id="3c1b9-521">아래 코드 조각에 이 메서드의 예가 나와 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-521">The code snippet below shows an example of this method.</span></span> <span data-ttu-id="3c1b9-522">이 코드는 이전 예제에서 "data:counter" 키와 연결된 현재 값을 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-522">This code returns the current value that's associated with the key "data:counter" from the previous example.</span></span> <span data-ttu-id="3c1b9-523">그런 다음 동일한 작업의 일부로 이 키의 값을 0으로 다시 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-523">Then it resets the value for this key back to zero, all as part of the same operation:</span></span>

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  string oldValue = await cache.StringGetSetAsync("data:counter", 0);
  ```

- <span data-ttu-id="3c1b9-524">문자열 값의 집합을 단일 작업으로 반환하거나 변경할 수 있는 `MGET` 및 `MSET`입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-524">`MGET` and `MSET`, which can return or change a set of string values as a single operation.</span></span> <span data-ttu-id="3c1b9-525">`IDatabase.StringGetAsync` 및 `IDatabase.StringSetAsync` 방법은 다음 예와 같이 이 기능을 지원하기 위해 오버로드됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-525">The `IDatabase.StringGetAsync` and `IDatabase.StringSetAsync` methods are overloaded to support this functionality, as shown in the following example:</span></span>

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  // Create a list of key-value pairs
  var keysAndValues =
      new List<KeyValuePair<RedisKey, RedisValue>>()
      {
          new KeyValuePair<RedisKey, RedisValue>("data:key1", "value1"),
          new KeyValuePair<RedisKey, RedisValue>("data:key99", "value2"),
          new KeyValuePair<RedisKey, RedisValue>("data:key322", "value3")
      };
  
  // Store the list of key-value pairs in the cache
  cache.StringSet(keysAndValues.ToArray());
  ...
  // Find all values that match a list of keys
  RedisKey[] keys = { "data:key1", "data:key99", "data:key322"};
  // values should contain { "value1", "value2", "value3" }
  RedisValue[] values = cache.StringGet(keys);

  ```

<span data-ttu-id="3c1b9-526">이 문서의 Redis 트랜잭션 및 배치 섹션에서 설명한 대로 여러 작업을 단일 Redis 트랜잭션으로 결합할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-526">You can also combine multiple operations into a single Redis transaction as described in the Redis transactions and batches section earlier in this article.</span></span> <span data-ttu-id="3c1b9-527">StackExchange 라이브러리는 `ITransaction` 인터페이스를 통해 트랜잭션에 대한 지원을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-527">The StackExchange library provides support for transactions through the `ITransaction` interface.</span></span>

<span data-ttu-id="3c1b9-528">`IDatabase.CreateTransaction` 메서드를 사용하여 `ITransaction` 개체를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-528">You create an `ITransaction` object by using the `IDatabase.CreateTransaction` method.</span></span> <span data-ttu-id="3c1b9-529">`ITransaction` 개체에서 제공하는 메서드를 사용하여 트랜잭션에 명령을 호출합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-529">You invoke commands to the transaction by using the methods provided by the `ITransaction` object.</span></span>

<span data-ttu-id="3c1b9-530">모든 메서드가 비동기임을 제외하고 `ITransaction` 인터페이스는 `IDatabase` 인터페이스에서 액세스하는 메서드와 유사한 메서드 집합에 대한 액세스를 제공합니다. </span><span class="sxs-lookup"><span data-stu-id="3c1b9-530">The `ITransaction` interface provides access to a set of methods that's similar to those accessed by the `IDatabase` interface, except that all the methods are asynchronous.</span></span> <span data-ttu-id="3c1b9-531">즉, `ITransaction.Execute` 메서드가 호출될 때만 수행됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-531">This means that they are only performed when the `ITransaction.Execute` method is invoked.</span></span> <span data-ttu-id="3c1b9-532">`ITransaction.Execute` 메서드에 의해 반환되는 값은 트랜잭션이 성공적으로 만들어졌는지(true), 실패했는지(false) 여부를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-532">The value that's returned by the `ITransaction.Execute` method indicates whether the transaction was created successfully (true) or if it failed (false).</span></span>

<span data-ttu-id="3c1b9-533">다음 코드 조각은 동일한 트랜잭션의 일부로 두 카운터를 증가 및 감소시키는 예제를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-533">The following code snippet shows an example that increments and decrements two counters as part of the same transaction:</span></span>

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
ITransaction transaction = cache.CreateTransaction();
var tx1 = transaction.StringIncrementAsync("data:counter1");
var tx2 = transaction.StringDecrementAsync("data:counter2");
bool result = transaction.Execute();
Console.WriteLine("Transaction {0}", result ? "succeeded" : "failed");
Console.WriteLine("Result of increment: {0}", tx1.Result);
Console.WriteLine("Result of decrement: {0}", tx2.Result);
```

<span data-ttu-id="3c1b9-534">Redis 트랜잭션은 관계형 데이터베이스의 트랜잭션과 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-534">Remember that Redis transactions are unlike transactions in relational databases.</span></span> <span data-ttu-id="3c1b9-535">`Execute` 메서드는 실행용 트랜잭션을 구성하는 모든 명령을 큐에 대기시키고 그 중 하나라도 잘못되면 트랜잭션이 중단됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-535">The `Execute` method simply queues all the commands that comprise the transaction to be run, and if any of them is malformed then the transaction is stopped.</span></span> <span data-ttu-id="3c1b9-536">모든 명령이 성공적으로 큐에 대기한 경우 각 명령은 비동기적으로 실행됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-536">If all the commands have been queued successfully, each command runs asynchronously.</span></span>

<span data-ttu-id="3c1b9-537">어떤 명령이 실패해도 나머지는 여전히 계속 처리됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-537">If any command fails, the others still continue processing.</span></span> <span data-ttu-id="3c1b9-538">명령이 성공적으로 완료되었는지 확인하려는 경우 해당 작업의 **Result** 속성을 사용하여 위의 예제와 같이 명령의 결과를 가져와야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-538">If you need to verify that a command has completed successfully, you must fetch the results of the command by using the **Result** property of the corresponding task, as shown in the example above.</span></span> <span data-ttu-id="3c1b9-539">**Result** 속성을 읽으면 작업을 완료할 때까지 호출 스레드를 차단합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-539">Reading the **Result** property will block the calling thread until the task has completed.</span></span>

<span data-ttu-id="3c1b9-540">자세한 내용은 [Redis의 트랜잭션](https://stackexchange.github.io/StackExchange.Redis/Transactions)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-540">For more information, see [Transactions in Redis](https://stackexchange.github.io/StackExchange.Redis/Transactions).</span></span>

<span data-ttu-id="3c1b9-541">배치 작업을 수행할 때 StackExchange 라이브러리의 `IBatch` 인터페이스를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-541">When performing batch operations, you can use the `IBatch` interface of the StackExchange library.</span></span> <span data-ttu-id="3c1b9-542">이 인터페이스는 모든 메서드가 비동기 작업인 경우를 제외하고 `IDatabase` 인터페이스와 비슷한 메서드 집합에 액세스를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-542">This interface provides access to a set of methods similar to those accessed by the `IDatabase` interface, except that all the methods are asynchronous.</span></span>

<span data-ttu-id="3c1b9-543">다음 예제와 같이 `IDatabase.CreateBatch` 메서드를 사용하여 `IBatch` 개체를 만든 다음 `IBatch.Execute` 메서드를 사용하여 배치를 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-543">You create an `IBatch` object by using the `IDatabase.CreateBatch` method, and then run the batch by using the `IBatch.Execute` method, as shown in the following example.</span></span> <span data-ttu-id="3c1b9-544">이 코드는 단순히 문자열 값을 설정하고 이전 예제에서 사용한 동일한 카운터를 증가 및 감소시키며 결과를 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-544">This code simply sets a string value, increments and decrements the same counters used in the previous example, and displays the results:</span></span>

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
IBatch batch = cache.CreateBatch();
batch.StringSetAsync("data:key1", 11);
var t1 = batch.StringIncrementAsync("data:counter1");
var t2 = batch.StringDecrementAsync("data:counter2");
batch.Execute();
Console.WriteLine("{0}", t1.Result);
Console.WriteLine("{0}", t2.Result);
```

<span data-ttu-id="3c1b9-545">트랜잭션과 달리 배치에서 명령의 형식이 잘못되어 실패한 경우 다른 명령은 계속 실행될 수 있다는 점을 이해해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-545">It is important to understand that unlike a transaction, if a command in a batch fails because it is malformed, the other commands might still run.</span></span> <span data-ttu-id="3c1b9-546">`IBatch.Execute` 메서드는 성공 또는 실패의 모든 표시를 반환하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-546">The `IBatch.Execute` method does not return any indication of success or failure.</span></span>

### <a name="perform-fire-and-forget-cache-operations"></a><span data-ttu-id="3c1b9-547">실행 후 제거 캐시 작업 수행</span><span class="sxs-lookup"><span data-stu-id="3c1b9-547">Perform fire and forget cache operations</span></span>

<span data-ttu-id="3c1b9-548">Redis는 명령 플래그를 사용하여 실행 후 제거 작업을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-548">Redis supports fire and forget operations by using command flags.</span></span> <span data-ttu-id="3c1b9-549">이 경우 클라이언트는 단순히 작업을 시작하지만 결과에 관심이 없고 명령이 완료되기를 기다리지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-549">In this situation, the client simply initiates an operation but has no interest in the result and does not wait for the command to be completed.</span></span> <span data-ttu-id="3c1b9-550">아래 예제에서는 INCR 명령을 실행 후 제거 작업으로 수행하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-550">The example below shows how to perform the INCR command as a fire and forget operation:</span></span>

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
await cache.StringSetAsync("data:key1", 99);
...
cache.StringIncrement("data:key1", flags: CommandFlags.FireAndForget);
```

### <a name="specify-automatically-expiring-keys"></a><span data-ttu-id="3c1b9-551">자동 만료 키 지정</span><span class="sxs-lookup"><span data-stu-id="3c1b9-551">Specify automatically expiring keys</span></span>

<span data-ttu-id="3c1b9-552">Redis 캐시에 항목을 저장하는 경우 어떤 항목을 자동으로 캐시에서 제거할지 시간 제한을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-552">When you store an item in a Redis cache, you can specify a timeout after which the item will be automatically removed from the cache.</span></span> <span data-ttu-id="3c1b9-553">`TTL` 명령을 사용하여 키가 만료되기 전에 얼마나 더 많은 시간이 필요한지 쿼리할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-553">You can also query how much more time a key has before it expires by using the `TTL` command.</span></span> <span data-ttu-id="3c1b9-554">이 명령은 `IDatabase.KeyTimeToLive` 메서드를 사용하여 StackExchange 애플리케이션에 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-554">This command is available to StackExchange applications by using the `IDatabase.KeyTimeToLive` method.</span></span>

<span data-ttu-id="3c1b9-555">다음 코드 조각은 키에 20초로 만료 시간을 설정하고 키의 남은 수명을 쿼리하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-555">The following code snippet shows how to set an expiration time of 20 seconds on a key, and query the remaining lifetime of the key:</span></span>

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Add a key with an expiration time of 20 seconds
await cache.StringSetAsync("data:key1", 99, TimeSpan.FromSeconds(20));
...
// Query how much time a key has left to live
// If the key has already expired, the KeyTimeToLive function returns a null
TimeSpan? expiry = cache.KeyTimeToLive("data:key1");
```

<span data-ttu-id="3c1b9-556">또한 EXPIRE 명령을 사용하여 특정 날짜 및 시간에 만료 시간을 설정할 수 있으며 이는 StackExchange 라이브러리의 `KeyExpireAsync` 메서드로 구현 가능합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-556">You can also set the expiration time to a specific date and time by using the EXPIRE command, which is available in the StackExchange library as the `KeyExpireAsync` method:</span></span>

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Add a key with an expiration date of midnight on 1st January 2015
await cache.StringSetAsync("data:key1", 99);
await cache.KeyExpireAsync("data:key1",
    new DateTime(2015, 1, 1, 0, 0, 0, DateTimeKind.Utc));
...
```

> [!TIP]
> <span data-ttu-id="3c1b9-557">DEL 명령을 사용하여 캐시에서 항목을 수동으로 제거할 수 있으며 이는 `IDatabase.KeyDeleteAsync` 메서드로 StackExchange 라이브러리를 통해 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-557">You can manually remove an item from the cache by using the DEL command, which is available through the StackExchange library as the `IDatabase.KeyDeleteAsync` method.</span></span>

### <a name="use-tags-to-cross-correlate-cached-items"></a><span data-ttu-id="3c1b9-558">태그를 사용하여 캐시된 항목을 상호 비교</span><span class="sxs-lookup"><span data-stu-id="3c1b9-558">Use tags to cross-correlate cached items</span></span>

<span data-ttu-id="3c1b9-559">Redis 집합은 단일 키를 공유하는 여러 항목의 컬렉션입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-559">A Redis set is a collection of multiple items that share a single key.</span></span> <span data-ttu-id="3c1b9-560">SADD 명령을 사용하여 집합을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-560">You can create a set by using the SADD command.</span></span> <span data-ttu-id="3c1b9-561">SMEMBERS 명령을 사용하여 집합의 항목을 검색할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-561">You can retrieve the items in a set by using the SMEMBERS command.</span></span> <span data-ttu-id="3c1b9-562">StackExchange 라이브러리는 `IDatabase.SetAddAsync` 메서드로 SADD 명령을 구현하고 `IDatabase.SetMembersAsync` 메서드로 SMEMBERS 명령을 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-562">The StackExchange library implements the SADD command with the `IDatabase.SetAddAsync` method, and the SMEMBERS command with the `IDatabase.SetMembersAsync` method.</span></span>

<span data-ttu-id="3c1b9-563">SDIFF(차집합), SINTER(교집합) 및 SUNION(합집합) 명령을 사용하여 새 집합을 만들려면 기존 세트를 결합할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-563">You can also combine existing sets to create new sets by using the SDIFF (set difference), SINTER (set intersection), and SUNION (set union) commands.</span></span> <span data-ttu-id="3c1b9-564">StackExchange 라이브러리는 `IDatabase.SetCombineAsync` 메서드에서 이러한 작업을 통합합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-564">The StackExchange library unifies these operations in the `IDatabase.SetCombineAsync` method.</span></span> <span data-ttu-id="3c1b9-565">이 메서드에 대한 첫 번째 매개 변수는 수행할 집합 작업을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-565">The first parameter to this method specifies the set operation to perform.</span></span>

<span data-ttu-id="3c1b9-566">다음 코드 조각은 신속한 저장 및 관련된 항목의 컬렉션을 검색하는데 집합이 유용할 수 있음을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-566">The following code snippets show how sets can be useful for quickly storing and retrieving collections of related items.</span></span> <span data-ttu-id="3c1b9-567">이 코드는 이 문서 앞부분의 Redis Cache 클라이언트 애플리케이션 구현 섹션에 설명된 `BlogPost` 형식을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-567">This code uses the `BlogPost` type that was described in the section Implement Redis Cache Client Applications earlier in this article.</span></span>

<span data-ttu-id="3c1b9-568">`BlogPost` 개체는 ID, 제목, 순위 점수 및 태그의 컬렉션 등 4개의 필드를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-568">A `BlogPost` object contains four fields—an ID, a title, a ranking score, and a collection of tags.</span></span> <span data-ttu-id="3c1b9-569">아래 첫 번째 코드 조각에서 `BlogPost` 개체의 C# 목록을 채우기 위해 사용되는 샘플 데이터를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-569">The first code snippet below shows the sample data that's used for populating a C# list of `BlogPost` objects:</span></span>

```csharp
List<string[]> tags = new List<string[]>
{
    new[] { "iot","csharp" },
    new[] { "iot","azure","csharp" },
    new[] { "csharp","git","big data" },
    new[] { "iot","git","database" },
    new[] { "database","git" },
    new[] { "csharp","database" },
    new[] { "iot" },
    new[] { "iot","database","git" },
    new[] { "azure","database","big data","git","csharp" },
    new[] { "azure" }
};

List<BlogPost> posts = new List<BlogPost>();
int blogKey = 1;
int numberOfPosts = 20;
Random random = new Random();
for (int i = 0; i < numberOfPosts; i++)
{
    blogKey++;
    posts.Add(new BlogPost(
        blogKey,                  // Blog post ID
        string.Format(CultureInfo.InvariantCulture, "Blog Post #{0}",
            blogKey),             // Blog post title
        random.Next(100, 10000),  // Ranking score
        tags[i % tags.Count]));   // Tags--assigned from a collection
                                  // in the tags list
}
```

<span data-ttu-id="3c1b9-570">Redis 캐시의 집합으로 각 `BlogPost` 개체에 대한 태그를 저장하고 각 집합을 `BlogPost`의 ID와 연결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-570">You can store the tags for each `BlogPost` object as a set in a Redis cache and associate each set with the ID of the `BlogPost`.</span></span> <span data-ttu-id="3c1b9-571">이를 통해 애플리케이션이 특정 블로그 게시물에 속한 모든 태그를 신속하게 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-571">This enables an application to quickly find all the tags that belong to a specific blog post.</span></span> <span data-ttu-id="3c1b9-572">반대 방향으로 검색을 수행하고 특정 태그를 공유하는 모든 블로그 게시물을 발견하려면 키에서 태그 ID를 참조하는 블로그 게시물을 보유한 다른 집합을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-572">To enable searching in the opposite direction and find all blog posts that share a specific tag, you can create another set that holds the blog posts referencing the tag ID in the key:</span></span>

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Tags are easily represented as Redis Sets
foreach (BlogPost post in posts)
{
    string redisKey = string.Format(CultureInfo.InvariantCulture,
        "blog:posts:{0}:tags", post.Id);
    // Add tags to the blog post in Redis
    await cache.SetAddAsync(
        redisKey, post.Tags.Select(s => (RedisValue)s).ToArray());

    // Now do the inverse so we can figure how which blog posts have a given tag
    foreach (var tag in post.Tags)
    {
        await cache.SetAddAsync(string.Format(CultureInfo.InvariantCulture,
            "tag:{0}:blog:posts", tag), post.Id);
    }
}
```

<span data-ttu-id="3c1b9-573">이러한 구조를 사용하여 많은 일반적인 쿼리를 매우 효율적으로 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-573">These structures enable you to perform many common queries very efficiently.</span></span> <span data-ttu-id="3c1b9-574">예를 들어 다음과 같은 블로그 게시물 1에 대한 태그를 모두 표시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-574">For example, you can find and display all of the tags for blog post 1 like this:</span></span>

```csharp
// Show the tags for blog post #1
foreach (var value in await cache.SetMembersAsync("blog:posts:1:tags"))
{
    Console.WriteLine(value);
}
```

<span data-ttu-id="3c1b9-575">다음과 같이 교집합 연산을 수행하여 블로그 게시물 1 및 2에 공통되는 모든 태그를 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-575">You can find all tags that are common to blog post 1 and blog post 2 by performing a set intersection operation, as follows:</span></span>

```csharp
// Show the tags in common for blog posts #1 and #2
foreach (var value in await cache.SetCombineAsync(SetOperation.Intersect, new RedisKey[]
    { "blog:posts:1:tags", "blog:posts:2:tags" }))
{
    Console.WriteLine(value);
}
```

<span data-ttu-id="3c1b9-576">특정 태그를 포함하는 모든 블로그 게시물을 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-576">And you can find all blog posts that contain a specific tag:</span></span>

```csharp
// Show the ids of the blog posts that have the tag "iot".
foreach (var value in await cache.SetMembersAsync("tag:iot:blog:posts"))
{
    Console.WriteLine(value);
}
```

### <a name="find-recently-accessed-items"></a><span data-ttu-id="3c1b9-577">최근에 액세스된 항목 찾기</span><span class="sxs-lookup"><span data-stu-id="3c1b9-577">Find recently accessed items</span></span>

<span data-ttu-id="3c1b9-578">많은 애플리케이션의 일반적인 작업은 가장 최근에 액세스한 항목을 찾는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-578">A common task required of many applications is to find the most recently accessed items.</span></span> <span data-ttu-id="3c1b9-579">예를 들어 사이트를 블로깅하면서 가장 최근에 읽은 블로그 게시물에 대한 정보를 표시하려 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-579">For example, a blogging site might want to display information about the most recently read blog posts.</span></span>

<span data-ttu-id="3c1b9-580">Redis 목록을 사용하여 이 기능을 구현할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-580">You can implement this functionality by using a Redis list.</span></span> <span data-ttu-id="3c1b9-581">Redis 목록은 동일한 키를 공유하는 여러 항목을 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-581">A Redis list contains multiple items that share the same key.</span></span> <span data-ttu-id="3c1b9-582">목록은 양쪽이 큐 역할을 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-582">The list acts as a double-ended queue.</span></span> <span data-ttu-id="3c1b9-583">LPUSH(왼쪽 밀어넣기) 및 RPUSH(오른쪽 밀어넣기) 명령을 사용하여 항목을 목록의 한쪽 끝에 푸시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-583">You can push items to either end of the list by using the LPUSH (left push) and RPUSH (right push) commands.</span></span> <span data-ttu-id="3c1b9-584">LPOP 및 RPOP 명령을 사용하여 목록의 한쪽 끝에서 항목을 검색할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-584">You can retrieve items from either end of the list by using the LPOP and RPOP commands.</span></span> <span data-ttu-id="3c1b9-585">또한 LRANGE 및 RRANGE 명령을 사용하여 요소 집합을 반환할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-585">You can also return a set of elements by using the LRANGE and RRANGE commands.</span></span>

<span data-ttu-id="3c1b9-586">아래 코드 조각은 StackExchange 라이브러리를 사용하여 이러한 작업을 수행하는 방법을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-586">The code snippets below show how you can perform these operations by using the StackExchange library.</span></span> <span data-ttu-id="3c1b9-587">이 코드는 이전 예제의 `BlogPost` 형식을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-587">This code uses the `BlogPost` type from the previous examples.</span></span> <span data-ttu-id="3c1b9-588">사용자가 블로그 게시물을 읽을 때 `IDatabase.ListLeftPushAsync` 메서드가 Redis 캐시의 "blog:recent_posts" 키와 연결된 목록에 블로그 게시물의 제목을 푸시합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-588">As a blog post is read by a user, the `IDatabase.ListLeftPushAsync` method pushes the title of the blog post onto a list that's associated with the key "blog:recent_posts" in the Redis cache.</span></span>

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
string redisKey = "blog:recent_posts";
BlogPost blogPost = ...; // Reference to the blog post that has just been read
await cache.ListLeftPushAsync(
    redisKey, blogPost.Title); // Push the blog post onto the list
```

<span data-ttu-id="3c1b9-589">더 많은 블로그 게시물을 읽을수록 제목은 같은 목록에 푸시됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-589">As more blog posts are read, their titles are pushed onto the same list.</span></span> <span data-ttu-id="3c1b9-590">목록은 제목이 추가된 순서로 정렬됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-590">The list is ordered by the sequence in which the titles have been added.</span></span> <span data-ttu-id="3c1b9-591">가장 최근에 읽은 블로그 게시물은 목록의 왼쪽 끝 쪽에 위치합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-591">The most recently read blog posts are towards the left end of the list.</span></span> <span data-ttu-id="3c1b9-592">(동일한 블로그 게시물이 두 번 이상 읽혀진 경우 목록에 여러 항목이 있게 됨)</span><span class="sxs-lookup"><span data-stu-id="3c1b9-592">(If the same blog post is read more than once, it will have multiple entries in the list.)</span></span>

<span data-ttu-id="3c1b9-593">`IDatabase.ListRange` 메서드를 사용하여 가장 최근에 읽은 게시물의 제목을 표시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-593">You can display the titles of the most recently read posts by using the `IDatabase.ListRange` method.</span></span> <span data-ttu-id="3c1b9-594">이 메서드는 목록, 시작점 및 끝점을 포함하는 키를 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-594">This method takes the key that contains the list, a starting point, and an ending point.</span></span> <span data-ttu-id="3c1b9-595">다음 코드는 목록의 가장 왼쪽 끝에서 10개의 블로그 게시물(0에서 9까지의 항목)의 제목을 검색합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-595">The following code retrieves the titles of the 10 blog posts (items from 0 to 9) at the left-most end of the list:</span></span>

```csharp
// Show latest ten posts
foreach (string postTitle in await cache.ListRangeAsync(redisKey, 0, 9))
{
    Console.WriteLine(postTitle);
}
```

<span data-ttu-id="3c1b9-596">`ListRangeAsync` 메서드가 목록에서 항목을 제거하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-596">Note that the `ListRangeAsync` method does not remove items from the list.</span></span> <span data-ttu-id="3c1b9-597">이를 위해 `IDatabase.ListLeftPopAsync` 및 `IDatabase.ListRightPopAsync` 메서드를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-597">To do this, you can use the `IDatabase.ListLeftPopAsync` and `IDatabase.ListRightPopAsync` methods.</span></span>

<span data-ttu-id="3c1b9-598">목록이 무한정 커지지 않도록 하려면 목록을 트리밍하여 항목을 주기적으로 추려내 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-598">To prevent the list from growing indefinitely, you can periodically cull items by trimming the list.</span></span> <span data-ttu-id="3c1b9-599">아래 코드 조각은 목록에서 가장 왼쪽의 5개 항목을 제외하고 모두 제거하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-599">The code snippet below shows you how to remove all but the five left-most items from the list:</span></span>

```csharp
await cache.ListTrimAsync(redisKey, 0, 5);
```

### <a name="implement-a-leader-board"></a><span data-ttu-id="3c1b9-600">리더 보드 구현</span><span class="sxs-lookup"><span data-stu-id="3c1b9-600">Implement a leader board</span></span>

<span data-ttu-id="3c1b9-601">기본적으로 집합에서 항목은 특정 순서로 유지되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-601">By default, the items in a set are not held in any specific order.</span></span> <span data-ttu-id="3c1b9-602">ZADD 명령을 사용하여 정렬된 집합을 만들 수 있습니다(StackExchange 라이브러리의 `IDatabase.SortedSetAdd` 메서드).</span><span class="sxs-lookup"><span data-stu-id="3c1b9-602">You can create an ordered set by using the ZADD command (the `IDatabase.SortedSetAdd` method in the StackExchange library).</span></span> <span data-ttu-id="3c1b9-603">명령에 매개 변수로 제공된 점수인 숫자 값을 사용하여 항목이 정렬됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-603">The items are ordered by using a numeric value called a score, which is provided as a parameter to the command.</span></span>

<span data-ttu-id="3c1b9-604">다음 코드 조각은 정렬된 목록에 블로그 게시물의 제목을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-604">The following code snippet adds the title of a blog post to an ordered list.</span></span> <span data-ttu-id="3c1b9-605">예제에서 각 블로그 게시물에 블로그 게시물의 순위를 포함하는 점수 필드도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-605">In this example, each blog post also has a score field that contains the ranking of the blog post.</span></span>

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
string redisKey = "blog:post_rankings";
BlogPost blogPost = ...; // Reference to a blog post that has just been rated
await cache.SortedSetAddAsync(redisKey, blogPost.Title, blogPost.Score);
```

<span data-ttu-id="3c1b9-606">`IDatabase.SortedSetRangeByRankWithScores` 메서드를 사용하여 블로그 게시물 제목과 점수를 점수 오름차순으로 검색할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-606">You can retrieve the blog post titles and scores in ascending score order by using the `IDatabase.SortedSetRangeByRankWithScores` method:</span></span>

```csharp
foreach (var post in await cache.SortedSetRangeByRankWithScoresAsync(redisKey))
{
    Console.WriteLine(post);
}
```

> [!NOTE]
> <span data-ttu-id="3c1b9-607">StackExchange 라이브러리가 점수 순서로 데이터를 반환하는 `IDatabase.SortedSetRangeByRankAsync` 메서드도 제공하지만 점수를 반환하지는 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-607">The StackExchange library also provides the `IDatabase.SortedSetRangeByRankAsync` method, which returns the data in score order, but does not return the scores.</span></span>

<span data-ttu-id="3c1b9-608">`IDatabase.SortedSetRangeByRankWithScoresAsync` 메서드에 추가 매개 변수를 제공하여 점수의 내림차순에서 항목을 검색하고 반환되는 항목 수를 제한할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-608">You can also retrieve items in descending order of scores, and limit the number of items that are returned by providing additional parameters to the `IDatabase.SortedSetRangeByRankWithScoresAsync` method.</span></span> <span data-ttu-id="3c1b9-609">다음 예제에서 상위 10개의 블로그 게시물의 제목 및 점수를 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-609">The next example displays the titles and scores of the top 10 ranked blog posts:</span></span>

```csharp
foreach (var post in await cache.SortedSetRangeByRankWithScoresAsync(
                               redisKey, 0, 9, Order.Descending))
{
    Console.WriteLine(post);
}
```

<span data-ttu-id="3c1b9-610">다음 예제에서는 지정된 점수 범위 내에 있는 대상에게 반환되는 항목을 제한하기 위해 사용할 수 있는 `IDatabase.SortedSetRangeByScoreWithScoresAsync` 메서드가 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-610">The next example uses the `IDatabase.SortedSetRangeByScoreWithScoresAsync` method, which you can use to limit the items that are returned to those that fall within a given score range:</span></span>

```csharp
// Blog posts with scores between 5000 and 100000
foreach (var post in await cache.SortedSetRangeByScoreWithScoresAsync(
                               redisKey, 5000, 100000))
{
    Console.WriteLine(post);
}
```

### <a name="message-by-using-channels"></a><span data-ttu-id="3c1b9-611">채널을 사용한 메시지</span><span class="sxs-lookup"><span data-stu-id="3c1b9-611">Message by using channels</span></span>

<span data-ttu-id="3c1b9-612">데이터 캐시의 역할 외에도 Redis 서버는 고성능 게시자/구독자 메커니즘을 통해 메시징을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-612">Apart from acting as a data cache, a Redis server provides messaging through a high-performance publisher/subscriber mechanism.</span></span> <span data-ttu-id="3c1b9-613">클라이언트 애플리케이션은 채널을 구독할 수 있고 다른 애플리케이션이나 서비스는 채널에 메시지를 게시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-613">Client applications can subscribe to a channel, and other applications or services can publish messages to the channel.</span></span> <span data-ttu-id="3c1b9-614">애플리케이션을 구독하면 이러한 메시지를 받고 처리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-614">Subscribing applications will then receive these messages and can process them.</span></span>

<span data-ttu-id="3c1b9-615">Redis는 채널 구독에 사용하도록 클라이언트 애플리케이션을 위한 SUBSCRIBE 명령을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-615">Redis provides the SUBSCRIBE command for client applications to use to subscribe to channels.</span></span> <span data-ttu-id="3c1b9-616">이 명령은 애플리케이션이 메시지를 받는 하나 이상의 채널의 이름을 필요로 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-616">This command expects the name of one or more channels on which the application will accept messages.</span></span> <span data-ttu-id="3c1b9-617">StackExchange 라이브러리에는 .NET Framework 애플리케이션이 채널을 구독하고 채널에 게시할 수 있는 `ISubscription` 인터페이스가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-617">The StackExchange library includes the `ISubscription` interface, which enables a .NET Framework application to subscribe and publish to channels.</span></span>

<span data-ttu-id="3c1b9-618">Redis 서버를 연결하는 `GetSubscriber` 메서드를 사용하여 `ISubscription` 개체를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-618">You create an `ISubscription` object by using the `GetSubscriber` method of the connection to the Redis server.</span></span> <span data-ttu-id="3c1b9-619">그런 다음 이 개체의 `SubscribeAsync` 메서드를 사용하여 채널에서 메시지를 수신 대기합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-619">Then you listen for messages on a channel by using the `SubscribeAsync` method of this object.</span></span> <span data-ttu-id="3c1b9-620">다음 코드 예제에서 "messages:blogPosts"라는 채널을 구독하는 방법을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-620">The following code example shows how to subscribe to a channel named "messages:blogPosts":</span></span>

```csharp
ConnectionMultiplexer redisHostConnection = ...;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
...
await subscriber.SubscribeAsync("messages:blogPosts", (channel, message) => Console.WriteLine("Title is: {0}", message));
```

<span data-ttu-id="3c1b9-621">`Subscribe` 메서드의 첫 번째 매개 변수는 채널의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-621">The first parameter to the `Subscribe` method is the name of the channel.</span></span> <span data-ttu-id="3c1b9-622">이 이름은 캐시의 키에서 사용되는 동일한 규칙을 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-622">This name follows the same conventions that are used by keys in the cache.</span></span> <span data-ttu-id="3c1b9-623">좋은 성능과 관리 효율을 보장하도록 비교적 짧고 의미 있는 문자열을 사용하는 것이 좋지만 이름에는 모든 이진 데이터가 포함될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-623">The name can contain any binary data, although it is advisable to use relatively short, meaningful strings to help ensure good performance and maintainability.</span></span>

<span data-ttu-id="3c1b9-624">또한 채널에서 사용되는 네임스페이스는 키에서 사용되는 네임스페이스와는 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-624">Note also that the namespace used by channels is separate from that used by keys.</span></span> <span data-ttu-id="3c1b9-625">즉, 애플리케이션 코드를 유지하는 데 더 어렵게 만들 수도 있지만 동일한 이름의 채널 및 키를 가질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-625">This means you can have channels and keys that have the same name, although this may make your application code more difficult to maintain.</span></span>

<span data-ttu-id="3c1b9-626">두 번째 매개 변수는 Action 대리자입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-626">The second parameter is an Action delegate.</span></span> <span data-ttu-id="3c1b9-627">이 대리자는 새 메시지가 채널에 나타날 때마다 비동기적으로 실행됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-627">This delegate runs asynchronously whenever a new message appears on the channel.</span></span> <span data-ttu-id="3c1b9-628">이 예에서 단순히 콘솔에 메시지를 표시합니다.(메시지는 블로그 게시물의 제목을 포함합니다)</span><span class="sxs-lookup"><span data-stu-id="3c1b9-628">This example simply displays the message on the console (the message will contain the title of a blog post).</span></span>

<span data-ttu-id="3c1b9-629">채널에 게시하려면 애플리케이션이 Redis PUBLISH 명령을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-629">To publish to a channel, an application can use the Redis PUBLISH command.</span></span> <span data-ttu-id="3c1b9-630">StackExchange 라이브러리는 이 작업을 수행할 `IServer.PublishAsync` 메서드를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-630">The StackExchange library provides the `IServer.PublishAsync` method to perform this operation.</span></span> <span data-ttu-id="3c1b9-631">다음 코드 조각에서 "messages:blogPosts" 채널에 메시지를 게시하는 방법을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-631">The next code snippet shows how to publish a message to the "messages:blogPosts" channel:</span></span>

```csharp
ConnectionMultiplexer redisHostConnection = ...;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
...
BlogPost blogPost = ...;
subscriber.PublishAsync("messages:blogPosts", blogPost.Title);
```

<span data-ttu-id="3c1b9-632">다음은 게시/구독 메커니즘에 대해 이해해야 할 몇 가지 사항입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-632">There are several points you should understand about the publish/subscribe mechanism:</span></span>

- <span data-ttu-id="3c1b9-633">여러 구독자가 동일한 채널을 구독할 수 있고 해당 채널에 게시된 메시지를 받을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-633">Multiple subscribers can subscribe to the same channel, and they will all receive the messages that are published to that channel.</span></span>
- <span data-ttu-id="3c1b9-634">구독자는 구독한 후 게시된 메시지를 단지 수신합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-634">Subscribers only receive messages that have been published after they have subscribed.</span></span> <span data-ttu-id="3c1b9-635">채널은 버퍼링되지 않으며 메시지가 게시되면 Redis 인프라가 각 구독자에게 메시지를 푸시한 다음 제거합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-635">Channels are not buffered, and once a message is published, the Redis infrastructure pushes the message to each subscriber and then removes it.</span></span>
- <span data-ttu-id="3c1b9-636">기본적으로 구독자는 보낸 순서대로 메시지를 수신합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-636">By default, messages are received by subscribers in the order in which they are sent.</span></span> <span data-ttu-id="3c1b9-637">메시지 및 많은 구독자와 게시자 다수를 포함한 매우 활발 시스템에서 메시지의 보장된 순차적인 배달은 시스템의 성능을 저하시킬 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-637">In a highly active system with a large number of messages and many subscribers and publishers, guaranteed sequential delivery of messages can slow performance of the system.</span></span> <span data-ttu-id="3c1b9-638">각 메시지가 독립적이며 순서가 중요하지 않은 경우 응답성을 향상시킬 수 있는 Redis 시스템에서 동시 처리를 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-638">If each message is independent and the order is unimportant, you can enable concurrent processing by the Redis system, which can help to improve responsiveness.</span></span> <span data-ttu-id="3c1b9-639">구독자가 False에 사용하는 연결의 PreserveAsyncOrder를 설정하여 StackExchange 클라이언트에서 이를 달성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-639">You can achieve this in a StackExchange client by setting the PreserveAsyncOrder of the connection used by the subscriber to false:</span></span>

```csharp
ConnectionMultiplexer redisHostConnection = ...;
redisHostConnection.PreserveAsyncOrder = false;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
```

### <a name="serialization-considerations"></a><span data-ttu-id="3c1b9-640">직렬화 시 고려 사항</span><span class="sxs-lookup"><span data-stu-id="3c1b9-640">Serialization considerations</span></span>

<span data-ttu-id="3c1b9-641">직렬화 형식을 선택할 때는 성능, 상호 운용성, 버전 관리, 기존 시스템과의 호환성, 데이터 압축 및 메모리 오버 헤드 간의 상쇄를 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-641">When you choose a serialization format, consider tradeoffs between performance, interoperability, versioning, compatibility with existing systems, data compression, and memory overhead.</span></span> <span data-ttu-id="3c1b9-642">성능을 평가하는 경우 벤치마크가 컨텍스트에 따라 크게 달라진다는 점에 유의합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-642">When you are evaluating performance, remember that benchmarks are highly dependent on context.</span></span> <span data-ttu-id="3c1b9-643">실제 작업을 반영하지 않을 수 있으며, 최신 라이브러리 또는 버전을 고려하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-643">They may not reflect your actual workload, and may not consider newer libraries or versions.</span></span> <span data-ttu-id="3c1b9-644">모든 시나리오에 대해 "가장 빠른" 단일 직렬 변환기가 있는 것은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-644">There is no single "fastest" serializer for all scenarios.</span></span>

<span data-ttu-id="3c1b9-645">다음은 몇 가지 고려해야 할 옵션입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-645">Some options to consider include:</span></span>

- <span data-ttu-id="3c1b9-646">[프로토콜 버퍼](https://github.com/google/protobuf)(protobuf라고도 함)는 Google에서 구조화된 데이터를 효율적으로 직렬화하기 위해 개발한 직렬화 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-646">[Protocol Buffers](https://github.com/google/protobuf) (also called protobuf) is a serialization format developed by Google for serializing structured data efficiently.</span></span> <span data-ttu-id="3c1b9-647">여기서는 강력한 형식의 정의 파일을 사용하여 메시지 구조를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-647">It uses strongly-typed definition files to define message structures.</span></span> <span data-ttu-id="3c1b9-648">이러한 정의 파일은 메시지의 직렬화 및 역직렬화를 위해 언어 관련 코드로 컴파일됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-648">These definition files are then compiled to language-specific code for serializing and deserializing messages.</span></span> <span data-ttu-id="3c1b9-649">Protobuf를 기존 RPC 메커니즘 대신 사용할 수도 있고, 이를 통해 RPC 서비스를 생성할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-649">Protobuf can be used over existing RPC mechanisms, or it can generate an RPC service.</span></span>

- <span data-ttu-id="3c1b9-650">[Apache Thrift](https://thrift.apache.org/)는 강력한 형식의 정의 파일 및 컴파일 단계와 함께 유사한 방법을 사용하여 직렬화 코드 및 RPC 서비스를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-650">[Apache Thrift](https://thrift.apache.org/) uses a similar approach, with strongly typed definition files and a compilation step to generate the serialization code and RPC services.</span></span>

- <span data-ttu-id="3c1b9-651">[Apache Avro](https://avro.apache.org/)는 프로토콜 버퍼 및 Thrift에 유사한 기능을 제공하지만 컴파일 단계는 없습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-651">[Apache Avro](https://avro.apache.org/) provides similar functionality to Protocol Buffers and Thrift, but there is no compilation step.</span></span> <span data-ttu-id="3c1b9-652">대신, 직렬화된 데이터는 항상 구조를 설명하는 스키마를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-652">Instead, serialized data always includes a schema that describes the structure.</span></span>

- <span data-ttu-id="3c1b9-653">[JSON](https://json.org/)은 인간이 이해하기 쉬운 텍스트 필드를 사용하는 개방형 표준입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-653">[JSON](https://json.org/) is an open standard that uses human-readable text fields.</span></span> <span data-ttu-id="3c1b9-654">광범위한 플랫폼 간 지원을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-654">It has broad cross-platform support.</span></span> <span data-ttu-id="3c1b9-655">JSON은 메시지 스키마를 사용하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-655">JSON does not use message schemas.</span></span> <span data-ttu-id="3c1b9-656">텍스트 기반 형식이기 때문에, 네트워크상에서는 별로 효율적이지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-656">Being a text-based format, it is not very efficient over the wire.</span></span> <span data-ttu-id="3c1b9-657">그러나 경우에 따라, HTTP를 통해 클라이언트로 직접 캐시된 항목을 반환할 수 있습니다. 이 경우 JSON을 저장하면 다른 형식에서 역직렬화했다가 JSON으로 직렬화하는 비용을 절감할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-657">In some cases, however, you may be returning cached items directly to a client via HTTP, in which case storing JSON could save the cost of deserializing from another format and then serializing to JSON.</span></span>

- <span data-ttu-id="3c1b9-658">[BSON](http://bsonspec.org/)은 JSON과 유사한 구조를 사용하는 이진 직렬화 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-658">[BSON](http://bsonspec.org/) is a binary serialization format that uses a structure similar to JSON.</span></span> <span data-ttu-id="3c1b9-659">BSON은 JSON에 비해 가볍고, 검색이 용이하고, 직렬화 및 역직렬화가 빠르도록 디자인되었습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-659">BSON was designed to be lightweight, easy to scan, and fast to serialize and deserialize, relative to JSON.</span></span> <span data-ttu-id="3c1b9-660">페이로드는 JSON에 크기와 비슷합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-660">Payloads are comparable in size to JSON.</span></span> <span data-ttu-id="3c1b9-661">데이터에 따라, BSON 페이로드는 JSON 페이로드보다 작거나 클 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-661">Depending on the data, a BSON payload may be smaller or larger than a JSON payload.</span></span> <span data-ttu-id="3c1b9-662">BSON은 JSON에서는 사용할 수 없는 몇 가지 추가 데이터 형식을 갖습니다. 특히 BinData(바이트 배열) 및 Date가 여기에 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-662">BSON has some additional data types that are not available in JSON, notably BinData (for byte arrays) and Date.</span></span>

- <span data-ttu-id="3c1b9-663">[MessagePack](https://msgpack.org/)은 네트워크를 통해 전송하기 위해 압축되도록 디자인된 이진 직렬화 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-663">[MessagePack](https://msgpack.org/) is a binary serialization format that is designed to be compact for transmission over the wire.</span></span> <span data-ttu-id="3c1b9-664">메시지 스키마 또는 메시지 형식 확인은 수행되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-664">There are no message schemas or message type checking.</span></span>

- <span data-ttu-id="3c1b9-665">[Bond](https://microsoft.github.io/bond/)는 스키마화된 데이터로 작업하기 위한 플랫폼 간 프레임워크입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-665">[Bond](https://microsoft.github.io/bond/) is a cross-platform framework for working with schematized data.</span></span> <span data-ttu-id="3c1b9-666">이 프레임워크는 언어 간 직렬화 및 역직렬화를 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-666">It supports cross-language serialization and deserialization.</span></span> <span data-ttu-id="3c1b9-667">여기에 나열된 다른 시스템과의 중요한 차이점은 상속, 형식 별칭 및 제네릭에 대한 지원입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-667">Notable differences from other systems listed here are support for inheritance, type aliases, and generics.</span></span>

- <span data-ttu-id="3c1b9-668">[gRPC](https://www.grpc.io/)는 는 Google에서 개발한 오픈 소스 RPC 시스템입니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-668">[gRPC](https://www.grpc.io/) is an open source RPC system developed by Google.</span></span> <span data-ttu-id="3c1b9-669">기본적으로 프로토콜 버퍼를 해당 정의 언어 및 기본 메시지 교환 형식으로 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-669">By default, it uses Protocol Buffers as its definition language and underlying message interchange format.</span></span>

## <a name="related-patterns-and-guidance"></a><span data-ttu-id="3c1b9-670">관련 패턴 및 지침</span><span class="sxs-lookup"><span data-stu-id="3c1b9-670">Related patterns and guidance</span></span>

<span data-ttu-id="3c1b9-671">애플리케이션에서 캐싱을 구현하는 경우 다음 패턴도 시나리오와 관련이 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-671">The following patterns might also be relevant to your scenario when you implement caching in your applications:</span></span>

- <span data-ttu-id="3c1b9-672">[캐시 배제 패턴](../patterns/cache-aside.md): 이 패턴은 요청 시 데이터를 데이터 저장소에서 캐시로 로드하는 방법을 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-672">[Cache-aside pattern](../patterns/cache-aside.md): This pattern describes how to load data on demand into a cache from a data store.</span></span> <span data-ttu-id="3c1b9-673">또한 이 패턴은 캐시에 저장된 데이터 및 원래 데이터 저장소의 데이터 간 일관성을 유지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-673">This pattern also helps to maintain consistency between data that's held in the cache and the data in the original data store.</span></span>

- <span data-ttu-id="3c1b9-674">[분할 패턴](../patterns/sharding.md)은 많은 양의 데이터를 저장 및 액세스하는 경우 확장성을 향상시키는 수평 분할을 구현할 정보를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="3c1b9-674">The [Sharding pattern](../patterns/sharding.md) provides information about implementing horizontal partitioning to help improve scalability when storing and accessing large volumes of data.</span></span>

## <a name="more-information"></a><span data-ttu-id="3c1b9-675">자세한 정보</span><span class="sxs-lookup"><span data-stu-id="3c1b9-675">More information</span></span>

- [<span data-ttu-id="3c1b9-676">Azure Redis Cache 설명서</span><span class="sxs-lookup"><span data-stu-id="3c1b9-676">Azure Redis Cache documentation</span></span>](https://azure.microsoft.com/documentation/services/cache/) 
- [<span data-ttu-id="3c1b9-677">Azure Redis Cache FAQ</span><span class="sxs-lookup"><span data-stu-id="3c1b9-677">Azure Redis Cache FAQ</span></span>](/azure/redis-cache/cache-faq)
- [<span data-ttu-id="3c1b9-678">작업 기반 비동기 패턴</span><span class="sxs-lookup"><span data-stu-id="3c1b9-678">Task-based Asynchronous pattern</span></span>](/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)
- [<span data-ttu-id="3c1b9-679">Redis 설명서</span><span class="sxs-lookup"><span data-stu-id="3c1b9-679">Redis documentation</span></span>](https://redis.io/documentation)
- [<span data-ttu-id="3c1b9-680">StackExchange.Redis</span><span class="sxs-lookup"><span data-stu-id="3c1b9-680">StackExchange.Redis</span></span>](https://stackexchange.github.io/StackExchange.Redis/)
- [<span data-ttu-id="3c1b9-681">데이터 분할 지침</span><span class="sxs-lookup"><span data-stu-id="3c1b9-681">Data partitioning guide</span></span>](https://msdn.microsoft.com/library/dn589795.aspx)
