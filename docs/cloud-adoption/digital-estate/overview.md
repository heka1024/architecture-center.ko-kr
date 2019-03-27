---
title: 'CAF: 디지털 자산이란?'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 디지털 자산이란?
author: BrianBlanchard
ms.date: 12/10/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.openlocfilehash: d23bdbdd98226e8b9cdb9bbb5fefa5a918a498d8
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58241654"
---
<!-- markdownlint-disable MD026 -->

# <a name="caf-what-is-a-digital-estate"></a><span data-ttu-id="91119-103">CAF: 디지털 자산이란?</span><span class="sxs-lookup"><span data-stu-id="91119-103">CAF: What is a digital estate?</span></span>

<span data-ttu-id="91119-104">오늘날 모든 회사에는 특정 형태의 디지털 자산이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91119-104">Every modern company has some form of digital estate.</span></span> <span data-ttu-id="91119-105">물리적 자산과 마찬가지로, 디지털 자산은 여러 실체적인 소유 자산을 가리키는 추상적인 용어입니다.</span><span class="sxs-lookup"><span data-stu-id="91119-105">Much like a physical estate, a digital estate is an abstract reference to a number of tangible, owned assets.</span></span> <span data-ttu-id="91119-106">디지털 자산에서 이러한 자산은 VM(가상 머신), 서버, 애플리케이션, 데이터 등으로 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="91119-106">In a digital estate, those assets are comprised of virtual machines (VMs), servers, applications, data, and so on.</span></span> <span data-ttu-id="91119-107">기본적으로, 디지털 자산은 비즈니스 프로세스 및 지원 작업을 지원하는 IT 자산의 컬렉션입니다.</span><span class="sxs-lookup"><span data-stu-id="91119-107">Essentially, a digital estate is the collection of IT assets that power business processes and supporting operations.</span></span>

<span data-ttu-id="91119-108">디지털 자산의 중요성은 디지털 전환 활동의 계획과 실행 단계에서 매우 명백히 드러납니다.</span><span class="sxs-lookup"><span data-stu-id="91119-108">The importance of a digital estate is most obvious during planning and execution of Digital Transformation efforts.</span></span> <span data-ttu-id="91119-109">전환 과정 중에 클라우드 전략 팀은 디지털 자산을 사용하여 릴리스 계획과 기술 활동에 비즈니스 결과를 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="91119-109">During transformation journeys, the digital estate is how Cloud Strategy teams map the business outcomes to release plans and technical efforts.</span></span> <span data-ttu-id="91119-110">이는 모두 조직이 현재 소유한 디지털 자산의 인벤토리와 측정으로 시작됩니다.</span><span class="sxs-lookup"><span data-stu-id="91119-110">That all starts with an inventory and measurement of the digital assets the organization owns today.</span></span>

## <a name="how-can-a-digital-estate-be-measured"></a><span data-ttu-id="91119-111">디지털 자산을 측정하는 방법은?</span><span class="sxs-lookup"><span data-stu-id="91119-111">How can a digital estate be measured?</span></span>

<span data-ttu-id="91119-112">디지털 자산의 측정값은 원하는 비즈니스 결과에 따라 바뀝니다.</span><span class="sxs-lookup"><span data-stu-id="91119-112">The measurement of a digital estate changes depending on the desired business outcomes.</span></span>

- <span data-ttu-id="91119-113">**인프라 마이그레이션**.</span><span class="sxs-lookup"><span data-stu-id="91119-113">**Infrastructure migrations**.</span></span> <span data-ttu-id="91119-114">조직이 내부 지향형이고 비용, 운영 프로세스, 민첩성 또는 운영 최적화의 다른 요소를 최적화하려는 경우 디지털 자산은 VM, 서버, 지원 워크로드에 중점을 둡니다.</span><span class="sxs-lookup"><span data-stu-id="91119-114">When an organization is inward facing and seeks to optimize cost, operational processes, agility, or other aspects of optimizing operations, the digital estate focuses on VMs, servers, and supporting workloads.</span></span>

- <span data-ttu-id="91119-115">**애플리케이션 혁신**.</span><span class="sxs-lookup"><span data-stu-id="91119-115">**Application innovation**.</span></span> <span data-ttu-id="91119-116">고객에 집착하는 전환의 경우에는 관점이 약간 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="91119-116">For customer-obsessed transformations, the lens is a bit different.</span></span> <span data-ttu-id="91119-117">고객을 지원하는 애플리케이션, API, 트랜잭션 데이터에 중점을 두어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="91119-117">The focus should be placed in the applications, APIs, and transactional data that support the customers.</span></span> <span data-ttu-id="91119-118">VM과 네트워크 어플라이언스에는 덜 중점을 두는 경우가 많습니다.</span><span class="sxs-lookup"><span data-stu-id="91119-118">VMs and network appliances are often of less focus.</span></span>

- <span data-ttu-id="91119-119">**데이터 중심 혁신**.</span><span class="sxs-lookup"><span data-stu-id="91119-119">**Data-driven innovation**.</span></span> <span data-ttu-id="91119-120">오늘날과 같은 디지털 중심 시장에서는 데이터에 튼튼한 토대 없이 새로운 제품이나 서비스를 출시하기가 어렵습니다.</span><span class="sxs-lookup"><span data-stu-id="91119-120">In today's digitally driven market, it's difficult to launch a new product or service without a strong foundation in data.</span></span> <span data-ttu-id="91119-121">파괴적 전환 시에는 조직 전체의 데이터 사일로에 좀 더 중점을 둡니다.</span><span class="sxs-lookup"><span data-stu-id="91119-121">During disruptive transformation, the focus is more on the silos of data across the organization.</span></span>

<span data-ttu-id="91119-122">조직이 가장 중요한 전환 형태를 이해하게 되면 디지털 자산 계획을 관리하기가 훨씬 더 쉬워집니다.</span><span class="sxs-lookup"><span data-stu-id="91119-122">Once an organization understands the most important form of transformation, digital estate planning becomes much easier to manage.</span></span>

> [!TIP]
> <span data-ttu-id="91119-123">각 전환 유형은 세 가지 관점 중 하나로 측정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91119-123">Each type of transformation could be measured with any of the three views.</span></span> <span data-ttu-id="91119-124">또한 회사에서 모든 세 전환을 동시에 실행하는 것은 일반적입니다.</span><span class="sxs-lookup"><span data-stu-id="91119-124">Further, it is common for companies to execute all three transformations in parallel.</span></span> <span data-ttu-id="91119-125">경영진과 클라우드 전략 팀이 비즈니스 성공에 가장 중요한 전환과 관련하여 견고한 공동 전선을 유지할 것을 적극 권장합니다.</span><span class="sxs-lookup"><span data-stu-id="91119-125">It is highly suggested that the leadership and Cloud Strategy team be firmly aligned regarding the transformation that is most important for business success.</span></span> <span data-ttu-id="91119-126">이러한 이해는 여러 이니셔티브 간에 공통적인 언어와 메트릭의 기준 역할을 할 것입니다.</span><span class="sxs-lookup"><span data-stu-id="91119-126">That understanding will serve as the basis for common language and metrics across multiple initiatives.</span></span>

## <a name="how-can-a-financial-model-be-updated-to-reflect-the-digital-estate"></a><span data-ttu-id="91119-127">디지털 자산을 반영하도록 금융 모델을 업데이트하는 방법은?</span><span class="sxs-lookup"><span data-stu-id="91119-127">How can a financial model be updated to reflect the digital estate?</span></span>

<span data-ttu-id="91119-128">디지털 자산 분석은 클라우드 도입 활동을 유도합니다.</span><span class="sxs-lookup"><span data-stu-id="91119-128">An analysis of the digital estate will drive cloud adoption activities.</span></span> <span data-ttu-id="91119-129">또한 클라우드 비용 모델을 제공하여 투자 수익률(ROI)을 창출함으로써 금융 모델을 알려줍니다.</span><span class="sxs-lookup"><span data-stu-id="91119-129">It will also inform financial models by providing cloud costing models, which will in turn drive the return on investment (ROI).</span></span>

<span data-ttu-id="91119-130">디지털 자산 분석을 완료하려면 다음 단계를 수행하세요.</span><span class="sxs-lookup"><span data-stu-id="91119-130">To complete the digital estate analysis, perform the following steps:</span></span>

1. [<span data-ttu-id="91119-131">분석 방법 결정</span><span class="sxs-lookup"><span data-stu-id="91119-131">Determine analysis approach</span></span>](approach.md)
1. [<span data-ttu-id="91119-132">현재 상태 인벤토리 수집</span><span class="sxs-lookup"><span data-stu-id="91119-132">Collect current state inventory</span></span>](inventory.md)
1. [<span data-ttu-id="91119-133">디지털 자산의 자산 합리화</span><span class="sxs-lookup"><span data-stu-id="91119-133">Rationalize the assets in the digital estate</span></span>](rationalize.md)
1. [<span data-ttu-id="91119-134">클라우드 솔루션에 자산을 맞춰 가격 계산</span><span class="sxs-lookup"><span data-stu-id="91119-134">Align assets to cloud offerings to calculate pricing</span></span>](calculate.md)

<span data-ttu-id="91119-135">합리화되고 가격이 책정된 자산을 반영하도록 금융 모델 및 마이그레이션 백로그를 수정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91119-135">Financial models and migration backlogs can be modified to reflect the rationalized and priced estate.</span></span>

## <a name="next-steps"></a><span data-ttu-id="91119-136">다음 단계</span><span class="sxs-lookup"><span data-stu-id="91119-136">Next steps</span></span>

<span data-ttu-id="91119-137">디지털 자산 계획을 시작하기 전에 어떤 접근 방식을 사용할지 결정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="91119-137">Before digital estate planning can begin, you must determine what approach to use.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="91119-138">디지털 자산 계획에 대한 접근 방법</span><span class="sxs-lookup"><span data-stu-id="91119-138">Approaches to digital estate planning</span></span>](approach.md)