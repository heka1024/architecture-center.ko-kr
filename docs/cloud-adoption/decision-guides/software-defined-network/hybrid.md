---
title: 'CAF: 소프트웨어 정의 네트워크 - 하이브리드 네트워크'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 하이브리드 네트워크를 통해 클라우드 가상 네트워크를 온-프레미스 리소스에 연결하는 방법을 설명합니다.
author: rotycenh
ms.openlocfilehash: 02d181db0ae9baef3b453b8623d212b624f6b16a
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901293"
---
# <a name="software-defined-networks-hybrid-network"></a><span data-ttu-id="72b0b-103">소프트웨어 정의 네트워크: 하이브리드 네트워크</span><span class="sxs-lookup"><span data-stu-id="72b0b-103">Software Defined Networks: Hybrid network</span></span>

<span data-ttu-id="72b0b-104">하이브리드 클라우드 네트워크 아키텍처는 가상 네트워크가 온-프레미스 리소스 및 서비스에, 그리고 그 반대로 액세스하는 것을 허용하며, ExpressRoute 같은 전용 WAN 연결 또는 기타 연결 방법을 사용하여 네트워크를 직접 연결합니다.</span><span class="sxs-lookup"><span data-stu-id="72b0b-104">The hybrid cloud network architecture allows virtual networks to access your on-premises resources and services and vice versa, using a Dedicated WAN connection such as ExpressRoute or other connection method to directly connect the networks.</span></span>

![하이브리드 네트워크](../../../reference-architectures/hybrid-networking/images/expressroute.png)

<span data-ttu-id="72b0b-106">클라우드 네이티브 가상 네트워크 아키텍처를 기반으로 하는 하이브리드 가상 네트워크는 처음에 생성될 때 격리됩니다.</span><span class="sxs-lookup"><span data-stu-id="72b0b-106">Building on the cloud native virtual network architecture, a hybrid virtual network is isolated when initially created.</span></span> <span data-ttu-id="72b0b-107">온-프레미스 환경에 대한 연결을 추가하면 온-프레미스 네트워크 액세스 권한이 부여되지만, 가상 네트워크의 리소스를 대상으로 하는 그 외의 모든 인바운드 트래픽은 명시적으로 허용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="72b0b-107">Adding connectivity to the on-premises environment grants access to and from the on-premises network, although all other inbound traffic targeting resources in the virtual network need to be explicitly allowed.</span></span> <span data-ttu-id="72b0b-108">액세스를 제한하는 가상 방화벽 디바이스 및 회람 규칙을 사용하여 연결을 보호할 수도 있고, 클라우드 네이티브 라우팅 기능을 사용하여 또는 트래픽을 관리할 NVA(네트워크 가상 어플라이언스)를 배포하여 두 네트워크 간에 액세스할 수 있는 서비스를 정확하게 지정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="72b0b-108">You can secure the connection using virtual firewall devices and routing rules to limit access or you can specify exactly what services can be accessed between the two networks using cloud-native routing features or deploying network virtual appliances (NVAs) to manage traffic.</span></span>

<span data-ttu-id="72b0b-109">하이브리드 네트워킹 아키텍처는 VPN 연결을 지원하지만, 더 높은 성능과 향상된 보안을 제공하는 ExpressRoute 같은 전용 WAN 연결이 주로 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="72b0b-109">Although the hybrid networking architecture supports VPN connections, dedicated WAN connections like ExpressRoute are generally preferred due to higher performance and increased security.</span></span>

## <a name="hybrid-assumptions"></a><span data-ttu-id="72b0b-110">하이브리드 가정</span><span class="sxs-lookup"><span data-stu-id="72b0b-110">Hybrid assumptions</span></span>

<span data-ttu-id="72b0b-111">하이브리드 가상 네트워크를 배포할 때 다음과 같이 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="72b0b-111">Deploying a hybrid virtual network assumes the following:</span></span>

- <span data-ttu-id="72b0b-112">IT 보안 팀에서는 클라우드 기반 가상 네트워크를 믿고 온-프레미스 시스템과 직접 통신할 수 있도록 온-프레미스 및 클라우드 기반 네트워크 보안 정책을 적절하게 조정했습니다.</span><span class="sxs-lookup"><span data-stu-id="72b0b-112">Your IT security teams have aligned on-premises and cloud-based network security policy to ensure cloud-based virtual networks can be trusted to communicated directly with on-premises systems.</span></span>
- <span data-ttu-id="72b0b-113">클라우드 기반 워크로드가 온-프레미스 또는 타사 네트워크에 호스팅되는 스토리지, 애플리케이션 및 서비스에 액세스해야 하거나, 온-프레미스의 사용자 또는 애플리케이션이 클라우드 호스팅 리소스에 액세스해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="72b0b-113">Your cloud-based workloads require access to storage, applications, and services hosted on your on-premises or third-party networks, or your users or applications in your on-premises need access to cloud-hosted resources.</span></span>
- <span data-ttu-id="72b0b-114">온-프레미스 리소스를 사용하는 기존 애플리케이션 및 서비스를 마이그레이션해야 하지만, 이러한 종속성을 제거하기 위한 재개발에 리소스를 투입할 생각은 없습니다.</span><span class="sxs-lookup"><span data-stu-id="72b0b-114">You need to migrate existing applications and services that depend on on-premises resources, but don't want to expend the resources on redevelopment to remove those dependencies.</span></span>
- <span data-ttu-id="72b0b-115">온-프레미스 네트워크와 클라우드 공급 기업 간에 VPN 또는 전용 WAN 연결 구현을 방해하는 회사 정책, 규정 요구 사항 또는 기술 호환성 문제가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="72b0b-115">Implementing a VPN or dedicated WAN connection between your on-premises networks and cloud provider is not prevented by corporate policy, regulatory requirements, or technical compatibility issues.</span></span>
- <span data-ttu-id="72b0b-116">구독 리소스 제한을 무시하기 위해 워크로드에 여러 구독이 필요하지 않거나, 워크로드에 여러 구독이 필요하지만 여러 구독에 분산된 리소스에서 사용하는 연결 또는 공유 서비스를 중앙에서 관리할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="72b0b-116">Your workloads either do not require multiple subscriptions to bypass subscription resource limits, OR your workloads involve multiple subscriptions but do not require central management of connectivity or shared services used by resources spread across multiple subscriptions.</span></span>

<span data-ttu-id="72b0b-117">클라우드 도입 팀은 하이브리드 가상 네트워킹 아키텍처의 구현을 살펴볼 때 다음과 같은 문제를 고려해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="72b0b-117">Your Cloud Adoption team should consider the following issues when looking at implementing a hybrid virtual networking architecture:</span></span>

- <span data-ttu-id="72b0b-118">온-프레미스 네트워크를 클라우드 네트워크와 연결하면 보안 요구 사항이 복잡해집니다.</span><span class="sxs-lookup"><span data-stu-id="72b0b-118">Connecting on-premises networks with cloud networks increases the complexity of your security requirements.</span></span> <span data-ttu-id="72b0b-119">두 네트워크를 외부 취약성 및 하이브리드 환경의 양면에서 시도되는 무단 액세스로부터 보호해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="72b0b-119">Both networks need to be secured against external vulnerabilities and unauthorized access from both sides of the hybrid environment.</span></span>
- <span data-ttu-id="72b0b-120">하이브리드 클라우드 환경 내에서 워크로드의 수와 크기를 조정하면 라우팅 및 트래픽 관리가 매우 복잡해질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="72b0b-120">Scaling the number and size of workloads within a hybrid cloud environment can add significant complexity to routing and traffic management.</span></span>
- <span data-ttu-id="72b0b-121">조직 전체의 거버넌스를 일관적으로 유지하는 호환성 관리 및 액세스 제어 정책을 개발해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="72b0b-121">You will need to develop compatible management and access control policies to maintain consistent governance throughout your organization.</span></span>

## <a name="learn-more"></a><span data-ttu-id="72b0b-122">자세한 정보</span><span class="sxs-lookup"><span data-stu-id="72b0b-122">Learn more</span></span>

<span data-ttu-id="72b0b-123">Azure 플랫폼의 하이브리드 네트워킹에 대한 자세한 내용은 다음을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="72b0b-123">See the following for more information about hybrid networking in the Azure platform.</span></span>

- <span data-ttu-id="72b0b-124">[하이브리드 네트워크 참조 아키텍처](../../../reference-architectures/hybrid-networking/expressroute.md).</span><span class="sxs-lookup"><span data-stu-id="72b0b-124">[Hybrid network reference architecture](../../../reference-architectures/hybrid-networking/expressroute.md).</span></span> <span data-ttu-id="72b0b-125">Azure 하이브리드 가상 네트워크는 ExpressRoute 회로 또는 Azure VPN을 사용하여 가상 네트워크를 조직의 기존 비 Azure 호스팅 IT 자산과 연결합니다.</span><span class="sxs-lookup"><span data-stu-id="72b0b-125">Azure hybrid virtual networks use either an ExpressRoute circuit or Azure VPN to connect your virtual network with your organization's existing non-Azure hosted IT assets.</span></span> <span data-ttu-id="72b0b-126">이 문서에서는 Azure의 하이브리드 네트워크 만들기 옵션을 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="72b0b-126">This article discusses the options for creating a hybrid network in Azure.</span></span>
