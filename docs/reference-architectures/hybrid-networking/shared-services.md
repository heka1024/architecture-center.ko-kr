---
title: 허브-스포크 네트워크 토폴로지 구현
titleSuffix: Azure Reference Architectures
description: Azure에서 공유 서비스를 사용하여 허브-스포크 네트워크 토폴로지를 구현합니다.
author: telmosampaio
ms.date: 10/09/2018
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: seodec18, networking
ms.openlocfilehash: dd7632c3a84f6a0cee5d8b35e6a943ab8c52caf8
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54488310"
---
# <a name="implement-a-hub-spoke-network-topology-with-shared-services-in-azure"></a><span data-ttu-id="a70f1-103">Azure에서 공유 서비스를 사용하여 허브-스포크 네트워크 토폴로지 구현</span><span class="sxs-lookup"><span data-stu-id="a70f1-103">Implement a hub-spoke network topology with shared services in Azure</span></span>

<span data-ttu-id="a70f1-104">이 참조 아키텍처는 모든 스포크에서 사용할 수 있는 허브의 공유 서비스를 포함하는 [hub-spoke][guidance-hub-spoke] 아키텍처에 기반합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-104">This reference architecture builds on the [hub-spoke][guidance-hub-spoke] reference architecture to include shared services in the hub that can be consumed by all spokes.</span></span> <span data-ttu-id="a70f1-105">데이터 센터를 클라우드로 마이그레이션하고 [가상 데이터 센터]를 빌드하는 첫 번째 단계로 공유해야 하는 첫 번째 서비스는 ID와 보안입니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-105">As a first step toward migrating a datacenter to the cloud, and building a [virtual datacenter], the first services you need to share are identity and security.</span></span> <span data-ttu-id="a70f1-106">이 참조 아키텍처에서는 온-프레미스 데이터 센터에서 Azure로 Active Directory 서비스를 확장하는 방법 및 허브-스포크 토폴로지에서 방화벽으로 사용할 수 있는 NVA(네트워크 가상 어플라이언스)를 추가하는 방법을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-106">This reference architecture shows you how to extend your Active Directory services from your on-premises datacenter to Azure, and how to add a network virtual appliance (NVA) that can act as a firewall, in a hub-spoke topology.</span></span>  <span data-ttu-id="a70f1-107">[**이 솔루션을 배포합니다**](#deploy-the-solution).</span><span class="sxs-lookup"><span data-stu-id="a70f1-107">[**Deploy this solution**](#deploy-the-solution).</span></span>

> [!NOTE]
> <span data-ttu-id="a70f1-108">이 시나리오는 클라우드 기반 네트워크 보안 서비스인 [Azure Firewall](/azure/firewall/)을 사용하여 수행할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-108">This scenario can also be accomplished using [Azure Firewall](/azure/firewall/), a cloud-based network security service.</span></span>

![Azure의 공유 서비스 토폴로지](./images/shared-services.png)

<span data-ttu-id="a70f1-110">*이 아키텍처의 [Visio 파일][visio-download] 다운로드*</span><span class="sxs-lookup"><span data-stu-id="a70f1-110">*Download a [Visio file][visio-download] of this architecture*</span></span>

<span data-ttu-id="a70f1-111">이 토폴로지의 이점은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-111">The benefits of this topology include:</span></span>

- <span data-ttu-id="a70f1-112">**비용 절감** 네트워크 가상 어플라이언스(NVA), DNS 서버와 같이 여러 워크로드에서 공유할 수 있는 서비스를 하나의 위치로 일원화하여 비용을 절감합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-112">**Cost savings** by centralizing services that can be shared by multiple workloads, such as network virtual appliances (NVAs) and DNS servers, in a single location.</span></span>
- <span data-ttu-id="a70f1-113">**구독 제한 극복**: 여러 구독의 VNet을 중앙 허브로 피어링하여 구독의 제한을 극복합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-113">**Overcome subscriptions limits** by peering VNets from different subscriptions to the central hub.</span></span>
- <span data-ttu-id="a70f1-114">**문제 구분**: 중앙 IT(SecOps, InfraOps)와 워크로드(DevOps)를 문제를 분리합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-114">**Separation of concerns** between central IT (SecOps, InfraOps) and workloads (DevOps).</span></span>

<span data-ttu-id="a70f1-115">이 아키텍처의 일반적인 용도는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-115">Typical uses for this architecture include:</span></span>

- <span data-ttu-id="a70f1-116">개발, 테스트, 생산과 같이 서로 다른 환경에 배포되고 DNS, IDS, NTP 또는 AD DS와 같은 공유 서비스가 필요한 워크로드.</span><span class="sxs-lookup"><span data-stu-id="a70f1-116">Workloads deployed in different environments, such as development, testing, and production, that require shared services such as DNS, IDS, NTP, or AD DS.</span></span> <span data-ttu-id="a70f1-117">공유 서비스는 허브 VNet에 배치되고 각 환경은 스포크에 배포되어 격리 상태 유지.</span><span class="sxs-lookup"><span data-stu-id="a70f1-117">Shared services are placed in the hub VNet, while each environment is deployed to a spoke to maintain isolation.</span></span>
- <span data-ttu-id="a70f1-118">서로 연결할 필요가 없으나 공유 서비스에 대한 액세스가 필요한 워크로드.</span><span class="sxs-lookup"><span data-stu-id="a70f1-118">Workloads that do not require connectivity to each other, but require access to shared services.</span></span>
- <span data-ttu-id="a70f1-119">DMZ로 기능한 허브의 방화벽, 각 스포크에서 워크로드에 대한 별도의 관리 등 보안 측면에 대한 중앙 제어가 필요한 엔터프라이즈.</span><span class="sxs-lookup"><span data-stu-id="a70f1-119">Enterprises that require central control over security aspects, such as a firewall in the hub as a DMZ, and segregated management for the workloads in each spoke.</span></span>

## <a name="architecture"></a><span data-ttu-id="a70f1-120">아키텍처</span><span class="sxs-lookup"><span data-stu-id="a70f1-120">Architecture</span></span>

<span data-ttu-id="a70f1-121">이 아키텍처는 다음 구성 요소로 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-121">The architecture consists of the following components.</span></span>

- <span data-ttu-id="a70f1-122">**온-프레미스 네트워크**.</span><span class="sxs-lookup"><span data-stu-id="a70f1-122">**On-premises network**.</span></span> <span data-ttu-id="a70f1-123">조직 내에서 실행되는 개인 로컬 영역 네트워크입니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-123">A private local-area network running within an organization.</span></span>

- <span data-ttu-id="a70f1-124">**VPN 디바이스**.</span><span class="sxs-lookup"><span data-stu-id="a70f1-124">**VPN device**.</span></span> <span data-ttu-id="a70f1-125">온-프레미스 네트워크에 외부 연결을 제공하는 디바이스 또는 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-125">A device or service that provides external connectivity to the on-premises network.</span></span> <span data-ttu-id="a70f1-126">VPN 디바이스는 하드웨어 디바이스일 수도 있고 Windows Server 2012의 RRAS(라우팅 및 원격 액세스 서비스)와 같은 소프트웨어 솔루션일 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-126">The VPN device may be a hardware device, or a software solution such as the Routing and Remote Access Service (RRAS) in Windows Server 2012.</span></span> <span data-ttu-id="a70f1-127">지원되는 VPN 어플라이언스 목록 및 선택한 VPN 어플라이언스를 Azure에 연결하도록 구성하는 방법에 대한 자세한 내용은 [사이트 간 VPN Gateway 연결을 위한 VPN 디바이스 정보][vpn-appliance]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="a70f1-127">For a list of supported VPN appliances and information on configuring selected VPN appliances for connecting to Azure, see [About VPN devices for Site-to-Site VPN Gateway connections][vpn-appliance].</span></span>

- <span data-ttu-id="a70f1-128">**VPN 가상 네트워크 게이트웨이 또는 ExpressRoute 게이트웨이**.</span><span class="sxs-lookup"><span data-stu-id="a70f1-128">**VPN virtual network gateway or ExpressRoute gateway**.</span></span> <span data-ttu-id="a70f1-129">가상 네트워크 게이트웨이를 사용하면 VNet을 온-프레미스 네트워크에 연결하는 데 사용되는 VPN 디바이스 또는 ExpressRoute 회로에 연결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-129">The virtual network gateway enables the VNet to connect to the VPN device, or ExpressRoute circuit, used for connectivity with your on-premises network.</span></span> <span data-ttu-id="a70f1-130">자세한 내용은 [온-프레미스 네트워크를 Microsoft Azure virtual network에 연결][connect-to-an-Azure-vnet]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="a70f1-130">For more information, see [Connect an on-premises network to a Microsoft Azure virtual network][connect-to-an-Azure-vnet].</span></span>

> [!NOTE]
> <span data-ttu-id="a70f1-131">이 참조 아키텍처의 배포 스크립트는 연결을 위해 VPN 게이트웨이를 사용하고, 사용자의 온-프레미스 네트워크를 시뮬레이션하기 위해 Azure의 VNet을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-131">The deployment scripts for this reference architecture use a VPN gateway for connectivity, and a VNet in Azure to simulate your on-premises network.</span></span>

- <span data-ttu-id="a70f1-132">**허브 VNet**.</span><span class="sxs-lookup"><span data-stu-id="a70f1-132">**Hub VNet**.</span></span> <span data-ttu-id="a70f1-133">허브-스포크 토폴로지에서 허브로 사용되는 Azure VNet입니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-133">Azure VNet used as the hub in the hub-spoke topology.</span></span> <span data-ttu-id="a70f1-134">허브는 사용자의 온-프레미스 네트워크에 대한 연결의 중앙 위치이자 스포크 VNet에 호스팅된 다양한 워크로드에 의해 사용되는 서비스가 호스팅되는 장소입니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-134">The hub is the central point of connectivity to your on-premises network, and a place to host services that can be consumed by the different workloads hosted in the spoke VNets.</span></span>

- <span data-ttu-id="a70f1-135">**게이트웨이 서브넷**.</span><span class="sxs-lookup"><span data-stu-id="a70f1-135">**Gateway subnet**.</span></span> <span data-ttu-id="a70f1-136">가상 네트워크 게이트웨이는 동일한 서브넷에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-136">The virtual network gateways are held in the same subnet.</span></span>

- <span data-ttu-id="a70f1-137">**공유 서비스 서브넷**.</span><span class="sxs-lookup"><span data-stu-id="a70f1-137">**Shared services subnet**.</span></span> <span data-ttu-id="a70f1-138">DNS, AD DS를 비롯한 모든 스포크에서 공유될 수 있는 서비스를 호스팅하는 데 사용되는 허브 VNet의 서브넷입니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-138">A subnet in the hub VNet used to host services that can be shared among all spokes, such as DNS or AD DS.</span></span>

- <span data-ttu-id="a70f1-139">**DMZ 서브넷**.</span><span class="sxs-lookup"><span data-stu-id="a70f1-139">**DMZ subnet**.</span></span> <span data-ttu-id="a70f1-140">방화벽과 같은 보안 어플라이언스로 사용할 수 있는 NVA를 호스트하는 데 사용되는 허브 VNet의 서브넷입니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-140">A subnet in the hub VNet used to host NVAs that can act as security appliances, such as firewalls.</span></span>

- <span data-ttu-id="a70f1-141">**스포크 VNet**.</span><span class="sxs-lookup"><span data-stu-id="a70f1-141">**Spoke VNets**.</span></span> <span data-ttu-id="a70f1-142">허브-스포크 토폴로지에서 스포크로 사용되는 하나 이상의 Azure VNet입니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-142">One or more Azure VNets that are used as spokes in the hub-spoke topology.</span></span> <span data-ttu-id="a70f1-143">스포크는 다른 스포크와 별도로 관리되는 자체 VNet에서 워크로드를 격리하는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-143">Spokes can be used to isolate workloads in their own VNets, managed separately from other spokes.</span></span> <span data-ttu-id="a70f1-144">각 워크로드에는 Azure Load Balancer를 통해 여러 서브넷이 연결된 여러 계층이 포함될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-144">Each workload might include multiple tiers, with multiple subnets connected through Azure load balancers.</span></span> <span data-ttu-id="a70f1-145">애플리케이션 인프라에 대한 자세한 내용은 [Windows VM 워크로드 실행][windows-vm-ra] 및 [Linux VM 워크로드 실행][linux-vm-ra]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="a70f1-145">For more information about the application infrastructure, see [Running Windows VM workloads][windows-vm-ra] and [Running Linux VM workloads][linux-vm-ra].</span></span>

- <span data-ttu-id="a70f1-146">**VNet 피어링**.</span><span class="sxs-lookup"><span data-stu-id="a70f1-146">**VNet peering**.</span></span> <span data-ttu-id="a70f1-147">동일한 Azure 지역에 있는 2개의 VNet을 [피어링 연결][vnet-peering]을 사용하여 연결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-147">Two VNets in the same Azure region can be connected using a [peering connection][vnet-peering].</span></span> <span data-ttu-id="a70f1-148">피어링 연결은 VNet 사이에 적용되는 비전이적이고 대기 시간이 낮은 연결입니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-148">Peering connections are non-transitive, low latency connections between VNets.</span></span> <span data-ttu-id="a70f1-149">피어링이 적용되면 VNet은 라우터가 없어도 Azure 백본을 사용하여 트래픽을 교환합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-149">Once peered, the VNets exchange traffic by using the Azure backbone, without the need for a router.</span></span> <span data-ttu-id="a70f1-150">허브-스포크 네트워크 토폴로지에서는 VNet 피어링을 사용하여 허브를 각 스포크에 연결합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-150">In a hub-spoke network topology, you use VNet peering to connect the hub to each spoke.</span></span>

> [!NOTE]
> <span data-ttu-id="a70f1-151">이 문서에서는 [리소스 관리자](/azure/azure-resource-manager/resource-group-overview) 배포만 다루고 있지만, 동일한 구독에서 클래식 VNet을 리소스 관리자에 연결할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-151">This article only covers [Resource Manager](/azure/azure-resource-manager/resource-group-overview) deployments, but you can also connect a classic VNet to a Resource Manager VNet in the same subscription.</span></span> <span data-ttu-id="a70f1-152">이렇게 하면 스포크가 클래식 배포를 호스팅하면서도 허브에서 공유되는 서비스를 이용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-152">That way, your spokes can host classic deployments and still benefit from services shared in the hub.</span></span>

## <a name="recommendations"></a><span data-ttu-id="a70f1-153">권장 사항</span><span class="sxs-lookup"><span data-stu-id="a70f1-153">Recommendations</span></span>

<span data-ttu-id="a70f1-154">[hub-spoke][guidance-hub-spoke] 참조 아키텍처에 대한 모든 권장 사항은 공유 서비스 참조 아키텍처에도 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-154">All the recommendations for the [hub-spoke][guidance-hub-spoke] reference architecture also apply to the shared services reference architecture.</span></span>

<span data-ttu-id="a70f1-155">또한 다음에 나오는 권장 사항은 대부분의 공유 서비스 시나리오에 이 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-155">Also, the following recommendations apply for most scenarios under shared services.</span></span> <span data-ttu-id="a70f1-156">이러한 권장 사항을 재정의하라는 특정 요구 사항이 있는 경우가 아니면 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-156">Follow these recommendations unless you have a specific requirement that overrides them.</span></span>

### <a name="identity"></a><span data-ttu-id="a70f1-157">ID</span><span class="sxs-lookup"><span data-stu-id="a70f1-157">Identity</span></span>

<span data-ttu-id="a70f1-158">대부분의 엔터프라이즈 조직은 온-프레미스 데이터 센터에 ADDS(Active Directory 디렉터리 서비스) 환경을 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-158">Most enterprise organizations have an Active Directory Directory Services (ADDS) environment in their on-premises datacenter.</span></span> <span data-ttu-id="a70f1-159">ADDS를 사용하는 온-프레미스 네트워크에서 Azure로 이동된 자산 관리를 용이하게 하려면 Azure에서 ADDS 도메인 컨트롤러를 호스트하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-159">To facilitate management of assets moved to Azure from your on-premises network that depend on ADDS, it is recommended to host ADDS domain controllers in Azure.</span></span>

<span data-ttu-id="a70f1-160">Azure 및 온-프레미스 환경에서 개별적으로 제어하도록 그룹 정책 개체를 사용하는 경우 각 Azure 지역에 다른 AD 사이트를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-160">If you make use of Group Policy Objects, that you want to control separately for Azure and your on-premises environment, use a different AD site for each Azure region.</span></span> <span data-ttu-id="a70f1-161">종속 워크로드가 액세스할 수 있는 중앙 VNet(허브)에 도메인 컨트롤러를 배치합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-161">Place your domain controllers in a central VNet (hub) that dependent workloads can access.</span></span>

### <a name="security"></a><span data-ttu-id="a70f1-162">보안</span><span class="sxs-lookup"><span data-stu-id="a70f1-162">Security</span></span>

<span data-ttu-id="a70f1-163">온-프레미스 환경에서 Azure로 워크로드를 이동할 때 이러한 일부 워크로드는 VM에서 호스트되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-163">As you move workloads from your on-premises environment to Azure, some of these workloads will require to be hosted in VMs.</span></span> <span data-ttu-id="a70f1-164">규정 준수상 해당 워크로드를 트래버스하는 트래픽에 대한 제한 사항을 적용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-164">For compliance reasons, you may need to enforce restrictions on traffic traversing those workloads.</span></span>

<span data-ttu-id="a70f1-165">Azure에서 NVA(네트워크 가상 어플라이언스)를 사용하여 다른 형식의 보안 및 성능 서비스를 호스트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-165">You can use network virtual appliances (NVAs) in Azure to host different types of security and performance services.</span></span> <span data-ttu-id="a70f1-166">지정된 집합의 어플라이언스 온-프레미스에 익숙하다면 해당되는 경우 Azure에서 동일한 가상 어플라이언스를 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-166">If you are familiar with a given set of appliances on-premises today, it is recommended to use the same virtualized appliances in Azure, where applicable.</span></span>

> [!NOTE]
> <span data-ttu-id="a70f1-167">이 참조 아키텍처의 배포 스크립트는 네트워크 가상 어플라이언스를 모방하기 위해 사용하는 IP를 전달하여 Ubuntu VM을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-167">The deployment scripts for this reference architecture use an Ubuntu VM with IP forwarding enabled to mimic a network virtual appliance.</span></span>

## <a name="considerations"></a><span data-ttu-id="a70f1-168">고려 사항</span><span class="sxs-lookup"><span data-stu-id="a70f1-168">Considerations</span></span>

### <a name="overcoming-vnet-peering-limits"></a><span data-ttu-id="a70f1-169">VNet 피어링 제한 문제 해결</span><span class="sxs-lookup"><span data-stu-id="a70f1-169">Overcoming VNet peering limits</span></span>

<span data-ttu-id="a70f1-170">반드시 Azure의 [VNet 하나당 허용되는 VNet 피어링 개수 제한][vnet-peering-limit]을 고려해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-170">Make sure you consider the [limitation on number of VNets peerings per VNet][vnet-peering-limit] in Azure.</span></span> <span data-ttu-id="a70f1-171">허용되는 한도보다 많은 스포크가 필요한 경우 첫 번째 수준의 스포크가 허브로서 기능하는 허브-스포크-허브-스포크 토폴로지를 만드는 방법을 고려할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-171">If you decide you need more spokes than the limit will allow, consider creating a hub-spoke-hub-spoke topology, where the first level of spokes also act as hubs.</span></span> <span data-ttu-id="a70f1-172">다음 다이어그램은 이 토폴로지를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-172">The following diagram shows this approach.</span></span>

![Azure의 허브-스포크-허브-스포크 토폴로지](./images/hub-spokehub-spoke.svg)

<span data-ttu-id="a70f1-174">또한, 허브가 다수의 스포크에 맞게 확장될 수 있도록 허브에서 어떤 서비스가 공유되는지도 고려해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-174">Also consider what services are shared in the hub, to ensure the hub scales for a larger number of spokes.</span></span> <span data-ttu-id="a70f1-175">예를 들어 허브에서 방화벽 서비스를 제공한다면 복수의 스포크를 추가할 때 방화벽 솔루션의 대역폭 제한을 고려해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-175">For instance, if your hub provides firewall services, consider the bandwidth limits of your firewall solution when adding multiple spokes.</span></span> <span data-ttu-id="a70f1-176">공유 서비스 중 일부를 두 번째 수준의 허브로 이동하는 것이 좋을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-176">You might want to move some of these shared services to a second level of hubs.</span></span>

## <a name="deploy-the-solution"></a><span data-ttu-id="a70f1-177">솔루션 배포</span><span class="sxs-lookup"><span data-stu-id="a70f1-177">Deploy the solution</span></span>

<span data-ttu-id="a70f1-178">이 아키텍처에 대한 배포는 [GitHub][ref-arch-repo]에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-178">A deployment for this architecture is available on [GitHub][ref-arch-repo].</span></span> <span data-ttu-id="a70f1-179">배포는 구독에서 다음과 같은 리소스 그룹을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-179">The deployment creates the following resource groups in your subscription:</span></span>

- <span data-ttu-id="a70f1-180">hub-adds-rg</span><span class="sxs-lookup"><span data-stu-id="a70f1-180">hub-adds-rg</span></span>
- <span data-ttu-id="a70f1-181">hub-nva-rg</span><span class="sxs-lookup"><span data-stu-id="a70f1-181">hub-nva-rg</span></span>
- <span data-ttu-id="a70f1-182">hub-vnet-rg</span><span class="sxs-lookup"><span data-stu-id="a70f1-182">hub-vnet-rg</span></span>
- <span data-ttu-id="a70f1-183">onprem-vnet-rg</span><span class="sxs-lookup"><span data-stu-id="a70f1-183">onprem-vnet-rg</span></span>
- <span data-ttu-id="a70f1-184">spoke1-vnet-rg</span><span class="sxs-lookup"><span data-stu-id="a70f1-184">spoke1-vnet-rg</span></span>
- <span data-ttu-id="a70f1-185">spoke2-vnet-rg</span><span class="sxs-lookup"><span data-stu-id="a70f1-185">spoke2-vnet-rg</span></span>

<span data-ttu-id="a70f1-186">템플릿 매개 변수 파일은 이러한 이름을 참조하므로 이름을 변경하는 경우 매개 변수 파일을 일치하도록 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-186">The template parameter files refer to these names, so if you change them, update the parameter files to match.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="a70f1-187">필수 조건</span><span class="sxs-lookup"><span data-stu-id="a70f1-187">Prerequisites</span></span>

[!INCLUDE [ref-arch-prerequisites.md](../../../includes/ref-arch-prerequisites.md)]

### <a name="deploy-the-simulated-on-premises-datacenter-using-azbb"></a><span data-ttu-id="a70f1-188">azbb를 사용하여 시뮬레이션된 온-프레미스 데이터 센터 배포</span><span class="sxs-lookup"><span data-stu-id="a70f1-188">Deploy the simulated on-premises datacenter using azbb</span></span>

<span data-ttu-id="a70f1-189">이 단계에서는 시뮬레이션된 온-프레미스 데이터 센터를 Azure VNet으로서 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-189">This step deploys the simulated on-premises datacenter as an Azure VNet.</span></span>

1. <span data-ttu-id="a70f1-190">GitHub 리포지토리의 `hybrid-networking\shared-services-stack\` 폴더로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-190">Navigate to the `hybrid-networking\shared-services-stack\` folder of the GitHub repository.</span></span>

2. <span data-ttu-id="a70f1-191">`onprem.json` 파일을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-191">Open the `onprem.json` file.</span></span>

3. <span data-ttu-id="a70f1-192">`UserName`, `adminUserName`, `Password` 및 `adminPassword`의 모든 인스턴스를 검색합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-192">Search for all instances of `UserName`, `adminUserName`,`Password`, and `adminPassword`.</span></span> <span data-ttu-id="a70f1-193">매개 변수에서 사용자 이름과 암호에 값을 입력하고 파일을 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-193">Enter values for the user name and password in the parameters and save the file.</span></span>

4. <span data-ttu-id="a70f1-194">다음 명령 실행:</span><span class="sxs-lookup"><span data-stu-id="a70f1-194">Run the following command:</span></span>

   ```bash
   azbb -s <subscription_id> -g onprem-vnet-rg -l <location> -p onprem.json --deploy
   ```

5. <span data-ttu-id="a70f1-195">배포가 완료될 때까지 기다립니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-195">Wait for the deployment to finish.</span></span> <span data-ttu-id="a70f1-196">이 배포는 가상 네트워크, Windows를 실행하는 가상 머신 및 VPN 게이트웨이를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-196">This deployment creates a virtual network, a virtual machine running Windows, and a VPN gateway.</span></span> <span data-ttu-id="a70f1-197">VPN 게이트웨이의 생성이 완료되기까지 40분 이상이 걸릴 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-197">The VPN gateway creation can take more than 40 minutes to complete.</span></span>

### <a name="deploy-the-hub-vnet"></a><span data-ttu-id="a70f1-198">허브 VNet 배포</span><span class="sxs-lookup"><span data-stu-id="a70f1-198">Deploy the hub VNet</span></span>

<span data-ttu-id="a70f1-199">이 단계에서는 허브 VNet을 배포하고 시뮬레이션된 온-프레미스 VNet에 연결합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-199">This step deploys the hub VNet and connects it to the simulated on-premises VNet.</span></span>

1. <span data-ttu-id="a70f1-200">`hub-vnet.json` 파일을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-200">Open the `hub-vnet.json` file.</span></span>

2. <span data-ttu-id="a70f1-201">`adminPassword`를 검색하고 매개 변수에서 사용자 이름 및 암호를 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-201">Search for `adminPassword` and enter a user name and password in the parameters.</span></span>

3. <span data-ttu-id="a70f1-202">`sharedKey`의 모든 인스턴스를 검색하고 공유 키에 대한 값을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-202">Search for all instances of `sharedKey` and enter a value for a shared key.</span></span> <span data-ttu-id="a70f1-203">파일을 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-203">Save the file.</span></span>

   ```json
   "sharedKey": "abc123",
   ```

4. <span data-ttu-id="a70f1-204">다음 명령 실행:</span><span class="sxs-lookup"><span data-stu-id="a70f1-204">Run the following command:</span></span>

   ```bash
   azbb -s <subscription_id> -g hub-vnet-rg -l <location> -p hub-vnet.json --deploy
   ```

5. <span data-ttu-id="a70f1-205">배포가 완료될 때까지 기다립니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-205">Wait for the deployment to finish.</span></span> <span data-ttu-id="a70f1-206">이 배포는 가상 네트워크, 가상 머신, VPN 게이트웨이 및 이전 섹션에서 생성한 게이트웨이에 대한 연결을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-206">This deployment creates a virtual network, a virtual machine, a VPN gateway, and a connection to the gateway created in the previous section.</span></span> <span data-ttu-id="a70f1-207">VPN 게이트웨이를 완료하는 데 40분 이상이 걸릴 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-207">The VPN gateway can take more than 40 minutes to complete.</span></span>

### <a name="deploy-ad-ds-in-azure"></a><span data-ttu-id="a70f1-208">Azure에서 AD DS 배포</span><span class="sxs-lookup"><span data-stu-id="a70f1-208">Deploy AD DS in Azure</span></span>

<span data-ttu-id="a70f1-209">이 단계에서는 Azure에서 AD DS 도메인 컨트롤러를 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-209">This step deploys AD DS domain controllers in Azure.</span></span>

1. <span data-ttu-id="a70f1-210">`hub-adds.json` 파일을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-210">Open the `hub-adds.json` file.</span></span>

2. <span data-ttu-id="a70f1-211">`Password` 및 `adminPassword`의 모든 인스턴스를 검색합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-211">Search for all instances of `Password` and `adminPassword`.</span></span> <span data-ttu-id="a70f1-212">매개 변수에서 사용자 이름과 암호에 값을 입력하고 파일을 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-212">Enter values for the user name and password in the parameters and save the file.</span></span>

3. <span data-ttu-id="a70f1-213">다음 명령 실행:</span><span class="sxs-lookup"><span data-stu-id="a70f1-213">Run the following command:</span></span>

   ```bash
   azbb -s <subscription_id> -g hub-adds-rg -l <location> -p hub-adds.json --deploy
   ```

<span data-ttu-id="a70f1-214">시뮬레이션된 온-프레미스 데이터 센터에서 호스팅되는 도메인에 두 VM을 조인시키고 AD DS를 설치하기 때문에 이 배포 단계는 몇 분 정도 걸릴 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-214">This deployment step may take several minutes, because it joins the two VMs to the domain hosted in the simulated on-premises datacenter, and installs AD DS on them.</span></span>

### <a name="deploy-the-spoke-vnets"></a><span data-ttu-id="a70f1-215">스포크 VNet 배포</span><span class="sxs-lookup"><span data-stu-id="a70f1-215">Deploy the spoke VNets</span></span>

<span data-ttu-id="a70f1-216">이 단계에서는 스포크 VNet을 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-216">This step deploys the spoke VNets.</span></span>

1. <span data-ttu-id="a70f1-217">`spoke1.json` 파일을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-217">Open the `spoke1.json` file.</span></span>

2. <span data-ttu-id="a70f1-218">`adminPassword`를 검색하고 매개 변수에서 사용자 이름 및 암호를 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-218">Search for `adminPassword` and enter a user name and password in the parameters.</span></span>

3. <span data-ttu-id="a70f1-219">다음 명령 실행:</span><span class="sxs-lookup"><span data-stu-id="a70f1-219">Run the following command:</span></span>

   ```bash
   azbb -s <subscription_id> -g spoke1-vnet-rg -l <location> -p spoke1.json --deploy
   ```

4. <span data-ttu-id="a70f1-220">`spoke2.json` 파일에 대해 1 및 2단계 반복합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-220">Repeat steps 1 and 2 for the file `spoke2.json`.</span></span>

5. <span data-ttu-id="a70f1-221">다음 명령 실행:</span><span class="sxs-lookup"><span data-stu-id="a70f1-221">Run the following command:</span></span>

   ```bash
   azbb -s <subscription_id> -g spoke2-vnet-rg -l <location> -p spoke2.json --deploy
   ```

### <a name="peer-the-hub-vnet-to-the-spoke-vnets"></a><span data-ttu-id="a70f1-222">허브 VNet과 스포크 VNet 간 피어링</span><span class="sxs-lookup"><span data-stu-id="a70f1-222">Peer the hub VNet to the spoke VNets</span></span>

<span data-ttu-id="a70f1-223">허브 VNet에서 스포크 VNet으로의 피어링을 연결하려면 다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-223">To create a peering connection from the hub VNet to the spoke VNets, run the following command:</span></span>

```bash
azbb -s <subscription_id> -g hub-vnet-rg -l <location> -p hub-vnet-peering.json --deploy
```

### <a name="deploy-the-nva"></a><span data-ttu-id="a70f1-224">NVA 배포</span><span class="sxs-lookup"><span data-stu-id="a70f1-224">Deploy the NVA</span></span>

<span data-ttu-id="a70f1-225">이 단계에서는 `dmz` 서브넷에서 NVA를 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-225">This step deploys an NVA in the `dmz` subnet.</span></span>

1. <span data-ttu-id="a70f1-226">`hub-nva.json` 파일을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-226">Open the `hub-nva.json` file.</span></span>

2. <span data-ttu-id="a70f1-227">`adminPassword`를 검색하고 매개 변수에서 사용자 이름 및 암호를 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-227">Search for `adminPassword` and enter a user name and password in the parameters.</span></span>

3. <span data-ttu-id="a70f1-228">다음 명령 실행:</span><span class="sxs-lookup"><span data-stu-id="a70f1-228">Run the following command:</span></span>

   ```bash
   azbb -s <subscription_id> -g hub-nva-rg -l <location> -p hub-nva.json --deploy
   ```

### <a name="test-connectivity"></a><span data-ttu-id="a70f1-229">연결 테스트</span><span class="sxs-lookup"><span data-stu-id="a70f1-229">Test connectivity</span></span>

<span data-ttu-id="a70f1-230">시뮬레이션된 온-프레미스 환경에서 허브 VNet으로의 연결을 테스트합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-230">Test conectivity from the simulated on-premises environment to the hub VNet.</span></span>

1. <span data-ttu-id="a70f1-231">Azure Portal을 사용하여 `onprem-jb-rg` 리소스 그룹에서 `jb-vm1`이라는 VM을 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-231">Use the Azure portal to find the VM named `jb-vm1` in the `onprem-jb-rg` resource group.</span></span>

2. <span data-ttu-id="a70f1-232">`Connect`를 클릭하여 VM에 대한 원격 데스크톱 세션을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-232">Click `Connect` to open a remote desktop session to the VM.</span></span> <span data-ttu-id="a70f1-233">`onprem.json` 매개 변수 파일에서 지정한 암호를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-233">Use the password that you specified in the `onprem.json` parameter file.</span></span>

3. <span data-ttu-id="a70f1-234">VM에서 PowerShell 콘솔을 열고, `Test-NetConnection` cmdlet을 사용하여 허브 VNet에서 jumpbox VM에 연결할 수 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-234">Open a PowerShell console in the VM, and use the `Test-NetConnection` cmdlet to verify that you can connect to the jumpbox VM in the hub VNet.</span></span>

   ```powershell
   Test-NetConnection 10.0.0.68 -CommonTCPPort RDP
   ```

<span data-ttu-id="a70f1-235">출력은 다음과 비슷해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-235">The output should look similar to the following:</span></span>

```powershell
ComputerName     : 10.0.0.68
RemoteAddress    : 10.0.0.68
RemotePort       : 3389
InterfaceAlias   : Ethernet 2
SourceAddress    : 192.168.1.000
TcpTestSucceeded : True
```

> [!NOTE]
> <span data-ttu-id="a70f1-236">기본적으로 Windows Server VM을 사용하면 Azure에서 ICMP 응답을 허용하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-236">By default, Windows Server VMs do not allow ICMP responses in Azure.</span></span> <span data-ttu-id="a70f1-237">`ping`을 사용하여 연결을 테스트하려는 경우 각 VM에 대한 Windows 고급 방화벽에서 ICMP 트래픽을 사용하도록 설정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-237">If you want to use `ping` to test connectivity, you need to enable ICMP traffic in the Windows Advanced Firewall for each VM.</span></span>

<span data-ttu-id="a70f1-238">같은 단계를 반복하여 스포크 VNet에 대한 연결을 테스트합니다.</span><span class="sxs-lookup"><span data-stu-id="a70f1-238">Repeat the sames steps to test connectivity to the spoke VNets:</span></span>

```powershell
Test-NetConnection 10.1.0.68 -CommonTCPPort RDP
Test-NetConnection 10.2.0.68 -CommonTCPPort RDP
```

<!-- links -->

[azure-cli-2]: /azure/install-azure-cli
[azbb]: https://github.com/mspnp/template-building-blocks/wiki/Install-Azure-Building-Blocks
[guidance-hub-spoke]: ./hub-spoke.md
[azure-vpn-gateway]: /azure/vpn-gateway/vpn-gateway-about-vpngateways
[best-practices-security]: /azure/best-practices-network-securit
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[guidance-expressroute]: ./expressroute.md
[guidance-vpn]: ./vpn.md
[linux-vm-ra]: ../virtual-machines-linux/index.md
[hybrid-ha]: ./expressroute-vpn-failover.md
[naming conventions]: /azure/guidance/guidance-naming-conventions
[resource-manager-overview]: /azure/azure-resource-manager/resource-group-overview
[가상 데이터 센터]: https://aka.ms/vdc
[virtual datacenter]: https://aka.ms/vdc
[vnet-peering]: /azure/virtual-network/virtual-network-peering-overview
[vnet-peering-limit]: /azure/azure-subscription-service-limits#networking-limits
[vpn-appliance]: /azure/vpn-gateway/vpn-gateway-about-vpn-devices
[windows-vm-ra]: ../virtual-machines-windows/index.md

[visio-download]: https://archcenter.blob.core.windows.net/cdn/hybrid-network-hub-spoke.vsdx
[ref-arch-repo]: https://github.com/mspnp/reference-architectures
