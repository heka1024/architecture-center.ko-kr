---
title: Azure에서 뉴스 피드 대량 수집 및 분석
description: Azure Cosmos DB 및 Azure Cognitive Services를 포함한 Azure 서비스만을 사용하여 RSS 뉴스 피드에서 텍스트, 이미지, 감정 및 다른 데이터를 수집하고 분석하는 파이프라인을 만듭니다.
author: njray
ms.date: 2/1/2019
ms.custom: azcat-ai, AI
social_image_url: /azure/architecture/example-scenario/ai/media/mass-ingestion-newsfeeds-architecture.png
ms.topic: example-scenario
ms.service: architecture-center
ms.subservice: example-scenario
ms.openlocfilehash: e1bc636103753d474b545a6e3e9118b2ef302b34
ms.sourcegitcommit: ea97ac004c38c6b456794c1a8eef29f8d2b77d50
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58489267"
---
# <a name="mass-ingestion-and-analysis-of-news-feeds-on-azure"></a><span data-ttu-id="90565-103">Azure에서 뉴스 피드 대량 수집 및 분석</span><span class="sxs-lookup"><span data-stu-id="90565-103">Mass ingestion and analysis of news feeds on Azure</span></span>

<span data-ttu-id="90565-104">이 예제 시나리오는 거의 실시간으로 분석할 공용 RSS 뉴스 피드를 사용 하 여 문서 및 대량 수집에 대 한 파이프라인을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-104">This example scenario describes a pipeline for mass ingestion and near real-time analysis of documents using public RSS news feeds.</span></span>  <span data-ttu-id="90565-105">Azure Cognitive Services를 사용 하 여 텍스트 번역, 얼굴 인식 및 감정 검색을 포함 하 여 유용한 정보를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-105">It uses Azure Cognitive Services to offer useful insights including text translation, facial recognition, and sentiment detection.</span></span>

<span data-ttu-id="90565-106">이 시나리오에 대 한 예제가 [영어][english]합니다 [러시아어][russian], 및 [독일어] [ german] 뉴스 피드, 있지만 쉽게 확장할 수 다른 RSS 피드입니다.</span><span class="sxs-lookup"><span data-stu-id="90565-106">This scenario contains examples for [English][english], [Russian][russian], and [German][german] news feeds, but you can easily extend it to other RSS feeds.</span></span> <span data-ttu-id="90565-107">쉽게 배포 하기에 대 한 데이터 수집, 처리 및 분석 전적으로 Azure 서비스에 기반한 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-107">For ease of deployment, the data collection, processing, and analysis are based entirely on Azure services.</span></span>

## <a name="relevant-use-cases"></a><span data-ttu-id="90565-108">관련 사용 사례</span><span class="sxs-lookup"><span data-stu-id="90565-108">Relevant use cases</span></span>

<span data-ttu-id="90565-109">이 시나리오를 처리 RSS 피드를 기반으로 하는 동안에 문서, 웹 사이트 또는 문서에는 필요한 모든 관련 된:</span><span class="sxs-lookup"><span data-stu-id="90565-109">While this scenario is based on processing of RSS feeds, it's relevant to any document, website, or article where you would need to:</span></span>

* <span data-ttu-id="90565-110">선택한 언어로 텍스트를 변환 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-110">Translate any text to the language of choice.</span></span>
* <span data-ttu-id="90565-111">디지털 콘텐츠에서 키 구, 엔터티 및 사용자 데이터를 찾을.</span><span class="sxs-lookup"><span data-stu-id="90565-111">Find key phrases, entities, and user sentiment in digital content.</span></span>
* <span data-ttu-id="90565-112">디지털 문서와 연결 된 이미지에서 개체, 텍스트 및 랜드마크를 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-112">Detect objects, text, and landmarks in images associated with a digital article.</span></span>
* <span data-ttu-id="90565-113">디지털 콘텐츠를 사용 하 여 연결 된 모든 이미지에 해당 성별 및 연령으로 사용자를 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-113">Detect people by their gender and age in any image associated with digital content.</span></span>

## <a name="architecture"></a><span data-ttu-id="90565-114">아키텍처</span><span class="sxs-lookup"><span data-stu-id="90565-114">Architecture</span></span>

![][architecture]

<span data-ttu-id="90565-115">솔루션을 통한 데이터 흐름은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="90565-115">The data flows through the solution as follows:</span></span>

1. <span data-ttu-id="90565-116">RSS 뉴스 피드는 문서 또는 문서에서 데이터를 가져오는 생성자 처럼 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-116">An RSS news feed acts as the generator that obtains data from a document or article.</span></span> <span data-ttu-id="90565-117">예를 들어 아티클을 사용 하 여 데이터 일반적으로 제목, 뉴스 항목의 원래 본문의 요약이 포함 됩니다 및 경우에 따라 이미지입니다.</span><span class="sxs-lookup"><span data-stu-id="90565-117">For example, with an article, data typically includes a title, a summary of the original body of the news item, and sometimes images.</span></span>

2. <span data-ttu-id="90565-118">Azure Cosmos DB에 문서와 연결 된 모든 이미지를 삽입 하는 생성기 또는 수집 프로세스 [컬렉션][collection]합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-118">A generator or ingestion process inserts the article and any associated images into an Azure Cosmos DB [Collection][collection].</span></span>

3. <span data-ttu-id="90565-119">알림을은 CosmosDB 및 Azure Blob Storage에서 문서 이미지 (있는 경우)의 문서 텍스트를 저장 하는 Azure Functions에서 수집 함수를 트리거합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-119">A notification triggers an ingest function in Azure Functions that stores the article text in CosmosDB and the article images (if any) in Azure Blob Storage.</span></span>  <span data-ttu-id="90565-120">그런 다음 문서는 다음 큐로 전달 됩니다.</span><span class="sxs-lookup"><span data-stu-id="90565-120">The article is then passed to the next queue.</span></span>

4. <span data-ttu-id="90565-121">변환 함수는 큐 이벤트에 의해 트리거됩니다.</span><span class="sxs-lookup"><span data-stu-id="90565-121">A translate function is triggered by the queue event.</span></span> <span data-ttu-id="90565-122">사용 된 [텍스트 변환 API] [ translate-text] 언어를 검색 하도록 Azure Cognitive Services의 필요한 경우 변환 하 고 제목 및 본문에서 감정, 핵심 문구 및 엔터티를 수집 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-122">It uses the [Translate Text API][translate-text] of Azure Cognitive Services to detect the language, translate if necessary, and collect the sentiment, key phrases, and entities from the body and the title.</span></span> <span data-ttu-id="90565-123">그러면 다음 큐로 문서를 전달합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-123">Then it passes the article to the next queue.</span></span>

5. <span data-ttu-id="90565-124">검색 함수는 큐에 대기 중인된 문서에서 트리거됩니다.</span><span class="sxs-lookup"><span data-stu-id="90565-124">A detect function is triggered from the queued article.</span></span> <span data-ttu-id="90565-125">사용 된 [Computer Vision] [ vision] 개체, 랜드마크, 및 연결된 된 이미지에서 작성 된 단어 다음 문서를 전달 다음 큐를 검색 하는 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="90565-125">It uses the [Computer Vision][vision] service to detect objects, landmarks, and written words in the associated image, then passes the article to the next queue.</span></span>

6. <span data-ttu-id="90565-126">얼굴 함수가 트리거될 큐에 대기 중인된 문서에서 트리거됩니다.</span><span class="sxs-lookup"><span data-stu-id="90565-126">A face function is triggered is triggered from the queued article.</span></span> <span data-ttu-id="90565-127">사용 된 [Azure Face API] [ face] 검색할 서비스 성별 및 연령에 연결된 된 이미지에 대 한 얼굴을 다음 큐로 문서를 전달 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-127">It uses the [Azure Face API][face] service to detect faces for gender and age in the associated image, then passes the article to the next queue.</span></span>

7. <span data-ttu-id="90565-128">모든 함수가 완료 되 면 알림 함수가 트리거될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="90565-128">When all functions are complete, the notify function is triggered.</span></span> <span data-ttu-id="90565-129">아티클에 대 한 처리 된 레코드를 로드 하 고 모든 결과 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-129">It loads the processed records for the article and scans them for any results you want.</span></span> <span data-ttu-id="90565-130">발견 내용을 플래그가 지정 된 경우 사용자가 선택한 시스템에는 알림이 전송 됩니다.</span><span class="sxs-lookup"><span data-stu-id="90565-130">If found, the content is flagged and a notification is sent to the system of your choice.</span></span>

<span data-ttu-id="90565-131">함수는 각 처리 단계에서 Azure Cosmos DB에 결과 씁니다.</span><span class="sxs-lookup"><span data-stu-id="90565-131">At each processing step, the function writes the results to Azure Cosmos DB.</span></span> <span data-ttu-id="90565-132">궁극적으로 데이터를 사용할 수 있습니다 원하는대로 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-132">Ultimately, the data can be used as desired.</span></span> <span data-ttu-id="90565-133">예를 들어, 비즈니스 프로세스를 개선, 새 고객을 찾거나, 고객 만족 문제를 식별 하려면 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="90565-133">For example, you can use it to enhance business processes, locate new customers, or identify customer satisfaction issues.</span></span>

### <a name="components"></a><span data-ttu-id="90565-134">구성 요소</span><span class="sxs-lookup"><span data-stu-id="90565-134">Components</span></span>

<span data-ttu-id="90565-135">다음 Azure 구성 요소 목록은이 예제에서 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="90565-135">The following list of Azure components is used in this example.</span></span>

* <span data-ttu-id="90565-136">[Azure Storage] [ storage] 원시 이미지 및 문서와 연결 된 비디오 파일을 저장 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="90565-136">[Azure Storage][storage] is used to hold raw image and video files associated with an article.</span></span> <span data-ttu-id="90565-137">보조 저장소 계정을 Azure App Service를 사용 하 여 만들어지고 Azure Function 코드 및 로그를 호스트 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="90565-137">A secondary storage account is created with Azure App Service and is used to host the Azure Function code and logs.</span></span>

* <span data-ttu-id="90565-138">[Azure Cosmos DB] [ cosmos-db] 포함 문서 텍스트, 이미지 및 비디오 정보를 추적 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-138">[Azure Cosmos DB][cosmos-db] holds article text, image, and video tracking information.</span></span> <span data-ttu-id="90565-139">Cognitive Services 단계의 결과가 여기에 저장 됩니다.</span><span class="sxs-lookup"><span data-stu-id="90565-139">The results of the Cognitive Services steps are also stored here.</span></span>

* <span data-ttu-id="90565-140">[Azure Functions] [ functions] 큐 메시지에 응답 하 고 들어오는 콘텐츠를 변환 하는 데 사용 하는 함수 코드를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-140">[Azure Functions][functions] executes the function code used to respond to queue messages and transform the incoming content.</span></span> <span data-ttu-id="90565-141">[Azure App Service] [ aas] 함수 코드를 호스트 하 고 레코드를 순차적으로 처리 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-141">[Azure App Service][aas] hosts the function code and processes the records serially.</span></span> <span data-ttu-id="90565-142">이 시나리오는 다섯 가지 함수를 포함 합니다. 수집, 변환, 얼굴, 개체 감지 및 알림.</span><span class="sxs-lookup"><span data-stu-id="90565-142">This scenario includes five functions: Ingest, Transform, Detect Object, Face, and Notify.</span></span>

* <span data-ttu-id="90565-143">[Azure Service Bus] [ service-bus] 함수가 사용 하는 Azure Service Bus 큐를 호스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-143">[Azure Service Bus][service-bus] hosts the Azure Service Bus queues used by the functions.</span></span>

* <span data-ttu-id="90565-144">[Azure Cognitive Services] [ acs] 의 구현을 기반으로 하는 파이프라인에 대 한 인공 지능을 제공 합니다 [Computer Vision] [ vision] 서비스 [얼굴 API][face], 및 [번역할 텍스트] [ translate-text] 기계 번역 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="90565-144">[Azure Cognitive Services][acs] provides the AI for the pipeline based on implementations of the [Computer Vision][vision] service, [Face API][face], and [Translate Text][translate-text] machine translation service.</span></span>

* <span data-ttu-id="90565-145">[Azure Application Insights] [ aai] 문제를 진단할 수 있도록 하는 응용 프로그램의 기능을 이해 하는 분석을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-145">[Azure Application Insights][aai] provides analytics to help you diagnose issues and to understand functionality of your application.</span></span>

### <a name="alternatives"></a><span data-ttu-id="90565-146">대안</span><span class="sxs-lookup"><span data-stu-id="90565-146">Alternatives</span></span>

* <span data-ttu-id="90565-147">알림 큐 및 Azure Functions에 따라 패턴을 사용 하는 대신이 데이터 흐름에 대 한 또 다른 패턴을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-147">Instead of using a pattern based on queue notification and Azure Functions, use another pattern for this data flow.</span></span> <span data-ttu-id="90565-148">예를 들어 [Azure Service Bus 토픽] [ topics] 다양 한 부분으로 병렬로 문서 반대로이 예제에서 수행 되는 직렬 처리 하는 프로세스에 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="90565-148">For example, [Azure Service Bus Topics][topics] can be used to processes the various parts of the article in parallel as opposed to the serial processing done in this example.</span></span> <span data-ttu-id="90565-149">자세한 내용은 비교 [큐 및 토픽][queues-topics]합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-149">For more information, compare [queues and topics][queues-topics].</span></span>

* <span data-ttu-id="90565-150">사용 하 여 [Azure Logic App] [ logic-app] 함수 코드를 구현 하 여 레코드 수준 같은 잠금을 구현 [Redlock] [ redlock] (병렬 필요 합니다. Azure Cosmos DB는 지원 될 때까지 처리 [부분 문서 업데이트][partial]).</span><span class="sxs-lookup"><span data-stu-id="90565-150">Use [Azure Logic App][logic-app] to implement the function code and implement record-level locking such as [Redlock][redlock] (needed for parallel processing until Azure Cosmos DB supports [partial document updates][partial]).</span></span> <span data-ttu-id="90565-151">자세한 내용은 [Functions 및 Logic Apps 비교][compare]합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-151">For more information, [compare Functions and Logic Apps][compare].</span></span>

* <span data-ttu-id="90565-152">기존 Azure 서비스 하는 대신 사용자 지정된 AI 구성 요소를 사용 하 여이 아키텍처를 구현 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-152">Implement this architecture using customized AI components rather than existing Azure services.</span></span> <span data-ttu-id="90565-153">예를 들어, 성별, 일반 사용자 수를 달리 이미지에서 특정인을 검색 하는 사용자 지정된 모델을 사용 하 여 파이프라인을 확장 하 고이 예제에서 수집 된 데이터를 기간.</span><span class="sxs-lookup"><span data-stu-id="90565-153">For example, extend the pipeline using a customized model that detects certain people in an image as opposed to the generic people count, gender, and age data collected in this example.</span></span> <span data-ttu-id="90565-154">이 아키텍처에서 사용자 지정된 기계 학습 또는 인공 지능 모델 사용 하려면 Azure Functions에서 호출할 수 있도록에 RESTful 끝점으로 모델을 빌드하십시오.</span><span class="sxs-lookup"><span data-stu-id="90565-154">To use customized machine learning or AI models with this architecture, build the models as RESTful endpoints so they can be called from Azure Functions.</span></span>

* <span data-ttu-id="90565-155">RSS 피드 대신 다른 입력된 메커니즘을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-155">Use a different input mechanism instead of RSS feeds.</span></span> <span data-ttu-id="90565-156">Azure Cosmos DB 및 Azure Storage를 제공 하기 위해 여러 생성기 또는 수집 프로세스를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-156">Use multiple generators or ingestion processes to feed Azure Cosmos DB and Azure Storage.</span></span>

## <a name="considerations"></a><span data-ttu-id="90565-157">고려 사항</span><span class="sxs-lookup"><span data-stu-id="90565-157">Considerations</span></span>

<span data-ttu-id="90565-158">편의상이 예제 시나리오는 사용 가능한 Api 및 Azure Cognitive Services에서 서비스의 일부만을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-158">For simplicity, this example scenario uses only a few of the available APIs and services from Azure Cognitive Services.</span></span> <span data-ttu-id="90565-159">예를 들어, 이미지의 텍스트를 분석할 수 있습니다 사용 하 여 [Text Analytics API][text-analytics]합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-159">For example, text in images can be analyzed using the [Text Analytics API][text-analytics].</span></span> <span data-ttu-id="90565-160">이 시나리오에서 대상 언어는 영어로 간주 됩니다 있지만 모든 입력을 변경할 수 있습니다 [지원 되는 언어] [ language] 세요.</span><span class="sxs-lookup"><span data-stu-id="90565-160">The target language in this scenario is assumed to be English, but you can change the input to any [supported language][language] of your choice.</span></span>

### <a name="scalability"></a><span data-ttu-id="90565-161">확장성</span><span class="sxs-lookup"><span data-stu-id="90565-161">Scalability</span></span>

<span data-ttu-id="90565-162">Azure Functions 크기 조정에 따라 달라 집니다 합니다 [호스팅 계획] [ plan] 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-162">Azure Functions scaling depends on the [hosting plan][plan] you use.</span></span> <span data-ttu-id="90565-163">이 솔루션에서는 가정 된 [소비 계획][plan-c], 어떤 계산 power 자동으로 필요한 경우 함수에 할당 됩니다.</span><span class="sxs-lookup"><span data-stu-id="90565-163">This solution assumes a [Consumption plan][plan-c], in which compute power is automatically allocated to the functions when required.</span></span> <span data-ttu-id="90565-164">함수는 실행 하는 경우에 지불 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-164">You pay only when your functions are running.</span></span> <span data-ttu-id="90565-165">다른 옵션은 사용 하는 [Azure App Service] [ plan-aas] 계획 서로 다른 양의 리소스를 할당 하는 계층 간에 크기를 조정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="90565-165">Another option is to use an [Azure App Service][plan-aas] plan, which allows you to scale between tiers to allocate a different amount of resources.</span></span>

<span data-ttu-id="90565-166">Azure Cosmos DB를 사용 하 여 키가 균등 하 게 워크 로드가 거의 충분히 많은 수의 간에 [파티션 키][keys]합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-166">With Azure Cosmos DB, the key is to distribute your workload roughly evenly among a sufficiently large number of [partition keys][keys].</span></span> <span data-ttu-id="90565-167">컨테이너를 저장할 수 있는 데이터의 총 크기 또는 총 제한이 [처리량] [ throughput] 컨테이너를 지원할 수 있는 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-167">There's no limit to the total amount of data that a container can store or to the total amount of [throughput][throughput] that a container can support.</span></span>

### <a name="management-and-logging"></a><span data-ttu-id="90565-168">관리 및 로깅</span><span class="sxs-lookup"><span data-stu-id="90565-168">Management and logging</span></span>

<span data-ttu-id="90565-169">이 솔루션에서는 [Application Insights] [ aai] 성능 및 로깅 정보를 수집 하도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-169">This solution uses [Application Insights][aai] to collect performance and logging information.</span></span> <span data-ttu-id="90565-170">이 배포에 필요한 다른 서비스와 동일한 리소스 그룹 배포를 사용 하 여 Application Insights 인스턴스에 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="90565-170">An instance of Application Insights is created with the deployment in the same resource group as the other services needed for this deployment.</span></span>

<span data-ttu-id="90565-171">솔루션에서 생성 한 로그를 보려면:</span><span class="sxs-lookup"><span data-stu-id="90565-171">To view the logs generated by the solution:</span></span>

1. <span data-ttu-id="90565-172">로 이동 [Azure portal] [ portal] 배포에 대해 만든 리소스 그룹으로 이동 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-172">Go to [Azure portal][portal] and navigate to the resource group created for the deployment.</span></span>

2. <span data-ttu-id="90565-173">클릭 합니다 **Application Insights** 인스턴스.</span><span class="sxs-lookup"><span data-stu-id="90565-173">Click the **Application Insights** instance.</span></span>

3. <span data-ttu-id="90565-174">**Application Insights** 섹션에서 이동할 **조사\\검색** 및 데이터를 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-174">From the **Application Insights** section, navigate to **Investigate\\Search** and search the data.</span></span>

### <a name="security"></a><span data-ttu-id="90565-175">보안</span><span class="sxs-lookup"><span data-stu-id="90565-175">Security</span></span>

<span data-ttu-id="90565-176">Azure Cosmos DB는 보안된 연결 및 공유 액세스 서명을 통해 C\# SDK Microsoft에서 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-176">Azure Cosmos DB uses a secured connection and shared access signature through the C\# SDK provided by Microsoft.</span></span> <span data-ttu-id="90565-177">다른 외부와 접한 노출 영역이 없습니다 있습니다.</span><span class="sxs-lookup"><span data-stu-id="90565-177">There are no other externally facing surface areas.</span></span> <span data-ttu-id="90565-178">보안에 자세히 알아보려면 [모범 사례] [ db-practices] Azure Cosmos DB에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-178">Learn more about security [best practices][db-practices] for Azure Cosmos DB.</span></span>

## <a name="pricing"></a><span data-ttu-id="90565-179">가격</span><span class="sxs-lookup"><span data-stu-id="90565-179">Pricing</span></span>

<span data-ttu-id="90565-180">예상된 일일 비용을 계속 사용할 수 있는이 배포는 대략 \$20 시스템을 통해 이동 하는 데이터가 없는 미국.</span><span class="sxs-lookup"><span data-stu-id="90565-180">The estimated daily cost to keep this deployment available is approximately \$20 U.S. with no data moving through the system.</span></span>

<span data-ttu-id="90565-181">Azure Cosmos DB는 강력 하지만 가장 큰 요소를 발생 시킵니다 [비용] [ db-cost] 이 배포 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-181">Azure Cosmos DB is powerful but incurs the greatest [cost][db-cost] in this deployment.</span></span> <span data-ttu-id="90565-182">제공 하는 Azure Functions 코드를 리팩터링하여 다른 저장소 솔루션을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="90565-182">You can use another storage solution by refactoring the Azure Functions code provided.</span></span>

<span data-ttu-id="90565-183">에 따라 달라 집니다 Azure Functions에 대 한 가격 책정 합니다 [계획] [ function-plan] 에서 실행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="90565-183">Pricing for Azure Functions varies depending on the [plan][function-plan] it runs in.</span></span>

## <a name="deploy-the-scenario"></a><span data-ttu-id="90565-184">시나리오 배포</span><span class="sxs-lookup"><span data-stu-id="90565-184">Deploy the scenario</span></span>

> [!NOTE]
> <span data-ttu-id="90565-185">기존 Azure 계정이 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-185">You must have an existing Azure account.</span></span> <span data-ttu-id="90565-186">Azure 구독이 없는 경우 시작하기 전에 [체험 계정][free]을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="90565-186">If you don't have an Azure subscription, create a [free account][free] before you begin.</span></span>

<span data-ttu-id="90565-187">이 시나리오에 대 한 모든 코드는 합니다 [GitHub] [ github] 리포지토리.</span><span class="sxs-lookup"><span data-stu-id="90565-187">All the code for this scenario is available in the [GitHub][github] repository.</span></span> <span data-ttu-id="90565-188">이 리포지토리는이 데모에 대 한 파이프라인을 공급 하는 생성기 응용 프로그램을 빌드하는 데 사용 되는 소스 코드를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="90565-188">This repository contains the source code used to build the generator application that feeds the pipeline for this demo.</span></span>

[architecture]: ./media/mass-ingestion-newsfeeds-architecture.png
[aai]: /azure/azure-monitor/app/app-insights-overview
[aas]: https://azure.microsoft.com/try/app-service/
[acs]: https://azure.microsoft.com/services/cognitive-services/directory/
[collection]: /rest/api/cosmos-db/collections
[compare]: /azure/azure-functions/functions-compare-logic-apps-ms-flow-webjobs#compare-azure-functions-and-azure-logic-apps
[cosmos-db]: /azure/cosmos-db/introduction
[db-cost]: https://azure.microsoft.com/pricing/details/cosmos-db/
[db-practices]: /azure/cosmos-db/database-security
[db-collection]: /azure/cosmos-db/databases-containers-items
[english]: https://www.nasa.gov/rss/dyn/breaking_news.rss
[face]: /azure/cognitive-services/face/overview
[free]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F
[functions]: /azure/azure-functions/functions-overview
[function-plan]: /azure/azure-functions/functions-scale
[german]: http://www.bamf.de/SiteGlobals/Functions/RSS/DE/Feed/RSSNewsfeed_Meldungen
[github]: https://github.com/Azure/cognitive-services
[keys]: /azure/cosmos-db/partition-data
[language]: /azure/cognitive-services/translator/reference/v3-0-languages
[logic-app]: /azure/logic-apps/logic-apps-overview
[queues-topics]: /azure/service-bus-messaging/service-bus-queues-topics-subscriptions
[partial]: https://feedback.azure.com/forums/263030-azure-cosmos-db/suggestions/6693091-be-able-to-do-partial-updates-on-document
[plan]: /azure/azure-functions/functions-scale
[plan-aas]: /azure/azure-functions/functions-scale#app-service-plan
[plan-c]: /azure/azure-functions/functions-scale#consumption-plan
[portal]: http://portal.azure.com
[redlock]: https://redis.io/topics/distlock
[russian]: http://government.ru/all/rss/
[service-bus]: /azure/service-bus-messaging/
[storage]: /azure/storage/common/storage-account-overview 
[throughput]: /azure/cosmos-db/scaling-throughput
[topics]: /azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions
[text-analytics]: /azure/cognitive-services/text-analytics/
[translate-text]: /azure/cognitive-services/translator/translator-info-overview
[vision]: /azure/cognitive-services/computer-vision/home
