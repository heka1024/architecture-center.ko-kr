---
title: Azure Monitor를 사용 하 여 Azure Databricks에 대 한 문제를 해결 하는 성능
titleSuffix: ''
description: Grafana 대시보드를 사용 하 여 Azure Databricks에서 성능 문제를 해결 하려면
author: petertaylor9999
ms.date: 04/02/2019
ms.topic: ''
ms.service: ''
ms.subservice: ''
ms.openlocfilehash: 49ec63d0c45ab388ca83b3ab0562428327539619
ms.sourcegitcommit: 1a3cc91530d56731029ea091db1f15d41ac056af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58888113"
---
# <a name="troubleshoot-performance-bottlenecks-in-azure-databricks"></a><span data-ttu-id="56213-103">Azure Databricks에서 성능 병목 현상 문제 해결</span><span class="sxs-lookup"><span data-stu-id="56213-103">Troubleshoot performance bottlenecks in Azure Databricks</span></span>

<span data-ttu-id="56213-104">이 문서에서는 Azure Databricks에서 Spark 작업에서 성능 병목 현상의 찾을 모니터링 대시보드를 사용 하는 방법을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-104">This article describes how to use monitoring dashboards to find performance bottlenecks in Spark jobs on Azure Databricks.</span></span>

<span data-ttu-id="56213-105">[Azure Databricks](/azure/azure-databricks/) 은 [Apache Spark](https://spark.apache.org/)– 쉽게 빠르게 개발 및 빅 데이터 분석을 배포 하는 분석 서비스를 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-105">[Azure Databricks](/azure/azure-databricks/) is an [Apache Spark](https://spark.apache.org/)–based analytics service that makes it easy to rapidly develop and deploy big data analytics.</span></span> <span data-ttu-id="56213-106">모니터링 및 성능 문제 해결이 프로덕션 Azure Databricks 워크 로드를 작동 하는 경우에 중요 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-106">Monitoring and troubleshooting performance issues is a critical when operating production Azure Databricks workloads.</span></span> <span data-ttu-id="56213-107">일반적인 성능 문제를 식별 하려면 원격 분석 데이터를 기반으로 하는 모니터링 시각화를 사용 하는 것이 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-107">To identify common performance issues, it's helpful to use monitoring visualizations based on telemetry data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="56213-108">필수 조건</span><span class="sxs-lookup"><span data-stu-id="56213-108">Prerequisites</span></span>

<span data-ttu-id="56213-109">이 문서에 나와 있는 Grafana 대시보드를 설정 하려면:</span><span class="sxs-lookup"><span data-stu-id="56213-109">To set up the Grafana dashboards shown in this article:</span></span>

- <span data-ttu-id="56213-110">Azure Databricks 모니터링 라이브러리를 사용 하 여 Log Analytics 작업 영역에 원격 분석을 보내도록 Databricks 클러스터를 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-110">Configure your Databricks cluster to send telemetry to a Log Analytics workspace, using the Azure Databricks Monitoring Library.</span></span> <span data-ttu-id="56213-111">자세한 내용은 참조 하세요 [메트릭을 Azure Monitor로 보내도록 Azure Databricks 구성](./configure-cluster.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-111">For details, see [Configure Azure Databricks to send metrics to Azure Monitor](./configure-cluster.md).</span></span>

- <span data-ttu-id="56213-112">가상 컴퓨터에서 Grafana를 배포 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-112">Deploy Grafana in a virtual machine.</span></span> <span data-ttu-id="56213-113">참조 [대시보드를 사용 하 여 Azure Databricks 메트릭을 시각화](./dashboards.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-113">See [Use dashboards to visualize Azure Databricks metrics](./dashboards.md).</span></span>

<span data-ttu-id="56213-114">Grafana 대시보드 배포 되는 시계열 시각화는 집합을 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-114">The Grafana dashboard that is deployed includes a set of time-series visualizations.</span></span> <span data-ttu-id="56213-115">각 그래프는 시계열 도표를 Apache Spark 작업에 관련 된 메트릭 작업 및 각 단계를 구성 하는 작업의 단계입니다.</span><span class="sxs-lookup"><span data-stu-id="56213-115">Each graph is time-series plot of metrics related to an Apache Spark job, the stages of the job, and tasks that make up each stage.</span></span>

## <a name="azure-databricks-performance-overview"></a><span data-ttu-id="56213-116">Azure Databricks 성능 개요</span><span class="sxs-lookup"><span data-stu-id="56213-116">Azure Databricks performance overview</span></span>

<span data-ttu-id="56213-117">Azure Databricks는 Apache Spark, 범용 분산된 컴퓨팅 시스템을 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-117">Azure Databricks is based on Apache Spark, a general-purpose distributed computing system.</span></span> <span data-ttu-id="56213-118">라고 하는 응용 프로그램 코드를 **작업**, 클러스터 관리자가 조정 하는 Apache Spark 클러스터에서 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-118">Application code, known as a **job**, executes on an Apache Spark cluster, coordinated by the cluster manager.</span></span> <span data-ttu-id="56213-119">일반적으로 작업은 최상위 단위 계산입니다.</span><span class="sxs-lookup"><span data-stu-id="56213-119">In general, a job is the highest-level unit of computation.</span></span> <span data-ttu-id="56213-120">작업을 Spark 응용 프로그램에서 수행 된 전체 작업을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="56213-120">A job represents the complete operation performed by the Spark application.</span></span> <span data-ttu-id="56213-121">일반적인 작업을 원본에서 데이터를 읽고, 데이터 변환 적용 및 결과 저장소 또는 다른 대상에 쓰기를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-121">A typical operation includes reading data from a source, applying data transformations, and writing the results to storage or another destination.</span></span>

<span data-ttu-id="56213-122">작업으로 분할 됩니다 **단계**합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-122">Jobs are broken down into **stages**.</span></span> <span data-ttu-id="56213-123">작업 진행 단계를 통해 순차적으로 이후 단계 이전 단계를 완료 대기 해야 함을 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-123">The job advances through the stages sequentially, which means that later stages must wait for earlier stages to complete.</span></span> <span data-ttu-id="56213-124">동일한 그룹을 포함 하는 단계 **작업** Spark 클러스터의 여러 노드에서 동시에 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="56213-124">Stages contain groups of identical **tasks** that can be executed in parallel on multiple nodes of the Spark cluster.</span></span> <span data-ttu-id="56213-125">작업은 데이터의 하위 집합에서 수행 하는 실행의 가장 세분화 된 단위입니다.</span><span class="sxs-lookup"><span data-stu-id="56213-125">Tasks are the most granular unit of execution taking place on a subset of the data.</span></span>

<span data-ttu-id="56213-126">다음 섹션에서는 성능 문제 해결에 유용한 일부 대시보드 시각화에 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-126">The next sections describe some dashboard visualizations that are useful for performance troubleshooting.</span></span>

## <a name="job-and-stage-latency"></a><span data-ttu-id="56213-127">단계와 작업 미만의 대기 시간</span><span class="sxs-lookup"><span data-stu-id="56213-127">Job and stage latency</span></span>

<span data-ttu-id="56213-128">작업 대기 시간에서 작업 실행 기간 때 완료 될 때까지 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-128">Job latency is the duration of a job execution from when it starts until it completes.</span></span> <span data-ttu-id="56213-129">이상 값의 시각화를 허용 하도록 클러스터 및 응용 프로그램 ID 당 작업 실행의 백분위 수로 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="56213-129">It is shown as percentiles of a job execution per cluster and application ID, to allow the visualization of outliers.</span></span> <span data-ttu-id="56213-130">다음 그래프에서 작업 기록의 경우 90 번째 백분위 수 50 초에 도달 하면 여기서 50 번째 백분위 수를 10 초 정도 일관 되 게 하는 경우에 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-130">The following graph shows a job history where the 90th percentile reached 50 seconds, even though the 50th percentile was consistently around 10 seconds.</span></span>

![작업 대기 시간을 보여 주는 그래프](./_images/grafana-job-latency.png)

<span data-ttu-id="56213-132">클러스터 및 응용 프로그램, 대기 시간에서 급증을 찾고 작업 실행을 조사 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-132">Investigate job execution by cluster and application, looking for spikes in latency.</span></span> <span data-ttu-id="56213-133">클러스터 및 대기 시간이 긴 응용 프로그램을 식별 한 후 이동할 단계 대기 시간을 조사 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-133">Once clusters and applications with high latency are identified, move on to investigate stage latency.</span></span>

<span data-ttu-id="56213-134">스테이지 대기 시간 이상 값의 시각화를 허용 하도록 백분위 수로 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="56213-134">Stage latency is also shown as percentiles to allow the visualization of outliers.</span></span> <span data-ttu-id="56213-135">스테이지 대기 시간 클러스터, 응용 프로그램 및 단계 이름에 따라 분류 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="56213-135">Stage latency is broken out by cluster, application, and stage name.</span></span> <span data-ttu-id="56213-136">태스크 보유 한 다시 단계의 완료를 확인 하려면 그래프의 작업 대기 시간 급증을 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-136">Identify spikes in task latency in the graph to determine which tasks are holding back completion of the stage.</span></span>

![스테이지 대기 시간을 보여 주는 그래프](./_images/grafana-stage-latency.png)

<span data-ttu-id="56213-138">클러스터 처리량 그래프 작업, 단계 및 분당 완료 된 작업 수를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="56213-138">The cluster throughput graph shows the number of jobs, stages, and tasks completed per minute.</span></span> <span data-ttu-id="56213-139">이 옵션을 사용 하면 상대 단계 및 작업 별로 작업 수를 기준으로 워크 로드를 이해 하도록 도와줍니다.</span><span class="sxs-lookup"><span data-stu-id="56213-139">This helps you to understand the workload in terms of the relative number of stages and tasks per job.</span></span> <span data-ttu-id="56213-140">여기서 확인할 수 있습니다 단계 수는 약 12 분 당 작업 수가 2 및 6 사이의 범위 &ndash; 분당 24입니다.</span><span class="sxs-lookup"><span data-stu-id="56213-140">Here you can see that the number of jobs per minute ranges between 2 and 6, while the number of stages is about 12 &ndash; 24 per minute.</span></span>

![그래프 표시 클러스터 처리량](./_images/grafana-cluster-throughput.png)

## <a name="sum-of-task-execution-latency"></a><span data-ttu-id="56213-142">총 작업 실행 대기 시간</span><span class="sxs-lookup"><span data-stu-id="56213-142">Sum of task execution latency</span></span>

<span data-ttu-id="56213-143">이 시각화는 호스트 클러스터에서 실행 당 작업 실행 대기의 합계를 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-143">This visualization shows the sum of task execution latency per host running on a cluster.</span></span> <span data-ttu-id="56213-144">이 그래프를 사용 하 여 인해 느려지는 호스트 클러스터 또는 작업 실행 기 당 misallocation 느리게 실행 되는 작업을 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-144">Use this graph to detect tasks that run slowly due to the host slowing down on a cluster, or a misallocation of tasks per executor.</span></span> <span data-ttu-id="56213-145">다음 그래프는 대부분의 호스트에는 30 초 정도의 합계입니다.</span><span class="sxs-lookup"><span data-stu-id="56213-145">In the following graph, most of the hosts have a sum of about 30 seconds.</span></span> <span data-ttu-id="56213-146">그러나 두 호스트의 경우 약 10 분 가리킨는 합계</span><span class="sxs-lookup"><span data-stu-id="56213-146">However, two of the hosts have sums that hover around 10 minutes.</span></span> <span data-ttu-id="56213-147">호스트는 느리게 실행 됩니다. 또는 실행 기 당 작업 수가 misallocated 됩니다.</span><span class="sxs-lookup"><span data-stu-id="56213-147">Either the hosts are running slow or the number of tasks per executor is misallocated.</span></span>

![호스트당 태스크 실행의 합계가 되는 그래프 표시](./_images/grafana-sum-task-exec.png)

<span data-ttu-id="56213-149">실행 기 당 작업 수가 병목 현상이 발생 하 두 명의 실행 기 수 작업의 불균형 할당 되어 있는지를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="56213-149">The number of tasks per executor shows that two executors are assigned a disproportionate number of tasks, causing a bottleneck.</span></span>

![실행 기 당 작업을 보여 주는 그래프](./_images/grafana-tasks-per-exec.png)

## <a name="task-metrics-per-stage"></a><span data-ttu-id="56213-151">단계 당 작업 메트릭</span><span class="sxs-lookup"><span data-stu-id="56213-151">Task metrics per stage</span></span>

<span data-ttu-id="56213-152">작업 메트릭 시각화 태스크 실행에 대 한 비용 분석을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-152">The task metrics visualization gives the cost breakdown for a task execution.</span></span> <span data-ttu-id="56213-153">사용할 수 있습니다 소요 된 상대적 시간 참조와 같은 serialization 및 deserialization 작업에 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-153">You can use it see the relative time spent on tasks such as serialization and deserialization.</span></span> <span data-ttu-id="56213-154">이 데이터에 최적화할 기회를 표시할 수 있습니다 &mdash; 를 사용 하 여 예를 들어 [변수](https://spark.apache.org/docs/2.2.0/rdd-programming-guide.html#broadcast-variables) 전달 데이터를 방지 하려면.</span><span class="sxs-lookup"><span data-stu-id="56213-154">This data might show opportunities to optimize &mdash; for example, by using [broadcast variables](https://spark.apache.org/docs/2.2.0/rdd-programming-guide.html#broadcast-variables) to avoid shipping data.</span></span> <span data-ttu-id="56213-155">작업 메트릭도 작업을 순서 섞기 데이터 크기를 표시 및 순서를 섞는 읽기 쓰기 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="56213-155">The task metrics also show the shuffle data size for a task, and the shuffle read and write times.</span></span> <span data-ttu-id="56213-156">이러한 값이 높으면, 네트워크를 통해 많은 양의 데이터를 이동 임을 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-156">If these values are high, it means that a lot of data is moving across the network.</span></span>

<span data-ttu-id="56213-157">다른 작업 메트릭은 스케줄러가 지연 되는 작업을 예약 하는 데 걸리는 시간을 측정 하는 경우</span><span class="sxs-lookup"><span data-stu-id="56213-157">Another task metric is the scheduler delay, which measures how long it takes to schedule a task.</span></span> <span data-ttu-id="56213-158">이상적으로이 값은 낮은 해야 실제로 작업을 실행 소요 되는 실행 기는 계산 시간에 비해, 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-158">Ideally, this value should be low compared to the executor compute time, which is the time spent actually executing the task.</span></span>

<span data-ttu-id="56213-159">다음 그래프와 스케줄러 지연 시간 (3.7 s) 실행자 계산 시간을 초과 하는 (1.1 s).</span><span class="sxs-lookup"><span data-stu-id="56213-159">The following graph shows a scheduler delay time (3.7 s) that exceeds the executor compute time (1.1 s).</span></span> <span data-ttu-id="56213-160">즉, 실제 작업을 수행 하는 보다 예약 작업에 대 한 대기 시간이 더 깁니다.</span><span class="sxs-lookup"><span data-stu-id="56213-160">That means more time is spent waiting for tasks to be scheduled than doing the actual work.</span></span>

![단계 당 작업 메트릭을 보여 주는 그래프](./_images/grafana-metrics-per-stage.png)

<span data-ttu-id="56213-162">이 경우 오버 헤드가 많이 발생 하는 너무 많은 파티션이 있으면에서 문제가 발생 했습니다.</span><span class="sxs-lookup"><span data-stu-id="56213-162">In this case, the problem was caused by having too many partitions, which caused a lot of overhead.</span></span> <span data-ttu-id="56213-163">파티션 수를 줄이면 스케줄러 지연 시간을 낮췄습니다.</span><span class="sxs-lookup"><span data-stu-id="56213-163">Reducing the number of partitions lowered the scheduler delay time.</span></span> <span data-ttu-id="56213-164">다음 그래프에서는 대부분의 작업을 실행 하는 데 소요 된.</span><span class="sxs-lookup"><span data-stu-id="56213-164">The next graph shows that most of the time is spent executing the task.</span></span>

![단계 당 작업 메트릭을 보여 주는 그래프](./_images/grafana-metrics-per-stage2.png)

## <a name="streaming-throughput-and-latency"></a><span data-ttu-id="56213-166">스트리밍 처리량 및 대기 시간</span><span class="sxs-lookup"><span data-stu-id="56213-166">Streaming throughput and latency</span></span>

<span data-ttu-id="56213-167">처리량 스트리밍을 직접 관련이 구조화 된 스트리밍 있습니다.</span><span class="sxs-lookup"><span data-stu-id="56213-167">Streaming throughput is directly related to structured streaming.</span></span> <span data-ttu-id="56213-168">두 가지 중요 한 메트릭이 있습니다 스트리밍 처리량을 사용 하 여 연결 합니다. 두 번째 및 처리 된 행 / 초 당 입력된 행입니다.</span><span class="sxs-lookup"><span data-stu-id="56213-168">There are two important metrics associated with streaming throughput: Input rows per second and processed rows per second.</span></span> <span data-ttu-id="56213-169">초당 입력된 행 outpaces 초당 처리 된 행, 스트림 처리 시스템 속도가 늦는 지 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-169">If input rows per second outpaces processed rows per second, it means the stream processing system is falling behind.</span></span> <span data-ttu-id="56213-170">또한 입력된 데이터를 Event Hubs 또는 Kafka에서 상태가 되 면 다음 입력된 행 / 초 해야 따라갈 프런트 엔드에 데이터 수집 속도.</span><span class="sxs-lookup"><span data-stu-id="56213-170">Also, if the input data comes from Event Hubs or Kafka, then input rows per second should keep up with the data ingestion rate at the front end.</span></span>

<span data-ttu-id="56213-171">두 작업은 매우 다양 한 스트리밍 메트릭 하지만 유사한 클러스터 처리량이 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="56213-171">Two jobs can have similar cluster throughput but very different streaming metrics.</span></span> <span data-ttu-id="56213-172">다음 스크린샷은 두 가지 다른 워크 로드를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="56213-172">The following screenshot shows two different workloads.</span></span> <span data-ttu-id="56213-173">이러한 클러스터 처리량 (작업, 단계 및 작업 분당) 측면에서 비슷합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-173">They are similar in terms of cluster throughput (jobs, stages, and tasks per minute).</span></span> <span data-ttu-id="56213-174">하지만 두 번째 실행을 4,000 행 수/초 및 12,000 행/초를 처리 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-174">But the second run processes 12,000 rows/sec versus 4,000 rows/sec.</span></span>

![그래프 보여 주는 스트리밍 처리량](./_images/grafana-streaming-throughput.png)

<span data-ttu-id="56213-176">스트리밍 처리량은 처리 되는 데이터 레코드의 수를 측정 하기 때문에 클러스터 처리량 보다 더 나은 비즈니스 메트릭을 경우가 많습니다.</span><span class="sxs-lookup"><span data-stu-id="56213-176">Streaming throughput is often a better business metric than cluster throughput, because it measures the number of data records that are processed.</span></span>

## <a name="resource-consumption-per-executor"></a><span data-ttu-id="56213-177">실행 기 당 리소스 사용량</span><span class="sxs-lookup"><span data-stu-id="56213-177">Resource consumption per executor</span></span>

<span data-ttu-id="56213-178">이러한 메트릭은 각 실행 기를 수행 하는 작업을 이해 하는 데 도움이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="56213-178">These metrics help to understand the work that each executor performs.</span></span>

<span data-ttu-id="56213-179">**비율 메트릭** 에서 전체 실행 기는 계산 시간에 소요 된 시간의 백분율로 표현 되는 다양 한 항목을 보내는 실행 기를 얼마나 많은 시간을 측정 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-179">**Percentage metrics** measure how much time an executor spends on various things, expressed as a ratio of time spent versus the overall executor compute time.</span></span> <span data-ttu-id="56213-180">메트릭은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="56213-180">The metrics are:</span></span>

- <span data-ttu-id="56213-181">% 시간 serialize</span><span class="sxs-lookup"><span data-stu-id="56213-181">% Serialize time</span></span>
- <span data-ttu-id="56213-182">% 시간을 deserialize 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-182">% Deserialize time</span></span>
- <span data-ttu-id="56213-183">%CPU 실행 기 시간</span><span class="sxs-lookup"><span data-stu-id="56213-183">% CPU executor time</span></span>
- <span data-ttu-id="56213-184">JVM 시간 (%)</span><span class="sxs-lookup"><span data-stu-id="56213-184">% JVM time</span></span>

<span data-ttu-id="56213-185">이러한 시각화 요소를 처리 하는 전체 실행 기에 기여 하는 이러한 메트릭의 양을 각 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="56213-185">These visualizations show how much each of these metrics contributes to overall executor processing.</span></span>

![백분율 메트릭을 보여 주는 그래프](./_images/grafana-percentage.png)

<span data-ttu-id="56213-187">**메트릭 순서 섞기** 는 메트릭 데이터와 관련 된 실행 기에서 순서 섞기 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-187">**Shuffle metrics** are metrics related to data shuffling across the executors.</span></span>

- <span data-ttu-id="56213-188">순서 섞기 I/O</span><span class="sxs-lookup"><span data-stu-id="56213-188">Shuffle I/O</span></span>
- <span data-ttu-id="56213-189">순서 섞기 메모리</span><span class="sxs-lookup"><span data-stu-id="56213-189">Shuffle memory</span></span>
- <span data-ttu-id="56213-190">파일 시스템 사용량</span><span class="sxs-lookup"><span data-stu-id="56213-190">File system usage</span></span>
- <span data-ttu-id="56213-191">디스크 사용량</span><span class="sxs-lookup"><span data-stu-id="56213-191">Disk usage</span></span>

## <a name="common-performance-bottlenecks"></a><span data-ttu-id="56213-192">일반적인 성능 병목 상태</span><span class="sxs-lookup"><span data-stu-id="56213-192">Common performance bottlenecks</span></span>

<span data-ttu-id="56213-193">Spark에서 두 가지 일반적인 성능 병목 *지연 작업* 와 *최적이 아닌 셔플 파티션 수*입니다.</span><span class="sxs-lookup"><span data-stu-id="56213-193">Two common performance bottlenecks in Spark are *task stragglers* and a *non-optimal shuffle partition count*.</span></span>

### <a name="task-stragglers"></a><span data-ttu-id="56213-194">작업 지연</span><span class="sxs-lookup"><span data-stu-id="56213-194">Task stragglers</span></span>

<span data-ttu-id="56213-195">작업의 단계는 뒷부분에 나오는 단계를 차단 하는 이전 단계를 사용 하 여 순차적으로 실행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="56213-195">The stages in a job are executed sequentially, with earlier stages blocking later stages.</span></span> <span data-ttu-id="56213-196">하나 이상의 태스크 순서 섞기 파티션을 다른 작업 보다 더 느리게 실행 하는 경우 클러스터의 모든 작업 단계를 끝낼 수 전에 따라잡을 수 느린 작업 대기 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-196">If one task executes a shuffle partition more slowly than other tasks, all tasks in the cluster must wait for the slow task to catch up before the stage can end.</span></span> <span data-ttu-id="56213-197">이 작업은 다음과 같은 이유로 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="56213-197">This can happen for the following reasons:</span></span>

1. <span data-ttu-id="56213-198">호스트 또는 호스트 그룹을 느리게 실행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="56213-198">A host or group of hosts are running slow.</span></span> <span data-ttu-id="56213-199">증상: 높은 작업, 단계 또는 작업 대기 시간 및 낮은 클러스터 처리량</span><span class="sxs-lookup"><span data-stu-id="56213-199">Symptoms: High task, stage, or job latency and low cluster throughput.</span></span> <span data-ttu-id="56213-200">호스트당 작업 대기 시간의 합계를 고르게 분산 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="56213-200">The summation of tasks latencies per host won't be evenly distributed.</span></span> <span data-ttu-id="56213-201">그러나 실행 기에서 리소스 소비를 균등 하 게 배포 됩니다.</span><span class="sxs-lookup"><span data-stu-id="56213-201">However, resource consumption will be evenly distributed across executors.</span></span>

1. <span data-ttu-id="56213-202">작업 (데이터 기울이기)를 실행 하는 비용이 많이 드는 집계를 했습니다.</span><span class="sxs-lookup"><span data-stu-id="56213-202">Tasks have an expensive aggregation to execute (data skewing).</span></span> <span data-ttu-id="56213-203">증상: 높은 작업, 단계 또는 작업 하며 낮은 클러스터 처리량 호스트당 대기 시간 합계 균등 하 게 분산 됩니다.</span><span class="sxs-lookup"><span data-stu-id="56213-203">Symptoms: High task, stage, or job and low cluster throughput, but the summation of latencies per host is evenly distributed.</span></span> <span data-ttu-id="56213-204">실행 기에서 리소스 소비를 균등 하 게 배포 됩니다.</span><span class="sxs-lookup"><span data-stu-id="56213-204">Resource consumption will be evenly distributed across executors.</span></span>

1. <span data-ttu-id="56213-205">파티션이 서로 다른 크기의 경우 더 큰 파티션에 불균형된 작업 실행 (파티션 기울이기) 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="56213-205">If partitions are of unequal size, a larger partition may cause unbalanced task execution (partition skewing).</span></span> <span data-ttu-id="56213-206">증상: Executor 리소스 사용이 클러스터에서 실행 하는 다른 실행 기에 비해 높습니다.</span><span class="sxs-lookup"><span data-stu-id="56213-206">Symptoms: Executor resource consumption is high compared to other executors running on the cluster.</span></span> <span data-ttu-id="56213-207">실행 기에서 실행 중인 모든 작업 느리게 실행 되 고 파이프라인의 단계 실행을 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-207">All tasks running on that executor will run slow and hold the stage execution in the pipeline.</span></span> <span data-ttu-id="56213-208">이러한 단계 있다고 *장벽을 단계*합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-208">Those stages are said to be *stage barriers*.</span></span>

### <a name="non-optimal-shuffle-partition-count"></a><span data-ttu-id="56213-209">순서 섞기 최적화 되지 않은 파티션 수</span><span class="sxs-lookup"><span data-stu-id="56213-209">Non-optimal shuffle partition count</span></span>

<span data-ttu-id="56213-210">구조적된 스트리밍 쿼리를 하는 동안 태스크는 실행 기를 할당 클러스터에 대 한 리소스 집약적 작업입니다.</span><span class="sxs-lookup"><span data-stu-id="56213-210">During a structured streaming query, the assignment of a task to an executor is a resource-intensive operation for the cluster.</span></span> <span data-ttu-id="56213-211">순서 섞기 데이터 최적의 크기를 없는 경우 작업에 대 한 지연 시간을 부정적인 영향을 줍니다 처리량 및 대기 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="56213-211">If the shuffle data isn't the optimal size, the amount of delay for a task will negatively impact throughput and latency.</span></span> <span data-ttu-id="56213-212">너무 적은 파티션이 있는 경우 클러스터의 코어는 충분히 활용 되지 비효율성 처리 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="56213-212">If there are too few partitions, the cores in the cluster will be underutilized which can result in processing inefficiency.</span></span> <span data-ttu-id="56213-213">반대로, 너무 많은 파티션이 있으면 많은 오버 헤드가 적은 수의 작업에 대 한 관리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="56213-213">Conversely, if there are too many partitions, there's a great deal of management overhead for a small number of tasks.</span></span>

<span data-ttu-id="56213-214">리소스 소비 메트릭을 사용 하 여 파티션 기울이기 및 클러스터에서 실행 기 misallocation 문제를 해결 합니다.</span><span class="sxs-lookup"><span data-stu-id="56213-214">Use the resource consumption metrics to troubleshoot partition skewing and misallocation of executors on the cluster.</span></span> <span data-ttu-id="56213-215">파티션을 왜곡 되는 경우 클러스터에서 실행 중인 다른 실행자를 비교 하 여 상승 될 executor 리소스입니다.</span><span class="sxs-lookup"><span data-stu-id="56213-215">If a partition is skewed, executor resources will be elevated in comparison to other executors running on the cluster.</span></span>

<span data-ttu-id="56213-216">예를 들어 다음 그래프는 처음 두 명의 실행 기에서 순서 섞기에서 사용 하는 메모리가 다른 실행 기 보다 큰 X 90 임을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="56213-216">For example, the following graph shows that the memory used by shuffling on the first two executors is 90X bigger than the other executors:</span></span>

![백분율 메트릭을 보여 주는 그래프](./_images/grafana-shuffle-memory.png)