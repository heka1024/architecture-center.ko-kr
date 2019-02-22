---
title: Azure에서 실시간 추천 API 빌드
description: Azure에서 모델을 학습시키는 Azure Databricks 및 Azure Data Science Virtual Machines(DSVM)를 사용하여 기계 학습으로 추천을 자동화하세요.
author: njray
ms.date: 12/12/2018
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: azcat-ai
ms.openlocfilehash: c4bfd6e92fc9c770a03a63355fc922d19ef27b7b
ms.sourcegitcommit: f4ed242dff8b204cfd8ebebb7778f356a19f5923
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56224167"
---
# <a name="build-a-real-time-recommendation-api-on-azure"></a>Azure에서 실시간 추천 API 빌드

이 참조 아키텍처에서는 Azure Databricks를 사용하여 추천 모델을 학습시키고 Azure Cosmos DB, Azure Machine Learning, Azure Kubernetes Service(AKS)를 사용하여 API로 배포하는 방법을 보여줍니다. 이 아키텍처는 제품, 동영상, 뉴스 추천 등 대부분의 추천 엔진 시나리오에 맞게 일반화할 수 있습니다.

이 아키텍처에 대한 참조 구현은 [GitHub](https://github.com/Microsoft/Recommenders/blob/master/notebooks/05_operationalize/als_movie_o16n.ipynb)에서 사용할 수 있습니다.

![학습 동영상 추천을 위한 기계 학습 모델 아키텍처](./_images/recommenders-architecture.png)

**시나리오**: 한 미디어 조직에서 사용자에게 동영상 추천을 제공하려고 합니다. 이 조직에서는 개인 맞춤형 추천을 제공하여 클릭률 증가, 사이트 참여 증가, 사용자 만족도 증가를 포함한 몇 가지 비즈니스 목표를 충족합니다.

이 참조 아키텍처는 특정 사용자에게 상위 10개의 동영상 추천을 제공할 수 있는 실시간 추천 서비스 API를 학습시키고 배포하는 데 사용됩니다.

이 추천 모델의 데이터 흐름은 다음과 같습니다.

1. 사용자 동작을 추적합니다. 예를 들어, 사용자가 동영상을 평가하거나 제품 또는 뉴스 기사를 클릭할 때 백 엔드 서비스가 이를 기록할 수 있습니다.

2. 사용 가능한 [데이터 원본][data-source]에서 Azure Databricks로 데이터를 로드합니다.

3. 데이터를 준비하고 학습 및 테스트 집합으로 분할하여 모델을 학습시킵니다. ([이 가이드][guide]에서는 데이터 분할 옵션을 설명합니다.)

4. [Spark Collaborative Filtering][als] 모델을 데이터에 맞춥니다.

5. 등급 및 순위 메트릭을 사용하여 모델의 품질을 평가합니다. ([이 가이드][eval-guide]에서는 추천 시스템을 평가하는 데 기준으로 사용할 수 있는 메트릭에 대한 세부 정보를 제공합니다.)

6. 사용자별 상위 10개 추천 항목을 미리 계산하고 Azure Cosmos DB에 캐시로 저장합니다.

7. API를 컨테이너화하고 배포하는 Azure Machine Learning API를 사용하여 AKS에 API 서비스를 배포합니다.

8. 백 엔드 서비스가 사용자로부터 요청을 받으면 AKS에서 호스팅되는 추천 API를 호출하여 상위 10개 추천 항목을 가져와 사용자에게 표시하세요.

## <a name="architecture"></a>아키텍처

이 아키텍처는 다음과 같은 구성 요소로 구성됩니다.

[Azure Databricks][databricks]. Databricks는 입력 데이터를 준비하고 추천 모델에게 Spark 클러스터를 학습시키는 데 사용되는 개발 환경입니다. Azure Databricks는 또한 데이터 처리 또는 기계 학습 작업을 위해 노트북에서 실행하고 공동 작업을 수행할 수 있는 대화형 작업 영역을 제공합니다.

[Azure Kubernetes Service][aks](AKS). AKS는 Kubernetes 클러스터에서 기계 학습 모델 서비스 API를 배포하고 운영하는 데 사용됩니다. AKS는 컨테이너화된 모델을 호스팅하고, 처리량 요구 사항에 맞는 확장성, ID 및 액세스 관리, 로깅 및 상태 모니터링을 제공합니다.

[Azure Cosmos DB][cosmosdb]. Cosmos DB는 각 사용자를 위한 상위 10개 추천 동영상을 저장하는 데 사용되는 글로벌 분산 데이터베이스 서비스입니다. Azure Cosmos DB는 특정 사용자를 위한 상위 추천 항목을 읽을 때 대기 시간이 짧으므로(99%의 확률로 10ms) 이러한 시나리오에 매우 적합합니다.

[Azure Machine Learning Service][mls]. 이 서비스는 기계 학습 모델을 추적 및 관리하고, 이러한 모델을 패키징하여 확장 가능한 AKS 환경에 배포하는 데 사용됩니다.

[Microsoft 추천 시스템][github]. 이 오픈 소스 리포지토리에는 사용자가 추천 시스템 빌드, 평가, 운영을 시작하는 데 도움이 되는 유틸리티 코드와 샘플이 포함되어 있습니다.

## <a name="performance-considerations"></a>성능 고려 사항

실시간 추천 항목에서 성능은 주된 고려 사항입니다. 추천 항목은 일반적으로 사용자가 사이트에서 수행하는 요청의 중요 경로에 속하기 때문입니다.

AKS와 Azure Cosmos DB의 조합은 이러한 아키텍처가 최소한의 오버헤드로 중간 규모 워크로드에 추천 항목을 제공하는 데 좋은 시작점이 되도록 합니다. 동시 사용자가 200명인 부하 테스트에서 이 아키텍처는 약 60ms의 평균 대기 시간으로 추천 항목을 제공하고, 초당 180개 요청을 처리할 수 있습니다. 이 부하 테스트는 기본 배포 구성(Azure Cosmos DB용으로 프로비전된 vCPU 12개, 메모리 42GB, [초당 요청 단위(RU)][ru] 11,000개를 지원하는 D3 v2 AKS 클러스터 3개)을 대상으로 실행되었습니다.

![성능 그래프](./_images/recommenders-performance.png)

![처리량 그래프](./_images/recommenders-throughput.png)

Azure Cosmos DB는 턴키 글로벌 배포 및 앱의 모든 데이터베이스 요구 사항을 충족하는 유용성 때문에 권장됩니다. 약간 [더 짧은 대기 시간][latency]이 필요할 경우 Azure Cosmos DB를 사용하여 조회 기능을 제공하는 대신 [Azure Redis Cache][redis]를 사용하는 것이 좋습니다. Redis Cache는 백 엔드 저장소의 데이터에 크게 의존하는 시스템의 성능을 개선할 수 있습니다.

## <a name="scalability-considerations"></a>확장성 고려 사항

Spark를 사용할 계획이 없거나 분산이 필요 없는 소규모 워크로드를 보유하고 있을 경우 Azure Databricks 대신 [Data Science Virtual Machine][dsvm](DSVM)을 사용하는 것이 좋습니다. DSVM은 기계 학습 및 데이터 과학을 위한 딥 러닝 프레임워크 및 도구를 사용하는 Azure 가상 머신입니다. Azure Databricks와 마찬가지로 DSVM에서 만든 모델도 Azure Machine Learning을 통해 AKS에서 서비스로 운영할 수 있습니다.

학습 중에는 Azure Databricks에 대규모 고정 크기의 Spark 클러스터를 프로비전하거나 [자동 확장][autoscaling]을 구성합니다. 자동 크기 조정을 사용할 경우 Databricks가 클러스터의 부하를 모니터링하고 필요 시 규모를 확장하거나 축소합니다. 대규모의 데이터가 있고 데이터 준비나 모델링 작업에 걸리는 시간을 줄이려는 경우 대규모 클러스터를 프로비전하거나 확장하세요.

성능 및 처리량 요구 사항에 맞게 AKS 클러스터를 확장하세요. 클러스터를 완전히 활용할 수 있게 [Pod][scale] 수를 확장하고, 서비스의 필요에 맞게 클러스터의 [노드][nodes]를 확장하도록 유의하세요. 추천 시스템 서비스의 성능 및 처리량 요구 사항에 맞게 클러스터를 확장하는 방법에 대한 자세한 내용은 [Azure Container Service 카탈로그 확장][blog]을 참조하세요.

Azure Cosmos DB 성능을 관리하려면 초당 필요한 읽기 수를 측정하고, 필요한 [초당 RU][ru] 수(처리량)를 프로비전하세요. [분할 및 수평 확장][partition-data]에 대한 모범 사례를 따르세요.

## <a name="cost-considerations"></a>비용 고려 사항

이 시나리오의 주요 비용 동인은 다음과 같습니다.

- 학습에 필요한 Azure Databricks 클러스터 크기.
- 성능 요구 사항을 충족하는 데 필요한 AKS 클러스터 크기.
- 성능 요구 사항에 맞게 프로비전된 Azure Cosmos DB RU.

Spark 클러스터를 사용하지 않을 때 자주 재학습시키지 않고 끄는 방법으로 Azure Databricks 비용을 관리하세요. AKS 및 Azure Cosmos DB 비용은 사이트의 처리량 및 성능 요구 사항과 관련이 있으며, 사이트로 들어오는 트래픽의 양에 따라 확장 및 축소됩니다.

## <a name="deploy-the-solution"></a>솔루션 배포

이 아키텍처를 배포하려면 먼저 Azure Databricks 환경을 만들어 데이터를 준비하고 추천 시스템 모델을 학습시키세요.

1. [Azure Databricks 작업 영역][workspace]을 만듭니다.

2. Azure Databricks에 새 클러스터를 만듭니다. 다음과 같은 구성이 필요합니다.

    - 클러스터 모드: Standard
    - Databricks 런타임 버전: 4.1(Apache Spark 2.3.0, Scala 2.11 포함)
    - Python 버전: 3
    - 드라이버 유형: 표준\_DS3\_v2
    - 작업자 유형: 표준\_DS3\_v2(필요에 따라 최소 및 최대 구성)
    - 자동 종료: (필요 시)
    - Spark 구성: (필요 시)
    - 환경 변수: (필요 시)

3. 로컬 컴퓨터에 [Microsoft 추천 시스템][github] 리포지토리를 복제합니다.

4. 추천 시스템 폴더 안에 콘텐츠의 압축을 풉니다.

    ```console
    cd Recommenders
    zip -r Recommenders.zip
    ```

5. 다음과 같이 클러스터에 추천 시스템 라이브러리를 연결합니다.

    1. 다음 메뉴에서 라이브러리를 가져오는 옵션("jar 또는 egg와 같은 라이브러리를 가져오려면 여기를 클릭")을 사용하고 **여기를 클릭**을 누릅니다.

    2. 첫 번째 드롭다운 메뉴에서 **Python egg 또는 PyPI 업로드** 옵션을 선택합니다.

    3. **업로드할 라이브러리 egg 여기에 놓기**를 선택하고 방금 전에 만든 Recommenders.zip 파일을 선택합니다.

    4. **라이브러리 만들기**를 선택하여 .zip 파일을 업로드하고 작업 영역에서 사용할 수 있게 합니다.

    5. 다음 메뉴에서 클러스터에 라이브러리를 연결합니다.

6. 작업 영역에서 [ALS Movie Operationalization 예][als-example]를 가져옵니다.

7. ALS Movie Operationalization 노트북을 실행하여 특정 사용자를 위한 상위 10개 동영상 추천 항목을 제공하는 추천 API를 만드는 데 필요한 리소스를 만듭니다.

## <a name="related-architectures"></a>관련 아키텍처

Spark 및 Azure Databricks를 사용하여 예약된 [일괄 처리 점수 매기기 프로세스][batch-scoring]를 실행하는 참조 아키텍처도 구축했습니다. 새 추천 사항을 정기적으로 생성하는 데 추천되는 방법을 이해하려면 해당 참조 아키텍처를 참조하세요.

<!-- links -->
[aci]: /azure/container-instances/container-instances-overview
[aad]: /azure/active-directory-b2c/active-directory-b2c-overview
[aks]: /azure/aks/intro-kubernetes
[als]: https://spark.apache.org/docs/latest/ml-collaborative-filtering.html
[als-example]: https://github.com/Microsoft/Recommenders/blob/master/notebooks/04_operationalize/als_movie_o16n.ipynb
[autoscaling]: https://docs.azuredatabricks.net/user-guide/clusters/sizing.html
[autoscale]: https://docs.azuredatabricks.net/user-guide/clusters/sizing.html#autoscaling
[availability]: /azure/architecture/checklist/availability
[batch-scoring]: /azure/architecture/reference-architectures/ai/batch-scoring-databricks
[blob]: /azure/storage/blobs/storage-blobs-introduction
[blog]: https://blogs.technet.microsoft.com/machinelearning/2018/03/20/scaling-azure-container-service-cluster/
[clusters]: https://docs.azuredatabricks.net/user-guide/clusters/configure.html
[cosmosdb]: /azure/cosmos-db/introduction
[data-source]: https://docs.azuredatabricks.net/spark/latest/data-sources/index.html
[databricks]: /azure/azure-databricks/what-is-azure-databricks
[dsvm]: /azure/machine-learning/data-science-virtual-machine/overview
[dsvm-ubuntu]: /azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro
[eval-guide]: https://github.com/Microsoft/Recommenders/blob/master/notebooks/03_evaluate/evaluation.ipynb
[free]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F
[github]: https://github.com/Microsoft/Recommenders
[guide]: https://github.com/Microsoft/Recommenders/blob/master/notebooks/01_prepare_data/data_split.ipynb
[latency]: https://github.com/jessebenson/azure-performance
[mls]: /azure/machine-learning/service/
[n-tier]: /azure/architecture/reference-architectures/n-tier/n-tier-cassandra
[ndcg]: https://en.wikipedia.org/wiki/Discounted_cumulative_gain
[nodes]: /azure/aks/scale-cluster
[notebook]: https://github.com/Microsoft/Recommenders/notebooks/00_quick_start/als_pyspark_movielens.ipynb
[partition-data]: /azure/cosmos-db/partition-data
[redis]: /azure/redis-cache/cache-overview
[regions]: https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines&regions=all
[resiliency]: /azure/architecture/resiliency/
[ru]: /azure/cosmos-db/request-units
[sec-docs]: /azure/security/
[setup]: https://github.com/Microsoft/Recommenders/blob/master/SETUP.md%60
[scale]: /azure/aks/tutorial-kubernetes-scale
[sla]: https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/
[vm-size]: /azure/virtual-machines/virtual-machines-linux-change-vm-size
[workspace]: https://docs.azuredatabricks.net/getting-started/index.html
