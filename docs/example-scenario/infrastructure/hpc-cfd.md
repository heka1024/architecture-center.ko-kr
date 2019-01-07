---
title: CFD 시뮬레이션 실행
titleSuffix: Azure Example Scenarios
description: Azure에서 CFD(컴퓨팅 유체 역학) 시뮬레이션을 실행합니다.
author: mikewarr
ms.date: 09/20/2018
ms.custom: fasttrack
ms.openlocfilehash: af43a60e952d75f84b4c7903a1567b0c76b9f4c4
ms.sourcegitcommit: bb7fcffbb41e2c26a26f8781df32825eb60df70c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/20/2018
ms.locfileid: "53643937"
---
# <a name="running-computational-fluid-dynamics-cfd-simulations-on-azure"></a><span data-ttu-id="76c9b-103">Azure에서 CFD(컴퓨팅 유체 역학) 시뮬레이션 실행</span><span class="sxs-lookup"><span data-stu-id="76c9b-103">Running computational fluid dynamics (CFD) simulations on Azure</span></span>

<span data-ttu-id="76c9b-104">CFD(계산 유체 역학) 시뮬레이션에는 특수 하드웨어와 함께 상당한 계산 시간이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-104">Computational Fluid Dynamics (CFD) simulations require significant compute time along with specialized hardware.</span></span> <span data-ttu-id="76c9b-105">클러스터 사용량이 증가하면 시뮬레이션 시간 및 전반적인 그리드 사용량이 증가하여 예비 용량 및 긴 큐 시간 문제로 이어집니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-105">As cluster usage increases, simulation times and overall grid use grow, leading to issues with spare capacity and long queue times.</span></span> <span data-ttu-id="76c9b-106">물리적 하드웨어를 추가하는 방법은 비용이 많이 들고, 기업에서 발생하는 최대 사용량과 일치하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-106">Adding physical hardware can be expensive, and may not align to the usage peaks and valleys that a business goes through.</span></span> <span data-ttu-id="76c9b-107">Azure를 활용하면 자본 지출 없이 이러한 문제를 대부분 해결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-107">By taking advantage of Azure, many of these challenges can be overcome with no capital expenditure.</span></span>

<span data-ttu-id="76c9b-108">Azure는 GPU 및 CPU 가상 머신에서 CFD 작업을 실행하는 데 필요한 하드웨어를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-108">Azure provides the hardware you need to run your CFD jobs on both GPU and CPU virtual machines.</span></span> <span data-ttu-id="76c9b-109">RDMA(Remote Direct Memory Access)가 설정된 VM 크기는 짧은 대기 시간으로 MPI(메시지 전달 인터페이스) 통신이 가능한 FDR InfiniBand 기반 네트워킹을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-109">RDMA (Remote Direct Memory Access) enabled VM sizes have FDR InfiniBand-based networking which allows for low latency MPI (Message Passing Interface) communication.</span></span> <span data-ttu-id="76c9b-110">Avere vFXT와 결합하면 엔터프라이즈 규모의 클러스터링 파일 시스템을 제공하므로 고객이 Azure에서 읽기 작업에 최대 처리량을 투입할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-110">Combined with the Avere vFXT, which provides an enterprise-scale clustered file system, customers can ensure maximum throughput for read operations in Azure.</span></span>

<span data-ttu-id="76c9b-111">HPC 클러스터의 생성, 관리 및 최적화를 간소화하기 위해 Azure CycleCloud를 사용하여 하이브리드 및 클라우드 시나리오에서 클러스터를 프로비전하고 데이터를 오케스트레이션할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-111">To simplify the creation, management, and optimization of HPC clusters, Azure CycleCloud can be used to provision clusters and orchestrate data in both hybrid and cloud scenarios.</span></span> <span data-ttu-id="76c9b-112">CycleCloud는 보류 중인 작업을 모니터링하다가 사용자가 선택한 워크로드 스케줄러에 연결된 주문형 계산을 자동으로 실행하며, 사용만 만큼만 요금을 지불하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-112">By monitoring the pending jobs, CycleCloud will automatically launch on-demand compute, where you only pay for what you use, connected to the workload scheduler of your choice.</span></span>

## <a name="relevant-use-cases"></a><span data-ttu-id="76c9b-113">관련 사용 사례</span><span class="sxs-lookup"><span data-stu-id="76c9b-113">Relevant use cases</span></span>

<span data-ttu-id="76c9b-114">CFD 애플리케이션에 대한 기타 관련 산업은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-114">Other relevant industries for CFD applications include:</span></span>

- <span data-ttu-id="76c9b-115">항공학</span><span class="sxs-lookup"><span data-stu-id="76c9b-115">Aeronautics</span></span>
- <span data-ttu-id="76c9b-116">자동차</span><span class="sxs-lookup"><span data-stu-id="76c9b-116">Automotive</span></span>
- <span data-ttu-id="76c9b-117">HVAC 제작</span><span class="sxs-lookup"><span data-stu-id="76c9b-117">Building HVAC</span></span>
- <span data-ttu-id="76c9b-118">석유 및 가스</span><span class="sxs-lookup"><span data-stu-id="76c9b-118">Oil and gas</span></span>
- <span data-ttu-id="76c9b-119">생명 과학</span><span class="sxs-lookup"><span data-stu-id="76c9b-119">Life sciences</span></span>

## <a name="architecture"></a><span data-ttu-id="76c9b-120">아키텍처</span><span class="sxs-lookup"><span data-stu-id="76c9b-120">Architecture</span></span>

![아키텍처 다이어그램][architecture]

<span data-ttu-id="76c9b-122">이 다이어그램은 Azure에서 주문형 노드의 작업 모니터링을 제공하는 일반적인 하이브리드 디자인의 대략적인 개요를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-122">This diagram shows a high-level overview of a typical hybrid design providing job monitoring of the on-demand nodes in Azure:</span></span>

1. <span data-ttu-id="76c9b-123">클러스터를 구성하려면 Azure CycleCloud 서버에 연결합니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-123">Connect to the Azure CycleCloud server to configure the cluster.</span></span>
2. <span data-ttu-id="76c9b-124">MPI에 RDMA 지원 머신을 사용하여 클러스터 헤드 노드를 구성하고 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-124">Configure and create the cluster head node, using RDMA enabled machines for MPI.</span></span>
3. <span data-ttu-id="76c9b-125">온-프레미스 헤드 노드를 추가하고 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-125">Add and configure the on-premises head node.</span></span>
4. <span data-ttu-id="76c9b-126">리소스가 부족한 경우 Azure CycleCloud가 Azure에서 계산 리소스를 강화(또는 축소)합니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-126">If there are insufficient resources, Azure CycleCloud will scale up (or down) compute resources in Azure.</span></span> <span data-ttu-id="76c9b-127">미리 결정된 제한을 정의하여 초과 할당을 방지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-127">A predetermined limit can be defined to prevent over allocation.</span></span>
5. <span data-ttu-id="76c9b-128">작업을 실행 노드에 할당합니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-128">Tasks allocated to the execute nodes.</span></span>
6. <span data-ttu-id="76c9b-129">온-프레미스 NFS 서버의 데이터를 Azure에 캐시합니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-129">Data cached in Azure from on-premises NFS server.</span></span>
7. <span data-ttu-id="76c9b-130">Azure 캐시용 Avere vFXT에서 데이터를 읽어 옵니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-130">Data read in from the Avere vFXT for Azure cache.</span></span>
8. <span data-ttu-id="76c9b-131">작업 및 태스크 정보를 Azure CycleCloud 서버에 릴레이합니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-131">Job and task information relayed to the Azure CycleCloud server.</span></span>

### <a name="components"></a><span data-ttu-id="76c9b-132">구성 요소</span><span class="sxs-lookup"><span data-stu-id="76c9b-132">Components</span></span>

- <span data-ttu-id="76c9b-133">[Azure CycleCloud][cyclecloud] - Azure에서 HPC 및 큰 계산 클러스터를 만들고 관리하고 운영하고 최적화할 수 있는 도구입니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-133">[Azure CycleCloud][cyclecloud] a tool for creating, managing, operating, and optimizing HPC and Big Compute clusters in Azure.</span></span>
- <span data-ttu-id="76c9b-134">[Azure 기반의 Avere vFXT][avere] - 클라우드용으로 빌드된 엔터프라이즈 규모 클러스터링 파일 시스템을 제공하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-134">[Avere vFXT on Azure][avere] is used to provide an enterprise-scale clustered file system built for the cloud.</span></span>
- <span data-ttu-id="76c9b-135">[Azure VM(Virtual Machines)][vms] - 정적 계산 인스턴스 집합을 만드는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-135">[Azure Virtual Machines (VMs)][vms] are used to create a static set of compute instances.</span></span>
- <span data-ttu-id="76c9b-136">[Virtual Machine Scale Sets(가상 머신 확장 집합)][vmss] - Azure CycleCloud가 강화 또는 축소할 수 있는 동일한 VM 그룹을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-136">[Virtual Machine Scale Sets (virtual machine scale set)][vmss] provide a group of identical VMs capable of being scaled up or down by Azure CycleCloud.</span></span>
- <span data-ttu-id="76c9b-137">[Azure Storage 계정](/azure/storage/common/storage-introduction) - 동기화 및 데이터 보존에 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-137">[Azure Storage accounts](/azure/storage/common/storage-introduction) are used for synchronization and data retention.</span></span>
- <span data-ttu-id="76c9b-138">[Virtual Network](/azure/virtual-network/virtual-networks-overview) - Azure VM(Virtual Machines) 같은 다양한 형식의 Azure 리소스가 상호 간 통신, 인터넷 통신 및 온-프레미스 네트워크 통신을 안전하게 수행할 수 있게 해줍니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-138">[Virtual Networks](/azure/virtual-network/virtual-networks-overview) enable many types of Azure resources, such as Azure Virtual Machines (VMs), to securely communicate with each other, the internet, and on-premises networks.</span></span>

### <a name="alternatives"></a><span data-ttu-id="76c9b-139">대안</span><span class="sxs-lookup"><span data-stu-id="76c9b-139">Alternatives</span></span>

<span data-ttu-id="76c9b-140">고객은 Azure CycleCloud를 사용하여 Azure 전체에서 그리드를 만들 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-140">Customers can also use Azure CycleCloud to create a grid entirely in Azure.</span></span> <span data-ttu-id="76c9b-141">이 설치에서 Azure CycleCloud 서버는 Azure 구독 내에서 실행됩니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-141">In this setup, the Azure CycleCloud server is run within your Azure subscription.</span></span>

<span data-ttu-id="76c9b-142">워크로드 스케줄러 관리가 필요 없는 최신 애플리케이션 방식에서는 [Azure Batch][batch]가 도움이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-142">For a modern application approach where management of a workload scheduler is not needed, [Azure Batch][batch] can help.</span></span> <span data-ttu-id="76c9b-143">Azure Batch는 클라우드에서 대규모 병렬 및 HPC(고성능 컴퓨팅) 애플리케이션을 효율적으로 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-143">Azure Batch can run large-scale parallel and high-performance computing (HPC) applications efficiently in the cloud.</span></span> <span data-ttu-id="76c9b-144">Azure Batch를 사용하여 인프라를 수동으로 구성하거나 관리하지 않고 병렬 또는 대규모로 애플리케이션을 실행하도록 Azure 계산 리소스를 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-144">Azure Batch allows you to define the Azure compute resources to execute your applications in parallel or at scale without manually configuring or managing infrastructure.</span></span> <span data-ttu-id="76c9b-145">Azure Batch는 요구 사항에 따라 계산 집약적 작업을 예약하고 계산 리소스를 동적으로 추가 및 제거합니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-145">Azure Batch schedules compute-intensive tasks and dynamically adds and removes compute resources based on your requirements.</span></span>

### <a name="scalability-and-security"></a><span data-ttu-id="76c9b-146">확장성 및 보안</span><span class="sxs-lookup"><span data-stu-id="76c9b-146">Scalability, and Security</span></span>

<span data-ttu-id="76c9b-147">수동으로 또는 자동 크기 조정을 사용하여 Azure CycleCloud의 실행 노드 수를 조정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-147">Scaling the execute nodes on Azure CycleCloud can be accomplished either manually or using autoscaling.</span></span> <span data-ttu-id="76c9b-148">자세한 내용은 [CycleCloud 자동 크기 조정][cycle-scale]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="76c9b-148">For more information, see [CycleCloud Autoscaling][cycle-scale].</span></span>

<span data-ttu-id="76c9b-149">보안 솔루션 설계에 대한 일반적인 지침은 [Azure 보안 설명서][security]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="76c9b-149">For general guidance on designing secure solutions, see the [Azure security documentation][security].</span></span>

## <a name="deploy-the-scenario"></a><span data-ttu-id="76c9b-150">시나리오 배포</span><span class="sxs-lookup"><span data-stu-id="76c9b-150">Deploy the scenario</span></span>

### <a name="prerequisites"></a><span data-ttu-id="76c9b-151">필수 조건</span><span class="sxs-lookup"><span data-stu-id="76c9b-151">Prerequisites</span></span>

<span data-ttu-id="76c9b-152">Resource Manager 템플릿을 배포하기 전에 다음 단계를 수행하세요.</span><span class="sxs-lookup"><span data-stu-id="76c9b-152">Follow these steps before deploying the Resource Manager template:</span></span>

1. <span data-ttu-id="76c9b-153">appId, displayName, 이름, 암호 및 테넌트를 검색하는 데 사용할 [서비스 주체][cycle-svcprin]를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-153">Create a [service principal][cycle-svcprin] for retrieving the appId, displayName, name, password, and tenant.</span></span>
2. <span data-ttu-id="76c9b-154">CycleCloud 서버에 안전하게 로그인하기 위한 [SSH 키 쌍][cycle-ssh]을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-154">Generate an [SSH key pair][cycle-ssh] to sign in securely to the CycleCloud server.</span></span>

    <!-- markdownlint-disable MD033 -->

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FCycleCloudCommunity%2Fcyclecloud_arm%2Fmaster%2Fazuredeploy.json" target="_blank">
        <img src="https://azuredeploy.net/deploybutton.png"/>
    </a>

    <!-- markdownlint-enable MD033 -->

3. <span data-ttu-id="76c9b-155">[CycleCloud 서버에 로그인][cycle-login]하여 새 클러스터를 만들고 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-155">[Log into the CycleCloud server][cycle-login] to configure and create a new cluster.</span></span>
4. <span data-ttu-id="76c9b-156">[클러스터를 만듭니다][cycle-create].</span><span class="sxs-lookup"><span data-stu-id="76c9b-156">[Create a cluster][cycle-create].</span></span>

<span data-ttu-id="76c9b-157">Avere Cache는 애플리케이션 작업 데이터의 읽기 처리량을 대폭 높일 수 있는 선택적 솔루션입니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-157">The Avere Cache is an optional solution that can drastically increase read throughput for the application job data.</span></span> <span data-ttu-id="76c9b-158">Avere vFXT for Azure는 이러한 엔터프라이즈 HPC 애플리케이션을 클라우드에서 실행하는 문제를 해결하는 한편, 온-프레미스 또는 Azure Blob 저장소에 저장된 데이터를 활용합니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-158">Avere vFXT for Azure solves the problem of running these enterprise HPC applications in the cloud while leveraging data stored on-premises or in Azure Blob storage.</span></span>

<span data-ttu-id="76c9b-159">온-프레미스 저장소와 클라우드 계산을 모두 사용하는 하이브리드 인프라를 계획하고 있는 조직의 경우 HPC 애플리케이션이 NAS 장치에 저장된 데이터를 사용하여 Azure로 “버스트”하고 필요에 따라 가상 CPU를 작동할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-159">For organizations that are planning for a hybrid infrastructure with both on-premises storage and cloud computing, HPC applications can “burst” into Azure using data stored in NAS devices and spin up virtual CPUs as needed.</span></span> <span data-ttu-id="76c9b-160">데이터 집합은 클라우드로 완전히 이동되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-160">The data set is never moved completely into the cloud.</span></span> <span data-ttu-id="76c9b-161">요청된 바이트는 처리 중에 Avere 클러스터를 사용하여 일시적으로 캐시됩니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-161">The requested bytes are temporarily cached using an Avere cluster during processing.</span></span>

<span data-ttu-id="76c9b-162">Avere vFXT 설치를 설정하고 구성하려면 [Avere 설치 및 구성 가이드][avere]를 따르세요.</span><span class="sxs-lookup"><span data-stu-id="76c9b-162">To set up and configure an Avere vFXT installation, follow the [Avere Setup and Configuration guide][avere].</span></span>

## <a name="pricing"></a><span data-ttu-id="76c9b-163">가격</span><span class="sxs-lookup"><span data-stu-id="76c9b-163">Pricing</span></span>

<span data-ttu-id="76c9b-164">CycleCloud 서버를 사용하여 HPC 구현을 실행하는 비용은 여러 가지 요인에 따라 달라집니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-164">The cost of running an HPC implementation using CycleCloud server will vary depending on a number of factors.</span></span> <span data-ttu-id="76c9b-165">예를 들어 CycleCloud는 사용된 계산 시간에 따라 요금이 청구되며, 마스터 및 CycleCloud 서버는 일반적으로 일관적으로 할당 및 실행됩니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-165">For example, CycleCloud is charged by the amount of compute time that is used, with the Master and CycleCloud server typically being constantly allocated and running.</span></span> <span data-ttu-id="76c9b-166">실행 노드를 실행하는 비용은 실행 시간 및 사용하는 크기에 따라 달라집니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-166">The cost of running the Execute nodes will depend on how long these are up and running as well as what size is used.</span></span> <span data-ttu-id="76c9b-167">또한 저장소 및 네트워킹에 대한 일반 Azure 요금이 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-167">The normal Azure charges for storage and networking also apply.</span></span>

<span data-ttu-id="76c9b-168">이 시나리오는 CFD 애플리케이션을 Azure에서 실행하는 방법을 보여주며, 이렇게 하려면 특정 VM 크기에서만 사용할 수 있는 RDMA 기능이 머신에 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-168">This scenario shows how CFD applications can be run in Azure, so the machines will require RDMA functionality, which is only available on specific VM sizes.</span></span> <span data-ttu-id="76c9b-169">다음은 확장 집합을 1개월 동안 하루에 8시간 연속으로 할당하고 데이터 1TB를 송신할 때 발생할 수 있는 비용 예제입니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-169">The following are examples of costs that could be incurred for a scale set that is allocated continuously for eight hours per day for one month, with data egress of 1 TB.</span></span> <span data-ttu-id="76c9b-170">Azure CycleCloud 서버 및 Avere vFXT for Azure 설치에 대한 가격 책정도 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="76c9b-170">It also includes pricing for the Azure CycleCloud server and the Avere vFXT for Azure install:</span></span>

- <span data-ttu-id="76c9b-171">지역: 북유럽</span><span class="sxs-lookup"><span data-stu-id="76c9b-171">Region: North Europe</span></span>
- <span data-ttu-id="76c9b-172">Azure CycleCloud 서버: 표준 D3 1개(CPU 4개, 14GB 메모리, 표준 HDD 32GB)</span><span class="sxs-lookup"><span data-stu-id="76c9b-172">Azure CycleCloud Server: 1 x Standard D3 (4 x CPUs, 14 GB Memory, Standard HDD 32 GB)</span></span>
- <span data-ttu-id="76c9b-173">Azure CycleCloud 마스터 서버: 표준 D12 v 1개(CPU 4개, 28GB 메모리, 표준 HDD 32GB)</span><span class="sxs-lookup"><span data-stu-id="76c9b-173">Azure CycleCloud Master Server: 1 x Standard D12 v (4 x CPUs, 28 GB Memory, Standard HDD 32 GB)</span></span>
- <span data-ttu-id="76c9b-174">Azure CycleCloud 노드 배열: 표준 H16r 10개(CPU 16개, 112GB 메모리)</span><span class="sxs-lookup"><span data-stu-id="76c9b-174">Azure CycleCloud Node Array: 10 x Standard H16r (16 x CPUs, 112 GB Memory)</span></span>
- <span data-ttu-id="76c9b-175">Azure 클러스터의 Avere vFXT: D16s v3 3개(200GB OS, 프리미엄 SSD 1TB 데이터 디스크)</span><span class="sxs-lookup"><span data-stu-id="76c9b-175">Avere vFXT on Azure Cluster: 3 x D16s v3 (200 GB OS, Premium SSD 1-TB data disk)</span></span>
- <span data-ttu-id="76c9b-176">데이터 송신: 1TB</span><span class="sxs-lookup"><span data-stu-id="76c9b-176">Data Egress: 1 TB</span></span>

<span data-ttu-id="76c9b-177">위에 나열된 하드웨어는 이 [예상 가격][pricing]을 검토하세요.</span><span class="sxs-lookup"><span data-stu-id="76c9b-177">Review this [price estimate][pricing] for the hardware listed above.</span></span>

## <a name="next-steps"></a><span data-ttu-id="76c9b-178">다음 단계</span><span class="sxs-lookup"><span data-stu-id="76c9b-178">Next Steps</span></span>

<span data-ttu-id="76c9b-179">샘플을 배포한 후에는 [Azure CycleCloud][cyclecloud]에 대해 자세히 알아보세요.</span><span class="sxs-lookup"><span data-stu-id="76c9b-179">Once you've deployed the sample, learn more about [Azure CycleCloud][cyclecloud].</span></span>

## <a name="related-resources"></a><span data-ttu-id="76c9b-180">관련 리소스</span><span class="sxs-lookup"><span data-stu-id="76c9b-180">Related resources</span></span>

- <span data-ttu-id="76c9b-181">[RDMA 지원 머신 인스턴스][rdma]</span><span class="sxs-lookup"><span data-stu-id="76c9b-181">[RDMA Capable Machine Instances][rdma]</span></span>
- <span data-ttu-id="76c9b-182">[RDMA 인스턴스 VM 사용자 지정][rdma-custom]</span><span class="sxs-lookup"><span data-stu-id="76c9b-182">[Customizing an RDMA Instance VM][rdma-custom]</span></span>

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
