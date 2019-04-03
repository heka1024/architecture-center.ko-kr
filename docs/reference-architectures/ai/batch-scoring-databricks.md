---
title: Azure Databricks에서 Spark 모델의 일괄 처리 점수 매기기
description: Azure Databricks를 사용하여 일정에 따라 Apache Spark 분류 모델의 일괄 처리 점수 매기기를 수행하는 확장 가능한 솔루션을 빌드합니다.
author: njray
ms.date: 02/07/2019
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: azcat-ai
ms.openlocfilehash: cba8d272ddbdbf2c2da94f68b288e9fb79be7de2
ms.sourcegitcommit: 1a3cc91530d56731029ea091db1f15d41ac056af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58887814"
---
# <a name="batch-scoring-of-spark-machine-learning-models-on-azure-databricks"></a>Azure Databricks에서 Spark 기계 학습 모델 점수 매기기 배치

이 참조 아키텍처에는 Azure용으로 최적화된 Apache Spark 기반 분석 플랫폼인 Azure Databricks를 사용하여 일정에 따라 Apache Spark 분류 모델의 일괄 처리 점수 매기기를 수행하는 확장 가능한 솔루션을 빌드하는 방법을 소개합니다. 이 솔루션은 다른 시나리오에도 일반적으로 사용할 수 있는 템플릿으로 사용 가능합니다.

이 아키텍처에 대한 참조 구현은  [GitHub][github]에서 사용할 수 있습니다.

![Azure Databricks에서 Spark 모델의 일괄 처리 점수 매기기](./_images/batch-scoring-spark.png)

**시나리오**: 자산을 많이 사용하는 업계의 한 기업이 예기치 않은 기계 오류와 연관된 비용과 가동 중지 시간을 최소화하고자 합니다. 이 기업은 기계에서 수집된 IoT 데이터를 사용해 예측 유지 관리 모델을 만들 수 있습니다. 이 모델을 통해 구성 요소를 사전에 유지 관리하고 오류가 발생하기 전에 수리할 수 있습니다. 기계 구성 요소의 사용률을 최대화하면 비용을 제어하고 가동 중지 시간을 줄일 수 있습니다.

예측 유지 관리 모델은 기계에서 데이터를 수집하고 구성 요소 오류의 예제 기록을 보존합니다. 그리고 나면 이 모델을 사용하여 구성 요소의 현재 상태를 모니터링하고 지정된 구성 요소에서 조만간 오류가 발생할지 여부를 예측할 수 있습니다. 일반적인 사용 사례와 모델링 방식은 [예측 유지 관리 솔루션에 대한 Azure AI 가이드][ai-guide]를 참조하세요.

이 참조 아키텍처는 구성 요소 기계에 새 데이터가 있으면 트리거되는 워크로드용으로 설계되었습니다. 처리에는 다음 단계가 포함됩니다.

1. 외부 데이터 저장소의 데이터를 Azure Databricks 데이터 저장소로 수집합니다.

2. 데이터를 학습 데이터 세트로 변환한 다음 Spark MLlib 모델을 작성하여 기계 학습 모델을 학습시킵니다. MLlib는 Spark 데이터 확장성 기능을 활용할 수 있도록 최적화된 가장 일반적인 기계 학습 알고리즘과 유틸리티로 구성되어 있습니다.

3. 데이터를 점수 매기기 데이터 세트로 변환하여 구성 요소 오류를 예측(분류)하기 위해 학습된 모델을 적용합니다. Spark MLLib 모델을 사용하여 데이터의 점수를 매깁니다.

4. 사후 처리에 사용할 수 있도록 Databricks 데이터 저장소에 결과를 저장합니다.

이러한 각 작업을 수행하기 위한 Notebook은  [GitHub][github]에서 제공됩니다.

## <a name="architecture"></a>아키텍처

이 아키텍처는 순차적으로 실행되는 [Notebook][notebooks] 세트를 기준으로 하여 [Azure Databricks][databricks] 내에 완전히 포함되는 데이터 흐름을 정의하며, 다음 구성 요소로 구성됩니다.

**[데이터 파일][github]**. 참조 구현에서는 정적 데이터 파일 5개에 포함된 시뮬레이션된 데이터 세트를 사용합니다.

**[수집][notebooks]**. 데이터 수집 Notebook은 입력 데이터 파일을 Databrick 데이터 세트 컬렉션에 다운로드합니다. 실제 시나리오에서는 IoT 장치의 데이터가 Azure SQL Server 또는 Azure Blob Storage와 같은 Databricks에서 액세스 가능한 스토리지로 스트리밍됩니다. Databricks는 여러 [데이터 원본][data-sources]을 지원합니다.

**학습 파이프라인**. 이 Notebook은 기능 엔지니어링 Notebook을 실행하여 수집된 데이터에서 분석 데이터 세트를 만듭니다. 그런 다음 [Apache Spark MLlib][mllib] 확장 가능 기계 학습 라이브러리를 사용해 기계 학습 모델을 학습시키는 모델 빌드 Notebook을 실행합니다.

**점수 매기기 파이프라인**. 이 Notebook은 기능 엔지니어링 Notebook을 실행하여 수집된 데이터에서 점수 매기기 데이터 세트를 만들고 점수 매기기 Notebook을 실행합니다. 점수 매기기 Notebook은 학습된 [Spark MLlib][mllib-spark] 모델을 사용해 점수 매기기 데이터 세트에서 관찰된 내용에 대한 예측 정보를 생성합니다. 예측 정보는 Databricks 데이터 저장소의 새로운 데이터 세트인 결과 저장소에 저장됩니다.

**스케줄러**. 예약된 Databricks [작업][job]이 Spark 모델을 사용하여 일괄 처리 점수 매기기를 처리합니다. 이 작업은 점수 매기기 파이프라인 Notebook을 실행하며, Notebook 매개 변수를 통해 변수 인수를 전달하여 점수 매기기 데이터 세트 생성을 위한 세부 정보와 결과 데이터 세트를 저장할 위치를 지정합니다.

해당 시나리오는 파이프라인 흐름으로 생성됩니다. 각 Notebook은 각 작업(수집, 기능 엔지니어링, 모델 빌드, 모델 점수 매기기)을 수행할 수 있도록 최적화되어 있습니다. 이 작업을 수행하기 위해 기능 엔지니어링 Notebook은 학습, 보정, 테스트 또는 점수 매기기 작업용으로 일반 데이터 세트를 생성하도록 설계되어 있습니다. 이 시나리오에서는 이러한 작업에 임시 분할 전략을 사용하므로 Notebook 매개 변수를 사용하여 날짜 범위 필터링을 설정합니다.

이 시나리오에서는 일괄 처리 파이프라인을 작성하므로 파이프라인 Notebook의 출력을 살펴볼 수 있도록 검사 Notebook 세트(선택 사항)가 제공됩니다. 이러한 Notebook 세트는 GitHub 리포지토리에서 확인할 수 있습니다.

- `1a_raw-data_exploring`
- `2a_feature_exploration`
- `2b_model_testing`
- `3b_model_scoring_evaluation`

## <a name="recommendations"></a>권장 사항

Databricks는 학습된 모델을 로드 및 배포하여 새 데이터로 예측을 할 수 있도록 설정되어 있습니다. 이 시나리오에서는 다음과 같은 이점이 추가로 제공되므로 Databricks를 사용합니다.

- Azure Active Directory 자격 증명을 사용하는 Single Sign-On 지원
- 프로덕션 파이프라인에 대해 작업을 실행할 수 있는 작업 스케줄러
- 공동 작업, 대시보드, REST API가 포함된 완벽한 대화형 Notebook
- 무제한 클러스터 제공(어떤 크기로든 확장 가능)
- 고급 보안, 역할 기반 액세스 제어 및 감사 로그

Azure Databricks 서비스와 상호 작용하려는 경우 웹 브라우저나 [CLI(명령줄 인터페이스)][cli]에서 [작업 영역][workspace] 인터페이스를 사용합니다. Python 2.7.9~3.6을 지원하는 플랫폼에서 Databricks CLI에 액세스합니다.

참조 구현에서는 [Notebook][notebooks]을 사용하여 작업을 순차적으로 실행합니다. 각 Notebook은 중간 데이터 아티팩트(학습, 테스트, 점수 매기기 또는 결과 데이터 세트)를 입력 데이터와 같은 데이터 저장소에 저장합니다. 따라서 특정 사용 사례에 필요한 대로 Notebook을 쉽게 사용할 수 있습니다. 실제로는 Notebook이 스토리지를 직접 읽고 스토리지에 쓰기 저장할 수 있도록 데이터 원본을 Azure Databricks 인스턴스에 연결합니다.

Databricks 사용자 인터페이스, 데이터 저장소 또는 Databricks [CLI][cli]를 통해 필요에 따라 작업 실행을 모니터링할 수 있습니다. Databricks에서 제공되는 [이벤트 로그][log] 및 기타 [메트릭][metrics]을 사용하여 클러스터를 모니터링합니다.

## <a name="performance-considerations"></a>성능 고려 사항

Azure Databricks 클러스터는 기본적으로 자동 크기 조정이 가능하므로, Databricks는 작업 특성에 맞게 작업자를 계정에 동적으로 재할당합니다. 파이프라인의 특정 부분에서는 다른 부분보다 컴퓨팅이 더 많이 수행될 수도 있습니다. Databricks는 작업의 이러한 단계 중에 작업자를 더 추가하며 작업자가 더 이상 필요하지 않을 때는 제거합니다. 자동 크기 조정 기능을 사용하면 [클러스터 사용률][cluster]을 더 쉽게 높일 수 있습니다. 워크로드와 일치하는 클러스터를 프로비전할 필요가 없기 때문입니다.

또한 [Azure Data Factory][adf]를 Azure Databricks와 함게 사용하면 더 복잡한 예약 파이프라인을 개발할 수 있습니다.

## <a name="storage-considerations"></a>저장소 고려 사항

이 참조 구현에서는 작업을 간편하게 수행할 수 있도록 데이터가 Databricks 내에 직접 저장됩니다. 하지만 프로덕션 설정에서는 [Azure Blob Storage][blob] 등의 클라우드 데이터 스토리지에 데이터를 저장할 수 있습니다. [Databricks][databricks-connect]는 Azure Data Lake Store, Azure SQL Data Warehouse, Azure Cosmos DB, Apache Kafka 및 Hadoop도 지원합니다.

## <a name="cost-considerations"></a>비용 고려 사항

Azure Databricks는 관련 비용이 부과되는 프리미엄 Spark 제품입니다. 또한 표준 및 프리미엄 Databricks [가격 책정 계층][pricing]이 제공됩니다.

이 시나리오에서는 표준 가격 책정 계층을 사용해도 충분합니다. 그러나 특정 애플리케이션에 규모가 더 큰 워크로드를 처리하기 위해 자동으로 크기가 조정되는 클러스터나 대화형 Databricks 대시보드가 필요한 경우 프리미엄 수준을 선택하면 비용이 더 증가합니다.

솔루션 Notebook은 최소한의 편집을 수행해 Databricks 관련 패키지를 제거하면 모든 Spark 기반 플랫폼에서 실행할 수 있습니다. 다양한 Azure 플랫폼용으로 제공되는 다음의 유사한 솔루션을 참조하세요.

- [Azure Machine Learning Studio의 Python][python-aml]
- [SQL Server R Services][sql-r]
- [Azure Data Science Virtual Machine의 PySpark][py-dvsm]

## <a name="deploy-the-solution"></a>솔루션 배포

이 참조 아키텍처를 배포하려면  [GitHub][github] 리포지토리에 설명되어 있는 단계에 따라 Azure Databricks에서 일괄 처리를 통해 Spark 모델의 점수를 매기는 확장 가능한 솔루션을 빌드합니다.

## <a name="related-architectures"></a>관련 아키텍처

Spark를 사용하여 미리 계산된 오프라인 점수로 [실시간 추천 시스템][recommendation]을 빌드하는 참조 아키텍처도 빌드되었습니다. 이러한 추천 시스템은 점수가 일괄 처리되는 일반적인 시나리오입니다.

[adf]: https://azure.microsoft.com/blog/operationalize-azure-databricks-notebooks-using-data-factory/
[ai-guide]: /azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance
[blob]: https://docs.databricks.com/spark/latest/data-sources/azure/azure-storage.html
[cli]: https://docs.databricks.com/user-guide/dev-tools/databricks-cli.html
[cluster]: https://docs.azuredatabricks.net/user-guide/clusters/sizing.html
[databricks]: /azure/azure-databricks/
[databricks-connect]: /azure/azure-databricks/databricks-connect-to-data-sources
[data-sources]: https://docs.databricks.com/spark/latest/data-sources/index.html
[github]: https://github.com/Azure/BatchSparkScoringPredictiveMaintenance
[job]: https://docs.databricks.com/user-guide/jobs.html
[log]: https://docs.databricks.com/user-guide/clusters/event-log.html
[metrics]: https://docs.databricks.com/user-guide/clusters/metrics.html
[mllib]: https://docs.databricks.com/spark/latest/mllib/index.html
[mllib-spark]: https://docs.databricks.com/spark/latest/mllib/index.html#apache-spark-mllib
[notebooks]: https://docs.databricks.com/user-guide/notebooks/index.html
[pricing]: https://azure.microsoft.com/en-us/pricing/details/databricks/
[python-aml]: https://gallery.azure.ai/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1
[py-dvsm]: https://gallery.azure.ai/Tutorial/Predictive-Maintenance-using-PySpark
[recommendation]: /azure/architecture/reference-architectures/ai/real-time-recommendation
[sql-r]: https://gallery.azure.ai/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1
[workspace]: https://docs.databricks.com/user-guide/workspace.html
