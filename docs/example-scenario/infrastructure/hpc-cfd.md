---
title: Azure에서 CFD(컴퓨팅 유체 역학) 시뮬레이션 실행
description: Azure에서 CFD(컴퓨팅 유체 역학) 시뮬레이션을 실행합니다.
author: mikewarr
ms.date: 09/20/2018
ms.openlocfilehash: f32e055838d6c62584130f61a0d92b06cc46ec63
ms.sourcegitcommit: 0a31fad9b68d54e2858314ca5fe6cba6c6b95ae4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51610637"
---
# <a name="running-computational-fluid-dynamics-cfd-simulations-on-azure"></a>Azure에서 CFD(컴퓨팅 유체 역학) 시뮬레이션 실행

CFD(계산 유체 역학) 시뮬레이션에는 특수 하드웨어와 함께 상당한 계산 시간이 필요합니다. 클러스터 사용량이 증가하면 시뮬레이션 시간 및 전반적인 그리드 사용량이 증가하여 예비 용량 및 긴 큐 시간 문제로 이어집니다. 물리적 하드웨어를 추가하는 방법은 비용이 많이 들고, 기업에서 발생하는 최대 사용량과 일치하지 않을 수 있습니다. Azure를 활용하면 자본 지출 없이 이러한 문제를 대부분 해결할 수 있습니다.

Azure는 GPU 및 CPU 가상 머신에서 CFD 작업을 실행하는 데 필요한 하드웨어를 제공합니다. RDMA(Remote Direct Memory Access)가 설정된 VM 크기는 짧은 대기 시간으로 MPI(메시지 전달 인터페이스) 통신이 가능한 FDR InfiniBand 기반 네트워킹을 제공합니다. Avere vFXT와 결합하면 엔터프라이즈 규모의 클러스터링 파일 시스템을 제공하므로 고객이 Azure에서 읽기 작업에 최대 처리량을 투입할 수 있습니다.

HPC 클러스터의 생성, 관리 및 최적화를 간소화하기 위해 Azure CycleCloud를 사용하여 하이브리드 및 클라우드 시나리오에서 클러스터를 프로비전하고 데이터를 오케스트레이션할 수 있습니다. CycleCloud는 보류 중인 작업을 모니터링하다가 사용자가 선택한 워크로드 스케줄러에 연결된 주문형 계산을 자동으로 실행하며, 사용만 만큼만 요금을 지불하면 됩니다.

## <a name="relevant-use-cases"></a>관련 사용 사례

CFD 애플리케이션에 대한 기타 관련 산업은 다음과 같습니다.

* 항공학
* 자동차
* HVAC 제작
* 석유 및 가스
* 생명 과학

## <a name="architecture"></a>아키텍처

![아키텍처 다이어그램][architecture]

이 다이어그램은 Azure에서 주문형 노드의 작업 모니터링을 제공하는 일반적인 하이브리드 디자인의 대략적인 개요를 보여줍니다.

1. 클러스터를 구성하려면 Azure CycleCloud 서버에 연결합니다.
2. MPI에 RDMA 지원 머신을 사용하여 클러스터 헤드 노드를 구성하고 만듭니다.
3. 온-프레미스 헤드 노드를 추가하고 구성합니다.
4. 리소스가 부족한 경우 Azure CycleCloud가 Azure에서 계산 리소스를 강화(또는 축소)합니다. 미리 결정된 제한을 정의하여 초과 할당을 방지할 수 있습니다.
5. 작업을 실행 노드에 할당합니다.
6. 온-프레미스 NFS 서버의 데이터를 Azure에 캐시합니다.
7. Azure 캐시용 Avere vFXT에서 데이터를 읽어 옵니다.
8. 작업 및 태스크 정보를 Azure CycleCloud 서버에 릴레이합니다.

### <a name="components"></a>구성 요소

* [Azure CycleCloud][cyclecloud] - Azure에서 HPC 및 큰 계산 클러스터를 만들고 관리하고 운영하고 최적화할 수 있는 도구입니다.
* [Azure 기반의 Avere vFXT][avere] - 클라우드용으로 빌드된 엔터프라이즈 규모 클러스터링 파일 시스템을 제공하는 데 사용됩니다.
* [Azure VM(Virtual Machines)][vms] - 정적 계산 인스턴스 집합을 만드는 데 사용됩니다.
* [Virtual Machine Scale Sets(가상 머신 확장 집합)][vmss] - Azure CycleCloud가 강화 또는 축소할 수 있는 동일한 VM 그룹을 제공합니다.
* [Azure Storage 계정](/azure/storage/common/storage-introduction) - 동기화 및 데이터 보존에 사용됩니다.
* [Virtual Network](/azure/virtual-network/virtual-networks-overview) - Azure VM(Virtual Machines) 같은 다양한 형식의 Azure 리소스가 상호 간 통신, 인터넷 통신 및 온-프레미스 네트워크 통신을 안전하게 수행할 수 있게 해줍니다.

### <a name="alternatives"></a>대안

고객은 Azure CycleCloud를 사용하여 Azure 전체에서 그리드를 만들 수도 있습니다. 이 설치에서 Azure CycleCloud 서버는 Azure 구독 내에서 실행됩니다.

워크로드 스케줄러 관리가 필요 없는 최신 응용 프로그램 방식에서는 [Azure Batch][batch]가 도움이 될 수 있습니다. Azure Batch는 클라우드에서 대규모 병렬 및 HPC(고성능 컴퓨팅) 응용 프로그램을 효율적으로 실행할 수 있습니다. Azure Batch를 사용하여 인프라를 수동으로 구성하거나 관리하지 않고 병렬 또는 대규모로 응용 프로그램을 실행하도록 Azure 계산 리소스를 정의할 수 있습니다. Azure Batch는 요구 사항에 따라 계산 집약적 작업을 예약하고 계산 리소스를 동적으로 추가 및 제거합니다.

### <a name="scalability-and-security"></a>확장성 및 보안

수동으로 또는 자동 크기 조정을 사용하여 Azure CycleCloud의 실행 노드 수를 조정할 수 있습니다. 자세한 내용은 [CycleCloud 자동 크기 조정][cycle-scale]을 참조하세요.

보안 솔루션 설계에 대한 일반적인 지침은 [Azure 보안 설명서][security]를 참조하세요.

## <a name="deploy-this-scenario"></a>시나리오 배포

Azure에 배포하기 전에, 몇 가지 필수 조건이 있습니다. Resource Manager 템플릿을 배포하기 전에 다음 단계를 수행하세요.
1. appId, displayName, 이름, 암호 및 테넌트를 검색하는 데 사용할 [서비스 주체][cycle-svcprin]를 만듭니다.
2. CycleCloud 서버에 안전하게 로그인하기 위한 [SSH 키 쌍][cycle-ssh]을 생성합니다.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FCycleCloudCommunity%2Fcyclecloud_arm%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="https://azuredeploy.net/deploybutton.png"/>
</a>

3. [CycleCloud 서버에 로그인][cycle-login]하여 새 클러스터를 만들고 구성합니다.
4. [클러스터를 만듭니다][cycle-create].

Avere Cache는 응용 프로그램 작업 데이터의 읽기 처리량을 대폭 높일 수 있는 선택적 솔루션입니다. Avere vFXT for Azure는 이러한 엔터프라이즈 HPC 응용 프로그램을 클라우드에서 실행하는 문제를 해결하는 한편, 온-프레미스 또는 Azure Blob 저장소에 저장된 데이터를 활용합니다.

온-프레미스 저장소와 클라우드 계산을 모두 사용하는 하이브리드 인프라를 계획하고 있는 조직의 경우 HPC 응용 프로그램이 NAS 장치에 저장된 데이터를 사용하여 Azure로 “버스트”하고 필요에 따라 가상 CPU를 작동할 수 있습니다. 데이터 집합은 클라우드로 완전히 이동되지 않습니다. 요청된 바이트는 처리 중에 Avere 클러스터를 사용하여 일시적으로 캐시됩니다.

Avere vFXT 설치를 설정하고 구성하려면 [Avere 설치 및 구성 가이드][avere]를 따르세요.

## <a name="pricing"></a>가격

CycleCloud 서버를 사용하여 HPC 구현을 실행하는 비용은 여러 가지 요인에 따라 달라집니다. 예를 들어 CycleCloud는 사용된 계산 시간에 따라 요금이 청구되며, 마스터 및 CycleCloud 서버는 일반적으로 일관적으로 할당 및 실행됩니다. 실행 노드를 실행하는 비용은 실행 시간 및 사용하는 크기에 따라 달라집니다. 또한 저장소 및 네트워킹에 대한 일반 Azure 요금이 적용됩니다.

이 시나리오는 CFD 응용 프로그램을 Azure에서 실행하는 방법을 보여주며, 이렇게 하려면 특정 VM 크기에서만 사용할 수 있는 RDMA 기능이 머신에 필요합니다. 다음은 확장 집합을 1개월 동안 하루에 8시간 연속으로 할당하고 데이터 1TB를 송신할 때 발생할 수 있는 비용 예제입니다. Azure CycleCloud 서버 및 Avere vFXT for Azure 설치에 대한 가격 책정도 포함되어 있습니다.

* 지역: 북유럽
* Azure CycleCloud 서버: 1 x 표준 D3(4 x CPU, 14GB 메모리, 표준 HDD 32GB)
* Azure CycleCloud 마스터 서버: 1 x 표준 D12 v(4 x CPU, 28GB 메모리, 표준 HDD 32GB)
* Azure CycleCloud 노드 배열: 10 x 표준 H16r(16 x CPU, 112GB 메모리)
* Azure 클러스터 기반의 Avere vFXT: 3 x D16s v3(200GB OS, 프리미엄 SSD 1TB 데이터 디스크)
* 데이터 송신: 1TB

위에 나열된 하드웨어는 이 [예상 가격][pricing]을 검토하세요.

## <a name="next-steps"></a>다음 단계

샘플을 배포한 후에는 [Azure CycleCloud][cyclecloud]에 대해 자세히 알아보세요.

## <a name="related-resources"></a>관련 리소스

* [RDMA 지원 머신 인스턴스][rdma]
* [RDMA 인스턴스 VM 사용자 지정][rdma-custom]

<!-- links -->
[architecture]: ./media/architecture-hpc-cfd.png
[calculator]: https://azure.com/e/
[availability]: /azure/architecture/checklist/availability
[resource-groups]: /azure/azure-resource-manager/resource-group-overview
[resiliency]: /azure/architecture/resiliency/
[security]: /azure/security/
[scalability]: /azure/architecture/checklist/scalability
[vmss]: /azure/virtual-machine-scale-sets/overview
[cyclecloud]: /azure/cyclecloud/
[rdma]: /azure/virtual-machines/windows/sizes-hpc#rdma-capable-instances
[gpu]: /azure/virtual-machines/windows/sizes-gpu
[hpcsizes]: /azure/virtual-machines/windows/sizes-hpc
[vms]: /azure/virtual-machines/
[low-pri]: /azure/virtual-machine-scale-sets/virtual-machine-scale-sets-use-low-priority
[batch]: /azure/batch/
[avere]: https://github.com/Azure/Avere/blob/master/README.md
[cycle-prereq]: /azure/cyclecloud/quickstart-install-cyclecloud#prerequisites
[cycle-svcprin]: /azure/cyclecloud/quickstart-install-cyclecloud#service-principal
[cycle-ssh]: /azure/cyclecloud/quickstart-install-cyclecloud#ssh-keypair
[cycle-login]: /azure/cyclecloud/quickstart-install-cyclecloud#log-into-the-cyclecloud-application-server
[cycle-create]: /azure/cyclecloud/quickstart-create-and-run-cluster
[rdma]: /azure/virtual-machines/windows/sizes-hpc#rdma-capable-instances
[rdma-custom]: /azure/virtual-machines/linux/classic/rdma-cluster#customize-the-vm
[pricing]: https://azure.com/e/53030a04a2ab47a289156e2377a4247a
[cycle-scale]: /azure/cyclecloud/autoscale
