---
title: Azure의 SAP 워크로드에 대한 개발/테스트 환경
description: SAP 워크로드에 대한 개발/테스트 환경을 빌드합니다.
author: AndrewDibbins
ms.date: 7/11/18
ms.openlocfilehash: b47e4cb527d3e4ecd74bee7bcf08f2794da56d6c
ms.sourcegitcommit: 62945777e519d650159f0f963a2489b6bb6ce094
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2018
ms.locfileid: "48876802"
---
# <a name="devtest-environments-for-sap-workloads-on-azure"></a><span data-ttu-id="ad326-103">Azure의 SAP 워크로드에 대한 개발/테스트 환경</span><span class="sxs-lookup"><span data-stu-id="ad326-103">Dev/test environments for SAP workloads on Azure</span></span>

<span data-ttu-id="ad326-104">이 예제는 Azure의 Windows 또는 Linux에서 SAP NetWeaver용 개발/테스트 환경을 설정하는 방법을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-104">This example shows how to establish a dev/test environment for SAP NetWeaver in a Windows or Linux environment on Azure.</span></span> <span data-ttu-id="ad326-105">사용되는 데이터베이스는 지원되는 모든 DBMS(SAP HANA가 아님)를 의미하는 SAP 용어인 AnyDB입니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-105">The database used is AnyDB, the SAP term for any supported DBMS (that isn't SAP HANA).</span></span> <span data-ttu-id="ad326-106">이 아키텍처는 비프로덕션 환경에 대해 설계되었기 때문에 단일 VM(가상 머신)으로 배포되며 크기는 조직의 요구에 맞게 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-106">Because this architecture is designed for non-production environments, it's deployed with just a single virtual machine (VM) and it's size can be changed to accommodate your organization's needs.</span></span>

<span data-ttu-id="ad326-107">프로덕션 사용 사례의 경우 아래에서 사용할 수 있는 SAP 참조 아키텍처를 검토합니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-107">For production use cases review the SAP reference architectures available below:</span></span>

* <span data-ttu-id="ad326-108">[AnyDB용 SAP NetWeaver][sap-netweaver]</span><span class="sxs-lookup"><span data-stu-id="ad326-108">[SAP NetWeaver for AnyDB][sap-netweaver]</span></span>
* <span data-ttu-id="ad326-109">[SAP S/4HANA][sap-hana]</span><span class="sxs-lookup"><span data-stu-id="ad326-109">[SAP S/4HANA][sap-hana]</span></span>
* <span data-ttu-id="ad326-110">[Azure의 SAP(대규모 인스턴스)][sap-large]</span><span class="sxs-lookup"><span data-stu-id="ad326-110">[SAP on Azure large instances][sap-large]</span></span>

## <a name="relevant-use-cases"></a><span data-ttu-id="ad326-111">관련 사용 사례</span><span class="sxs-lookup"><span data-stu-id="ad326-111">Relevant use cases</span></span>

<span data-ttu-id="ad326-112">이 시나리오에 적합한 사용 사례는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-112">Consider this scenario for the following use cases:</span></span>

* <span data-ttu-id="ad326-113">중요하지 않은 비생산적 SAP 워크로드(샌드박스, 개발, 테스트, 품질 보증)</span><span class="sxs-lookup"><span data-stu-id="ad326-113">Non-critical SAP non-productive workloads (sandbox, development, test, quality assurance)</span></span>
* <span data-ttu-id="ad326-114">중요하지 않은 SAP 비즈니스 워크로드</span><span class="sxs-lookup"><span data-stu-id="ad326-114">Non-critical SAP business workloads</span></span>

## <a name="architecture"></a><span data-ttu-id="ad326-115">아키텍처</span><span class="sxs-lookup"><span data-stu-id="ad326-115">Architecture</span></span>

![SAP 워크로드용 개발/테스트 환경을 위한 아키텍처 다이어그램](media/architecture-sap-dev-test.png)

<span data-ttu-id="ad326-117">이 시나리오는 단일 가상 머신에 단일 SAP 시스템 데이터베이스와 SAP 응용 프로그램 서버를 프로비전하는 방법을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-117">This scenario demonstrates provisioning a single SAP system database and SAP application server on a single virtual machine.</span></span> <span data-ttu-id="ad326-118">시나리오를 통한 데이터 흐름은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-118">The data flows through the scenario as follows:</span></span>

1. <span data-ttu-id="ad326-119">고객이 SAP 사용자 인터페이스 또는 다른 클라이언트 도구(Excel, 웹 브라우저 또는 기타 웹 응용 프로그램)를 사용하여 Azure 기반 SAP 시스템에 액세스합니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-119">Customers use the SAP user interface or other client tools (Excel, a web browser, or other web application) to access the Azure-based SAP system.</span></span>
2. <span data-ttu-id="ad326-120">설정된 ExpressRoute를 사용하여 연결이 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-120">Connectivity is provided through the use of an established ExpressRoute.</span></span> <span data-ttu-id="ad326-121">ExpressRoute 연결은 Azure의 ExpressRoute 게이트웨이에서 종료됩니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-121">The ExpressRoute connection is terminated in Azure at the ExpressRoute gateway.</span></span> <span data-ttu-id="ad326-122">네트워크 트래픽은 ExpressRoute 게이트웨이를 통해 게이트웨이 서브넷으로 라우팅되고, 게이트웨이 서브넷에서 응용 프로그램 계층 스포크 서브넷([허브-스포크][hub-spoke] 패턴 참조)으로 라우팅되며, 네트워크 보안 게이트웨이를 통해 SAP 응용 프로그램 가상 머신으로 라우팅됩니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-122">Network traffic routes through the ExpressRoute gateway to the gateway subnet, and from the gateway subnet to the application-tier spoke subnet (see the [hub-spoke][hub-spoke] pattern) and via a Network Security Gateway to the SAP application virtual machine.</span></span>
3. <span data-ttu-id="ad326-123">ID 관리 서버는 인증 서비스를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-123">The identity management servers provide authentication services.</span></span>
4. <span data-ttu-id="ad326-124">점프 박스는 로컬 관리 기능을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-124">The jump box provides local management capabilities.</span></span>

### <a name="components"></a><span data-ttu-id="ad326-125">구성 요소</span><span class="sxs-lookup"><span data-stu-id="ad326-125">Components</span></span>

* <span data-ttu-id="ad326-126">[가상 네트워크](/azure/virtual-network/virtual-networks-overview)는 Azure 내에서 네트워크 통신의 기초입니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-126">[Virtual Networks](/azure/virtual-network/virtual-networks-overview) are the basis of network communication within Azure.</span></span>
* <span data-ttu-id="ad326-127">[가상 머신](/azure/virtual-machines/windows/overview) Azure Virtual Machines는 Windows 또는 Linux 서버를 사용하여 안전하고 가상화된 주문형 대규모 인프라를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-127">[Virtual Machine](/azure/virtual-machines/windows/overview) Azure Virtual Machines provides on-demand, high-scale, secure, virtualized infrastructure using Windows or Linux Server.</span></span>
* <span data-ttu-id="ad326-128">[ExpressRoute](/azure/expressroute/expressroute-introduction)를 사용하면 연결 공급자가 지원하는 개인 연결을 통해 온-프레미스 네트워크를 Microsoft 클라우드로 확장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-128">[ExpressRoute](/azure/expressroute/expressroute-introduction) lets you extend your on-premises networks into the Microsoft cloud over a private connection facilitated by a connectivity provider.</span></span>
* <span data-ttu-id="ad326-129">[네트워크 보안 그룹](/azure/virtual-network/security-overview)을 사용하면 네트워크 트래픽을 가상 네트워크의 리소스로 제한할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-129">[Network Security Group](/azure/virtual-network/security-overview) lets you limit network traffic to resources in a virtual network.</span></span> <span data-ttu-id="ad326-130">네트워크 보안 그룹에는 원본 또는 대상 IP 주소, 포트 및 프로토콜에 따라 인바운드 또는 아웃바운드 네트워크 트래픽을 허용하거나 거부하는 보안 규칙 목록이 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-130">A network security group contains a list of security rules that allow or deny inbound or outbound network traffic based on source or destination IP address, port, and protocol.</span></span> 
* <span data-ttu-id="ad326-131">[리소스 그룹](/azure/azure-resource-manager/resource-group-overview#resource-groups)은 Azure 리소스에 대한 논리 컨테이너 역할을 합니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-131">[Resource Groups](/azure/azure-resource-manager/resource-group-overview#resource-groups) act as logical containers for Azure resources.</span></span>

## <a name="considerations"></a><span data-ttu-id="ad326-132">고려 사항</span><span class="sxs-lookup"><span data-stu-id="ad326-132">Considerations</span></span>

### <a name="availability"></a><span data-ttu-id="ad326-133">가용성</span><span class="sxs-lookup"><span data-stu-id="ad326-133">Availability</span></span>

 <span data-ttu-id="ad326-134">Microsoft는 단일 VM 인스턴스에 대한 SLA(서비스 수준 계약)를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-134">Microsoft offers a service level agreement (SLA) for single VM instances.</span></span> <span data-ttu-id="ad326-135">Virtual Machines와 관련된 Microsoft Azure 서비스 수준 계약에 대한 자세한 내용은 [Virtual Machines에 대한 SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="ad326-135">For more information on Microsoft Azure Service Level Agreement for Virtual Machines [SLA For Virtual Machines](https://azure.microsoft.com/support/legal/sla/virtual-machines)</span></span>

### <a name="scalability"></a><span data-ttu-id="ad326-136">확장성</span><span class="sxs-lookup"><span data-stu-id="ad326-136">Scalability</span></span>

<span data-ttu-id="ad326-137">확장 가능한 솔루션 설계에 대한 일반적인 지침은 Azure 아키텍처 센터의 [확장성 검사 목록][scalability]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="ad326-137">For general guidance on designing scalable solutions, see the [scalability checklist][scalability] in the Azure Architecture Center.</span></span>

### <a name="security"></a><span data-ttu-id="ad326-138">보안</span><span class="sxs-lookup"><span data-stu-id="ad326-138">Security</span></span>

<span data-ttu-id="ad326-139">보안 솔루션 설계에 대한 일반적인 지침은 [Azure 보안 설명서][security]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="ad326-139">For general guidance on designing secure solutions, see the [Azure Security Documentation][security].</span></span>

### <a name="resiliency"></a><span data-ttu-id="ad326-140">복원력</span><span class="sxs-lookup"><span data-stu-id="ad326-140">Resiliency</span></span>

<span data-ttu-id="ad326-141">복원력 있는 솔루션 설계에 대한 일반적인 지침은 [복원력 있는 Azure 응용 프로그램 디자인][resiliency]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="ad326-141">For general guidance on designing resilient solutions, see [Designing resilient applications for Azure][resiliency].</span></span>

## <a name="pricing"></a><span data-ttu-id="ad326-142">가격</span><span class="sxs-lookup"><span data-stu-id="ad326-142">Pricing</span></span>

<span data-ttu-id="ad326-143">이 시나리오를 실행하는 데 들어가는 비용을 살펴볼 수 있도록, 모든 서비스가 비용 계산기에 미리 구성되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-143">To help you explore the cost of running this scenario, all of the services are pre-configured in the cost calculator examples below.</span></span> <span data-ttu-id="ad326-144">특정 사용 사례에 대한 가격이 변경되는 정도를 확인하려면 필요한 트래픽에 맞게 적절한 변수를 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-144">To see how the pricing would change for your particular use case, change the appropriate variables to match your expected traffic.</span></span>

<span data-ttu-id="ad326-145">받을 것으로 예상되는 트래픽 양에 따라 다음과 같은 네 가지 샘플 비용 프로필이 제공되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-145">We have provided four sample cost profiles based on amount of traffic you expect to receive:</span></span>

|<span data-ttu-id="ad326-146">크기</span><span class="sxs-lookup"><span data-stu-id="ad326-146">Size</span></span>|<span data-ttu-id="ad326-147">SAP</span><span class="sxs-lookup"><span data-stu-id="ad326-147">SAPs</span></span>|<span data-ttu-id="ad326-148">VM 유형</span><span class="sxs-lookup"><span data-stu-id="ad326-148">VM Type</span></span>|<span data-ttu-id="ad326-149">Storage</span><span class="sxs-lookup"><span data-stu-id="ad326-149">Storage</span></span>|<span data-ttu-id="ad326-150">Azure 요금 계산기</span><span class="sxs-lookup"><span data-stu-id="ad326-150">Azure Pricing Calculator</span></span>|
|----|----|-------|-------|---------------|
|<span data-ttu-id="ad326-151">작음</span><span class="sxs-lookup"><span data-stu-id="ad326-151">Small</span></span>|<span data-ttu-id="ad326-152">8000</span><span class="sxs-lookup"><span data-stu-id="ad326-152">8000</span></span>|<span data-ttu-id="ad326-153">D8s_v3</span><span class="sxs-lookup"><span data-stu-id="ad326-153">D8s_v3</span></span>|<span data-ttu-id="ad326-154">2xP20, 1xP10</span><span class="sxs-lookup"><span data-stu-id="ad326-154">2xP20, 1xP10</span></span>|[<span data-ttu-id="ad326-155">소형</span><span class="sxs-lookup"><span data-stu-id="ad326-155">Small</span></span>](https://azure.com/e/9d26b9612da9466bb7a800eab56e71d1)|
|<span data-ttu-id="ad326-156">중간</span><span class="sxs-lookup"><span data-stu-id="ad326-156">Medium</span></span>|<span data-ttu-id="ad326-157">16000</span><span class="sxs-lookup"><span data-stu-id="ad326-157">16000</span></span>|<span data-ttu-id="ad326-158">D16s_v3</span><span class="sxs-lookup"><span data-stu-id="ad326-158">D16s_v3</span></span>|<span data-ttu-id="ad326-159">3xP20, 1xP10</span><span class="sxs-lookup"><span data-stu-id="ad326-159">3xP20, 1xP10</span></span>|[<span data-ttu-id="ad326-160">중형</span><span class="sxs-lookup"><span data-stu-id="ad326-160">Medium</span></span>](https://azure.com/e/465bd07047d148baab032b2f461550cd)|
<span data-ttu-id="ad326-161">큰</span><span class="sxs-lookup"><span data-stu-id="ad326-161">Large</span></span>|<span data-ttu-id="ad326-162">32000</span><span class="sxs-lookup"><span data-stu-id="ad326-162">32000</span></span>|<span data-ttu-id="ad326-163">E32s_v3</span><span class="sxs-lookup"><span data-stu-id="ad326-163">E32s_v3</span></span>|<span data-ttu-id="ad326-164">3xP20, 1xP10</span><span class="sxs-lookup"><span data-stu-id="ad326-164">3xP20, 1xP10</span></span>|[<span data-ttu-id="ad326-165">대형</span><span class="sxs-lookup"><span data-stu-id="ad326-165">Large</span></span>](https://azure.com/e/ada2e849d68b41c3839cc976000c6931)|
<span data-ttu-id="ad326-166">초대형</span><span class="sxs-lookup"><span data-stu-id="ad326-166">Extra Large</span></span>|<span data-ttu-id="ad326-167">64000</span><span class="sxs-lookup"><span data-stu-id="ad326-167">64000</span></span>|<span data-ttu-id="ad326-168">M64s</span><span class="sxs-lookup"><span data-stu-id="ad326-168">M64s</span></span>|<span data-ttu-id="ad326-169">4xP20, 1xP10</span><span class="sxs-lookup"><span data-stu-id="ad326-169">4xP20, 1xP10</span></span>|[<span data-ttu-id="ad326-170">초대형</span><span class="sxs-lookup"><span data-stu-id="ad326-170">Extra Large</span></span>](https://azure.com/e/975fb58a965c4fbbb54c5c9179c61cef)|

> [!NOTE]
> <span data-ttu-id="ad326-171">이 가격 책정은 VM 및 저장소 비용만 나타내는 가이드입니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-171">This pricing is a guide that only indicates the VMs and storage costs.</span></span> <span data-ttu-id="ad326-172">네트워킹, 백업 저장소 및 데이터 수신/송신 요금은 제외됩니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-172">It excludes networking, backup storage, and data ingress/egress charges.</span></span>

* <span data-ttu-id="ad326-173">[소형](https://azure.com/e/9d26b9612da9466bb7a800eab56e71d1): 소형 시스템은 8개 vCPU, 32GB RAM 및 200GB 임시 저장소가 있는 D8s_v3 VM 유형으로 구성되며, 2개 512GB 및 1개 128GB 프리미엄 저장소 디스크도 추가로 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-173">[Small](https://azure.com/e/9d26b9612da9466bb7a800eab56e71d1): A small system consists of VM type D8s_v3 with 8x vCPUs, 32 GB RAM and 200 GB temp storage, additionally two 512 GB and one 128 GB premium storage disks.</span></span>
* <span data-ttu-id="ad326-174">[중형](https://azure.com/e/465bd07047d148baab032b2f461550cd): 중형 시스템은 16개 vCPU, 64GB RAM 및 400GB 임시 저장소가 있는 D16s_v3 VM 유형으로 구성되며, 3개 512GB 및 1개 128GB 프리미엄 저장소 디스크도 추가로 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-174">[Medium](https://azure.com/e/465bd07047d148baab032b2f461550cd): A medium system consists of VM type D16s_v3 with 16x vCPUs, 64 GB RAM and 400 GB temp storage, additionally three 512 GB and one 128 GB premium storage disks.</span></span>
* <span data-ttu-id="ad326-175">[대형](https://azure.com/e/ada2e849d68b41c3839cc976000c6931): 대형 시스템은 32개 vCPU, 256GB RAM 및 512GB 임시 저장소가 있는 E32s_v3 VM 유형으로 구성되며, 3개 512GB 및 1개 128GB 프리미엄 저장소 디스크도 추가로 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-175">[Large](https://azure.com/e/ada2e849d68b41c3839cc976000c6931): A large system consists of VM type E32s_v3 with 32x vCPUs, 256 GB RAM and 512 GB temp storage, additionally three 512GB and one 128GB premium storage disks.</span></span>
* <span data-ttu-id="ad326-176">[초대형](https://azure.com/e/975fb58a965c4fbbb54c5c9179c61cef): 초대형 시스템은 64개 vCPU, 1024GB RAM 및 2000GB 임시 저장소가 있는 M64s VM 유형으로 구성되며, 4개 512GB 및 1개 128GB 프리미엄 저장소 디스크도 추가로 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-176">[Extra Large](https://azure.com/e/975fb58a965c4fbbb54c5c9179c61cef): An extra large system consists of a VM type M64s with 64x vCPUs, 1024 GB RAM and 2000 GB temp storage, additionally four 512 GB and one 128 GB premium storage disks.</span></span>

## <a name="deployment"></a><span data-ttu-id="ad326-177">배포</span><span class="sxs-lookup"><span data-stu-id="ad326-177">Deployment</span></span>

<span data-ttu-id="ad326-178">이 시나리오를 위한 기본 인프라를 배포하려면 여기를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-178">Click here to deploy the underlying infrastructure for this scenario.</span></span>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Fsolution-architectures%2Fmaster%2Fapps%2Fsap-2tier%2Fazuredeploy.json" target="_blank">
    <img src="https://azuredeploy.net/deploybutton.png"/>
</a>

> [!NOTE]
> <span data-ttu-id="ad326-179">SAP 및 Oracle은 이 배포 중에 설치되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-179">SAP and Oracle are not installed during this deployment.</span></span> <span data-ttu-id="ad326-180">이러한 구성 요소를 별도로 배포해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ad326-180">You will need to deploy these components separately.</span></span>

<!-- links -->
[resiliency]: /azure/architecture/resiliency/
[security]: /azure/security/
[scalability]: /azure/architecture/checklist/scalability
[sap-netweaver]: /azure/architecture/reference-architectures/sap/sap-netweaver
[sap-hana]: /azure/architecture/reference-architectures/sap/sap-s4hana
[sap-large]: /azure/architecture/reference-architectures/sap/hana-large-instances
[hub-spoke]: /azure/architecture/reference-architectures/hybrid-networking/hub-spoke
