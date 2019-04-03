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
# <a name="batch-scoring-of-r-machine-learning-models-on-azure"></a>Azure에서 R 기계 학습 모델 점수 매기기 배치

이 참조 아키텍처는 Azure Batch를 사용 하 여 R 모델을 사용 하 여 점수 매기기 일괄 처리를 수행 하는 방법을 보여 줍니다. 시나리오는 소매 저장소 판매 예측 기반 하지만 R 모델을 사용 하 여 큰 scaler에 대 한 예측 생성에 필요한 모든 시나리오에 대 한이 아키텍처를 일반화할 수 있습니다. 이 아키텍처에 대한 참조 구현은 [GitHub][github]에서 사용할 수 있습니다.

![아키텍처 다이어그램][0]

**시나리오**: 슈퍼마켓 체인이 예정 된 분기를 통해 제품의 판매를 예측 해야 합니다. 예측 이용할 자사의 공급망을 더 잘 관리 하 고 해당 저장소의 각 제품에 대 한 수요를 충족할 수 있는지 확인 합니다. 회사 업데이트 예측 매주는 지난 주의 새 판매 데이터를 사용할 수 있게 되며 다음 분기에 대 한 마케팅 전략 제품 설정 됩니다. 개별 판매 예측의 불확실성을 예측 하려면 변 위치 예측 생성 됩니다.

처리에는 다음 단계가 포함됩니다.

1. Azure Logic App을 일주일에 한 번 예측된 생성 프로세스를 트리거합니다.

1. 논리 앱 스케줄러 Batch 클러스터에서 점수 매기기 작업을 트리거하는 Docker 컨테이너를 실행 하는 Azure Container Instance를 시작 합니다.

1. 점수 매기기 작업 일괄 처리 클러스터의 노드에서 병렬로 실행 됩니다. 각 노드:

    1. 작업자 Docker 허브에서 Docker 이미지를 가져오고 및 컨테이너를 시작 합니다.

    1. 입력된 데이터를 읽고 미리 Azure Blob storage에서 R 모델을 학습 합니다.

    1. 예측을 생성 하기 위해 데이터를 얻었습니다.

    1. Blob 저장소에 예측된 결과 씁니다.

아래 그림에서는 네 가지 제품 (Sku)에 대 한 예측된 판매량에 하나의 저장소 나와 있습니다. 검정색 선은 판매 기록과, 파선 예측 중앙값 (q50), 분홍색 밴드 25 번째 및/72-다섯 번째 백분위 수를 나타내는 이며 파랑 밴드 다섯 번째 및 90 번째 백분위 수를 나타냅니다.

![판매 예측][1]

## <a name="architecture"></a>아키텍처

이 아키텍처는 다음과 같은 구성 요소로 구성됩니다.

[Azure Batch] [ batch] 가상 머신의 클러스터에서 병렬로 예측된 생성 작업을 실행 하는 데 사용 됩니다. 미리 학습 된 기계를 사용 하 여 예측을 수행 하는 학습 모델 R. Azure Batch에서 구현 된 클러스터에 제출 된 작업의 수에 따라 Vm 수를 자동으로 조정할 수 있습니다. 각 노드에서 데이터 점수를 매기고 예측을 생성 하는 Docker 컨테이너 내에서 R 스크립트를 실행 합니다.

[Azure Blob Storage] [ blob] 입력된 데이터, 미리 학습 된 기계 학습 모델 및 예측된 결과 저장 하는 데 사용 됩니다. 이 작업에 필요한 성능에 대 한 매우 비용 효율적인 저장소를 제공 합니다.

[Azure Container Instances] [ aci] 주문형 서버 리스 계산을 제공 합니다. 이 경우 예측을 생성 하는 일괄 처리 작업을 트리거하는 일정에 따라 컨테이너 인스턴스 배포 됩니다. 일괄 처리 작업을 사용 하 여 R 스크립트에서 트리거되는지 합니다 [doAzureParallel] [ doAzureParallel] 패키지 있습니다. 작업이 완료 되 면 자동으로 컨테이너 인스턴스를 종료 합니다.

[Azure Logic Apps] [ logic-apps] 일정에 따라 container instances를 배포 하 여 전체 워크플로 트리거합니다. Logic Apps에서 Azure Container Instances 커넥터를 다양 한 트리거 이벤트 시 배포할 인스턴스 수 있습니다.

## <a name="performance-considerations"></a>성능 고려 사항

### <a name="containerized-deployment"></a>컨테이너 화 된 배포

이 아키텍처를 사용 하 여 모든 R 스크립트 내에서 실행 [Docker](https://www.docker.com/) 컨테이너입니다. 이렇게 하면 스크립트가 실행 될 때마다에 동일한 R 버전 및 패키지 버전을 사용 하 여 일관 된 환경에서. 다른 R 패키지 종속성 집합을 사용 하기 때문에 별도 Docker 이미지 컨테이너를 스케줄러 및 작업자에 사용 됩니다.

Azure Container Instances는 scheduler 컨테이너를 실행 하는 서버 리스 환경을 제공 합니다. Scheduler 컨테이너 클러스터를 Azure Batch에서 실행 되는 개별 점수 매기기 작업을 트리거하는 R 스크립트를 실행 합니다.

일괄 처리 클러스터의 각 노드에 점수 매기기 스크립트를 실행 하는 작업자 컨테이너를 실행 합니다.

### <a name="parallelizing-the-workload"></a>작업 병렬 처리

일괄 처리 하면 R 모델을 사용 하 여 데이터 점수 매기기 작업을 병렬화 하는 방법을 것이 좋습니다. 입력된 데이터 점수 매기기 작업 클러스터 노드에 분산 될 수 있도록 어떤 이유로 든 분할 되어야 합니다. 워크 로드 배포에 가장 적합 한를 검색 하는 다른 방법을 시도 합니다. 경우에 따라 다음을 고려 합니다.

- 얼마나 많은 데이터를 로드 하 고 단일 노드의 메모리에서 처리 수입니다.
- 각 일괄 처리 작업을 시작 하는 오버 헤드입니다.
- R 모델을 로드 하는 오버 헤드입니다.

이 예제에 사용 되는 시나리오에서 모델 개체는 대형 및 개별 제품에 대 한 예측을 생성 하는 데 몇 초만 걸립니다. 이러한 이유로 제품을 그룹화 수 있으며 노드당 단일 일괄 처리 작업을 실행할 수 있습니다. 각 작업 내에서 루프는 순차적으로 제품에 대 한 예측을 생성합니다. 이 메서드는이 특정 워크 로드를 병렬화 하는 가장 효율적인 방법은 것으로 밝혀졌습니다. 더 작은 여러 일괄 처리 작업을 시작 하 고 반복적으로 R 모델을 로드 오버 헤드를 방지 하는 것입니다.

또 다른 방법은 제품당 하나의 일괄 처리 작업을 트리거할 수 있습니다. Azure Batch는 자동으로 작업 큐를 형성 하 고 노드를 사용할 수 있게 면 클러스터에서 실행 되도록 제출 합니다. 사용 하 여 [자동 크기 조정을] [ autoscale] 작업의 수에 따라 클러스터의 노드 수를 조정 합니다. 이 이렇게 하면 작업을 시작 및 모델 개체를 다시 로드 하는 오버 헤드를 정당화 각 점수 매기기 작업을 완료 하는 데는 비교적 오랜 시간이 걸리는 경우 것이 더 적절 하 게 합니다. 이 이렇게 간단 하 게 구현 되며 자동 크기 조정을 사용 하는 데 유연성을 제공-총 워크 로드의 크기를 알 수 없는 경우 사전에 중요 한 고려 사항입니다.

## <a name="monitoring-and-logging-considerations"></a>모니터링 및 로깅 고려 사항

### <a name="monitoring-azure-batch-jobs"></a>Azure Batch 작업 모니터링

모니터링 및에서 일괄 처리 작업을 종료 합니다 **작업** Azure portal에서 Batch 계정 창입니다. 일괄 처리에서 개별 노드의 상태를 포함 하 여 클러스터를 모니터링 합니다 **풀** 창입니다.

### <a name="logging-with-doazureparallel"></a>DoAzureParallel을 사용 하 여 로깅

DoAzureParallel 패키지는 자동으로 Azure Batch에서 제출 된 모든 작업에 대 한 모든 stdout/stderr 로그를 수집 합니다. 설치 프로그램에서 만든 저장소 계정에서 찾을 수 있습니다. 해당 명령을 보려면 사용 하 여 저장소 탐색 도구와 같은 [Azure Storage 탐색기] [ storage-explorer] 또는 Azure portal.

개발 하는 동안 일괄 처리 작업을 신속 하 게 디버깅을 사용 하 여 로컬 R 세션에서 로그를 인쇄 합니다 [getJobFiles] [ getJobFiles] doAzureParallel의 함수입니다.

## <a name="cost-considerations"></a>비용 고려 사항

이 참조 아키텍처에 사용 되는 계산 리소스가 가장 비용이 높은 구성 요소입니다. 이 시나리오에 대 한 고정 된 크기의 클러스터에 작업이 트리거되고 다음 종료 작업이 완료 될 때마다 만들어집니다. 클러스터 노드 시작, 실행 또는 종료 하는 동안에 비용이 발생 합니다. 이 방법은 있는 예측을 생성 하는 데 필요한 계산 리소스를 일정 하 게 유지 비교적 작업에서 작업 하는 시나리오에 적합 합니다.

여기서 작업을 완료 하는 데 필요한 계산의 크기는 미리 알 수 없는 시나리오에서 자동 크기 조정을 사용 하는 것이 적합할 수 있습니다. 이 방법을 사용 하 여 클러스터의 크기는 확장 또는 축소할 작업의 크기에 따라 합니다. Azure Batch에서는 자동 크기 조정 공식 사용 하 여 클러스터를 정의할 때 설정할 수 있는 범위를 지원 합니다 [doAzureParallel] [ doAzureParallel] API.

일부 시나리오에서는 작업 간의 시간 너무 짧을 수 종료 하 고 클러스터를 시작 합니다. 이러한 경우에 해당 하는 경우 작업 간에 실행 하는 클러스터를 유지 합니다.

Azure Batch 및 doAzureParallel 우선 순위가 낮은 Vm 사용을 지원 합니다. 이러한 Vm 할인 있지만 위험을 높은 우선 순위가 다른 작업으로 책정 되 고 함께 제공 됩니다. 따라서 이러한 Vm 사용에 중요 한 프로덕션 워크 로드에 대 한 없습니다 권장 됩니다. 그러나 매우 유용에 대 한 실험적 또는 개발 워크 로드.

## <a name="deployment"></a>배포

이 참조 아키텍처를 배포 하려면에 설명 된 단계를 수행 합니다 [GitHub] [ github] 리포지토리.


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