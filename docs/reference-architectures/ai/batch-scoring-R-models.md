---
title: Azure에서 R 모델을 사용 하 여 점수 매기기 배치
description: 일괄 처리 소매 저장소 판매 예측에 따라 데이터 집합 및 Azure Batch를 사용 하 여 R 모델을 사용 하 여 점수 매기기를 수행 합니다.
author: njray
ms.date: 03/29/2019
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: azcat-ai
ms.openlocfilehash: 4fa57168c337b01c8e7d0fc86ba54fee59a7ae47
ms.sourcegitcommit: 1a3cc91530d56731029ea091db1f15d41ac056af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58888065"
---
# <a name="batch-scoring-of-r-machine-learning-models-on-azure"></a><span data-ttu-id="7eebf-103">Azure에서 R 기계 학습 모델 점수 매기기 배치</span><span class="sxs-lookup"><span data-stu-id="7eebf-103">Batch scoring of R machine learning models on Azure</span></span>

<span data-ttu-id="7eebf-104">이 참조 아키텍처는 Azure Batch를 사용 하 여 R 모델을 사용 하 여 점수 매기기 일괄 처리를 수행 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-104">This reference architecture shows how to perform batch scoring with R models using Azure Batch.</span></span> <span data-ttu-id="7eebf-105">시나리오는 소매 저장소 판매 예측 기반 하지만 R 모델을 사용 하 여 큰 scaler에 대 한 예측 생성에 필요한 모든 시나리오에 대 한이 아키텍처를 일반화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-105">The scenario is based on retail store sales forecasting, but this architecture can be generalized for any scenario requiring the generation of predictions on a large scaler using R models.</span></span> <span data-ttu-id="7eebf-106">이 아키텍처에 대한 참조 구현은 [GitHub][github]에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-106">A reference implementation for this architecture is available on [GitHub][github].</span></span>

![아키텍처 다이어그램][0]

<span data-ttu-id="7eebf-108">**시나리오**: 슈퍼마켓 체인이 예정 된 분기를 통해 제품의 판매를 예측 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-108">**Scenario**: A supermarket chain needs to forecast sales of products over the upcoming quarter.</span></span> <span data-ttu-id="7eebf-109">예측 이용할 자사의 공급망을 더 잘 관리 하 고 해당 저장소의 각 제품에 대 한 수요를 충족할 수 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-109">The forecast allows the company to manage its supply chain better and ensure it can meet demand for products at each of its stores.</span></span> <span data-ttu-id="7eebf-110">회사 업데이트 예측 매주는 지난 주의 새 판매 데이터를 사용할 수 있게 되며 다음 분기에 대 한 마케팅 전략 제품 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-110">The company updates its forecasts every week as new sales data from the previous week becomes available and the product marketing strategy for next quarter is set.</span></span> <span data-ttu-id="7eebf-111">개별 판매 예측의 불확실성을 예측 하려면 변 위치 예측 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-111">Quantile forecasts are generated to estimate the uncertainty of the individual sales forecasts.</span></span>

<span data-ttu-id="7eebf-112">처리에는 다음 단계가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-112">Processing involves the following steps:</span></span>

1. <span data-ttu-id="7eebf-113">Azure Logic App을 일주일에 한 번 예측된 생성 프로세스를 트리거합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-113">An Azure Logic App triggers the forecast generation process once per week.</span></span>

1. <span data-ttu-id="7eebf-114">논리 앱 스케줄러 Batch 클러스터에서 점수 매기기 작업을 트리거하는 Docker 컨테이너를 실행 하는 Azure Container Instance를 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-114">The logic app starts an Azure Container Instance running the scheduler Docker container, which triggers the scoring jobs on the Batch cluster.</span></span>

1. <span data-ttu-id="7eebf-115">점수 매기기 작업 일괄 처리 클러스터의 노드에서 병렬로 실행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-115">Scoring jobs run in parallel across the nodes of the Batch cluster.</span></span> <span data-ttu-id="7eebf-116">각 노드:</span><span class="sxs-lookup"><span data-stu-id="7eebf-116">Each node:</span></span>

    1. <span data-ttu-id="7eebf-117">작업자 Docker 허브에서 Docker 이미지를 가져오고 및 컨테이너를 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-117">Pulls the worker Docker image from Docker Hub and starts a container.</span></span>

    1. <span data-ttu-id="7eebf-118">입력된 데이터를 읽고 미리 Azure Blob storage에서 R 모델을 학습 합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-118">Reads input data and pre-trained R models from Azure Blob storage.</span></span>

    1. <span data-ttu-id="7eebf-119">예측을 생성 하기 위해 데이터를 얻었습니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-119">Scores the data to produce the forecasts.</span></span>

    1. <span data-ttu-id="7eebf-120">Blob 저장소에 예측된 결과 씁니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-120">Writes the forecast results to blob storage.</span></span>

<span data-ttu-id="7eebf-121">아래 그림에서는 네 가지 제품 (Sku)에 대 한 예측된 판매량에 하나의 저장소 나와 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-121">The figure below shows the forecasted sales for four products (SKUs) in one store.</span></span> <span data-ttu-id="7eebf-122">검정색 선은 판매 기록과, 파선 예측 중앙값 (q50), 분홍색 밴드 25 번째 및/72-다섯 번째 백분위 수를 나타내는 이며 파랑 밴드 다섯 번째 및 90 번째 백분위 수를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-122">The black line is the sales history, the dashed line is the median (q50) forecast, the pink band represents the twenty-fifth and seventy-fifth percentiles, and the blue band represents the fifth and ninety-fifth percentiles.</span></span>

![판매 예측][1]

## <a name="architecture"></a><span data-ttu-id="7eebf-124">아키텍처</span><span class="sxs-lookup"><span data-stu-id="7eebf-124">Architecture</span></span>

<span data-ttu-id="7eebf-125">이 아키텍처는 다음과 같은 구성 요소로 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-125">This architecture consists of the following components.</span></span>

<span data-ttu-id="7eebf-126">[Azure Batch] [ batch] 가상 머신의 클러스터에서 병렬로 예측된 생성 작업을 실행 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-126">[Azure Batch][batch] is used to run forecast generation jobs in parallel on a cluster of virtual machines.</span></span> <span data-ttu-id="7eebf-127">미리 학습 된 기계를 사용 하 여 예측을 수행 하는 학습 모델 R. Azure Batch에서 구현 된 클러스터에 제출 된 작업의 수에 따라 Vm 수를 자동으로 조정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-127">Predictions are made using pre-trained machine learning models implemented in R. Azure Batch can automatically scale the number of VMs based on the number of jobs submitted to the cluster.</span></span> <span data-ttu-id="7eebf-128">각 노드에서 데이터 점수를 매기고 예측을 생성 하는 Docker 컨테이너 내에서 R 스크립트를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-128">On each node, an R script runs within a Docker container to score data and generate forecasts.</span></span>

<span data-ttu-id="7eebf-129">[Azure Blob Storage] [ blob] 입력된 데이터, 미리 학습 된 기계 학습 모델 및 예측된 결과 저장 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-129">[Azure Blob Storage][blob] is used to store the input data, the pre-trained machine learning models, and the forecast results.</span></span> <span data-ttu-id="7eebf-130">이 작업에 필요한 성능에 대 한 매우 비용 효율적인 저장소를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-130">It delivers very cost-effective storage for the performance that this workload requires.</span></span>

<span data-ttu-id="7eebf-131">[Azure Container Instances] [ aci] 주문형 서버 리스 계산을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-131">[Azure Container Instances][aci] provide serverless compute on demand.</span></span> <span data-ttu-id="7eebf-132">이 경우 예측을 생성 하는 일괄 처리 작업을 트리거하는 일정에 따라 컨테이너 인스턴스 배포 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-132">In this case, a container instance is deployed on a schedule to trigger the Batch jobs that generate the forecasts.</span></span> <span data-ttu-id="7eebf-133">일괄 처리 작업을 사용 하 여 R 스크립트에서 트리거되는지 합니다 [doAzureParallel] [ doAzureParallel] 패키지 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-133">The Batch jobs are triggered from an R script using the [doAzureParallel][doAzureParallel] package.</span></span> <span data-ttu-id="7eebf-134">작업이 완료 되 면 자동으로 컨테이너 인스턴스를 종료 합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-134">The container instance automatically shuts down once the jobs have finished.</span></span>

<span data-ttu-id="7eebf-135">[Azure Logic Apps] [ logic-apps] 일정에 따라 container instances를 배포 하 여 전체 워크플로 트리거합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-135">[Azure Logic Apps][logic-apps] trigger the entire workflow by deploying the container instances on a schedule.</span></span> <span data-ttu-id="7eebf-136">Logic Apps에서 Azure Container Instances 커넥터를 다양 한 트리거 이벤트 시 배포할 인스턴스 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-136">An Azure Container Instances connector in Logic Apps allows an instance to be deployed upon a range of trigger events.</span></span>

## <a name="performance-considerations"></a><span data-ttu-id="7eebf-137">성능 고려 사항</span><span class="sxs-lookup"><span data-stu-id="7eebf-137">Performance considerations</span></span>

### <a name="containerized-deployment"></a><span data-ttu-id="7eebf-138">컨테이너 화 된 배포</span><span class="sxs-lookup"><span data-stu-id="7eebf-138">Containerized deployment</span></span>

<span data-ttu-id="7eebf-139">이 아키텍처를 사용 하 여 모든 R 스크립트 내에서 실행 [Docker](https://www.docker.com/) 컨테이너입니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-139">With this architecture, all R scripts run within [Docker](https://www.docker.com/) containers.</span></span> <span data-ttu-id="7eebf-140">이렇게 하면 스크립트가 실행 될 때마다에 동일한 R 버전 및 패키지 버전을 사용 하 여 일관 된 환경에서.</span><span class="sxs-lookup"><span data-stu-id="7eebf-140">This ensures that the scripts run in a consistent environment, with the same R version and packages versions, every time.</span></span> <span data-ttu-id="7eebf-141">다른 R 패키지 종속성 집합을 사용 하기 때문에 별도 Docker 이미지 컨테이너를 스케줄러 및 작업자에 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-141">Separate Docker images are used for the scheduler and worker containers, because each has a different set of R package dependencies.</span></span>

<span data-ttu-id="7eebf-142">Azure Container Instances는 scheduler 컨테이너를 실행 하는 서버 리스 환경을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-142">Azure Container Instances provides a serverless environment to run the scheduler container.</span></span> <span data-ttu-id="7eebf-143">Scheduler 컨테이너 클러스터를 Azure Batch에서 실행 되는 개별 점수 매기기 작업을 트리거하는 R 스크립트를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-143">The scheduler container runs an R script that triggers the individual scoring jobs running on an Azure Batch cluster.</span></span>

<span data-ttu-id="7eebf-144">일괄 처리 클러스터의 각 노드에 점수 매기기 스크립트를 실행 하는 작업자 컨테이너를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-144">Each node of the Batch cluster runs the worker container, which executes the scoring script.</span></span>

### <a name="parallelizing-the-workload"></a><span data-ttu-id="7eebf-145">작업 병렬 처리</span><span class="sxs-lookup"><span data-stu-id="7eebf-145">Parallelizing the workload</span></span>

<span data-ttu-id="7eebf-146">일괄 처리 하면 R 모델을 사용 하 여 데이터 점수 매기기 작업을 병렬화 하는 방법을 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-146">When batch scoring data with R models, consider how to parallelize the workload.</span></span> <span data-ttu-id="7eebf-147">입력된 데이터 점수 매기기 작업 클러스터 노드에 분산 될 수 있도록 어떤 이유로 든 분할 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-147">The input data must be partitioned somehow so that the scoring operation can be distributed  across the cluster nodes.</span></span> <span data-ttu-id="7eebf-148">워크 로드 배포에 가장 적합 한를 검색 하는 다른 방법을 시도 합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-148">Try different approaches to discover the best choice for distributing your workload.</span></span> <span data-ttu-id="7eebf-149">경우에 따라 다음을 고려 합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-149">On a case-by-case basis, consider the following:</span></span>

- <span data-ttu-id="7eebf-150">얼마나 많은 데이터를 로드 하 고 단일 노드의 메모리에서 처리 수입니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-150">How much data can be loaded and processed in the memory of a single node.</span></span>
- <span data-ttu-id="7eebf-151">각 일괄 처리 작업을 시작 하는 오버 헤드입니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-151">The overhead of starting each batch job.</span></span>
- <span data-ttu-id="7eebf-152">R 모델을 로드 하는 오버 헤드입니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-152">The overhead of loading the R models.</span></span>

<span data-ttu-id="7eebf-153">이 예제에 사용 되는 시나리오에서 모델 개체는 대형 및 개별 제품에 대 한 예측을 생성 하는 데 몇 초만 걸립니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-153">In the scenario used for this example, the model objects are large, and it takes only a few seconds to generate a forecast for individual products.</span></span> <span data-ttu-id="7eebf-154">이러한 이유로 제품을 그룹화 수 있으며 노드당 단일 일괄 처리 작업을 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-154">For this reason, you can group the products and execute a single Batch job per node.</span></span> <span data-ttu-id="7eebf-155">각 작업 내에서 루프는 순차적으로 제품에 대 한 예측을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-155">A loop within each job generates forecasts for the products sequentially.</span></span> <span data-ttu-id="7eebf-156">이 메서드는이 특정 워크 로드를 병렬화 하는 가장 효율적인 방법은 것으로 밝혀졌습니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-156">This method turns out to be the most efficient way to parallelize this particular workload.</span></span> <span data-ttu-id="7eebf-157">더 작은 여러 일괄 처리 작업을 시작 하 고 반복적으로 R 모델을 로드 오버 헤드를 방지 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-157">It avoids the overhead of starting many smaller Batch jobs and repeatedly loading the R models.</span></span>

<span data-ttu-id="7eebf-158">또 다른 방법은 제품당 하나의 일괄 처리 작업을 트리거할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-158">An alternative approach is to trigger one Batch job per product.</span></span> <span data-ttu-id="7eebf-159">Azure Batch는 자동으로 작업 큐를 형성 하 고 노드를 사용할 수 있게 면 클러스터에서 실행 되도록 제출 합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-159">Azure Batch automatically forms a queue of jobs and submits them to be executed on the cluster as nodes become available.</span></span> <span data-ttu-id="7eebf-160">사용 하 여 [자동 크기 조정을] [ autoscale] 작업의 수에 따라 클러스터의 노드 수를 조정 합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-160">Use [automatic scaling][autoscale] to adjust the number of nodes in the cluster depending on the number of jobs.</span></span> <span data-ttu-id="7eebf-161">이 이렇게 하면 작업을 시작 및 모델 개체를 다시 로드 하는 오버 헤드를 정당화 각 점수 매기기 작업을 완료 하는 데는 비교적 오랜 시간이 걸리는 경우 것이 더 적절 하 게 합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-161">This approach makes more sense if it takes a relatively long time to complete each scoring operation, justifying the overhead of starting the jobs and reloading the model objects.</span></span> <span data-ttu-id="7eebf-162">이 이렇게 간단 하 게 구현 되며 자동 크기 조정을 사용 하는 데 유연성을 제공-총 워크 로드의 크기를 알 수 없는 경우 사전에 중요 한 고려 사항입니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-162">This approach is also simpler to implement and gives you the flexibility to use automatic scaling—an important consideration if the size of the total workload is not known in advance.</span></span>

## <a name="monitoring-and-logging-considerations"></a><span data-ttu-id="7eebf-163">모니터링 및 로깅 고려 사항</span><span class="sxs-lookup"><span data-stu-id="7eebf-163">Monitoring and logging considerations</span></span>

### <a name="monitoring-azure-batch-jobs"></a><span data-ttu-id="7eebf-164">Azure Batch 작업 모니터링</span><span class="sxs-lookup"><span data-stu-id="7eebf-164">Monitoring Azure Batch jobs</span></span>

<span data-ttu-id="7eebf-165">모니터링 및에서 일괄 처리 작업을 종료 합니다 **작업** Azure portal에서 Batch 계정 창입니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-165">Monitor and terminate Batch jobs from the **Jobs** pane of the Batch account in the Azure portal.</span></span> <span data-ttu-id="7eebf-166">일괄 처리에서 개별 노드의 상태를 포함 하 여 클러스터를 모니터링 합니다 **풀** 창입니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-166">Monitor the batch cluster, including the state of individual nodes, from the **Pools** pane.</span></span>

### <a name="logging-with-doazureparallel"></a><span data-ttu-id="7eebf-167">DoAzureParallel을 사용 하 여 로깅</span><span class="sxs-lookup"><span data-stu-id="7eebf-167">Logging with doAzureParallel</span></span>

<span data-ttu-id="7eebf-168">DoAzureParallel 패키지는 자동으로 Azure Batch에서 제출 된 모든 작업에 대 한 모든 stdout/stderr 로그를 수집 합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-168">The doAzureParallel package automatically collects logs of all stdout/stderr for every job submitted on Azure Batch.</span></span> <span data-ttu-id="7eebf-169">설치 프로그램에서 만든 저장소 계정에서 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-169">These can be found in the storage account created at setup.</span></span> <span data-ttu-id="7eebf-170">해당 명령을 보려면 사용 하 여 저장소 탐색 도구와 같은 [Azure Storage 탐색기] [ storage-explorer] 또는 Azure portal.</span><span class="sxs-lookup"><span data-stu-id="7eebf-170">To view them, use a storage navigation tool such as [Azure Storage Explorer][storage-explorer] or Azure portal.</span></span>

<span data-ttu-id="7eebf-171">개발 하는 동안 일괄 처리 작업을 신속 하 게 디버깅을 사용 하 여 로컬 R 세션에서 로그를 인쇄 합니다 [getJobFiles] [ getJobFiles] doAzureParallel의 함수입니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-171">To quickly debug Batch jobs during development, print logs in your local R session using the [getJobFiles][getJobFiles] function of doAzureParallel.</span></span>

## <a name="cost-considerations"></a><span data-ttu-id="7eebf-172">비용 고려 사항</span><span class="sxs-lookup"><span data-stu-id="7eebf-172">Cost considerations</span></span>

<span data-ttu-id="7eebf-173">이 참조 아키텍처에 사용 되는 계산 리소스가 가장 비용이 높은 구성 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-173">The compute resources used in this reference architecture are the most costly components.</span></span> <span data-ttu-id="7eebf-174">이 시나리오에 대 한 고정 된 크기의 클러스터에 작업이 트리거되고 다음 종료 작업이 완료 될 때마다 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-174">For this scenario, a cluster of fixed size is created whenever the job is triggered and then shut down after the job has completed.</span></span> <span data-ttu-id="7eebf-175">클러스터 노드 시작, 실행 또는 종료 하는 동안에 비용이 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-175">Cost is incurred only while the cluster nodes are starting, running, or shutting down.</span></span> <span data-ttu-id="7eebf-176">이 방법은 있는 예측을 생성 하는 데 필요한 계산 리소스를 일정 하 게 유지 비교적 작업에서 작업 하는 시나리오에 적합 합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-176">This approach is suitable for a scenario where the compute resources required to generate the forecasts remain relatively constant from job to job.</span></span>

<span data-ttu-id="7eebf-177">여기서 작업을 완료 하는 데 필요한 계산의 크기는 미리 알 수 없는 시나리오에서 자동 크기 조정을 사용 하는 것이 적합할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-177">In scenarios where the amount of compute required to complete the job is not known in advance, it may be more suitable to use automatic scaling.</span></span> <span data-ttu-id="7eebf-178">이 방법을 사용 하 여 클러스터의 크기는 확장 또는 축소할 작업의 크기에 따라 합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-178">With this approach, the size of the cluster is scaled up or down depending on the size of the job.</span></span> <span data-ttu-id="7eebf-179">Azure Batch에서는 자동 크기 조정 공식 사용 하 여 클러스터를 정의할 때 설정할 수 있는 범위를 지원 합니다 [doAzureParallel] [ doAzureParallel] API.</span><span class="sxs-lookup"><span data-stu-id="7eebf-179">Azure Batch supports a range of auto-scale formulae which you can set when defining the cluster using the [doAzureParallel][doAzureParallel] API.</span></span>

<span data-ttu-id="7eebf-180">일부 시나리오에서는 작업 간의 시간 너무 짧을 수 종료 하 고 클러스터를 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-180">For some scenarios, the time between jobs may be too short to shut down and start up the cluster.</span></span> <span data-ttu-id="7eebf-181">이러한 경우에 해당 하는 경우 작업 간에 실행 하는 클러스터를 유지 합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-181">In these cases, keep the cluster running between jobs if appropriate.</span></span>

<span data-ttu-id="7eebf-182">Azure Batch 및 doAzureParallel 우선 순위가 낮은 Vm 사용을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-182">Azure Batch and doAzureParallel support the use of low-priority VMs.</span></span> <span data-ttu-id="7eebf-183">이러한 Vm 할인 있지만 위험을 높은 우선 순위가 다른 작업으로 책정 되 고 함께 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-183">These VMs come with a significant discount but risk being appropriated by other higher priority workloads.</span></span> <span data-ttu-id="7eebf-184">따라서 이러한 Vm 사용에 중요 한 프로덕션 워크 로드에 대 한 없습니다 권장 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7eebf-184">The use of these VMs are therefore not recommended for critical production workloads.</span></span> <span data-ttu-id="7eebf-185">그러나 매우 유용에 대 한 실험적 또는 개발 워크 로드.</span><span class="sxs-lookup"><span data-stu-id="7eebf-185">However, they are very useful for experimental or development workloads.</span></span>

## <a name="deployment"></a><span data-ttu-id="7eebf-186">배포</span><span class="sxs-lookup"><span data-stu-id="7eebf-186">Deployment</span></span>

<span data-ttu-id="7eebf-187">이 참조 아키텍처를 배포 하려면에 설명 된 단계를 수행 합니다 [GitHub] [ github] 리포지토리.</span><span class="sxs-lookup"><span data-stu-id="7eebf-187">To deploy this reference architecture, follow the steps described in the [GitHub][github] repo.</span></span>


[0]: ./_images/batch-scoring-r-models.png
[1]: ./_images/sales-forecasts.png
[aci]: /azure/container-instances/container-instances-overview
[autoscale]: /azure/batch/batch-automatic-scaling
[batch]: /azure/batch/batch-technical-overview
[blob]: /azure/storage/blobs/storage-blobs-introduction
[doAzureParallel]: https://github.com/Azure/doAzureParallel/blob/master/docs/32-autoscale.md
[getJobFiles]: /azure/machine-learning/service/how-to-train-ml-models
[github]: https://github.com/Azure/RBatchScoring
[logic-apps]: /azure/logic-apps/logic-apps-overview
[storage-explorer]: /azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows