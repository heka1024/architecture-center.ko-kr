---
title: R 기계 학습 모델의 실시간 채점
description: AKS(Azure Kubernetes Service)에서 실행되는 Machine Learning Server를 사용하여 R에서 실시간 예측 서비스를 구현합니다.
author: njray
ms.date: 12/12/2018
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: azcat-ai
ms.openlocfilehash: 5f3cc62c81c9ef9e5c3c27b1d66badd3e481c228
ms.sourcegitcommit: 1a3cc91530d56731029ea091db1f15d41ac056af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58887848"
---
# <a name="real-time-scoring-of-r-machine-learning-models-on-azure"></a>Azure 기반 R 기계 학습 모델의 실시간 채점

이 참조 아키텍처는 AKS(Azure Kubernetes Service)에서 실행되는 Microsoft Machine Learning Server를 사용하여 R에서 실시간(동기) 예측 서비스를 구현하는 방법을 보여줍니다. 이 아키텍처는 실시간 서비스로 배포할 목적으로 R에서 빌드한 예측 모델에 일반적으로 사용할 수 있도록 제작되었습니다. **[이 솔루션을 배포하세요][github]**.

## <a name="architecture"></a>아키텍처

![Azure 기반 R 기계 학습 모델의 실시간 채점][0]

이 참조 아키텍처는 컨테이너 기반 접근 방식을 사용합니다. R 및 새 데이터를 채점하는 데 필요한 다양한 아티팩트를 포함하는 Docker 이미지가 빌드됩니다. 여기에는 모델 개체 자체와 채점 스크립트가 포함됩니다. 이 이미지는 Azure에 호스트되는 Docker 레지스트리로 푸시된 다음, Azure에서도 Kubernetes 클러스터에 배포됩니다.

이 워크플로의 아키텍처는 다음 구성 요소를 포함합니다.

- **[Azure Container Registry][acr]** 는 이 워크플로의 이미지를 저장하는 데 사용됩니다. Container Registry를 사용하여 만든 레지스트리는 [Docker 레지스트리 V2 API][docker] 및 클라이언트를 통해 관리할 수 있습니다.

- **[Azure Kubernetes Service][aks]** 는 배포 및 서비스를 호스트하는 데 사용됩니다. AKS를 사용하여 만든 클러스터는 표준 [Kubernetes API][k-api] 및 클라이언트(kubectl)를 사용하여 관리할 수 있습니다.

- **[Microsoft Machine Learning Server][mmls]** 는 서비스용 REST API를 정의하는 데 사용되며 [모델 운영화][operationalization]를 포함합니다. 이 서비스 지향 웹 서버 프로세스는 요청을 수신 대기하며, 결과를 생성하는 실제 R 코드를 실행하는 다른 백그라운드 프로세스에 전달됩니다. 이 모든 프로세스는 이 구성의 단일 노드에서 실행되고, 컨테이너에 래핑됩니다. 개발 또는 테스트 환경 외부에서 이 서비스를 사용하는 방법에 대한 자세한 내용은 Microsoft 담당자에게 문의하세요.

## <a name="performance-considerations"></a>성능 고려 사항

기계 학습 워크로드는 새 데이터를 학습하고 채점할 때 컴퓨팅 집약적인 경향이 있습니다. 경험에 비춰보면, 채점 프로세스를 코어당 하나만 실행하는 것이 좋습니다. Machine Learning Server를 통해 각 컨테이너에서 실행되는 R 프로세스의 수를 정의할 수 있습니다. 기본값은 프로세스 5개입니다. 변수가 많지 않은 선형 회귀처럼 비교적 간단한 모델 또는 소형 의사 결정 트리를 만들 때에는 프로세스 수를 늘려도 됩니다. 클러스터 노드의 CPU 로드를 모니터링하여 컨테이너 수를 적절하게 제한할 수 있습니다.

GPU 지원 클러스터는 일부 워크로드 유형, 특히 딥러닝 모델의 속도를 높일 수 있습니다. 모든 워크로드가 GPU를 활용할 수 있는 것은 아니고, 행렬 대수를 많이 사용하는 워크로드만 활용할 수 있습니다. 예를 들어 임의 포리스트 및 부스팅 모델을 비롯한 트리 기반 모델은 일반적으로 GPU에서 아무런 이점도 얻을 수 없습니다.

임의 포리스트 같은 일부 모델 유형은 CPU에서 대규모 병렬 처리가 가능합니다. 이 경우 워크로드를 여러 코어에 분산하여 단일 요청 채점 속도를 높일 수 있습니다. 그러나 이렇게 하면 클러스터 크기가 고정되므로 여러 채점 요청을 처리하는 용량이 감소합니다.

일반적으로 오픈 소스 R 모델은 모든 데이터를 메모리에 저장하므로 노드에서 동시에 실행하려는 프로세스를 처리하기에 충분한 메모리가 확보됩니다. Machine Learning Server를 사용하여 모델을 적절하게 조정하려면 모든 데이터를 메모리로 읽어 들이지 않고 디스크에서 데이터를 처리할 수 있는 라이브러리를 사용하세요. 이렇게 하면 메모리 요구 사항을 대폭 줄일 수 있습니다. Machine Learning Server를 사용하든 오픈 소스 R을 사용하든, 채점 프로세스 때문에 메모리가 고갈되는 일이 없도록 노드를 모니터링해야 합니다.

## <a name="security-considerations"></a>보안 고려 사항

### <a name="network-encryption"></a>네트워크 암호화

이 참조 아키텍처에서는 클러스터와의 통신에 HTTPS를 사용하며, [Let’s Encrypt][encrypt]의 준비 인증서가 사용됩니다. 프로덕션 환경에서는 적절한 서명 기관의 인증서로 바꾸면 됩니다.

### <a name="authentication-and-authorization"></a>인증 및 권한 부여

Machine Learning Server [모델 운영화][operationalization]를 사용하려면 채점 요청을 인증해야 합니다. 이 배포에는 사용자 이름과 암호가 사용됩니다. 엔터프라이즈 설정에서 [Azure Active Directory][AAD]를 사용하여 인증을 지원할 수도 있고 [Azure API Management][API]를 사용하여 별도의 프런트 엔드를 만들 수도 있습니다.

컨테이너에서 모델 운영화가 Machine Learning Server와 함께 제대로 작동하려면 JWT(JSON Web Token) 인증서를 설치해야 합니다. 이 배포에서는 Microsoft가 제공하는 인증서를 사용합니다. 프로덕션 설정에서는 사용자 고유의 인증서를 제공하면 됩니다.

Container Registry와 AKS 간 트래픽에 대해서는 [RBAC(역할 기반 액세스 제어)][rbac]를 사용하여 꼭 필요한 액세스 권한만 허용하도록 제한하는 방안을 고려해야 합니다.

### <a name="separate-storage"></a>별도의 스토리지

이 참조 아키텍처는 애플리케이션(R) 및 데이터(모델 개체 및 채점 스크립트)를 단일 이미지에 번들로 구성합니다. 분리하는 것이 유리한 경우도 있습니다. 모델 데이터와 코드를 Azure BLOB 또는 파일 [스토리지][storage]에 배치하고, 컨테이너 초기화 시 검색할 수 있습니다. 이 경우 인증된 액세스만 허용하고 HTTPS를 요구하도록 스토리지 계정을 설정해야 합니다.

## <a name="monitoring-and-logging-considerations"></a>모니터링 및 로깅 고려 사항

[Kubernetes 대시보드][dashboard]를 사용하여 AKS 클러스터의 전반적인 상태를 모니터링할 수 있습니다. 자세한 내용은 Azure Portal에서 클러스터의 개요 블레이드를 참조하세요. [GitHub][github] 리소스 역시 R에서 대시보드를 불러오는 방법을 보여줍니다

대시보드가 클러스터의 전반적인 상태를 보여주지만, 개별 컨테이너의 상태를 추적하는 것도 중요합니다. 개별 컨테이너의 상태를 추적하려면 Azure Portal의 클러스터 개요 블레이드에서 [Azure Monitor Insights][monitor]를 설정하거나 [컨테이너용 Azure Monitor][monitor-containers](미리 보기)를 참조하세요.

## <a name="cost-considerations"></a>비용 고려 사항

Machine Learning Server는 코어 단위로 라이선스가 제공되며, Machine Learning Server를 실행하는 클러스터의 모든 코어가 집계됩니다. Machine Learning Server 또는 Microsoft SQL Server 고객인 경우 Microsoft 담당자에게 자세한 가격 책정 정보를 문의하세요.

Machine Learning Server를 대체하는 오픈 소스 [Plumber][plumber]는 코드를 REST API로 변환하는 R 패키지입니다. Plumber는 Machine Learning Server보다 적은 기능을 제공합니다. 예를 들어 요청 인증을 제공하는 기능이 기본적으로 포함되지 않습니다. Plumber를 사용하는 인증 세부 정보를 처리할 수 있도록 [Azure API Management][API]를 사용하는 것이 좋습니다.

라이선스 외에도 주요 비용 요인으로 Kubernetes 클러스터의 계산 리소스를 고려해야 합니다. 클러스터가 피크 시간대의 예상 요청 볼륨을 처리할 수 있도록 충분히 커야 하지만, 이 방법은 그 외의 시간에는 리소스가 유휴 상태로 남게 됩니다. 유휴 리소스의 영향을 최소화하려면 kubectl 도구를 사용하여 클러스터에 [수평 자동 크기 조정기][autoscaler]를 사용하도록 설정합니다. 또는 AKS [클러스터 자동 크기 조정기][cluster-autoscaler]를 사용합니다.

## <a name="deploy-the-solution"></a>솔루션 배포

이 참조 아키텍처 구현은 [GitHub][github]에서 제공됩니다. GitHub에 설명된 단계에 따라 간단한 예측 모델을 서비스로 배포해 보세요.

<!-- links -->
[AAD]: /azure/active-directory/fundamentals/active-directory-whatis
[API]: /azure/api-management/api-management-key-concepts
[ACR]: /azure/container-registry/container-registry-intro
[AKS]: /azure/aks/intro-kubernetes
[autoscaler]: https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/
[cluster-autoscaler]: /azure/aks/autoscaler
[monitor]: /azure/monitoring/monitoring-container-insights-overview
[dashboard]: /azure/aks/kubernetes-dashboard
[docker]: https://docs.docker.com/registry/spec/api/
[encrypt]: https://letsencrypt.org/
[gitHub]: https://github.com/Azure/RealtimeRDeployment
[K-API]: https://kubernetes.io/docs/reference/
[MMLS]: /machine-learning-server/what-is-machine-learning-server
[monitor-containers]: /azure/azure-monitor/insights/container-insights-overview
[operationalization]: /machine-learning-server/what-is-operationalization
[plumber]: https://www.rplumber.io
[RBAC]: /azure/role-based-access-control/overview
[storage]: /azure/storage/common/storage-introduction
[0]: ./_images/realtime-scoring-r.png
