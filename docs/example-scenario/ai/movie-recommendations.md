---
title: Azure에서 동영상 추천
description: 기계 학습을 사용하여 영화, 제품 및 기타 추천 사항을 자동화하고, 기계 학습과 Azure DSVM(Data Science Virtual Machine)을 사용하여 Azure에서 모델을 학습합니다.
author: njray
ms.date: 1/9/2019
ms.custom: azcat-ai, AI
social_image_url: /azure/architecture/example-scenario/ai/media/architecture-movie-recommender.png
ms.topic: example-scenario
ms.service: architecture-center
ms.subservice: example-scenario
ms.openlocfilehash: 9387ab7989695df29df53d7aa4a437010cdd9fdf
ms.sourcegitcommit: 14226018a058e199523106199be9c07f6a3f8592
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55482997"
---
# <a name="movie-recommendations-on-azure"></a><span data-ttu-id="2df43-103">Azure에서 동영상 추천</span><span class="sxs-lookup"><span data-stu-id="2df43-103">Movie recommendations on Azure</span></span>

<span data-ttu-id="2df43-104">이 시나리오 예제에서는 비즈니스에서 고객을 위해 기계 학습을 사용하여 제품 추천을 자동화하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-104">This example scenario shows how a business can use machine learning to automate product recommendations for their customers.</span></span> <span data-ttu-id="2df43-105">Azure DSVM(Data Science Virtual Machine)은 Azure에서 모델을 학습하는 데 사용되며, 동영상에 지정된 등급에 따라 사용자에게 동영상을 추천합니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-105">An Azure Data Science Virtual Machine (DSVM) is used to train a model on Azure that recommends movies to users based on ratings that have been given to movies.</span></span>

<span data-ttu-id="2df43-106">추천 사항은 소매업에서 뉴스, 미디어에 이르기까지 다양한 산업에서 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-106">Recommendations can be useful in various industries from retail to news to media.</span></span> <span data-ttu-id="2df43-107">잠재적인 애플리케이션에는 가상 상점에서 제품 추천 사항을 제공하거나, 뉴스 또는 게시 추천 사항을 제공하거나, 음악 추천 사항을 제공하는 것이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-107">Potential applications include providing  product recommendations in a virtual store, providing news or post recommendations, or providing music recommendations.</span></span> <span data-ttu-id="2df43-108">일반적으로 기업은 고객에게 맞춤형 추천 사항을 제공하기 위해 보조 직원을 고용하여 교육해야 했습니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-108">Traditionally, businesses had to hire and train assistants to make personalized recommendations to customers.</span></span> <span data-ttu-id="2df43-109">이제 Azure를 활용하여 고객 선호도를 파악하기 위한 모델을 학습함으로써 규모에 맞게 맞춤형 추천 사항을 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-109">Today, we can  provide customized recommendations at scale by utilizing Azure to train models to understand customer preferences.</span></span>

## <a name="relevant-use-cases"></a><span data-ttu-id="2df43-110">관련 사용 사례</span><span class="sxs-lookup"><span data-stu-id="2df43-110">Relevant use cases</span></span>

<span data-ttu-id="2df43-111">이 시나리오에 적합한 사용 사례는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-111">Consider this scenario for the following use cases:</span></span>

* <span data-ttu-id="2df43-112">웹 사이트의 동영상 추천 사항입니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-112">Movie recommendations on a website.</span></span>
* <span data-ttu-id="2df43-113">모바일 앱의 소비자 제품 추천 사항입니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-113">Consumer product recommendations in a mobile app.</span></span>
* <span data-ttu-id="2df43-114">스트리밍 미디어의 뉴스 추천 사항입니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-114">News recommendations on streaming media.</span></span>

## <a name="architecture"></a><span data-ttu-id="2df43-115">아키텍처</span><span class="sxs-lookup"><span data-stu-id="2df43-115">Architecture</span></span>

![학습 동영상 추천을 위한 기계 학습 모델 아키텍처][architecture]

<span data-ttu-id="2df43-117">이 시나리오에서는 동영상 등급 데이터 세트에 대한 Spark [ALS(Alternating Least Square)][als] 알고리즘을 사용하여 기계 학습 모델을 학습하고 평가합니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-117">This scenario covers the training and evaluating of the machine learning model using the Spark [alternating least squares][als] (ALS) algorithm on a dataset of movie ratings.</span></span> <span data-ttu-id="2df43-118">이 시나리오의 단계는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-118">The steps for this scenario are as following:</span></span>

1. <span data-ttu-id="2df43-119">프런트 엔드 웹 사이트 또는 앱 서비스에서 사용자, 항목 및 숫자 등급 튜플의 테이블에 표시되는 사용자-동영상 상호 작용의 기록 데이터를 수집합니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-119">The front-end website or app service collects historical data of user-movie interactions, which are represented in a table of user, item, and numerical rating tuples.</span></span>

2. <span data-ttu-id="2df43-120">수집된 기록 데이터가 Blob 스토리지에 저장됩니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-120">The collected historical data is stored in a blob storage.</span></span>

3. <span data-ttu-id="2df43-121">DSVM은 Spark ALS 추천기 모델을 실험하거나 제품화하기 위해 사용되는 경우가 많습니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-121">A DSVM is often used to experiment with or productize a Spark ALS recommender model.</span></span> <span data-ttu-id="2df43-122">ALS 모델은 적절한 데이터 분할 전략을 적용하여 전체 데이터 세트에서 생성되는 학습 데이터 세트를 사용하여 학습됩니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-122">The ALS model is trained using a training dataset, which is produced from the overall dataset by applying the appropriate data splitting strategy.</span></span> <span data-ttu-id="2df43-123">예를 들어 데이터 세트는 비즈니스 요구 사항에 따라 임의, 연대순 또는 계층화된 세트로 분할할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-123">For example, the dataset can be split into sets randomly, chronologically, or stratified, depending on the business requirement.</span></span> <span data-ttu-id="2df43-124">다른 기계 학습 작업과 마찬가지로 추천기도 평가 메트릭(예: precision\@*k*, recall\@*k*, [MAP][map], [nDCG\@k][ndcg])을 사용하여 검사됩니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-124">Similar to other machine learning tasks, a recommender is validated by using evaluation metrics (for example, precision\@*k*, recall\@*k*, [MAP][map], [nDCG\@k][ndcg]).</span></span>

4. <span data-ttu-id="2df43-125">Azure Machine Learning Service는 하이퍼 매개 변수 비우기 및 모델 관리와 같은 실험을 조정하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-125">Azure Machine Learning service is used for coordinating the experimentation, such as hyperparameter sweeping and model management.</span></span>

5. <span data-ttu-id="2df43-126">학습된 모델은 Azure Cosmos DB에서 유지되며, 지정된 사용자에게 상위 *k*개 동영상을 추천하는 데 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-126">A trained model is preserved on Azure Cosmos DB, which can then be applied for recommending the top *k* movies for a given user.</span></span>

6. <span data-ttu-id="2df43-127">그러면 Azure Container Instances 또는 Azure Kubernetes Service를 사용하여 웹 또는 앱 서비스에 모델을 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-127">The model is then deployed onto a web or app service by using Azure Container Instances or Azure Kubernetes Service.</span></span>

<span data-ttu-id="2df43-128">추천기 서비스 빌드 및 크기 조정에 대한 자세한 지침은 [Azure에서 실시간 추천 API 빌드][ref-arch]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="2df43-128">For an in-depth guide to building and scaling a recommender service, see [Build a real-time recommendation API on Azure][ref-arch].</span></span>

### <a name="components"></a><span data-ttu-id="2df43-129">구성 요소</span><span class="sxs-lookup"><span data-stu-id="2df43-129">Components</span></span>

* <span data-ttu-id="2df43-130">[DSVM(Data Science Virtual Machine)][dsvm]은 기계 학습 및 데이터 과학을 위한 딥 러닝 프레임워크 및 도구가 있는 Azure 가상 머신입니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-130">[Data Science Virtual Machine][dsvm] (DSVM) is an Azure virtual machine with deep learning frameworks and tools for machine learning and data science.</span></span> <span data-ttu-id="2df43-131">DSVM에는 ALS를 실행하는 데 사용할 수 있는 독립 실행형 Spark 환경이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-131">The DSVM has a standalone Spark environment that can be used to run ALS.</span></span>

* <span data-ttu-id="2df43-132">[Azure Blob 스토리지][blob]는 동영상 추천 사항에 대한 데이터 세트를 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-132">[Azure Blob storage][blob] stores the dataset for movie recommendations.</span></span>

* <span data-ttu-id="2df43-133">[Azure Machine Learning Service][mls]는 기계 학습 모델의 빌드, 관리 및 배포를 가속화하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-133">[Azure Machine Learning service][mls] is used to accelerate the building, managing, and deploying of machine learning models.</span></span>

* <span data-ttu-id="2df43-134">[Azure Cosmos DB][cosmosdb]는 글로벌 분산 및 다중 모델 데이터베이스 스토리지를 사용할 수 있게 합니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-134">[Azure Cosmos DB][cosmosdb] enables globally distributed and multi-model database storage.</span></span>

* <span data-ttu-id="2df43-135">[Azure Container Instances][aci]는 필요에 따라 [Azure Kubernetes Service][aks]를 사용하여 웹 또는 애플리케이션 서비스에 학습된 모델을 배포하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-135">[Azure Container Instances][aci] is used to deploy the trained models to web or app services, optionally using [Azure Kubernetes Service][aks].</span></span>

### <a name="alternatives"></a><span data-ttu-id="2df43-136">대안</span><span class="sxs-lookup"><span data-stu-id="2df43-136">Alternatives</span></span>

<span data-ttu-id="2df43-137">[Azure Databricks][databricks]는 모델 학습 및 평가가 수행되는 관리형 Spark 클러스터입니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-137">[Azure Databricks][databricks] is a managed Spark cluster where model training and evaluating is performed.</span></span> <span data-ttu-id="2df43-138">관리형 Spark 환경을 몇 분 내에 설정하고 [자동으로 크기를 강화하거나 축소][autoscale]하여 클러스터 크기 수동 조정과 관련된 리소스와 비용을 줄일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-138">You can set up a managed Spark environment in minutes, and [autoscale][autoscale] up and down to help reduce the resources and costs associated with scaling clusters manually.</span></span> <span data-ttu-id="2df43-139">또 다른 리소스 절약 옵션은 비활성 [클러스터][clusters]를 자동으로 종료하도록 구성하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-139">Another resource-saving option is to configure inactive [clusters][clusters] to terminate automatically.</span></span>

## <a name="considerations"></a><span data-ttu-id="2df43-140">고려 사항</span><span class="sxs-lookup"><span data-stu-id="2df43-140">Considerations</span></span>

### <a name="availability"></a><span data-ttu-id="2df43-141">가용성</span><span class="sxs-lookup"><span data-stu-id="2df43-141">Availability</span></span>

<span data-ttu-id="2df43-142">기계 학습 기반 앱은 학습용 리소스와 서비스용 리소스라는 두 가지 리소스 구성 요소로 분할됩니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-142">Machine-learning-built apps are split into two resource components: resources for training, and resources for serving.</span></span> <span data-ttu-id="2df43-143">라이브 프로덕션 요청이 이러한 리소스에 직접 도달하지 않으므로 일반적으로 학습에 필요한 리소스에는 고가용성이 필요하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-143">Resources required for training generally do not need  high availability, as live production requests do not directly hit these resources.</span></span> <span data-ttu-id="2df43-144">서비스에 필요한 리소스에는 고객 요청을 처리하기 위해 HA(고가용성)가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-144">Resources required for serving need to have high availability to serve customer requests.</span></span>

<span data-ttu-id="2df43-145">DSVM은 학습을 위해 전 세계 [여러 지역][regions]에서 사용할 수 있으며 가상 머신에 대한 [SLA(서비스 수준 계약)][sla]를 충족합니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-145">For training, the DSVM is available in [multiple regions][regions] around the globe and meets the [service level agreement][sla] (SLA) for virtual machines.</span></span> <span data-ttu-id="2df43-146">Azure Kubernetes Service는 서비스를 제공하기 위해 [고가용성][ha] 인프라를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-146">For serving, Azure Kubernetes Service provides a [highly available][ha] infrastructure.</span></span> <span data-ttu-id="2df43-147">또한 에이전트 노드는 가상 머신에 대한 [SLA][sla-aks]를 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-147">Agent nodes also follow the [SLA][sla-aks] for virtual machines.</span></span>

### <a name="scalability"></a><span data-ttu-id="2df43-148">확장성</span><span class="sxs-lookup"><span data-stu-id="2df43-148">Scalability</span></span>

<span data-ttu-id="2df43-149">데이터 크기가 큰 경우 DSVM을 확장하여 학습 시간을 줄일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-149">If you have a large data size, you can scale your DSVM to shorten training time.</span></span> <span data-ttu-id="2df43-150">[VM 크기][vm-size]를 변경하여 VM의 크기를 강화하거나 축소할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-150">You can scale a VM up or down by changing the [VM size][vm-size].</span></span> <span data-ttu-id="2df43-151">학습에 걸리는 시간을 줄이려면 메모리 내 데이터 세트에 맞게 충분히 큰 메모리 크기와 더 많은 vCPU 수를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-151">Choose a memory size large enough to fit your dataset in-memory and a higher vCPU count in order to decrease the amount of time that training takes.</span></span>

### <a name="security"></a><span data-ttu-id="2df43-152">보안</span><span class="sxs-lookup"><span data-stu-id="2df43-152">Security</span></span>

<span data-ttu-id="2df43-153">이 시나리오에서는 Azure Active Directory를 사용하여 코드, 모델 및 데이터(메모리 내)가 포함된 [DSVM에 대한 액세스][dsvm-id]를 인증할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-153">This scenario can use Azure Active Directory to authenticate users for [access to the DSVM][dsvm-id], which contains your code, models, and (in-memory) data.</span></span> <span data-ttu-id="2df43-154">데이터는 DSVM에 로드되기 전에 Azure Storage에 저장되며, [스토리지 서비스 암호화][storage-security]를 사용하여 자동으로 암호화됩니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-154">Data is stored in Azure Storage prior to being loaded on a DSVM, where it is automatically encrypted using [Storage Service Encryption][storage-security].</span></span> <span data-ttu-id="2df43-155">권한은 Azure Active Directory 인증 또는 역할 기반 액세스 제어를 통해 관리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-155">Permissions can be managed via Azure Active Directory authentication or role-based access control.</span></span>

## <a name="deploy-this-scenario"></a><span data-ttu-id="2df43-156">시나리오 배포</span><span class="sxs-lookup"><span data-stu-id="2df43-156">Deploy this scenario</span></span>

<span data-ttu-id="2df43-157">**필수 조건**: 기존 Azure 계정이 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-157">**Prerequisites**: You must have an existing Azure account.</span></span> <span data-ttu-id="2df43-158">Azure 구독이 없는 경우 시작하기 전에 [체험 계정][free]을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-158">If you don't have an Azure subscription, create a [free account][free] before you begin.</span></span>

<span data-ttu-id="2df43-159">이 시나리오에 대한 모든 코드는 [Microsoft 추천기 리포지토리][github]에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-159">All the code for this scenario is available in the [Microsoft Recommenders repository][github].</span></span>

<span data-ttu-id="2df43-160">[ALS 빠른 시작 Notebook][notebook]을 실행하려면 다음 단계를 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-160">Follow these steps to run the [ALS quickstart notebook][notebook]:</span></span>

1. <span data-ttu-id="2df43-161">Azure Portal에서 [DSVM을 만듭니다][dsvm-ubuntu].</span><span class="sxs-lookup"><span data-stu-id="2df43-161">[Create a DSVM][dsvm-ubuntu] from the Azure portal.</span></span>

2. <span data-ttu-id="2df43-162">notebooks 폴더에서 리포지토리를 복제합니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-162">Clone the repo in the Notebooks folder:</span></span>

    ```shell
    cd notebooks
    git clone https://github.com/Microsoft/Recommenders
    ```

3. <span data-ttu-id="2df43-163">[SETUP.md][setup] 파일에 설명된 단계에 따라 conda 종속성을 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-163">Install the conda dependencies following the steps described in the [SETUP.md][setup] file.</span></span>

4. <span data-ttu-id="2df43-164">브라우저에서 jupyterlab VM으로 이동하고, `notebooks/00_quick_start/als_pyspark_movielens.ipynb`로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-164">In a browser, go to your jupyterlab VM and navigate to `notebooks/00_quick_start/als_pyspark_movielens.ipynb`.</span></span>

5. <span data-ttu-id="2df43-165">Notebook을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="2df43-165">Execute the notebook.</span></span>

## <a name="related-resources"></a><span data-ttu-id="2df43-166">관련 리소스</span><span class="sxs-lookup"><span data-stu-id="2df43-166">Related resources</span></span>

<span data-ttu-id="2df43-167">추천기 서비스 빌드 및 크기 조정에 대한 자세한 지침은 [Azure에서 실시간 추천 API 빌드][ref-arch]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="2df43-167">For an in-depth guide to building and scaling a recommender service, see [Build a real-time recommendation API on Azure][ref-arch].</span></span> <span data-ttu-id="2df43-168">자습서 및 추천 시스템 예제는 [Microsoft 추천기 리포지토리][github]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="2df43-168">For tutorials and examples of recommendation systems, see [Microsoft Recommenders repository][github].</span></span>

[architecture]: ./media/architecture-movie-recommender.png
[aci]: /azure/container-instances/container-instances-overview
[aad]: /azure/active-directory-b2c/active-directory-b2c-overview
[aks]: /azure/aks/intro-kubernetes
[als]: https://spark.apache.org/docs/latest/ml-collaborative-filtering.html
[autoscale]: https://docs.azuredatabricks.net/user-guide/clusters/sizing.html#autoscaling
[blob]: /azure/storage/blobs/storage-blobs-introduction
[clusters]: https://docs.azuredatabricks.net/user-guide/clusters/configure.html
[cosmosdb]: /azure/cosmos-db/introduction
[databricks]: /azure/azure-databricks/what-is-azure-databricks
[dsvm]: /azure/machine-learning/data-science-virtual-machine/overview
[dsvm-id]: /azure/machine-learning/data-science-virtual-machine/dsvm-common-identity
[dsvm-ubuntu]: /azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro
[free]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F
[github]: https://github.com/Microsoft/Recommenders
[ha]: /azure/aks/container-service-quotas
[map]: https://en.wikipedia.org/wiki/Evaluation_measures_(information_retrieval)
[mls]: /azure/machine-learning/service/
[n-tier]: /azure/architecture/reference-architectures/n-tier/n-tier-cassandra
[ndcg]: https://en.wikipedia.org/wiki/Discounted_cumulative_gain
[notebook]: https://github.com/Microsoft/Recommenders/notebooks/00_quick_start/als_pyspark_movielens.ipynb
[ref-arch]: /azure/architecture/reference-architectures/ai/real-time-recommendation
[regions]: https://azure.microsoft.com/en-us/global-infrastructure/services/?products=virtual-machines&regions=all
[resiliency]: /azure/architecture/resiliency/
[sec-docs]: /azure/security/
[setup]: https://github.com/Microsoft/Recommenders/blob/master/SETUP.md%60
[sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_8/
[sla-aks]: https://azure.microsoft.com/en-us/support/legal/sla/kubernetes-service/v1_0/
[storage-security]: /azure/storage/common/storage-service-encryption
[vm-size]: /azure/virtual-machines/virtual-machines-linux-change-vm-size
