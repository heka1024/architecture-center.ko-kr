---
title: Azure Databricks를 구성하여 Azure Monitor에 메트릭 보내기
description: 메트릭의 모니터링 및 Azure Log Analytics에서 데이터의 로깅을 사용할 수 있도록 scala 라이브러리
author: petertaylor9999
ms.date: 03/26/2019
ms.openlocfilehash: f2fc1fd19da661b74ddf032dd1d5153ce575345c
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59639889"
---
<!-- markdownlint-disable MD040 -->

# <a name="configure-azure-databricks-to-send-metrics-to-azure-monitor"></a>Azure Databricks를 구성하여 Azure Monitor에 메트릭 보내기

이 문서에 메트릭을 보내도록 Azure Databricks 클러스터를 구성 하는 방법을 보여 줍니다는 [Log Analytics 작업 영역](/azure/azure-monitor/platform/manage-access)합니다. 사용 된 [Azure Databricks 모니터링 라이브러리](https://github.com/mspnp/spark-monitoring), GitHub에서 사용할 수 있는 합니다. 필수 구성 요소로 Java, Scala 및 Maven의 이해를 사용 하는 것이 좋습니다.

## <a name="about-the-azure-databricks-monitoring-library"></a>라이브러리를 모니터링 하는 Azure Databricks에 대 한

합니다 [GitHub 리포지토리에서](https://github.com/mspnp/spark-monitoring) Azure Databricks 모니터링 라이브러리에는 다음 디렉터리 구조에 대 한 합니다.

```
/src  
    /spark-jobs  
    /spark-listeners-loganalytics  
    /spark-listeners  
    /pom.xml  
```

합니다 **spark 작업** 디렉터리는 Spark 응용 프로그램 메트릭 카운터를 구현 하는 방법을 보여 주는 샘플 Spark 응용 프로그램입니다.

합니다 **spark 수신기** 디렉터리에 Apache Spark로 이벤트를 보내는 서비스 수준에서 Azure Log Analytics 작업 영역을 Azure Databricks를 사용 하도록 설정 하는 기능이 포함 되어 있습니다. Azure Databricks는 데이터 집합, DataFrames 및 SQL을 사용 하 여 데이터를 처리 하는 일괄 처리에 대 한 구조화 된 Api 집합을 포함 하는 Apache Spark 기반 서비스입니다. Apache Spark 2.0 지원에 대 한 추가 되었습니다 [Structured Streaming](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html), 데이터 스트림 Api를 처리 하는 Spark의 일괄 처리 기반 API를 처리 합니다.

합니다 **spark-수신기-loganalytics** 디렉터리 Spark 수신기, DropWizard에 대 한 싱크 및 Azure Log Analytics 작업 영역에 대 한 클라이언트에 대 한 싱크를 포함 합니다. 이 디렉터리에는 Apache Spark 응용 프로그램 로그에 대 한 log4j 어 펜더를도 포함 됩니다.

합니다 **spark-수신기-loganalytics** 하 고 **spark 수신기** Databricks 클러스터에 배포 된 두 개의 JAR 파일을 작성 하는 것에 대 한 코드를 포함 하는 디렉터리입니다. 합니다 **spark 수신기** 디렉터리를 **스크립트** JAR을 복사 하는 클러스터 노드 초기화 스크립트를 포함 하는 디렉터리를 Azure Databricks 파일 시스템의 준비 디렉터리에서 파일 실행 노드입니다.

합니다 **pom.xml** 파일은 전체 프로젝트에 대 한 기본 Maven 빌드 파일입니다.

## <a name="prerequisites"></a>필수 조건

시작 하려면 다음과 같은 Azure 리소스를 배포 합니다.

- Azure Databricks 작업 영역입니다. [빠른 시작: Azure portal을 사용 하 여 Azure Databricks에서 Spark 작업 실행](/azure/azure-databricks/quickstart-create-databricks-workspace-portal)합니다.
- Log Analytics 작업 영역. 참조 [Azure portal에서 Log Analytics 작업 영역 만들기](/azure/azure-monitor/learn/quick-create-workspace)합니다.

그런 다음 설치 합니다 [Azure Databricks CLI](https://docs.databricks.com/user-guide/dev-tools/databricks-cli.html#install-the-cli)합니다. Azure Databricks 개인용 액세스 토큰을 CLI를 사용 해야 합니다. 자세한 내용은 [토큰을 생성할](https://docs.azuredatabricks.net/api/latest/authentication.html#token-management)합니다.

Log Analytics 작업 영역 ID 및 키를 찾습니다. 자세한 내용은 [작업 영역 ID 및 키 가져오기](/azure/azure-monitor/platform/agent-windows#obtain-workspace-id-and-key)합니다. Databricks 클러스터를 구성한 경우 이러한 값이 필요 합니다.

모니터링 라이브러리를 작성 하려면 다음 리소스를 사용 하 여 Java IDE를 사용 해야 합니다.

- [Java 개발 키트 (JDK) 버전 1.8](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
- [Scala SDK 2.11 언어](https://www.scala-lang.org/download/)
- [Apache Maven 3.5.4](http://maven.apache.org/download.cgi)

## <a name="build-the-azure-databricks-monitoring-library"></a>라이브러리를 모니터링 하는 Azure Databricks를 빌드

Azure Databricks 모니터링 라이브러리를 작성 하려면 다음 단계를 수행 합니다.

1. 복제, 포크 또는 다운로드 합니다 [GitHub 리포지토리](https://github.com/mspnp/spark-monitoring)합니다.

1. Maven 프로젝트 개체 모델 파일을 가져옵니다 _pom.xml_에 있는 합니다 **/src** 프로젝트 폴더입니다. 이 옵션은 세 개의 프로젝트를 가져옵니다.

    - spark-jobs
    - spark 수신기
    - spark-listeners-loganalytics

1. Maven을 실행 **패키지** 이러한 각 프로젝트에 대 한 JAR 파일을 빌드하기 위해 Java IDE에서 빌드 단계:

    |Project| JAR 파일|
    |-------|---------|
    |spark-jobs|spark-jobs-1.0-SNAPSHOT.jar|
    |spark 수신기|spark-listeners-1.0-SNAPSHOT.jar|
    |spark-listeners-loganalytics|spark-listeners-loganalytics-1.0-SNAPSHOT.jar|

1. Azure Databricks CLI를 사용 하 여 라는 디렉터리를 만들려면 **dbfs: / databricks/모니터링-스테이징**:  

    ```bash
    dbfs mkdirs dbfs:/databricks/monitoring-staging
    ```

1. Azure Databricks CLI를 사용 하 여 복사할 **/src/spark-listeners/scripts/listeners.sh** 이전 디렉터리:

    ```bash
    dbfs cp ./src/spark-listeners/scripts/listeners.sh dbfs:/databricks/monitoring-staging/listeners.sh
    ```

1. Azure Databricks CLI를 사용 하 여 복사할 **/src/spark-listeners/scripts/metrics.properties** 이전에 만든 디렉터리로:

    ```bash
    dbfs cp <local path to metrics.properties> dbfs:/databricks/monitoring-staging/metrics.properties
    ```

1. Azure Databricks CLI를 사용 하 여 복사할 **spark-수신기-1.0-SNAPSHOT.jar** 하 고 **spark-수신기-loganalytics-1.0-SNAPSHOT.jar** 이전에 만든 디렉터리로:

    ```bash
    dbfs cp ./src/spark-listeners/target/spark-listeners-1.0-SNAPSHOT.jar dbfs:/databricks/monitoring-staging/spark-listeners-1.0-SNAPSHOT.jar
    dbfs cp ./src/spark-listeners-loganalytics/target/spark-listeners-loganalytics-1.0-SNAPSHOT.jar dbfs:/databricks/monitoring-staging/spark-listeners-loganalytics-1.0-SNAPSHOT.jar
    ```

## <a name="create-and-configure-an-azure-databricks-cluster"></a>만들기 및 Azure Databricks 클러스터를 구성 합니다.

를 만들고 Azure Databricks 클러스터를 구성 하려면 다음이 단계를 수행 합니다.

1. Azure portal에서 Azure Databricks 작업 영역으로 이동 합니다.
1. 홈 페이지에서 클릭 **새 클러스터**합니다.
1. 클러스터의 이름을 입력 합니다 **클러스터 이름** 입력란입니다.
1. 에 **Databricks 런타임 버전** 드롭다운 **4.3** 이상.
1. 아래 **고급 옵션**를 클릭 합니다 **Spark** 탭 합니다. 다음 이름-값 쌍을 입력 합니다 **Spark 구성** 텍스트 상자:

    ```
    spark.extraListeners com.databricks.backend.daemon.driver.DBCEventLoggingListener,org.apache.spark.listeners.UnifiedSparkListener
    spark.unifiedListener.sink org.apache.spark.listeners.sink.loganalytics.LogAnalyticsListenerSink
    spark.unifiedListener.logBlockUpdates false
    ```

1. 여전히 아래에 있는 동안 합니다 **Spark** 탭에서 다음 값을 입력 합니다 **환경 변수** 텍스트 상자:

    ```
    LOG_ANALYTICS_WORKSPACE_ID=[your Azure Log Analytics workspace ID]
    LOG_ANALYTICS_WORKSPACE_KEY=[your Azure Log Analytics workspace key]
    ```

    ![Databricks UI의 스크린샷](./_images/create-cluster1.png)

1. 여전히 아래에 있는 동안 합니다 **고급 옵션** 섹션을 클릭 합니다 **Init 스크립트** 탭 합니다.
1. 에 **대상** 드롭다운 **DBFS**합니다.
1. 아래 **Init 스크립트 경로**, 입력 `dbfs:/databricks/monitoring-staging/listeners.sh`합니다.
1. **추가**를 클릭합니다.

    ![Databricks UI의 스크린샷](./_images/create-cluster2.png)

1. 클릭 **클러스터 만들기** 클러스터를 만듭니다.

## <a name="view-metrics"></a>메트릭 보기

다음이 단계를 완료 하면 Databricks 클러스터는 Azure Monitor로 클러스터 자체에 대 한 몇 가지 메트릭 데이터를 스트리밍합니다. 아래에서 Azure Log Analytics 작업 영역에서이 로그 데이터를 사용할 수는 "활성 | 사용자 지정 로그 | SparkMetric_CL"스키마입니다. 기록 되는 메트릭 유형에 대 한 자세한 내용은 참조 하세요. [메트릭을 Core](https://metrics.dropwizard.io/4.0.0/manual/core.html) Dropwizard 설명서에서. 계기, 카운터 또는 히스토그램와 같은 메트릭 형식에는 Type 필드에 기록 됩니다.

또한 라이브러리는 Apache Spark 수준 이벤트 및 Azure Monitor로 작업에서 Spark Structured Streaming 메트릭 스트리밍합니다. 이러한 이벤트 및 메트릭 용 응용 프로그램 코드를 변경 하지 않아도 됩니다. Spark Structured Streaming 로그 데이터는 사용할 수 있습니다는 "활성 | 사용자 지정 로그 | SparkListenerEvent_CL"스키마입니다.

![Log Analytics 작업 영역 스크린샷](./_images/workspace.png)

로그를 보는 방법에 대 한 자세한 내용은 참조 하세요. [보기 및 Azure Monitor의 로그 데이터를 분석](/azure/azure-monitor/log-query/portals)합니다.

## <a name="next-steps"></a>다음 단계

이 문서에서는 Azure Monitor로 메트릭을 전송 하 여 클러스터를 구성 하는 방법을 설명 합니다. 다음 문서에서는 Azure Databricks 모니터링 라이브러리를 사용 하 여 응용 프로그램 메트릭 및 로그를 보내는 방법을 보여 줍니다.

> [!div class="nextstepaction"]
> [Azure Monitor 응용 프로그램 로그 보내기](./application-logs.md)
