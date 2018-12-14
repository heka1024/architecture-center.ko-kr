---
title: Azure Databricks를 사용하는 스트림 처리
titleSuffix: Azure Reference Architectures
description: Azure Databricks를 사용하여 Azure에서 엔드투엔드 스트림 처리 파이프라인을 만듭니다.
author: petertaylor9999
ms.date: 11/30/2018
ms.custom: seodec18
ms.openlocfilehash: 822a3c448dcc2bdd4ae77ef2a2b7a9ffad633440
ms.sourcegitcommit: 88a68c7e9b6b772172b7faa4b9fd9c061a9f7e9d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53120325"
---
# <a name="create-a-stream-processing-pipeline-with-azure-databricks"></a><span data-ttu-id="1a30c-103">Azure Databricks를 사용하는 스트림 처리 파이프라인 만들기</span><span class="sxs-lookup"><span data-stu-id="1a30c-103">Create a stream processing pipeline with Azure Databricks</span></span>

<span data-ttu-id="1a30c-104">이 참조 아키텍처는 엔드투엔드 [스트림 처리](/azure/architecture/data-guide/big-data/real-time-processing) 파이프라인을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-104">This reference architecture shows an end-to-end [stream processing](/azure/architecture/data-guide/big-data/real-time-processing) pipeline.</span></span> <span data-ttu-id="1a30c-105">이 유형의 파이프라인은 수집, 처리, 저장, 분석 및 보고의 4단계로 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-105">This type of pipeline has four stages: ingest, process, store, and analysis and reporting.</span></span> <span data-ttu-id="1a30c-106">이 참조 아키텍처의 경우 파이프라인은 실시간으로 두 원본에서 데이터를 수집하고, 각 스트림의 관련 레코드에 대해 조인을 수행하고, 결과를 보강하고, 평균을 계산합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-106">For this reference architecture, the pipeline ingests data from two sources, performs a join on related records from each stream, enriches the result, and calculates an average in real time.</span></span> <span data-ttu-id="1a30c-107">추가 분석을 위해 결과가 저장됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-107">The results are stored for further analysis.</span></span> <span data-ttu-id="1a30c-108">[**이 솔루션을 배포합니다**](#deploy-the-solution).</span><span class="sxs-lookup"><span data-stu-id="1a30c-108">[**Deploy this solution**](#deploy-the-solution).</span></span>

![Azure Databricks를 사용하는 스트림 처리의 참조 아키텍처](./images/stream-processing-databricks.png)

<span data-ttu-id="1a30c-110">**시나리오**: 한 택시 회사에서 각 택시 운행 데이터를 수집합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-110">**Scenario**: A taxi company collects data about each taxi trip.</span></span> <span data-ttu-id="1a30c-111">이 시나리오의 경우 두 개의 별도 장치가 데이터를 전송하고 있다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-111">For this scenario, we assume there are two separate devices sending data.</span></span> <span data-ttu-id="1a30c-112">택시에는 각 승객 &mdash; 기간, 거리, 승차 및 하차 위치에 대한 정보를 전송하는 미터가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-112">The taxi has a meter that sends information about each ride &mdash; the duration, distance, and pickup and dropoff locations.</span></span> <span data-ttu-id="1a30c-113">별도 장치는 고객의 지불을 수락하고 요금에 대한 데이터를 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-113">A separate device accepts payments from customers and sends data about fares.</span></span> <span data-ttu-id="1a30c-114">택시 회사는 승객 추세를 파악하기 위해 각 환경의 마일당 평균 팁을 실시간으로 계산하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-114">To spot ridership trends, the taxi company wants to calculate the average tip per mile driven, in real time, for each neighborhood.</span></span>

## <a name="architecture"></a><span data-ttu-id="1a30c-115">아키텍처</span><span class="sxs-lookup"><span data-stu-id="1a30c-115">Architecture</span></span>

<span data-ttu-id="1a30c-116">이 아키텍처는 다음 구성 요소로 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-116">The architecture consists of the following components.</span></span>

<span data-ttu-id="1a30c-117">**데이터 원본**.</span><span class="sxs-lookup"><span data-stu-id="1a30c-117">**Data sources**.</span></span> <span data-ttu-id="1a30c-118">이 아키텍처에는 실시간으로 데이터 스트림을 생성하는 두 개의 데이터 원본이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-118">In this architecture, there are two data sources that generate data streams in real time.</span></span> <span data-ttu-id="1a30c-119">첫 번째 스트림에는 승객 정보가 포함되고 두 번째 스트림에는 요금 정보가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-119">The first stream contains ride information, and the second contains fare information.</span></span> <span data-ttu-id="1a30c-120">참조 아키텍처에는 정적 파일 집합을 읽고 Event Hubs에 데이터를 푸시하는 시뮬레이트된 데이터 생성기가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-120">The reference architecture includes a simulated data generator that reads from a set of static files and pushes the data to Event Hubs.</span></span> <span data-ttu-id="1a30c-121">실제 애플리케이션의 데이터 원본은 택시에 설치되는 디바이스입니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-121">The data sources in a real application would be devices installed in the taxi cabs.</span></span>

<span data-ttu-id="1a30c-122">**Azure Event Hubs** -</span><span class="sxs-lookup"><span data-stu-id="1a30c-122">**Azure Event Hubs**.</span></span> <span data-ttu-id="1a30c-123">[Event Hubs](/azure/event-hubs/)는 이벤트 수집 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-123">[Event Hubs](/azure/event-hubs/) is an event ingestion service.</span></span> <span data-ttu-id="1a30c-124">이 아키텍처는 각 데이터 원본에 대해 하나씩 두 개의 이벤트 허브 인스턴스를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-124">This architecture uses two event hub instances, one for each data source.</span></span> <span data-ttu-id="1a30c-125">각 데이터 원본은 연결된 이벤트 허브에 데이터 스트림을 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-125">Each data source sends a stream of data to the associated event hub.</span></span>

<span data-ttu-id="1a30c-126">**Azure Databricks**.</span><span class="sxs-lookup"><span data-stu-id="1a30c-126">**Azure Databricks**.</span></span> <span data-ttu-id="1a30c-127">[Databricks](/azure/azure-databricks/)는 Microsoft Azure Cloud Services 플랫폼에 대해 최적화된 Apache Spark 기반 분석 플랫폼입니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-127">[Databricks](/azure/azure-databricks/) is an Apache Spark-based analytics platform optimized for the Microsoft Azure cloud services platform.</span></span> <span data-ttu-id="1a30c-128">Databricks는 Databricks 파일 시스템에 저장된 환경 데이터를 사용하여 택시 운행과 요금 데이터 간의 상관 관계를 지정하고 상관 관계 데이터를 보강하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-128">Databricks is used to correlate of the taxi ride and fare data, and also to enrich the correlated data with neighborhood data stored in the Databricks file system.</span></span>

<span data-ttu-id="1a30c-129">**Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="1a30c-129">**Cosmos DB**.</span></span> <span data-ttu-id="1a30c-130">Azure Databricks 작업의 출력은 Cassandra API를 사용하여 [Cosmos DB](/azure/cosmos-db/)에 기록되는 일련의 레코드입니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-130">The output from Azure Databricks job is a series of records, which are written to [Cosmos DB](/azure/cosmos-db/) using the Cassandra API.</span></span> <span data-ttu-id="1a30c-131">Cassandra API를 사용하는 이유는 시계열 데이터 모델링을 지원하기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-131">The Cassandra API is used because it supports time series data modeling.</span></span>

<span data-ttu-id="1a30c-132">**Azure Log Analytics**.</span><span class="sxs-lookup"><span data-stu-id="1a30c-132">**Azure Log Analytics**.</span></span> <span data-ttu-id="1a30c-133">[Azure Monitor](/azure/monitoring-and-diagnostics/)를 통해 수집되는 애플리케이션 로그 데이터는 [Log Analytics 작업 영역](/azure/log-analytics)에 저장됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-133">Application log data collected by [Azure Monitor](/azure/monitoring-and-diagnostics/) is stored in a [Log Analytics workspace](/azure/log-analytics).</span></span> <span data-ttu-id="1a30c-134">Log Analytics 쿼리는 메트릭을 분석 및 시각화하고 로그 메시지를 검사하여 애플리케이션 내부의 문제를 식별하는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-134">Log Analytics queries can be used to analyze and visualize metrics and inspect log messages to identify issues within the application.</span></span>

## <a name="data-ingestion"></a><span data-ttu-id="1a30c-135">데이터 수집</span><span class="sxs-lookup"><span data-stu-id="1a30c-135">Data ingestion</span></span>

<span data-ttu-id="1a30c-136">데이터 원본을 시뮬레이션하기 위해 이 참조 아키텍처는 [뉴욕시 택시 데이터](https://uofi.app.box.com/v/NYCtaxidata/folder/2332218797) 데이터 세트<sup>[[1]](#note1)</sup>를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-136">To simulate a data source, this reference architecture uses the [New York City Taxi Data](https://uofi.app.box.com/v/NYCtaxidata/folder/2332218797) dataset<sup>[[1]](#note1)</sup>.</span></span> <span data-ttu-id="1a30c-137">이 데이터 세트에는 4년(2010&ndash;2013) 간의 뉴욕시 택시 운행 데이터가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-137">This dataset contains data about taxi trips in New York City over a four-year period (2010 &ndash; 2013).</span></span> <span data-ttu-id="1a30c-138">두 가지 유형의 레코드를 포함합니다. 그것은 바로 승객 데이터와 요금 데이터입니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-138">It contains two types of record: Ride data and fare data.</span></span> <span data-ttu-id="1a30c-139">승객 데이터에는 여정 기간, 여정 거리 및 승차 및 하차 위치가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-139">Ride data includes trip duration, trip distance, and pickup and dropoff location.</span></span> <span data-ttu-id="1a30c-140">요금 데이터에는 요금, 세금 및 팁 금액이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-140">Fare data includes fare, tax, and tip amounts.</span></span> <span data-ttu-id="1a30c-141">레코드 형식 모두의 공통 필드에는 등록 번호, 택시 라이선스 및 공급 업체 ID가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-141">Common fields in both record types include medallion number, hack license, and vendor ID.</span></span> <span data-ttu-id="1a30c-142">이러한 세 필드는 택시와 드라이버를 고유하게 식별합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-142">Together these three fields uniquely identify a taxi plus a driver.</span></span> <span data-ttu-id="1a30c-143">데이터가 CSV 형식으로 저장됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-143">The data is stored in CSV format.</span></span>

<span data-ttu-id="1a30c-144">데이터 생성기는 레코드를 읽고 Azure Event Hubs로 전송하는 .NET Core 응용 프로그램입니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-144">The data generator is a .NET Core application that reads the records and sends them to Azure Event Hubs.</span></span> <span data-ttu-id="1a30c-145">생성기는 승객 데이터를 JSON 형식으로 보내고, 요금 데이터를 CSV 형식으로 전송합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-145">The generator sends ride data in JSON format and fare data in CSV format.</span></span>

<span data-ttu-id="1a30c-146">Event Hubs는 [파티션](/azure/event-hubs/event-hubs-features#partitions)을 사용하여 데이터를 분할합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-146">Event Hubs uses [partitions](/azure/event-hubs/event-hubs-features#partitions) to segment the data.</span></span> <span data-ttu-id="1a30c-147">파티션을 사용하면 소비자가 각 파티션을 병렬로 읽을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-147">Partitions allow a consumer to read each partition in parallel.</span></span> <span data-ttu-id="1a30c-148">Event Hubs에 데이터를 보낼 때 파티션 키를 명시적으로 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-148">When you send data to Event Hubs, you can specify the partition key explicitly.</span></span> <span data-ttu-id="1a30c-149">그렇지 않으면 레코드는 라운드 로빈 방식으로 파티션에 할당됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-149">Otherwise, records are assigned to partitions in round-robin fashion.</span></span>

<span data-ttu-id="1a30c-150">이 시나리오에서 승객 데이터 및 요금 데이터는 지정된 택시에 대해 동일한 파티션 ID를 사용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-150">In this scenario, ride data and fare data should end up with the same partition ID for a given taxi cab.</span></span> <span data-ttu-id="1a30c-151">그러면 Databricks가 두 스트림의 상관 관계를 지정할 때 일정 수준의 병렬 처리를 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-151">This enables Databricks to apply a degree of parallelism when it correlates the two streams.</span></span> <span data-ttu-id="1a30c-152">승객 데이터의 *n* 파티션에 있는 레코드는 요금 데이터의 *n* 파티션에 있는 레코드와 일치합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-152">A record in partition *n* of the ride data will match a record in partition *n* of the fare data.</span></span>

![Azure Databricks 및 Event Hubs를 사용하는 스트림 처리 다이어그램](./images/stream-processing-databricks-eh.png)

<span data-ttu-id="1a30c-154">데이터 생성기에서 두 레코드 형식에 대한 공통 데이터 모델에는 `Medallion`, `HackLicense` 및 `VendorId`의 연결인 `PartitionKey` 속성이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-154">In the data generator, the common data model for both record types has a `PartitionKey` property that is the concatenation of `Medallion`, `HackLicense`, and `VendorId`.</span></span>

```csharp
public abstract class TaxiData
{
    public TaxiData()
    {
    }

    [JsonProperty]
    public long Medallion { get; set; }

    [JsonProperty]
    public long HackLicense { get; set; }

    [JsonProperty]
    public string VendorId { get; set; }

    [JsonProperty]
    public DateTimeOffset PickupTime { get; set; }

    [JsonIgnore]
    public string PartitionKey
    {
        get => $"{Medallion}_{HackLicense}_{VendorId}";
    }
```

<span data-ttu-id="1a30c-155">이 속성을 사용하여 Event Hubs로 전송하는 경우 명시적인 파티션 키를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-155">This property is used to provide an explicit partition key when sending to Event Hubs:</span></span>

```csharp
using (var client = pool.GetObject())
{
    return client.Value.SendAsync(new EventData(Encoding.UTF8.GetBytes(
        t.GetData(dataFormat))), t.PartitionKey);
}
```

### <a name="event-hubs"></a><span data-ttu-id="1a30c-156">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="1a30c-156">Event Hubs</span></span>

<span data-ttu-id="1a30c-157">Event Hubs의 처리량 용량은 [처리량 단위](/azure/event-hubs/event-hubs-features#throughput-units)로 제어됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-157">The throughput capacity of Event Hubs is measured in [throughput units](/azure/event-hubs/event-hubs-features#throughput-units).</span></span> <span data-ttu-id="1a30c-158">[자동 팽창](/azure/event-hubs/event-hubs-auto-inflate)을 사용하도록 설정하여 이벤트 허브를 자동 크기 조정할 수 있습니다. 그러면 트래픽에 따라 처리량 단위를 구성된 최댓값까지 자동으로 크기 조정합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-158">You can autoscale an event hub by enabling [auto-inflate](/azure/event-hubs/event-hubs-auto-inflate), which automatically scales the throughput units based on traffic, up to a configured maximum.</span></span>

## <a name="stream-processing"></a><span data-ttu-id="1a30c-159">스트림 처리</span><span class="sxs-lookup"><span data-stu-id="1a30c-159">Stream processing</span></span>

<span data-ttu-id="1a30c-160">Azure Databricks에서 데이터 처리는 작업을 통해 수행됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-160">In Azure Databricks, data processing is performed by a job.</span></span> <span data-ttu-id="1a30c-161">작업은 클러스터에 할당되고 클러스터에서 실행됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-161">The job is assigned to and runs on a cluster.</span></span> <span data-ttu-id="1a30c-162">작업은 Java로 작성된 사용자 지정 코드이거나 Spark [Notebook](https://docs.databricks.com/user-guide/notebooks/index.html)입니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-162">The job can either be custom code written in Java, or a Spark [notebook](https://docs.databricks.com/user-guide/notebooks/index.html).</span></span>

<span data-ttu-id="1a30c-163">이 참조 아키텍처의 작업은 클래스가 Java 및 Scala로 작성된 Java 아카이브입니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-163">In this reference architecture, the job is a Java archive with classes written in both Java and Scala.</span></span> <span data-ttu-id="1a30c-164">Databricks 작업의 Java 아카이브를 지정할 때 Databricks 클러스터의 실행에 대한 클래스가 지정됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-164">When specifying the Java archive for a Databricks job, the class is specified for execution by the Databricks cluster.</span></span> <span data-ttu-id="1a30c-165">여기서 **com.microsoft.pnp.TaxiCabReader** 클래스의 **주** 메서드에는 데이터 처리 논리가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-165">Here, the **main** method of the **com.microsoft.pnp.TaxiCabReader** class contains the data processing logic.</span></span>

### <a name="reading-the-stream-from-the-two-event-hub-instances"></a><span data-ttu-id="1a30c-166">두 이벤트 허브 인스턴스에서 스트림 읽기</span><span class="sxs-lookup"><span data-stu-id="1a30c-166">Reading the stream from the two event hub instances</span></span>

<span data-ttu-id="1a30c-167">데이터 처리 논리는 [Spark 구조적 스트리밍](https://spark.apache.org/docs/2.1.2/structured-streaming-programming-guide.html)을 사용하여 두 Azure 이벤트 허브 인스턴스에서 스트림을 읽습니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-167">The data processing logic uses [Spark structured streaming](https://spark.apache.org/docs/2.1.2/structured-streaming-programming-guide.html) to read from the two Azure event hub instances:</span></span>

```scala
val rideEventHubOptions = EventHubsConf(rideEventHubConnectionString)
      .setConsumerGroup(conf.taxiRideConsumerGroup())
      .setStartingPosition(EventPosition.fromStartOfStream)
    val rideEvents = spark.readStream
      .format("eventhubs")
      .options(rideEventHubOptions.toMap)
      .load

    val fareEventHubOptions = EventHubsConf(fareEventHubConnectionString)
      .setConsumerGroup(conf.taxiFareConsumerGroup())
      .setStartingPosition(EventPosition.fromStartOfStream)
    val fareEvents = spark.readStream
      .format("eventhubs")
      .options(fareEventHubOptions.toMap)
      .load
```

### <a name="enriching-the-data-with-the-neighborhood-information"></a><span data-ttu-id="1a30c-168">환경 정보를 사용하여 데이터 보강</span><span class="sxs-lookup"><span data-stu-id="1a30c-168">Enriching the data with the neighborhood information</span></span>

<span data-ttu-id="1a30c-169">승객 데이터에는 승차 및 하차 위치의 위도와 경도 좌표가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-169">The ride data includes the latitude and longitude coordinates of the pick up and drop off locations.</span></span> <span data-ttu-id="1a30c-170">이러한 좌표는 유용하기는 하지만 분석에 쉽게 사용하기 어렵습니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-170">While these coordinates are useful, they are not easily consumed for analysis.</span></span> <span data-ttu-id="1a30c-171">따라서 [셰이프 파일](https://en.wikipedia.org/wiki/Shapefile)에서 읽은 환경 데이터를 사용하여 이 데이터를 보강합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-171">Therefore, this data is enriched with neighborhood data that is read from a [shapefile](https://en.wikipedia.org/wiki/Shapefile).</span></span>

<span data-ttu-id="1a30c-172">셰이프 파일 형식은 이진이고 간단하게 구문 분석할 수 없지만, [GeoTools](http://geotools.org/) 라이브러리에서 셰이프 파일 형식을 사용하는 지리 공간 데이터를 분석할 수 있는 도구를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-172">The shapefile format is binary and not easily parsed, but the [GeoTools](http://geotools.org/) library provides tools for geospatial data that use the shapefile format.</span></span> <span data-ttu-id="1a30c-173">이 라이브러리는 **com.microsoft.pnp.GeoFinder** 클래스에 사용되어 승차 및 하차 좌표를 기준으로 환경 이름을 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-173">This library is used in the **com.microsoft.pnp.GeoFinder** class to determine the neighborhood name based on the pick up and drop off coordinates.</span></span>

```scala
val neighborhoodFinder = (lon: Double, lat: Double) => {
      NeighborhoodFinder.getNeighborhood(lon, lat).get()
    }
```

### <a name="joining-the-ride-and-fare-data"></a><span data-ttu-id="1a30c-174">승객 및 요금 데이터 조인</span><span class="sxs-lookup"><span data-stu-id="1a30c-174">Joining the ride and fare data</span></span>

<span data-ttu-id="1a30c-175">먼저 승객 및 요금 데이터가 변환됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-175">First the ride and fare data is transformed:</span></span>

```scala
    val rides = transformedRides
      .filter(r => {
        if (r.isNullAt(r.fieldIndex("errorMessage"))) {
          true
        }
        else {
          malformedRides.add(1)
          false
        }
      })
      .select(
        $"ride.*",
        to_neighborhood($"ride.pickupLon", $"ride.pickupLat")
          .as("pickupNeighborhood"),
        to_neighborhood($"ride.dropoffLon", $"ride.dropoffLat")
          .as("dropoffNeighborhood")
      )
      .withWatermark("pickupTime", conf.taxiRideWatermarkInterval())

    val fares = transformedFares
      .filter(r => {
        if (r.isNullAt(r.fieldIndex("errorMessage"))) {
          true
        }
        else {
          malformedFares.add(1)
          false
        }
      })
      .select(
        $"fare.*",
        $"pickupTime"
      )
      .withWatermark("pickupTime", conf.taxiFareWatermarkInterval())
```

<span data-ttu-id="1a30c-176">그런 다음, 승객 데이터가 요금 데이터에 조인됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-176">And then the ride data is joined with the fare data:</span></span>

```scala
val mergedTaxiTrip = rides.join(fares, Seq("medallion", "hackLicense", "vendorId", "pickupTime"))
```

### <a name="processing-the-data-and-inserting-into-cosmos-db"></a><span data-ttu-id="1a30c-177">데이터를 처리하여 Cosmos DB에 삽입</span><span class="sxs-lookup"><span data-stu-id="1a30c-177">Processing the data and inserting into Cosmos DB</span></span>

<span data-ttu-id="1a30c-178">지정된 시간 간격마다 각 환경의 평균 요금이 계산됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-178">The average fare amount for each neighborhood is calculated for a given time interval:</span></span>

```scala
val maxAvgFarePerNeighborhood = mergedTaxiTrip.selectExpr("medallion", "hackLicense", "vendorId", "pickupTime", "rateCode", "storeAndForwardFlag", "dropoffTime", "passengerCount", "tripTimeInSeconds", "tripDistanceInMiles", "pickupLon", "pickupLat", "dropoffLon", "dropoffLat", "paymentType", "fareAmount", "surcharge", "mtaTax", "tipAmount", "tollsAmount", "totalAmount", "pickupNeighborhood", "dropoffNeighborhood")
      .groupBy(window($"pickupTime", conf.windowInterval()), $"pickupNeighborhood")
      .agg(
        count("*").as("rideCount"),
        sum($"fareAmount").as("totalFareAmount"),
        sum($"tipAmount").as("totalTipAmount")
      )
      .select($"window.start", $"window.end", $"pickupNeighborhood", $"rideCount", $"totalFareAmount", $"totalTipAmount")
```

<span data-ttu-id="1a30c-179">계산된 요금은 Cosmos DB에 삽입됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-179">Which is then inserted into Cosmos DB:</span></span>

```scala
maxAvgFarePerNeighborhood
      .writeStream
      .queryName("maxAvgFarePerNeighborhood_cassandra_insert")
      .outputMode(OutputMode.Append())
      .foreach(new CassandraSinkForeach(connector))
      .start()
      .awaitTermination()
```

## <a name="security-considerations"></a><span data-ttu-id="1a30c-180">보안 고려 사항</span><span class="sxs-lookup"><span data-stu-id="1a30c-180">Security considerations</span></span>

<span data-ttu-id="1a30c-181">Azure Database 작업 영역에 대한 액세스는 [관리자 콘솔](https://docs.databricks.com/administration-guide/admin-settings/index.html)을 사용하여 제어됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-181">Access to the Azure Database workspace is controlled using the [administrator console](https://docs.databricks.com/administration-guide/admin-settings/index.html).</span></span> <span data-ttu-id="1a30c-182">관리자 콘솔에는 사용자를 추가하고, 사용자 권한을 관리하고, Single Sign-On을 설정하는 기능이 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-182">The administrator console includes functionality to add users, manage user permissions, and set up single sign-on.</span></span> <span data-ttu-id="1a30c-183">관리자 콘솔을 통해 작업 영역, 클러스터, 작업 및 테이블에 대한 액세스 제어를 설정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-183">Access control for workspaces, clusters, jobs, and tables can also be set through the administrator console.</span></span>

### <a name="managing-secrets"></a><span data-ttu-id="1a30c-184">암호 관리</span><span class="sxs-lookup"><span data-stu-id="1a30c-184">Managing secrets</span></span>

<span data-ttu-id="1a30c-185">Azure Databricks에는 연결 문자열, 액세스 키, 사용자 이름 및 암호를 포함하여 비밀을 저장하는 데 사용되는 [비밀 저장소](https://docs.azuredatabricks.net/user-guide/secrets/index.html)가 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-185">Azure Databricks includes a [secret store](https://docs.azuredatabricks.net/user-guide/secrets/index.html) that is used to store secrets, including connection strings, access keys, user names, and passwords.</span></span> <span data-ttu-id="1a30c-186">Azure Databricks 비밀 저장소의 비밀은 **범위**에 따라 분할됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-186">Secrets within the Azure Databricks secret store are partitioned by **scopes**:</span></span>

```bash
databricks secrets create-scope --scope "azure-databricks-job"
```

<span data-ttu-id="1a30c-187">비밀은 범위 수준에서 추가됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-187">Secrets are added at the scope level:</span></span>

```bash
databricks secrets put --scope "azure-databricks-job" --key "taxi-ride"
```

> [!NOTE]
> <span data-ttu-id="1a30c-188">기본 Azure Databricks 범위 대신 Azure Key Vault에서 지원되는 범위를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-188">An Azure Key Vault-backed scope can be used instead of the native Azure Databricks scope.</span></span> <span data-ttu-id="1a30c-189">자세한 내용은 [Azure Key Vault에서 지원하는 범위](https://docs.azuredatabricks.net/user-guide/secrets/secret-scopes.html#azure-key-vault-backed-scopes)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="1a30c-189">To learn more, see [Azure Key Vault-backed scopes](https://docs.azuredatabricks.net/user-guide/secrets/secret-scopes.html#azure-key-vault-backed-scopes).</span></span>

<span data-ttu-id="1a30c-190">코드에서 비밀은 Azure Databricks [비밀 유틸리티](https://docs.databricks.com/user-guide/dev-tools/dbutils.html#secrets-utilities)를 통해 액세스됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-190">In code, secrets are accessed via the Azure Databricks [secrets utilities](https://docs.databricks.com/user-guide/dev-tools/dbutils.html#secrets-utilities).</span></span>

## <a name="monitoring-considerations"></a><span data-ttu-id="1a30c-191">모니터링 고려 사항</span><span class="sxs-lookup"><span data-stu-id="1a30c-191">Monitoring considerations</span></span>

<span data-ttu-id="1a30c-192">Azure Databricks는 Apache Spark를 기반으로 하며, 둘 다 로깅의 표준 라이브러리로 [log4j](https://logging.apache.org/log4j/2.x/)를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-192">Azure Databricks is based on Apache Spark, and both use [log4j](https://logging.apache.org/log4j/2.x/) as the standard library for logging.</span></span> <span data-ttu-id="1a30c-193">Apache Spark에서 제공하는 기본 로깅 외에도, 이 참조 아키텍처는 [Azure Log Analytics](/azure/log-analytics/)로 로그 및 메트릭을 전송합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-193">In addition to the default logging provided by Apache Spark, this reference architecture sends logs and metrics to [Azure Log Analytics](/azure/log-analytics/).</span></span>

<span data-ttu-id="1a30c-194">**com.microsoft.pnp.TaxiCabReader** 클래스는 **log4j.properties** 파일의 값을 사용하여 Azure Log Analytics로 로그를 보내도록 Apache Spark 로깅 시스템을 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-194">The **com.microsoft.pnp.TaxiCabReader** class configures the Apache Spark logging system to send its logs to Azure Log Analytics using the values in the **log4j.properties** file.</span></span> <span data-ttu-id="1a30c-195">Apache Spark 로거 메시지는 문자열이지만, Azure Log Analytics는 로그 메시지 형식으로 JSON을 요구합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-195">While the Apache Spark logger messages are strings, Azure Log Analytics requires log messages to be formatted as JSON.</span></span> <span data-ttu-id="1a30c-196">**com.microsoft.pnp.log4j.LogAnalyticsAppender** 클래스는 이러한 메시지를 JSON으로 변환합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-196">The **com.microsoft.pnp.log4j.LogAnalyticsAppender** class transforms these messages to JSON:</span></span>

```scala

    @Override
    protected void append(LoggingEvent loggingEvent) {
        if (this.layout == null) {
            this.setLayout(new JSONLayout());
        }

        String json = this.getLayout().format(loggingEvent);
        try {
            this.client.send(json, this.logType);
        } catch(IOException ioe) {
            LogLog.warn("Error sending LoggingEvent to Log Analytics", ioe);
        }
    }

```

<span data-ttu-id="1a30c-197">**com.microsoft.pnp.TaxiCabReader** 클래스가 승객 및 요금 메시지를 처리할 때 둘 중 하나의 형식이 잘못되어 유효하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-197">As the **com.microsoft.pnp.TaxiCabReader** class processes ride and fare messages, it's possible that either one may be malformed and therefore not valid.</span></span> <span data-ttu-id="1a30c-198">프로덕션 환경에서는 신속히 문제를 해결하여 데이터 손실을 방지할 수 있도록 이처럼 잘못된 형식의 메시지를 분석하여 데이터 원본의 문제를 파악하는 것이 중요합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-198">In a production environment, it's important to analyze these malformed messages to identify a problem with the data sources so it can be fixed quickly to prevent data loss.</span></span> <span data-ttu-id="1a30c-199">**com.microsoft.pnp.TaxiCabReader** 클래스는 형식이 잘못된 요금 및 승객 레코드 수를 추적하는 Apache Spark 누적기를 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-199">The **com.microsoft.pnp.TaxiCabReader** class registers an Apache Spark Accumulator that keeps track of the number of malformed fare and ride records:</span></span>

```scala
    @transient val appMetrics = new AppMetrics(spark.sparkContext)
    appMetrics.registerGauge("metrics.malformedrides", AppAccumulators.getRideInstance(spark.sparkContext))
    appMetrics.registerGauge("metrics.malformedfares", AppAccumulators.getFareInstance(spark.sparkContext))
    SparkEnv.get.metricsSystem.registerSource(appMetrics)
```

<span data-ttu-id="1a30c-200">Apache Spark는 Dropwizard 라이브러리를 사용하여 메트릭을 보내며, 네이티브 Dropwizard 메트릭 필드 중 일부는 Azure Log Analytics와 호환되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-200">Apache Spark uses the Dropwizard library to send metrics, and some of the native Dropwizard metrics fields are incompatible with Azure Log Analytics.</span></span> <span data-ttu-id="1a30c-201">이러한 이유로 이 참조 아키텍처에는 사용자 지정 Dropwizard 싱크 및 보고자가 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-201">Therefore, this reference architecture includes a custom Dropwizard sink and reporter.</span></span> <span data-ttu-id="1a30c-202">사용자 지정 Dropwizard 싱크 및 보고자는 Azure Log Analytics에서 예상하는 형식으로 메트릭을 포맷합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-202">It formats the metrics in the format expected by Azure Log Analytics.</span></span> <span data-ttu-id="1a30c-203">Apache Spark가 메트릭을 보고할 때 잘못된 승객 및 요금 데이터에 대한 사용자 지정 메트릭도 전송됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-203">When Apache Spark reports metrics, the custom metrics for the malformed ride and fare data are also sent.</span></span>

<span data-ttu-id="1a30c-204">Azure Log Analytics 작업 영역에 기록할 마지막 메트릭은 Spark 구조적 스트림 작업 진행률의 누적 진행률입니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-204">The last metric to be logged to the Azure Log Analytics workspace is the cumulative progress of the Spark Structured Streaming job progress.</span></span> <span data-ttu-id="1a30c-205">이 작업은 **com.microsoft.pnp.StreamingMetricsListener** 클래스에 구현된 사용자 지정 StreamingQuery 수신기를 사용하여 수행됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-205">This is done using a custom StreamingQuery listener implemented in the **com.microsoft.pnp.StreamingMetricsListener** class.</span></span> <span data-ttu-id="1a30c-206">이 클래스는 작업이 실행될 때 Apache Spark 세션에 등록됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-206">This class is registered to the Apache Spark Session when the job runs:</span></span>

```scala
spark.streams.addListener(new StreamingMetricsListener())
```

<span data-ttu-id="1a30c-207">StreamingMetricsListener의 메서드는 구조적 스트리밍 이벤트가 발생할 때마다 Apache Spark 런타임에 의해 호출되어 Azure Log Analytics 작업 영역으로 메시지 및 메트릭을 전송합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-207">The methods in the StreamingMetricsListener are called by the Apache Spark runtime whenever a structured steaming event occurs, sending log messages and metrics to the Azure Log Analytics workspace.</span></span> <span data-ttu-id="1a30c-208">작업 영역에서 다음 쿼리를 사용하여 애플리케이션을 모니터링할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-208">You can use the following queries in your workspace to monitor the application:</span></span>

### <a name="latency-and-throughput-for-streaming-queries"></a><span data-ttu-id="1a30c-209">스트리밍 쿼리의 대기 시간 및 처리량</span><span class="sxs-lookup"><span data-stu-id="1a30c-209">Latency and throughput for streaming queries</span></span>

```shell
taxijob_CL
| where TimeGenerated > startofday(datetime(<date>)) and TimeGenerated < endofday(datetime(<date>))
| project  mdc_inputRowsPerSecond_d, mdc_durationms_triggerExecution_d
| render timechart
```

### <a name="exceptions-logged-during-stream-query-execution"></a><span data-ttu-id="1a30c-210">스트림 쿼리를 실행하는 동안 기록된 예외</span><span class="sxs-lookup"><span data-stu-id="1a30c-210">Exceptions logged during stream query execution</span></span>

```shell
taxijob_CL
| where TimeGenerated > startofday(datetime(<date>)) and TimeGenerated < endofday(datetime(<date>))
| where Level contains "Error"
```

### <a name="accumulation-of-malformed-fare-and-ride-data"></a><span data-ttu-id="1a30c-211">잘못된 형식의 요금 및 승객 데이터의 누적</span><span class="sxs-lookup"><span data-stu-id="1a30c-211">Accumulation of malformed fare and ride data</span></span>

```shell
SparkMetric_CL
| where TimeGenerated > startofday(datetime(<date>)) and TimeGenerated < endofday(datetime(<date>))
| render timechart
| where name_s contains "metrics.malformedrides"

SparkMetric_CL
| where TimeGenerated > startofday(datetime(<date>)) and TimeGenerated < endofday(datetime(<date>))
| render timechart
| where name_s contains "metrics.malformedfares"
```

### <a name="job-execution-to-trace-resiliency"></a><span data-ttu-id="1a30c-212">복원력을 추적하는 작업 실행</span><span class="sxs-lookup"><span data-stu-id="1a30c-212">Job execution to trace resiliency</span></span>

```shell
SparkMetric_CL
| where TimeGenerated > startofday(datetime(<date>)) and TimeGenerated < endofday(datetime(<date>))
| render timechart
| where name_s contains "driver.DAGScheduler.job.allJobs"
```

## <a name="deploy-the-solution"></a><span data-ttu-id="1a30c-213">솔루션 배포</span><span class="sxs-lookup"><span data-stu-id="1a30c-213">Deploy the solution</span></span>

<span data-ttu-id="1a30c-214">이 참조 아키텍처에 대한 배포는 [GitHub](https://github.com/mspnp/azure-databricks-streaming-analytics)에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-214">A deployment for this reference architecture is available on [GitHub](https://github.com/mspnp/azure-databricks-streaming-analytics).</span></span>

### <a name="prerequisites"></a><span data-ttu-id="1a30c-215">필수 조건</span><span class="sxs-lookup"><span data-stu-id="1a30c-215">Prerequisites</span></span>

1. <span data-ttu-id="1a30c-216">[Azure Databricks를 사용한 스트림 처리](https://github.com/mspnp/azure-databricks-streaming-analytics) GitHub 리포지토리를 복제, 포크 또는 다운로드합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-216">Clone, fork, or download the [stream processing with Azure Databricks](https://github.com/mspnp/azure-databricks-streaming-analytics) GitHub repository.</span></span>

2. <span data-ttu-id="1a30c-217">[Docker](https://www.docker.com/)를 설치하여 데이터 생성기를 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-217">Install [Docker](https://www.docker.com/) to run the data generator.</span></span>

3. <span data-ttu-id="1a30c-218">[Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest)을 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-218">Install [Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest).</span></span>

4. <span data-ttu-id="1a30c-219">[Databricks CLI](https://docs.databricks.com/user-guide/dev-tools/databricks-cli.html)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-219">Install [Databricks CLI](https://docs.databricks.com/user-guide/dev-tools/databricks-cli.html).</span></span>

5. <span data-ttu-id="1a30c-220">명령 프롬프트, bash 프롬프트 또는 PowerShell 프롬프트에서 다음과 같은 Azure 계정에 로그인합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-220">From a command prompt, bash prompt, or PowerShell prompt, sign into your Azure account as follows:</span></span>
    ```shell
    az login
    ```
6. <span data-ttu-id="1a30c-221">다음 리소스를 사용하여 Java IDE를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-221">Install a Java IDE, with the following resources:</span></span>
    - <span data-ttu-id="1a30c-222">JDK 1.8</span><span class="sxs-lookup"><span data-stu-id="1a30c-222">JDK 1.8</span></span>
    - <span data-ttu-id="1a30c-223">Scala SDK 2.11</span><span class="sxs-lookup"><span data-stu-id="1a30c-223">Scala SDK 2.11</span></span>
    - <span data-ttu-id="1a30c-224">Maven 3.5.4</span><span class="sxs-lookup"><span data-stu-id="1a30c-224">Maven 3.5.4</span></span>

### <a name="download-the-new-york-city-taxi-and-neighborhood-data-files"></a><span data-ttu-id="1a30c-225">뉴욕시 택시 및 환경 데이터 파일을 다운로드</span><span class="sxs-lookup"><span data-stu-id="1a30c-225">Download the New York City taxi and neighborhood data files</span></span>

1. <span data-ttu-id="1a30c-226">로컬 파일 시스템에서 복제된 Github 리포지토리의 루트에 `DataFile`이라는 디렉터리를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-226">Create a directory named `DataFile` in the root of the cloned Github repository in your local file system.</span></span>

2. <span data-ttu-id="1a30c-227">웹 브라우저를 열고 https://uofi.app.box.com/v/NYCtaxidata/folder/2332219935로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-227">Open a web browser and navigate to https://uofi.app.box.com/v/NYCtaxidata/folder/2332219935.</span></span>

3. <span data-ttu-id="1a30c-228">이 페이지에서 **다운로드**를 클릭하여 해당 연도에 대한 모든 택시 데이터의 zip 파일을 다운로드합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-228">Click the **Download** button on this page to download a zip file of all the taxi data for that year.</span></span>

4. <span data-ttu-id="1a30c-229">`DataFile` 디렉터리에 zip 파일의 압축을 풉니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-229">Extract the zip file to the `DataFile` directory.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1a30c-230">이 zip 파일에는 다른 zip 파일이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-230">This zip file contains other zip files.</span></span> <span data-ttu-id="1a30c-231">자식 zip 파일의 압축을 풀지 마십시오.</span><span class="sxs-lookup"><span data-stu-id="1a30c-231">Don't extract the child zip files.</span></span>

    <span data-ttu-id="1a30c-232">디렉터리 구조는 다음과 같이 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-232">The directory structure must look like the following:</span></span>

    ```shell
    /DataFile
        /FOIL2013
            trip_data_1.zip
            trip_data_2.zip
            trip_data_3.zip
            ...
    ```

5. <span data-ttu-id="1a30c-233">웹 브라우저를 열고 https://www.zillow.com/howto/api/neighborhood-boundaries.htm로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-233">Open a web browser and navigate to https://www.zillow.com/howto/api/neighborhood-boundaries.htm.</span></span>

6. <span data-ttu-id="1a30c-234">**뉴욕 환경 경계**를 클릭하여 파일을 다운로드합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-234">Click on **New York Neighborhood Boundaries** to download the file.</span></span>

7. <span data-ttu-id="1a30c-235">브라우저의 **다운로드** 디렉터리에서 `DataFile` 디렉터리로 **ZillowNeighborhoods NY.zip** 파일을 복사합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-235">Copy the **ZillowNeighborhoods-NY.zip** file from your browser's **downloads** directory to the `DataFile` directory.</span></span>

### <a name="deploy-the-azure-resources"></a><span data-ttu-id="1a30c-236">Azure 리소스 배포</span><span class="sxs-lookup"><span data-stu-id="1a30c-236">Deploy the Azure resources</span></span>

1. <span data-ttu-id="1a30c-237">셸 또는 Windows 명령 프롬프트에서 다음 명령을 실행하고 로그인 프롬프트를 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-237">From a shell or Windows Command Prompt, run the following command and follow the sign-in prompt:</span></span>

    ```bash
    az login
    ```

2. <span data-ttu-id="1a30c-238">GitHub 리포지토리의 `azure` 폴더로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-238">Navigate to the folder named `azure` in the GitHub repository:</span></span>

    ```bash
    cd azure
    ```

3. <span data-ttu-id="1a30c-239">다음 명령을 실행하여 Azure 리소스를 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-239">Run the following commands to deploy the Azure resources:</span></span>

    ```bash
    export resourceGroup='[Resource group name]'
    export resourceLocation='[Region]'
    export eventHubNamespace='[Event Hubs namespace name]'
    export databricksWorkspaceName='[Azure Databricks workspace name]'
    export cosmosDatabaseAccount='[Cosmos DB database name]'
    export logAnalyticsWorkspaceName='[Log Analytics workspace name]'
    export logAnalyticsWorkspaceRegion='[Log Analytics region]'

    # Create a resource group
    az group create --name $resourceGroup --location $resourceLocation

    # Deploy resources
    az group deployment create --resource-group $resourceGroup \
        --template-file deployresources.json --parameters \
        eventHubNamespace=$eventHubNamespace \
        databricksWorkspaceName=$databricksWorkspaceName \
        cosmosDatabaseAccount=$cosmosDatabaseAccount \
        logAnalyticsWorkspaceName=$logAnalyticsWorkspaceName \
        logAnalyticsWorkspaceRegion=$logAnalyticsWorkspaceRegion
    ```

4. <span data-ttu-id="1a30c-240">작업이 완료되면 배포의 출력이 콘솔에 기록됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-240">The output of the deployment is written to the console once complete.</span></span> <span data-ttu-id="1a30c-241">다음 JSON에 대한 출력을 검색합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-241">Search the output for the following JSON:</span></span>

```json
"outputs": {
        "cosmosDb": {
          "type": "Object",
          "value": {
            "hostName": <value>,
            "secret": <value>,
            "username": <value>
          }
        },
        "eventHubs": {
          "type": "Object",
          "value": {
            "taxi-fare-eh": <value>,
            "taxi-ride-eh": <value>
          }
        },
        "logAnalytics": {
          "type": "Object",
          "value": {
            "secret": <value>,
            "workspaceId": <value>
          }
        }
},
```

<span data-ttu-id="1a30c-242">이러한 값은 다음 섹션에서 Databricks 비밀에 추가될 비밀입니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-242">These values are the secrets that will be added to Databricks secrets in upcoming sections.</span></span> <span data-ttu-id="1a30c-243">다음 섹션에서 추가할 때까지 비밀을 안전하게 보관하세요.</span><span class="sxs-lookup"><span data-stu-id="1a30c-243">Keep them secure until you add them in those sections.</span></span>

### <a name="add-a-cassandra-table-to-the-cosmos-db-account"></a><span data-ttu-id="1a30c-244">Cosmos DB 계정에 Cassandra 테이블 추가</span><span class="sxs-lookup"><span data-stu-id="1a30c-244">Add a Cassandra table to the Cosmos DB Account</span></span>

1. <span data-ttu-id="1a30c-245">Azure Portal에서, 이전에 **Azure 리소스 배포** 섹션에서 만든 리소스 그룹으로 이동 합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-245">In the Azure portal, navigate to the resource group created in the **deploy the Azure resources** section above.</span></span> <span data-ttu-id="1a30c-246">**Azure Cosmos DB 계정**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-246">Click on **Azure Cosmos DB Account**.</span></span> <span data-ttu-id="1a30c-247">Cassandra API를 사용하여 테이블을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-247">Create a table with the Cassandra API.</span></span>

2. <span data-ttu-id="1a30c-248">**개요** 블레이드에서 **테이블 추가**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-248">In the **overview** blade, click **add table**.</span></span>

3. <span data-ttu-id="1a30c-249">**테이블 추가** 블레이드가 열리면 **키스페이스 이름** 텍스트 상자에 `newyorktaxi`를 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-249">When the **add table** blade opens, enter `newyorktaxi` in the **Keyspace name** text box.</span></span>

4. <span data-ttu-id="1a30c-250">**테이블을 만들려면 CQL 명령을 입력** 섹션에서, `newyorktaxi` 옆에 있는 텍스트 상자에 `neighborhoodstats`를 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-250">In the **enter CQL command to create the table** section, enter `neighborhoodstats` in the text box beside `newyorktaxi`.</span></span>

5. <span data-ttu-id="1a30c-251">아래 텍스트 상자에 다음을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-251">In the text box below, enter the following:</span></span>
    ```shell
    (neighborhood text, window_end timestamp, number_of_rides bigint,total_fare_amount double, primary key(neighborhood, window_end))
    ```
6. <span data-ttu-id="1a30c-252">**처리량(1,000-1,000,000RU/s)** 텍스트 상자에 `4000` 값을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-252">In the **Throughput (1,000 - 1,000,000 RU/s)** text box enter the value `4000`.</span></span>

7. <span data-ttu-id="1a30c-253">**확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-253">Click **OK**.</span></span>

### <a name="add-the-databricks-secrets-using-the-databricks-cli"></a><span data-ttu-id="1a30c-254">Databricks CLI를 사용하여 Databricks 비밀 추가</span><span class="sxs-lookup"><span data-stu-id="1a30c-254">Add the Databricks secrets using the Databricks CLI</span></span>

<span data-ttu-id="1a30c-255">먼저 EventHub의 비밀을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-255">First, enter the secrets for EventHub:</span></span>

1. <span data-ttu-id="1a30c-256">필수 구성 요소의 2단계에서 설치한 **Azure Databricks CLI**를 사용하여 Azure Databricks 비밀 범위를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-256">Using the **Azure Databricks CLI** installed in step 2 of the prerequisites, create the Azure Databricks secret scope:</span></span>
    ```shell
    databricks secrets create-scope --scope "azure-databricks-job"
    ```
2. <span data-ttu-id="1a30c-257">택시 승객 EventHub의 비밀을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-257">Add the secret for the taxi ride EventHub:</span></span>
    ```shell
    databricks secrets put --scope "azure-databricks-job" --key "taxi-ride"
    ```
    <span data-ttu-id="1a30c-258">실행되면 이 명령이 vi 편집기를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-258">Once executed, this command opens the vi editor.</span></span> <span data-ttu-id="1a30c-259">*Azure 리소스 배포* 섹션의 4단계에서 나오는 **eventHubs** 출력 섹션의 **taxi-ride-eh** 값을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-259">Enter the **taxi-ride-eh** value from the **eventHubs** output section in step 4 of the *deploy the Azure resources* section.</span></span> <span data-ttu-id="1a30c-260">vi를 저장하고 종료합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-260">Save and exit vi.</span></span>

3. <span data-ttu-id="1a30c-261">택시 요금 EventHub의 비밀을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-261">Add the secret for the taxi fare EventHub:</span></span>
    ```shell
    databricks secrets put --scope "azure-databricks-job" --key "taxi-fare"
    ```
    <span data-ttu-id="1a30c-262">실행되면 이 명령이 vi 편집기를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-262">Once executed, this command opens the vi editor.</span></span> <span data-ttu-id="1a30c-263">*Azure 리소스 배포* 섹션의 4단계에서 나오는 **eventHubs** 출력 섹션의 **taxi-fare-eh** 값을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-263">Enter the **taxi-fare-eh** value from the **eventHubs** output section in step 4 of the *deploy the Azure resources* section.</span></span> <span data-ttu-id="1a30c-264">vi를 저장하고 종료합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-264">Save and exit vi.</span></span>

<span data-ttu-id="1a30c-265">다음으로 Cosmos DB의 비밀을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-265">Next, enter the secrets for Cosmos DB:</span></span>

1. <span data-ttu-id="1a30c-266">Azure Portal에서, **Azure 리소스 배포** 섹션의 3단계에서 지정한 리소스 그룹으로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-266">Open the Azure portal, and navigate to the resource group specified in step 3 of the **deploy the Azure resources** section.</span></span> <span data-ttu-id="1a30c-267">Azure Cosmos DB 계정을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-267">Click on the Azure Cosmos DB Account.</span></span>

2. <span data-ttu-id="1a30c-268">**Azure Databricks CLI**를 사용하여 Cosmos DB 사용자 이름의 비밀을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-268">Using the **Azure Databricks CLI**, add the secret for the Cosmos DB user name:</span></span>
    ```shell
    databricks secrets put --scope azure-databricks-job --key "cassandra-username"
    ```
<span data-ttu-id="1a30c-269">실행되면 이 명령이 vi 편집기를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-269">Once executed, this command opens the vi editor.</span></span> <span data-ttu-id="1a30c-270">*Azure 리소스 배포* 섹션의 4단계에서 나오는 **CosmosDb** 출력 섹션의 **사용자 이름** 값을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-270">Enter the **username** value from the **CosmosDb** output section in step 4 of the *deploy the Azure resources* section.</span></span> <span data-ttu-id="1a30c-271">vi를 저장하고 종료합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-271">Save and exit vi.</span></span>

3. <span data-ttu-id="1a30c-272">다음으로 Cosmos DB 암호의 비밀을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-272">Next, add the secret for the Cosmos DB password:</span></span>
    ```shell
    databricks secrets put --scope azure-databricks-job --key "cassandra-password"
    ```

<span data-ttu-id="1a30c-273">실행되면 이 명령이 vi 편집기를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-273">Once executed, this command opens the vi editor.</span></span> <span data-ttu-id="1a30c-274">*Azure 리소스 배포* 섹션의 4단계에서 나오는 **CosmosDb** 출력 섹션의 **비밀** 값을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-274">Enter the **secret** value from the **CosmosDb** output section in step 4 of the *deploy the Azure resources* section.</span></span> <span data-ttu-id="1a30c-275">vi를 저장하고 종료합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-275">Save and exit vi.</span></span>

> [!NOTE]
> <span data-ttu-id="1a30c-276">[Azure Key Vault에서 지원하는 비밀 범위](https://docs.azuredatabricks.net/user-guide/secrets/secret-scopes.html#azure-key-vault-backed-scopes)를 사용하는 경우 범위 이름을 **azure-databricks-job**으로 지정해야 하며 비밀 이름이 위와 동일해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-276">If using an [Azure Key Vault-backed secret scope](https://docs.azuredatabricks.net/user-guide/secrets/secret-scopes.html#azure-key-vault-backed-scopes), the scope must be named **azure-databricks-job** and the secrets must have the exact same names as those above.</span></span>

### <a name="add-the-zillow-neighborhoods-data-file-to-the-databricks-file-system"></a><span data-ttu-id="1a30c-277">Databricks 파일 시스템에 Zillow 환경을 데이터 파일을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-277">Add the Zillow Neighborhoods data file to the Databricks file system</span></span>

1. <span data-ttu-id="1a30c-278">Databricks 파일 시스템에 다음 디렉터리를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-278">Create a directory in the Databricks file system:</span></span>
    ```bash
    dbfs mkdirs dbfs:/azure-databricks-jobs
    ```

2. <span data-ttu-id="1a30c-279">`DataFile` 디렉터리로 이동하고 다음을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-279">Navigate to the `DataFile` directory and enter the following:</span></span>
    ```bash
    dbfs cp ZillowNeighborhoods-NY.zip dbfs:/azure-databricks-jobs
    ```

### <a name="add-the-azure-log-analytics-workspace-id-and-primary-key-to-configuration-files"></a><span data-ttu-id="1a30c-280">구성 파일에 Azure Log Analytics 작업 영역 ID 및 기본 키를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-280">Add the Azure Log Analytics workspace ID and primary key to configuration files</span></span>

<span data-ttu-id="1a30c-281">이 섹션에서는 Log Analytics 작업 영역 ID 및 기본 키가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-281">For this section, you require the Log Analytics workspace ID and primary key.</span></span> <span data-ttu-id="1a30c-282">작업 영역 ID는 *Azure 리소스 배포* 섹션의 4단계에서 나오는 **logAnalytics** 출력 섹션의 **workspaceId** 값입니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-282">The workspace ID is the **workspaceId** value from the **logAnalytics** output section in step 4 of the *deploy the Azure resources* section.</span></span> <span data-ttu-id="1a30c-283">기본 키는 출력 섹션의 **비밀**입니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-283">The primary key is the **secret** from the output section.</span></span>

1. <span data-ttu-id="1a30c-284">log4j 로깅을 구성하려면 `\azure\AzureDataBricksJob\src\main\resources\com\microsoft\pnp\azuredatabricksjob\log4j.properties`를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-284">To configure log4j logging, open `\azure\AzureDataBricksJob\src\main\resources\com\microsoft\pnp\azuredatabricksjob\log4j.properties`.</span></span> <span data-ttu-id="1a30c-285">다음 두 값을 편집합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-285">Edit the following two values:</span></span>
    ```shell
    log4j.appender.A1.workspaceId=<Log Analytics workspace ID>
    log4j.appender.A1.secret=<Log Analytics primary key>
    ```

2. <span data-ttu-id="1a30c-286">사용자 지정 로깅을 구성하려면 `\azure\azure-databricks-monitoring\scripts\metrics.properties`를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-286">To configure custom logging, open `\azure\azure-databricks-monitoring\scripts\metrics.properties`.</span></span> <span data-ttu-id="1a30c-287">다음 두 값을 편집합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-287">Edit the following two values:</span></span>
    ```shell
    *.sink.loganalytics.workspaceId=<Log Analytics workspace ID>
    *.sink.loganalytics.secret=<Log Analytics primary key>
    ```

### <a name="build-the-jar-files-for-the-databricks-job-and-databricks-monitoring"></a><span data-ttu-id="1a30c-288">Databricks 작업 및 Databricks 모니터링에 대한 .jar 파일 빌드</span><span class="sxs-lookup"><span data-stu-id="1a30c-288">Build the .jar files for the Databricks job and Databricks monitoring</span></span>

1. <span data-ttu-id="1a30c-289">Java IDE를 사용하여 루트 디렉터리에 있는 **pom.xml**이라는 Maven 프로젝트 파일을 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-289">Use your Java IDE to import the Maven project file named **pom.xml** located in the root directory.</span></span>

2. <span data-ttu-id="1a30c-290">클린 빌드를 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-290">Perform a clean build.</span></span> <span data-ttu-id="1a30c-291">이 빌드의 출력은 **azure-databricks-job-1.0-SNAPSHOT.jar** 및 **azure-databricks-monitoring-0.9.jar**라는 이름의 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-291">The output of this build is files named **azure-databricks-job-1.0-SNAPSHOT.jar** and **azure-databricks-monitoring-0.9.jar**.</span></span>

### <a name="configure-custom-logging-for-the-databricks-job"></a><span data-ttu-id="1a30c-292">Databricks 작업에 대한 사용자 지정 로깅 구성</span><span class="sxs-lookup"><span data-stu-id="1a30c-292">Configure custom logging for the Databricks job</span></span>

1. <span data-ttu-id="1a30c-293">**Databricks CLI**에 다음 명령을 입력하여 **azure-databricks-monitoring-0.9.jar** 파일을 Databricks 파일 시스템으로 복사합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-293">Copy the **azure-databricks-monitoring-0.9.jar** file to the Databricks file system by entering the following command in the **Databricks CLI**:</span></span>
    ```shell
    databricks fs cp --overwrite azure-databricks-monitoring-0.9.jar dbfs:/azure-databricks-job/azure-databricks-monitoring-0.9.jar
    ```

2. <span data-ttu-id="1a30c-294">다음 명령을 입력하여 `\azure\azure-databricks-monitoring\scripts\metrics.properties`의 사용자 지정 로깅 속성을 Databricks 파일 시스템으로 복사합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-294">Copy the custom logging properties from `\azure\azure-databricks-monitoring\scripts\metrics.properties` to the Databricks file system by entering the following command:</span></span>
    ```shell
    databricks fs cp --overwrite metrics.properties dbfs:/azure-databricks-job/metrics.properties
    ```

3. <span data-ttu-id="1a30c-295">Databricks 클러스터의 이름을 아직 결정하지 않은 경우 지금 결정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-295">While you haven't yet decided on a name for your Databricks cluster, select one now.</span></span> <span data-ttu-id="1a30c-296">클러스터의 Databricks 파일 시스템 경로에 이름을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-296">You'll enter the name below in the Databricks file system path for your cluster.</span></span> <span data-ttu-id="1a30c-297">다음 명령을 입력하여 `\azure\azure-databricks-monitoring\scripts\spark.metrics`의 초기화 스크립트를 Databricks 파일 시스템으로 복사합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-297">Copy the initialization script from `\azure\azure-databricks-monitoring\scripts\spark.metrics` to the Databricks file system by entering the following command:</span></span>
    ```shell
    databricks fs cp --overwrite spark-metrics.sh dbfs:/databricks/init/<cluster-name>/spark-metrics.sh
    ```

### <a name="create-a-databricks-cluster"></a><span data-ttu-id="1a30c-298">Databricks 클러스터 만들기</span><span class="sxs-lookup"><span data-stu-id="1a30c-298">Create a Databricks cluster</span></span>

1. <span data-ttu-id="1a30c-299">Databricks 작업 영역에서 "클러스터"를 클릭한 다음, "클러스터 만들기"를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-299">In the Databricks workspace, click "Clusters", then click "create cluster".</span></span> <span data-ttu-id="1a30c-300">**Databricks 작업에 대한 사용자 지정 로깅 구성** 섹션의 3단계에서 만든 클러스터 이름을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-300">Enter the cluster name you created in step 3 of the **configure custom logging for the Databricks job** section above.</span></span>

2. <span data-ttu-id="1a30c-301">**표준** 클러스터 모드를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-301">Select a **standard** cluster mode.</span></span>

3. <span data-ttu-id="1a30c-302">**Databricks 런타임 버전**을 **4.3(Apache Spark 2.3.1, Scala 2.11 포함)** 으로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-302">Set **Databricks runtime version** to **4.3 (includes Apache Spark 2.3.1, Scala 2.11)**</span></span>

4. <span data-ttu-id="1a30c-303">**Python 버전**을 **2**로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-303">Set **Python version** to **2**.</span></span>

5. <span data-ttu-id="1a30c-304">**드라이버 유형**을 **작업자와 동일**로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-304">Set **Driver Type** to **Same as worker**</span></span>

6. <span data-ttu-id="1a30c-305">**작업자 유형**을 **Standard_DS3_v2**로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-305">Set **Worker Type** to **Standard_DS3_v2**.</span></span>

7. <span data-ttu-id="1a30c-306">**최소 작업자**를 **2**로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-306">Set **Min Workers** to **2**.</span></span>

8. <span data-ttu-id="1a30c-307">**자동 크기 조정 사용**의 선택을 취소합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-307">Deselect **Enable autoscaling**.</span></span>

9. <span data-ttu-id="1a30c-308">**자동 종료** 대화 상자 아래에서 **Init 스크립트**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-308">Below the **Auto Termination** dialog box, click on **Init Scripts**.</span></span>

10. <span data-ttu-id="1a30c-309">**dbfs:/databricks/init/<cluster-name>/spark-metrics.sh**를 입력하고, <cluster-name>을 1단계에서 만든 클러스터 이름으로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-309">Enter **dbfs:/databricks/init/<cluster-name>/spark-metrics.sh**, substituting the cluster name created in step 1 for <cluster-name>.</span></span>

11. <span data-ttu-id="1a30c-310">**추가** 단추를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-310">Click the **Add** button.</span></span>

12. <span data-ttu-id="1a30c-311">**클러스터 만들기** 단추를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-311">Click the **Create Cluster** button.</span></span>

### <a name="create-a-databricks-job"></a><span data-ttu-id="1a30c-312">Databricks 작업 만들기</span><span class="sxs-lookup"><span data-stu-id="1a30c-312">Create a Databricks job</span></span>

1. <span data-ttu-id="1a30c-313">Databricks 작업 영역에서 "작업", "작업 만들기"를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-313">In the Databricks workspace, click "Jobs", "create job".</span></span>

2. <span data-ttu-id="1a30c-314">작업 이름을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-314">Enter a job name.</span></span>

3. <span data-ttu-id="1a30c-315">"jar 설정"을 클릭하면 "실행할 JAR 업로드" 대화 상자가 열립니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-315">Click "set jar", this opens the "Upload JAR to Run" dialog box.</span></span>

4. <span data-ttu-id="1a30c-316">**Databricks 작업에 사용할 .jar 빌드** 섹션에서 만든 **azure-databricks-job-1.0-SNAPSHOT.jar** 파일을 **업로드할 JAR를 여기에 드롭** 상자로 끌어다 놓습니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-316">Drag the **azure-databricks-job-1.0-SNAPSHOT.jar** file you created in the **build the .jar for the Databricks job** section to the **Drop JAR here to upload** box.</span></span>

5. <span data-ttu-id="1a30c-317">**기본 클래스** 필드에 **com.microsoft.pnp.TaxiCabReader**를 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-317">Enter **com.microsoft.pnp.TaxiCabReader** in the **Main Class** field.</span></span>

6. <span data-ttu-id="1a30c-318">인수 필드에 다음을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-318">In the arguments field, enter the following:</span></span>
    ```shell
    -n jar:file:/dbfs/azure-databricks-jobs/ZillowNeighborhoods-NY.zip!/ZillowNeighborhoods-NY.shp --taxi-ride-consumer-group taxi-ride-eh-cg --taxi-fare-consumer-group taxi-fare-eh-cg --window-interval "1 minute" --cassandra-host <Cosmos DB Cassandra host name from above>
    ```

7. <span data-ttu-id="1a30c-319">다음 단계에 따라 종속 라이브러리를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-319">Install the dependent libraries by following these steps:</span></span>

    1. <span data-ttu-id="1a30c-320">Databricks 사용자 인터페이스에서 **홈** 단추를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-320">In the Databricks user interface, click on the **home** button.</span></span>

    2. <span data-ttu-id="1a30c-321">**사용자** 드롭다운 목록에서 해당하는 사용자 계정 이름을 클릭하여 계정 작업 영역 설정을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-321">In the **Users** drop-down, click on your user account name to open your account workspace settings.</span></span>

    3. <span data-ttu-id="1a30c-322">계정 이름 옆에 있는 드롭다운 화살표를 클릭하고, **만들기**를 클릭하고, **라이브러리**를 클릭하여 **새 라이브러리** 대화 상자를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-322">Click on the drop-down arrow beside your account name, click on **create**, and click on **Library** to open the **New Library** dialog.</span></span>

    4. <span data-ttu-id="1a30c-323">**원본** 드롭다운 컨트롤에서 **Maven 좌표**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-323">In the **Source** drop-down control, select **Maven Coordinate**.</span></span>

    5. <span data-ttu-id="1a30c-324">**Maven 아티팩트 설치** 제목 아래의 **좌표** 텍스트 상자에 `com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.5`를 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-324">Under the **Install Maven Artifacts** heading, enter `com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.5` in the **Coordinate** text box.</span></span>

    6. <span data-ttu-id="1a30c-325">**라이브러리 만들기**를 클릭하여 **아티팩트** 창을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-325">Click on **Create Library** to open the **Artifacts** window.</span></span>

    7. <span data-ttu-id="1a30c-326">**실행 중인 클러스터의 상태** 아래에서 **모든 클러스터에 자동 연결** 확인란을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-326">Under **Status on running clusters** check the **Attach automatically to all clusters** checkbox.</span></span>

    8. <span data-ttu-id="1a30c-327">`com.microsoft.azure.cosmosdb:azure-cosmos-cassandra-spark-helper:1.0.0` Maven 좌표에 대해 1-7단계를 반복합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-327">Repeat steps 1 - 7 for the `com.microsoft.azure.cosmosdb:azure-cosmos-cassandra-spark-helper:1.0.0` Maven coordinate.</span></span>

    9. <span data-ttu-id="1a30c-328">`org.geotools:gt-shapefile:19.2` Maven 좌표에 대해 1-6단계를 반복합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-328">Repeat steps 1 - 6 for the `org.geotools:gt-shapefile:19.2` Maven coordinate.</span></span>

    10. <span data-ttu-id="1a30c-329">**고급 옵션**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-329">Click on **Advanced Options**.</span></span>

    11. <span data-ttu-id="1a30c-330">**리포지토리** 텍스트 상자에 `http://download.osgeo.org/webdav/geotools/`를 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-330">Enter `http://download.osgeo.org/webdav/geotools/` in the **Repository** text box.</span></span>

    12. <span data-ttu-id="1a30c-331">**라이브러리 만들기**를 클릭하여 **아티팩트** 창을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-331">Click **Create Library** to open the **Artifacts** window.</span></span> 

    13. <span data-ttu-id="1a30c-332">**실행 중인 클러스터의 상태** 아래에서 **모든 클러스터에 자동 연결** 확인란을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-332">Under **Status on running clusters** check the **Attach automatically to all clusters** checkbox.</span></span>

8. <span data-ttu-id="1a30c-333">7단계에서 추가된 종속 라이브러리를 6단계 마지막 부분에서 만든 작업에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-333">Add the dependent libraries added in step 7 to the job created at the end of step 6:</span></span>

    1. <span data-ttu-id="1a30c-334">Azure Databricks 작업 영역에서 **작업**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-334">In the Azure Databricks workspace, click on **Jobs**.</span></span>

    2. <span data-ttu-id="1a30c-335">**Databricks 작업 만들기** 섹션의 2단계에서 만든 작업 이름을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-335">Click on the job name created in step 2 of the **create a Databricks job** section.</span></span>

    3. <span data-ttu-id="1a30c-336">**종속 라이브러리** 섹션 옆에서 **추가**를 클릭하여 **종속 라이브러리 추가** 대화 상자를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-336">Beside the **Dependent Libraries** section, click on **Add** to open the **Add Dependent Library** dialog.</span></span>

    4. <span data-ttu-id="1a30c-337">**다음 위치의 라이브러리** 아래에서 **작업 영역**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-337">Under **Library From** select **Workspace**.</span></span>

    5. <span data-ttu-id="1a30c-338">**사용자**를 클릭하고 사용자 이름을 클릭한 다음, `azure-eventhubs-spark_2.11:2.3.5`를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-338">Click on **users**, then your username, then click on `azure-eventhubs-spark_2.11:2.3.5`.</span></span>

    6. <span data-ttu-id="1a30c-339">**확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-339">Click **OK**.</span></span>

    7. <span data-ttu-id="1a30c-340">`spark-cassandra-connector_2.11:2.3.1` 및 `gt-shapefile:19.2`에 대해 1-6단계를 반복합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-340">Repeat steps 1 - 6 for `spark-cassandra-connector_2.11:2.3.1` and `gt-shapefile:19.2`.</span></span>

9. <span data-ttu-id="1a30c-341">**클러스터:** 옆에 있는 **편집**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-341">Beside **Cluster:**, click on **Edit**.</span></span> <span data-ttu-id="1a30c-342">그러면 **클러스터 구성** 대화 상자가 열립니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-342">This opens the **Configure Cluster** dialog.</span></span> <span data-ttu-id="1a30c-343">**클러스터 유형** 드롭다운 목록에서 **기존 클러스터**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-343">In the **Cluster Type** drop-down, select **Existing Cluster**.</span></span> <span data-ttu-id="1a30c-344">**클러스터 선택** 드롭다운 목록에서, **Databricks 클러스터 만들기** 섹션에서 만든 클러스터를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-344">In the **Select Cluster** drop-down, select the cluster created the **create a Databricks cluster** section.</span></span> <span data-ttu-id="1a30c-345">**확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-345">Click **confirm**.</span></span>

10. <span data-ttu-id="1a30c-346">**지금 실행**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-346">Click **run now**.</span></span>

### <a name="run-the-data-generator"></a><span data-ttu-id="1a30c-347">데이터 생성기 실행</span><span class="sxs-lookup"><span data-stu-id="1a30c-347">Run the data generator</span></span>

1. <span data-ttu-id="1a30c-348">GitHub 리포지토리의 `onprem` 디렉터리로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-348">Navigate to the directory named `onprem` in the GitHub repository.</span></span>

2. <span data-ttu-id="1a30c-349">**main.env** 파일의 값을 다음과 같이 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-349">Update the values in the file **main.env** as follows:</span></span>

    ```shell
    RIDE_EVENT_HUB=[Connection string for the taxi-ride event hub]
    FARE_EVENT_HUB=[Connection string for the taxi-fare event hub]
    RIDE_DATA_FILE_PATH=/DataFile/FOIL2013
    MINUTES_TO_LEAD=0
    PUSH_RIDE_DATA_FIRST=false
    ```
    <span data-ttu-id="1a30c-350">택시 승객 이벤트 허브의 연결 문자열은 *Azure 리소스 배포* 섹션의 4단계에서 나오는 **eventHubs** 출력 섹션의 **taxi-ride-eh** 값입니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-350">The connection string for the taxi-ride event hub is the **taxi-ride-eh** value from the **eventHubs** output section in step 4 of the *deploy the Azure resources* section.</span></span> <span data-ttu-id="1a30c-351">택시 요금 이벤트 허브의 연결 문자열은 *Azure 리소스 배포* 섹션의 4단계에서 나오는 **eventHubs** 출력 섹션의 **taxi-fare-eh** 값입니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-351">The connection string for the taxi-fare event hub the **taxi-fare-eh** value from the **eventHubs** output section in step 4 of the *deploy the Azure resources* section.</span></span>

3. <span data-ttu-id="1a30c-352">다음 명령을 실행하여 Docker 이미지를 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-352">Run the following command to build the Docker image.</span></span>

    ```bash
    docker build --no-cache -t dataloader .
    ```

4. <span data-ttu-id="1a30c-353">부모 디렉터리로 다시 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-353">Navigate back to the parent directory.</span></span>

    ```bash
    cd ..
    ```

5. <span data-ttu-id="1a30c-354">다음 명령을 실행하여 Docker 이미지를 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-354">Run the following command to run the Docker image.</span></span>

    ```bash
    docker run -v `pwd`/DataFile:/DataFile --env-file=onprem/main.env dataloader:latest
    ```

<span data-ttu-id="1a30c-355">출력은 다음과 비슷합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-355">The output should look like the following:</span></span>

```
Created 10000 records for TaxiFare
Created 10000 records for TaxiRide
Created 20000 records for TaxiFare
Created 20000 records for TaxiRide
Created 30000 records for TaxiFare
...
```

<span data-ttu-id="1a30c-356">Databricks 작업이 올바르게 실행 중인지 확인하려면 Azure Portal을 열고 Cosmos DB 데이터베이스로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-356">To verify the Databricks job is running correctly, open the Azure portal and navigate to the Cosmos DB database.</span></span> <span data-ttu-id="1a30c-357">**데이터 탐색기** 블레이드를 열고 **택시 레코드** 테이블의 데이터를 검사합니다.</span><span class="sxs-lookup"><span data-stu-id="1a30c-357">Open the **Data Explorer** blade and examine the data in the **taxi records** table.</span></span> 

<span data-ttu-id="1a30c-358">[1] <span id="note1">Donovan, Brian; Work, Dan(2016): 뉴욕시 택시 운행 데이터(2010-2013).</span><span class="sxs-lookup"><span data-stu-id="1a30c-358">[1] <span id="note1">Donovan, Brian; Work, Dan (2016): New York City Taxi Trip Data (2010-2013).</span></span> <span data-ttu-id="1a30c-359">University of Illinois at Urbana-Champaign</span><span class="sxs-lookup"><span data-stu-id="1a30c-359">University of Illinois at Urbana-Champaign.</span></span> <span data-ttu-id="1a30c-360">https://doi.org/10.13012/J8PN93H8</span><span class="sxs-lookup"><span data-stu-id="1a30c-360">https://doi.org/10.13012/J8PN93H8</span></span>
