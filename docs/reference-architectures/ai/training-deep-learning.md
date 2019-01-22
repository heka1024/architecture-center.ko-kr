---
title: Azure에서 딥 러닝 모델의 분산 학습
description: 이 참조 아키텍처에서는 Azure Batch AI를 사용하여 GPU 사용 VM 클러스터 전반에서 딥 러닝 모델의 분산 학습을 수행하는 방법을 보여 줍니다.
author: njray
ms.date: 01/14/19
ms.custom: azcat-ai
ms.openlocfilehash: 800defeb851f5a31dc730038c3699e1a3d54b923
ms.sourcegitcommit: d5ea427c25f9f7799cc859b99f328739ca2d8c1c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/15/2019
ms.locfileid: "54307782"
---
# <a name="distributed-training-of-deep-learning-models-on-azure"></a>Azure에서 딥 러닝 모델의 분산 학습

이 참조 아키텍처에서는 GPU 사용 VM 클러스터 전반에서 딥 러닝 모델의 분산 학습을 수행하는 방법을 보여 줍니다. 시나리오는 이미지 분류이지만, 솔루션은 구분 및 개체 감지와 같은 다른 딥 러닝 시나리오에 대해 일반화할 수 있습니다.

이 아키텍처에 대한 참조 구현은 [GitHub][github]에서 사용할 수 있습니다.

![분산 딥 러닝 아키텍처][0]

**시나리오**: 이미지 분류는 컴퓨터 비전에서 널리 적용되는 기술이며, CNN(나선형 신경망)을 학습함으로써 해결되는 경우가 많습니다. 특히 큰 데이터 세트가 있는 대규모 모델의 경우 학습 프로세스는 단일 GPU에서 몇 주 또는 몇 개월이 걸릴 수 있습니다. 상황에 따라 모델이 너무 커서 GPU에 적절한 일괄 처리 크기를 맞출 수 없는 경우가 있습니다. 이러한 상황에서 분산 학습을 사용하면 학습 시간을 줄일 수 있습니다.

이 특정 시나리오에서 [ResNet50 CNN 모델][resnet]은 [ Imagenet 데이터 세트][imagenet] 및 가상 데이터에 [Horovod][horovod]를 사용하여 학습됩니다. 참조 구현에서는 가장 인기 있는 세 가지 딥 러닝 프레임워크인 TensorFlow, Keras 및 PyTorch를 사용하여 이 작업을 수행하는 방법을 보여 줍니다.

동기 또는 비동기 업데이트를 기반으로 하는 데이터 병렬 및 모델 병렬 방식을 포함한 분산 방식으로 딥 러닝 모델을 학습하는 여러 가지 방법이 있습니다. 현재 가장 일반적인 시나리오는 동기 업데이트와 병렬로 처리되는 데이터입니다. 이 방법은 가장 쉽게 구현할 수 있고 대부분의 사용 사례에 적합합니다.

동기 업데이트를 사용하는 데이터 병렬 분산 학습에서는 모델이 *n*개의 하드웨어 디바이스에 복제됩니다. 학습 샘플의 미니 일괄 처리(mini-batch)는 *n*개의 마이크로 일괄 처리(micro-batch)로 나뉩니다. 각 디바이스는 마이크로 일괄 처리에 대해 정방향 및 역방향 전달을 수행합니다. 디바이스에서 프로세스가 완료되면 다른 디바이스와 업데이트를 공유합니다. 이러한 값은 전체 미니 일괄 처리에 대한 업데이트된 가중치를 계산하는 데 사용되며, 가중치는 모델 간에 동기화됩니다. 이 시나리오는 [GitHub][github] 리포지토리에서 설명됩니다.

![데이터 병렬 분산 학습][1]

이 아키텍처는 모델 병렬 및 비동기 업데이트에도 사용할 수 있습니다. 모델 병렬 분산 학습에서는 모델이 *n*개의 하드웨어 디바이스로 분할되며, 각 디바이스는 모델의 일부를 보유합니다. 가장 간단한 구현에서 각 디바이스는 네트워크 계층을 보유할 수 있으며, 정보는 정방향 및 역방향 전달 중에 디바이스 간에 전달됩니다. 더 큰 신경망은 이 방식으로 학습되지만, 디바이스에서 정방향 또는 역방향 전달이 완료될 때까지 서로를 계속 기다리므로 성능이 떨어질 수 있습니다. 일부 고급 기술에서 가상 그라데이션을 사용하여 이 문제를 부분적으로 완화시키려고 합니다.

학습 단계는 다음과 같습니다.

1. 클러스터에서 실행할 스크립트를 만들고, 모델을 학습한 다음, 파일 스토리지로 전송합니다.

1. 데이터를 Blob Storage에 기록합니다.

1. Batch AI 파일 서버를 만들고, 데이터를 Blob Storage에서 이 서버로 다운로드합니다.

1. 각 딥 러닝 프레임워크에 대한 Docker 컨테이너를 만들고, 컨테이너 레지스트리(Docker 허브)로 전송합니다.

1. Batch AI 파일 서버도 탑재하는 Batch AI 풀을 만듭니다.

1. 작업을 제출합니다. 각각 적절한 Docker 이미지와 스크립트를 가져옵니다.

1. 작업이 완료되면 모든 결과를 Files 스토리지에 기록합니다.

## <a name="architecture"></a>아키텍처

이 아키텍처는 다음과 같은 구성 요소로 구성됩니다.

**[Azure Batch AI][batch-ai]** 는 필요에 따라 리소스를 강화하거나 축소하여 이 아키텍처에서 중심적인 역할을 수행합니다. Batch AI는 VM 클러스터를 프로비전 및 관리하고, 작업을 예약하고, 결과를 수집하고, 리소스 크기를 조정하고, 오류를 처리하고, 적절한 스토리지를 만드는 데 도움이 되는 서비스입니다. 딥 러닝 작업을 위해 GPU 사용 VM을 지원합니다. Batch AI에서 Python SDK 및 CLI(명령줄 인터페이스)를 사용할 수 있습니다.

> [!NOTE]
> Azure Batch AI 서비스는 2019년 3월에 사용 중지되며, 이제 [Azure Machine Learning Service][amls]에서 규모에 맞게 해당 학습 및 채점 기능을 사용할 수 있습니다. 이 참조 아키텍처는 기계 학습 모델을 학습, 배포 및 채점하기 위해 [Azure Machine Learning 컴퓨팅][aml-compute]이라는 관리형 컴퓨팅 대상을 제공하는 Machine Learning을 사용하도록 곧 업데이트 될 예정입니다.

**[Blob 스토리지][azure-blob]** 는 데이터를 준비하는 데 사용됩니다. 이 데이터는 학습 중에 Batch AI 파일 서버로 다운로드됩니다.

**[Azure Files][files]** 는 스크립트, 로그 및 학습의 최종 결과를 저장하는 데 사용됩니다. File 스토리지는 로그 및 스크립트 저장에 적합하지만 Blob Storage만큼 성능이 좋지 않으므로 데이터 집약적인 작업에는 사용할 수 없습니다.

**[Batch AI 파일 서버][batch-ai-files]** 는 이 아키텍처에서 학습 데이터를 저장하는 데 사용되는 단일 노드 NFS 공유입니다. Batch AI는 NFS 공유를 만들어 클러스터에 탑재합니다. Batch AI 파일 서버는 필요한 처리량을 통해 클러스터에 데이터를 제공하도록 추천되는 방법입니다.

**[Docker 허브][docker]** 는 Batch AI에서 학습을 실행하기 위해 사용하는 Docker 이미지를 저장하는 데 사용됩니다. 이 아키텍처에서는 Docker 허브를 사용하기 쉽고 Docker 사용자의 기본 이미지 리포지토리이므로 이 항목을 선택했습니다. [Azure Container Registry][acr]를 사용할 수도 있습니다.

## <a name="performance-considerations"></a>성능 고려 사항

Azure에서는 딥 러닝 모델을 학습하는 데 적합한 4개의 [GPU 사용 VM 유형][gpu]을 제공합니다. 가격과 속도의 범위는 다음과 같이 다양합니다.

| **Azure VM 시리즈** | **NVIDIA GPU** |
|---------------------|----------------|
| NC                  | K80            |
| ND                  | P40            |
| NCv2                | P100           |
| NCv3                | V100           |

규모를 확장하기 전에 학습을 강화하는 것이 좋습니다. 예를 들어 여러 개의 K80으로 구성되는 클러스터를 시도하기 전에 하나의 V100을 시도합니다.

다음 그래프에서는 Batch AI에서 TensorFlow 및 Horovod를 사용하여 수행된 [벤치마킹 테스트][benchmark]에 기반한 다양한 GPU 유형 간의 성능 차이를 보여 줍니다. 이 그래프는 다양한 GPU 유형 및 MPI 버전의 다양한 모델에 걸친 32개 GPU 클러스터의 처리량을 보여 줍니다. 모델은 TensorFlow 1.9에서 구현되었습니다.

![GPU 클러스터의 TensorFlow 모델에 대한 처리량 결과][2]

앞의 표에 나오는 각 VM 시리즈에는 InfiniBand를 사용하는 구성이 포함되어 있습니다. 분산 학습을 실행할 때 노드 간 통신을 더 빠르게 수행하기 위해 InfiniBand 구성을 사용합니다. 또한 InfiniBand는 이를 활용할 수 있는 프레임워크에 대한 학습의 확장 효율성을 높입니다. 자세한 내용은 Infiniband [벤치마크 비교][benchmark]를 참조하세요.

Batch AI는 [blobfuse][blobfuse] 어댑터를 사용하여 Blob Storage를 탑재할 수 있지만, 성능이 필요한 처리량을 처리하는 데 충분하지 않으므로 분산 학습에는 Blob Storage를 이러한 방식으로 사용하지 않는 것이 좋습니다. 아키텍처 다이어그램과 같이 데이터를 Batch AI 파일 서버로 이동합니다.

## <a name="scalability-considerations"></a>확장성 고려 사항

분산 학습의 크기 조정 효율성은 네트워크 오버헤드로 인해 항상 100% 미만이므로 디바이스 간에 전체 모델을 동기화하면 병목 상태가 발생합니다. 따라서 분산 학습은 단일 GPU에서 적절한 일괄 처리 크기를 사용하여 학습할 수 없는 대규모 모델 또는 모델을 간단한 병렬 방식으로 배포하여 해결할 수 없는 문제에 가장 적합합니다.

하이퍼 매개 변수 검색을 실행하는 경우 분산 학습을 사용하지 않는 것이 좋습니다. 확장 효율성은 성능에 영향을 미치고, 분산 방법은 여러 모델 구성을 개별적으로 학습하는 것보다 효율적이지 않습니다.

확장 효율성을 높이는 한 가지 방법은 일괄 처리 크기를 늘리는 것입니다. 그러나 다른 매개 변수를 조정하지 않고 일괄 처리 크기를 늘리면 모델의 최종 성능이 떨어질 수 있으므로 이 작업은 신중하게 수행해야 합니다.

## <a name="storage-considerations"></a>저장소 고려 사항

딥 러닝 모델을 학습할 때 자주 간과되는 측면은 데이터가 저장되는 위치입니다. 스토리지가 너무 느려 GPU의 요구 사항을 충족할 수 없는 경우 학습 성능이 떨어질 수 있습니다.

Batch AI는 많은 스토리지 솔루션을 지원합니다. 이 아키텍처는 사용 편의성과 성능 간에 최상의 균형을 제공하므로 Batch AI 파일 서버를 사용합니다. 최상의 성능을 얻기 위해 데이터를 로컬로 로드합니다. 그러나 모든 노드에서 Blob Storage의 데이터를 다운로드해야 하고 ImageNet 데이터 세트를 사용하는 경우 몇 시간이 걸릴 수 있으므로 이 작업이 번거로울 수 있습니다. [Azure Premium Blob Storage][blob](제한된 공개 미리 보기)는 고려할 수 있는 또 다른 좋은 옵션입니다.

Blob 및 File 스토리지는 분산 학습용 데이터 저장소로 탑재하지 않습니다. 이러한 스토리지를 사용하는 경우 속도가 너무 느리고 학습 성능이 떨어집니다.

## <a name="security-considerations"></a>보안 고려 사항

### <a name="restrict-access-to-azure-blob-storage"></a>Azure Blob Storage에 대한 액세스 제한

이 아키텍처는 [스토리지 계정 키][security-guide]를 사용하여 Blob 스토리지에 액세스합니다. 추가 제어 및 보호를 위해 SAS(공유 액세스 서명)를 대신 사용하는 것을 고려하세요. 이렇게 하면 계정 키를 하드 코딩하거나 일반 텍스트로 저장할 필요 없이 스토리지의 개체에 대해 제한된 액세스 권한을 부여합니다. 또한 SAS를 사용하면 스토리지 계정에 적절한 거버넌스가 있고, 해당 액세스 권한을 의도한 사용자에게만 부여할 수 있습니다.

보다 중요한 데이터가 있는 시나리오의 경우, 이러한 키가 워크로드의 모든 입력 및 출력 데이터에 대해 모든 액세스 권한을 부여하므로 모든 스토리지 키가 보호될 수 있습니다.

### <a name="encrypt-data-at-rest-and-in-motion"></a>저장 데이터 및 이동 중 데이터 암호화

중요한 데이터를 사용하는 시나리오에서는 저장 데이터, 즉 스토리지에 있는 데이터를 암호화합니다. 데이터가 한 위치에서 다음 위치로 이동할 때마다 SSL을 사용하여 데이터 전송을 보호합니다. 자세한 내용은 [Azure Storage 보안 가이드][security-guide]를 참조하세요.

### <a name="secure-data-in-a-virtual-network"></a>가상 네트워크의 데이터 보안

프로덕션 배포의 경우 지정한 가상 네트워크의 서브넷에 Batch AI 클러스터를 배포하는 것이 좋습니다. 이렇게 하면 클러스터의 컴퓨팅 노드에서 다른 가상 머신 또는 온-프레미스 네트워크와 안전하게 통신할 수 있습니다. 또한 Blob 스토리지와 함께 [서비스 엔드포인트][endpoints]를 사용하여 가상 네트워크에서 액세스 권한을 부여하거나 Batch AI를 통해 가상 네트워크 내의 단일 노드 NFS를 사용할 수도 있습니다.

## <a name="monitoring-considerations"></a>모니터링 고려 사항

작업을 실행하는 동안 진행 상황을 모니터링하고 예상대로 작동하는지 확인하는 것이 중요합니다. 그러나 활성 노드의 클러스터를 모니터링할 경우 어려움이 있을 수 있습니다.

Batch AI 파일 서버는 Azure Portal을 통하거나 [Azure CLI][cli] 및 Python SDK를 통해 관리할 수 있습니다. 클러스터의 전반적인 상태를 파악하려면 Azure Portal에서 **Batch AI** 블레이드로 이동하여 클러스터 노드의 상태를 검사합니다. 노드가 비활성 상태이거나 작업이 실패하면 오류 로그가 Blob 스토리지에 저장되며, Azure Portal의 **작업** 아래에서도 액세스할 수 있습니다.

로그를 [Azure Application Insights][ai]에 연결하거나 Batch AI 클러스터 및 해당 작업의 상태를 폴링하는 별도의 프로세스를 실행하여 모니터링을 강화합니다.

Batch AI는 모든 stdout/stderr을 관련 Blob 스토리지 계정에 자동으로 기록합니다. 로그 파일을 검색하는 경우 사용하기 쉬운 환경인 [Azure Storage 탐색기][storage-explorer]와 같은 스토리지 탐색 도구를 사용합니다.

또한 각 작업에 대한 로그를 스트림할 수도 있습니다. 이 옵션에 대한 자세한 내용은 [GitHub][github]의 개발 단계를 참조하세요.

## <a name="deployment"></a>배포

이 참조 아키텍처 구현은 [GitHub][github]에서 제공됩니다. 여기에 설명된 단계에 따라 GPU 사용 VM의 클러스터에서 딥 러닝 모델에 대한 분산 학습을 수행합니다.

## <a name="next-steps"></a>다음 단계

이 아키텍처의 출력은 Blob 스토리지에 저장되도록 학습된 모델입니다. 이 모델은 실시간 채점 또는 일괄 처리 채점에 맞게 조작할 수 있습니다. 자세한 내용은 다음 참조 아키텍처를 참조하세요.

- [Azure의 Python Scikit-Learn 및 딥 러닝 모델의 실시간 채점][real-time-scoring]
- [Azure의 딥 러닝 모델 일괄 처리 채점][batch-scoring]

[0]: ./_images/distributed_dl_architecture.png
[1]: ./_images/distributed_dl_flow.png
[2]: ./_images/distributed_dl_tests.png
[acr]: /azure/container-registry/container-registry-intro
[ai]: /azure/application-insights/app-insights-overview
[aml-compute]: /azure/machine-learning/service/how-to-set-up-training-targets#amlcompute
[amls]: /azure/machine-learning/service/overview-what-is-azure-ml
[azure-blob]: /azure/storage/blobs/storage-blobs-introduction
[batch-ai]: /azure/batch-ai/overview
[batch-ai-files]: /azure/batch-ai/resource-concepts#file-server
[batch-scoring]: /azure/architecture/reference-architectures/ai/batch-scoring-deep-learning
[benchmark]: https://github.com/msalvaris/BatchAIHorovodBenchmark
[blob]: https://azure.microsoft.com/en-gb/blog/introducing-azure-premium-blob-storage-limited-public-preview/
[blobfuse]: https://github.com/Azure/azure-storage-fuse
[cli]: https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md
[docker]: https://hub.docker.com/
[endpoints]: /azure/storage/common/storage-network-security?toc=%2fazure%2fvirtual-network%2ftoc.json#grant-access-from-a-virtual-network
[files]: /azure/storage/files/storage-files-introduction
[github]: https://github.com/Azure/DistributedDeepLearning/
[gpu]: /azure/virtual-machines/windows/sizes-gpu
[horovod]: https://github.com/uber/horovod
[imagenet]: http://www.image-net.org/
[real-time-scoring]: /azure/architecture/reference-architectures/ai/realtime-scoring-python
[resnet]: https://arxiv.org/abs/1512.03385
[security-guide]: /azure/storage/common/storage-security-guide
[storage-explorer]: /azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows
[tutorial]: https://github.com/Azure/DistributedDeepLearning