---
title: Azure Monitor에 메트릭을 보내도록 Azure Databricks를 구성 합니다.
description: 메트릭의 모니터링 및 Azure Log Analytics에서 데이터의 로깅을 사용할 수 있도록 scala 라이브러리
author: petertaylor9999
ms.date: 03/26/2019
ms.openlocfilehash: af6b6433f87964ac60c179ecf498e54129344126
ms.sourcegitcommit: 9854bd27fb5cf92041bbfb743d43045cd3552a69
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58503437"
---
<!-- markdownlint-disable MD040 -->

# <a name="configure-azure-databricks-to-send-metrics-to-azure-monitor"></a><span data-ttu-id="bc64c-103">Azure Monitor에 메트릭을 보내도록 Azure Databricks를 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-103">Configure Azure Databricks to send metrics to Azure Monitor</span></span>

<span data-ttu-id="bc64c-104">이 문서에 메트릭을 보내도록 Azure Databricks 클러스터를 구성 하는 방법을 보여 줍니다는 [Log Analytics 작업 영역](/azure/azure-monitor/platform/manage-access)합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-104">This article shows how to configure an Azure Databricks cluster to send metrics to a [Log Analytics workspace](/azure/azure-monitor/platform/manage-access).</span></span> <span data-ttu-id="bc64c-105">사용 된 [Azure Databricks 모니터링 라이브러리](https://github.com/mspnp/spark-monitoring), GitHub에서 사용할 수 있는 합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-105">It uses the [Azure Databricks Monitoring Library](https://github.com/mspnp/spark-monitoring), which is available on GitHub.</span></span> <span data-ttu-id="bc64c-106">Java, Scala 및 Maven에 대 한 이해가 prerequisistes 사례로 권장 됩니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-106">Understanding of Java, Scala, and Maven are recommended as prerequisistes.</span></span>

## <a name="about-the-azure-databricks-monitoring-library"></a><span data-ttu-id="bc64c-107">라이브러리를 모니터링 하는 Azure Databricks에 대 한</span><span class="sxs-lookup"><span data-stu-id="bc64c-107">About the Azure Databricks Monitoring Library</span></span>

<span data-ttu-id="bc64c-108">합니다 [GitHub 리포지토리에서](https://github.com/mspnp/spark-monitoring) Azure Databricks 모니터링 라이브러리에는 다음 디렉터리 구조에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-108">The [GitHub repo](https://github.com/mspnp/spark-monitoring) for the Azure Databricks Monitoring Library has following directory structure:</span></span>

```
/src  
    /spark-jobs  
    /spark-listeners-loganalytics  
    /spark-listeners  
    /pom.xml  
```

<span data-ttu-id="bc64c-109">합니다 **spark 작업** 디렉터리는 Spark 응용 프로그램 메트릭 카운터를 구현 하는 방법을 보여 주는 샘플 Spark 응용 프로그램입니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-109">The **spark-jobs** directory is a sample Spark application demonstrating how to implement a Spark application metric counter.</span></span>

<span data-ttu-id="bc64c-110">합니다 **spark 수신기** 디렉터리에 Apache Spark로 이벤트를 보내는 서비스 수준에서 Azure Log Analytics 작업 영역을 Azure Databricks를 사용 하도록 설정 하는 기능이 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-110">The **spark-listeners** directory includes functionality that enables Azure Databricks to send Apache Spark events at the service level to an Azure Log Analytics workspace.</span></span> <span data-ttu-id="bc64c-111">Azure Databricks는 데이터 집합, DataFrames 및 SQL을 사용 하 여 데이터를 처리 하는 일괄 처리에 대 한 구조화 된 Api 집합을 포함 하는 Apache Spark 기반 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-111">Azure Databricks is a service based on Apache Spark, which includes a set of structured APIs for batch processing data using Datasets, DataFrames, and SQL.</span></span> <span data-ttu-id="bc64c-112">Apache Spark 2.0 지원에 대 한 추가 되었습니다 [Structured Streaming](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html), 데이터 스트림 Api를 처리 하는 Spark의 일괄 처리 기반 API를 처리 합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-112">With Apache Spark 2.0, support was added for [Structured Streaming](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html), a data stream processing API built on Spark's batch processing APIs.</span></span>

<span data-ttu-id="bc64c-113">합니다 **spark-수신기-loganalytics** 디렉터리 Spark 수신기, DropWizard에 대 한 싱크 및 Azure Log Analytics 작업 영역에 대 한 클라이언트에 대 한 싱크를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-113">The **spark-listeners-loganalytics** directory includes a sink for Spark listeners, a sink for DropWizard, and a client for an Azure Log Analytics Workspace.</span></span> <span data-ttu-id="bc64c-114">이 디렉터리에는 Apache Spark 응용 프로그램 로그에 대 한 log4j 어 펜더를도 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-114">This directory also includes a log4j Appender for your Apache Spark application logs.</span></span>

<span data-ttu-id="bc64c-115">합니다 **spark-수신기-loganalytics** 하 고 **spark 수신기** Databricks 클러스터에 배포 된 두 개의 JAR 파일을 작성 하는 것에 대 한 코드를 포함 하는 디렉터리입니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-115">The **spark-listeners-loganalytics** and **spark-listeners** directories contain the code for building the two JAR files that are deployed to the Databricks cluster.</span></span> <span data-ttu-id="bc64c-116">합니다 **spark 수신기** 디렉터리를 **스크립트** JAR을 복사 하는 클러스터 노드 초기화 스크립트를 포함 하는 디렉터리를 Azure Databricks 파일 시스템의 준비 디렉터리에서 파일 실행 노드입니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-116">The **spark-listeners** directory includes a **scripts** directory that contains a cluster node initialization script to copy the JAR files from a staging directory in the Azure Databricks file system to execution nodes.</span></span>

<span data-ttu-id="bc64c-117">합니다 **pom.xml** 파일은 전체 프로젝트에 대 한 기본 Maven 빌드 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-117">The **pom.xml** file is the main Maven build file for the entire project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bc64c-118">필수 조건</span><span class="sxs-lookup"><span data-stu-id="bc64c-118">Prerequisites</span></span>

<span data-ttu-id="bc64c-119">시작 하려면 다음과 같은 Azure 리소스를 배포 합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-119">To get started, deploy the following Azure resources:</span></span>

- <span data-ttu-id="bc64c-120">Azure Databricks 작업 영역입니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-120">An Azure Databricks workspace.</span></span> <span data-ttu-id="bc64c-121">[빠른 시작: Azure portal을 사용 하 여 Azure Databricks에서 Spark 작업 실행](/azure/azure-databricks/quickstart-create-databricks-workspace-portal)합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-121">See [Quickstart: Run a Spark job on Azure Databricks using the Azure portal](/azure/azure-databricks/quickstart-create-databricks-workspace-portal).</span></span>
- <span data-ttu-id="bc64c-122">Log Analytics 작업 영역.</span><span class="sxs-lookup"><span data-stu-id="bc64c-122">A Log Analytics workspace.</span></span> <span data-ttu-id="bc64c-123">참조 [Azure portal에서 Log Analytics 작업 영역 만들기](/azure/azure-monitor/learn/quick-create-workspace)합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-123">See [Create a Log Analytics workspace in the Azure portal](/azure/azure-monitor/learn/quick-create-workspace).</span></span>

<span data-ttu-id="bc64c-124">그런 다음 설치 합니다 [Azure Databricks CLI](https://docs.databricks.com/user-guide/dev-tools/databricks-cli.html#install-the-cli)합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-124">Next, install the [Azure Databricks CLI](https://docs.databricks.com/user-guide/dev-tools/databricks-cli.html#install-the-cli).</span></span> <span data-ttu-id="bc64c-125">Azure Databricks 개인용 액세스 토큰을 CLI를 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-125">An Azure Databricks personal access token is required to use the CLI.</span></span> <span data-ttu-id="bc64c-126">자세한 내용은 [토큰을 생성할](https://docs.azuredatabricks.net/api/latest/authentication.html#token-management)합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-126">For instructions, see [Generate a token](https://docs.azuredatabricks.net/api/latest/authentication.html#token-management).</span></span>

<span data-ttu-id="bc64c-127">Log Analytics 작업 영역 ID 및 키를 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-127">Find your Log Analytics workspace ID and key.</span></span> <span data-ttu-id="bc64c-128">자세한 내용은 [작업 영역 ID 및 키 가져오기](/azure/azure-monitor/platform/agent-windows#obtain-workspace-id-and-key)합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-128">For instructions, see [Obtain workspace ID and key](/azure/azure-monitor/platform/agent-windows#obtain-workspace-id-and-key).</span></span> <span data-ttu-id="bc64c-129">Databricks 클러스터를 구성한 경우 이러한 값이 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-129">You will need these values when you configure the Databricks cluster.</span></span>

<span data-ttu-id="bc64c-130">모니터링 라이브러리를 작성 하려면 다음 리소스를 사용 하 여 Java IDE를 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-130">To build the monitoring library, you will need a Java IDE with the following resources:</span></span>

- [<span data-ttu-id="bc64c-131">Java 개발 키트 (JDK) 버전 1.8</span><span class="sxs-lookup"><span data-stu-id="bc64c-131">Java Development Kit (JDK) version 1.8</span></span>](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
- [<span data-ttu-id="bc64c-132">Scala SDK 2.11 언어</span><span class="sxs-lookup"><span data-stu-id="bc64c-132">Scala language SDK 2.11</span></span>](https://www.scala-lang.org/download/)
- [<span data-ttu-id="bc64c-133">Apache Maven 3.5.4</span><span class="sxs-lookup"><span data-stu-id="bc64c-133">Apache Maven 3.5.4</span></span>](http://maven.apache.org/download.cgi)

## <a name="build-the-azure-databricks-monitoring-library"></a><span data-ttu-id="bc64c-134">라이브러리를 모니터링 하는 Azure Databricks를 빌드</span><span class="sxs-lookup"><span data-stu-id="bc64c-134">Build the Azure Databricks Monitoring Library</span></span>

<span data-ttu-id="bc64c-135">Azure Databricks 모니터링 라이브러리를 작성 하려면 다음 단계를 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-135">To build the Azure Databricks Monitoring Library, perform the following steps:</span></span>

1. <span data-ttu-id="bc64c-136">복제, 포크 또는 다운로드 합니다 [GitHub 리포지토리](https://github.com/mspnp/spark-monitoring)합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-136">Clone, fork, or download the [GitHub repository](https://github.com/mspnp/spark-monitoring).</span></span>

1. <span data-ttu-id="bc64c-137">Maven 프로젝트 개체 모델 파일을 가져옵니다 _pom.xml_에 있는 합니다 **/src** 프로젝트 폴더입니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-137">Import the Maven project object model file, _pom.xml_, located in the **/src** folder into your project.</span></span> <span data-ttu-id="bc64c-138">이 옵션은 세 개의 프로젝트를 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-138">This will import three projects:</span></span>

    - <span data-ttu-id="bc64c-139">spark-jobs</span><span class="sxs-lookup"><span data-stu-id="bc64c-139">spark-jobs</span></span>
    - <span data-ttu-id="bc64c-140">spark 수신기</span><span class="sxs-lookup"><span data-stu-id="bc64c-140">spark-listeners</span></span>
    - <span data-ttu-id="bc64c-141">spark-listeners-loganalytics</span><span class="sxs-lookup"><span data-stu-id="bc64c-141">spark-listeners-loganalytics</span></span>

1. <span data-ttu-id="bc64c-142">Maven을 실행 **패키지** 이러한 각 프로젝트에 대 한 JAR 파일을 빌드하기 위해 Java IDE에서 빌드 단계:</span><span class="sxs-lookup"><span data-stu-id="bc64c-142">Execute the Maven **package** build phase in your Java IDE to build the JAR files for each of these projects:</span></span>

    |<span data-ttu-id="bc64c-143">Project</span><span class="sxs-lookup"><span data-stu-id="bc64c-143">Project</span></span>| <span data-ttu-id="bc64c-144">JAR 파일</span><span class="sxs-lookup"><span data-stu-id="bc64c-144">JAR file</span></span>|
    |-------|---------|
    |<span data-ttu-id="bc64c-145">spark-jobs</span><span class="sxs-lookup"><span data-stu-id="bc64c-145">spark-jobs</span></span>|<span data-ttu-id="bc64c-146">spark-jobs-1.0-SNAPSHOT.jar</span><span class="sxs-lookup"><span data-stu-id="bc64c-146">spark-jobs-1.0-SNAPSHOT.jar</span></span>|
    |<span data-ttu-id="bc64c-147">spark 수신기</span><span class="sxs-lookup"><span data-stu-id="bc64c-147">spark-listeners</span></span>|<span data-ttu-id="bc64c-148">spark-listeners-1.0-SNAPSHOT.jar</span><span class="sxs-lookup"><span data-stu-id="bc64c-148">spark-listeners-1.0-SNAPSHOT.jar</span></span>|
    |<span data-ttu-id="bc64c-149">spark-listeners-loganalytics</span><span class="sxs-lookup"><span data-stu-id="bc64c-149">spark-listeners-loganalytics</span></span>|<span data-ttu-id="bc64c-150">spark-listeners-loganalytics-1.0-SNAPSHOT.jar</span><span class="sxs-lookup"><span data-stu-id="bc64c-150">spark-listeners-loganalytics-1.0-SNAPSHOT.jar</span></span>|

1. <span data-ttu-id="bc64c-151">Azure Databricks CLI를 사용 하 여 라는 디렉터리를 만들려면 **dbfs: / databricks/모니터링-스테이징**:</span><span class="sxs-lookup"><span data-stu-id="bc64c-151">Use the Azure Databricks CLI to create a directory named **dbfs:/databricks/monitoring-staging**:</span></span>  

    ```bash
    dbfs mkdirs dbfs:/databricks/monitoring-staging
    ```

1. <span data-ttu-id="bc64c-152">Azure Databricks CLI를 사용 하 여 복사할 **/src/spark-listeners/scripts/listeners.sh** 이전 디렉터리:</span><span class="sxs-lookup"><span data-stu-id="bc64c-152">Use the Azure Databricks CLI to copy **/src/spark-listeners/scripts/listeners.sh** to the directory previously:</span></span>

    ```bash
    dbfs cp ./src/spark-listeners/scripts/listeners.sh dbfs:/databricks/monitoring-staging/listeners.sh
    ```

1. <span data-ttu-id="bc64c-153">Azure Databricks CLI를 사용 하 여 복사할 **/src/spark-listeners/scripts/metrics.properties** 이전에 만든 디렉터리로:</span><span class="sxs-lookup"><span data-stu-id="bc64c-153">Use the Azure Databricks CLI to copy **/src/spark-listeners/scripts/metrics.properties** to the directory created previously:</span></span>

    ```bash
    dbfs cp <local path to metrics.properties> dbfs:/databricks/monitoring-staging/metrics.properties
    ```

1. <span data-ttu-id="bc64c-154">Azure Databricks CLI를 사용 하 여 복사할 **spark-수신기-1.0-SNAPSHOT.jar** 하 고 **spark-수신기-loganalytics-1.0-SNAPSHOT.jar** 이전에 만든 디렉터리로:</span><span class="sxs-lookup"><span data-stu-id="bc64c-154">Use the Azure Databricks CLI to copy **spark-listeners-1.0-SNAPSHOT.jar** and **spark-listeners-loganalytics-1.0-SNAPSHOT.jar** to the directory created previously:</span></span>

    ```bash
    dbfs cp ./src/spark-listeners/target/spark-listeners-1.0-SNAPSHOT.jar dbfs:/databricks/monitoring-staging/spark-listeners-1.0-SNAPSHOT.jar
    dbfs cp ./src/spark-listeners-loganalytics/target/spark-listeners-loganalytics-1.0-SNAPSHOT.jar dbfs:/databricks/monitoring-staging/spark-listeners-loganalytics-1.0-SNAPSHOT.jar
    ```

## <a name="create-and-configure-an-azure-databricks-cluster"></a><span data-ttu-id="bc64c-155">만들기 및 Azure Databricks 클러스터를 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-155">Create and configure an Azure Databricks cluster</span></span>

<span data-ttu-id="bc64c-156">를 만들고 Azure Databricks 클러스터를 구성 하려면 다음이 단계를 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-156">To create and configure an Azure Databricks cluster, follow these steps:</span></span>

1. <span data-ttu-id="bc64c-157">Azure portal에서 Azure Databricks 작업 영역으로 이동 합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-157">Navigate to your Azure Databricks workspace in the Azure portal.</span></span>
1. <span data-ttu-id="bc64c-158">홈 페이지에서 클릭 **새 클러스터**합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-158">On the home page, click **New Cluster**.</span></span>
1. <span data-ttu-id="bc64c-159">클러스터의 이름을 입력 합니다 **클러스터 이름** 입력란입니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-159">Enter a name for your cluster in the **Cluster Name** text box.</span></span>
1. <span data-ttu-id="bc64c-160">에 **Databricks 런타임 버전** 드롭다운 **4.3** 이상.</span><span class="sxs-lookup"><span data-stu-id="bc64c-160">In the **Databricks Runtime Version** dropdown, select **4.3** or greater.</span></span>
1. <span data-ttu-id="bc64c-161">아래 **고급 옵션**를 클릭 합니다 **Spark** 탭 합니다. 다음 이름-값 쌍을 입력 합니다 **Spark 구성** 텍스트 상자:</span><span class="sxs-lookup"><span data-stu-id="bc64c-161">Under **Advanced Options**, click on the **Spark** tab. Enter the following name-value pairs in the **Spark Config** text box:</span></span>

    ```
    spark.extraListeners com.databricks.backend.daemon.driver.DBCEventLoggingListener,org.apache.spark.listeners.UnifiedSparkListener
    spark.unifiedListener.sink org.apache.spark.listeners.sink.loganalytics.LogAnalyticsListenerSink
    spark.unifiedListener.logBlockUpdates false
    ```

1. <span data-ttu-id="bc64c-162">여전히 아래에 있는 동안 합니다 **Spark** 탭에서 다음 값을 입력 합니다 **환경 변수** 텍스트 상자:</span><span class="sxs-lookup"><span data-stu-id="bc64c-162">While still under the **Spark** tab, enter the following values in the **Environment Variables** text box:</span></span>

    ```
    LOG_ANALYTICS_WORKSPACE_ID=[your Azure Log Analytics workspace ID]
    LOG_ANALYTICS_WORKSPACE_KEY=[your Azure Log Analytics workspace key]
    ```

    ![Databricks UI의 스크린샷](./_images/create-cluster1.png)

1. <span data-ttu-id="bc64c-164">여전히 아래에 있는 동안 합니다 **고급 옵션** 섹션을 클릭 합니다 **Init 스크립트** 탭 합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-164">While still under the **Advanced Options** section, click on the **Init Scripts** tab.</span></span>
1. <span data-ttu-id="bc64c-165">에 **대상** 드롭다운 **DBFS**합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-165">In the **Destination** dropdown, select **DBFS**.</span></span>
1. <span data-ttu-id="bc64c-166">아래 **Init 스크립트 경로**, 입력 `dbfs:/databricks/monitoring-staging/listeners.sh`합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-166">Under **Init Script Path**, enter `dbfs:/databricks/monitoring-staging/listeners.sh`.</span></span>
1. <span data-ttu-id="bc64c-167">**추가**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-167">Click **Add**.</span></span>

    ![Databricks UI의 스크린샷](./_images/create-cluster2.png)

1. <span data-ttu-id="bc64c-169">클릭 **클러스터 만들기** 클러스터를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-169">Click **Create Cluster** to create the cluster.</span></span>

## <a name="view-metrics"></a><span data-ttu-id="bc64c-170">메트릭 보기</span><span class="sxs-lookup"><span data-stu-id="bc64c-170">View metrics</span></span>

<span data-ttu-id="bc64c-171">다음이 단계를 완료 하면 Databricks 클러스터는 Azure Monitor로 클러스터 자체에 대 한 몇 가지 메트릭 데이터를 스트리밍합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-171">After you complete these steps, your Databricks cluster streams some metric data about the cluster itself to Azure Monitor.</span></span> <span data-ttu-id="bc64c-172">아래에서 Azure Log Analytics 작업 영역에서이 로그 데이터를 사용할 수는 "활성 | 사용자 지정 로그 | SparkMetric_CL"스키마입니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-172">This log data is available in your Azure Log Analytics workspace under the "Active | Custom Logs | SparkMetric_CL" schema.</span></span> <span data-ttu-id="bc64c-173">기록 되는 메트릭 유형에 대 한 자세한 내용은 참조 하세요. [메트릭을 Core](https://metrics.dropwizard.io/4.0.0/manual/core.html) Dropwizard 설명서에서.</span><span class="sxs-lookup"><span data-stu-id="bc64c-173">For more information about the types of metrics that are logged, see [Metrics Core](https://metrics.dropwizard.io/4.0.0/manual/core.html) in the Dropwizard documentation.</span></span> <span data-ttu-id="bc64c-174">계기, 카운터 또는 히스토그램와 같은 메트릭 형식에는 Type 필드에 기록 됩니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-174">The metric type, such as gauge, counter, or histogram, is written to the Type field.</span></span>

<span data-ttu-id="bc64c-175">또한 라이브러리는 Apache Spark 수준 이벤트 및 Azure Monitor로 작업에서 Spark Structured Streaming 메트릭 스트리밍합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-175">In addition, the library streams Apache Spark level events and Spark Structured Streaming metrics from your jobs to Azure Monitor.</span></span> <span data-ttu-id="bc64c-176">이러한 이벤트 및 메트릭 용 응용 프로그램 코드를 변경 하지 않아도 됩니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-176">You don't need to make any changes to your application code for these events and metrics.</span></span> <span data-ttu-id="bc64c-177">Spark Structured Streaming 로그 데이터는 사용할 수 있습니다는 "활성 | 사용자 지정 로그 | SparkListenerEvent_CL"스키마입니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-177">Spark Structured Streaming log data is available under the "Active | Custom Logs | SparkListenerEvent_CL" schema.</span></span>

![Log Analytics 작업 영역 스크린샷](./_images/workspace.png)

<span data-ttu-id="bc64c-179">로그를 보는 방법에 대 한 자세한 내용은 참조 하세요. [보기 및 Azure Monitor의 로그 데이터를 분석](/azure/azure-monitor/log-query/portals)합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-179">For more information about viewing logs, see [Viewing and analyzing log data in Azure Monitor](/azure/azure-monitor/log-query/portals).</span></span>

## <a name="next-steps"></a><span data-ttu-id="bc64c-180">다음 단계</span><span class="sxs-lookup"><span data-stu-id="bc64c-180">Next steps</span></span>

<span data-ttu-id="bc64c-181">이 문서에서는 Azure Monitor로 메트릭을 전송 하 여 클러스터를 구성 하는 방법을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-181">This article described how to configure your cluster to send metrics to Azure Monitor.</span></span> <span data-ttu-id="bc64c-182">다음 문서에서는 Azure Databricks 모니터링 라이브러리를 사용 하 여 응용 프로그램 메트릭 및 로그를 보내는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="bc64c-182">The next article shows how to use the Azure Databricks Monitoring Library to send application metrics and logs.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="bc64c-183">Azure Monitor 응용 프로그램 로그 보내기</span><span class="sxs-lookup"><span data-stu-id="bc64c-183">Send application logs to Azure Monitor</span></span>](./application-logs.md)
