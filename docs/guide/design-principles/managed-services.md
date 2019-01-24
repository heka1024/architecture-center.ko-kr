---
title: 관리되는 서비스 사용
titleSuffix: Azure Application Architecture Guide
description: 가능하면 IaaS(Infrastructure as a Service)보다 PaaS(Platform as a Service)를 사용합니다.
author: MikeWasson
ms.date: 08/30/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: seojan19
ms.openlocfilehash: f08914e1eacc4f02fdb16093fe590f46249e3da3
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54485811"
---
# <a name="use-managed-services"></a><span data-ttu-id="160f2-103">관리되는 서비스 사용</span><span class="sxs-lookup"><span data-stu-id="160f2-103">Use managed services</span></span>

## <a name="when-possible-use-platform-as-a-service-paas-rather-than-infrastructure-as-a-service-iaas"></a><span data-ttu-id="160f2-104">가능하면 IaaS(Infrastructure as a Service)보다 PaaS(Platform as a Service)를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="160f2-104">When possible, use platform as a service (PaaS) rather than infrastructure as a service (IaaS)</span></span>

<span data-ttu-id="160f2-105">IaaS는 부품 상자를 사용하는 것과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="160f2-105">IaaS is like having a box of parts.</span></span> <span data-ttu-id="160f2-106">어떤 것도 작성할 수 있지만 직접 조립해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="160f2-106">You can build anything, but you have to assemble it yourself.</span></span> <span data-ttu-id="160f2-107">관리되는 서비스를 보다 쉽게 구성 및 관리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="160f2-107">Managed services are easier to configure and administer.</span></span> <span data-ttu-id="160f2-108">VM을 프로비전하거나, Vnet을 설정하거나, 패치 및 업데이트와 VM에서 소프트웨어를 실행하는 것과 관련된 기타 모든 오버헤드를 관리할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="160f2-108">You don't need to provision VMs, set up VNets, manage patches and updates, and all of the other overhead associated with running software on a VM.</span></span>

<span data-ttu-id="160f2-109">예를 들어, 애플리케이션에 메시지 큐가 필요하다고 가정해보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="160f2-109">For example, suppose your application needs a message queue.</span></span> <span data-ttu-id="160f2-110">RabbitMQ 등을 사용하여 VM에서 자체 메시징 서비스를 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="160f2-110">You could set up your own messaging service on a VM, using something like RabbitMQ.</span></span> <span data-ttu-id="160f2-111">그렇지만 Azure Service Bus는 신뢰할 수 있는 메시징을 이미 서비스로 제공하고 있으므로 더 간단하게 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="160f2-111">But Azure Service Bus already provides reliable messaging as service, and it's simpler to set up.</span></span> <span data-ttu-id="160f2-112">Service Bus 네임 스페이스를 만든 다음(배포 스크립트의 일부로 수행할 수 있음) 클라이언트 SDK를 사용하여 Service Bus를 호출합니다.</span><span class="sxs-lookup"><span data-stu-id="160f2-112">Just create a Service Bus namespace (which can be done as part of a deployment script) and then call Service Bus using the client SDK.</span></span>

<span data-ttu-id="160f2-113">물론, 애플리케이션에 IaaS 방식이 더 적합할 수 밖에 없는 특정 요구 사항이 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="160f2-113">Of course, your application may have specific requirements that make an IaaS approach more suitable.</span></span> <span data-ttu-id="160f2-114">그러나 애플리케이션이 IaaS를 기준으로 하더라도 관리 서비스를 통합하는 것이 적절한 경우를 고려해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="160f2-114">However, even if your application is based on IaaS, look for places where it may be natural to incorporate managed services.</span></span> <span data-ttu-id="160f2-115">여기에는 캐시, 큐 및 데이터 저장소가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="160f2-115">These include cache, queues, and data storage.</span></span>

| <span data-ttu-id="160f2-116">기존 실행 서비스...</span><span class="sxs-lookup"><span data-stu-id="160f2-116">Instead of running...</span></span> | <span data-ttu-id="160f2-117">새 서비스...</span><span class="sxs-lookup"><span data-stu-id="160f2-117">Consider using...</span></span> |
|-----------------------|-------------|
| <span data-ttu-id="160f2-118">Active Directory</span><span class="sxs-lookup"><span data-stu-id="160f2-118">Active Directory</span></span> | <span data-ttu-id="160f2-119">Azure Active Directory Domain Services</span><span class="sxs-lookup"><span data-stu-id="160f2-119">Azure Active Directory Domain Services</span></span> |
| <span data-ttu-id="160f2-120">Elasticsearch</span><span class="sxs-lookup"><span data-stu-id="160f2-120">Elasticsearch</span></span> | <span data-ttu-id="160f2-121">Azure Search</span><span class="sxs-lookup"><span data-stu-id="160f2-121">Azure Search</span></span> |
| <span data-ttu-id="160f2-122">Hadoop은</span><span class="sxs-lookup"><span data-stu-id="160f2-122">Hadoop</span></span> | <span data-ttu-id="160f2-123">HDInsight</span><span class="sxs-lookup"><span data-stu-id="160f2-123">HDInsight</span></span> |
| <span data-ttu-id="160f2-124">IIS</span><span class="sxs-lookup"><span data-stu-id="160f2-124">IIS</span></span> | <span data-ttu-id="160f2-125">App Service</span><span class="sxs-lookup"><span data-stu-id="160f2-125">App Service</span></span> |
| <span data-ttu-id="160f2-126">MongoDB</span><span class="sxs-lookup"><span data-stu-id="160f2-126">MongoDB</span></span> | <span data-ttu-id="160f2-127">Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="160f2-127">Cosmos DB</span></span> |
| <span data-ttu-id="160f2-128">Redis</span><span class="sxs-lookup"><span data-stu-id="160f2-128">Redis</span></span> | <span data-ttu-id="160f2-129">Azure Redis 캐시(영문)</span><span class="sxs-lookup"><span data-stu-id="160f2-129">Azure Redis Cache</span></span> |
| <span data-ttu-id="160f2-130">SQL Server</span><span class="sxs-lookup"><span data-stu-id="160f2-130">SQL Server</span></span> | <span data-ttu-id="160f2-131">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="160f2-131">Azure SQL Database</span></span> |
