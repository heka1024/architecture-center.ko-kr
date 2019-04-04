---
title: Azure의 HPC(고성능 컴퓨팅)
description: Azure에서 실행되는 HPC 워크로드를 빌드하는 방법에 대한 가이드
author: adamboeglin
ms.date: 2/4/2019
ms.openlocfilehash: 5263dd3a06e5244bf804df4be6ec57d789574f76
ms.sourcegitcommit: ea97ac004c38c6b456794c1a8eef29f8d2b77d50
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58489201"
---
<!-- markdownlint-disable MD033 -->
<!-- markdownlint-disable MD026 -->

# <a name="high-performance-computing-hpc-on-azure"></a>Azure의 HPC(고성능 컴퓨팅)

## <a name="introduction-to-hpc"></a>HPC 소개

<!-- markdownlint-disable MD034 -->

> [!VIDEO https://www.youtube.com/embed/rKURT32faJk]

<!-- markdownlint-enable MD034 -->

"빅 컴퓨팅"이라고도 하는 HPC(고성능 컴퓨팅)는 복잡한 수학 작업을 해결하기 위해 수많은 CPU 또는 GPU 기반 컴퓨터를 사용합니다.

많은 산업에서 업계의 가장 어려운 문제를 해결하기 위해 HPC를 사용합니다.  여기에는 다음과 같은 워크로드가 포함됩니다.

- Genomics
- 석유 및 가스 시뮬레이션
- 재무
- 반도체 디자인
- 공학
- 날씨 모델링

### <a name="how-is-hpc-different-on-the-cloud"></a>클라우드의 HPC와 어떤 차이가 있습니까?

온-프레미스 HPC 시스템과 클라우드의 시스템의 주요 차이점 중 하나는 필요할 때 리소스를 동적으로 추가하고 제거하는 기능입니다.  동적 크기 조정은 병목 현상이 발생하는 컴퓨팅 용량을 제거하고, 그 대신 고객이 작업의 요구 사항에 맞게 인프라 크기를 적절하게 조정할 수 있게 해줍니다.

다음 문서는 이 동적 크기 조정 기능에 대한 자세한 정보를 제공합니다.

- [빅 컴퓨팅 아키텍처 스타일](/azure/architecture/guide/architecture-styles/big-compute?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [자동 크기 조정 모범 사례](/azure/architecture/best-practices/auto-scaling?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)

## <a name="implementation-checklist"></a>구현 검사 목록

Azure에 자체 HPC 솔루션을 구현하고자 할 때 다음 토픽을 꼭 검토하세요.

<!-- markdownlint-disable MD032 -->

> [!div class="checklist"]
> - 요구 사항에 따라 적절한 [아키텍처](#infrastructure) 선택
> - 워크로드에 적합한 [컴퓨팅](#compute) 옵션 알아보기
> - 요구 사항을 충족하는 적절한 [스토리지](#storage) 솔루션 식별
> - 모든 리소스를 어떻게 [관리](#management)할 것인지 결정
> - 클라우드용 [애플리케이션](#hpc-applications) 최적화
> - 인프라 [보안](#security)

<!-- markdownlint-enable MD032 -->

## <a name="infrastructure"></a>인프라

HPC 시스템을 빌드하는 데 필요한 여러 인프라 구성 요소가 있습니다.  HPC 워크로드를 관리하는 방법으로 무엇을 선택하든 컴퓨팅, 스토리지 및 네트워킹은 기본 구성 요소를 제공합니다.

### <a name="example-hpc-architectures"></a>HPC 아키텍처 예제

Azure에서 HPC 아키텍처를 디자인하고 구현하는 다양한 방법이 있습니다.  HPC 애플리케이션은 컴퓨팅 코어 수천 개로 확장하거나, 온-프레미스 클러스터로 확장하거나, 100% 클라우드 네이티브 솔루션으로 실행할 수 있습니다.

다음 시나리오는 HPC 솔루션을 빌드하는 일반적인 방법 중 몇 가지를 간략하게 설명합니다.

<ul class="columns is-multiline has-margin-left-none has-margin-bottom-none has-padding-top-medium">
    <li class="column is-one-third has-padding-top-small-mobile has-padding-bottom-small">
        <a class="is-undecorated is-full-height is-block"
            href="/azure/architecture/example-scenario/apps/hpc-saas?context=/azure/architecture/topics/high-performance-computing/context/hpc-context">
            <article class="card has-outline-hover is-relative is-fullheight">
                    <figure class="image has-margin-right-none has-margin-left-none has-margin-top-none has-margin-bottom-none">
                        <img role="presentation" alt="" src="../../example-scenario/apps/media/architecture-hpc-saas.png">
                    </figure>
                <div class="card-content has-text-overflow-ellipsis">
                    <div class="has-padding-bottom-none">
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary">Azure의 컴퓨터 지원 엔지니어링 서비스</h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p>Azure에서 CAE(컴퓨터 지원 엔지니어링)에 대한 SaaS(Software-as-a-Service) 플랫폼을 제공합니다.</p>
                    </div>
                </div>
            </article>
        </a>
    </li>
    <li class="column is-one-third has-padding-top-small-mobile has-padding-bottom-small">
        <a class="is-undecorated is-full-height is-block"
            href="/azure/architecture/example-scenario/infrastructure/hpc-cfd?context=/azure/architecture/topics/high-performance-computing/context/hpc-context">
            <article class="card has-outline-hover is-relative is-fullheight">
                    <figure class="image has-margin-right-none has-margin-left-none has-margin-top-none has-margin-bottom-none">
                        <img role="presentation" alt="" src="../../example-scenario/infrastructure/media/architecture-hpc-cfd.png">
                    </figure>
                <div class="card-content has-text-overflow-ellipsis">
                    <div class="has-padding-bottom-none">
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary">Azure의 CFD(컴퓨팅 유체 역학) 시뮬레이션</h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p>Azure에서 CFD(컴퓨팅 유체 역학) 시뮬레이션을 실행합니다.</p>
                    </div>
                </div>
            </article>
        </a>
    </li>
    <li class="column is-one-third has-padding-top-small-mobile has-padding-bottom-small">
        <a class="is-undecorated is-full-height is-block"
            href="/azure/architecture/example-scenario/infrastructure/video-rendering?context=/azure/architecture/topics/high-performance-computing/context/hpc-context">
            <article class="card has-outline-hover is-relative is-fullheight">
                    <figure class="image has-margin-right-none has-margin-left-none has-margin-top-none has-margin-bottom-none">
                        <img role="presentation" alt="" src="../../example-scenario/infrastructure/media/architecture-video-rendering.png">
                    </figure>
                <div class="card-content has-text-overflow-ellipsis">
                    <div class="has-padding-bottom-none">
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary">Azure의 3D 비디오 렌더링</h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p>Azure Batch 서비스를 사용하여 Azure에서 네이티브 HPC 워크로드를 실행합니다.</p>
                    </div>
                </div>
            </article>
        </a>
    </li>
</ul>

### <a name="compute"></a>컴퓨팅

Azure는 CPU 및 GPU 사용량이 많은 워크로드에 최적화된 크기 범위를 제공합니다.

#### <a name="cpu-based-virtual-machines"></a>CPU 기반 가상 머신

- [Linux VM](/azure/virtual-machines/linux/sizes-hpc?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [Windows VM의](/azure/virtual-machines/windows/sizes-hpc?context=/azure/architecture/topics/high-performance-computing/context/hpc-context) VM
  
#### <a name="gpu-enabled-virtual-machines"></a>GPU 사용 가상 머신

N 시리즈 VM은 AI(인공 지능) 학습 및 시각화를 포함한 계산 집약적 또는 그래픽 집약적 애플리케이션을 위해 설계된 NVIDIA GPU를 특징으로 합니다.

- [Linux VM](/azure/virtual-machines/linux/sizes-gpu?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [Windows VM](/azure/virtual-machines/windows/sizes-gpu?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)

### <a name="storage"></a>Storage

대규모 Batch 및 HPC 작업에는 기존 클라우드 파일 시스템의 용량을 초과하는 데이터 저장 및 액세스에 대한 요구 사항이 있습니다.  Azure 기반 HPC 애플리케이션의 속도 및 용량 요구 사항을 관리하는 여러 가지 솔루션이 있습니다.

- [Avere vFXT](https://azure.microsoft.com/services/storage/avere-vfxt/) - 에지의 고성능 컴퓨팅을 위한 더 빠르고 더 쉽게 액세스할 수 있는 데이터 스토리지
- [BeeGFS](https://azure.microsoft.com/resources/implement-glusterfs-on-azure/)
- [스토리지 최적화 가상 머신](/azure/virtual-machines/windows/sizes-storage?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [Blob, 테이블 및 Queue Storage](/azure/storage/storage-introduction?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [Azure SMB 파일 스토리지](/azure/storage/files/storage-files-introduction?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [Intel Cloud Edition Lustre](https://azuremarketplace.microsoft.com/marketplace/apps/intel.intel-cloud-edition-gs)

Azure 기반의 Lustre, GlusterFS 및 BeeGFS를 자세히 비교하는 내용은 [Azure eBook의 병렬 파일 시스템](https://blogs.msdn.microsoft.com/azurecat/2018/06/11/azurecat-ebook-parallel-virtual-file-systems-on-microsoft-azure/)을 검토하세요.

### <a name="networking"></a>네트워킹

H16r, H16mr, A8 및 A9 VM은 높은 처리량 백 엔드 RDMA 네트워크에 연결할 수 있습니다. 이 네트워크는 Microsoft MPI 또는 Intel MPI에서 실행되는 긴밀하게 결합된 병렬 애플리케이션의 성능을 개선할 수 있습니다.

- [RDMA 지원 인스턴스](/azure/virtual-machines/windows/sizes-hpc?context=/azure/architecture/topics/high-performance-computing/context/hpc-context#rdma-capable-instances)
- [Virtual Network](/azure/virtual-network/virtual-networks-overview?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [ExpressRoute](/azure/expressroute/expressroute-introduction?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)

## <a name="management"></a>관리

### <a name="do-it-yourself"></a>DIY(Do It Yourself)

Azure에서 HPC 시스템을 처음부터 새로 빌드하면 유연성의 측면에서 상당한 장점이 있지만, 종종 유지 관리 업무가 매우 많아지는 단점이 있습니다.  

1. Azure 가상 머신 또는 [가상 머신 확장 집합](/azure/virtual-machine-scale-sets/overview?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)에서 고유한 클러스터 환경을 설정합니다.
2. Azure Resource Manager 템플릿을 사용하여 선행 [워크로드 관리자](#workload-managers), 인프라 및 [애플리케이션](#hpc-applications)을 배포합니다.
3. MPI 또는 GPU 워크로드에 대한 특수한 하드웨어 및 네트워크 연결을 포함하는 HPC 및 GPU [VM 크기](#compute)를 선택합니다.
4. I/O 사용량이 많은 워크로드에 [고성능 저장소](#storage)를 추가합니다.

### <a name="hybrid-and-cloud-bursting"></a>하이브리드 및 클라우드 버스팅

기존 온-프레미스 HPC 시스템을 Azure에 연결하고 싶을 때 작업에 바로 사용할 수 있는 다양한 리소스가 있습니다.

먼저, 설명서의 [온-프레미스 네트워크를 Azure에 연결하는 옵션](/azure/architecture/reference-architectures/hybrid-networking/?context=/azure/architecture/topics/high-performance-computing/context/hpc-context) 문서를 검토합니다.  그 과정에서 다음과 같은 연결 옵션에 대한 정보가 필요할 수 있습니다.

<ul class="columns is-multiline has-margin-left-none has-margin-bottom-none has-padding-top-medium">
    <li class="column is-one-third has-padding-top-small-mobile has-padding-bottom-small">
        <a class="is-undecorated is-full-height is-block"
            href="/azure/architecture/reference-architectures/hybrid-networking/vpn?context=/azure/architecture/topics/high-performance-computing/context/hpc-context">
            <article class="card has-outline-hover is-relative is-fullheight">
                    <figure class="image has-margin-right-none has-margin-left-none has-margin-top-none has-margin-bottom-none">
                        <img role="presentation" alt="" src="/azure/architecture/reference-architectures/hybrid-networking/images/vpn.png">
                    </figure>
                <div class="card-content has-text-overflow-ellipsis">
                    <div class="has-padding-bottom-none">
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary">VPN 게이트웨이를 사용하여 온-프레미스 네트워크를 Azure에 연결</h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p>이 참조 아키텍처에서는 사이트 간 VPN(가상 사설 네트워크)을 사용하여 온-프레미스 네트워크를 Azure로 확장하는 방법을 설명합니다.</p>
                    </div>
                </div>
            </article>
        </a>
    </li>
    <li class="column is-one-third has-padding-top-small-mobile has-padding-bottom-small">
        <a class="is-undecorated is-full-height is-block"
            href="/azure/architecture/reference-architectures/hybrid-networking/expressroute?context=/azure/architecture/topics/high-performance-computing/context/hpc-context">
            <article class="card has-outline-hover is-relative is-fullheight">
                    <figure class="image has-margin-right-none has-margin-left-none has-margin-top-none has-margin-bottom-none">
                        <img role="presentation" alt="" src="/azure/architecture/reference-architectures/hybrid-networking/images/expressroute.png">
                    </figure>
                <div class="card-content has-text-overflow-ellipsis">
                    <div class="has-padding-bottom-none">
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary">ExpressRoute를 사용하여 온-프레미스 네트워크를 Azure에 연결</h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p>ExpressRoute 연결은 타사 연결 공급자를 통해 개인 전용 연결을 사용합니다. 개인 연결은 온-프레미스 네트워크를 Azure로 확장합니다.</p>
                    </div>
                </div>
            </article>
        </a>
    </li>
    <li class="column is-one-third has-padding-top-small-mobile has-padding-bottom-small">
        <a class="is-undecorated is-full-height is-block"
            href="/azure/architecture/reference-architectures/hybrid-networking/expressroute-vpn-failover?context=/azure/architecture/topics/high-performance-computing/context/hpc-context">
            <article class="card has-outline-hover is-relative is-fullheight">
                    <figure class="image has-margin-right-none has-margin-left-none has-margin-top-none has-margin-bottom-none">
                        <img role="presentation" alt="" src="/azure/architecture/reference-architectures/hybrid-networking/images/expressroute-vpn-failover.png">
                    </figure>
                <div class="card-content has-text-overflow-ellipsis">
                    <div class="has-padding-bottom-none">
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary">VPN 장애 조치(failover)를 사용하는 ExpressRoute를 사용하여 온-프레미스 네트워크를 Azure에 연결</h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p>VPN Gateway 장애 조치(failover)를 사용하는 ExpressRoute를 사용하여 연결된 Azure 가상 네트워크 및 온-프레미스 네트워크를 포함하는 고가용성의 보안 사이트 간 네트워크 아키텍처를 구축합니다.</p>
                    </div>
                </div>
            </article>
        </a>
    </li>
</ul>

네트워크 연결이 안전하게 설정되면 기존 [워크로드 관리자](#workload-managers)의 버스팅 기능을 통해 필요할 때 클라우드 컴퓨팅 리소스 사용을 시작할 수 있습니다.

### <a name="marketplace-solutions"></a>Marketplace 솔루션

[Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/)에 다양한 워크로드 관리자가 제공됩니다.

- [RogueWave CentOS 기반 HPC](https://azuremarketplace.microsoft.com/marketplace/apps/RogueWave.CentOSbased73HPC?tab=Overview)
- [SUSE Linux Enterprise Server for HPC](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver12optimizedforhighperformancecompute/)
- [TIBCO Grid Server Engine](https://azuremarketplace.microsoft.com/marketplace/apps/tibco-software.gridserverlinuxengine?tab=Overview)
- [Windows 및 Linux용 Azure 데이터 과학 VM](/azure/machine-learning/data-science-virtual-machine/overview?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [D3View](https://azuremarketplace.microsoft.com/marketplace/apps/xfinityinc.d3view-v5?tab=Overview)
- [UberCloud](https://azure.microsoft.com/search/marketplace/?q=ubercloud)

### <a name="azure-batch"></a>Azure Batch

[Azure Batch](/azure/batch/batch-technical-overview?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)는 클라우드에서 대규모 병렬 및 HPC(고성능 컴퓨팅) 애플리케이션을 효율적으로 실행하기 위한 플랫폼 서비스입니다. Azure Batch는 가상 머신의 관리되는 풀에서 실행되는 계산 집약적 작업을 예약하고, 작업 요구에 맞게 계산 리소스를 자동으로 크기 조정할 수 있습니다.

SaaS 공급자 및 개발자는 Batch SDK 및 도구를 사용하여 HPC 애플리케이션 또는 컨테이너 작업을 Azure에 통합하고, 데이터를 Azure로 스테이징하고, 작업 실행 파이프라인을 빌드할 수 있습니다.

### <a name="azure-cyclecloud"></a>Azure CycleCloud

[Azure CycleCloud](/azure/cyclecloud/?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)는 Azure에서 아무 스케줄러(예: Slurm, Grid Engine, HPC Pack, HTCondor, LSF, PBS Pro 또는 Symphony)를 사용하여 HPC 워크로드를 관리하는 가장 간단한 방법을 제공합니다.

CycleCloud로 할 수 있는 일:

- 전체 클러스터 및 기타 리소스(스케줄러, 컴퓨팅 VM, 스토리지, 네트워킹 및 캐시 포함) 배포
- 작업, 데이터 및 클라우드 워크플로 오케스트레이션
- 관리자에게 작업을 실행할 수 있는 사용자, 위치 및 비용을 제어할 수 있는 모든 권한 부여
- 비용 제어, Active Directory 통합, 모니터링 및 보고를 비롯한 고급 정책 및 거버넌스 기능을 통해 클러스터를 사용자 지정 및 최적화
- 현재 작업 스케줄러 및 애플리케이션을 수정하지 않고 사용
- 다양한 HPC 워크로드 및 산업에 기본 제공 자동 크기 조정 및 입증된 참조 아키텍처 활용

### <a name="workload-managers"></a>워크로드 관리자

Azure 인프라에서 실행할 수 있는 클러스터 및 워크로드 관리자의 예는 다음과 같습니다. Azure VM에서 독립 실행형 클러스터 또는 온-프레미스 클러스터에서 Azure VM에 대한 버스트를 만듭니다.

- [Alces 비행 계산](https://azuremarketplace.microsoft.com/marketplace/apps/alces-flight-limited.alces-flight-compute-solo?tab=Overview)
- [TIBCO DataSynapse GridServer](https://azure.microsoft.com/blog/tibco-datasynapse-comes-to-the-azure-marketplace/)
- [Bright Cluster Manager](http://www.brightcomputing.com/technology-partners/microsoft)
- [IBM Spectrum Symphony 및 Symphony LSF](https://azure.microsoft.com/blog/ibm-and-microsoft-azure-support-spectrum-symphony-and-spectrum-lsf/)
- [PBS Pro](http://pbspro.org)
- [Altair](http://www.altair.com/)
- [Rescale](https://www.rescale.com/azure/)
- [Microsoft HPC Pack](https://technet.microsoft.com/library/mt744885.aspx)
  - [Windows용 HPC 팩](/azure/virtual-machines/windows/hpcpack-cluster-options?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
  - [Linux용 HPC 팩](/azure/virtual-machines/linux/hpcpack-cluster-options?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)

#### <a name="containers"></a>컨테이너

일부 HPC 워크로드 관리에 컨테이너를 사용할 수도 있습니다.  AKS(Azure Kubernetes Service) 같은 서비스를 사용하면 Azure에 관리형 Kubernetes 클러스터를 간단하게 배포할 수 있습니다.

- [AKS(Azure Kubernetes Service)](/azure/aks/intro-kubernetes?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [컨테이너 레지스트리](/azure/container-registry/container-registry-intro?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)

## <a name="cost-management"></a>비용 관리

Azure의 HPC 비용을 관리하는 몇 가지 방법이 있습니다.  [Azure 구매 옵션](https://azure.microsoft.com/pricing/purchase-options/)을 검토하여 조직에 가장 적합한 방법을 찾을 수 있습니다.

[우선 순위가 낮은 VM](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-use-low-priority?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)을 사용하면 사용되지 않는 용량을 매우 저렴한 비용으로 활용할 수 있습니다.

## <a name="security"></a>보안

Azure의 보안 모범 사례에 대한 개요는 [Azure 보안 설명서](/azure/security/azure-security?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)를 검토하세요.  

[클라우드 버스팅](#hybrid-and-cloud-bursting) 섹션에 제공된 네트워크 구성 외에도, 컴퓨팅 리소스를 격리하기 위해 허브/스포크 구성을 구현해야 하는 경우가 있습니다.

<ul class="columns is-multiline has-margin-left-none has-margin-bottom-none has-padding-top-medium">
    <li class="column is-one-third has-padding-top-small-mobile has-padding-bottom-small">
        <a class="is-undecorated is-full-height is-block"
            href="/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?context=/azure/architecture/topics/high-performance-computing/context/hpc-context">
            <article class="card has-outline-hover is-relative is-fullheight">
                    <figure class="image has-margin-right-none has-margin-left-none has-margin-top-none has-margin-bottom-none">
                        <img role="presentation" alt="" src="/azure/architecture/reference-architectures/hybrid-networking/images/hub-spoke.png">
                    </figure>
                <div class="card-content has-text-overflow-ellipsis">
                    <div class="has-padding-bottom-none">
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary">Azure에서 허브-스포크 네트워크 토폴로지 구현</h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p>허브는 Azure의 VNet(가상 네트워크)로서 사용자의 온-프레미스 네트워크에 대한 연결의 중심으로 기능합니다. 스포크는 허브와 피어링하는 Vnet이며 워크로드를 격리하는 데 사용할 수 있습니다.</p>
                    </div>
                </div>
            </article>
        </a>
    </li>
    <li class="column is-one-third has-padding-top-small-mobile has-padding-bottom-small">
        <a class="is-undecorated is-full-height is-block"
            href="/azure/architecture/reference-architectures/hybrid-networking/shared-services?context=/azure/architecture/topics/high-performance-computing/context/hpc-context">
            <article class="card has-outline-hover is-relative is-fullheight">
                    <figure class="image has-margin-right-none has-margin-left-none has-margin-top-none has-margin-bottom-none">
                        <img role="presentation" alt="" src="/azure/architecture/reference-architectures/hybrid-networking/images/shared-services.png">
                    </figure>
                <div class="card-content has-text-overflow-ellipsis">
                    <div class="has-padding-bottom-none">
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary">Azure에서 공유 서비스를 사용하여 허브-스포크 네트워크 토폴로지 구현</h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p>이 참조 아키텍처는 모든 스포크에서 사용할 수 있는 공유 서비스를 허브에 포함하는 허브-스포크 참조 아키텍처를 기반으로 합니다.</p>
                    </div>
                </div>
            </article>
        </a>
    </li>
</ul>

## <a name="hpc-applications"></a>HPC 애플리케이션

Azure에서 사용자 지정 또는 상용 HPC 애플리케이션을 실행합니다. 이 섹션에서 일부 예제는 추가 VM 또는 계산 코어를 통해 효율적으로 확장하도록 벤치마킹됩니다. 솔루션 배포 준비는 [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace)를 방문합니다.

> [!NOTE]
> 클라우드에서 실행하기 위해 라이선스나 기타 제한 사항에 대한 상용 애플리케이션을 공급 업체에 확인합니다. 일부 공급 업체는 종량제 라이선스를 제공합니다. 솔루션에 대한 라이선스 서버가 클라우드에 필요하거나 온-프레미스 라이선스 서버에 연결할 수 있습니다.

### <a name="engineering-applications"></a>엔지니어링 애플리케이션

- [Altair RADIOSS](https://azure.microsoft.com/blog/availability-of-altair-radioss-rdma-on-microsoft-azure/)
- [ANSYS CFD](https://azure.microsoft.com/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/)
- [MATLAB 분산 컴퓨팅 서버](/azure/virtual-machines/windows/matlab-mdcs-cluster?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [StarCCM+](https://blogs.msdn.microsoft.com/azurecat/2017/07/07/run-star-ccm-in-an-azure-hpc-cluster/)
- [OpenFOAM](https://simulation.azure.com/casestudies/Team-182-ABB-UC-Final.pdf)

### <a name="graphics-and-rendering"></a>그래픽 및 렌더링

- Azure Batch의 [Autodesk Maya, 3ds Max 및 Arnold](/azure/batch/batch-rendering-service?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)

### <a name="ai-and-deep-learning"></a>AI 및 심층 학습

- [Microsoft Cognitive 도구 키트](/cognitive-toolkit/cntk-on-azure)
- [심층 학습 VM](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning)
- [심층 학습용 Batch Shipyard 레시피](https://github.com/Azure/batch-shipyard/tree/master/recipes#deeplearning)

### <a name="mpi-providers"></a>MPI 공급자

- [Microsoft MPI](/message-passing-interface/microsoft-mpi)

## <a name="remote-visualization"></a>원격 시각화

<ul class="columns is-multiline has-margin-left-none has-margin-bottom-none has-padding-top-medium">
    <li class="column is-one-third has-padding-top-small-mobile has-padding-bottom-small">
        <a class="is-undecorated is-full-height is-block"
            href="/azure/architecture/example-scenario/infrastructure/linux-vdi-citrix?context=/azure/architecture/topics/high-performance-computing/context/hpc-context">
            <article class="card has-outline-hover is-relative is-fullheight">
                    <figure class="image has-margin-right-none has-margin-left-none has-margin-top-none has-margin-bottom-none">
                        <img role="presentation" alt="" src="../../example-scenario/infrastructure/media/azure-citrix-sample-diagram.png">
                    </figure>
                <div class="card-content has-text-overflow-ellipsis">
                    <div class="has-padding-bottom-none">
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary">Citrix를 사용한 Linux 가상 데스크톱</h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p>Azure에서 Citrix를 사용하여 Linux 데스크톱에 대해 VDI 환경을 빌드합니다.</p>
                    </div>
                </div>
            </article>
        </a>
    </li>
</ul>

## <a name="performance-benchmarks"></a>성능 벤치마크

- [컴퓨팅 벤치마크](/azure/virtual-machines/windows/compute-benchmark-scores?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)

## <a name="customer-stories"></a>고객 사례

HPC 워크로드에 Azure를 사용하여 대성공을 거둔 수많은 고객 사례가 있습니다.  아래에서 이러한 고객 사례 중 일부를 확인할 수 있습니다.

- [ANEO](https://customers.microsoft.com/story/it-provider-finds-highly-scalable-cloud-based-hpc-redu)
- [AXA Global P&C](https://customers.microsoft.com/story/axa-global-p-and-c)
- [Axioma](https://customers.microsoft.com/story/axioma-delivers-fintechs-first-born-in-the-cloud-multi-asset-class-enterprise-risk-solution)
- [d3View](https://customers.microsoft.com/story/big-data-solution-provider-adopts-new-cloud-gains-thou)
- [EFS](https://customers.microsoft.com/story/efs-professionalservices-azure)
- [Hymans Robertson](https://customers.microsoft.com/story/hymans-robertson)
- [MetLife](https://enterprise.microsoft.com/customer-story/industries/insurance/metlife/)
- [Microsoft Research](https://customers.microsoft.com/doclink/fast-lmm-and-windows-azure-put-genetics-research-on-fa)
- [Milliman](https://customers.microsoft.com/story/actuarial-firm-works-to-transform-insurance-industry-w)
- [Mitsubishi UFJ Securities International](https://customers.microsoft.com/story/powering-risk-compute-grids-in-the-cloud)
- [NeuroInitiative](https://customers.microsoft.com/story/neuroinitiative-health-provider-azure)
- [Schlumberger](https://azure.microsoft.com/blog/big-compute-for-large-engineering-simulations)
- [Towers Watson](https://customers.microsoft.com/story/insurance-tech-provider-delivers-disruptive-solutions)

## <a name="other-important-information"></a>기타 중요 정보

- [vCPU 할당량](/azure/virtual-machines/linux/quotas?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)이 증가했는지 확인한 후 대규모 워크로드를 실행하세요.

## <a name="next-steps"></a>다음 단계

최신 공지는 다음 항목을 참조하세요.

- [Microsoft HPC 및 Batch 팀 블로그](http://blogs.technet.com/b/windowshpc/)
- [Azure 블로그](https://azure.microsoft.com/blog/tag/hpc/)

### <a name="microsoft-batch-examples"></a>Microsoft Batch 예제

다음 자습서는 Microsoft Batch에서 애플리케이션을 실행하기 위한 세부 정보를 제공합니다.

- [Batch를 사용하여 개발 시작](/azure/batch/quick-run-dotnet?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [Azure Batch 샘플 코드 사용(영문)](https://github.com/Azure/azure-batch-samples)
- [Batch에서 낮은 우선 순위 VM 사용](/azure/batch/batch-low-pri-vms?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [Batch Shipyard를 사용하여 컨테이너화된 HPC 작업 실행(영문)](https://github.com/Azure/batch-shipyard)
- [일괄 처리에서 병렬 R 워크로드 실행](https://github.com/Azure/doAzureParallel)
- [일괄 처리에서 주문형 Spark 작업 실행](https://github.com/Azure/aztk)
- [Batch 풀에서 계산 집약적 VM 사용](/azure/batch/batch-pool-compute-intensive-sizes?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)