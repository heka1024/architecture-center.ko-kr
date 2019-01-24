---
title: 데이터 레이크
description: ''
author: zoinerTejada
ms.date: 02/12/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.openlocfilehash: d433867886ce0afc219fcc9f35eb7f8b3ce6bee1
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54484706"
---
# <a name="data-lakes"></a><span data-ttu-id="00ba6-102">데이터 레이크</span><span class="sxs-lookup"><span data-stu-id="00ba6-102">Data lakes</span></span>

<span data-ttu-id="00ba6-103">데이터 레이크는 대량의 데이터를 네이티브, 원시 형식으로 보관하는 저장소 리포지토리입니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-103">A data lake is a storage repository that holds a large amount of data in its native, raw format.</span></span> <span data-ttu-id="00ba6-104">데이터 레이크 저장소는 테라바이트 및 페타바이트 규모의 데이터에 맞게 크기를 조정할 수 있도록 최적화되었습니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-104">Data lake stores are optimized for scaling to terabytes and petabytes of data.</span></span> <span data-ttu-id="00ba6-105">데이터는 일반적으로 여러 소스에서 오며 구조화, 반 조화 또는 구조화되지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-105">The data typically comes from multiple heterogeneous sources, and may be structured, semi-structured, or unstructured.</span></span> <span data-ttu-id="00ba6-106">모든 것을 변형되지 않은 원래 상태로 저장하는 것이 데이터 레이크의 개념입니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-106">The idea with a data lake is to store everything in its original, untransformed state.</span></span> <span data-ttu-id="00ba6-107">이 접근 방식은 데이터를 수집할 때 데이터를 변환하고 처리하는 기존의 [데이터 웨어하우스](../relational-data/data-warehousing.md)와는 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-107">This approach differs from a traditional [data warehouse](../relational-data/data-warehousing.md), which transforms and processes the data at the time of ingestion.</span></span>

<span data-ttu-id="00ba6-108">데이터 레이크의 장점은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-108">Advantages of a data lake:</span></span>

- <span data-ttu-id="00ba6-109">데이터가 원시 형식으로 저장되므로 절대 throw되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-109">Data is never thrown away, because the data is stored in its raw format.</span></span> <span data-ttu-id="00ba6-110">이러한 특징은 데이터에서 어떤 통찰력을 얻을 수 있을지 미리 알 수 없는 빅 데이터 환경에서 특히 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-110">This is especially useful in a big data environment, when you may not know in advance what insights are available from the data.</span></span>
- <span data-ttu-id="00ba6-111">사용자는 데이터를 탐색하고 직접 쿼리를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-111">Users can explore the data and create their own queries.</span></span>
- <span data-ttu-id="00ba6-112">기존의 ETL 도구보다 빠를 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-112">May be faster than traditional ETL tools.</span></span>
- <span data-ttu-id="00ba6-113">구조화되지 않은 데이터 및 반구조화된 데이터를 저장할 수 있으므로 데이터 웨어하우스보다 유연합니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-113">More flexible than a data warehouse, because it can store unstructured and semi-structured data.</span></span>

<span data-ttu-id="00ba6-114">완전한 데이터 레이크 솔루션은 저장과 처리로 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-114">A complete data lake solution consists of both storage and processing.</span></span> <span data-ttu-id="00ba6-115">Data Lake Storage는 내결함성, 무한 확장성, 다양한 모양과 크기의 데이터를 수집하는 높은 처리량이라는 목표를 중심으로 설계되었습니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-115">Data lake storage is designed for fault-tolerance, infinite scalability, and high-throughput ingestion of data with varying shapes and sizes.</span></span> <span data-ttu-id="00ba6-116">데이터 레이크 처리는 이러한 목표로 구축된 하나 이상의 처리 엔진을 사용하며, 데이터 레이크에 저장된 대규모 데이터에 작동될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-116">Data lake processing involves one or more processing engines built with these goals in mind, and can operate on data stored in a data lake at scale.</span></span>

## <a name="when-to-use-a-data-lake"></a><span data-ttu-id="00ba6-117">데이터 레이크를 사용하는 경우</span><span class="sxs-lookup"><span data-stu-id="00ba6-117">When to use a data lake</span></span>

<span data-ttu-id="00ba6-118">데이터 레이크는 일반적으로 [데이터 탐색](./interactive-data-exploration.md), 데이터 분석 및 기계 학습에 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-118">Typical uses for a data lake include [data exploration](./interactive-data-exploration.md), data analytics, and machine learning.</span></span>

<span data-ttu-id="00ba6-119">데이터 레이크를 데이터 웨어하우스의 데이터 원본으로 사용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-119">A data lake can also act as the data source for a data warehouse.</span></span> <span data-ttu-id="00ba6-120">이 방법에서는 원시 데이터가 데이터 레이크로 수집된 후 구조화된 쿼리 가능 형식으로 변환합니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-120">With this approach, the raw data is ingested into the data lake and then transformed into a structured queryable format.</span></span> <span data-ttu-id="00ba6-121">일반적으로 이 변환에서는 [ELT](../relational-data/etl.md#extract-load-and-transform-elt)(추출-부하-변형) 파이프라인을 사용하며, 여기서 데이터가 수집되고 변환됩니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-121">Typically this transformation uses an [ELT](../relational-data/etl.md#extract-load-and-transform-elt) (extract-load-transform) pipeline, where the data is ingested and transformed in place.</span></span> <span data-ttu-id="00ba6-122">이미 관계가 있는 원본 데이터는 ETL 프로세스를 사용하여 데이터 레이크를 건너뛰고 곧바로 데이터 웨어하우스로 이동할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-122">Source data that is already relational may go directly into the data warehouse, using an ETL process, skipping the data lake.</span></span>

<span data-ttu-id="00ba6-123">데이터 레이크 저장소는 변환 또는 스키마 정의 없이도 많은 양의 관계형 및 비관계형 데이터를 유지할 수 있으므로 이벤트 스트리밍 또는 IoT 시나리오에서 자주 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-123">Data lake stores are often used in event streaming or IoT scenarios, because they can persist large amounts of relational and nonrelational data without transformation or schema definition.</span></span> <span data-ttu-id="00ba6-124">이 저장소는 짧은 대기 시간으로 많은 양의 작은 쓰기를 처리하도록 고안되었으며 대규모 처리량에 최적화되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-124">They are built to handle high volumes of small writes at low latency, and are optimized for massive throughput.</span></span>

## <a name="challenges"></a><span data-ttu-id="00ba6-125">과제</span><span class="sxs-lookup"><span data-stu-id="00ba6-125">Challenges</span></span>

- <span data-ttu-id="00ba6-126">스키마 또는 설명 메타데이터가 없으면 데이터를 사용 또는 쿼리하기가 어려울 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-126">Lack of a schema or descriptive metadata can make the data hard to consume or query.</span></span>
- <span data-ttu-id="00ba6-127">데이터의 의미 체계가 일관적이지 않은 경우 데이터 분석에 숙련된 사용자가 아니면 데이터를 분석하기 어려울 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-127">Lack of semantic consistency across the data can make it challenging to perform analysis on the data, unless users are highly skilled at data analytics.</span></span>
- <span data-ttu-id="00ba6-128">데이터 레이크로 이동하는 데이터의 품질을 보장하기 어려울 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-128">It can be hard to guarantee the quality of the data going into the data lake.</span></span>
- <span data-ttu-id="00ba6-129">적절한 거버넌스가 없으면 액세스 제어 및 개인 정보 관련 문제가 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-129">Without proper governance, access control and privacy issues can be problems.</span></span> <span data-ttu-id="00ba6-130">데이터 레이크로 이동하는 정보의 종류, 데이터에 액세스할 수 있는 사람, 데이터의 용도는 어떻게 될까요?</span><span class="sxs-lookup"><span data-stu-id="00ba6-130">What information is going into the data lake, who can access that data, and for what uses?</span></span>
- <span data-ttu-id="00ba6-131">데이터 레이크는 이미 관계가 있는 데이터를 통합하기에 가장 좋은 방법이 아닐 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-131">A data lake may not be the best way to integrate data that is already relational.</span></span>
- <span data-ttu-id="00ba6-132">데이터 레이크 그 자체로는 조직 전체에서 사용되는 통합 보기 또는 전체 보기를 제공하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-132">By itself, a data lake does not provide integrated or holistic views across the organization.</span></span>
- <span data-ttu-id="00ba6-133">데이터 레이크는 실제로 분석 또는 마이닝할 일이 전혀 없는 데이터를 내다 버리는 폐기물 처리장이 될 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-133">A data lake may become a dumping ground for data that is never actually analyzed or mined for insights.</span></span>

## <a name="relevant-azure-services"></a><span data-ttu-id="00ba6-134">관련 Azure 서비스</span><span class="sxs-lookup"><span data-stu-id="00ba6-134">Relevant Azure services</span></span>

- <span data-ttu-id="00ba6-135">[Data Lake Store](/azure/data-lake-store/)는 하이퍼 스케일, Hadoop 호환 리포지토리입니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-135">[Data Lake Store](/azure/data-lake-store/) is a hyper-scale, Hadoop-compatible repository.</span></span>
- <span data-ttu-id="00ba6-136">[Data Lake Analytics](/azure/data-lake-analytics/)는 빅 데이터 분석을 간소화하는 주문형 분석 작업 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="00ba6-136">[Data Lake Analytics](/azure/data-lake-analytics/) is an on-demand analytics job service to simplify big data analytics.</span></span>
