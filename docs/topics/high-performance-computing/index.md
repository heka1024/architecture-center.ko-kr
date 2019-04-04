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

# <a name="high-performance-computing-hpc-on-azure"></a><span data-ttu-id="d76d5-103">Azure의 HPC(고성능 컴퓨팅)</span><span class="sxs-lookup"><span data-stu-id="d76d5-103">High Performance Computing (HPC) on Azure</span></span>

## <a name="introduction-to-hpc"></a><span data-ttu-id="d76d5-104">HPC 소개</span><span class="sxs-lookup"><span data-stu-id="d76d5-104">Introduction to HPC</span></span>

<!-- markdownlint-disable MD034 -->

> [!VIDEO https://www.youtube.com/embed/rKURT32faJk]

<!-- markdownlint-enable MD034 -->

<span data-ttu-id="d76d5-105">"빅 컴퓨팅"이라고도 하는 HPC(고성능 컴퓨팅)는 복잡한 수학 작업을 해결하기 위해 수많은 CPU 또는 GPU 기반 컴퓨터를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-105">High Performance Computing (HPC), also called "Big Compute", uses a large number of CPU or GPU-based computers to solve complex mathematical tasks.</span></span>

<span data-ttu-id="d76d5-106">많은 산업에서 업계의 가장 어려운 문제를 해결하기 위해 HPC를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-106">Many industries use HPC to solve some of their most difficult problems.</span></span>  <span data-ttu-id="d76d5-107">여기에는 다음과 같은 워크로드가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-107">These include workloads such as:</span></span>

- <span data-ttu-id="d76d5-108">Genomics</span><span class="sxs-lookup"><span data-stu-id="d76d5-108">Genomics</span></span>
- <span data-ttu-id="d76d5-109">석유 및 가스 시뮬레이션</span><span class="sxs-lookup"><span data-stu-id="d76d5-109">Oil and gas simulations</span></span>
- <span data-ttu-id="d76d5-110">재무</span><span class="sxs-lookup"><span data-stu-id="d76d5-110">Finance</span></span>
- <span data-ttu-id="d76d5-111">반도체 디자인</span><span class="sxs-lookup"><span data-stu-id="d76d5-111">Semiconductor design</span></span>
- <span data-ttu-id="d76d5-112">공학</span><span class="sxs-lookup"><span data-stu-id="d76d5-112">Engineering</span></span>
- <span data-ttu-id="d76d5-113">날씨 모델링</span><span class="sxs-lookup"><span data-stu-id="d76d5-113">Weather modeling</span></span>

### <a name="how-is-hpc-different-on-the-cloud"></a><span data-ttu-id="d76d5-114">클라우드의 HPC와 어떤 차이가 있습니까?</span><span class="sxs-lookup"><span data-stu-id="d76d5-114">How is HPC different on the cloud?</span></span>

<span data-ttu-id="d76d5-115">온-프레미스 HPC 시스템과 클라우드의 시스템의 주요 차이점 중 하나는 필요할 때 리소스를 동적으로 추가하고 제거하는 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-115">One of the primary differences between an on-premise HPC system and one in the cloud is the ability for resources to dynamically be added and removed as they're needed.</span></span>  <span data-ttu-id="d76d5-116">동적 크기 조정은 병목 현상이 발생하는 컴퓨팅 용량을 제거하고, 그 대신 고객이 작업의 요구 사항에 맞게 인프라 크기를 적절하게 조정할 수 있게 해줍니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-116">Dynamic scaling removes compute capacity as a bottleneck and instead allow customers to right size their infrastructure for the requirements of their jobs.</span></span>

<span data-ttu-id="d76d5-117">다음 문서는 이 동적 크기 조정 기능에 대한 자세한 정보를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-117">The following articles provide more detail about this dynamic scaling capability.</span></span>

- [<span data-ttu-id="d76d5-118">빅 컴퓨팅 아키텍처 스타일</span><span class="sxs-lookup"><span data-stu-id="d76d5-118">Big Compute Architecture Style</span></span>](/azure/architecture/guide/architecture-styles/big-compute?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [<span data-ttu-id="d76d5-119">자동 크기 조정 모범 사례</span><span class="sxs-lookup"><span data-stu-id="d76d5-119">Autoscaling best practices</span></span>](/azure/architecture/best-practices/auto-scaling?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)

## <a name="implementation-checklist"></a><span data-ttu-id="d76d5-120">구현 검사 목록</span><span class="sxs-lookup"><span data-stu-id="d76d5-120">Implementation checklist</span></span>

<span data-ttu-id="d76d5-121">Azure에 자체 HPC 솔루션을 구현하고자 할 때 다음 토픽을 꼭 검토하세요.</span><span class="sxs-lookup"><span data-stu-id="d76d5-121">As you're looking to implement your own HPC solution on Azure, ensure you're reviewed the following topics:</span></span>

<!-- markdownlint-disable MD032 -->

> [!div class="checklist"]
> - <span data-ttu-id="d76d5-122">요구 사항에 따라 적절한 [아키텍처](#infrastructure) 선택</span><span class="sxs-lookup"><span data-stu-id="d76d5-122">Choose the appropriate [architecture](#infrastructure) based on your requirements</span></span>
> - <span data-ttu-id="d76d5-123">워크로드에 적합한 [컴퓨팅](#compute) 옵션 알아보기</span><span class="sxs-lookup"><span data-stu-id="d76d5-123">Know which [compute](#compute) options is right for your workload</span></span>
> - <span data-ttu-id="d76d5-124">요구 사항을 충족하는 적절한 [스토리지](#storage) 솔루션 식별</span><span class="sxs-lookup"><span data-stu-id="d76d5-124">Identify the right [storage](#storage) solution that meets your needs</span></span>
> - <span data-ttu-id="d76d5-125">모든 리소스를 어떻게 [관리](#management)할 것인지 결정</span><span class="sxs-lookup"><span data-stu-id="d76d5-125">Decide how you're going to [manage](#management) all your resources</span></span>
> - <span data-ttu-id="d76d5-126">클라우드용 [애플리케이션](#hpc-applications) 최적화</span><span class="sxs-lookup"><span data-stu-id="d76d5-126">Optimize your [application](#hpc-applications) for the cloud</span></span>
> - <span data-ttu-id="d76d5-127">인프라 [보안](#security)</span><span class="sxs-lookup"><span data-stu-id="d76d5-127">[Secure](#security) your Infrastructure</span></span>

<!-- markdownlint-enable MD032 -->

## <a name="infrastructure"></a><span data-ttu-id="d76d5-128">인프라</span><span class="sxs-lookup"><span data-stu-id="d76d5-128">Infrastructure</span></span>

<span data-ttu-id="d76d5-129">HPC 시스템을 빌드하는 데 필요한 여러 인프라 구성 요소가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-129">There are a number of infrastructure components necessary to build an HPC system.</span></span>  <span data-ttu-id="d76d5-130">HPC 워크로드를 관리하는 방법으로 무엇을 선택하든 컴퓨팅, 스토리지 및 네트워킹은 기본 구성 요소를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-130">Compute, Storage, and Networking provide the underlying components, no matter how you choose to manage your HPC workloads.</span></span>

### <a name="example-hpc-architectures"></a><span data-ttu-id="d76d5-131">HPC 아키텍처 예제</span><span class="sxs-lookup"><span data-stu-id="d76d5-131">Example HPC architectures</span></span>

<span data-ttu-id="d76d5-132">Azure에서 HPC 아키텍처를 디자인하고 구현하는 다양한 방법이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-132">There are a number of different ways to design and implement your HPC architecture on Azure.</span></span>  <span data-ttu-id="d76d5-133">HPC 애플리케이션은 컴퓨팅 코어 수천 개로 확장하거나, 온-프레미스 클러스터로 확장하거나, 100% 클라우드 네이티브 솔루션으로 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-133">HPC applications can scale to thousands of compute cores, extend on-premises clusters, or run as a 100% cloud native solution.</span></span>

<span data-ttu-id="d76d5-134">다음 시나리오는 HPC 솔루션을 빌드하는 일반적인 방법 중 몇 가지를 간략하게 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-134">The following scenarios outline a few of the common ways HPC solutions are built.</span></span>

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
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary"><span data-ttu-id="d76d5-135">Azure의 컴퓨터 지원 엔지니어링 서비스</span><span class="sxs-lookup"><span data-stu-id="d76d5-135">Computer-aided engineering services on Azure</span></span></h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p><span data-ttu-id="d76d5-136">Azure에서 CAE(컴퓨터 지원 엔지니어링)에 대한 SaaS(Software-as-a-Service) 플랫폼을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-136">Provide a software-as-a-service (SaaS) platform for computer-aided engineering (CAE) on Azure.</span></span></p>
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
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary"><span data-ttu-id="d76d5-137">Azure의 CFD(컴퓨팅 유체 역학) 시뮬레이션</span><span class="sxs-lookup"><span data-stu-id="d76d5-137">Computational fluid dynamics (CFD) simulations on Azure</span></span></h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p><span data-ttu-id="d76d5-138">Azure에서 CFD(컴퓨팅 유체 역학) 시뮬레이션을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-138">Execute computational fluid dynamics (CFD) simulations on Azure.</span></span></p>
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
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary"><span data-ttu-id="d76d5-139">Azure의 3D 비디오 렌더링</span><span class="sxs-lookup"><span data-stu-id="d76d5-139">3D video rendering on Azure</span></span></h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p><span data-ttu-id="d76d5-140">Azure Batch 서비스를 사용하여 Azure에서 네이티브 HPC 워크로드를 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-140">Run native HPC workloads in Azure using the Azure Batch service</span></span></p>
                    </div>
                </div>
            </article>
        </a>
    </li>
</ul>

### <a name="compute"></a><span data-ttu-id="d76d5-141">컴퓨팅</span><span class="sxs-lookup"><span data-stu-id="d76d5-141">Compute</span></span>

<span data-ttu-id="d76d5-142">Azure는 CPU 및 GPU 사용량이 많은 워크로드에 최적화된 크기 범위를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-142">Azure offers a range of sizes that are optimized for both CPU & GPU intensive workloads.</span></span>

#### <a name="cpu-based-virtual-machines"></a><span data-ttu-id="d76d5-143">CPU 기반 가상 머신</span><span class="sxs-lookup"><span data-stu-id="d76d5-143">CPU-based virtual machines</span></span>

- [<span data-ttu-id="d76d5-144">Linux VM</span><span class="sxs-lookup"><span data-stu-id="d76d5-144">Linux VMs</span></span>](/azure/virtual-machines/linux/sizes-hpc?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- <span data-ttu-id="d76d5-145">[Windows VM의](/azure/virtual-machines/windows/sizes-hpc?context=/azure/architecture/topics/high-performance-computing/context/hpc-context) VM</span><span class="sxs-lookup"><span data-stu-id="d76d5-145">[Windows VM's](/azure/virtual-machines/windows/sizes-hpc?context=/azure/architecture/topics/high-performance-computing/context/hpc-context) VMs</span></span>
  
#### <a name="gpu-enabled-virtual-machines"></a><span data-ttu-id="d76d5-146">GPU 사용 가상 머신</span><span class="sxs-lookup"><span data-stu-id="d76d5-146">GPU-enabled virtual machines</span></span>

<span data-ttu-id="d76d5-147">N 시리즈 VM은 AI(인공 지능) 학습 및 시각화를 포함한 계산 집약적 또는 그래픽 집약적 애플리케이션을 위해 설계된 NVIDIA GPU를 특징으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-147">N-series VMs feature NVIDIA GPUs designed for compute-intensive or graphics-intensive applications including artificial intelligence (AI) learning and visualization.</span></span>

- [<span data-ttu-id="d76d5-148">Linux VM</span><span class="sxs-lookup"><span data-stu-id="d76d5-148">Linux VMs</span></span>](/azure/virtual-machines/linux/sizes-gpu?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [<span data-ttu-id="d76d5-149">Windows VM</span><span class="sxs-lookup"><span data-stu-id="d76d5-149">Windows VMs</span></span>](/azure/virtual-machines/windows/sizes-gpu?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)

### <a name="storage"></a><span data-ttu-id="d76d5-150">Storage</span><span class="sxs-lookup"><span data-stu-id="d76d5-150">Storage</span></span>

<span data-ttu-id="d76d5-151">대규모 Batch 및 HPC 작업에는 기존 클라우드 파일 시스템의 용량을 초과하는 데이터 저장 및 액세스에 대한 요구 사항이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-151">Large-scale Batch and HPC workloads have demands for data storage and access that exceed the capabilities of traditional cloud file systems.</span></span>  <span data-ttu-id="d76d5-152">Azure 기반 HPC 애플리케이션의 속도 및 용량 요구 사항을 관리하는 여러 가지 솔루션이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-152">There are a number of solutions to manage both the speed and capacity needs of HPC applications on Azure</span></span>

- <span data-ttu-id="d76d5-153">[Avere vFXT](https://azure.microsoft.com/services/storage/avere-vfxt/) - 에지의 고성능 컴퓨팅을 위한 더 빠르고 더 쉽게 액세스할 수 있는 데이터 스토리지</span><span class="sxs-lookup"><span data-stu-id="d76d5-153">[Avere vFXT](https://azure.microsoft.com/services/storage/avere-vfxt/) for faster, more accessible data storage for high-performance computing at the edge</span></span>
- [<span data-ttu-id="d76d5-154">BeeGFS</span><span class="sxs-lookup"><span data-stu-id="d76d5-154">BeeGFS</span></span>](https://azure.microsoft.com/resources/implement-glusterfs-on-azure/)
- [<span data-ttu-id="d76d5-155">스토리지 최적화 가상 머신</span><span class="sxs-lookup"><span data-stu-id="d76d5-155">Storage Optimized Virtual Machines</span></span>](/azure/virtual-machines/windows/sizes-storage?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [<span data-ttu-id="d76d5-156">Blob, 테이블 및 Queue Storage</span><span class="sxs-lookup"><span data-stu-id="d76d5-156">Blob, table, and queue storage</span></span>](/azure/storage/storage-introduction?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [<span data-ttu-id="d76d5-157">Azure SMB 파일 스토리지</span><span class="sxs-lookup"><span data-stu-id="d76d5-157">Azure SMB File storage</span></span>](/azure/storage/files/storage-files-introduction?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [<span data-ttu-id="d76d5-158">Intel Cloud Edition Lustre</span><span class="sxs-lookup"><span data-stu-id="d76d5-158">Intel Cloud Edition Lustre</span></span>](https://azuremarketplace.microsoft.com/marketplace/apps/intel.intel-cloud-edition-gs)

<span data-ttu-id="d76d5-159">Azure 기반의 Lustre, GlusterFS 및 BeeGFS를 자세히 비교하는 내용은 [Azure eBook의 병렬 파일 시스템](https://blogs.msdn.microsoft.com/azurecat/2018/06/11/azurecat-ebook-parallel-virtual-file-systems-on-microsoft-azure/)을 검토하세요.</span><span class="sxs-lookup"><span data-stu-id="d76d5-159">For more information comparing Lustre, GlusterFS, and BeeGFS on Azure, review the [Parallel Files Systems on Azure eBook](https://blogs.msdn.microsoft.com/azurecat/2018/06/11/azurecat-ebook-parallel-virtual-file-systems-on-microsoft-azure/)</span></span>

### <a name="networking"></a><span data-ttu-id="d76d5-160">네트워킹</span><span class="sxs-lookup"><span data-stu-id="d76d5-160">Networking</span></span>

<span data-ttu-id="d76d5-161">H16r, H16mr, A8 및 A9 VM은 높은 처리량 백 엔드 RDMA 네트워크에 연결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-161">H16r, H16mr, A8, and A9 VMs can connect to a high throughput back-end RDMA network.</span></span> <span data-ttu-id="d76d5-162">이 네트워크는 Microsoft MPI 또는 Intel MPI에서 실행되는 긴밀하게 결합된 병렬 애플리케이션의 성능을 개선할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-162">This network can improve the performance of tightly coupled parallel applications running under Microsoft MPI or Intel MPI.</span></span>

- [<span data-ttu-id="d76d5-163">RDMA 지원 인스턴스</span><span class="sxs-lookup"><span data-stu-id="d76d5-163">RDMA Capable Instances</span></span>](/azure/virtual-machines/windows/sizes-hpc?context=/azure/architecture/topics/high-performance-computing/context/hpc-context#rdma-capable-instances)
- [<span data-ttu-id="d76d5-164">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="d76d5-164">Virtual Network</span></span>](/azure/virtual-network/virtual-networks-overview?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [<span data-ttu-id="d76d5-165">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="d76d5-165">ExpressRoute</span></span>](/azure/expressroute/expressroute-introduction?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)

## <a name="management"></a><span data-ttu-id="d76d5-166">관리</span><span class="sxs-lookup"><span data-stu-id="d76d5-166">Management</span></span>

### <a name="do-it-yourself"></a><span data-ttu-id="d76d5-167">DIY(Do It Yourself)</span><span class="sxs-lookup"><span data-stu-id="d76d5-167">Do-it-yourself</span></span>

<span data-ttu-id="d76d5-168">Azure에서 HPC 시스템을 처음부터 새로 빌드하면 유연성의 측면에서 상당한 장점이 있지만, 종종 유지 관리 업무가 매우 많아지는 단점이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-168">Building an HPC system from scratch on Azure offers a significant amount of flexibility, but is often very maintenance intensive.</span></span>  

1. <span data-ttu-id="d76d5-169">Azure 가상 머신 또는 [가상 머신 확장 집합](/azure/virtual-machine-scale-sets/overview?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)에서 고유한 클러스터 환경을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-169">Set up your own cluster environment in Azure virtual machines or [virtual machine scale sets](/azure/virtual-machine-scale-sets/overview?context=/azure/architecture/topics/high-performance-computing/context/hpc-context).</span></span>
2. <span data-ttu-id="d76d5-170">Azure Resource Manager 템플릿을 사용하여 선행 [워크로드 관리자](#workload-managers), 인프라 및 [애플리케이션](#hpc-applications)을 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-170">Use Azure Resource Manager templates to deploy leading [workload managers](#workload-managers), infrastructure, and [applications](#hpc-applications).</span></span>
3. <span data-ttu-id="d76d5-171">MPI 또는 GPU 워크로드에 대한 특수한 하드웨어 및 네트워크 연결을 포함하는 HPC 및 GPU [VM 크기](#compute)를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-171">Choose HPC and GPU [VM sizes](#compute) that include specialized hardware and network connections for MPI or GPU workloads.</span></span>
4. <span data-ttu-id="d76d5-172">I/O 사용량이 많은 워크로드에 [고성능 저장소](#storage)를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-172">Add [high performance storage](#storage) for I/O-intensive workloads.</span></span>

### <a name="hybrid-and-cloud-bursting"></a><span data-ttu-id="d76d5-173">하이브리드 및 클라우드 버스팅</span><span class="sxs-lookup"><span data-stu-id="d76d5-173">Hybrid and cloud Bursting</span></span>

<span data-ttu-id="d76d5-174">기존 온-프레미스 HPC 시스템을 Azure에 연결하고 싶을 때 작업에 바로 사용할 수 있는 다양한 리소스가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-174">If you have an existing on-premise HPC system that you'd like to connect to Azure, there are a number of resources to help get you started.</span></span>

<span data-ttu-id="d76d5-175">먼저, 설명서의 [온-프레미스 네트워크를 Azure에 연결하는 옵션](/azure/architecture/reference-architectures/hybrid-networking/?context=/azure/architecture/topics/high-performance-computing/context/hpc-context) 문서를 검토합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-175">First, review the [Options for connecting an on-premises network to Azure](/azure/architecture/reference-architectures/hybrid-networking/?context=/azure/architecture/topics/high-performance-computing/context/hpc-context) article in the documentation.</span></span>  <span data-ttu-id="d76d5-176">그 과정에서 다음과 같은 연결 옵션에 대한 정보가 필요할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-176">From there, you may want information on these connectivity options:</span></span>

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
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary"><span data-ttu-id="d76d5-177">VPN 게이트웨이를 사용하여 온-프레미스 네트워크를 Azure에 연결</span><span class="sxs-lookup"><span data-stu-id="d76d5-177">Connect an on-premises network to Azure using a VPN gateway</span></span></h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p><span data-ttu-id="d76d5-178">이 참조 아키텍처에서는 사이트 간 VPN(가상 사설 네트워크)을 사용하여 온-프레미스 네트워크를 Azure로 확장하는 방법을 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-178">This reference architecture shows how to extend an on-premises network to Azure, using a site-to-site virtual private network (VPN).</span></span></p>
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
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary"><span data-ttu-id="d76d5-179">ExpressRoute를 사용하여 온-프레미스 네트워크를 Azure에 연결</span><span class="sxs-lookup"><span data-stu-id="d76d5-179">Connect an on-premises network to Azure using ExpressRoute</span></span></h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p><span data-ttu-id="d76d5-180">ExpressRoute 연결은 타사 연결 공급자를 통해 개인 전용 연결을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-180">ExpressRoute connections use a private, dedicated connection through a third-party connectivity provider.</span></span> <span data-ttu-id="d76d5-181">개인 연결은 온-프레미스 네트워크를 Azure로 확장합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-181">The private connection extends your on-premises network into Azure.</span></span></p>
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
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary"><span data-ttu-id="d76d5-182">VPN 장애 조치(failover)를 사용하는 ExpressRoute를 사용하여 온-프레미스 네트워크를 Azure에 연결</span><span class="sxs-lookup"><span data-stu-id="d76d5-182">Connect an on-premises network to Azure using ExpressRoute with VPN failover</span></span></h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p><span data-ttu-id="d76d5-183">VPN Gateway 장애 조치(failover)를 사용하는 ExpressRoute를 사용하여 연결된 Azure 가상 네트워크 및 온-프레미스 네트워크를 포함하는 고가용성의 보안 사이트 간 네트워크 아키텍처를 구축합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-183">Implement a highly available and secure site-to-site network architecture that spans an Azure virtual network and an on-premises network connected using ExpressRoute with VPN gateway failover.</span></span></p>
                    </div>
                </div>
            </article>
        </a>
    </li>
</ul>

<span data-ttu-id="d76d5-184">네트워크 연결이 안전하게 설정되면 기존 [워크로드 관리자](#workload-managers)의 버스팅 기능을 통해 필요할 때 클라우드 컴퓨팅 리소스 사용을 시작할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-184">Once network connectivity is securely established, you can start using cloud compute resources on-demand with the bursting capabilities of your existing [workload manager](#workload-managers).</span></span>

### <a name="marketplace-solutions"></a><span data-ttu-id="d76d5-185">Marketplace 솔루션</span><span class="sxs-lookup"><span data-stu-id="d76d5-185">Marketplace solutions</span></span>

<span data-ttu-id="d76d5-186">[Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/)에 다양한 워크로드 관리자가 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-186">There are a number of workload managers offered in the [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/).</span></span>

- [<span data-ttu-id="d76d5-187">RogueWave CentOS 기반 HPC</span><span class="sxs-lookup"><span data-stu-id="d76d5-187">RogueWave CentOS-based HPC</span></span>](https://azuremarketplace.microsoft.com/marketplace/apps/RogueWave.CentOSbased73HPC?tab=Overview)
- [<span data-ttu-id="d76d5-188">SUSE Linux Enterprise Server for HPC</span><span class="sxs-lookup"><span data-stu-id="d76d5-188">SUSE Linux Enterprise Server for HPC</span></span>](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver12optimizedforhighperformancecompute/)
- [<span data-ttu-id="d76d5-189">TIBCO Grid Server Engine</span><span class="sxs-lookup"><span data-stu-id="d76d5-189">TIBCO Grid Server Engine</span></span>](https://azuremarketplace.microsoft.com/marketplace/apps/tibco-software.gridserverlinuxengine?tab=Overview)
- [<span data-ttu-id="d76d5-190">Windows 및 Linux용 Azure 데이터 과학 VM</span><span class="sxs-lookup"><span data-stu-id="d76d5-190">Azure Data Science VM for Windows and Linux</span></span>](/azure/machine-learning/data-science-virtual-machine/overview?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [<span data-ttu-id="d76d5-191">D3View</span><span class="sxs-lookup"><span data-stu-id="d76d5-191">D3View</span></span>](https://azuremarketplace.microsoft.com/marketplace/apps/xfinityinc.d3view-v5?tab=Overview)
- [<span data-ttu-id="d76d5-192">UberCloud</span><span class="sxs-lookup"><span data-stu-id="d76d5-192">UberCloud</span></span>](https://azure.microsoft.com/search/marketplace/?q=ubercloud)

### <a name="azure-batch"></a><span data-ttu-id="d76d5-193">Azure Batch</span><span class="sxs-lookup"><span data-stu-id="d76d5-193">Azure Batch</span></span>

<span data-ttu-id="d76d5-194">[Azure Batch](/azure/batch/batch-technical-overview?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)는 클라우드에서 대규모 병렬 및 HPC(고성능 컴퓨팅) 애플리케이션을 효율적으로 실행하기 위한 플랫폼 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-194">[Azure Batch](/azure/batch/batch-technical-overview?context=/azure/architecture/topics/high-performance-computing/context/hpc-context) is a platform service for running large-scale parallel and high-performance computing (HPC) applications efficiently in the cloud.</span></span> <span data-ttu-id="d76d5-195">Azure Batch는 가상 머신의 관리되는 풀에서 실행되는 계산 집약적 작업을 예약하고, 작업 요구에 맞게 계산 리소스를 자동으로 크기 조정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-195">Azure Batch schedules compute-intensive work to run on a managed pool of virtual machines, and can automatically scale compute resources to meet the needs of your jobs.</span></span>

<span data-ttu-id="d76d5-196">SaaS 공급자 및 개발자는 Batch SDK 및 도구를 사용하여 HPC 애플리케이션 또는 컨테이너 작업을 Azure에 통합하고, 데이터를 Azure로 스테이징하고, 작업 실행 파이프라인을 빌드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-196">SaaS providers or developers can use the Batch SDKs and tools to integrate HPC applications or container workloads with Azure, stage data to Azure, and build job execution pipelines.</span></span>

### <a name="azure-cyclecloud"></a><span data-ttu-id="d76d5-197">Azure CycleCloud</span><span class="sxs-lookup"><span data-stu-id="d76d5-197">Azure CycleCloud</span></span>

<span data-ttu-id="d76d5-198">[Azure CycleCloud](/azure/cyclecloud/?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)는 Azure에서 아무 스케줄러(예: Slurm, Grid Engine, HPC Pack, HTCondor, LSF, PBS Pro 또는 Symphony)를 사용하여 HPC 워크로드를 관리하는 가장 간단한 방법을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-198">[Azure CycleCloud](/azure/cyclecloud/?context=/azure/architecture/topics/high-performance-computing/context/hpc-context) Provides the simplest way to manage HPC workloads using any scheduler (like Slurm, Grid Engine, HPC Pack, HTCondor, LSF, PBS Pro, or Symphony), on Azure</span></span>

<span data-ttu-id="d76d5-199">CycleCloud로 할 수 있는 일:</span><span class="sxs-lookup"><span data-stu-id="d76d5-199">CycleCloud allows you to:</span></span>

- <span data-ttu-id="d76d5-200">전체 클러스터 및 기타 리소스(스케줄러, 컴퓨팅 VM, 스토리지, 네트워킹 및 캐시 포함) 배포</span><span class="sxs-lookup"><span data-stu-id="d76d5-200">Deploy full clusters and other resources, including scheduler, compute VMs, storage, networking, and cache</span></span>
- <span data-ttu-id="d76d5-201">작업, 데이터 및 클라우드 워크플로 오케스트레이션</span><span class="sxs-lookup"><span data-stu-id="d76d5-201">Orchestrate job, data, and cloud workflows</span></span>
- <span data-ttu-id="d76d5-202">관리자에게 작업을 실행할 수 있는 사용자, 위치 및 비용을 제어할 수 있는 모든 권한 부여</span><span class="sxs-lookup"><span data-stu-id="d76d5-202">Give admins full control over which users can run jobs, as well as where and at what cost</span></span>
- <span data-ttu-id="d76d5-203">비용 제어, Active Directory 통합, 모니터링 및 보고를 비롯한 고급 정책 및 거버넌스 기능을 통해 클러스터를 사용자 지정 및 최적화</span><span class="sxs-lookup"><span data-stu-id="d76d5-203">Customize and optimize clusters through advanced policy and governance features, including cost controls, Active Directory integration, monitoring, and reporting</span></span>
- <span data-ttu-id="d76d5-204">현재 작업 스케줄러 및 애플리케이션을 수정하지 않고 사용</span><span class="sxs-lookup"><span data-stu-id="d76d5-204">Use your current job scheduler and applications without modification</span></span>
- <span data-ttu-id="d76d5-205">다양한 HPC 워크로드 및 산업에 기본 제공 자동 크기 조정 및 입증된 참조 아키텍처 활용</span><span class="sxs-lookup"><span data-stu-id="d76d5-205">Take advantage of built-in autoscaling and battle-tested reference architectures for a wide range of HPC workloads and industries</span></span>

### <a name="workload-managers"></a><span data-ttu-id="d76d5-206">워크로드 관리자</span><span class="sxs-lookup"><span data-stu-id="d76d5-206">Workload managers</span></span>

<span data-ttu-id="d76d5-207">Azure 인프라에서 실행할 수 있는 클러스터 및 워크로드 관리자의 예는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-207">The following are examples of cluster and workload managers that can run in Azure infrastructure.</span></span> <span data-ttu-id="d76d5-208">Azure VM에서 독립 실행형 클러스터 또는 온-프레미스 클러스터에서 Azure VM에 대한 버스트를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-208">Create stand-alone clusters in Azure VMs or burst to Azure VMs from an on-premises cluster.</span></span>

- [<span data-ttu-id="d76d5-209">Alces 비행 계산</span><span class="sxs-lookup"><span data-stu-id="d76d5-209">Alces Flight Compute</span></span>](https://azuremarketplace.microsoft.com/marketplace/apps/alces-flight-limited.alces-flight-compute-solo?tab=Overview)
- [<span data-ttu-id="d76d5-210">TIBCO DataSynapse GridServer</span><span class="sxs-lookup"><span data-stu-id="d76d5-210">TIBCO DataSynapse GridServer</span></span>](https://azure.microsoft.com/blog/tibco-datasynapse-comes-to-the-azure-marketplace/)
- [<span data-ttu-id="d76d5-211">Bright Cluster Manager</span><span class="sxs-lookup"><span data-stu-id="d76d5-211">Bright Cluster Manager</span></span>](http://www.brightcomputing.com/technology-partners/microsoft)
- [<span data-ttu-id="d76d5-212">IBM Spectrum Symphony 및 Symphony LSF</span><span class="sxs-lookup"><span data-stu-id="d76d5-212">IBM Spectrum Symphony and Symphony LSF</span></span>](https://azure.microsoft.com/blog/ibm-and-microsoft-azure-support-spectrum-symphony-and-spectrum-lsf/)
- [<span data-ttu-id="d76d5-213">PBS Pro</span><span class="sxs-lookup"><span data-stu-id="d76d5-213">PBS Pro</span></span>](http://pbspro.org)
- [<span data-ttu-id="d76d5-214">Altair</span><span class="sxs-lookup"><span data-stu-id="d76d5-214">Altair</span></span>](http://www.altair.com/)
- [<span data-ttu-id="d76d5-215">Rescale</span><span class="sxs-lookup"><span data-stu-id="d76d5-215">Rescale</span></span>](https://www.rescale.com/azure/)
- [<span data-ttu-id="d76d5-216">Microsoft HPC Pack</span><span class="sxs-lookup"><span data-stu-id="d76d5-216">Microsoft HPC Pack</span></span>](https://technet.microsoft.com/library/mt744885.aspx)
  - [<span data-ttu-id="d76d5-217">Windows용 HPC 팩</span><span class="sxs-lookup"><span data-stu-id="d76d5-217">HPC Pack for Windows</span></span>](/azure/virtual-machines/windows/hpcpack-cluster-options?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
  - [<span data-ttu-id="d76d5-218">Linux용 HPC 팩</span><span class="sxs-lookup"><span data-stu-id="d76d5-218">HPC Pack for Linux</span></span>](/azure/virtual-machines/linux/hpcpack-cluster-options?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)

#### <a name="containers"></a><span data-ttu-id="d76d5-219">컨테이너</span><span class="sxs-lookup"><span data-stu-id="d76d5-219">Containers</span></span>

<span data-ttu-id="d76d5-220">일부 HPC 워크로드 관리에 컨테이너를 사용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-220">Containers can also be used to manage some HPC workloads.</span></span>  <span data-ttu-id="d76d5-221">AKS(Azure Kubernetes Service) 같은 서비스를 사용하면 Azure에 관리형 Kubernetes 클러스터를 간단하게 배포할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-221">Services like the Azure Kubernetes Service (AKS) makes it simple to deploy a managed Kubernetes cluster in Azure.</span></span>

- [<span data-ttu-id="d76d5-222">AKS(Azure Kubernetes Service)</span><span class="sxs-lookup"><span data-stu-id="d76d5-222">Azure Kubernetes Service (AKS)</span></span>](/azure/aks/intro-kubernetes?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [<span data-ttu-id="d76d5-223">컨테이너 레지스트리</span><span class="sxs-lookup"><span data-stu-id="d76d5-223">Container Registry</span></span>](/azure/container-registry/container-registry-intro?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)

## <a name="cost-management"></a><span data-ttu-id="d76d5-224">비용 관리</span><span class="sxs-lookup"><span data-stu-id="d76d5-224">Cost management</span></span>

<span data-ttu-id="d76d5-225">Azure의 HPC 비용을 관리하는 몇 가지 방법이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-225">Managing your HPC cost on Azure can be done through a few different ways.</span></span>  <span data-ttu-id="d76d5-226">[Azure 구매 옵션](https://azure.microsoft.com/pricing/purchase-options/)을 검토하여 조직에 가장 적합한 방법을 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-226">Ensure you've reviewed the [Azure purchasing options](https://azure.microsoft.com/pricing/purchase-options/) to find the method that works best for your organization.</span></span>

<span data-ttu-id="d76d5-227">[우선 순위가 낮은 VM](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-use-low-priority?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)을 사용하면 사용되지 않는 용량을 매우 저렴한 비용으로 활용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-227">[Low priority VMs](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-use-low-priority?context=/azure/architecture/topics/high-performance-computing/context/hpc-context) allow you to take advantage of our unutilized capacity at a significant cost savings.</span></span>

## <a name="security"></a><span data-ttu-id="d76d5-228">보안</span><span class="sxs-lookup"><span data-stu-id="d76d5-228">Security</span></span>

<span data-ttu-id="d76d5-229">Azure의 보안 모범 사례에 대한 개요는 [Azure 보안 설명서](/azure/security/azure-security?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)를 검토하세요.</span><span class="sxs-lookup"><span data-stu-id="d76d5-229">For an overview of security best practices on Azure, review the [Azure Security Documentation](/azure/security/azure-security?context=/azure/architecture/topics/high-performance-computing/context/hpc-context).</span></span>  

<span data-ttu-id="d76d5-230">[클라우드 버스팅](#hybrid-and-cloud-bursting) 섹션에 제공된 네트워크 구성 외에도, 컴퓨팅 리소스를 격리하기 위해 허브/스포크 구성을 구현해야 하는 경우가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-230">In addition to the network configurations available in the [Cloud Bursting](#hybrid-and-cloud-bursting) section, you may want to implement a hub/spoke configuration to isolate your compute resources:</span></span>

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
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary"><span data-ttu-id="d76d5-231">Azure에서 허브-스포크 네트워크 토폴로지 구현</span><span class="sxs-lookup"><span data-stu-id="d76d5-231">Implement a hub-spoke network topology in Azure</span></span></h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p><span data-ttu-id="d76d5-232">허브는 Azure의 VNet(가상 네트워크)로서 사용자의 온-프레미스 네트워크에 대한 연결의 중심으로 기능합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-232">The hub is a virtual network (VNet) in Azure that acts as a central point of connectivity to your on-premises network.</span></span> <span data-ttu-id="d76d5-233">스포크는 허브와 피어링하는 Vnet이며 워크로드를 격리하는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-233">The spokes are VNets that peer with the hub, and can be used to isolate workloads.</span></span></p>
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
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary"><span data-ttu-id="d76d5-234">Azure에서 공유 서비스를 사용하여 허브-스포크 네트워크 토폴로지 구현</span><span class="sxs-lookup"><span data-stu-id="d76d5-234">Implement a hub-spoke network topology with shared services in Azure</span></span></h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p><span data-ttu-id="d76d5-235">이 참조 아키텍처는 모든 스포크에서 사용할 수 있는 공유 서비스를 허브에 포함하는 허브-스포크 참조 아키텍처를 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-235">This reference architecture builds on the hub-spoke reference architecture to include shared services in the hub that can be consumed by all spokes.</span></span></p>
                    </div>
                </div>
            </article>
        </a>
    </li>
</ul>

## <a name="hpc-applications"></a><span data-ttu-id="d76d5-236">HPC 애플리케이션</span><span class="sxs-lookup"><span data-stu-id="d76d5-236">HPC applications</span></span>

<span data-ttu-id="d76d5-237">Azure에서 사용자 지정 또는 상용 HPC 애플리케이션을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-237">Run custom or commercial HPC applications in Azure.</span></span> <span data-ttu-id="d76d5-238">이 섹션에서 일부 예제는 추가 VM 또는 계산 코어를 통해 효율적으로 확장하도록 벤치마킹됩니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-238">Several examples in this section are benchmarked to scale efficiently with additional VMs or compute cores.</span></span> <span data-ttu-id="d76d5-239">솔루션 배포 준비는 [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace)를 방문합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-239">Visit the [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace) for ready-to-deploy solutions.</span></span>

> [!NOTE]
> <span data-ttu-id="d76d5-240">클라우드에서 실행하기 위해 라이선스나 기타 제한 사항에 대한 상용 애플리케이션을 공급 업체에 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-240">Check with the vendor of any commercial application for licensing or other restrictions for running in the cloud.</span></span> <span data-ttu-id="d76d5-241">일부 공급 업체는 종량제 라이선스를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-241">Not all vendors offer pay-as-you-go licensing.</span></span> <span data-ttu-id="d76d5-242">솔루션에 대한 라이선스 서버가 클라우드에 필요하거나 온-프레미스 라이선스 서버에 연결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-242">You might need a licensing server in the cloud for your solution, or connect to an on-premises license server.</span></span>

### <a name="engineering-applications"></a><span data-ttu-id="d76d5-243">엔지니어링 애플리케이션</span><span class="sxs-lookup"><span data-stu-id="d76d5-243">Engineering applications</span></span>

- [<span data-ttu-id="d76d5-244">Altair RADIOSS</span><span class="sxs-lookup"><span data-stu-id="d76d5-244">Altair RADIOSS</span></span>](https://azure.microsoft.com/blog/availability-of-altair-radioss-rdma-on-microsoft-azure/)
- [<span data-ttu-id="d76d5-245">ANSYS CFD</span><span class="sxs-lookup"><span data-stu-id="d76d5-245">ANSYS CFD</span></span>](https://azure.microsoft.com/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/)
- [<span data-ttu-id="d76d5-246">MATLAB 분산 컴퓨팅 서버</span><span class="sxs-lookup"><span data-stu-id="d76d5-246">MATLAB Distributed Computing Server</span></span>](/azure/virtual-machines/windows/matlab-mdcs-cluster?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [<span data-ttu-id="d76d5-247">StarCCM+</span><span class="sxs-lookup"><span data-stu-id="d76d5-247">StarCCM+</span></span>](https://blogs.msdn.microsoft.com/azurecat/2017/07/07/run-star-ccm-in-an-azure-hpc-cluster/)
- [<span data-ttu-id="d76d5-248">OpenFOAM</span><span class="sxs-lookup"><span data-stu-id="d76d5-248">OpenFOAM</span></span>](https://simulation.azure.com/casestudies/Team-182-ABB-UC-Final.pdf)

### <a name="graphics-and-rendering"></a><span data-ttu-id="d76d5-249">그래픽 및 렌더링</span><span class="sxs-lookup"><span data-stu-id="d76d5-249">Graphics and rendering</span></span>

- <span data-ttu-id="d76d5-250">Azure Batch의 [Autodesk Maya, 3ds Max 및 Arnold](/azure/batch/batch-rendering-service?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)</span><span class="sxs-lookup"><span data-stu-id="d76d5-250">[Autodesk Maya, 3ds Max, and Arnold](/azure/batch/batch-rendering-service?context=/azure/architecture/topics/high-performance-computing/context/hpc-context) on Azure Batch</span></span>

### <a name="ai-and-deep-learning"></a><span data-ttu-id="d76d5-251">AI 및 심층 학습</span><span class="sxs-lookup"><span data-stu-id="d76d5-251">AI and deep learning</span></span>

- [<span data-ttu-id="d76d5-252">Microsoft Cognitive 도구 키트</span><span class="sxs-lookup"><span data-stu-id="d76d5-252">Microsoft Cognitive Toolkit</span></span>](/cognitive-toolkit/cntk-on-azure)
- [<span data-ttu-id="d76d5-253">심층 학습 VM</span><span class="sxs-lookup"><span data-stu-id="d76d5-253">Deep Learning VM</span></span>](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning)
- [<span data-ttu-id="d76d5-254">심층 학습용 Batch Shipyard 레시피</span><span class="sxs-lookup"><span data-stu-id="d76d5-254">Batch Shipyard recipes for deep learning</span></span>](https://github.com/Azure/batch-shipyard/tree/master/recipes#deeplearning)

### <a name="mpi-providers"></a><span data-ttu-id="d76d5-255">MPI 공급자</span><span class="sxs-lookup"><span data-stu-id="d76d5-255">MPI Providers</span></span>

- [<span data-ttu-id="d76d5-256">Microsoft MPI</span><span class="sxs-lookup"><span data-stu-id="d76d5-256">Microsoft MPI</span></span>](/message-passing-interface/microsoft-mpi)

## <a name="remote-visualization"></a><span data-ttu-id="d76d5-257">원격 시각화</span><span class="sxs-lookup"><span data-stu-id="d76d5-257">Remote visualization</span></span>

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
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary"><span data-ttu-id="d76d5-258">Citrix를 사용한 Linux 가상 데스크톱</span><span class="sxs-lookup"><span data-stu-id="d76d5-258">Linux virtual desktops with Citrix</span></span></h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p><span data-ttu-id="d76d5-259">Azure에서 Citrix를 사용하여 Linux 데스크톱에 대해 VDI 환경을 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-259">Build a VDI environment for Linux Desktops using Citrix on Azure.</span></span></p>
                    </div>
                </div>
            </article>
        </a>
    </li>
</ul>

## <a name="performance-benchmarks"></a><span data-ttu-id="d76d5-260">성능 벤치마크</span><span class="sxs-lookup"><span data-stu-id="d76d5-260">Performance Benchmarks</span></span>

- [<span data-ttu-id="d76d5-261">컴퓨팅 벤치마크</span><span class="sxs-lookup"><span data-stu-id="d76d5-261">Compute benchmarks</span></span>](/azure/virtual-machines/windows/compute-benchmark-scores?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)

## <a name="customer-stories"></a><span data-ttu-id="d76d5-262">고객 사례</span><span class="sxs-lookup"><span data-stu-id="d76d5-262">Customer stories</span></span>

<span data-ttu-id="d76d5-263">HPC 워크로드에 Azure를 사용하여 대성공을 거둔 수많은 고객 사례가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-263">There are a number of customers who have seen great success by using Azure for their HPC workloads.</span></span>  <span data-ttu-id="d76d5-264">아래에서 이러한 고객 사례 중 일부를 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-264">You can find a few of these customer case studies below:</span></span>

- [<span data-ttu-id="d76d5-265">ANEO</span><span class="sxs-lookup"><span data-stu-id="d76d5-265">ANEO</span></span>](https://customers.microsoft.com/story/it-provider-finds-highly-scalable-cloud-based-hpc-redu)
- [<span data-ttu-id="d76d5-266">AXA Global P&C</span><span class="sxs-lookup"><span data-stu-id="d76d5-266">AXA Global P&C</span></span>](https://customers.microsoft.com/story/axa-global-p-and-c)
- [<span data-ttu-id="d76d5-267">Axioma</span><span class="sxs-lookup"><span data-stu-id="d76d5-267">Axioma</span></span>](https://customers.microsoft.com/story/axioma-delivers-fintechs-first-born-in-the-cloud-multi-asset-class-enterprise-risk-solution)
- [<span data-ttu-id="d76d5-268">d3View</span><span class="sxs-lookup"><span data-stu-id="d76d5-268">d3View</span></span>](https://customers.microsoft.com/story/big-data-solution-provider-adopts-new-cloud-gains-thou)
- [<span data-ttu-id="d76d5-269">EFS</span><span class="sxs-lookup"><span data-stu-id="d76d5-269">EFS</span></span>](https://customers.microsoft.com/story/efs-professionalservices-azure)
- [<span data-ttu-id="d76d5-270">Hymans Robertson</span><span class="sxs-lookup"><span data-stu-id="d76d5-270">Hymans Robertson</span></span>](https://customers.microsoft.com/story/hymans-robertson)
- [<span data-ttu-id="d76d5-271">MetLife</span><span class="sxs-lookup"><span data-stu-id="d76d5-271">MetLife</span></span>](https://enterprise.microsoft.com/customer-story/industries/insurance/metlife/)
- [<span data-ttu-id="d76d5-272">Microsoft Research</span><span class="sxs-lookup"><span data-stu-id="d76d5-272">Microsoft Research</span></span>](https://customers.microsoft.com/doclink/fast-lmm-and-windows-azure-put-genetics-research-on-fa)
- [<span data-ttu-id="d76d5-273">Milliman</span><span class="sxs-lookup"><span data-stu-id="d76d5-273">Milliman</span></span>](https://customers.microsoft.com/story/actuarial-firm-works-to-transform-insurance-industry-w)
- [<span data-ttu-id="d76d5-274">Mitsubishi UFJ Securities International</span><span class="sxs-lookup"><span data-stu-id="d76d5-274">Mitsubishi UFJ Securities International</span></span>](https://customers.microsoft.com/story/powering-risk-compute-grids-in-the-cloud)
- [<span data-ttu-id="d76d5-275">NeuroInitiative</span><span class="sxs-lookup"><span data-stu-id="d76d5-275">NeuroInitiative</span></span>](https://customers.microsoft.com/story/neuroinitiative-health-provider-azure)
- [<span data-ttu-id="d76d5-276">Schlumberger</span><span class="sxs-lookup"><span data-stu-id="d76d5-276">Schlumberger</span></span>](https://azure.microsoft.com/blog/big-compute-for-large-engineering-simulations)
- [<span data-ttu-id="d76d5-277">Towers Watson</span><span class="sxs-lookup"><span data-stu-id="d76d5-277">Towers Watson</span></span>](https://customers.microsoft.com/story/insurance-tech-provider-delivers-disruptive-solutions)

## <a name="other-important-information"></a><span data-ttu-id="d76d5-278">기타 중요 정보</span><span class="sxs-lookup"><span data-stu-id="d76d5-278">Other important information</span></span>

- <span data-ttu-id="d76d5-279">[vCPU 할당량](/azure/virtual-machines/linux/quotas?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)이 증가했는지 확인한 후 대규모 워크로드를 실행하세요.</span><span class="sxs-lookup"><span data-stu-id="d76d5-279">Ensure your [vCPU quota](/azure/virtual-machines/linux/quotas?context=/azure/architecture/topics/high-performance-computing/context/hpc-context) has been increased before attempting to run large-scale workloads.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d76d5-280">다음 단계</span><span class="sxs-lookup"><span data-stu-id="d76d5-280">Next steps</span></span>

<span data-ttu-id="d76d5-281">최신 공지는 다음 항목을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="d76d5-281">For the latest announcements, see:</span></span>

- [<span data-ttu-id="d76d5-282">Microsoft HPC 및 Batch 팀 블로그</span><span class="sxs-lookup"><span data-stu-id="d76d5-282">Microsoft HPC and Batch team blog</span></span>](http://blogs.technet.com/b/windowshpc/)
- <span data-ttu-id="d76d5-283">[Azure 블로그](https://azure.microsoft.com/blog/tag/hpc/)</span><span class="sxs-lookup"><span data-stu-id="d76d5-283">Visit the [Azure blog](https://azure.microsoft.com/blog/tag/hpc/).</span></span>

### <a name="microsoft-batch-examples"></a><span data-ttu-id="d76d5-284">Microsoft Batch 예제</span><span class="sxs-lookup"><span data-stu-id="d76d5-284">Microsoft Batch Examples</span></span>

<span data-ttu-id="d76d5-285">다음 자습서는 Microsoft Batch에서 애플리케이션을 실행하기 위한 세부 정보를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="d76d5-285">These tutorials will provide you with details on running applications on Microsoft Batch</span></span>

- [<span data-ttu-id="d76d5-286">Batch를 사용하여 개발 시작</span><span class="sxs-lookup"><span data-stu-id="d76d5-286">Get started developing with Batch</span></span>](/azure/batch/quick-run-dotnet?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [<span data-ttu-id="d76d5-287">Azure Batch 샘플 코드 사용(영문)</span><span class="sxs-lookup"><span data-stu-id="d76d5-287">Use Azure Batch code samples</span></span>](https://github.com/Azure/azure-batch-samples)
- [<span data-ttu-id="d76d5-288">Batch에서 낮은 우선 순위 VM 사용</span><span class="sxs-lookup"><span data-stu-id="d76d5-288">Use low-priority VMs with Batch</span></span>](/azure/batch/batch-low-pri-vms?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [<span data-ttu-id="d76d5-289">Batch Shipyard를 사용하여 컨테이너화된 HPC 작업 실행(영문)</span><span class="sxs-lookup"><span data-stu-id="d76d5-289">Run containerized HPC workloads with Batch Shipyard</span></span>](https://github.com/Azure/batch-shipyard)
- [<span data-ttu-id="d76d5-290">일괄 처리에서 병렬 R 워크로드 실행</span><span class="sxs-lookup"><span data-stu-id="d76d5-290">Run parallel R workloads on Batch</span></span>](https://github.com/Azure/doAzureParallel)
- [<span data-ttu-id="d76d5-291">일괄 처리에서 주문형 Spark 작업 실행</span><span class="sxs-lookup"><span data-stu-id="d76d5-291">Run on-demand Spark jobs on Batch</span></span>](https://github.com/Azure/aztk)
- [<span data-ttu-id="d76d5-292">Batch 풀에서 계산 집약적 VM 사용</span><span class="sxs-lookup"><span data-stu-id="d76d5-292">Use compute-intensive VMs in Batch pools</span></span>](/azure/batch/batch-pool-compute-intensive-sizes?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)