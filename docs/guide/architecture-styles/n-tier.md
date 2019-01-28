---
title: N 계층 아키텍처 스타일
titleSuffix: Azure Application Architecture Guide
description: Azure에서 N 계층 아키텍처의 혜택, 과제 및 모범 사례를 설명합니다.
author: MikeWasson
ms.date: 08/30/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: seojan19
ms.openlocfilehash: d7b94d56831a6b9172a9091f0e4f7fa63a8881f1
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54481493"
---
# <a name="n-tier-architecture-style"></a><span data-ttu-id="d14a4-103">N 계층 아키텍처 스타일</span><span class="sxs-lookup"><span data-stu-id="d14a4-103">N-tier architecture style</span></span>

<span data-ttu-id="d14a4-104">N 계층 아키텍처는 애플리케이션을 **논리 레이어**와 **물리적 계층**으로 나눕니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-104">An N-tier architecture divides an application into **logical layers** and **physical tiers**.</span></span>

![N 계층 아키텍처 스타일의 논리 다이어그램](./images/n-tier-logical.svg)

<span data-ttu-id="d14a4-106">레이어는 책임을 구분하고 종속성을 관리하는 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-106">Layers are a way to separate responsibilities and manage dependencies.</span></span> <span data-ttu-id="d14a4-107">레이어마다 특정 책임이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-107">Each layer has a specific responsibility.</span></span> <span data-ttu-id="d14a4-108">상위 레이어는 하위 레이어의 서비스를 사용할 수 있지만 하위 레이어는 상위 레이어의 서비스를 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-108">A higher layer can use services in a lower layer, but not the other way around.</span></span>

<span data-ttu-id="d14a4-109">계층은 물리적으로 분리되어 별도의 시스템에서 실행됩니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-109">Tiers are physically separated, running on separate machines.</span></span> <span data-ttu-id="d14a4-110">계층은 다른 계층을 직접 호출하거나 비동기 메시징(메시지 큐)을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-110">A tier can call to another tier directly, or use asynchronous messaging (message queue).</span></span> <span data-ttu-id="d14a4-111">각 레이어가 자체 계층에 호스트될 수 있지만 필수는 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-111">Although each layer might be hosted in its own tier, that's not required.</span></span> <span data-ttu-id="d14a4-112">여러 레이어가 동일한 계층에 호스트될 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-112">Several layers might be hosted on the same tier.</span></span> <span data-ttu-id="d14a4-113">계층을 물리적으로 분리하면 확장성과 복원력이 향상되지만 추가 네트워크 통신으로 인해 대기 시간도 증가합니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-113">Physically separating the tiers improves scalability and resiliency, but also adds latency from the additional network communication.</span></span>

<span data-ttu-id="d14a4-114">기존 3계층 애플리케이션에는 프레젠테이션 계층, 중간 계층 및 데이터베이스 계층이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-114">A traditional three-tier application has a presentation tier, a middle tier, and a database tier.</span></span> <span data-ttu-id="d14a4-115">중간 계층은 선택 사항입니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-115">The middle tier is optional.</span></span> <span data-ttu-id="d14a4-116">더 복잡한 애플리케이션은 4개 이상의 계층으로 구성될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-116">More complex applications can have more than three tiers.</span></span> <span data-ttu-id="d14a4-117">위 다이어그램은 각기 다른 기능 영역을 캡슐화하는 두 개의 중간 계층이 있는 애플리케이션을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-117">The diagram above shows an application with two middle tiers, encapsulating different areas of functionality.</span></span>

<span data-ttu-id="d14a4-118">N 계층 애플리케이션은 **폐쇄형 레이어 아키텍처** 또는 **개방형 레이어 아키텍처**를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-118">An N-tier application can have a **closed layer architecture** or an **open layer architecture**:</span></span>

- <span data-ttu-id="d14a4-119">폐쇄형 레이어 아키텍처에서는 레이어가 바로 아래에 있는 다음 레이어만 호출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-119">In a closed layer architecture, a layer can only call the next layer immediately down.</span></span>
- <span data-ttu-id="d14a4-120">개방형 레이어 아키텍처에서는 레이어가 아래에 있는 모든 레이어를 호출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-120">In an open layer architecture, a layer can call any of the layers below it.</span></span>

<span data-ttu-id="d14a4-121">폐쇄형 레이어 아키텍처는 레이어 간의 종속성을 제한합니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-121">A closed layer architecture limits the dependencies between layers.</span></span> <span data-ttu-id="d14a4-122">그러나 레이어가 단순히 요청을 다음 레이어로 전달하는 경우 불필요한 네트워크 트래픽을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-122">However, it might create unnecessary network traffic, if one layer simply passes requests along to the next layer.</span></span>

## <a name="when-to-use-this-architecture"></a><span data-ttu-id="d14a4-123">이 아키텍처를 사용하는 경우</span><span class="sxs-lookup"><span data-stu-id="d14a4-123">When to use this architecture</span></span>

<span data-ttu-id="d14a4-124">N 계층 아키텍처는 일반적으로 각 계층이 개별 VM 집합에서 실행되는 IaaS(Infrastructure-as-Service)애플리케이션으로 구현됩니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-124">N-tier architectures are typically implemented as infrastructure-as-service (IaaS) applications, with each tier running on a separate set of VMs.</span></span> <span data-ttu-id="d14a4-125">그러나 N 계층 애플리케이션은 순수 IaaS가 아니어도 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-125">However, an N-tier application doesn't need to be pure IaaS.</span></span> <span data-ttu-id="d14a4-126">특히 캐싱, 메시징, 데이터 저장소 같은 일부 아키텍처 부분에는 관리 서비스를 사용하는 것이 유리한 경우가 많습니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-126">Often, it's advantageous to use managed services for some parts of the architecture, particularly caching, messaging, and data storage.</span></span>

<span data-ttu-id="d14a4-127">다음과 같은 경우 N 계층 아키텍처를 고려해 보세요.</span><span class="sxs-lookup"><span data-stu-id="d14a4-127">Consider an N-tier architecture for:</span></span>

- <span data-ttu-id="d14a4-128">단순 웹 애플리케이션.</span><span class="sxs-lookup"><span data-stu-id="d14a4-128">Simple web applications.</span></span>
- <span data-ttu-id="d14a4-129">최소 리팩터링으로 온-프레미스 애플리케이션을 Azure로 마이그레이션.</span><span class="sxs-lookup"><span data-stu-id="d14a4-129">Migrating an on-premises application to Azure with minimal refactoring.</span></span>
- <span data-ttu-id="d14a4-130">온-프레미스 및 클라우드 애플리케이션의 통합 개발.</span><span class="sxs-lookup"><span data-stu-id="d14a4-130">Unified development of on-premises and cloud applications.</span></span>

<span data-ttu-id="d14a4-131">N 계층 아키텍처는 기존 온-프레미스 애플리케이션에서 매우 일반적이므로 기존 워크로드를 Azure로 마이그레이션하는 데 적합합니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-131">N-tier architectures are very common in traditional on-premises applications, so it's a natural fit for migrating existing workloads to Azure.</span></span>

## <a name="benefits"></a><span data-ttu-id="d14a4-132">이점</span><span class="sxs-lookup"><span data-stu-id="d14a4-132">Benefits</span></span>

- <span data-ttu-id="d14a4-133">클라우드와 온-프레미스 간, 클라우드 플랫폼 간의 이식성.</span><span class="sxs-lookup"><span data-stu-id="d14a4-133">Portability between cloud and on-premises, and between cloud platforms.</span></span>
- <span data-ttu-id="d14a4-134">대부분의 개발자에게 필요한 학습 곡선 단축.</span><span class="sxs-lookup"><span data-stu-id="d14a4-134">Less learning curve for most developers.</span></span>
- <span data-ttu-id="d14a4-135">기존 애플리케이션 모델에서 자연스러운 발전.</span><span class="sxs-lookup"><span data-stu-id="d14a4-135">Natural evolution from the traditional application model.</span></span>
- <span data-ttu-id="d14a4-136">이기종 환경(Windows/Linux)에 대한 개방성.</span><span class="sxs-lookup"><span data-stu-id="d14a4-136">Open to heterogeneous environment (Windows/Linux)</span></span>

## <a name="challenges"></a><span data-ttu-id="d14a4-137">과제</span><span class="sxs-lookup"><span data-stu-id="d14a4-137">Challenges</span></span>

- <span data-ttu-id="d14a4-138">데이터베이스에서 CRUD 작업을 수행하여 유용한 작업 없이 대기 시간만 늘리는 중간 계층이 생성되기 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-138">It's easy to end up with a middle tier that just does CRUD operations on the database, adding extra latency without doing any useful work.</span></span>
- <span data-ttu-id="d14a4-139">모놀리식 디자인으로 인해 기능의 독립적 배포가 불가능합니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-139">Monolithic design prevents independent deployment of features.</span></span>
- <span data-ttu-id="d14a4-140">IaaS 애플리케이션 관리가 관리 서비스만 사용하는 애플리케이션보다 더 복잡합니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-140">Managing an IaaS application is more work than an application that uses only managed services.</span></span>
- <span data-ttu-id="d14a4-141">대규모 시스템에서는 네트워크 보안을 관리하기 어려울 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-141">It can be difficult to manage network security in a large system.</span></span>

## <a name="best-practices"></a><span data-ttu-id="d14a4-142">모범 사례</span><span class="sxs-lookup"><span data-stu-id="d14a4-142">Best practices</span></span>

- <span data-ttu-id="d14a4-143">자동 크기 조정을 사용하여 부하 변경을 처리합니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-143">Use autoscaling to handle changes in load.</span></span> <span data-ttu-id="d14a4-144">[자동 크기 조정 모범 사례][autoscaling]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="d14a4-144">See [Autoscaling best practices][autoscaling].</span></span>
- <span data-ttu-id="d14a4-145">비동기 메시징을 사용하여 계층을 분리합니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-145">Use asynchronous messaging to decouple tiers.</span></span>
- <span data-ttu-id="d14a4-146">반정적 데이터를 캐시합니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-146">Cache semi-static data.</span></span> <span data-ttu-id="d14a4-147">[캐싱 모범 사례][caching]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="d14a4-147">See [Caching best practices][caching].</span></span>
- <span data-ttu-id="d14a4-148">[SQL Server Always On 가용성 그룹][sql-always-on] 등의 솔루션을 사용하여 고가용성을 위해 데이터베이스 계층을 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-148">Configure database tier for high availability, using a solution such as [SQL Server Always On Availability Groups][sql-always-on].</span></span>
- <span data-ttu-id="d14a4-149">프런트 엔드와 인터넷 사이에 WAF(웹 애플리케이션 방화벽)를 배치합니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-149">Place a web application firewall (WAF) between the front end and the Internet.</span></span>
- <span data-ttu-id="d14a4-150">각 계층을 자체 서브넷에 배치하고 서브넷을 보안 경계로 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-150">Place each tier in its own subnet, and use subnets as a security boundary.</span></span>
- <span data-ttu-id="d14a4-151">중간 계층의 요청만 허용하여 데이터 계층에 대한 액세스를 제한합니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-151">Restrict access to the data tier, by allowing requests only from the middle tier(s).</span></span>

## <a name="n-tier-architecture-on-virtual-machines"></a><span data-ttu-id="d14a4-152">가상 머신의 N 계층 아키텍처</span><span class="sxs-lookup"><span data-stu-id="d14a4-152">N-tier architecture on virtual machines</span></span>

<span data-ttu-id="d14a4-153">이 섹션에서는 VM에서 실행되는 권장 N 계층 아키텍처에 대해 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-153">This section describes a recommended N-tier architecture running on VMs.</span></span>

![N 계층 아키텍처의 물리적 다이어그램](./images/n-tier-physical.png)

<span data-ttu-id="d14a4-155">각 계층은 두 개 이상의 VM으로 구성되며 가용성 집합 또는 VM 확장 집합에 배치됩니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-155">Each tier consists of two or more VMs, placed in an availability set or VM scale set.</span></span> <span data-ttu-id="d14a4-156">한 VM이 실패할 경우 여러 VM이 복원을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-156">Multiple VMs provide resiliency in case one VM fails.</span></span> <span data-ttu-id="d14a4-157">부하 분산 장치가 계층의 VM에 요청을 분배하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-157">Load balancers are used to distribute requests across the VMs in a tier.</span></span> <span data-ttu-id="d14a4-158">풀에 VM을 더 추가하여 계층을 수평적으로 확장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-158">A tier can be scaled horizontally by adding more VMs to the pool.</span></span>

<span data-ttu-id="d14a4-159">또한 각 계층이 자체 서브넷 안에 배치되므로 내부 IP 주소가 동일한 주소 범위 내에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-159">Each tier is also placed inside its own subnet, meaning their internal IP addresses fall within the same address range.</span></span> <span data-ttu-id="d14a4-160">이렇게 하면 NSG(네트워크 보안 그룹) 규칙을 적용하고 테이블을 개별 계층으로 라우트하기 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-160">That makes it easy to apply network security group (NSG) rules and route tables to individual tiers.</span></span>

<span data-ttu-id="d14a4-161">웹 계층과 비즈니스 계층은 상태 비저장입니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-161">The web and business tiers are stateless.</span></span> <span data-ttu-id="d14a4-162">모든 VM이 해당 계층에 대한 요청을 처리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-162">Any VM can handle any request for that tier.</span></span> <span data-ttu-id="d14a4-163">데이터 계층은 복제된 데이터베이스로 구성되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-163">The data tier should consist of a replicated database.</span></span> <span data-ttu-id="d14a4-164">Windows의 경우 SQL Server가 권장되며 고가용성을 위해 Always On 가용성 그룹을 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-164">For Windows, we recommend SQL Server, using Always On Availability Groups for high availability.</span></span> <span data-ttu-id="d14a4-165">Linux의 경우 복제를 지원하는 Apache Cassandra 등의 데이터베이스를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-165">For Linux, choose a database that supports replication, such as Apache Cassandra.</span></span>

<span data-ttu-id="d14a4-166">NSG(네트워크 보안 그룹)는 각 계층에 대한 액세스를 제한합니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-166">Network Security Groups (NSGs) restrict access to each tier.</span></span> <span data-ttu-id="d14a4-167">예를 들어 데이터베이스 계층은 비즈니스 계층의 액세스만 허용합니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-167">For example, the database tier only allows access from the business tier.</span></span>

<span data-ttu-id="d14a4-168">Azure에서 N 계층 애플리케이션 실행에 대한 자세한 내용은 다음을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="d14a4-168">For more information about running N-tier applications on Azure:</span></span>

- <span data-ttu-id="d14a4-169">[N 계층 애플리케이션에 대해 Windows VM 실행][n-tier-windows]</span><span class="sxs-lookup"><span data-stu-id="d14a4-169">[Run Windows VMs for an N-tier application][n-tier-windows]</span></span>
- <span data-ttu-id="d14a4-170">[SQL Server를 사용한 Azure의 Windows N 계층 애플리케이션][n-tier-linux]</span><span class="sxs-lookup"><span data-stu-id="d14a4-170">[Windows N-tier application on Azure with SQL Server][n-tier-linux]</span></span>
- [<span data-ttu-id="d14a4-171">Microsoft 학습 모듈: N 계층 아키텍처 스타일 둘러보기</span><span class="sxs-lookup"><span data-stu-id="d14a4-171">Microsoft Learn module: Tour the N-tier architecture style</span></span>](/learn/modules/n-tier-architecture/)

### <a name="additional-considerations"></a><span data-ttu-id="d14a4-172">추가 고려 사항</span><span class="sxs-lookup"><span data-stu-id="d14a4-172">Additional considerations</span></span>

- <span data-ttu-id="d14a4-173">N 계층 아키텍처는 3개의 계층으로 제한되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-173">N-tier architectures are not restricted to three tiers.</span></span> <span data-ttu-id="d14a4-174">더 복잡한 애플리케이션의 경우 추가 계층을 사용하는 것이 일반적입니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-174">For more complex applications, it is common to have more tiers.</span></span> <span data-ttu-id="d14a4-175">이 경우 레이어 7 라우팅을 사용하여 요청을 특정 계층으로 라우트하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-175">In that case, consider using layer-7 routing to route requests to a particular tier.</span></span>

- <span data-ttu-id="d14a4-176">계층은 확장성, 안정성 및 보안의 경계입니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-176">Tiers are the boundary of scalability, reliability, and security.</span></span> <span data-ttu-id="d14a4-177">이러한 영역에서 요구 사항이 각기 다른 서비스에 대해 개별 계층을 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-177">Consider having separate tiers for services with different requirements in those areas.</span></span>

- <span data-ttu-id="d14a4-178">자동 크기 조정을 위해 VM 확장 집합을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-178">Use VM Scale Sets for autoscaling.</span></span>

- <span data-ttu-id="d14a4-179">아키텍처에서 대규모 리팩터링 없이 관리 서비스를 사용할 수 있는 영역을 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-179">Look for places in the architecture where you can use a managed service without significant refactoring.</span></span> <span data-ttu-id="d14a4-180">특히 캐싱, 메시징, 저장소 및 데이터베이스를 찾으세요.</span><span class="sxs-lookup"><span data-stu-id="d14a4-180">In particular, look at caching, messaging, storage, and databases.</span></span>

- <span data-ttu-id="d14a4-181">보안 강화를 위해 애플리케이션 앞에 네트워크 DMZ를 배치합니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-181">For higher security, place a network DMZ in front of the application.</span></span> <span data-ttu-id="d14a4-182">DMZ에는 방화벽 및 패킷 검사와 같은 보안 기능을 구현하는 NVA(네트워크 가상 어플라이언스)가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-182">The DMZ includes network virtual appliances (NVAs) that implement security functionality such as firewalls and packet inspection.</span></span> <span data-ttu-id="d14a4-183">자세한 내용은 [네트워크 DMZ 참조 아키텍처][dmz]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="d14a4-183">For more information, see [Network DMZ reference architecture][dmz].</span></span>

- <span data-ttu-id="d14a4-184">고가용성을 위해 두 개 이상의 NVA를 가용성 집합에 배치하고 외부 부하 분산 장치를 사용하여 인스턴스에 인터넷 요청을 분배합니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-184">For high availability, place two or more NVAs in an availability set, with an external load balancer to distribute Internet requests across the instances.</span></span> <span data-ttu-id="d14a4-185">자세한 내용은 [고가용성 네트워크 가상 어플라이언스 배포][ha-nva]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="d14a4-185">For more information, see [Deploy highly available network virtual appliances][ha-nva].</span></span>

- <span data-ttu-id="d14a4-186">애플리케이션 코드를 실행하는 VM에 대한 직접 RDP 또는 SSH 액세스를 허용하지 않도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-186">Do not allow direct RDP or SSH access to VMs that are running application code.</span></span> <span data-ttu-id="d14a4-187">대신, 운영자가 요새 호스트라고도 하는 Jumpbox에 로그인해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-187">Instead, operators should log into a jumpbox, also called a bastion host.</span></span> <span data-ttu-id="d14a4-188">이는 관리자가 다른 VM에 연결할 때 사용하는 네트워크의 VM입니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-188">This is a VM on the network that administrators use to connect to the other VMs.</span></span> <span data-ttu-id="d14a4-189">Jumpbox에는 승인된 공용 IP 주소의 RDP 또는 SSH만 허용하는 NSG가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-189">The jumpbox has an NSG that allows RDP or SSH only from approved public IP addresses.</span></span>

- <span data-ttu-id="d14a4-190">사이트 간 VPN(가상 사설망) 또는 Azure ExpressRoute를 사용하여 Azure 가상 네트워크를 온-프레미스 네트워크로 확장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-190">You can extend the Azure virtual network to your on-premises network using a site-to-site virtual private network (VPN) or Azure ExpressRoute.</span></span> <span data-ttu-id="d14a4-191">자세한 내용은 [하이브리드 네트워크 참조 아키텍처][hybrid-network]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="d14a4-191">For more information, see [Hybrid network reference architecture][hybrid-network].</span></span>

- <span data-ttu-id="d14a4-192">조직에서 Active Directory를 사용하여 ID를 관리하는 경우 Active Directory 환경을 Azure VNet으로 확장하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-192">If your organization uses Active Directory to manage identity, you may want to extend your Active Directory environment to the Azure VNet.</span></span> <span data-ttu-id="d14a4-193">자세한 내용은 [ID 관리 참조 아키텍처][identity]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="d14a4-193">For more information, see [Identity management reference architecture][identity].</span></span>

- <span data-ttu-id="d14a4-194">VM용 Azure SLA가 제공하는 가용성보다 높은 가용성이 필요한 경우, 두 지역 간에 애플리케이션을 복제한 다음, 장애 조치(failover)를 위해 Azure Traffic Manager를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="d14a4-194">If you need higher availability than the Azure SLA for VMs provides, replicate the application across two regions and use Azure Traffic Manager for failover.</span></span> <span data-ttu-id="d14a4-195">자세한 내용은 [여러 지역에서 Windows VM 실행][multiregion-windows] 또는 [여러 지역에서 Linux VM 실행][multiregion-linux]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="d14a4-195">For more information, see [Run Windows VMs in multiple regions][multiregion-windows] or [Run Linux VMs in multiple regions][multiregion-linux].</span></span>

[autoscaling]: ../../best-practices/auto-scaling.md
[caching]: ../../best-practices/caching.md
[dmz]: ../../reference-architectures/dmz/index.md
[ha-nva]: ../../reference-architectures/dmz/nva-ha.md
[hybrid-network]: ../../reference-architectures/hybrid-networking/index.md
[identity]: ../../reference-architectures/identity/index.md
[multiregion-linux]: ../../reference-architectures/virtual-machines-linux/multi-region-application.md
[multiregion-windows]: ../../reference-architectures/virtual-machines-windows/multi-region-application.md
[n-tier-linux]: ../../reference-architectures/virtual-machines-linux/n-tier.md
[n-tier-windows]: ../../reference-architectures/virtual-machines-windows/n-tier.md
[sql-always-on]: /sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server