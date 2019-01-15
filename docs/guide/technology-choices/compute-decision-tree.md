---
title: Azure 계산 서비스에 대한 의사 결정 트리
titleSuffix: Azure Application Architecture Guide
description: 컴퓨팅 서비스를 선택하기 위한 순서도
author: MikeWasson
ms.date: 11/03/2018
ms.custom: seojan19
ms.openlocfilehash: 905b9956c9dcddddb21a87ea588af0ad5160ae2a
ms.sourcegitcommit: 1f4cdb08fe73b1956e164ad692f792f9f635b409
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54114082"
---
# <a name="decision-tree-for-azure-compute-services"></a><span data-ttu-id="a4b5c-103">Azure 계산 서비스에 대한 의사 결정 트리</span><span class="sxs-lookup"><span data-stu-id="a4b5c-103">Decision tree for Azure compute services</span></span>

<span data-ttu-id="a4b5c-104">Azure는 애플리케이션 코드를 호스트하는 다양한 방법을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="a4b5c-104">Azure offers a number of ways to host your application code.</span></span> <span data-ttu-id="a4b5c-105">*계산*이라는 용어는 애플리케이션이 실행되는 계산 리소스의 호스팅 모델을 말합니다.</span><span class="sxs-lookup"><span data-stu-id="a4b5c-105">The term *compute* refers to the hosting model for the computing resources that your application runs on.</span></span> <span data-ttu-id="a4b5c-106">다음 순서도는 애플리케이션에 대한 계산 서비스를 선택하는 데 도움이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a4b5c-106">The following flowchart will help you to choose a compute service for your application.</span></span> <span data-ttu-id="a4b5c-107">순서도는 권장 사항에 연결할 주요 의사 결정 기준의 집합을 안내합니다.</span><span class="sxs-lookup"><span data-stu-id="a4b5c-107">The flowchart guides you through a set of key decision criteria to reach a recommendation.</span></span>

<span data-ttu-id="a4b5c-108">**이 순서도를 시작점으로 처리합니다.**</span><span class="sxs-lookup"><span data-stu-id="a4b5c-108">**Treat this flowchart as a starting point.**</span></span> <span data-ttu-id="a4b5c-109">모든 애플리케이션에는 고유한 요구 사항이 있으므로 권장 사항을 시작점으로 처리합니다.</span><span class="sxs-lookup"><span data-stu-id="a4b5c-109">Every application has unique requirements, so use the recommendation as a starting point.</span></span> <span data-ttu-id="a4b5c-110">그런 다음, 다음과 같은 측면을 살펴보는 보다 자세한 평가를 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="a4b5c-110">Then perform a more detailed evaluation, looking at aspects such as:</span></span>

- <span data-ttu-id="a4b5c-111">기능 집합</span><span class="sxs-lookup"><span data-stu-id="a4b5c-111">Feature set</span></span>
- [<span data-ttu-id="a4b5c-112">서비스 한도</span><span class="sxs-lookup"><span data-stu-id="a4b5c-112">Service limits</span></span>](/azure/azure-subscription-service-limits)
- [<span data-ttu-id="a4b5c-113">비용</span><span class="sxs-lookup"><span data-stu-id="a4b5c-113">Cost</span></span>](https://azure.microsoft.com/pricing/)
- [<span data-ttu-id="a4b5c-114">SLA</span><span class="sxs-lookup"><span data-stu-id="a4b5c-114">SLA</span></span>](https://azure.microsoft.com/support/legal/sla/)
- [<span data-ttu-id="a4b5c-115">국가별 가용성</span><span class="sxs-lookup"><span data-stu-id="a4b5c-115">Regional availability</span></span>](https://azure.microsoft.com/global-infrastructure/services/)
- <span data-ttu-id="a4b5c-116">개발자 에코시스템 및 팀 기술</span><span class="sxs-lookup"><span data-stu-id="a4b5c-116">Developer ecosystem and team skills</span></span>
- [<span data-ttu-id="a4b5c-117">계산 비교 표</span><span class="sxs-lookup"><span data-stu-id="a4b5c-117">Compute comparison tables</span></span>](./compute-comparison.md)

<span data-ttu-id="a4b5c-118">애플리케이션이 여러 워크로드로 구성된 경우 각 워크로드를 개별적으로 평가합니다.</span><span class="sxs-lookup"><span data-stu-id="a4b5c-118">If your application consists of multiple workloads, evaluate each workload separately.</span></span> <span data-ttu-id="a4b5c-119">완벽한 솔루션은 두 개 이상의 계산 서비스를 통합할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a4b5c-119">A complete solution may incorporate two or more compute services.</span></span>

<span data-ttu-id="a4b5c-120">Azure에서 컨테이너 호스팅을 위한 옵션에 대한 자세한 내용은 [Azure 컨테이너](https://azure.microsoft.com/overview/containers/)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="a4b5c-120">For more information about your options for hosting containers in Azure, see [Azure Containers](https://azure.microsoft.com/overview/containers/).</span></span>

## <a name="flowchart"></a><span data-ttu-id="a4b5c-121">순서도</span><span class="sxs-lookup"><span data-stu-id="a4b5c-121">Flowchart</span></span>

![Azure 계산 서비스에 대한 의사 결정 트리](../images/compute-decision-tree.svg)

## <a name="definitions"></a><span data-ttu-id="a4b5c-123">정의</span><span class="sxs-lookup"><span data-stu-id="a4b5c-123">Definitions</span></span>

- <span data-ttu-id="a4b5c-124">**리프트 앤 시프트**는 애플리케이션을 다시 디자인하거나 코드를 변경하지 않고 워크로드를 클라우드로 마이그레이션하는 전략입니다.</span><span class="sxs-lookup"><span data-stu-id="a4b5c-124">**Lift and shift** is a strategy for migrating a workload to the cloud without redesigning the application or making code changes.</span></span> <span data-ttu-id="a4b5c-125">*재 호스팅*이라고도 합니다.</span><span class="sxs-lookup"><span data-stu-id="a4b5c-125">Also called *rehosting*.</span></span> <span data-ttu-id="a4b5c-126">자세한 내용은 [Azure 마이그레이션 센터](https://azure.microsoft.com/migration/)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="a4b5c-126">For more information, see [Azure migration center](https://azure.microsoft.com/migration/).</span></span>

- <span data-ttu-id="a4b5c-127">**클라우드 최적화**는 클라우드 고유 기능 및 성능을 활용하도록 애플리케이션을 리팩터링하여 클라우드로 마이그레이션하는 전략입니다.</span><span class="sxs-lookup"><span data-stu-id="a4b5c-127">**Cloud optimized** is a strategy for migrating to the cloud by refactoring an application to take advantage of cloud-native features and capabilities.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a4b5c-128">다음 단계</span><span class="sxs-lookup"><span data-stu-id="a4b5c-128">Next steps</span></span>

<span data-ttu-id="a4b5c-129">고려해야 할 추가 기준은 [Azure 계산 서비스를 선택하기 위한 조건](./compute-comparison.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="a4b5c-129">For additional criteria to consider, see [Criteria for choosing an Azure compute service](./compute-comparison.md).</span></span>
