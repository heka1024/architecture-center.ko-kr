---
title: VPN을 사용하여 온-프레미스 네트워크를 Azure에 연결
titleSuffix: Azure Reference Architectures
description: VPN을 사용하여 연결된 온-프레미스 네트워크 및 Azure Virtual Network를 포괄하는 보안 사이트 간 네트워크 아키텍처를 구현합니다.
author: RohitSharma-pnp
ms.date: 10/22/2018
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: networking
ms.openlocfilehash: 515cd3f5d23957e0e153c9d25198b3cb98b92a5d
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58245674"
---
# <a name="connect-an-on-premises-network-to-azure-using-a-vpn-gateway"></a><span data-ttu-id="8ee45-103">VPN 게이트웨이를 사용하여 온-프레미스 네트워크를 Azure에 연결</span><span class="sxs-lookup"><span data-stu-id="8ee45-103">Connect an on-premises network to Azure using a VPN gateway</span></span>

<span data-ttu-id="8ee45-104">이 참조 아키텍처에서는 사이트 간 VPN(가상 사설 네트워크)을 사용하여 온-프레미스 네트워크를 Azure로 확장하는 방법을 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-104">This reference architecture shows how to extend an on-premises network to Azure, using a site-to-site virtual private network (VPN).</span></span> <span data-ttu-id="8ee45-105">IPSec VPN 터널을 통해 온-프레미스 네트워크와 Azure 가상 네트워크(VNet) 사이에서 트래픽이 흐릅니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-105">Traffic flows between the on-premises network and an Azure Virtual Network (VNet) through an IPSec VPN tunnel.</span></span> <span data-ttu-id="8ee45-106">[**이 솔루션을 배포합니다**](#deploy-the-solution).</span><span class="sxs-lookup"><span data-stu-id="8ee45-106">[**Deploy this solution**](#deploy-the-solution).</span></span>

![온-프레미스 인프라와 Azure 인프라를 포괄하는 하이브리드 네트워크](./images/vpn.png)

<span data-ttu-id="8ee45-108">*이 아키텍처의 [Visio 파일][visio-download]을 다운로드합니다.*</span><span class="sxs-lookup"><span data-stu-id="8ee45-108">*Download a [Visio file][visio-download] of this architecture.*</span></span>

## <a name="architecture"></a><span data-ttu-id="8ee45-109">아키텍처</span><span class="sxs-lookup"><span data-stu-id="8ee45-109">Architecture</span></span>

<span data-ttu-id="8ee45-110">이 아키텍처는 다음 구성 요소로 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-110">The architecture consists of the following components.</span></span>

- <span data-ttu-id="8ee45-111">**온-프레미스 네트워크**.</span><span class="sxs-lookup"><span data-stu-id="8ee45-111">**On-premises network**.</span></span> <span data-ttu-id="8ee45-112">조직 내에서 실행되는 개인 로컬 영역 네트워크입니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-112">A private local-area network running within an organization.</span></span>

- <span data-ttu-id="8ee45-113">**VPN 어플라이언스**.</span><span class="sxs-lookup"><span data-stu-id="8ee45-113">**VPN appliance**.</span></span> <span data-ttu-id="8ee45-114">온-프레미스 네트워크에 외부 연결을 제공하는 디바이스 또는 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-114">A device or service that provides external connectivity to the on-premises network.</span></span> <span data-ttu-id="8ee45-115">VPN 어플라이언스는 하드웨어 디바이스일 수도 있고 Windows Server 2012의 RRAS(라우팅 및 원격 액세스 서비스)와 같은 소프트웨어 솔루션일 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-115">The VPN appliance may be a hardware device, or it can be a software solution such as the Routing and Remote Access Service (RRAS) in Windows Server 2012.</span></span> <span data-ttu-id="8ee45-116">지원되는 VPN 어플라이언스 목록 및 VPN 어플라이언스가 Azure VPN Gateway에 연결되도록 구성하는 방법은 [사이트 간 VPN Gateway 연결을 위한 VPN 디바이스 정보][vpn-appliance] 문서에서 선택한 디바이스에 대한 지침을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="8ee45-116">For a list of supported VPN appliances and information on configuring them to connect to an Azure VPN gateway, see the instructions for the selected device in the article [About VPN devices for Site-to-Site VPN Gateway connections][vpn-appliance].</span></span>

- <span data-ttu-id="8ee45-117">**가상 네트워크(VNet)**.</span><span class="sxs-lookup"><span data-stu-id="8ee45-117">**Virtual network (VNet)**.</span></span> <span data-ttu-id="8ee45-118">Azure VPN 게이트웨이의 클라우드 애플리케이션과 구성 요소는 동일한 [VNet][azure-virtual-network]에 존재합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-118">The cloud application and the components for the Azure VPN gateway reside in the same [VNet][azure-virtual-network].</span></span>

- <span data-ttu-id="8ee45-119">**Azure VPN 게이트웨이**.</span><span class="sxs-lookup"><span data-stu-id="8ee45-119">**Azure VPN gateway**.</span></span> <span data-ttu-id="8ee45-120">[VPN 게이트웨이][azure-vpn-gateway] 서비스를 사용하면 VPN 어플라이언스를 통해 VNet을 온-프레미스 네트워크에 연결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-120">The [VPN gateway][azure-vpn-gateway] service enables you to connect the VNet to the on-premises network through a VPN appliance.</span></span> <span data-ttu-id="8ee45-121">자세한 내용은 [온-프레미스 네트워크를 Microsoft Azure virtual network에 연결][connect-to-an-Azure-vnet]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="8ee45-121">For more information, see [Connect an on-premises network to a Microsoft Azure virtual network][connect-to-an-Azure-vnet].</span></span> <span data-ttu-id="8ee45-122">VPN 게이트웨이에는 다음과 같은 요소가 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-122">The VPN gateway includes the following elements:</span></span>

  - <span data-ttu-id="8ee45-123">**가상 네트워크 게이트웨이**.</span><span class="sxs-lookup"><span data-stu-id="8ee45-123">**Virtual network gateway**.</span></span> <span data-ttu-id="8ee45-124">VNet을 위한 가상 VPN 어플라이언스를 제공하는 리소스입니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-124">A resource that provides a virtual VPN appliance for the VNet.</span></span> <span data-ttu-id="8ee45-125">온-프레미스 네트워크에서 VNet으로 트래픽을 라우팅하는 역할을 담당합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-125">It is responsible for routing traffic from the on-premises network to the VNet.</span></span>
  - <span data-ttu-id="8ee45-126">**로컬 네트워크 게이트웨이**.</span><span class="sxs-lookup"><span data-stu-id="8ee45-126">**Local network gateway**.</span></span> <span data-ttu-id="8ee45-127">온-프레미스 VPN 어플라이언스가 추상화된 것입니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-127">An abstraction of the on-premises VPN appliance.</span></span> <span data-ttu-id="8ee45-128">클라우드 애플리케이션에서 온-프레미스 네트워크로 흐르는 네트워크 트래픽은 이 게이트웨이를 통과하도록 라우팅됩니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-128">Network traffic from the cloud application to the on-premises network is routed through this gateway.</span></span>
  - <span data-ttu-id="8ee45-129">**연결**.</span><span class="sxs-lookup"><span data-stu-id="8ee45-129">**Connection**.</span></span> <span data-ttu-id="8ee45-130">이 연결에는 연결 유형(IPSec) 및 트래픽을 암호화하기 위해 온-프레미스 VPN 어플라이언스와 공유되는 키를 지정하는 속성이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-130">The connection has properties that specify the connection type (IPSec) and the key shared with the on-premises VPN appliance to encrypt traffic.</span></span>
  - <span data-ttu-id="8ee45-131">**게이트웨이 서브넷**.</span><span class="sxs-lookup"><span data-stu-id="8ee45-131">**Gateway subnet**.</span></span> <span data-ttu-id="8ee45-132">가상 네트워크 게이트웨이는 자체 서브넷에 존재합니다. 자체 서브넷은 아래의 권장 사항 섹션에서 설명하는 다양한 요구 사항에 따라 달라집니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-132">The virtual network gateway is held in its own subnet, which is subject to various requirements, described in the Recommendations section below.</span></span>

- <span data-ttu-id="8ee45-133">**클라우드 애플리케이션**.</span><span class="sxs-lookup"><span data-stu-id="8ee45-133">**Cloud application**.</span></span> <span data-ttu-id="8ee45-134">Azure에 호스팅된 애플리케이션입니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-134">The application hosted in Azure.</span></span> <span data-ttu-id="8ee45-135">Azure Load Balancer를 통해 여러 서브넷이 연결된 여러 계층이 포함될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-135">It might include multiple tiers, with multiple subnets connected through Azure load balancers.</span></span> <span data-ttu-id="8ee45-136">애플리케이션 인프라에 대한 자세한 내용은 [Windows VM 워크로드 실행][windows-vm-ra] 및 [Linux VM 워크로드 실행][linux-vm-ra]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="8ee45-136">For more information about the application infrastructure, see [Running Windows VM workloads][windows-vm-ra] and [Running Linux VM workloads][linux-vm-ra].</span></span>

- <span data-ttu-id="8ee45-137">**내부 부하 분산 장치**.</span><span class="sxs-lookup"><span data-stu-id="8ee45-137">**Internal load balancer**.</span></span> <span data-ttu-id="8ee45-138">VPN 게이트웨이에서 전송되는 네트워크 트래픽은 내부 부하 분산 장치를 통해 클라우드 애플리케이션으로 라우팅됩니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-138">Network traffic from the VPN gateway is routed to the cloud application through an internal load balancer.</span></span> <span data-ttu-id="8ee45-139">부하 분산 장치는 애플리케이션의 프론트 엔드 서브넷에 위치합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-139">The load balancer is located in the front-end subnet of the application.</span></span>

## <a name="recommendations"></a><span data-ttu-id="8ee45-140">권장 사항</span><span class="sxs-lookup"><span data-stu-id="8ee45-140">Recommendations</span></span>

<span data-ttu-id="8ee45-141">대부분의 시나리오의 경우 다음 권장 사항을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-141">The following recommendations apply for most scenarios.</span></span> <span data-ttu-id="8ee45-142">이러한 권장 사항을 재정의하라는 특정 요구 사항이 있는 경우가 아니면 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-142">Follow these recommendations unless you have a specific requirement that overrides them.</span></span>

### <a name="vnet-and-gateway-subnet"></a><span data-ttu-id="8ee45-143">VNet과 게이트웨이 서브넷</span><span class="sxs-lookup"><span data-stu-id="8ee45-143">VNet and gateway subnet</span></span>

<span data-ttu-id="8ee45-144">필요한 리소스를 모두 수용할 수 있을 만큼 충분한 주소 공간을 사용하여 Azure VNet을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-144">Create an Azure VNet with an address space large enough for all of your required resources.</span></span> <span data-ttu-id="8ee45-145">추후 더 많은 VM이 필요할 경우 여분이 충분하도록 VNet 주소 공간을 할당합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-145">Ensure that the VNet address space has sufficient room for growth if additional VMs are likely to be needed in the future.</span></span> <span data-ttu-id="8ee45-146">VNet 주소 공간은 온-프레미스 네트워크와 겹치지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-146">The address space of the VNet must not overlap with the on-premises network.</span></span> <span data-ttu-id="8ee45-147">예를 들어 위 다이어그램에서는 VNet에 10.20.0.0/16의 주소 공간을 사용하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-147">For example, the diagram above uses the address space 10.20.0.0/16 for the VNet.</span></span>

<span data-ttu-id="8ee45-148">이름이 *GatewaySubnet*인 서브넷을 만듭니다. 주소 범위는 /27로 합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-148">Create a subnet named *GatewaySubnet*, with an address range of /27.</span></span> <span data-ttu-id="8ee45-149">이 서브넷은 가상 네트워크 게이트웨이에서 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-149">This subnet is required by the virtual network gateway.</span></span> <span data-ttu-id="8ee45-150">이 서브넷에 주소 32개를 할당하면 추후 게이트웨이 크기 제한에 도달하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-150">Allocating 32 addresses to this subnet will help to prevent reaching gateway size limitations in the future.</span></span> <span data-ttu-id="8ee45-151">주소 공간 가운데에 이 서브넷을 배치하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-151">Also, avoid placing this subnet in the middle of the address space.</span></span> <span data-ttu-id="8ee45-152">게이트웨이 서브넷을 VNet 주소 공간의 상단 끝에 배치하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-152">A good practice is to set the address space for the gateway subnet at the upper end of the VNet address space.</span></span> <span data-ttu-id="8ee45-153">다이어그램에서는 10.20.255.224/27을 사용하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-153">The example shown in the diagram uses 10.20.255.224/27.</span></span>  <span data-ttu-id="8ee45-154">다음은 [CIDR]을 간단하게 계산하는 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-154">Here is a quick procedure to calculate the [CIDR]:</span></span>

1. <span data-ttu-id="8ee45-155">VNet 주소 공간의 변수 비트를 게이트웨이 서브넷에 의해 사용되는 비트까지 모두 1로 설정하고, 나머지 비트를 0으로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-155">Set the variable bits in the address space of the VNet to 1, up to the bits being used by the gateway subnet, then set the remaining bits to 0.</span></span>
2. <span data-ttu-id="8ee45-156">결과 비트를 10진수로 변환한 다음 이것을 게이트웨이 서브넷의 크기로 설정된 접두사 길이를 사용하여 주소 공간으로 표현합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-156">Convert the resulting bits to decimal and express it as an address space with the prefix length set to the size of the gateway subnet.</span></span>

<span data-ttu-id="8ee45-157">예를 들어 IP 주소 범위가 10.20.0.0/16인 VNet에 1단계를 적용하면 10.20.0b11111111.0b11100000이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-157">For example, for a VNet with an IP address range of 10.20.0.0/16, applying step #1 above becomes 10.20.0b11111111.0b11100000.</span></span>  <span data-ttu-id="8ee45-158">이것을 10진수로 변환한 다음 주소 공간으로 표현하면 10.20.255.224/27이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-158">Converting that to decimal and expressing it as an address space yields 10.20.255.224/27.</span></span>

> [!WARNING]
> <span data-ttu-id="8ee45-159">게이트웨이 서브넷에는 VM을 배포하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-159">Do not deploy any VMs to the gateway subnet.</span></span> <span data-ttu-id="8ee45-160">또한, 이 서브넷에 NSG을 할당하지 않습니다. NSG를 할당하면 게이트웨이의 작동이 중지됩니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-160">Also, do not assign an NSG to this subnet, as it will cause the gateway to stop functioning.</span></span>
>

### <a name="virtual-network-gateway"></a><span data-ttu-id="8ee45-161">가상 네트워크 게이트웨이</span><span class="sxs-lookup"><span data-stu-id="8ee45-161">Virtual network gateway</span></span>

<span data-ttu-id="8ee45-162">가상 네트워크 게이트웨이에 공용 IP 주소를 할당합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-162">Allocate a public IP address for the virtual network gateway.</span></span>

<span data-ttu-id="8ee45-163">게이트웨이 서브넷에 가상 네트워크 게이트웨이를 만든 다음 이것을 새로 할당된 공용 IP 주소에 할당합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-163">Create the virtual network gateway in the gateway subnet and assign it the newly allocated public IP address.</span></span> <span data-ttu-id="8ee45-164">사용자 요구 사항에 가장 가까우면서 사용자의 VPN 어플라이언스가 지원하는 게이트웨이 유형을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-164">Use the gateway type that most closely matches your requirements and that is enabled by your VPN appliance:</span></span>

- <span data-ttu-id="8ee45-165">주소 접두사와 같은 정책 조건을 바탕으로 요청이 라우팅되는 방식을 제어해야 하는 경우 [policy-based gateway][policy-based-routing]를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-165">Create a [policy-based gateway][policy-based-routing] if you need to closely control how requests are routed based on policy criteria such as address prefixes.</span></span> <span data-ttu-id="8ee45-166">정책 기반 게이트웨이는 정적 라우팅을 사용하며, 사이트 간 연결에서만 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-166">Policy-based gateways use static routing, and only work with site-to-site connections.</span></span>

- <span data-ttu-id="8ee45-167">RRAS를 사용하여 온-프레미스 네트워크에 연결하거나 다중 사이트 또는 교차 지역 연결을 지원하는 경우, [route-based gateway][route-based-routing]를 만들거나 VNet 간 연결을 구현합니다(복수의 VNet을 이동하는 경로 포함).</span><span class="sxs-lookup"><span data-stu-id="8ee45-167">Create a [route-based gateway][route-based-routing] if you connect to the on-premises network using RRAS, support multi-site or cross-region connections, or implement VNet-to-VNet connections (including routes that traverse multiple VNets).</span></span> <span data-ttu-id="8ee45-168">경로 기반 게이트웨이는 네트워크 간에 트래픽을 전달할 때 동적 라우팅을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-168">Route-based gateways use dynamic routing to direct traffic between networks.</span></span> <span data-ttu-id="8ee45-169">동적 라우팅은 대체 경로를 시도하므로 정적 라우팅보다 네트워크 경로 장애에 대한 내결함성이 높습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-169">They can tolerate failures in the network path better than static routes because they can try alternative routes.</span></span> <span data-ttu-id="8ee45-170">경로 기반 게이트웨이에서는 네트워크 주소가 변경되어도 경로를 수동으로 업데이트할 필요가 없기 때문에 관리 오버헤드를 줄일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-170">Route-based gateways can also reduce the management overhead because routes might not need to be updated manually when network addresses change.</span></span>

<span data-ttu-id="8ee45-171">지원되는 VPN 어플라이언스 목록은 [사이트 간 VPN 게이트웨이 연결을 위한 VPN 디바이스 정보][vpn-appliances]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="8ee45-171">For a list of supported VPN appliances, see [About VPN devices for Site-to-Site VPN Gateway connections][vpn-appliances].</span></span>

> [!NOTE]
> <span data-ttu-id="8ee45-172">게이트웨이를 만든 뒤에는 게이트웨이 유형을 변경하려면 게이트웨이를 삭제하고 다시 만들어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-172">After the gateway has been created, you cannot change between gateway types without deleting and re-creating the gateway.</span></span>
>

<span data-ttu-id="8ee45-173">사용자의 처리량 요구 사항에 가장 가까운 Azure VPN 게이트웨이 SKU를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-173">Select the Azure VPN gateway SKU that most closely matches your throughput requirements.</span></span> <span data-ttu-id="8ee45-174">자세한 내용은 [게이트웨이 SKU][azure-gateway-skus]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="8ee45-174">For more information, see [Gateway SKUs][azure-gateway-skus]</span></span>

> [!NOTE]
> <span data-ttu-id="8ee45-175">기본 SKU는 Azure ExpressRoute와 호환되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-175">The Basic SKU is not compatible with Azure ExpressRoute.</span></span> <span data-ttu-id="8ee45-176">게이트웨이를 만든 뒤에 [SKU를 변경][changing-SKUs]할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-176">You can [change the SKU][changing-SKUs] after the gateway has been created.</span></span>
>

<span data-ttu-id="8ee45-177">요금은 게이트웨이가 프로비전되고 사용된 시간에 따라 청구됩니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-177">You are charged based on the amount of time that the gateway is provisioned and available.</span></span> <span data-ttu-id="8ee45-178">[VPN 게이트웨이 가격][azure-gateway-charges]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="8ee45-178">See [VPN Gateway Pricing][azure-gateway-charges].</span></span>

<span data-ttu-id="8ee45-179">요청을 애플리케이션 VM으로 직접 전달하는 대신 게이트웨이로부터 수신되는 애플리케이션 트래픽을 내부 부하 분산 장치로 전달하는 게이트웨이 서브넷을 위한 라우팅 규칙을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-179">Create routing rules for the gateway subnet that direct incoming application traffic from the gateway to the internal load balancer, rather than allowing requests to pass directly to the application VMs.</span></span>

### <a name="on-premises-network-connection"></a><span data-ttu-id="8ee45-180">온-프레미스 네트워크 연결</span><span class="sxs-lookup"><span data-stu-id="8ee45-180">On-premises network connection</span></span>

<span data-ttu-id="8ee45-181">로컬 네트워크 게이트웨이를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-181">Create a local network gateway.</span></span> <span data-ttu-id="8ee45-182">온-프레미스 VPN 어플라이언스의 공용 IP 주소와 온-프레미스 네트워크의 주소 공간을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-182">Specify the public IP address of the on-premises VPN appliance, and the address space of the on-premises network.</span></span> <span data-ttu-id="8ee45-183">온-프레미스 VPN 어플라이언스는 Azure VPN 게이트웨이에서 로컬 네트워크 게이트웨이에 의해 액세스할 수 있는 공용 IP 주소를 가져야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-183">Note that the on-premises VPN appliance must have a public IP address that can be accessed by the local network gateway in Azure VPN Gateway.</span></span> <span data-ttu-id="8ee45-184">VPN 디바이스는 NAT(Network Address Translator) 뒤에 배치될 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-184">The VPN device cannot be located behind a network address translation (NAT) device.</span></span>

<span data-ttu-id="8ee45-185">가상 네트워크 게이트웨이와 로컬 네트워크 게이트웨이의 사이트 간 연결을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-185">Create a site-to-site connection for the virtual network gateway and the local network gateway.</span></span> <span data-ttu-id="8ee45-186">사이트 간(IPSec) 연결 유형을 선택하고 공유 키를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-186">Select the site-to-site (IPSec) connection type, and specify the shared key.</span></span> <span data-ttu-id="8ee45-187">Azure VPN 게이트웨이를 사용하는 사이트 간 암호는 IPSec 프로토콜을 기반으로 하며, 인증을 위해 사전 공유된 키를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-187">Site-to-site encryption with the Azure VPN gateway is based on the IPSec protocol, using preshared keys for authentication.</span></span> <span data-ttu-id="8ee45-188">사전 공유된 키는 Azure VPN 게이트웨이를 만들 때 사용자가 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-188">You specify the key when you create the Azure VPN gateway.</span></span> <span data-ttu-id="8ee45-189">온-프레미스에서 실행되는 VPN 어플라이언스도 동일한 키로 구성해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-189">You must configure the VPN appliance running on-premises with the same key.</span></span> <span data-ttu-id="8ee45-190">현재 다른 인증 메커니즘은 지원되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-190">Other authentication mechanisms are not currently supported.</span></span>

<span data-ttu-id="8ee45-191">목적지가 Azure VNet에 속한 주소인 요청이 VPN 디바이스로 전달되도록 온-프레미스 라우팅 인프라가 구성되어 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-191">Ensure that the on-premises routing infrastructure is configured to forward requests intended for addresses in the Azure VNet to the VPN device.</span></span>

<span data-ttu-id="8ee45-192">온-프레미스 네트워크에서 클라우드 애플리케이션에 필요한 포트를 모두 엽니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-192">Open any ports required by the cloud application in the on-premises network.</span></span>

<span data-ttu-id="8ee45-193">연결을 테스트하여 다음을 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-193">Test the connection to verify that:</span></span>

- <span data-ttu-id="8ee45-194">온-프레미스 VPN 어플라이언스가 Azure VPN 게이트웨이를 통해 트래픽을 클라우드 애플리케이션으로 올바르게 라우팅합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-194">The on-premises VPN appliance correctly routes traffic to the cloud application through the Azure VPN gateway.</span></span>
- <span data-ttu-id="8ee45-195">VNet이 트래픽을 온-프레미스 네트워크로 올바르게 라우팅합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-195">The VNet correctly routes traffic back to the on-premises network.</span></span>
- <span data-ttu-id="8ee45-196">두 방향에서 금지된 트래픽이 올바르게 차단됩니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-196">Prohibited traffic in both directions is blocked correctly.</span></span>

## <a name="scalability-considerations"></a><span data-ttu-id="8ee45-197">확장성 고려 사항</span><span class="sxs-lookup"><span data-stu-id="8ee45-197">Scalability considerations</span></span>

<span data-ttu-id="8ee45-198">기본 또는 표준 VPN 게이트웨이 SKU에서 고성능 VPN SKU로 바꾸면 제한된 수직 확장성을 달성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-198">You can achieve limited vertical scalability by moving from the Basic or Standard VPN Gateway SKUs to the High Performance VPN SKU.</span></span>

<span data-ttu-id="8ee45-199">다량의 VPN 트래픽이 예상되는 VNet이라면 서로 다른 워크로드를 보다 작은 여러 개의 VNet으로 분배하고 각각에 대해 VPN 게이트웨이를 구성하는 방법을 고려할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-199">For VNets that expect a large volume of VPN traffic, consider distributing the different workloads into separate smaller VNets and configuring a VPN gateway for each of them.</span></span>

<span data-ttu-id="8ee45-200">VNet은 수평 또는 수직으로 분할할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-200">You can partition the VNet either horizontally or vertically.</span></span> <span data-ttu-id="8ee45-201">수평으로 분할하려면 각 계층에 속한 일부 VM 인스턴스를 새로운 VNet의 서브넷으로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-201">To partition horizontally, move some VM instances from each tier into subnets of the new VNet.</span></span> <span data-ttu-id="8ee45-202">이렇게 하면 각 VNet이 동일한 구조와 기능을 갖게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-202">The result is that each VNet has the same structure and functionality.</span></span> <span data-ttu-id="8ee45-203">수직으로 분할하려면 기능이 서로 다른 논리 영역(주문 처리, 송장 발행, 고객 계정 관리 등)으로 분할되도록 각 계층을 다시 디자인합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-203">To partition vertically, redesign each tier to divide the functionality into different logical areas (such as handling orders, invoicing, customer account management, and so on).</span></span> <span data-ttu-id="8ee45-204">이렇게 한 뒤 각 기능 영역을 자체 VNet에 배치할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-204">Each functional area can then be placed in its own VNet.</span></span>

<span data-ttu-id="8ee45-205">VNet에 온-프레미스 Active Directory 도메인 컨트롤러를 복제하고 VNet에 DNS를 구현하면 온-프레미스에서 클라우드로 흐르는 보안 관련 트래픽과 관리 트래픽 중 일부를 줄일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-205">Replicating an on-premises Active Directory domain controller in the VNet, and implementing DNS in the VNet, can help to reduce some of the security-related and administrative traffic flowing from on-premises to the cloud.</span></span> <span data-ttu-id="8ee45-206">자세한 내용은 [AD DS(Active Directory Domain Services)를 Azure로 확장][adds-extend-domain]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="8ee45-206">For more information, see [Extending Active Directory Domain Services (AD DS) to Azure][adds-extend-domain].</span></span>

## <a name="availability-considerations"></a><span data-ttu-id="8ee45-207">가용성 고려 사항</span><span class="sxs-lookup"><span data-stu-id="8ee45-207">Availability considerations</span></span>

<span data-ttu-id="8ee45-208">Azure VPN 게이트웨이에서 온-프레미스 네트워크에 계속해서 액세스할 수 있도록 하려면 온-프레미스 VPN 게이트웨이를 위한 장애 조치(failover) 클러스터를 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-208">If you need to ensure that the on-premises network remains available to the Azure VPN gateway, implement a failover cluster for the on-premises VPN gateway.</span></span>

<span data-ttu-id="8ee45-209">조직에 여러 개의 온-프레미스 사이트가 있는 경우 하나 이상의 Azure VNet에 [다중 사이트 연결][vpn-gateway-multi-site]을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-209">If your organization has multiple on-premises sites, create [multi-site connections][vpn-gateway-multi-site] to one or more Azure VNets.</span></span> <span data-ttu-id="8ee45-210">이렇게 하려면 동적(경로 기반) 라우팅이 필요하므로 온-프레미스 VPN 게이트웨이가 이 기능을 지원하는지 확인해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-210">This approach requires dynamic (route-based) routing, so make sure that the on-premises VPN gateway supports this feature.</span></span>

<span data-ttu-id="8ee45-211">서비스 수준 계약에 대한 자세한 내용은 [VPN 게이트웨이의 SLA][sla-for-vpn-gateway]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="8ee45-211">For details about service level agreements, see [SLA for VPN Gateway][sla-for-vpn-gateway].</span></span>

## <a name="manageability-considerations"></a><span data-ttu-id="8ee45-212">관리 효율성 고려 사항</span><span class="sxs-lookup"><span data-stu-id="8ee45-212">Manageability considerations</span></span>

<span data-ttu-id="8ee45-213">온-프레미스 VPN 어플라이언스에서 진단 정보를 모니터링합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-213">Monitor diagnostic information from on-premises VPN appliances.</span></span> <span data-ttu-id="8ee45-214">이 프로세스는 VPN 어플라이언스에서 제공하는 기능에 따라 달라집니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-214">This process depends on the features provided by the VPN appliance.</span></span> <span data-ttu-id="8ee45-215">예를 들어 Windows Server 2012에서 RRAS(라우팅 및 원격 액세스 서비스)를 사용한다면 [RRAS 로깅][rras-logging]이 모니터링 기능이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-215">For example, if you are using the Routing and Remote Access Service on Windows Server 2012, [RRAS logging][rras-logging].</span></span>

<span data-ttu-id="8ee45-216">[Azure VPN 게이트웨이 진단][gateway-diagnostic-logs]을 사용하여 연결 문제 정보를 캡처합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-216">Use [Azure VPN gateway diagnostics][gateway-diagnostic-logs] to capture information about connectivity issues.</span></span> <span data-ttu-id="8ee45-217">이 로그는 연결 요청의 소스와 목적지, 사용된 프로토콜, 연결이 설정된 방식(또는 시도가 실패한 이유)과 같은 정보를 추적하는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-217">These logs can be used to track information such as the source and destinations of connection requests, which protocol was used, and how the connection was established (or why the attempt failed).</span></span>

<span data-ttu-id="8ee45-218">Azure Portal에서 제공되는 감사 로크를 사용하여 Azure VPN 게이트웨이의 운영 로그를 모니터링합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-218">Monitor the operational logs of the Azure VPN gateway using the audit logs available in the Azure portal.</span></span> <span data-ttu-id="8ee45-219">로컬 네트워크 게이트웨이, Azure 네트워크 게이트웨이 및 연결 각각에 대한 개별적인 로그도 사용 가능합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-219">Separate logs are available for the local network gateway, the Azure network gateway, and the connection.</span></span> <span data-ttu-id="8ee45-220">이 정보는 게이트웨이에 적용된 변경 사항을 추적하는 데 사용할 수 있고, 잘 작동하는 게이트웨이가 어떤 이유로 작동이 중단되는 경우에도 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-220">This information can be used to track any changes made to the gateway, and can be useful if a previously functioning gateway stops working for some reason.</span></span>

![Azure Portal의 감사 로그](../_images/guidance-hybrid-network-vpn/audit-logs.png)

<span data-ttu-id="8ee45-222">연결을 모니터링하고 연결 장애 이벤트를 추적합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-222">Monitor connectivity, and track connectivity failure events.</span></span> <span data-ttu-id="8ee45-223">[Nagios][nagios]와 같은 모니터링 패키지를 사용하여 이 정보를 캡처 및 보고할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-223">You can use a monitoring package such as [Nagios][nagios] to capture and report this information.</span></span>

## <a name="security-considerations"></a><span data-ttu-id="8ee45-224">보안 고려 사항</span><span class="sxs-lookup"><span data-stu-id="8ee45-224">Security considerations</span></span>

<span data-ttu-id="8ee45-225">각 VPN 게이트웨이별로 서로 다른 공유 키를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-225">Generate a different shared key for each VPN gateway.</span></span> <span data-ttu-id="8ee45-226">무작위 공격에 대비할 수 있도록 강력한 공유 키를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-226">Use a strong shared key to help resist brute-force attacks.</span></span>

> [!NOTE]
> <span data-ttu-id="8ee45-227">현재 Azure Key Vault를 사용하여 Azure VPN 게이트웨이의 키를 사전에 공유할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-227">Currently, you cannot use Azure Key Vault to preshare keys for the Azure VPN gateway.</span></span>
>

<span data-ttu-id="8ee45-228">온-프레미스 VPN 어플라이언스가 [Azure VPN 게이트웨이와 호환되는][vpn-appliance-ipsec] 암호화 방법을 사용하는지 확인해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-228">Ensure that the on-premises VPN appliance uses an encryption method that is [compatible with the Azure VPN gateway][vpn-appliance-ipsec].</span></span> <span data-ttu-id="8ee45-229">정책 기반 라우팅의 경우 Azure VPN 게이트웨이는 AES256, AES128 및 3DES 암호화 알고리즘을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-229">For policy-based routing, the Azure VPN gateway supports the AES256, AES128, and 3DES encryption algorithms.</span></span> <span data-ttu-id="8ee45-230">경로 기반 게이트웨이의 경우 AES256 및 3DES를 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-230">Route-based gateways support AES256 and 3DES.</span></span>

<span data-ttu-id="8ee45-231">사용 중인 온-프레미스 VPN 어플라이언스가 경계 네트워크와 인터넷 사이에 존재하는 방화벽을 갖는 DMZ(경계 네트워크)상에 있는 경우에는 사이트 간 VPN 연결을 허용하기 위해 [추가적인 방화벽 규칙][additional-firewall-rules]을 구성해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-231">If your on-premises VPN appliance is on a perimeter network (DMZ) that has a firewall between the perimeter network and the Internet, you might have to configure [additional firewall rules][additional-firewall-rules] to allow the site-to-site VPN connection.</span></span>

<span data-ttu-id="8ee45-232">VNet의 애플리케이션이 인터넷으로 데이터를 전송하는 경우에는 목적지가 인터넷인 모든 트래픽이 온-프레미스 네트워크를 통과하여 라우팅되도록 [강제 터널링을 구현][forced-tunneling]하는 방법을 고려할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-232">If the application in the VNet sends data to the Internet, consider [implementing forced tunneling][forced-tunneling] to route all Internet-bound traffic through the on-premises network.</span></span> <span data-ttu-id="8ee45-233">이렇게 하면 온-프레미스 인프라에서 애플리케이션에 의해 발신 요청을 감사할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-233">This approach enables you to audit outgoing requests made by the application from the on-premises infrastructure.</span></span>

> [!NOTE]
> <span data-ttu-id="8ee45-234">강제 터널링은 Azure 서비스(저장소 서비스 등)와 Windows 라이선스 관리자로의 연결에 영향을 줄 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-234">Forced tunneling can impact connectivity to Azure services (the Storage Service, for example) and the Windows license manager.</span></span>
>

## <a name="deploy-the-solution"></a><span data-ttu-id="8ee45-235">솔루션 배포</span><span class="sxs-lookup"><span data-stu-id="8ee45-235">Deploy the solution</span></span>

<span data-ttu-id="8ee45-236">**필수 조건**.</span><span class="sxs-lookup"><span data-stu-id="8ee45-236">**Prerequisites**.</span></span> <span data-ttu-id="8ee45-237">적절한 네트워크 어플라이언스를 사용하여 기존 온-프레미스 인프라가 이미 구성된 상태여야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-237">You must have an existing on-premises infrastructure already configured with a suitable network appliance.</span></span>

<span data-ttu-id="8ee45-238">솔루션을 배포하려면 다음 단계를 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-238">To deploy the solution, perform the following steps.</span></span>

<!-- markdownlint-disable MD033 -->

1. <span data-ttu-id="8ee45-239">아래 단추를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-239">Click the button below:</span></span><br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fhybrid-networking%2Fvpn%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>
2. <span data-ttu-id="8ee45-240">Azure Portal에서 링크가 열릴 때까지 기다린 후 다음 단계를 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-240">Wait for the link to open in the Azure portal, then follow these steps:</span></span>
   - <span data-ttu-id="8ee45-241">**리소스 그룹** 이름이 매개 변수 파일에 이미 정의되어 있으므로 **새로 만들기**를 선택하고 텍스트 상자에 `ra-hybrid-vpn-rg`를 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-241">The **Resource group** name is already defined in the parameter file, so select **Create New** and enter `ra-hybrid-vpn-rg` in the text box.</span></span>
   - <span data-ttu-id="8ee45-242">**위치** 드롭다운 상자에서 하위 지역을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-242">Select the region from the **Location** drop down box.</span></span>
   - <span data-ttu-id="8ee45-243">**템플릿 루트 Uri** 또는 **매개 변수 루트 Uri** 텍스트 상자를 편집하지 마세요.</span><span class="sxs-lookup"><span data-stu-id="8ee45-243">Do not edit the **Template Root Uri** or the **Parameter Root Uri** text boxes.</span></span>
   - <span data-ttu-id="8ee45-244">사용 약관을 검토한 후 **위에 명시된 사용 약관에 동의함** 확인란을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-244">Review the terms and conditions, then click the **I agree to the terms and conditions stated above** checkbox.</span></span>
   - <span data-ttu-id="8ee45-245">**구매** 단추를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-245">Click the **Purchase** button.</span></span>
3. <span data-ttu-id="8ee45-246">배포가 완료될 때가지 기다립니다.</span><span class="sxs-lookup"><span data-stu-id="8ee45-246">Wait for the deployment to complete.</span></span>

<!-- markdownlint-enable MD033 -->

<span data-ttu-id="8ee45-247">연결 문제를 해결하려면 [하이브리드 VPN 연결 문제 해결](./troubleshoot-vpn.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="8ee45-247">To troubleshoot the connection, see [Troubleshoot a hybrid VPN connection](./troubleshoot-vpn.md).</span></span>

<!-- links -->

[adds-extend-domain]: ../identity/adds-extend-domain.md
[windows-vm-ra]: ../virtual-machines-windows/index.md
[linux-vm-ra]: ../virtual-machines-linux/index.md

[azure-cli]: /azure/virtual-machines-command-line-tools
[azure-virtual-network]: /azure/virtual-network/virtual-networks-overview
[vpn-appliance]: /azure/vpn-gateway/vpn-gateway-about-vpn-devices
[azure-vpn-gateway]: https://azure.microsoft.com/services/vpn-gateway/
[azure-gateway-charges]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[azure-gateway-skus]: /azure/vpn-gateway/vpn-gateway-about-vpngateways#gwsku
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[vpn-gateway-multi-site]: /azure/vpn-gateway/vpn-gateway-multi-site
[policy-based-routing]: https://en.wikipedia.org/wiki/Policy-based_routing
[route-based-routing]: https://en.wikipedia.org/wiki/Static_routing
[sla-for-vpn-gateway]: https://azure.microsoft.com/support/legal/sla/vpn-gateway/
[additional-firewall-rules]: https://technet.microsoft.com/library/dn786406.aspx#firewall
[nagios]: https://www.nagios.org/
[changing-SKUs]: https://azure.microsoft.com/blog/azure-virtual-network-gateway-improvements/
[gateway-diagnostic-logs]: https://blogs.technet.microsoft.com/keithmayer/2016/10/12/step-by-step-capturing-azure-resource-manager-arm-vnet-gateway-diagnostic-logs/
[rras-logging]: https://www.petri.com/enable-diagnostic-logging-in-windows-server-2012-r2-routing-and-remote-access
[forced-tunneling]: /azure/vpn-gateway/vpn-gateway-about-forced-tunneling
[vpn-appliances]: /azure/vpn-gateway/vpn-gateway-about-vpn-devices
[visio-download]: https://archcenter.blob.core.windows.net/cdn/hybrid-network-architectures.vsdx
[vpn-appliance-ipsec]: /azure/vpn-gateway/vpn-gateway-about-vpn-devices#ipsec-parameters
[azure-cli]: /cli/azure/install-azure-cli
[CIDR]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
