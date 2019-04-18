---
title: Azure Databricks 애플리케이션 로그를 Azure Monitor에 보내기
description: Azure Monitor를 Azure Databricks에서 사용자 지정 로그 및 메트릭을 보내는 방법
author: petertaylor9999
ms.date: 03/26/2019
ms.openlocfilehash: ea67122d7871663e8aaf42b7af0043492f63b6b1
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59639189"
---
# <a name="send-azure-databricks-application-logs-to-azure-monitor"></a>Azure Databricks 애플리케이션 로그를 Azure Monitor에 보내기

이 아티클에서 Azure Databricks에에서 응용 프로그램 로그 및 메트릭을 보내는 방법에는 [Log Analytics 작업 영역](/azure/azure-monitor/platform/manage-access)합니다. 사용 된 [Azure Databricks 모니터링 라이브러리](https://github.com/mspnp/spark-monitoring), GitHub에서 사용할 수 있는 합니다.

## <a name="prerequisites"></a>필수 조건

설명한 대로 모니터링 라이브러리를 사용 하 여 Azure Databricks 클러스터 구성 [메트릭을 Azure Monitor로 보내도록 Azure Databricks 구성][config-cluster]합니다.

> [!NOTE]
> 모니터링 라이브러리는 Apache Spark 수준 이벤트 및 작업 Azure Monitor로 메트릭이 Spark Structured Streaming을 스트리밍합니다. 이러한 이벤트 및 메트릭 용 응용 프로그램 코드를 변경 하지 않아도 됩니다.

## <a name="send-application-metrics-using-dropwizard"></a>Dropwizard를 사용 하 여 응용 프로그램 메트릭 보내기

Spark는 Dropwizard 메트릭 라이브러리 기반 구성 가능한 메트릭 시스템을 사용 합니다. 자세한 내용은 [메트릭을](https://spark.apache.org/docs/latest/monitoring.html#metrics) Spark 설명서에서.

응용 프로그램에 메트릭을 보내도록 응용 프로그램 코드를 Azure Databricks에서 Azure Monitor에서 다음이 단계를 수행 합니다.

1. 빌드를 **spark-수신기-loganalytics-1.0-SNAPSHOT.jar** JAR 파일에 설명 된 대로 [Azure Databricks 모니터링 라이브러리를 빌드할][build-lib]합니다.

1. 만들 Dropwizard [계기 또는 카운터](https://metrics.dropwizard.io/4.0.0/manual/core.html) 응용 프로그램 코드에서. 사용할 수는 `UserMetricsSystem` 모니터링 라이브러리에 정의 된 클래스입니다. 다음 예제에서는 카운터 라는 `counter1`합니다.

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

    모니터링 라이브러리에 포함 되어는 [샘플 응용 프로그램] [ sample-app] 사용 하는 방법을 보여 주는 `UserMetricsSystem` 클래스입니다.

## <a name="send-application-logs-using-log4j"></a>Log4j를 사용 하 여 응용 프로그램 로그 보내기

Azure Log Analytics를 사용 하 여 Azure Databricks 응용 프로그램 로그를 보내도록 합니다 [Log4j 어 펜더](https://logging.apache.org/log4j/2.x/manual/appenders.html) 라이브러리에서 다음이 단계를 수행 합니다.

1. 빌드를 **spark-수신기-loganalytics-1.0-SNAPSHOT.jar** JAR 파일에 설명 된 대로 [Azure Databricks 모니터링 라이브러리를 빌드할][build-lib]합니다.

1. 만들기는 **log4j.properties** [구성 파일](https://logging.apache.org/log4j/2.x/manual/configuration.html) 응용 프로그램에 대 한 합니다. 다음 구성 속성이 포함 됩니다. 표시 된 위치에 응용 프로그램 패키지 이름 및 로그 수준으로 대체 합니다.

    ```YAML
    log4j.appender.A1=com.microsoft.pnp.logging.loganalytics.LogAnalyticsAppender
    log4j.appender.A1.layout=com.microsoft.pnp.logging.JSONLayout
    log4j.appender.A1.layout.LocationInfo=false
    log4j.additivity.<your application package name>=false
    log4j.logger.<your application package name>=<log level>, A1
    ```

    샘플 구성 파일을 찾을 수 있습니다 [같습니다][log4j.properties]합니다.

1. 응용 프로그램 코드에 포함 된 **spark-수신기-loganalytics** 프로젝트를 가져와 `com.microsoft.pnp.logging.Log4jconfiguration` 응용 프로그램 코드에 합니다.

    ```Scala
    import com.microsoft.pnp.logging.Log4jConfiguration
    ```

1. Log4j를 사용 하 여 구성 합니다 **log4j.properties** 3 단계에서 만든 파일:

    ```Scala
    getClass.getResourceAsStream("<path to file in your JAR file>/log4j.properties")) {
          stream => {
            Log4jConfiguration.configure(stream)
          }
    }
    ```

1. 필요에 따라 코드에서 적절 한 수준에서 Apache Spark 로그 메시지를 추가 합니다. 예를 들어 사용을 `logDebug` 디버그 로그 메시지를 보내는 방법. 자세한 내용은 [로깅] [ spark-logging] Spark 설명서에서.

    ```Scala
    logTrace("Trace message")
    logDebug("Debug message")
    logInfo("Info message")
    logWarning("Warning message")
    logError("Error message")
    ```

## <a name="run-the-sample-application"></a>샘플 애플리케이션 실행

모니터링 라이브러리에 포함 된 [샘플 응용 프로그램] [ sample-app] Azure Monitor로 응용 프로그램 메트릭 및 응용 프로그램 로그를 전송 하는 방법을 보여 주는. 샘플을 실행하려면

1. 빌드를 **spark 작업** 에 설명 된 대로 모니터링 라이브러리의 프로젝트 [Azure Databricks 모니터링 라이브러리를 빌드할][build-lib]합니다.

1. Databricks 작업 영역으로 이동 하 고 설명 된 대로 새 작업을 만듭니다 [여기](https://docs.azuredatabricks.net/user-guide/jobs.html#create-a-job)합니다.

1. 작업 세부 정보 페이지에서 선택 **JAR 설정**합니다.

1. JAR 파일을 업로드 `/src/spark-jobs/target/spark-jobs-1.0-SNAPSHOT.jar`합니다.

1. 에 대 한 **주 클래스**, 입력 `com.microsoft.pnp.samplejob.StreamingQueryListenerSampleJob`합니다.

1. 모니터링 라이브러리를 사용 하도록 이미 구성 된 클러스터를 선택 합니다. 참조 [메트릭을 Azure Monitor로 보내도록 Azure Databricks 구성][config-cluster]합니다.

작업을 실행할 때 응용 프로그램 로그 및 메트릭을 Log Analytics 작업 영역에서 볼 수 있습니다.

응용 프로그램 로그 SparkLoggingEvent_CL 아래에 나타납니다.

```Kusto
SparkLoggingEvent_CL | where logger_name_s contains "com.microsoft.pnp"
```

응용 프로그램 메트릭 SparkMetric_CL 아래에 나타납니다.

```Kusto
SparkMetric_CL | where name_s contains "rowcounter" | limit 50
```

## <a name="next-steps"></a>다음 단계

성능 모니터링에서 프로덕션 Azure Databricks 워크 로드 성능 문제를 해결 하려면이 코드 라이브러리를 함께 제공 되는 대시보드를 배포 합니다.

> [!div class="nextstepaction"]
> [대시보드를 사용하여 Azure Databricks 메트릭 시각화](./dashboards.md)

<!-- links -->

[build-lib]: ./configure-cluster.md##build-the-azure-databricks-monitoring-library
[config-cluster]: ./configure-cluster.md
[log4j.properties]: https://github.com/mspnp/spark-monitoring/blob/master/src/spark-jobs/src/main/resources/com/microsoft/pnp/samplejob/log4j.properties
[sample-app]: https://github.com/mspnp/spark-monitoring/tree/master/src/spark-jobs
[spark-logging]: https://spark.apache.org/docs/2.3.0/api/java/org/apache/spark/internal/Logging.html