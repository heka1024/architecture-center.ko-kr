---
title: Windows VM으로 안전한 웹앱 빌드
titleSuffix: Azure Example Scenarios
description: 확장 집합, Application Gateway 및 부하 분산 장치를 사용하여 Azure의 Windows Server에서 안전한 다중 계층 웹 애플리케이션을 빌드합니다.
author: iainfoulds
ms.date: 12/06/2018
ms.topic: example-scenario
ms.service: architecture-center
ms.subservice: example-scenario
ms.custom: seodec18, Windows
ms.openlocfilehash: 12c7b4749507d4b96e5ce43f98739885c8133e7e
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54485539"
---
# <a name="building-secure-web-applications-with-windows-virtual-machines-on-azure"></a><span data-ttu-id="bfea8-103">Azure에서 Windows 가상 머신으로 안전한 웹 애플리케이션 빌드</span><span class="sxs-lookup"><span data-stu-id="bfea8-103">Building secure web applications with Windows virtual machines on Azure</span></span>

<span data-ttu-id="bfea8-104">이 시나리오는 Microsoft Azure에서 안전한 다중 계층 웹 애플리케이션을 실행하기 위한 아키텍처 및 설계 지침을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-104">This scenario provides architecture and design guidance for running secure, multi-tier web applications on Microsoft Azure.</span></span> <span data-ttu-id="bfea8-105">이 예제의 ASP.NET 애플리케이션은 가상 머신을 사용하여 보호되는 백 엔드 Microsoft SQL Server 클러스터에 안전하게 연결합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-105">In this example, an ASP.NET application securely connects to a protected back-end Microsoft SQL Server cluster using virtual machines.</span></span>

<span data-ttu-id="bfea8-106">기존에는 조직에서 안전한 인프라를 제공하기 위해 레거시 온-프레미스 애플리케이션 및 서비스를 유지해야 했습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-106">Traditionally, organizations had to maintain legacy on-premises applications and services to provide a secure infrastructure.</span></span> <span data-ttu-id="bfea8-107">조직은 Azure에서 Windows Server 애플리케이션을 안전하게 배포하여 배포를 현대화하고 온-프레미스 운영 비용 및 관리 오버헤드를 줄일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-107">By deploying these Windows Server applications securely in Azure, organizations can modernize their deployments and reduce their on-premises operating costs and management overhead.</span></span>

## <a name="relevant-use-cases"></a><span data-ttu-id="bfea8-108">관련 사용 사례</span><span class="sxs-lookup"><span data-stu-id="bfea8-108">Relevant use cases</span></span>

<span data-ttu-id="bfea8-109">다음은 이 시나리오를 적용할 수 있는 몇 가지 예입니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-109">A few examples of where this scenario may apply:</span></span>

- <span data-ttu-id="bfea8-110">보안 클라우드 환경에서 애플리케이션 배포 현대화</span><span class="sxs-lookup"><span data-stu-id="bfea8-110">Modernizing application deployments in a secure cloud environment.</span></span>
- <span data-ttu-id="bfea8-111">레거시 온-프레미스 애플리케이션 및 서비스의 관리 오버헤드 줄이기</span><span class="sxs-lookup"><span data-stu-id="bfea8-111">Reducing the management overhead of legacy on-premises applications and services.</span></span>
- <span data-ttu-id="bfea8-112">새 애플리케이션 플랫폼을 통해 환자에 대한 의료 서비스 및 환경 개선</span><span class="sxs-lookup"><span data-stu-id="bfea8-112">Improving patient healthcare and experience with new application platforms.</span></span>

## <a name="architecture"></a><span data-ttu-id="bfea8-113">아키텍처</span><span class="sxs-lookup"><span data-stu-id="bfea8-113">Architecture</span></span>

![규제 산업용 다중 계층 Windows Server 애플리케이션과 관련된 Azure 구성 요소 아키텍처에 대한 개요][architecture]

<span data-ttu-id="bfea8-115">이 시나리오는 백 엔드 데이터베이스에 연결하는 프런트 엔드 웹 애플리케이션을 보여주며, 둘 다 Windows Server 2016에서 실행됩니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-115">This scenario shows a front-end web application connecting to a back-end database, both running on Windows Server 2016.</span></span> <span data-ttu-id="bfea8-116">시나리오를 통한 데이터 흐름은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-116">The data flows through the scenario as follows:</span></span>

1. <span data-ttu-id="bfea8-117">사용자가 Azure Application Gateway를 통해 프런트 엔드 ASP.NET 애플리케이션에 액세스합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-117">Users access the front-end ASP.NET application through an Azure Application Gateway.</span></span>
2. <span data-ttu-id="bfea8-118">Application Gateway에서 Azure 가상 머신 확장 집합 내의 VM 인스턴스에 트래픽을 분산시킵니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-118">The Application Gateway distributes traffic to VM instances within an Azure virtual machine scale set.</span></span>
3. <span data-ttu-id="bfea8-119">애플리케이션이 Azure 부하 분산 장치를 통해 백 엔드 계층의 Microsoft SQL Server 클러스터에 연결합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-119">The application connects to Microsoft SQL Server cluster in a back-end tier via an Azure load balancer.</span></span> <span data-ttu-id="bfea8-120">이러한 백 엔드 SQL Server 인스턴스는 별도의 Azure 가상 네트워크에 있으며 트래픽 흐름을 제한하는 네트워크 보안 그룹 규칙으로 보호됩니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-120">These back-end SQL Server instances are in a separate Azure virtual network, secured by network security group rules that limit traffic flow.</span></span>
4. <span data-ttu-id="bfea8-121">부하 분산 장치에서 SQL Server 트래픽을 다른 가상 머신 확장 집합의 VM 인스턴스에 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-121">The load balancer distributes SQL Server traffic to VM instances in another virtual machine scale set.</span></span>
5. <span data-ttu-id="bfea8-122">Azure Blob Storage는 백 엔드 계층의 SQL Server 클러스터에 대한 [클라우드 감시][cloud-witness] 역할을 합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-122">Azure Blob Storage acts as a [cloud witness][cloud-witness] for the SQL Server cluster in the back-end tier.</span></span> <span data-ttu-id="bfea8-123">VNet 내에서의 연결은 Azure Storage에 대한 VNet 서비스 엔드포인트를 통해 가능합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-123">The connection from within the VNet is enabled with a VNet Service Endpoint for Azure Storage.</span></span>

### <a name="components"></a><span data-ttu-id="bfea8-124">구성 요소</span><span class="sxs-lookup"><span data-stu-id="bfea8-124">Components</span></span>

- <span data-ttu-id="bfea8-125">[Azure Application Gateway][appgateway-docs]는 애플리케이션을 인식하고 특정 라우팅 규칙에 따라 트래픽을 분산할 수 있는 7계층 웹 트래픽 부하 분산 장치입니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-125">[Azure Application Gateway][appgateway-docs] is a layer 7 web traffic load balancer that is application-aware and can distribute traffic based on specific routing rules.</span></span> <span data-ttu-id="bfea8-126">또한 App Gateway는 향상된 웹 서버 성능을 위해 SSL 오프로드를 처리할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-126">App Gateway can also handle SSL offloading for improved web server performance.</span></span>
- <span data-ttu-id="bfea8-127">[Azure Virtual Network][vnet-docs]는 VM과 같은 리소스에서 상호 간 통신, 인터넷 통신 및 온-프레미스 네트워크 통신을 안전하게 수행할 수 있게 합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-127">[Azure Virtual Network][vnet-docs] allows resources such as VMs to securely communicate with each other, the Internet, and on-premises networks.</span></span> <span data-ttu-id="bfea8-128">가상 네트워크는 격리 및 세분화를 제공하고, 트래픽을 필터링 및 라우팅하며, 위치 간 연결을 허용합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-128">Virtual networks provide isolation and segmentation, filter and route traffic, and allow connection between locations.</span></span> <span data-ttu-id="bfea8-129">이 시나리오에서는 적절한 NSG와 결합된 두 가상 네트워크를 사용하여 [DMZ][dmz](완충 영역) 및 애플리케이션 구성 요소 격리를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-129">Two virtual networks combined with the appropriate NSGs are used in this scenario to provide a [demilitarized zone][dmz] (DMZ) and isolation of the application components.</span></span> <span data-ttu-id="bfea8-130">가상 네트워크 피어링은 두 네트워크를 서로 연결합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-130">Virtual network peering connects the two networks together.</span></span>
- <span data-ttu-id="bfea8-131">[Azure 가상 머신 확장 집합][scaleset-docs]을 사용하면 부하 분산된 동일한 VM 그룹을 만들고 관리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-131">[Azure virtual machine scale set][scaleset-docs] lets you create and manager a group of identical, load balanced, VMs.</span></span> <span data-ttu-id="bfea8-132">VM 인스턴스의 수는 요구 또는 정의된 일정에 따라 자동으로 늘리거나 줄일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-132">The number of VM instances can automatically increase or decrease in response to demand or a defined schedule.</span></span> <span data-ttu-id="bfea8-133">이 시나리오에서는 각각 프런트 엔드 ASP.NET 애플리케이션 인스턴스 및 백 엔드 SQL Server 클러스터 VM 인스턴스에 대한 별도의 두 가상 머신 확장 집합이 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-133">Two separate virtual machine scale sets are used in this scenario - one for the front-end ASP.NET application instances, and one for the back-end SQL Server cluster VM instances.</span></span> <span data-ttu-id="bfea8-134">PowerShell DSC(Desired State Configuration) 또는 Azure 사용자 지정 스크립트 확장을 사용하여 VM 인스턴스에 필요한 소프트웨어 및 구성 설정을 프로비전할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-134">PowerShell desired state configuration (DSC) or the Azure custom script extension can be used to provision the VM instances with the required software and configuration settings.</span></span>
- <span data-ttu-id="bfea8-135">[Azure 네트워크 보안 그룹][nsg-docs]에는 원본 또는 대상 IP 주소, 포트 및 프로토콜에 따라 인바운드 또는 아웃바운드 네트워크 트래픽을 허용하거나 거부하는 보안 규칙 목록이 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-135">[Azure network security groups][nsg-docs] contain a list of security rules that allow or deny inbound or outbound network traffic based on source or destination IP address, port, and protocol.</span></span> <span data-ttu-id="bfea8-136">이 시나리오의 가상 네트워크는 애플리케이션 구성 요소 간의 트래픽 흐름을 제한하는 네트워크 보안 그룹 규칙으로 보호됩니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-136">The virtual networks in this scenario are secured with network security group rules that restrict the flow of traffic between the application components.</span></span>
- <span data-ttu-id="bfea8-137">[Azure 부하 분산 장치][loadbalancer-docs]는 규칙 및 상태 프로브에 따라 인바운드 트래픽을 분산시킵니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-137">[Azure load balancer][loadbalancer-docs] distributes inbound traffic according to rules and health probes.</span></span> <span data-ttu-id="bfea8-138">부하 분산 장치는 짧은 대기 시간과 높은 처리량을 제공하고, 모든 TCP 및 UDP 애플리케이션에 대해 최대 수백만 개의 흐름으로 확장합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-138">A load balancer provides low latency and high throughput, and scales up to millions of flows for all TCP and UDP applications.</span></span> <span data-ttu-id="bfea8-139">이 시나리오에서는 내부 부하 분산 장치를 사용하여 프런트 엔드 애플리케이션 계층에서 백 엔드 SQL Server 클러스터로 트래픽을 분산시킵니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-139">An internal load balancer is used in this scenario to distribute traffic from the front-end application tier to the back-end SQL Server cluster.</span></span>
- <span data-ttu-id="bfea8-140">[Azure Blob Storage][cloudwitness-docs]는 SQL Server 클러스터에 대한 클라우드 감시 위치 역할을 합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-140">[Azure Blob Storage][cloudwitness-docs] acts a Cloud Witness location for the SQL Server cluster.</span></span> <span data-ttu-id="bfea8-141">이 감시는 쿼럼을 결정하기 위해 추가 투표가 필요한 클러스터 작업 및 의사 결정에 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-141">This witness is used for cluster operations and decisions that require an additional vote to decide quorum.</span></span> <span data-ttu-id="bfea8-142">클라우드 감시를 사용하면 추가 VM에서 기존 파일 공유 감시 역할을 수행할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-142">Using Cloud Witness removes the need for an additional VM to act as a traditional File Share Witness.</span></span>

### <a name="alternatives"></a><span data-ttu-id="bfea8-143">대안</span><span class="sxs-lookup"><span data-stu-id="bfea8-143">Alternatives</span></span>

- <span data-ttu-id="bfea8-144">인프라가 운영 체제에 종속되지 않으므로 Linux와 Windows를 서로 바꿔서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-144">Linux and Windows can be used interchangeably since the infrastructure isn't dependent on the operating system.</span></span>

- <span data-ttu-id="bfea8-145">[Linux용 SQL Server][sql-linux]는 백 엔드 데이터 저장소를 대체할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-145">[SQL Server for Linux][sql-linux] can replace the back-end data store.</span></span>

- <span data-ttu-id="bfea8-146">[Cosmos DB](/azure/cosmos-db/introduction)는 데이터 저장소의 또 다른 대안입니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-146">[Cosmos DB](/azure/cosmos-db/introduction) is another alternative for the data store.</span></span>

## <a name="considerations"></a><span data-ttu-id="bfea8-147">고려 사항</span><span class="sxs-lookup"><span data-stu-id="bfea8-147">Considerations</span></span>

### <a name="availability"></a><span data-ttu-id="bfea8-148">가용성</span><span class="sxs-lookup"><span data-stu-id="bfea8-148">Availability</span></span>

<span data-ttu-id="bfea8-149">이 시나리오의 VM 인스턴스는 [가용성 영역](/azure/availability-zones/az-overview) 전체에 배포됩니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-149">The VM instances in this scenario are deployed across [Availability Zones](/azure/availability-zones/az-overview).</span></span> <span data-ttu-id="bfea8-150">각 영역은 독립된 전원, 냉각 및 네트워킹을 갖춘 하나 이상의 데이터 센터로 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-150">Each zone is made up of one or more datacenters equipped with independent power, cooling, and networking.</span></span> <span data-ttu-id="bfea8-151">설정된 각 지역에는 최소한 세 개의 가용성 영역이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-151">Each enabled region has a minimum of three availability zones.</span></span> <span data-ttu-id="bfea8-152">영역을 통한 이러한 VM 인스턴스 배포는 애플리케이션 계층에 고가용성을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-152">This distribution of VM instances across zones provides high availability to the application tiers.</span></span>

<span data-ttu-id="bfea8-153">데이터베이스 계층에서는 Always On 가용성 그룹을 사용하도록 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-153">The database tier can be configured to use Always On availability groups.</span></span> <span data-ttu-id="bfea8-154">이 SQL Server 구성을 사용하면 클러스터 내에서 하나의 주 데이터베이스가 최대 8개의 보조 데이터베이스로 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-154">With this SQL Server configuration, one primary database within a cluster is configured with up to eight secondary databases.</span></span> <span data-ttu-id="bfea8-155">주 데이터베이스에 문제가 발생하면 클러스터에서 보조 데이터베이스 중 하나에 장애 조치하여 애플리케이션을 계속 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-155">If an issue occurs with the primary database, the cluster fails over to one of the secondary databases, which allows the application to continue to be available.</span></span> <span data-ttu-id="bfea8-156">자세한 내용은 [SQL Server에 대한 Always On 가용성 그룹 개요][sqlalwayson-docs]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="bfea8-156">For more information, see [Overview of Always On availability groups for SQL Server][sqlalwayson-docs].</span></span>

<span data-ttu-id="bfea8-157">가용성 지침을 더 보려면 Azure 아키텍처 센터의 [가용성 검사 목록][availability]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="bfea8-157">For more availability guidance, see the [availability checklist][availability] in the Azure Architecture Center.</span></span>

### <a name="scalability"></a><span data-ttu-id="bfea8-158">확장성</span><span class="sxs-lookup"><span data-stu-id="bfea8-158">Scalability</span></span>

<span data-ttu-id="bfea8-159">이 시나리오에서는 프론트 엔드 및 백 엔드 구성 요소에 대한 가상 머신 확장 집합을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-159">This scenario uses virtual machine scale sets for the front-end and back-end components.</span></span> <span data-ttu-id="bfea8-160">확장 집합을 사용하면 프론트 엔드 애플리케이션 계층을 실행하는 VM 인스턴스의 수를 고객 요구 또는 정의된 일정에 따라 자동으로 조정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-160">With scale sets, the number of VM instances that run the front-end application tier can automatically scale in response to customer demand, or based on a defined schedule.</span></span> <span data-ttu-id="bfea8-161">자세한 내용은 [가상 머신 확장 집합을 사용한 자동 크기 조정 개요][vmssautoscale-docs]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="bfea8-161">For more information, see [Overview of autoscale with virtual machine scale sets][vmssautoscale-docs].</span></span>

<span data-ttu-id="bfea8-162">다른 확장성 항목에 대해서는 Azure 아키텍처 센터의 [확장성 검사 목록][scalability]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="bfea8-162">For other scalability topics, see the [scalability checklist][scalability] in the Azure Architecture Center.</span></span>

### <a name="security"></a><span data-ttu-id="bfea8-163">보안</span><span class="sxs-lookup"><span data-stu-id="bfea8-163">Security</span></span>

<span data-ttu-id="bfea8-164">프론트 엔드 애플리케이션 계층으로 전송되는 모든 가상 네트워크 트래픽은 네트워크 보안 그룹을 통해 보호됩니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-164">All the virtual network traffic into the front-end application tier and protected by network security groups.</span></span> <span data-ttu-id="bfea8-165">규칙은 프런트 엔드 애플리케이션 계층 VM 인스턴스만 백 엔드 데이터베이스 계층에 액세스할 수 있도록 트래픽 흐름을 제한합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-165">Rules limit the flow of traffic so that only the front-end application tier VM instances can access the back-end database tier.</span></span> <span data-ttu-id="bfea8-166">데이터베이스 계층에는 아웃바운드 인터넷 트래픽이 허용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-166">No outbound Internet traffic is allowed from the database tier.</span></span> <span data-ttu-id="bfea8-167">공격 공간을 줄이기 위해 직접 원격 관리 포트가 열려 있지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-167">To reduce the attack footprint, no direct remote management ports are open.</span></span> <span data-ttu-id="bfea8-168">자세한 내용은 [Azure 네트워크 보안 그룹][nsg-docs]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="bfea8-168">For more information, see [Azure network security groups][nsg-docs].</span></span>

<span data-ttu-id="bfea8-169">PCI DSS(지불 카드 산업 데이터 보안 표준) 3.2 규정 준수 인프라 배포에 대한 지침을 보려면 [규정 준수 인프라][pci-dss]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="bfea8-169">To view guidance on deploying Payment Card Industry Data Security Standards (PCI DSS 3.2) [compliant infrastructure][pci-dss].</span></span> <span data-ttu-id="bfea8-170">보안 시나리오 설계에 대한 일반적인 지침은 [Azure 보안 설명서][security]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="bfea8-170">For general guidance on designing secure scenarios, see the [Azure Security Documentation][security].</span></span>

### <a name="resiliency"></a><span data-ttu-id="bfea8-171">복원력</span><span class="sxs-lookup"><span data-stu-id="bfea8-171">Resiliency</span></span>

<span data-ttu-id="bfea8-172">이 시나리오에서는 가용성 영역 및 가상 머신 확장 집합을 사용할 뿐만 아니라 Azure Application Gateway 및 부하 분산 장치도 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-172">In combination with the use of Availability Zones and virtual machine scale sets, this scenario uses Azure Application Gateway and load balancer.</span></span> <span data-ttu-id="bfea8-173">이러한 두 네트워킹 구성 요소는 연결된 VM 인스턴스에 트래픽을 분산시키고, 트래픽이 정상 VM에만 분산되도록 하는 상태 프로브를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-173">These two networking components distribute traffic to the connected VM instances, and include health probes that ensure traffic is only distributed to healthy VMs.</span></span> <span data-ttu-id="bfea8-174">두 Application Gateway 인스턴스가 활성-수동 구성으로 구성되고, 영역 중복 부하 분산 장치가 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-174">Two Application Gateway instances are configured in an active-passive configuration, and a zone-redundant load balancer is used.</span></span> <span data-ttu-id="bfea8-175">이 구성을 사용하면 트래픽을 중단시키고 최종 사용자 액세스에 영향을 미칠 수 있는 문제로부터 네트워킹 리소스와 애플리케이션을 탄력적으로 복원할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-175">This configuration makes the networking resources and application resilient to issues that would otherwise disrupt traffic and impact end-user access.</span></span>

<span data-ttu-id="bfea8-176">복원력 있는 시나리오 설계에 대한 일반적인 지침은 [Azure용 복원 애플리케이션 디자인][resiliency]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="bfea8-176">For general guidance on designing resilient scenarios, see [Designing resilient applications for Azure][resiliency].</span></span>

## <a name="deploy-the-scenario"></a><span data-ttu-id="bfea8-177">시나리오 배포</span><span class="sxs-lookup"><span data-stu-id="bfea8-177">Deploy the scenario</span></span>

### <a name="prerequisites"></a><span data-ttu-id="bfea8-178">필수 조건</span><span class="sxs-lookup"><span data-stu-id="bfea8-178">Prerequisites</span></span>

- <span data-ttu-id="bfea8-179">기존 Azure 계정이 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-179">You must have an existing Azure account.</span></span> <span data-ttu-id="bfea8-180">Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-180">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

- <span data-ttu-id="bfea8-181">SQL Server 클러스터를 백 엔드 확장 집합에 배포하려면 Azure AD(Active Directory) 도메인 서비스의 도메인이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-181">To deploy a SQL Server cluster into the back-end scale set, you would need a domain in Azure Active Directory (AD) Domain Services.</span></span>

### <a name="deploy-the-components"></a><span data-ttu-id="bfea8-182">구성 요소 배포</span><span class="sxs-lookup"><span data-stu-id="bfea8-182">Deploy the components</span></span>

<span data-ttu-id="bfea8-183">Azure Resource Manager 템플릿을 사용하여 이 시나리오에 대한 핵심 인프라를 배포하려면 다음 단계를 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-183">To deploy the core infrastructure for this scenario with an Azure Resource Manager template, perform the following steps.</span></span>

<!-- markdownlint-disable MD033 -->

1. <span data-ttu-id="bfea8-184">**Azure에 배포** 단추를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-184">Select the **Deploy to Azure** button:</span></span><br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Fsolution-architectures%2Fmaster%2Finfrastructure%2Fregulated-multitier-app.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>
2. <span data-ttu-id="bfea8-185">Azure Portal에서 템플릿 배포가 열릴 때까지 기다린 후에 다음 단계를 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-185">Wait for the template deployment to open in the Azure portal, then complete the following steps:</span></span>
   - <span data-ttu-id="bfea8-186">리소스 그룹 **새로 만들기**를 선택한 다음, 텍스트 상자에서 이름(예: *myWindowsscenario*)을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-186">Choose to **Create new** resource group, then provide a name such as *myWindowsscenario* in the text box.</span></span>
   - <span data-ttu-id="bfea8-187">**위치** 드롭다운 상자에서 지역을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-187">Select a region from the **Location** drop-down box.</span></span>
   - <span data-ttu-id="bfea8-188">가상 머신 확장 집합 인스턴스에 대한 사용자 이름과 보안 암호를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-188">Provide a username and secure password for the virtual machine scale set instances.</span></span>
   - <span data-ttu-id="bfea8-189">사용 약관을 검토한 다음, **위에 명시된 사용 약관에 동의함**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-189">Review the terms and conditions, then check **I agree to the terms and conditions stated above**.</span></span>
   - <span data-ttu-id="bfea8-190">**구매** 단추를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-190">Select the **Purchase** button.</span></span>

<!-- markdownlint-enable MD033 -->

<span data-ttu-id="bfea8-191">배포가 완료되는 데 15-20분이 걸릴 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-191">It can take 15-20 minutes for the deployment to complete.</span></span>

## <a name="pricing"></a><span data-ttu-id="bfea8-192">가격</span><span class="sxs-lookup"><span data-stu-id="bfea8-192">Pricing</span></span>

<span data-ttu-id="bfea8-193">이 시나리오를 실행하는 데 들어가는 비용을 알아보기 위해 모든 서비스가 비용 계산기에서 미리 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-193">To explore the cost of running this scenario, all of the services are pre-configured in the cost calculator.</span></span> <span data-ttu-id="bfea8-194">특정 사용 사례에 대한 가격이 변경되는 정도를 확인하려면 필요한 트래픽에 맞게 적절한 변수를 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-194">To see how the pricing would change for your particular use case, change the appropriate variables to match your expected traffic.</span></span>

<span data-ttu-id="bfea8-195">애플리케이션을 실행하는 확장 집합 VM 인스턴스의 수를 기준으로 제공한 세 가지 샘플 비용 프로필은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-195">We have provided three sample cost profiles based on the number of scale set VM instances that run your applications.</span></span>

- <span data-ttu-id="bfea8-196">[소형][small-pricing]: 이 가격 책정 예제는 2개 프론트 엔드 및 2개 백 엔드 VM 인스턴스와 관련이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-196">[Small][small-pricing]: this pricing example correlates to two front-end and two back-end VM instances.</span></span>
- <span data-ttu-id="bfea8-197">[중형][medium-pricing]: 이 가격 책정 예제는 20개 프론트 엔드 및 5개 백 엔드 VM 인스턴스와 관련이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-197">[Medium][medium-pricing]: this pricing example correlates to 20 front-end and 5 back-end VM instances.</span></span>
- <span data-ttu-id="bfea8-198">[대형][large-pricing]: 이 가격 책정 예제는 100개 프론트 엔드 및 10개 백 엔드 VM 인스턴스와 관련이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-198">[Large][large-pricing]: this pricing example correlates to 100 front-end and 10 back-end VM instances.</span></span>

## <a name="related-resources"></a><span data-ttu-id="bfea8-199">관련 리소스</span><span class="sxs-lookup"><span data-stu-id="bfea8-199">Related resources</span></span>

<span data-ttu-id="bfea8-200">이 시나리오에서는 Microsoft SQL Server 클러스터를 실행하는 백 엔드 가상 머신 확장 집합을 사용했습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-200">This scenario used a back-end virtual machine scale set that runs a Microsoft SQL Server cluster.</span></span> <span data-ttu-id="bfea8-201">또한 Cosmos DB도 애플리케이션 데이터에 대한 확장 가능하고 안전한 데이터베이스 계층으로 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-201">Cosmos DB could also be used as a scalable and secure database tier for the application data.</span></span> <span data-ttu-id="bfea8-202">[Azure 가상 네트워크 서비스 엔드포인트][vnetendpoint-docs]를 사용하면 중요한 Azure 서비스 리소스를 가상 네트워크에서만 보호할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-202">An [Azure virtual network service endpoint][vnetendpoint-docs] allows you to secure your critical Azure service resources to only your virtual networks.</span></span> <span data-ttu-id="bfea8-203">이 시나리오에서는 VNet 엔드포인트를 통해 프런트 엔드 애플리케이션 계층과 Cosmos DB 간의 트래픽을 보호할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bfea8-203">In this scenario, VNet endpoints allow you to secure traffic between the front-end application tier and Cosmos DB.</span></span> <span data-ttu-id="bfea8-204">자세한 내용은 [Azure Cosmos DB 개요](/azure/cosmos-db/introduction)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="bfea8-204">For more information, see the [Azure Cosmos DB overview](/azure/cosmos-db/introduction).</span></span>

<span data-ttu-id="bfea8-205">보다 자세한 구현 가이드는 [SQL Server를 사용하는 N 계층 애플리케이션에 대한 참조 아키텍처][ntiersql-ra]를 검토하세요.</span><span class="sxs-lookup"><span data-stu-id="bfea8-205">For more detailed implementation guides, review the [reference architecture for N-tier applications using SQL Server][ntiersql-ra].</span></span>

<!-- links -->
[appgateway-docs]: /azure/application-gateway/overview
[architecture]: ./media/architecture-regulated-multitier-app.png
[autoscaling]: /azure/architecture/best-practices/auto-scaling
[availability]: ../../checklist/availability.md
[cloudwitness-docs]: /windows-server/failover-clustering/deploy-cloud-witness
[loadbalancer-docs]: /azure/load-balancer/load-balancer-overview
[nsg-docs]: /azure/virtual-network/security-overview
[ntiersql-ra]: /azure/architecture/reference-architectures/n-tier/n-tier-sql-server
[resiliency]: /azure/architecture/resiliency/
[security]: /azure/security/
[scalability]: /azure/architecture/checklist/scalability
[scaleset-docs]: /azure/virtual-machine-scale-sets/overview
[sqlalwayson-docs]: /sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server
[vmssautoscale-docs]: /azure/virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview
[vnet-docs]: /azure/virtual-network/virtual-networks-overview
[vnetendpoint-docs]: /azure/virtual-network/virtual-network-service-endpoints-overview
[pci-dss]: /azure/security/blueprints/pcidss-iaaswa-overview
[dmz]: /azure/virtual-network/virtual-networks-dmz-nsg
[sql-linux]: /sql/linux/sql-server-linux-overview?view=sql-server-linux-2017
[cloud-witness]: /windows-server/failover-clustering/deploy-cloud-witness
[small-pricing]: https://azure.com/e/711bbfcbbc884ef8aa91cdf0f2caff72
[medium-pricing]: https://azure.com/e/b622d82d79b34b8398c4bce35477856f
[large-pricing]: https://azure.com/e/1d99d8b92f90496787abecffa1473a93
