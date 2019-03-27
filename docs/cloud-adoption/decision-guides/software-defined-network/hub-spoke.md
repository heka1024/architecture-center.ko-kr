---
title: 'CAF: 소프트웨어 정의 네트워크 - 클라우드 네이티브'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 클라우드 네이티브 가상 네트워킹 서비스 설명
author: rotycenh
ms.openlocfilehash: e0ad6803f2ddc982ea0c42c59fdf2486e1710433
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58241133"
---
# <a name="software-defined-networks-hub-and-spoke"></a><span data-ttu-id="21566-103">소프트웨어 정의 네트워크: 허브 및 스포크</span><span class="sxs-lookup"><span data-stu-id="21566-103">Software Defined Networks: Hub and Spoke</span></span>

<span data-ttu-id="21566-104">허브 및 스포크 네트워킹 모델은 Azure 기반 클라우드 네트워크 인프라를 연결된 여러 가상 네트워크에 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="21566-104">The hub and spoke networking model organizes your Azure-based cloud network infrastructure into multiple connected virtual networks.</span></span> <span data-ttu-id="21566-105">이 모델을 사용하면 보다 효율적으로 일반 통신 또는 보안 요구 사항을 관리하고 잠재적인 구독 제한 사항을 처리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="21566-105">This model allows you to more efficiently manage common communication or security requirements and deal with potential subscription limitations.</span></span>

<span data-ttu-id="21566-106">허브 및 스포크 모델에서 *허브*는 외부 연결을 관리하고 여러 워크로드에서 사용된 서비스를 호스트하기 위해 중앙 위치로 작동하는 가상 네트워크입니다.</span><span class="sxs-lookup"><span data-stu-id="21566-106">In the hub and spoke model, the *hub* is a virtual network that acts as a central location for managing external connectivity and hosting services used by multiple workloads.</span></span> <span data-ttu-id="21566-107">*스포크*는 워크로드를 호스트하고 [가상 네트워크 피어링](/virtual-network/virtual-network-peering-overview)을 통해 중앙 허브에 연결되는 가상 네트워크입니다.</span><span class="sxs-lookup"><span data-stu-id="21566-107">The *spokes* are virtual networks that host workloads and connect to the central hub through [virtual network peering](/virtual-network/virtual-network-peering-overview).</span></span>

<span data-ttu-id="21566-108">워크로드 스포크 네트워크 안팎에 전달하는 모든 트래픽은 라우팅되고, 검사되고 또는 그렇지 않으면 중앙에서 관리되는 IT 규칙 또는 프로세스에 의해 관리되는 허브 네트워크를 통해 라우팅됩니다.</span><span class="sxs-lookup"><span data-stu-id="21566-108">All traffic passing in or out of the workload spoke networks is routed through the hub network where it can be routed, inspected, or otherwise managed by centrally managed IT rules or processes.</span></span>

<span data-ttu-id="21566-109">이 모델의 목적은 다음과 같은 문제를 해결하는 데 있습니다.</span><span class="sxs-lookup"><span data-stu-id="21566-109">This model aims to address the following issues:</span></span>

- <span data-ttu-id="21566-110">비용 절감 및 관리 효율성.</span><span class="sxs-lookup"><span data-stu-id="21566-110">Cost savings and management efficiency.</span></span> <span data-ttu-id="21566-111">NVA(네트워크 가상 어플라이언스), DNS 서버와 같이 여러 워크로드에서 공유할 수 있는 서비스를 하나의 위치로 일원화하면 IT가 여러 워크로드에서 리소스 중복 및 관리 노력을 최소화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="21566-111">Centralizing services that can be shared by multiple workloads, such as network virtual appliances (NVAs) and DNS servers, in a single location allows IT to minimize redundant resources and management effort across multiple workloads.</span></span>
- <span data-ttu-id="21566-112">구독 제한 문제를 해결합니다.</span><span class="sxs-lookup"><span data-stu-id="21566-112">Overcoming subscriptions limits.</span></span> <span data-ttu-id="21566-113">대규모 클라우드 기반 워크로드는 단일 Azure 구독 내에 허용되는 것보다 많은 리소스를 사용해야 할 수 있습니다([구독 제한](/azure/azure-subscription-service-limits) 참조).</span><span class="sxs-lookup"><span data-stu-id="21566-113">Large cloud-based workloads may require the use of more resources than are allowed within a single Azure subscription (see [subscription limits](/azure/azure-subscription-service-limits)).</span></span> <span data-ttu-id="21566-114">워크로드 가상 네트워크를 서로 다른 구독에서 중앙 허브로 피어링하면 이러한 제한 문제를 해결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="21566-114">Peering workload virtual networks from different subscriptions to a central hub can overcome these limits.</span></span>
- <span data-ttu-id="21566-115">문제 구분.</span><span class="sxs-lookup"><span data-stu-id="21566-115">Separation of concerns.</span></span> <span data-ttu-id="21566-116">중앙 IT 팀과 워크로드 팀 간에 개별 워크로드를 배포할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="21566-116">The ability to deploy individual workloads between central IT teams and workloads teams.</span></span>

<span data-ttu-id="21566-117">다음 다이어그램에서는 중앙에서 관리되는 하이브리드 연결을 포함하여 허브 및 스포크 아키텍처 예제를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="21566-117">The following diagram shows an example hub and spoke architecture including centrally managed hybrid connectivity.</span></span>

![허브-스포크 네트워크 아키텍처](../../../reference-architectures/hybrid-networking/images/hub-spoke.png)

<span data-ttu-id="21566-119">허브 및 스포크 아키텍처는 하이브리드 네트워킹 아키텍처와 함께 자주 사용되며, 여러 워크로드 간에 공유된 온-프레미스 환경에 중앙에서 관리되는 연결을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="21566-119">The hub and spoke architecture is often used alongside the hybrid networking architecture, providing a centrally managed connection to your on-premises environment shared between multiple workloads.</span></span> <span data-ttu-id="21566-120">이 시나리오에서 워크로드 및 온-프레미스 간에 이동하는 모든 트래픽은 관리 및 보호할 수 있는 허브를 통해 전달됩니다.</span><span class="sxs-lookup"><span data-stu-id="21566-120">In this scenario, all traffic traveling between the workloads and on-premises passes through the hub where it can be managed and secured.</span></span>

## <a name="hub-and-spoke-assumptions"></a><span data-ttu-id="21566-121">허브 및 스포크 가정</span><span class="sxs-lookup"><span data-stu-id="21566-121">Hub and spoke assumptions</span></span>

<span data-ttu-id="21566-122">허브 및 스포크 가상 네트워크 아키텍처의 구현은 다음을 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="21566-122">Implementing a hub and spoke virtual networking architecture assumes the following:</span></span>

- <span data-ttu-id="21566-123">클라우드 배포는 모두 DNS 또는 디렉터리 서비스와 같은 일반적인 서비스 세트를 사용하는 개발, 테스트 및 프로덕션과 같은 별도 작업 환경에서 호스팅되는 워크로드를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="21566-123">Your cloud deployments will involve workloads hosted in separate working environments, such as development, test, and production, that all rely on a set of common services such as DNS or directory services.</span></span>
- <span data-ttu-id="21566-124">워크로드는 서로 통신할 필요가 없지만 일반적인 외부 통신 및 공유된 서비스 요구 사항을 보유합니다.</span><span class="sxs-lookup"><span data-stu-id="21566-124">Your workloads do not need to communicate with each other but have common external communications and shared services requirements.</span></span>
- <span data-ttu-id="21566-125">워크로드에는 단일 Azure 구독 내에서 사용할 수 있는 것보다 많은 리소스가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="21566-125">Your workloads require more resources than are available within a single Azure subscription.</span></span>
- <span data-ttu-id="21566-126">자신의 리소스에 대한 위임된 관리 권한을 작업 팀에 제공하면서 외부 연결에 대한 중앙 집중식 보안 제어를 유지해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="21566-126">You need to provide workload teams with delegated management rights over their own resources while maintaining central security control over external connectivity.</span></span>

## <a name="global-hub-and-spoke"></a><span data-ttu-id="21566-127">글로벌 허브 및 스포크</span><span class="sxs-lookup"><span data-stu-id="21566-127">Global hub and spoke</span></span>

<span data-ttu-id="21566-128">허브 및 스포크 아키텍처는 일반적으로 네트워크 간 대기 시간을 최소화하기 위해 동일한 Azure 지역에 배포된 가상 네트워크를 통해 구현됩니다.</span><span class="sxs-lookup"><span data-stu-id="21566-128">Hub and spoke architectures are commonly implemented with virtual networks deployed to the same Azure Region to minimize latency between networks.</span></span> <span data-ttu-id="21566-129">그러나 글로벌 영향력이 있는 대규모 조직은 가용성, 재해 복구 또는 규정 요구 사항을 고려해 여러 지역에 워크로드를 배포해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="21566-129">However, large organizations with global reach may need to deploy workloads across multiple regions for availability, disaster recovery, or regulatory requirements.</span></span> <span data-ttu-id="21566-130">Azure [글로벌 가상 네트워크 피어링](/azure/virtual-network/virtual-network-peering-overview)을 통해 허브 및 스포크 모델은 전 세계에 분산된 워크로드를 지원하려면 지역에서 중앙 집중식 관리 및 공유 서비스를 확장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="21566-130">Through the use of Azure [global virtual network peering](/azure/virtual-network/virtual-network-peering-overview), the hub and spoke model can extend centralized management and shared services across regions to support workloads distributed across the world.</span></span>

## <a name="learn-more"></a><span data-ttu-id="21566-131">자세한 정보</span><span class="sxs-lookup"><span data-stu-id="21566-131">Learn more</span></span>

<span data-ttu-id="21566-132">Azure에서 허브 및 스포크 네트워크를 구현하는 방법에 대한 예제는 Azure 참조 아키텍처 사이트의 다음 예제를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="21566-132">For examples of how to implement hub and spoke networks on Azure, see the following examples on the Azure Reference Architectures site:</span></span>

- [<span data-ttu-id="21566-133">Azure에서 허브-스포크 네트워크 토폴로지 구현</span><span class="sxs-lookup"><span data-stu-id="21566-133">Implement a hub-spoke network topology in Azure</span></span>](../../../reference-architectures/hybrid-networking/hub-spoke.md)
- [<span data-ttu-id="21566-134">Azure에서 공유 서비스를 사용하여 허브-스포크 네트워크 토폴로지 구현</span><span class="sxs-lookup"><span data-stu-id="21566-134">Implement a hub-spoke network topology with shared services in Azure</span></span>](../../../reference-architectures/hybrid-networking/shared-services.md)
