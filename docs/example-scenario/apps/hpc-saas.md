---
title: Azure의 컴퓨터 지원 엔지니어링 서비스
description: Azure에서 CAE(컴퓨터 지원 엔지니어링)에 대한 SaaS(Software-as-a-Service) 플랫폼을 제공합니다.
author: alexbuckgit
ms.date: 08/22/2018
ms.openlocfilehash: 8bdf7198223f7194d0cd717949699bb3a508674e
ms.sourcegitcommit: 0a31fad9b68d54e2858314ca5fe6cba6c6b95ae4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51610552"
---
# <a name="a-computer-aided-engineering-service-on-azure"></a>Azure의 컴퓨터 지원 엔지니어링 서비스

이 예제 시나리오는 Azure의 HPC(고성능 컴퓨팅) 기능을 기반으로 SaaS(Software-as-a-Service) 플랫폼을 제공하는 방법을 보여줍니다. 이 시나리오는 엔지니어링 소프트웨어 솔루션을 기반으로 합니다. 그러나 아키텍처는 이미지 렌더링, 복잡한 모델링, 금융 위험 계산처럼 HPC 리소스가 필요한 기타 산업과 관련이 있습니다.

이 예제에서는 엔지니어링 회사 및 제조 기업에 CAE(Computer-Aided Engineering) 응용 프로그램을 제공하는 엔지니어링 소프트웨어 공급자를 보여줍니다. CAE 솔루션은 혁신을 가능하게 하고, 개발 시간을 줄이고, 제품의 전체 디자인 수명에서 비용을 절감합니다. 이러한 솔루션은 상당한 계산 리소스를 요구하며 대량의 데이터를 자주 처리합니다. 온-프레미스 HPC 어플라이언스 또는 고성능 워크스테이션은 비용이 많이 들기 때문에 소규모 엔지니어링 회사, 기업가 및 학생이 사용하기에는 부담이 큽니다.

이 회사는 클라우드 기반 HPC 기술이 지원되는 SaaS 플랫폼을 빌드하여 응용 프로그램 시장을 확장하기를 원합니다. 고객은 필요한 만큼만 계산 리소스 요금을 지불할 수 있어야 하며, 그렇지 않으면 비용이 매우 많이 드는 대규모 계산 성능을 이용해야 합니다.

이 회사의 목표는 다음과 같습니다.

* Azure에서 HPC 기능을 활용하여 제품 디자인 및 테스트 프로세스를 가속화합니다.
* 혁신적인 최신 하드웨어를 사용하여 복잡한 시뮬레이션을 실행하는 한편, 간단한 시뮬레이션 비용을 최소화합니다.
* 고성능 엔지니어링 워크스테이션 없이 사실적인 시각화를 사용하고 웹 브라우저에 렌더링합니다.

## <a name="relevant-use-cases"></a>관련 사용 사례

관련된 다른 사용 사례는 다음과 같습니다.

* 유전체학 연구
* 날씨 시뮬레이션
* 컴퓨터 화학 응용 프로그램

## <a name="architecture"></a>아키텍처

![HPC 기능을 지원하는 SaaS 솔루션용 아키텍처][architecture]

* 사용자는 [Apache Guacamole 서비스](https://guacamole.apache.org/)를 사용하는 HTML5 기반 RDP 연결을 통해 브라우저에서 NV 시리즈 VM(가상 머신)에 액세스할 수 있습니다. 이러한 VM 인스턴스는 렌더링 및 공동 작업을 위한 강력한 GPU를 제공합니다. 사용자는 고성능 모바일 컴퓨팅 디바이스 또는 랩톱을 사용하지 않고도 디자인을 편집하고 결과를 볼 수 있습니다. 스케줄러는 사용자가 정의한 추론을 기반으로 추가 VM을 작동합니다.
* 데스크톱 CAD 세션에서, 사용자는 사용 가능한 HPC 클러스터 노드에서 실행할 워크로드를 제출할 수 있습니다. 이러한 워크로드는 스트레스 분석 또는 컴퓨팅 유체 역학 계산 같은 작업을 수행하므로 전용 온-프레미스 계산 클러스터가 필요 없습니다. 계산 리소스에 대한 활성 사용자 요구에 따라 부하 또는 큐 깊이를 기준으로 규모를 자동으로 조정하도록 클러스터 노드를 구성할 수 있습니다.
* AKS(Azure Kubernetes Service)는 최종 사용자에게 제공되는 웹 리소스를 호스트하는 데 사용됩니다.

### <a name="components"></a>구성 요소

* [H 시리즈 가상 머신](/azure/virtual-machines/linux/sizes-hpc)은 분자 모델링 및 컴퓨팅 유체 역학 같은 계산 집약적 시뮬레이션을 실행하는 데 사용됩니다. 또한 이 솔루션은 RDMA(원격 직접 메모리 액세스) 연결 및 InfiniBand 네트워킹 같은 기술을 활용합니다.
* [NV 시리즈 가상 머신](/azure/virtual-machines/windows/sizes-gpu)은 표준 웹 브라우저에서 엔지니어에게 고성능 워크스테이션 기능을 제공합니다. 이러한 가상 머신은 고급 렌더링을 지원하고 단정밀도 워크로드를 실행할 수 있는 NVIDIA Tesla M60 GPU를 사용합니다.
* CentOS를 실행하는 [범용 가상 머신](/azure/virtual-machines/linux/sizes-general)은 웹 응용 프로그램처럼 보다 전통적인 워크로드를 처리합니다.
* [Application Gateway](/azure/application-gateway/overview)는 웹 서버로 들어오는 요청을 부하 분산합니다.
* [AKS(Azure Kubernetes Service)](/azure/aks/intro-kubernetes)는 HPC 또는 GPU 가상 머신의 고성능 기능이 필요 없는 시뮬레이션에 확장 가능한 워크로드를 저렴하게 실행하는 데 사용됩니다.
* [Altair PBS Works Suite](https://www.pbsworks.com/PBSProduct.aspx?n=PBS-Works-Suite&c=Overview-and-Capabilities)는 HPC 워크플로를 오케스트레이션하여 현재 부하를 처리하기에 충분한 가상 머신 인스턴스를 제공합니다. 또한 수요가 적으면 가상 머신을 할당 취소하여 비용을 줄입니다.
* [Blob 저장소](/azure/storage/blobs/storage-blobs-introduction)는 예약된 작업을 지원하는 파일을 저장합니다. 

### <a name="alternatives"></a>대안

* [Azure CycleCloud](/azure/cyclecloud/overview)는 HPC 클러스터 만들기, 관리, 운영 및 최적화를 간소화합니다. 고급 정책 및 거버넌스 기능을 제공합니다. CycleCloud는 모든 작업 스케줄러 또는 소프트웨어 스택을 지원합니다.
* [HPC Pack](/azure/virtual-machines/windows/hpcpack-cluster-options)은 Windows Server 기반 워크로드용 Azure HPC 클러스터를 만들고 관리할 수 있습니다. HPC 팩은 Linux 기반 워크로드에 사용 가능한 옵션이 아닙니다.
* [Azure Automation 상태 구성](/azure/automation/automation-dsc-overview)은 배포할 가상 머신과 소프트웨어를 정의하는 코드로써의 인프라 방식을 제공합니다. 작업 큐에 제출된 작업 수에 따라 계산 노드를 자동으로 조정하는 규칙을 사용하여 가상 머신 확장 집합의 일부로 가상 머신을 배포할 수 있습니다. 새 가상 머신이 필요한 경우 Azure 이미지 갤러리에서 최신 패치가 적용된 이미지를 사용하여 새 가상 머신을 프로비전한 다음, PowerShell DSC 구성 스크립트를 통해 필수 소프트웨어를 설치 및 구성합니다.
* [Azure 기능](/azure/azure-functions/functions-overview)

## <a name="considerations"></a>고려 사항

* 코드로써의 인프라 방식은 가상 머신 빌드 정의를 관리하는 좋은 방법이지만, 스크립트를 사용하여 새 가상 머신을 프로비전하는 시간이 오래 걸릴 수 있습니다. 이 솔루션은 골든 이미지를 주기적으로 만드는 DSC 스크립트를 사용하여 적절한 절충안을 찾아냈습니다. 이렇게 만든 이미지는 요청이 있을 시 VM을 완전히 새로 빌드하는 것보다 DSC를 사용하여 보다 빠르게 새 가상 머신을 프로비전할 수 있습니다. Azure DevOps Services 또는 다른 CI/CD 도구는 DSC 스크립트를 사용하여 주기적으로 골든 이미지를 새로 고칠 수 있습니다.
* 계산 리소스의 신속한 가용성을 통해 전체 솔루션 비용의 적절한 균형을 유지하는 것이 주요 고려 사항입니다. N 시리즈 가상 머신 인스턴스 풀을 프로비전하고 할당 취소하면 운영 비용이 절감됩니다. 추가 가상 머신이 필요한 경우 기존 인스턴스를 다시 할당하면 다른 호스트에서 가상 머신을 작동하는 과정이 포함되지만, OS가 GPU 드라이버를 식별하고 설치하는 데 필요한 PCI 버스 감지 시간이 제거됩니다. 프로비전 해제된 후 다시 프로비전되는 가상 머신은 다시 시작될 때 GPU에 대해 동일한 PCI 버스를 유지하기 때문입니다.
* 원본 아키텍처는 시뮬레이션을 실행하기 위해 전적으로 Azure 가상 머신에 의존합니다. 가상 머신의 일부 기능이 필요 없는 워크로드 비용을 줄이기 위해, 이러한 워크로드가 컨테이너화되어 AKS(Azure Kubernetes Service)에 배포되었습니다.
* 이 회사의 직원은 오픈 소스 기술에 대한 기존 기술을 갖고 있습니다. 직원은 Linux 및 Kubernetes 같은 기술을 토대로 이러한 기술을 활용할 수 있습니다. 

## <a name="pricing"></a>가격

이 시나리오를 실행하는 데 들어가는 비용을 살펴볼 수 있도록, 여러 필수 서비스가 [비용 계산기 예제][calculator]에 미리 구성되어 있습니다. 솔루션 비용은 요구 사항을 충족하는 데 필요한 서비스의 크기와 수에 따라 달라집니다.

다음 고려 사항은 이 솔루션의 비용 중 상당 부분을 차지합니다.
* Azure 가상 머신 비용은 추가 인스턴스가 프로비전될 때마다 선형으로 증가합니다. 할당 취소된 가상 머신은 저장소 비용만 발생시키고 계산 비용은 발생시키지 않습니다. 이처럼 할당 취소된 머신은 수요가 증가할 때 다시 할당할 수 있습니다.
* Azure Kubernetes Services 비용은 워크로드를 지원하기 위해 선택한 VM 유형에 따라 결정됩니다. 비용은 클러스터의 VM 수에 따라 선형적으로 증가합니다.

## <a name="next-steps"></a>다음 단계

* [Altair 고객 사례][source-document]를 읽어봅니다. 이 예제 시나리오는 이 고객의 아키텍처 버전을 기반으로 합니다.
* Azure에 제공되는 다른 [큰 계산 솔루션](https://azure.microsoft.com/solutions/big-compute)을 검토합니다.

<!-- links -->
[architecture]: ./media/architecture-hpc-saas.png
[source-document]: https://customers.microsoft.com/story/altair-manufacturing-azure
[calculator]: https://azure.com/e/3cb9ccdc893f41ffbcdb00c328178ccf
