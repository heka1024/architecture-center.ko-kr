---
title: Azure에 온-프레미스 네트워크 연결
titleSuffix: Azure Reference Architectures
description: 온-프레미스 네트워크를 Azure에 연결하기 위한 참조 아키텍처를 비교합니다.
author: telmosampaio
ms.date: 07/02/2018
ms.openlocfilehash: de509b6d95805f4fc871f6dbd76a87d2c0bec6f1
ms.sourcegitcommit: 88a68c7e9b6b772172b7faa4b9fd9c061a9f7e9d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53119917"
---
# <a name="choose-a-solution-for-connecting-an-on-premises-network-to-azure"></a><span data-ttu-id="7748b-103">온-프레미스 네트워크를 Azure에 연결하기 위한 솔루션 선택</span><span class="sxs-lookup"><span data-stu-id="7748b-103">Choose a solution for connecting an on-premises network to Azure</span></span>

<span data-ttu-id="7748b-104">이 문서에서는 온-프레미스 네트워크를 Azure VNet(Virtual Network)에 연결하는 옵션을 비교합니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-104">This article compares options for connecting an on-premises network to an Azure Virtual Network (VNet).</span></span> <span data-ttu-id="7748b-105">각 옵션에서 자세한 참조 아키텍처를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-105">For each option, a more detailed reference architecture is available.</span></span>

## <a name="vpn-connection"></a><span data-ttu-id="7748b-106">VPN 연결</span><span class="sxs-lookup"><span data-stu-id="7748b-106">VPN connection</span></span>

<span data-ttu-id="7748b-107">[VPN Gateway](/azure/vpn-gateway/vpn-gateway-about-vpngateways)는 가상 네트워크와 온-프레미스 위치 간에 암호화된 트래픽을 전송하는 가상 네트워크 게이트웨이의 유형입니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-107">A [VPN gateway](/azure/vpn-gateway/vpn-gateway-about-vpngateways) is a type of virtual network gateway that sends encrypted traffic between an Azure virtual network and an on-premises location.</span></span> <span data-ttu-id="7748b-108">암호화된 트래픽은 공용 인터넷을 통해 전송됩니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-108">The encrypted traffic goes over the public Internet.</span></span>

<span data-ttu-id="7748b-109">이 아키텍처는 온-프레미스 하드웨어와 클라우드 간의 트래픽이 가벼울 가능성이 높은 하이브리드 애플리케이션에 적합하거나, 클라우드의 유연성 및 처리 능력을 위해 대기 시간을 약간 연장하고자 하는 경우에 적합합니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-109">This architecture is suitable for hybrid applications where the traffic between on-premises hardware and the cloud is likely to be light, or you are willing to trade slightly extended latency for the flexibility and processing power of the cloud.</span></span>

<span data-ttu-id="7748b-110">**이점**</span><span class="sxs-lookup"><span data-stu-id="7748b-110">**Benefits**</span></span>

- <span data-ttu-id="7748b-111">구성이 간단합니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-111">Simple to configure.</span></span>

<span data-ttu-id="7748b-112">**과제**</span><span class="sxs-lookup"><span data-stu-id="7748b-112">**Challenges**</span></span>

- <span data-ttu-id="7748b-113">온-프레미스 VPN 디바이스가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-113">Requires an on-premises VPN device.</span></span>
- <span data-ttu-id="7748b-114">Microsoft에서는 각 VPN Gateway에 99.9%의 가용성을 보장하지만, 이 [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/)는 게이트웨이에 대한 네트워크 연결이 아닌 VPN Gateway만 다룹니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-114">Although Microsoft guarantees 99.9% availability for each VPN Gateway, this [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/) only covers the VPN gateway, and not your network connection to the gateway.</span></span>
- <span data-ttu-id="7748b-115">현재 Azure VPN Gateway를 통한 VPN 연결은 최대 1.25Gbps의 대역폭을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-115">A VPN connection over Azure VPN Gateway currently supports a maximum of 1.25 Gbps bandwidth.</span></span> <span data-ttu-id="7748b-116">이러한 처리량을 초과할 것으로 예상되는 경우 여러 VPN 연결에 걸쳐 Azure 가상 네트워크를 분할해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-116">You may need to partition your Azure virtual network across multiple VPN connections if you expect to exceed this throughput.</span></span>

<span data-ttu-id="7748b-117">**참조 아키텍처**</span><span class="sxs-lookup"><span data-stu-id="7748b-117">**Reference architecture**</span></span>

- [<span data-ttu-id="7748b-118">VPN Gateway를 사용하는 하이브리드 네트워크</span><span class="sxs-lookup"><span data-stu-id="7748b-118">Hybrid network with VPN gateway</span></span>](./vpn.md)

## <a name="azure-expressroute-connection"></a><span data-ttu-id="7748b-119">Azure ExpressRoute 연결</span><span class="sxs-lookup"><span data-stu-id="7748b-119">Azure ExpressRoute connection</span></span>

<span data-ttu-id="7748b-120">[ExpressRoute](/azure/expressroute/) 연결은 타사 연결 공급자를 통해 개인 전용 연결을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-120">[ExpressRoute](/azure/expressroute/) connections use a private, dedicated connection through a third-party connectivity provider.</span></span> <span data-ttu-id="7748b-121">개인 연결은 온-프레미스 네트워크를 Azure로 확장합니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-121">The private connection extends your on-premises network into Azure.</span></span> 

<span data-ttu-id="7748b-122">이 아키텍처는 높은 수준의 확장성이 필요한 대규모 중요 업무 워크로드를 실행하는 하이브리드 애플리케이션에 적합합니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-122">This architecture is suitable for hybrid applications running large-scale, mission-critical workloads that require a high degree of scalability.</span></span> 

<span data-ttu-id="7748b-123">**이점**</span><span class="sxs-lookup"><span data-stu-id="7748b-123">**Benefits**</span></span>

- <span data-ttu-id="7748b-124">연결 공급자에 따라 훨씬 더 높은 대역폭(최대 10Gbps)을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-124">Much higher bandwidth available; up to 10 Gbps depending on the connectivity provider.</span></span>
- <span data-ttu-id="7748b-125">동적 대역폭 확장을 지원하여 수요가 더 낮은 기간 동안 비용을 줄일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-125">Supports dynamic scaling of bandwidth to help reduce costs during periods of lower demand.</span></span> <span data-ttu-id="7748b-126">그러나 모든 연결 공급자에 이 옵션이 있는 것은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-126">However, not all connectivity providers have this option.</span></span>
- <span data-ttu-id="7748b-127">연결 공급자에 따라 조직에서 국가 클라우드에 직접 액세스할 수 있게 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-127">May allow your organization direct access to national clouds, depending on the connectivity provider.</span></span>
- <span data-ttu-id="7748b-128">전체 연결에서 99.9%의 가용성 SLA를 실현할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-128">99.9% availability SLA across the entire connection.</span></span>

<span data-ttu-id="7748b-129">**과제**</span><span class="sxs-lookup"><span data-stu-id="7748b-129">**Challenges**</span></span>

- <span data-ttu-id="7748b-130">설정이 복잡할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-130">Can be complex to set up.</span></span> <span data-ttu-id="7748b-131">ExpressRoute 연결을 만들려면 타사 연결 공급자를 함께 사용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-131">Creating an ExpressRoute connection requires working with a third-party connectivity provider.</span></span> <span data-ttu-id="7748b-132">해당 공급자가 네트워크 연결 프로비저닝을 담당합니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-132">The provider is responsible for provisioning the network connection.</span></span>
- <span data-ttu-id="7748b-133">온-프레미스에 높은 대역폭의 라우터가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-133">Requires high-bandwidth routers on-premises.</span></span>

<span data-ttu-id="7748b-134">**참조 아키텍처**</span><span class="sxs-lookup"><span data-stu-id="7748b-134">**Reference architecture**</span></span>

- [<span data-ttu-id="7748b-135">ExpressRoute를 사용하는 하이브리드 네트워크</span><span class="sxs-lookup"><span data-stu-id="7748b-135">Hybrid network with ExpressRoute</span></span>](./expressroute.md)

## <a name="expressroute-with-vpn-failover"></a><span data-ttu-id="7748b-136">VPN 장애 조치(failover)를 사용하는 ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="7748b-136">ExpressRoute with VPN failover</span></span>

<span data-ttu-id="7748b-137">이 옵션은 앞에 나온 두 가지 옵션을 결합합니다. 정상적인 조건에서는 ExpressRoute를 사용하고 ExpressRoute 회로에서 연결이 끊기면 VPN 연결로 장애 조치(failover)합니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-137">This options combines the previous two, using ExpressRoute in normal conditions, but failing over to a VPN connection if there is a loss of connectivity in the ExpressRoute circuit.</span></span>

<span data-ttu-id="7748b-138">이 아키텍처는 더 높은 ExpressRoute 대역폭도 필요하고 고가용성 네트워크 연결도 필요한 하이브리드 애플리케이션에 적합합니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-138">This architecture is suitable for hybrid applications that need the higher bandwidth of ExpressRoute, and also require highly available network connectivity.</span></span> 

<span data-ttu-id="7748b-139">**이점**</span><span class="sxs-lookup"><span data-stu-id="7748b-139">**Benefits**</span></span>

- <span data-ttu-id="7748b-140">ExpressRoute 회로가 고장나는 경우 대체 연결이 더 낮은 대역폭 네트워크에 있더라도 고가용성입니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-140">High availability if the ExpressRoute circuit fails, although the fallback connection is on a lower bandwidth network.</span></span>

<span data-ttu-id="7748b-141">**과제**</span><span class="sxs-lookup"><span data-stu-id="7748b-141">**Challenges**</span></span>

- <span data-ttu-id="7748b-142">구성이 복잡합니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-142">Complex to configure.</span></span> <span data-ttu-id="7748b-143">VPN 연결과 ExpressRoute 회로를 모두 설정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-143">You need to set up both a VPN connection and an ExpressRoute circuit.</span></span>
- <span data-ttu-id="7748b-144">중복 하드웨어(VPN 어플라이언스)와 중복 Azure VPN Gateway 연결이 필요하며 이는 유료입니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-144">Requires redundant hardware (VPN appliances), and a redundant Azure VPN Gateway connection for which you pay charges.</span></span>

<span data-ttu-id="7748b-145">**참조 아키텍처**</span><span class="sxs-lookup"><span data-stu-id="7748b-145">**Reference architecture**</span></span>

- [<span data-ttu-id="7748b-146">ExpressRoute 및 VPN 장애 조치를 사용하는 하이브리드 네트워크</span><span class="sxs-lookup"><span data-stu-id="7748b-146">Hybrid network with ExpressRoute and VPN failover</span></span>](./expressroute-vpn-failover.md)

## <a name="hub-spoke-network-topology"></a><span data-ttu-id="7748b-147">허프 스포크 네트워크 토폴로지</span><span class="sxs-lookup"><span data-stu-id="7748b-147">Hub-spoke network topology</span></span>

<span data-ttu-id="7748b-148">허브-스포크 네트워크 토폴로지는 ID 및 보안과 같은 서비스를 공유하는 동안 워크로드를 격리하는 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-148">A hub-spoke network topology is a way to isolate workloads while sharing services such as identity and security.</span></span> <span data-ttu-id="7748b-149">허브는 Azure의 VNet(가상 네트워크)로서 사용자의 온-프레미스 네트워크에 대한 연결의 중심으로 기능합니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-149">The hub is a virtual network (VNet) in Azure that acts as a central point of connectivity to your on-premises network.</span></span> <span data-ttu-id="7748b-150">스포크는 허브와 피어링된 VNet입니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-150">The spokes are VNets that peer with the hub.</span></span> <span data-ttu-id="7748b-151">개별 작업을 스포크로 배포하는 반면 공유 서비스는 허브에 배포됩니다.</span><span class="sxs-lookup"><span data-stu-id="7748b-151">Shared services are deployed in the hub, while individual workloads are deployed as spokes.</span></span>

<span data-ttu-id="7748b-152">**참조 아키텍처**</span><span class="sxs-lookup"><span data-stu-id="7748b-152">**Reference architectures**</span></span>

- [<span data-ttu-id="7748b-153">허브-스포크 토폴로지</span><span class="sxs-lookup"><span data-stu-id="7748b-153">Hub-spoke topology</span></span>](./hub-spoke.md)
- [<span data-ttu-id="7748b-154">공유 서비스를 사용하는 허브-스포크</span><span class="sxs-lookup"><span data-stu-id="7748b-154">Hub-spoke with shared services</span></span>](./shared-services.md)
