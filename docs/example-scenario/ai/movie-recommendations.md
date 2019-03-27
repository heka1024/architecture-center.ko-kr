---
title: Azure에서 동영상 추천
description: 기계 학습을 사용하여 영화, 제품 및 기타 추천 사항을 자동화하고, 기계 학습과 Azure DSVM(Data Science Virtual Machine)을 사용하여 Azure에서 모델을 학습합니다.
author: njray
ms.date: 01/09/2019
ms.custom: azcat-ai, AI
social_image_url: /azure/architecture/example-scenario/ai/media/architecture-movie-recommender.png
ms.topic: example-scenario
ms.service: architecture-center
ms.subservice: example-scenario
ms.openlocfilehash: be7959c1201e3a2aaf92c192d73a0ca47a990934
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58249648"
---
# <a name="movie-recommendations-on-azure"></a>Azure에서 동영상 추천

이 시나리오 예제에서는 비즈니스에서 고객을 위해 기계 학습을 사용하여 제품 추천을 자동화하는 방법을 보여 줍니다. Azure DSVM(Data Science Virtual Machine)은 Azure에서 모델을 학습하는 데 사용되며, 동영상에 지정된 등급에 따라 사용자에게 동영상을 추천합니다.

추천 사항은 소매업에서 뉴스, 미디어에 이르기까지 다양한 산업에서 유용할 수 있습니다. 잠재적인 애플리케이션에는 가상 상점에서 제품 추천 사항을 제공하거나, 뉴스 또는 게시 추천 사항을 제공하거나, 음악 추천 사항을 제공하는 것이 포함됩니다. 일반적으로 기업은 고객에게 맞춤형 추천 사항을 제공하기 위해 보조 직원을 고용하여 교육해야 했습니다. 이제 Azure를 활용하여 고객 선호도를 파악하기 위한 모델을 학습함으로써 규모에 맞게 맞춤형 추천 사항을 제공할 수 있습니다.

## <a name="relevant-use-cases"></a>관련 사용 사례

이 시나리오에 적합한 사용 사례는 다음과 같습니다.

* 웹 사이트의 동영상 추천 사항입니다.
* 모바일 앱의 소비자 제품 추천 사항입니다.
* 스트리밍 미디어의 뉴스 추천 사항입니다.

## <a name="architecture"></a>아키텍처

![학습 동영상 추천을 위한 기계 학습 모델 아키텍처][architecture]

이 시나리오에서는 동영상 등급 데이터 세트에 대한 Spark [ALS(Alternating Least Square)][als] 알고리즘을 사용하여 기계 학습 모델을 학습하고 평가합니다. 이 시나리오의 단계는 다음과 같습니다.

1. 프런트 엔드 웹 사이트 또는 앱 서비스에서 사용자, 항목 및 숫자 등급 튜플의 테이블에 표시되는 사용자-동영상 상호 작용의 기록 데이터를 수집합니다.

2. 수집된 기록 데이터가 Blob 스토리지에 저장됩니다.

3. DSVM은 Spark ALS 추천기 모델을 실험하거나 제품화하기 위해 사용되는 경우가 많습니다. ALS 모델은 적절한 데이터 분할 전략을 적용하여 전체 데이터 세트에서 생성되는 학습 데이터 세트를 사용하여 학습됩니다. 예를 들어 데이터 세트는 비즈니스 요구 사항에 따라 임의, 연대순 또는 계층화된 세트로 분할할 수 있습니다. 다른 기계 학습 작업과 마찬가지로 추천기도 평가 메트릭(예: precision\@*k*, recall\@*k*, [MAP][map], [nDCG\@k][ndcg])을 사용하여 검사됩니다.

4. Azure Machine Learning Service는 하이퍼 매개 변수 비우기 및 모델 관리와 같은 실험을 조정하는 데 사용됩니다.

5. 학습된 모델은 Azure Cosmos DB에서 유지되며, 지정된 사용자에게 상위 *k*개 동영상을 추천하는 데 적용할 수 있습니다.

6. 그러면 Azure Container Instances 또는 Azure Kubernetes Service를 사용하여 웹 또는 앱 서비스에 모델을 배포합니다.

추천기 서비스 빌드 및 크기 조정에 대한 자세한 지침은 [Azure에서 실시간 추천 API 빌드][ref-arch]를 참조하세요.

### <a name="components"></a>구성 요소

* [DSVM(Data Science Virtual Machine)][dsvm]은 기계 학습 및 데이터 과학을 위한 딥 러닝 프레임워크 및 도구가 있는 Azure 가상 머신입니다. DSVM에는 ALS를 실행하는 데 사용할 수 있는 독립 실행형 Spark 환경이 있습니다.

* [Azure Blob 스토리지][blob]는 동영상 추천 사항에 대한 데이터 세트를 저장합니다.

* [Azure Machine Learning Service][mls]는 기계 학습 모델의 빌드, 관리 및 배포를 가속화하는 데 사용됩니다.

* [Azure Cosmos DB][cosmosdb]는 글로벌 분산 및 다중 모델 데이터베이스 스토리지를 사용할 수 있게 합니다.

* [Azure Container Instances][aci]는 필요에 따라 [Azure Kubernetes Service][aks]를 사용하여 웹 또는 애플리케이션 서비스에 학습된 모델을 배포하는 데 사용됩니다.

### <a name="alternatives"></a>대안

[Azure Databricks][databricks]는 모델 학습 및 평가가 수행되는 관리형 Spark 클러스터입니다. 관리형 Spark 환경을 몇 분 내에 설정하고 [자동으로 크기를 강화하거나 축소][autoscale]하여 클러스터 크기 수동 조정과 관련된 리소스와 비용을 줄일 수 있습니다. 또 다른 리소스 절약 옵션은 비활성 [클러스터][clusters]를 자동으로 종료하도록 구성하는 것입니다.

## <a name="considerations"></a>고려 사항

### <a name="availability"></a>가용성

기계 학습 기반 앱은 학습용 리소스와 서비스용 리소스라는 두 가지 리소스 구성 요소로 분할됩니다. 라이브 프로덕션 요청이 이러한 리소스에 직접 도달하지 않으므로 일반적으로 학습에 필요한 리소스에는 고가용성이 필요하지 않습니다. 서비스에 필요한 리소스에는 고객 요청을 처리하기 위해 HA(고가용성)가 필요합니다.

DSVM은 학습을 위해 전 세계 [여러 지역][regions]에서 사용할 수 있으며 가상 머신에 대한 [SLA(서비스 수준 계약)][sla]를 충족합니다. Azure Kubernetes Service는 서비스를 제공하기 위해 [고가용성][ha] 인프라를 제공합니다. 또한 에이전트 노드는 가상 머신에 대한 [SLA][sla-aks]를 따릅니다.

### <a name="scalability"></a>확장성

데이터 크기가 큰 경우 DSVM을 확장하여 학습 시간을 줄일 수 있습니다. [VM 크기][vm-size]를 변경하여 VM의 크기를 강화하거나 축소할 수 있습니다. 학습에 걸리는 시간을 줄이려면 메모리 내 데이터 세트에 맞게 충분히 큰 메모리 크기와 더 많은 vCPU 수를 선택합니다.

### <a name="security"></a>보안

이 시나리오에서는 Azure Active Directory를 사용하여 코드, 모델 및 데이터(메모리 내)가 포함된 [DSVM에 대한 액세스][dsvm-id]를 인증할 수 있습니다. 데이터는 DSVM에 로드되기 전에 Azure Storage에 저장되며, [스토리지 서비스 암호화][storage-security]를 사용하여 자동으로 암호화됩니다. 권한은 Azure Active Directory 인증 또는 역할 기반 액세스 제어를 통해 관리할 수 있습니다.

## <a name="deploy-this-scenario"></a>시나리오 배포

**필수 조건**: 기존 Azure 계정이 있어야 합니다. Azure 구독이 없는 경우 시작하기 전에 [체험 계정][free]을 만듭니다.

이 시나리오에 대한 모든 코드는 [Microsoft 추천기 리포지토리][github]에서 사용할 수 있습니다.

[ALS 빠른 시작 Notebook][notebook]을 실행하려면 다음 단계를 수행합니다.

1. Azure Portal에서 [DSVM을 만듭니다][dsvm-ubuntu].

2. notebooks 폴더에서 리포지토리를 복제합니다.

    ```shell
    cd notebooks
    git clone https://github.com/Microsoft/Recommenders
    ```

3. [SETUP.md][setup] 파일에 설명된 단계에 따라 conda 종속성을 설치합니다.

4. 브라우저에서 jupyterlab VM으로 이동하고, `notebooks/00_quick_start/als_pyspark_movielens.ipynb`로 이동합니다.

5. Notebook을 실행합니다.

## <a name="related-resources"></a>관련 리소스

추천기 서비스 빌드 및 크기 조정에 대한 자세한 지침은 [Azure에서 실시간 추천 API 빌드][ref-arch]를 참조하세요. 자습서 및 추천 시스템 예제는 [Microsoft 추천기 리포지토리][github]를 참조하세요.

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
