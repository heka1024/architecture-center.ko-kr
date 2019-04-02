---
title: Azure Monitor에서 Azure Databricks 응용 프로그램 로그 보내기
description: Azure Monitor를 Azure Databricks에서 사용자 지정 로그 및 메트릭을 보내는 방법
author: petertaylor9999
ms.date: 03/26/2019
ms.openlocfilehash: 49c631687fb3e3bbd807ffbbb49d9c5f6526bfb4
ms.sourcegitcommit: 9854bd27fb5cf92041bbfb743d43045cd3552a69
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58503436"
---
# <a name="send-azure-databricks-application-logs-to-azure-monitor"></a><span data-ttu-id="7af91-103">Azure Monitor에서 Azure Databricks 응용 프로그램 로그 보내기</span><span class="sxs-lookup"><span data-stu-id="7af91-103">Send Azure Databricks application logs to Azure Monitor</span></span>

<span data-ttu-id="7af91-104">이 아티클에서 Azure Databricks에에서 응용 프로그램 로그 및 메트릭을 보내는 방법에는 [Log Analytics 작업 영역](/azure/azure-monitor/platform/manage-access)합니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-104">This article shows how to send application logs and metrics from Azure Databricks to a [Log Analytics workspace](/azure/azure-monitor/platform/manage-access).</span></span> <span data-ttu-id="7af91-105">사용 된 [Azure Databricks 모니터링 라이브러리](https://github.com/mspnp/spark-monitoring), GitHub에서 사용할 수 있는 합니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-105">It uses the [Azure Databricks Monitoring Library](https://github.com/mspnp/spark-monitoring), which is available on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7af91-106">필수 조건</span><span class="sxs-lookup"><span data-stu-id="7af91-106">Prerequisites</span></span>

<span data-ttu-id="7af91-107">설명한 대로 모니터링 라이브러리를 사용 하 여 Azure Databricks 클러스터 구성 [메트릭을 Azure Monitor로 보내도록 Azure Databricks 구성][config-cluster]합니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-107">Configure your Azure Databricks cluster to use the monitoring library, as described in [Configure Azure Databricks to send metrics to Azure Monitor][config-cluster].</span></span>

> [!NOTE]
> <span data-ttu-id="7af91-108">모니터링 라이브러리는 Apache Spark 수준 이벤트 및 작업 Azure Monitor로 메트릭이 Spark Structured Streaming을 스트리밍합니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-108">The monitoring library streams Apache Spark level events and Spark Structured Streaming metrics from your jobs to Azure Monitor.</span></span> <span data-ttu-id="7af91-109">이러한 이벤트 및 메트릭 용 응용 프로그램 코드를 변경 하지 않아도 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-109">You don't need to make any changes to your application code for these events and metrics.</span></span>

## <a name="send-application-metrics-using-dropwizard"></a><span data-ttu-id="7af91-110">Dropwizard를 사용 하 여 응용 프로그램 메트릭 보내기</span><span class="sxs-lookup"><span data-stu-id="7af91-110">Send application metrics using Dropwizard</span></span>

<span data-ttu-id="7af91-111">Spark는 Dropwizard 메트릭 라이브러리 기반 구성 가능한 메트릭 시스템을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-111">Spark uses a configurable metrics system based on the Dropwizard Metrics Library.</span></span> <span data-ttu-id="7af91-112">자세한 내용은 [메트릭을](https://spark.apache.org/docs/latest/monitoring.html#metrics) Spark 설명서에서.</span><span class="sxs-lookup"><span data-stu-id="7af91-112">For more information, see [Metrics](https://spark.apache.org/docs/latest/monitoring.html#metrics) in the Spark documentation.</span></span>

<span data-ttu-id="7af91-113">응용 프로그램에 메트릭을 보내도록 응용 프로그램 코드를 Azure Databricks에서 Azure Monitor에서 다음이 단계를 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-113">To send application metrics from Azure Databricks application code to Azure Monitor, follow these steps:</span></span>

1. <span data-ttu-id="7af91-114">빌드를 **spark-수신기-loganalytics-1.0-SNAPSHOT.jar** JAR 파일에 설명 된 대로 [Azure Databricks 모니터링 라이브러리를 빌드할][build-lib]합니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-114">Build the **spark-listeners-loganalytics-1.0-SNAPSHOT.jar** JAR file as described in [Build the Azure Databricks Monitoring Library][build-lib].</span></span>

1. <span data-ttu-id="7af91-115">만들 Dropwizard [계기 또는 카운터](https://metrics.dropwizard.io/4.0.0/manual/core.html) 응용 프로그램 코드에서.</span><span class="sxs-lookup"><span data-stu-id="7af91-115">Create Dropwizard [gauges or counters](https://metrics.dropwizard.io/4.0.0/manual/core.html) in your application code.</span></span> <span data-ttu-id="7af91-116">사용할 수는 `UserMetricsSystem` 모니터링 라이브러리에 정의 된 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-116">You can use the `UserMetricsSystem` class defined in the monitoring library.</span></span> <span data-ttu-id="7af91-117">다음 예제에서는 카운터 라는 `counter1`합니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-117">The following example creates a counter named `counter1`.</span></span>

    ```Scala
    import org.apache.spark.metrics.UserMetricsSystems
    import org.apache.spark.sql.SparkSession

    object StreamingQueryListenerSampleJob  {

      private final val METRICS_NAMESPACE = "samplejob"
      private final val COUNTER_NAME = "counter1"

      def main(args: Array[String]): Unit = {

        val spark = SparkSession
          .builder
          .getOrCreate

        val driverMetricsSystem = UserMetricsSystems
            .getMetricSystem(METRICS_NAMESPACE, builder => {
              builder.registerCounter(COUNTER_NAME)
            })

        driverMetricsSystem.counter(COUNTER_NAME).inc(5)
      }
    }
    ```

    <span data-ttu-id="7af91-118">모니터링 라이브러리에 포함 되어는 [샘플 응용 프로그램] [ sample-app] 사용 하는 방법을 보여 주는 `UserMetricsSystem` 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-118">The monitoring library includes a [sample application][sample-app] that demonstrates how to use the `UserMetricsSystem` class.</span></span>

## <a name="send-application-logs-using-log4j"></a><span data-ttu-id="7af91-119">Log4j를 사용 하 여 응용 프로그램 로그 보내기</span><span class="sxs-lookup"><span data-stu-id="7af91-119">Send application logs using Log4j</span></span>

<span data-ttu-id="7af91-120">Azure Log Analytics를 사용 하 여 Azure Databricks 응용 프로그램 로그를 보내도록 합니다 [Log4j 어 펜더](https://logging.apache.org/log4j/2.x/manual/appenders.html) 라이브러리에서 다음이 단계를 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-120">To send your Azure Databricks application logs to Azure Log Analytics using the [Log4j appender](https://logging.apache.org/log4j/2.x/manual/appenders.html) in the library, follow these steps:</span></span>

1. <span data-ttu-id="7af91-121">빌드를 **spark-수신기-loganalytics-1.0-SNAPSHOT.jar** JAR 파일에 설명 된 대로 [Azure Databricks 모니터링 라이브러리를 빌드할][build-lib]합니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-121">Build the **spark-listeners-loganalytics-1.0-SNAPSHOT.jar** JAR file as described in [Build the Azure Databricks Monitoring Library][build-lib].</span></span>

1. <span data-ttu-id="7af91-122">만들기는 **log4j.properties** [구성 파일](https://logging.apache.org/log4j/2.x/manual/configuration.html) 응용 프로그램에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-122">Create a **log4j.properties** [configuration file](https://logging.apache.org/log4j/2.x/manual/configuration.html) for your application.</span></span> <span data-ttu-id="7af91-123">다음 구성 속성이 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-123">Include the following configuration properties.</span></span> <span data-ttu-id="7af91-124">표시 된 위치에 응용 프로그램 패키지 이름 및 로그 수준으로 대체 합니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-124">Substitute your application package name and log level where indicated:</span></span>

    ```YAML
    log4j.appender.A1=com.microsoft.pnp.logging.loganalytics.LogAnalyticsAppender
    log4j.appender.A1.layout=com.microsoft.pnp.logging.JSONLayout
    log4j.appender.A1.layout.LocationInfo=false
    log4j.additivity.<your application package name>=false
    log4j.logger.<your application package name>=<log level>, A1
    ```

    <span data-ttu-id="7af91-125">샘플 구성 파일을 찾을 수 있습니다 [같습니다][log4j.properties]합니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-125">You can find a sample configuration file [here][log4j.properties].</span></span>

1. <span data-ttu-id="7af91-126">응용 프로그램 코드에 포함 된 **spark-수신기-loganalytics** 프로젝트를 가져와 `com.microsoft.pnp.logging.Log4jconfiguration` 응용 프로그램 코드에 합니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-126">In your application code, include the **spark-listeners-loganalytics** project, and import `com.microsoft.pnp.logging.Log4jconfiguration` to your application code.</span></span>

    ```Scala
    import com.microsoft.pnp.logging.Log4jConfiguration
    ```

1. <span data-ttu-id="7af91-127">Log4j를 사용 하 여 구성 합니다 **log4j.properties** 3 단계에서 만든 파일:</span><span class="sxs-lookup"><span data-stu-id="7af91-127">Configure Log4j using the **log4j.properties** file you created in step 3:</span></span>

    ```Scala
    getClass.getResourceAsStream("<path to file in your JAR file>/log4j.properties")) {
          stream => {
            Log4jConfiguration.configure(stream)
          }
    }
    ```

1. <span data-ttu-id="7af91-128">필요에 따라 코드에서 적절 한 수준에서 Apache Spark 로그 메시지를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-128">Add Apache Spark log messages at the appropriate level in your code as required.</span></span> <span data-ttu-id="7af91-129">예를 들어, 사용 하 여는 `logDebug` 디버그 로그 meesage를 전송 하는 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-129">For example, use the `logDebug` method to send a debug log meesage.</span></span> <span data-ttu-id="7af91-130">자세한 내용은 [로깅] [ spark-logging] Spark 설명서에서.</span><span class="sxs-lookup"><span data-stu-id="7af91-130">For more information, see [Logging][spark-logging] in the Spark documentation.</span></span>

    ```Scala
    logTrace("Trace message")
    logDebug("Debug message")
    logInfo("Info message")
    logWarning("Warning message")
    logError("Error message")
    ```

## <a name="run-the-sample-application"></a><span data-ttu-id="7af91-131">샘플 애플리케이션 실행</span><span class="sxs-lookup"><span data-stu-id="7af91-131">Run the sample application</span></span>

<span data-ttu-id="7af91-132">모니터링 라이브러리에 포함 된 [샘플 응용 프로그램] [ sample-app] Azure Monitor로 응용 프로그램 메트릭 및 응용 프로그램 로그를 전송 하는 방법을 보여 주는.</span><span class="sxs-lookup"><span data-stu-id="7af91-132">The monitoring library includes a [sample application][sample-app] that demonstrates how to send both application metrics and application logs to Azure Monitor.</span></span> <span data-ttu-id="7af91-133">샘플을 실행하려면</span><span class="sxs-lookup"><span data-stu-id="7af91-133">To run the sample:</span></span>

1. <span data-ttu-id="7af91-134">빌드를 **spark 작업** 에 설명 된 대로 모니터링 라이브러리의 프로젝트 [Azure Databricks 모니터링 라이브러리를 빌드할][build-lib]합니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-134">Build the **spark-jobs** project in the monitoring library, as described in [Build the Azure Databricks Monitoring Library][build-lib].</span></span>

1. <span data-ttu-id="7af91-135">Databricks 작업 영역으로 이동 하 고 설명 된 대로 새 작업을 만듭니다 [여기](https://docs.azuredatabricks.net/user-guide/jobs.html#create-a-job)합니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-135">Navigate to your Databricks workspace and create a new job, as described [here](https://docs.azuredatabricks.net/user-guide/jobs.html#create-a-job).</span></span>

1. <span data-ttu-id="7af91-136">작업 세부 정보 페이지에서 선택 **JAR 설정**합니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-136">In the job detail page, select **Set JAR**.</span></span>

1. <span data-ttu-id="7af91-137">JAR 파일을 업로드 `/src/spark-jobs/target/spark-jobs-1.0-SNAPSHOT.jar`합니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-137">Upload the JAR file from `/src/spark-jobs/target/spark-jobs-1.0-SNAPSHOT.jar`.</span></span>

1. <span data-ttu-id="7af91-138">에 대 한 **주 클래스**, 입력 `com.microsoft.pnp.samplejob.StreamingQueryListenerSampleJob`합니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-138">For **Main class**, enter `com.microsoft.pnp.samplejob.StreamingQueryListenerSampleJob`.</span></span>

1. <span data-ttu-id="7af91-139">모니터링 라이브러리를 사용 하도록 이미 구성 된 클러스터를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-139">Select a cluster that is already configured to use the monitoring library.</span></span> <span data-ttu-id="7af91-140">참조 [메트릭을 Azure Monitor로 보내도록 Azure Databricks 구성][config-cluster]합니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-140">See [Configure Azure Databricks to send metrics to Azure Monitor][config-cluster].</span></span>

<span data-ttu-id="7af91-141">작업을 실행할 때 응용 프로그램 로그 및 메트릭을 Log Analytics 작업 영역에서 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-141">When the job runs, you can view the application logs and metrics in your Log Analytics workspace.</span></span>

<span data-ttu-id="7af91-142">응용 프로그램 로그 SparkLoggingEvent_CL 아래에 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-142">Application logs appear under SparkLoggingEvent_CL:</span></span>

```Kusto
SparkLoggingEvent_CL | where logger_name_s contains "com.microsoft.pnp"
```

<span data-ttu-id="7af91-143">응용 프로그램 메트릭 SparkMetric_CL 아래에 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-143">Application metrics appear under SparkMetric_CL:</span></span>

```Kusto
SparkMetric_CL | where name_s contains "rowcounter" | limit 50
```

## <a name="next-steps"></a><span data-ttu-id="7af91-144">다음 단계</span><span class="sxs-lookup"><span data-stu-id="7af91-144">Next steps</span></span>

<span data-ttu-id="7af91-145">성능 모니터링에서 프로덕션 Azure Databricks 워크 로드 성능 문제를 해결 하려면이 코드 라이브러리를 함께 제공 되는 대시보드를 배포 합니다.</span><span class="sxs-lookup"><span data-stu-id="7af91-145">Deploy the performance monitoring dashboard that accompanies this code library to troubleshoot performance issues in your production Azure Databricks workloads.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="7af91-146">대시보드를 사용 하 여 Azure Databricks 메트릭을 시각화합니다</span><span class="sxs-lookup"><span data-stu-id="7af91-146">Use dashboards to visualize Azure Databricks metrics</span></span>](./dashboards.md)

<!-- links -->

[build-lib]: ./configure-cluster.md##build-the-azure-databricks-monitoring-library
[config-cluster]: ./configure-cluster.md
[log4j.properties]: https://github.com/mspnp/spark-monitoring/blob/master/src/spark-jobs/src/main/resources/com/microsoft/pnp/samplejob/log4j.properties
[sample-app]: https://github.com/mspnp/spark-monitoring/tree/master/src/spark-jobs
[spark-logging]: https://spark.apache.org/docs/2.3.0/api/java/org/apache/spark/internal/Logging.html