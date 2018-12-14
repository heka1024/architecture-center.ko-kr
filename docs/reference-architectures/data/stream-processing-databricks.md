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
# <a name="create-a-stream-processing-pipeline-with-azure-databricks"></a>Azure Databricks를 사용하는 스트림 처리 파이프라인 만들기

이 참조 아키텍처는 엔드투엔드 [스트림 처리](/azure/architecture/data-guide/big-data/real-time-processing) 파이프라인을 보여줍니다. 이 유형의 파이프라인은 수집, 처리, 저장, 분석 및 보고의 4단계로 구성됩니다. 이 참조 아키텍처의 경우 파이프라인은 실시간으로 두 원본에서 데이터를 수집하고, 각 스트림의 관련 레코드에 대해 조인을 수행하고, 결과를 보강하고, 평균을 계산합니다. 추가 분석을 위해 결과가 저장됩니다. [**이 솔루션을 배포합니다**](#deploy-the-solution).

![Azure Databricks를 사용하는 스트림 처리의 참조 아키텍처](./images/stream-processing-databricks.png)

**시나리오**: 한 택시 회사에서 각 택시 운행 데이터를 수집합니다. 이 시나리오의 경우 두 개의 별도 장치가 데이터를 전송하고 있다고 가정합니다. 택시에는 각 승객 &mdash; 기간, 거리, 승차 및 하차 위치에 대한 정보를 전송하는 미터가 있습니다. 별도 장치는 고객의 지불을 수락하고 요금에 대한 데이터를 보냅니다. 택시 회사는 승객 추세를 파악하기 위해 각 환경의 마일당 평균 팁을 실시간으로 계산하려고 합니다.

## <a name="architecture"></a>아키텍처

이 아키텍처는 다음 구성 요소로 구성됩니다.

**데이터 원본**. 이 아키텍처에는 실시간으로 데이터 스트림을 생성하는 두 개의 데이터 원본이 있습니다. 첫 번째 스트림에는 승객 정보가 포함되고 두 번째 스트림에는 요금 정보가 포함됩니다. 참조 아키텍처에는 정적 파일 집합을 읽고 Event Hubs에 데이터를 푸시하는 시뮬레이트된 데이터 생성기가 포함됩니다. 실제 애플리케이션의 데이터 원본은 택시에 설치되는 디바이스입니다.

**Azure Event Hubs** - [Event Hubs](/azure/event-hubs/)는 이벤트 수집 서비스입니다. 이 아키텍처는 각 데이터 원본에 대해 하나씩 두 개의 이벤트 허브 인스턴스를 사용합니다. 각 데이터 원본은 연결된 이벤트 허브에 데이터 스트림을 보냅니다.

**Azure Databricks**. [Databricks](/azure/azure-databricks/)는 Microsoft Azure Cloud Services 플랫폼에 대해 최적화된 Apache Spark 기반 분석 플랫폼입니다. Databricks는 Databricks 파일 시스템에 저장된 환경 데이터를 사용하여 택시 운행과 요금 데이터 간의 상관 관계를 지정하고 상관 관계 데이터를 보강하는 데 사용됩니다.

**Cosmos DB** Azure Databricks 작업의 출력은 Cassandra API를 사용하여 [Cosmos DB](/azure/cosmos-db/)에 기록되는 일련의 레코드입니다. Cassandra API를 사용하는 이유는 시계열 데이터 모델링을 지원하기 때문입니다.

**Azure Log Analytics**. [Azure Monitor](/azure/monitoring-and-diagnostics/)를 통해 수집되는 애플리케이션 로그 데이터는 [Log Analytics 작업 영역](/azure/log-analytics)에 저장됩니다. Log Analytics 쿼리는 메트릭을 분석 및 시각화하고 로그 메시지를 검사하여 애플리케이션 내부의 문제를 식별하는 데 사용할 수 있습니다.

## <a name="data-ingestion"></a>데이터 수집

데이터 원본을 시뮬레이션하기 위해 이 참조 아키텍처는 [뉴욕시 택시 데이터](https://uofi.app.box.com/v/NYCtaxidata/folder/2332218797) 데이터 세트<sup>[[1]](#note1)</sup>를 사용합니다. 이 데이터 세트에는 4년(2010&ndash;2013) 간의 뉴욕시 택시 운행 데이터가 포함됩니다. 두 가지 유형의 레코드를 포함합니다. 그것은 바로 승객 데이터와 요금 데이터입니다. 승객 데이터에는 여정 기간, 여정 거리 및 승차 및 하차 위치가 포함됩니다. 요금 데이터에는 요금, 세금 및 팁 금액이 포함됩니다. 레코드 형식 모두의 공통 필드에는 등록 번호, 택시 라이선스 및 공급 업체 ID가 포함됩니다. 이러한 세 필드는 택시와 드라이버를 고유하게 식별합니다. 데이터가 CSV 형식으로 저장됩니다.

데이터 생성기는 레코드를 읽고 Azure Event Hubs로 전송하는 .NET Core 응용 프로그램입니다. 생성기는 승객 데이터를 JSON 형식으로 보내고, 요금 데이터를 CSV 형식으로 전송합니다.

Event Hubs는 [파티션](/azure/event-hubs/event-hubs-features#partitions)을 사용하여 데이터를 분할합니다. 파티션을 사용하면 소비자가 각 파티션을 병렬로 읽을 수 있습니다. Event Hubs에 데이터를 보낼 때 파티션 키를 명시적으로 지정할 수 있습니다. 그렇지 않으면 레코드는 라운드 로빈 방식으로 파티션에 할당됩니다.

이 시나리오에서 승객 데이터 및 요금 데이터는 지정된 택시에 대해 동일한 파티션 ID를 사용해야 합니다. 그러면 Databricks가 두 스트림의 상관 관계를 지정할 때 일정 수준의 병렬 처리를 적용할 수 있습니다. 승객 데이터의 *n* 파티션에 있는 레코드는 요금 데이터의 *n* 파티션에 있는 레코드와 일치합니다.

![Azure Databricks 및 Event Hubs를 사용하는 스트림 처리 다이어그램](./images/stream-processing-databricks-eh.png)

데이터 생성기에서 두 레코드 형식에 대한 공통 데이터 모델에는 `Medallion`, `HackLicense` 및 `VendorId`의 연결인 `PartitionKey` 속성이 있습니다.

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

이 속성을 사용하여 Event Hubs로 전송하는 경우 명시적인 파티션 키를 제공합니다.

```csharp
using (var client = pool.GetObject())
{
    return client.Value.SendAsync(new EventData(Encoding.UTF8.GetBytes(
        t.GetData(dataFormat))), t.PartitionKey);
}
```

### <a name="event-hubs"></a>Event Hubs

Event Hubs의 처리량 용량은 [처리량 단위](/azure/event-hubs/event-hubs-features#throughput-units)로 제어됩니다. [자동 팽창](/azure/event-hubs/event-hubs-auto-inflate)을 사용하도록 설정하여 이벤트 허브를 자동 크기 조정할 수 있습니다. 그러면 트래픽에 따라 처리량 단위를 구성된 최댓값까지 자동으로 크기 조정합니다.

## <a name="stream-processing"></a>스트림 처리

Azure Databricks에서 데이터 처리는 작업을 통해 수행됩니다. 작업은 클러스터에 할당되고 클러스터에서 실행됩니다. 작업은 Java로 작성된 사용자 지정 코드이거나 Spark [Notebook](https://docs.databricks.com/user-guide/notebooks/index.html)입니다.

이 참조 아키텍처의 작업은 클래스가 Java 및 Scala로 작성된 Java 아카이브입니다. Databricks 작업의 Java 아카이브를 지정할 때 Databricks 클러스터의 실행에 대한 클래스가 지정됩니다. 여기서 **com.microsoft.pnp.TaxiCabReader** 클래스의 **주** 메서드에는 데이터 처리 논리가 포함됩니다.

### <a name="reading-the-stream-from-the-two-event-hub-instances"></a>두 이벤트 허브 인스턴스에서 스트림 읽기

데이터 처리 논리는 [Spark 구조적 스트리밍](https://spark.apache.org/docs/2.1.2/structured-streaming-programming-guide.html)을 사용하여 두 Azure 이벤트 허브 인스턴스에서 스트림을 읽습니다.

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

### <a name="enriching-the-data-with-the-neighborhood-information"></a>환경 정보를 사용하여 데이터 보강

승객 데이터에는 승차 및 하차 위치의 위도와 경도 좌표가 포함됩니다. 이러한 좌표는 유용하기는 하지만 분석에 쉽게 사용하기 어렵습니다. 따라서 [셰이프 파일](https://en.wikipedia.org/wiki/Shapefile)에서 읽은 환경 데이터를 사용하여 이 데이터를 보강합니다.

셰이프 파일 형식은 이진이고 간단하게 구문 분석할 수 없지만, [GeoTools](http://geotools.org/) 라이브러리에서 셰이프 파일 형식을 사용하는 지리 공간 데이터를 분석할 수 있는 도구를 제공합니다. 이 라이브러리는 **com.microsoft.pnp.GeoFinder** 클래스에 사용되어 승차 및 하차 좌표를 기준으로 환경 이름을 확인합니다.

```scala
val neighborhoodFinder = (lon: Double, lat: Double) => {
      NeighborhoodFinder.getNeighborhood(lon, lat).get()
    }
```

### <a name="joining-the-ride-and-fare-data"></a>승객 및 요금 데이터 조인

먼저 승객 및 요금 데이터가 변환됩니다.

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

그런 다음, 승객 데이터가 요금 데이터에 조인됩니다.

```scala
val mergedTaxiTrip = rides.join(fares, Seq("medallion", "hackLicense", "vendorId", "pickupTime"))
```

### <a name="processing-the-data-and-inserting-into-cosmos-db"></a>데이터를 처리하여 Cosmos DB에 삽입

지정된 시간 간격마다 각 환경의 평균 요금이 계산됩니다.

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

계산된 요금은 Cosmos DB에 삽입됩니다.

```scala
maxAvgFarePerNeighborhood
      .writeStream
      .queryName("maxAvgFarePerNeighborhood_cassandra_insert")
      .outputMode(OutputMode.Append())
      .foreach(new CassandraSinkForeach(connector))
      .start()
      .awaitTermination()
```

## <a name="security-considerations"></a>보안 고려 사항

Azure Database 작업 영역에 대한 액세스는 [관리자 콘솔](https://docs.databricks.com/administration-guide/admin-settings/index.html)을 사용하여 제어됩니다. 관리자 콘솔에는 사용자를 추가하고, 사용자 권한을 관리하고, Single Sign-On을 설정하는 기능이 포함되어 있습니다. 관리자 콘솔을 통해 작업 영역, 클러스터, 작업 및 테이블에 대한 액세스 제어를 설정할 수도 있습니다.

### <a name="managing-secrets"></a>암호 관리

Azure Databricks에는 연결 문자열, 액세스 키, 사용자 이름 및 암호를 포함하여 비밀을 저장하는 데 사용되는 [비밀 저장소](https://docs.azuredatabricks.net/user-guide/secrets/index.html)가 포함되어 있습니다. Azure Databricks 비밀 저장소의 비밀은 **범위**에 따라 분할됩니다.

```bash
databricks secrets create-scope --scope "azure-databricks-job"
```

비밀은 범위 수준에서 추가됩니다.

```bash
databricks secrets put --scope "azure-databricks-job" --key "taxi-ride"
```

> [!NOTE]
> 기본 Azure Databricks 범위 대신 Azure Key Vault에서 지원되는 범위를 사용할 수 있습니다. 자세한 내용은 [Azure Key Vault에서 지원하는 범위](https://docs.azuredatabricks.net/user-guide/secrets/secret-scopes.html#azure-key-vault-backed-scopes)를 참조하세요.

코드에서 비밀은 Azure Databricks [비밀 유틸리티](https://docs.databricks.com/user-guide/dev-tools/dbutils.html#secrets-utilities)를 통해 액세스됩니다.

## <a name="monitoring-considerations"></a>모니터링 고려 사항

Azure Databricks는 Apache Spark를 기반으로 하며, 둘 다 로깅의 표준 라이브러리로 [log4j](https://logging.apache.org/log4j/2.x/)를 사용합니다. Apache Spark에서 제공하는 기본 로깅 외에도, 이 참조 아키텍처는 [Azure Log Analytics](/azure/log-analytics/)로 로그 및 메트릭을 전송합니다.

**com.microsoft.pnp.TaxiCabReader** 클래스는 **log4j.properties** 파일의 값을 사용하여 Azure Log Analytics로 로그를 보내도록 Apache Spark 로깅 시스템을 구성합니다. Apache Spark 로거 메시지는 문자열이지만, Azure Log Analytics는 로그 메시지 형식으로 JSON을 요구합니다. **com.microsoft.pnp.log4j.LogAnalyticsAppender** 클래스는 이러한 메시지를 JSON으로 변환합니다.

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

**com.microsoft.pnp.TaxiCabReader** 클래스가 승객 및 요금 메시지를 처리할 때 둘 중 하나의 형식이 잘못되어 유효하지 않을 수 있습니다. 프로덕션 환경에서는 신속히 문제를 해결하여 데이터 손실을 방지할 수 있도록 이처럼 잘못된 형식의 메시지를 분석하여 데이터 원본의 문제를 파악하는 것이 중요합니다. **com.microsoft.pnp.TaxiCabReader** 클래스는 형식이 잘못된 요금 및 승객 레코드 수를 추적하는 Apache Spark 누적기를 등록합니다.

```scala
    @transient val appMetrics = new AppMetrics(spark.sparkContext)
    appMetrics.registerGauge("metrics.malformedrides", AppAccumulators.getRideInstance(spark.sparkContext))
    appMetrics.registerGauge("metrics.malformedfares", AppAccumulators.getFareInstance(spark.sparkContext))
    SparkEnv.get.metricsSystem.registerSource(appMetrics)
```

Apache Spark는 Dropwizard 라이브러리를 사용하여 메트릭을 보내며, 네이티브 Dropwizard 메트릭 필드 중 일부는 Azure Log Analytics와 호환되지 않습니다. 이러한 이유로 이 참조 아키텍처에는 사용자 지정 Dropwizard 싱크 및 보고자가 포함되어 있습니다. 사용자 지정 Dropwizard 싱크 및 보고자는 Azure Log Analytics에서 예상하는 형식으로 메트릭을 포맷합니다. Apache Spark가 메트릭을 보고할 때 잘못된 승객 및 요금 데이터에 대한 사용자 지정 메트릭도 전송됩니다.

Azure Log Analytics 작업 영역에 기록할 마지막 메트릭은 Spark 구조적 스트림 작업 진행률의 누적 진행률입니다. 이 작업은 **com.microsoft.pnp.StreamingMetricsListener** 클래스에 구현된 사용자 지정 StreamingQuery 수신기를 사용하여 수행됩니다. 이 클래스는 작업이 실행될 때 Apache Spark 세션에 등록됩니다.

```scala
spark.streams.addListener(new StreamingMetricsListener())
```

StreamingMetricsListener의 메서드는 구조적 스트리밍 이벤트가 발생할 때마다 Apache Spark 런타임에 의해 호출되어 Azure Log Analytics 작업 영역으로 메시지 및 메트릭을 전송합니다. 작업 영역에서 다음 쿼리를 사용하여 애플리케이션을 모니터링할 수 있습니다.

### <a name="latency-and-throughput-for-streaming-queries"></a>스트리밍 쿼리의 대기 시간 및 처리량

```shell
taxijob_CL
| where TimeGenerated > startofday(datetime(<date>)) and TimeGenerated < endofday(datetime(<date>))
| project  mdc_inputRowsPerSecond_d, mdc_durationms_triggerExecution_d
| render timechart
```

### <a name="exceptions-logged-during-stream-query-execution"></a>스트림 쿼리를 실행하는 동안 기록된 예외

```shell
taxijob_CL
| where TimeGenerated > startofday(datetime(<date>)) and TimeGenerated < endofday(datetime(<date>))
| where Level contains "Error"
```

### <a name="accumulation-of-malformed-fare-and-ride-data"></a>잘못된 형식의 요금 및 승객 데이터의 누적

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

### <a name="job-execution-to-trace-resiliency"></a>복원력을 추적하는 작업 실행

```shell
SparkMetric_CL
| where TimeGenerated > startofday(datetime(<date>)) and TimeGenerated < endofday(datetime(<date>))
| render timechart
| where name_s contains "driver.DAGScheduler.job.allJobs"
```

## <a name="deploy-the-solution"></a>솔루션 배포

이 참조 아키텍처에 대한 배포는 [GitHub](https://github.com/mspnp/azure-databricks-streaming-analytics)에서 사용할 수 있습니다.

### <a name="prerequisites"></a>필수 조건

1. [Azure Databricks를 사용한 스트림 처리](https://github.com/mspnp/azure-databricks-streaming-analytics) GitHub 리포지토리를 복제, 포크 또는 다운로드합니다.

2. [Docker](https://www.docker.com/)를 설치하여 데이터 생성기를 실행합니다.

3. [Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest)을 설치합니다.

4. [Databricks CLI](https://docs.databricks.com/user-guide/dev-tools/databricks-cli.html)를 설치합니다.

5. 명령 프롬프트, bash 프롬프트 또는 PowerShell 프롬프트에서 다음과 같은 Azure 계정에 로그인합니다.
    ```shell
    az login
    ```
6. 다음 리소스를 사용하여 Java IDE를 설치합니다.
    - JDK 1.8
    - Scala SDK 2.11
    - Maven 3.5.4

### <a name="download-the-new-york-city-taxi-and-neighborhood-data-files"></a>뉴욕시 택시 및 환경 데이터 파일을 다운로드

1. 로컬 파일 시스템에서 복제된 Github 리포지토리의 루트에 `DataFile`이라는 디렉터리를 만듭니다.

2. 웹 브라우저를 열고 https://uofi.app.box.com/v/NYCtaxidata/folder/2332219935로 이동합니다.

3. 이 페이지에서 **다운로드**를 클릭하여 해당 연도에 대한 모든 택시 데이터의 zip 파일을 다운로드합니다.

4. `DataFile` 디렉터리에 zip 파일의 압축을 풉니다.

    > [!NOTE]
    > 이 zip 파일에는 다른 zip 파일이 포함됩니다. 자식 zip 파일의 압축을 풀지 마십시오.

    디렉터리 구조는 다음과 같이 표시됩니다.

    ```shell
    /DataFile
        /FOIL2013
            trip_data_1.zip
            trip_data_2.zip
            trip_data_3.zip
            ...
    ```

5. 웹 브라우저를 열고 https://www.zillow.com/howto/api/neighborhood-boundaries.htm로 이동합니다.

6. **뉴욕 환경 경계**를 클릭하여 파일을 다운로드합니다.

7. 브라우저의 **다운로드** 디렉터리에서 `DataFile` 디렉터리로 **ZillowNeighborhoods NY.zip** 파일을 복사합니다.

### <a name="deploy-the-azure-resources"></a>Azure 리소스 배포

1. 셸 또는 Windows 명령 프롬프트에서 다음 명령을 실행하고 로그인 프롬프트를 따릅니다.

    ```bash
    az login
    ```

2. GitHub 리포지토리의 `azure` 폴더로 이동합니다.

    ```bash
    cd azure
    ```

3. 다음 명령을 실행하여 Azure 리소스를 배포합니다.

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

4. 작업이 완료되면 배포의 출력이 콘솔에 기록됩니다. 다음 JSON에 대한 출력을 검색합니다.

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

이러한 값은 다음 섹션에서 Databricks 비밀에 추가될 비밀입니다. 다음 섹션에서 추가할 때까지 비밀을 안전하게 보관하세요.

### <a name="add-a-cassandra-table-to-the-cosmos-db-account"></a>Cosmos DB 계정에 Cassandra 테이블 추가

1. Azure Portal에서, 이전에 **Azure 리소스 배포** 섹션에서 만든 리소스 그룹으로 이동 합니다. **Azure Cosmos DB 계정**을 클릭합니다. Cassandra API를 사용하여 테이블을 만듭니다.

2. **개요** 블레이드에서 **테이블 추가**를 클릭합니다.

3. **테이블 추가** 블레이드가 열리면 **키스페이스 이름** 텍스트 상자에 `newyorktaxi`를 입력합니다.

4. **테이블을 만들려면 CQL 명령을 입력** 섹션에서, `newyorktaxi` 옆에 있는 텍스트 상자에 `neighborhoodstats`를 입력합니다.

5. 아래 텍스트 상자에 다음을 입력합니다.
    ```shell
    (neighborhood text, window_end timestamp, number_of_rides bigint,total_fare_amount double, primary key(neighborhood, window_end))
    ```
6. **처리량(1,000-1,000,000RU/s)** 텍스트 상자에 `4000` 값을 입력합니다.

7. **확인**을 클릭합니다.

### <a name="add-the-databricks-secrets-using-the-databricks-cli"></a>Databricks CLI를 사용하여 Databricks 비밀 추가

먼저 EventHub의 비밀을 입력합니다.

1. 필수 구성 요소의 2단계에서 설치한 **Azure Databricks CLI**를 사용하여 Azure Databricks 비밀 범위를 만듭니다.
    ```shell
    databricks secrets create-scope --scope "azure-databricks-job"
    ```
2. 택시 승객 EventHub의 비밀을 추가합니다.
    ```shell
    databricks secrets put --scope "azure-databricks-job" --key "taxi-ride"
    ```
    실행되면 이 명령이 vi 편집기를 엽니다. *Azure 리소스 배포* 섹션의 4단계에서 나오는 **eventHubs** 출력 섹션의 **taxi-ride-eh** 값을 입력합니다. vi를 저장하고 종료합니다.

3. 택시 요금 EventHub의 비밀을 추가합니다.
    ```shell
    databricks secrets put --scope "azure-databricks-job" --key "taxi-fare"
    ```
    실행되면 이 명령이 vi 편집기를 엽니다. *Azure 리소스 배포* 섹션의 4단계에서 나오는 **eventHubs** 출력 섹션의 **taxi-fare-eh** 값을 입력합니다. vi를 저장하고 종료합니다.

다음으로 Cosmos DB의 비밀을 입력합니다.

1. Azure Portal에서, **Azure 리소스 배포** 섹션의 3단계에서 지정한 리소스 그룹으로 이동합니다. Azure Cosmos DB 계정을 클릭합니다.

2. **Azure Databricks CLI**를 사용하여 Cosmos DB 사용자 이름의 비밀을 추가합니다.
    ```shell
    databricks secrets put --scope azure-databricks-job --key "cassandra-username"
    ```
실행되면 이 명령이 vi 편집기를 엽니다. *Azure 리소스 배포* 섹션의 4단계에서 나오는 **CosmosDb** 출력 섹션의 **사용자 이름** 값을 입력합니다. vi를 저장하고 종료합니다.

3. 다음으로 Cosmos DB 암호의 비밀을 추가합니다.
    ```shell
    databricks secrets put --scope azure-databricks-job --key "cassandra-password"
    ```

실행되면 이 명령이 vi 편집기를 엽니다. *Azure 리소스 배포* 섹션의 4단계에서 나오는 **CosmosDb** 출력 섹션의 **비밀** 값을 입력합니다. vi를 저장하고 종료합니다.

> [!NOTE]
> [Azure Key Vault에서 지원하는 비밀 범위](https://docs.azuredatabricks.net/user-guide/secrets/secret-scopes.html#azure-key-vault-backed-scopes)를 사용하는 경우 범위 이름을 **azure-databricks-job**으로 지정해야 하며 비밀 이름이 위와 동일해야 합니다.

### <a name="add-the-zillow-neighborhoods-data-file-to-the-databricks-file-system"></a>Databricks 파일 시스템에 Zillow 환경을 데이터 파일을 추가합니다.

1. Databricks 파일 시스템에 다음 디렉터리를 만듭니다.
    ```bash
    dbfs mkdirs dbfs:/azure-databricks-jobs
    ```

2. `DataFile` 디렉터리로 이동하고 다음을 입력합니다.
    ```bash
    dbfs cp ZillowNeighborhoods-NY.zip dbfs:/azure-databricks-jobs
    ```

### <a name="add-the-azure-log-analytics-workspace-id-and-primary-key-to-configuration-files"></a>구성 파일에 Azure Log Analytics 작업 영역 ID 및 기본 키를 추가합니다.

이 섹션에서는 Log Analytics 작업 영역 ID 및 기본 키가 필요합니다. 작업 영역 ID는 *Azure 리소스 배포* 섹션의 4단계에서 나오는 **logAnalytics** 출력 섹션의 **workspaceId** 값입니다. 기본 키는 출력 섹션의 **비밀**입니다.

1. log4j 로깅을 구성하려면 `\azure\AzureDataBricksJob\src\main\resources\com\microsoft\pnp\azuredatabricksjob\log4j.properties`를 엽니다. 다음 두 값을 편집합니다.
    ```shell
    log4j.appender.A1.workspaceId=<Log Analytics workspace ID>
    log4j.appender.A1.secret=<Log Analytics primary key>
    ```

2. 사용자 지정 로깅을 구성하려면 `\azure\azure-databricks-monitoring\scripts\metrics.properties`를 엽니다. 다음 두 값을 편집합니다.
    ```shell
    *.sink.loganalytics.workspaceId=<Log Analytics workspace ID>
    *.sink.loganalytics.secret=<Log Analytics primary key>
    ```

### <a name="build-the-jar-files-for-the-databricks-job-and-databricks-monitoring"></a>Databricks 작업 및 Databricks 모니터링에 대한 .jar 파일 빌드

1. Java IDE를 사용하여 루트 디렉터리에 있는 **pom.xml**이라는 Maven 프로젝트 파일을 가져옵니다.

2. 클린 빌드를 수행합니다. 이 빌드의 출력은 **azure-databricks-job-1.0-SNAPSHOT.jar** 및 **azure-databricks-monitoring-0.9.jar**라는 이름의 파일입니다.

### <a name="configure-custom-logging-for-the-databricks-job"></a>Databricks 작업에 대한 사용자 지정 로깅 구성

1. **Databricks CLI**에 다음 명령을 입력하여 **azure-databricks-monitoring-0.9.jar** 파일을 Databricks 파일 시스템으로 복사합니다.
    ```shell
    databricks fs cp --overwrite azure-databricks-monitoring-0.9.jar dbfs:/azure-databricks-job/azure-databricks-monitoring-0.9.jar
    ```

2. 다음 명령을 입력하여 `\azure\azure-databricks-monitoring\scripts\metrics.properties`의 사용자 지정 로깅 속성을 Databricks 파일 시스템으로 복사합니다.
    ```shell
    databricks fs cp --overwrite metrics.properties dbfs:/azure-databricks-job/metrics.properties
    ```

3. Databricks 클러스터의 이름을 아직 결정하지 않은 경우 지금 결정해야 합니다. 클러스터의 Databricks 파일 시스템 경로에 이름을 입력합니다. 다음 명령을 입력하여 `\azure\azure-databricks-monitoring\scripts\spark.metrics`의 초기화 스크립트를 Databricks 파일 시스템으로 복사합니다.
    ```shell
    databricks fs cp --overwrite spark-metrics.sh dbfs:/databricks/init/<cluster-name>/spark-metrics.sh
    ```

### <a name="create-a-databricks-cluster"></a>Databricks 클러스터 만들기

1. Databricks 작업 영역에서 "클러스터"를 클릭한 다음, "클러스터 만들기"를 클릭합니다. **Databricks 작업에 대한 사용자 지정 로깅 구성** 섹션의 3단계에서 만든 클러스터 이름을 입력합니다.

2. **표준** 클러스터 모드를 선택합니다.

3. **Databricks 런타임 버전**을 **4.3(Apache Spark 2.3.1, Scala 2.11 포함)** 으로 설정합니다.

4. **Python 버전**을 **2**로 설정합니다.

5. **드라이버 유형**을 **작업자와 동일**로 설정합니다.

6. **작업자 유형**을 **Standard_DS3_v2**로 설정합니다.

7. **최소 작업자**를 **2**로 설정합니다.

8. **자동 크기 조정 사용**의 선택을 취소합니다.

9. **자동 종료** 대화 상자 아래에서 **Init 스크립트**를 클릭합니다.

10. **dbfs:/databricks/init/<cluster-name>/spark-metrics.sh**를 입력하고, <cluster-name>을 1단계에서 만든 클러스터 이름으로 바꿉니다.

11. **추가** 단추를 클릭합니다.

12. **클러스터 만들기** 단추를 클릭합니다.

### <a name="create-a-databricks-job"></a>Databricks 작업 만들기

1. Databricks 작업 영역에서 "작업", "작업 만들기"를 클릭합니다.

2. 작업 이름을 입력합니다.

3. "jar 설정"을 클릭하면 "실행할 JAR 업로드" 대화 상자가 열립니다.

4. **Databricks 작업에 사용할 .jar 빌드** 섹션에서 만든 **azure-databricks-job-1.0-SNAPSHOT.jar** 파일을 **업로드할 JAR를 여기에 드롭** 상자로 끌어다 놓습니다.

5. **기본 클래스** 필드에 **com.microsoft.pnp.TaxiCabReader**를 입력합니다.

6. 인수 필드에 다음을 입력합니다.
    ```shell
    -n jar:file:/dbfs/azure-databricks-jobs/ZillowNeighborhoods-NY.zip!/ZillowNeighborhoods-NY.shp --taxi-ride-consumer-group taxi-ride-eh-cg --taxi-fare-consumer-group taxi-fare-eh-cg --window-interval "1 minute" --cassandra-host <Cosmos DB Cassandra host name from above>
    ```

7. 다음 단계에 따라 종속 라이브러리를 설치합니다.

    1. Databricks 사용자 인터페이스에서 **홈** 단추를 클릭합니다.

    2. **사용자** 드롭다운 목록에서 해당하는 사용자 계정 이름을 클릭하여 계정 작업 영역 설정을 엽니다.

    3. 계정 이름 옆에 있는 드롭다운 화살표를 클릭하고, **만들기**를 클릭하고, **라이브러리**를 클릭하여 **새 라이브러리** 대화 상자를 엽니다.

    4. **원본** 드롭다운 컨트롤에서 **Maven 좌표**를 선택합니다.

    5. **Maven 아티팩트 설치** 제목 아래의 **좌표** 텍스트 상자에 `com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.5`를 입력합니다.

    6. **라이브러리 만들기**를 클릭하여 **아티팩트** 창을 엽니다.

    7. **실행 중인 클러스터의 상태** 아래에서 **모든 클러스터에 자동 연결** 확인란을 선택합니다.

    8. `com.microsoft.azure.cosmosdb:azure-cosmos-cassandra-spark-helper:1.0.0` Maven 좌표에 대해 1-7단계를 반복합니다.

    9. `org.geotools:gt-shapefile:19.2` Maven 좌표에 대해 1-6단계를 반복합니다.

    10. **고급 옵션**을 클릭합니다.

    11. **리포지토리** 텍스트 상자에 `http://download.osgeo.org/webdav/geotools/`를 입력합니다.

    12. **라이브러리 만들기**를 클릭하여 **아티팩트** 창을 엽니다. 

    13. **실행 중인 클러스터의 상태** 아래에서 **모든 클러스터에 자동 연결** 확인란을 선택합니다.

8. 7단계에서 추가된 종속 라이브러리를 6단계 마지막 부분에서 만든 작업에 추가합니다.

    1. Azure Databricks 작업 영역에서 **작업**을 클릭합니다.

    2. **Databricks 작업 만들기** 섹션의 2단계에서 만든 작업 이름을 클릭합니다.

    3. **종속 라이브러리** 섹션 옆에서 **추가**를 클릭하여 **종속 라이브러리 추가** 대화 상자를 엽니다.

    4. **다음 위치의 라이브러리** 아래에서 **작업 영역**을 선택합니다.

    5. **사용자**를 클릭하고 사용자 이름을 클릭한 다음, `azure-eventhubs-spark_2.11:2.3.5`를 클릭합니다.

    6. **확인**을 클릭합니다.

    7. `spark-cassandra-connector_2.11:2.3.1` 및 `gt-shapefile:19.2`에 대해 1-6단계를 반복합니다.

9. **클러스터:** 옆에 있는 **편집**을 클릭합니다. 그러면 **클러스터 구성** 대화 상자가 열립니다. **클러스터 유형** 드롭다운 목록에서 **기존 클러스터**를 선택합니다. **클러스터 선택** 드롭다운 목록에서, **Databricks 클러스터 만들기** 섹션에서 만든 클러스터를 선택합니다. **확인**을 클릭합니다.

10. **지금 실행**을 클릭합니다.

### <a name="run-the-data-generator"></a>데이터 생성기 실행

1. GitHub 리포지토리의 `onprem` 디렉터리로 이동합니다.

2. **main.env** 파일의 값을 다음과 같이 업데이트합니다.

    ```shell
    RIDE_EVENT_HUB=[Connection string for the taxi-ride event hub]
    FARE_EVENT_HUB=[Connection string for the taxi-fare event hub]
    RIDE_DATA_FILE_PATH=/DataFile/FOIL2013
    MINUTES_TO_LEAD=0
    PUSH_RIDE_DATA_FIRST=false
    ```
    택시 승객 이벤트 허브의 연결 문자열은 *Azure 리소스 배포* 섹션의 4단계에서 나오는 **eventHubs** 출력 섹션의 **taxi-ride-eh** 값입니다. 택시 요금 이벤트 허브의 연결 문자열은 *Azure 리소스 배포* 섹션의 4단계에서 나오는 **eventHubs** 출력 섹션의 **taxi-fare-eh** 값입니다.

3. 다음 명령을 실행하여 Docker 이미지를 빌드합니다.

    ```bash
    docker build --no-cache -t dataloader .
    ```

4. 부모 디렉터리로 다시 이동합니다.

    ```bash
    cd ..
    ```

5. 다음 명령을 실행하여 Docker 이미지를 실행합니다.

    ```bash
    docker run -v `pwd`/DataFile:/DataFile --env-file=onprem/main.env dataloader:latest
    ```

출력은 다음과 비슷합니다.

```
Created 10000 records for TaxiFare
Created 10000 records for TaxiRide
Created 20000 records for TaxiFare
Created 20000 records for TaxiRide
Created 30000 records for TaxiFare
...
```

Databricks 작업이 올바르게 실행 중인지 확인하려면 Azure Portal을 열고 Cosmos DB 데이터베이스로 이동합니다. **데이터 탐색기** 블레이드를 열고 **택시 레코드** 테이블의 데이터를 검사합니다. 

[1] <span id="note1">Donovan, Brian; Work, Dan(2016): 뉴욕시 택시 운행 데이터(2010-2013). University of Illinois at Urbana-Champaign https://doi.org/10.13012/J8PN93H8
