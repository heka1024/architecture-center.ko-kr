---
title: Azure 애플리케이션 디자인 원칙
titleSuffix: Azure Application Architecture Guide
description: Azure 애플리케이션 디자인 원칙
author: MikeWasson
ms.date: 08/30/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: seojan19
ms.openlocfilehash: 8aab710ef6ffde493b80810750d2c0bc299ffaa6
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54485556"
---
# <a name="ten-design-principles-for-azure-applications"></a><span data-ttu-id="85841-103">Azure 애플리케이션을 위한 10가지 설계 원칙</span><span class="sxs-lookup"><span data-stu-id="85841-103">Ten design principles for Azure applications</span></span>

<span data-ttu-id="85841-104">애플리케이션의 확장성, 복원력 및 관리성을 높이려면 다음과 같은 디자인 원칙을 따르십시오.</span><span class="sxs-lookup"><span data-stu-id="85841-104">Follow these design principles to make your application more scalable, resilient, and manageable.</span></span>

<span data-ttu-id="85841-105">**[자체 복구를 위한 디자인](self-healing.md)**.</span><span class="sxs-lookup"><span data-stu-id="85841-105">**[Design for self healing](self-healing.md)**.</span></span> <span data-ttu-id="85841-106">분산 시스템에서는 오류가 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="85841-106">In a distributed system, failures happen.</span></span> <span data-ttu-id="85841-107">오류가 발생하면 자체 복구되도록 애플리케이션을 디자인하십시오.</span><span class="sxs-lookup"><span data-stu-id="85841-107">Design your application to be self healing when failures occur.</span></span>

<span data-ttu-id="85841-108">**[모두 중복으로 구성](redundancy.md)**.</span><span class="sxs-lookup"><span data-stu-id="85841-108">**[Make all things redundant](redundancy.md)**.</span></span> <span data-ttu-id="85841-109">단일 실패 지점을 피하도록 애플리케이션에 중복성을 구축합니다.</span><span class="sxs-lookup"><span data-stu-id="85841-109">Build redundancy into your application, to avoid having single points of failure.</span></span>

<span data-ttu-id="85841-110">**[조정 최소화](minimize-coordination.md)**.</span><span class="sxs-lookup"><span data-stu-id="85841-110">**[Minimize coordination](minimize-coordination.md)**.</span></span> <span data-ttu-id="85841-111">확장성을 위해 애플리케이션 서비스 간의 조정을 최소화합니다.</span><span class="sxs-lookup"><span data-stu-id="85841-111">Minimize coordination between application services to achieve scalability.</span></span>

<span data-ttu-id="85841-112">**[규모 확장을 위한 디자인](scale-out.md)**. 수요에 따라 새 인스턴스를 추가하거나 제거하여 규모 확장이 가능하도록 애플리케이션을 디자인합니다.</span><span class="sxs-lookup"><span data-stu-id="85841-112">**[Design to scale out](scale-out.md)**. Design your application so that it can scale horizontally, adding or removing new instances as demand requires.</span></span>

<span data-ttu-id="85841-113">**[한도에 맞춘 분할](partition.md)**.</span><span class="sxs-lookup"><span data-stu-id="85841-113">**[Partition around limits](partition.md)**.</span></span> <span data-ttu-id="85841-114">분할을 사용하여 데이터베이스, 네트워크 및 계산 한도를 해결합니다.</span><span class="sxs-lookup"><span data-stu-id="85841-114">Use partitioning to work around database, network, and compute limits.</span></span>

<span data-ttu-id="85841-115">**[운영을 위한 디자인](design-for-operations.md)**.</span><span class="sxs-lookup"><span data-stu-id="85841-115">**[Design for operations](design-for-operations.md)**.</span></span> <span data-ttu-id="85841-116">운영 팀에 필요한 도구가 포함되도록 애플리케이션을 디자인합니다.</span><span class="sxs-lookup"><span data-stu-id="85841-116">Design your application so that the operations team has the tools they need.</span></span>

<span data-ttu-id="85841-117">**[관리되는 서비스 사용](managed-services.md)**.</span><span class="sxs-lookup"><span data-stu-id="85841-117">**[Use managed services](managed-services.md)**.</span></span> <span data-ttu-id="85841-118">가능하면 IaaS(Infrastructure as a Service)보다 PaaS(Platform as a Service)를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="85841-118">When possible, use platform as a service (PaaS) rather than infrastructure as a service (IaaS).</span></span>

<span data-ttu-id="85841-119">**[작업에 가장 적합한 데이터 저장소 사용](use-the-best-data-store.md)**.</span><span class="sxs-lookup"><span data-stu-id="85841-119">**[Use the best data store for the job](use-the-best-data-store.md)**.</span></span> <span data-ttu-id="85841-120">데이터에 가장 적합한 저장소 기술과 사용 방법을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="85841-120">Pick the storage technology that is the best fit for your data and how it will be used.</span></span>

<span data-ttu-id="85841-121">**[진화를 위한 디자인](design-for-evolution.md)**.</span><span class="sxs-lookup"><span data-stu-id="85841-121">**[Design for evolution](design-for-evolution.md)**.</span></span> <span data-ttu-id="85841-122">모든 성공적인 애플리케이션은 시간에 따라 변화합니다.</span><span class="sxs-lookup"><span data-stu-id="85841-122">All successful applications change over time.</span></span> <span data-ttu-id="85841-123">혁신적인 디자인은 지속적인 혁신의 핵심입니다.</span><span class="sxs-lookup"><span data-stu-id="85841-123">An evolutionary design is key for continuous innovation.</span></span>

<span data-ttu-id="85841-124">**[비즈니스 요구 사항에 맞게 구축](build-for-business.md)**.</span><span class="sxs-lookup"><span data-stu-id="85841-124">**[Build for the needs of business](build-for-business.md)**.</span></span> <span data-ttu-id="85841-125">모든 디자인 결정은 비즈니스 요구 사항에 맞춰 정당화되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="85841-125">Every design decision must be justified by a business requirement.</span></span>
