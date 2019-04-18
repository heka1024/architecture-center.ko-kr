---
title: Azure에서 Python 모델 일괄 채점
description: Azure Machine Learning Service를 사용하여 정기적으로 모델을 병렬로 일괄 채점하기 위한 확장 가능한 솔루션을 빌드합니다.
author: njray
ms.date: 01/30/2019
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: azcat-ai, AI
ms.openlocfilehash: 9341b9e4c17025e9623902a6202076c352b237b9
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59640552"
---
# <a name="batch-scoring-of-python-machine-learning-models-on-azure"></a>Azure에서 Python 기계 학습 모델 점수 매기기 배치

이 참조 아키텍처에서는 Azure Machine Learning Service를 사용하여 정기적으로 여러 모델을 병렬로 일괄 채점하기 위한 확장 가능한 솔루션을 빌드하는 방법을 보여줍니다. 솔루션은 템플릿으로 사용할 수 있으며 여러 문제에 맞게 일반화할 수 있습니다.

이 아키텍처에 대한 참조 구현은 [GitHub][github]에서 사용할 수 있습니다.

![Azure에서 Python 모델 일괄 채점](./_images/batch-scoring-python.png)

**시나리오**: 이 솔루션은 각 디바이스가 센서 판독값을 지속적으로 전송하는 IoT 환경에서 여러 디바이스의 작동을 모니터링합니다. 각 디바이스는 사전 정의된 시간 간격 동안 집계된 일련의 측정값이 변칙에 해당하는지 여부를 예측하는 데 사용되어야 하는 사전 학습된 변칙 검색 모델과 관련이 있는 것으로 간주됩니다. 실제 시나리오에서 이는 학습 또는 실시간 채점에 사용되기 전에 필터링되고 집계되어야 하는 센서 판독값 스트림이 될 수 있습니다. 이 솔루션은 채점 작업 실행 시 간소화를 위해 동일한 데이터 파일을 사용합니다.

이 참조 아키텍처는 정기적으로 트리거되는 워크로드용으로 설계되었습니다. 처리에는 다음 단계가 포함됩니다.

1. Azure Event Hubs로 수집을 위한 센서 판독값을 전송합니다.
2. 스트림 처리를 수행하고 원시 데이터를 저장합니다.
3. 작업을 시작할 준비가 된 Machine Learning 클러스터에 데이터를 전송합니다. 클러스터의 각 노드는 특정 센서에 대한 점수 매기기 작업을 실행합니다. 
4. Machine Learning Python 스크립트를 사용하여 병렬로 점수 매기기 작업을 실행하는 점수 매기기 파이프라인을 실행합니다. 파이프라인은 미리 정의된 기간에 실행되도록 생성, 게시 및 예약됩니다.
5. 예측을 생성하고 나중에 사용할 수 있도록 Blob Storage에 저장합니다.

## <a name="architecture"></a>아키텍처

이 아키텍처는 다음과 같은 구성 요소로 구성됩니다.

[Azure Event Hubs][event-hubs]. 이 메시지 수집 서비스는 초당 수백만 개의 이벤트 메시지를 수집할 수 있습니다. 이 아키텍처에서 센서는 데이터 스트림을 이벤트 허브로 보냅니다.

[Azure Stream Analytics][stream-analytics]. 이벤트 처리 엔진입니다. Stream Analytics 작업은 이벤트 허브에서 데이터 스트림을 읽고 스트림 처리를 수행합니다.

[Azure SQL Database][sql-database]. 센서 판독값의 데이터가 SQL Database에 로드됩니다. SQL이 처리, 스트리밍된 데이터(테이블 형식 및 구조적)를 저장하는 친숙한 방식이지만 다른 데이터 저장소도 사용할 수 있습니다.

[Azure Machine Learning Service][amls]. Machine Learning은 기계 학습 모델을 규모에 맞게 학습, 점수 매기기, 배포 및 관리하기 위한 클라우드 서비스입니다. 일괄 채점의 컨텍스트에서 Machine Learning은 자동 크기 조정 옵션을 사용하여 필요 시 가상 머신의 클러스터를 만들고, 클러스터의 각 노드는 특정 센서에 대한 채점 작업을 실행합니다. 점수 매기기 작업은 Machine Learning을 통해 큐에 대기되고 관리되는 Python 스크립트 단계와 병렬로 실행됩니다. 이러한 단계는 미리 정의된 시간 간격으로 실행하도록 생성, 게시 및 예정된 Machine Learning 파이프라인의 일부입니다.

[Azure Blob Storage][storage]. Blob 컨테이너는 미리 학습된 모델, 데이터 및 출력 예측 항목을 저장하는 데 사용됩니다. 모델은 [01_create_resources.ipynb][create-resources] 노트북의 Blob Storage에 업로드됩니다. 이러한 [1클래스 SVM][one-class-svm] 모델은 여러 디바이스의 다양한 센서 값을 나타내는 데이터를 학습합니다. 이 솔루션에서는 데이터 값이 고정된 시간 간격에 걸쳐 집계된다고 가정합니다.

[Azure Container Registry][acr]. 채점용 Python [스크립트][pyscript]는 클러스터의 각 노드에 생성된 Docker 컨테이너에서 실행되며, 관련 센서 데이터를 읽고, 예측 항목을 생성하여 Blob Storage에 저장합니다.

## <a name="performance-considerations"></a>성능 고려 사항

표준 Python 모델의 경우 일반적으로 워크로드를 처리할 CPU가 충분하다고 여겨집니다. 이 아키텍처에서는 CPU를 사용합니다. 그러나 [딥 러닝 워크 로드][deep]를 많은 양만큼 Gpu 일반적으로 보다 성능이 뛰어난 Cpu &mdash; Cpu의 많은 클러스터를 비교할 수 있는 성능을 얻기 위해 일반적으로 필요 합니다.

### <a name="parallelizing-across-vms-versus-cores"></a>코어와 Vm에서 병렬 처리

일괄 처리 모드로 여러 모델의 채점 프로세스를 실행할 때는 VM 전체에서 작업을 병렬 처리해야 합니다. 두 가지 방법이 가능합니다.

* 저비용 VM을 사용하여 대규모 클러스터를 만듭니다.

* 각각 추가 코어가 제공되는 고성능 VM을 사용하여 소규모 클러스터를 만듭니다.

일반적으로 표준 Python 모델을 채점하는 작업은 딥 러닝 모델을 채점하는 것만큼 까다롭지 않으며, 소규모 클러스터가 큐에 저장된 여러 모델을 효율적으로 처리할 수 있어야 합니다. 데이터 세트 크기가 증가함에 따라 클러스터 노드 수를 늘릴 수 있습니다.

이 시나리오에서는 편의상 단일 Machine Learning 파이프라인 단계 내에서 하나의 채점 작업이 제출됩니다. 그러나 동일한 파이프라인 단계 내에서 여러 데이터 청크를 채점하는 것이 더 효율적일 수 있습니다. 이러한 경우 여러 데이터 세트에서 읽을 사용자 지정 코드를 작성하고, 단일 단계 실행 중에 이에 대한 채점 스크립트를 실행합니다.

## <a name="management-considerations"></a>관리 고려 사항

- **작업 모니터링**. 실행 중인 작업의 진행률을 모니터링하는 것이 중요하지만 활성 노드의 클러스터 전체를 모니터링하기는 어려울 수 있습니다. 클러스터의 노드 상태를 검사하려면 [Azure Portal][portal]을 사용하여 [기계 학습 작업 영역][ml-workspace]을 관리합니다. 노드가 비활성 상태이거나 작업이 실패한 경우 오류 로그가 Blob Storage에 저장되며 파이프라인 섹션에서도 액세스할 수 있습니다. 모니터링을 추가로 보완하려면 [Application Insights][app-insights]에 로그를 연결하거나 클러스터 및 해당 작업의 상태를 폴링하기 위한 별도의 프로세스를 실행합니다.
- **로깅** Machine Learning Service는 모든 stdout/stderr을 관련 Azure Storage 계정에 기록합니다. 로그 파일을 손쉽게 보려면 [Azure Storage 탐색기][explorer]와 같은 스토리지 탐색 도구를 사용합니다.

## <a name="cost-considerations"></a>비용 고려 사항

이 참조 아키텍처에서 사용된 가장 광범위한 구성 요소는 컴퓨팅 리소스입니다. 컴퓨팅 클러스터 크기는 큐의 작업에 따라 확장 및 축소됩니다. 컴퓨팅의 프로비전 구성을 수정하여 Python SDK를 통해 자동 크기 조정을 프로그래밍 방식으로 활성화합니다. 또는 [Azure CLI][cli]를 사용하여 클러스터의 자동 크기 조정 매개 변수를 설정합니다.

즉각적인 처리가 필요하지 않은 작업의 경우, 기본 상태(최소)가 0 노드의 클러스터가 되도록 자동 크기 조정 수식을 구성합니다. 이 구성을 사용하면 클러스터는 0개 노드로 시작하고 큐에서 작업을 감지할 때만 규모가 확장됩니다. 일괄 채점 프로세스가 하루에 몇 번 또는 그 이하로만 발생할 경우 이 설정을 통해 비용을 크게 절감할 수 있습니다.

자동 크기 조정은 서로 너무 가깝게 위치하는 일괄 처리 작업에는 적절하지 않을 수 있습니다. 클러스터가 스핀 업 및 스핀 다운되는 데 걸리는 시간도 비용을 발생할 수 있으므로, 일괄 처리 워크로드가 이전 작업이 종료되고 몇 분 안에 시작될 경우 작업 중간에 클러스터를 계속 실행하도록 하는 것이 좀 더 비용 효율적일 수 있습니다. 이는 채점 프로세스가 매우 자주(예: 매 시간) 실행되도록 예약되었는지, 아니면 가끔(예: 매달) 실행되도록 예약되었는지에 따라 달라집니다.

## <a name="deployment"></a>배포

이 참조 아키텍처를 배포하려면 [GitHub 리포지토리][github]에 설명된 단계를 따르세요.

[acr]: /azure/container-registry/container-registry-intro
[ai]: /azure/application-insights/app-insights-overview
[aml-compute]: /azure/machine-learning/service/how-to-set-up-training-targets#amlcompute
[amls]: /azure/machine-learning/service/overview-what-is-azure-ml
[automatic-scaling]: /azure/batch/batch-automatic-scaling
[azure-files]: /azure/storage/files/storage-files-introduction
[cli]: /cli/azure
[create-resources]: https://github.com/Microsoft/AMLBatchScoringPipeline/blob/master/01_create_resources.ipynb
[deep]: /azure/architecture/reference-architectures/ai/batch-scoring-deep-learning
[event-hubs]: /azure/event-hubs/event-hubs-geo-dr
[explorer]: https://azure.microsoft.com/en-us/features/storage-explorer/
[github]: https://github.com/Microsoft/AMLBatchScoringPipeline
[one-class-svm]: http://scikit-learn.org/stable/modules/generated/sklearn.svm.OneClassSVM.html
[portal]: https://portal.azure.com
[ml-workspace]: /azure/machine-learning/studio/create-workspace
[python-script]: https://github.com/Azure/BatchAIAnomalyDetection/blob/master/batchai/predict.py
[pyscript]: https://github.com/Microsoft/AMLBatchScoringPipeline/blob/master/scripts/predict.py
[storage]: /azure/storage/blobs/storage-blobs-overview
[stream-analytics]: /azure/stream-analytics/
[sql-database]: /azure/sql-database/
[app-insights]: /azure/application-insights/app-insights-overview
