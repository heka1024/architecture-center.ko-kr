---
title: 'CAF: 소프트웨어 정의 네트워크 - 클라우드 DMZ'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 이 네트워크 아키텍처를 통해 온-프레미스 네트워크와 클라우드 기반 네트워크 간에 제한적으로 액세스할 수 있습니다.
author: rotycenh
ms.openlocfilehash: a192541dcfb0f3d713f4139a2ab0541d0c7202db
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58242814"
---
# <a name="software-defined-networks-cloud-dmz"></a><span data-ttu-id="86b2c-103">소프트웨어 정의 네트워크: Cloud DMZ</span><span class="sxs-lookup"><span data-stu-id="86b2c-103">Software Defined Networks: Cloud DMZ</span></span>

<span data-ttu-id="86b2c-104">클라우드 DMZ 네트워크 아키텍처를 사용하면 온-프레미스 네트워크와 클라우드 기반 네트워크 간에 제한적으로 액세스하여 VPN(가상 사설망)을 통해 네트워크를 연결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="86b2c-104">The Cloud DMZ network architecture allows limited access between your on-premises and cloud-based networks, using a virtual private network (VPN) to connect the networks.</span></span> <span data-ttu-id="86b2c-105">DMZ는 클라우드 기반 리소스에서 온-프레미스 네트워크에 대한 액세스를 보호하기 위해 클라우드에 배포됩니다.</span><span class="sxs-lookup"><span data-stu-id="86b2c-105">A DMZ is deployed in the cloud to secure access to the on-premises network from cloud-based resources.</span></span>

![하이브리드 네트워크 아키텍처 보안](../../../reference-architectures/dmz/images/dmz-private.png)

<span data-ttu-id="86b2c-107">이 아키텍처는 조직에서 클라우드 기반 워크로드를 온-프레미스 워크로드와 통합하려고 하지만 클라우드 보안 정책이 완전히 완성되지 않았거나 두 환경 간의 보안 전용 WAN 연결이 확보되지 않았을 수 있는 시나리오를 지원하도록 설계되었습니다.</span><span class="sxs-lookup"><span data-stu-id="86b2c-107">This architecture is designed to support scenarios where your organization wants to start integrating cloud-based workloads with on-premises workloads but may not have fully matured cloud security policies or acquired a secure dedicated WAN connection between the two environments.</span></span> <span data-ttu-id="86b2c-108">결과적으로 온-프레미스 서비스가 안전하게 보호되도록 클라우드 네트워크를 완충 영역처럼 취급해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="86b2c-108">As a result, cloud networks should be treated like a demilitarized zone to ensure on-premises services are secure.</span></span>

<span data-ttu-id="86b2c-109">DMZ는 방화벽 및 패킷 검사와 같은 보안 기능을 구현하는 NVA(네트워크 가상 어플라이언스)를 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="86b2c-109">The DMZ deploys network virtual appliances (NVAs) to implement security functionality such as firewalls and packet inspection.</span></span> <span data-ttu-id="86b2c-110">온-프레미스 애플리케이션과 클라우드 기반 애플리케이션 또는 서비스 간에 전달되는 트래픽은 감사할 수 있는 DMZ를 통과해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="86b2c-110">Traffic passing between on-premises and cloud-based applications or services must pass through the DMZ where it can be audited.</span></span> <span data-ttu-id="86b2c-111">DMZ 네트워크를 통해 허용되는 트래픽을 결정하는 VPN 연결 및 규칙은 IT 보안 팀에서 엄격하게 제어합니다.</span><span class="sxs-lookup"><span data-stu-id="86b2c-111">VPN connections and the rules determining what traffic is allowed through the DMZ network are strictly controlled by IT security teams.</span></span>

## <a name="cloud-dmz-assumptions"></a><span data-ttu-id="86b2c-112">클라우드 DMZ 가정</span><span class="sxs-lookup"><span data-stu-id="86b2c-112">Cloud DMZ assumptions</span></span>

<span data-ttu-id="86b2c-113">클라우드 DMZ는 다음을 전제로 하여 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="86b2c-113">Deploying a Cloud DMZ assumes the following:</span></span>

- <span data-ttu-id="86b2c-114">보안 팀에서 온-프레미스 및 클라우드 기반 보안 요구 사항과 정책을 완전하게 조정하지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="86b2c-114">Your security teams have not fully aligned on-premises and cloud-based security requirements and policies.</span></span>
- <span data-ttu-id="86b2c-115">클라우드 기반 워크로드가 온-프레미스 또는 타사 네트워크에서 호스팅되는 서비스에 제한적으로 액세스해야 하거나, 온-프레미스 환경의 사용자 또는 애플리케이션에서 클라우드 호스팅 리소스에 제한적으로 액세스해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="86b2c-115">Your cloud-based workloads require limited access to services hosted on your on-premises or third-party networks, or your users or applications in your on-premises environment need limited access to cloud-hosted resources.</span></span>
- <span data-ttu-id="86b2c-116">온-프레미스 네트워크와 클라우드 공급자 간의 VPN 연결 구현이 회사 정책, 규정 요구 사항 또는 기술 호환성 문제로 인해 방지되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="86b2c-116">Implementing a VPN connection between your on-premises networks and cloud provider is not prevented by corporate policy, regulatory requirements, or technical compatibility issues.</span></span>
- <span data-ttu-id="86b2c-117">구독 리소스 제한을 무시하기 위해 워크로드에 여러 구독이 필요하지 않거나, 여러 구독이 필요하지만 여러 구독에 분산된 리소스에서 사용하는 연결 또는 공유 서비스를 중앙에서 관리할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="86b2c-117">Your workloads either do not require multiple subscriptions to bypass subscription resource limits, or they involve multiple subscriptions but don't require central management of connectivity or shared services used by resources spread across multiple subscriptions.</span></span>

<span data-ttu-id="86b2c-118">클라우드 DMZ 가상 네트워킹 아키텍처의 구현을 살펴볼 때 클라우드 채택 팀에서 고려해야 하는 문제는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="86b2c-118">Your Cloud Adoption team should consider the following issues when looking at implementing a Cloud DMZ virtual networking architecture:</span></span>

- <span data-ttu-id="86b2c-119">온-프레미스 네트워크를 클라우드 네트워크에 연결하면 보안 요구 사항이 복잡해집니다.</span><span class="sxs-lookup"><span data-stu-id="86b2c-119">Connecting on-premises networks with cloud networks increases the complexity of your security requirements.</span></span> <span data-ttu-id="86b2c-120">클라우드 네트워크와 온-프레미스 환경 간의 연결이 보호되는 경우에도 클라우드 리소스를 안전하게 보호해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="86b2c-120">Even though the connection between cloud networks and the on-premises environment are secured, you still need to ensure cloud resources are secured.</span></span>
- <span data-ttu-id="86b2c-121">클라우드 DMZ 아키텍처는 일반적으로 디딤돌로 사용되는 한편 연결의 보안이 강화되고 보안 정책이 온-프레미스 네트워크와 클라우드 네트워크 간에 조정되어 전면적인 하이브리드 네트워킹 아키텍처를 광범위하게 채택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="86b2c-121">The Cloud DMZ architecture is commonly used as a stepping stone while connectivity is further secured and security policy aligned between on-premises and cloud networks, allowing a broader adoption of a full-scale hybrid networking architecture.</span></span>

## <a name="learn-more"></a><span data-ttu-id="86b2c-122">자세한 정보</span><span class="sxs-lookup"><span data-stu-id="86b2c-122">Learn more</span></span>

<span data-ttu-id="86b2c-123">Azure 플랫폼에서 클라우드 DMZ를 구현하는 방법에 대한 자세한 내용은 다음을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="86b2c-123">See the following for more information about the implementing a Cloud DMZ in the Azure platform.</span></span>

- <span data-ttu-id="86b2c-124">[Azure와 온-프레미스 데이터 센터 간의 DMZ를 구현합니다](../../../reference-architectures/dmz/secure-vnet-hybrid.md).</span><span class="sxs-lookup"><span data-stu-id="86b2c-124">[Implement a DMZ between Azure and your on-premises datacenter](../../../reference-architectures/dmz/secure-vnet-hybrid.md).</span></span> <span data-ttu-id="86b2c-125">이 문서에서는 Azure에서 보안 하이브리드 네트워크 아키텍처를 구현하는 방법에 대해 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="86b2c-125">This article discusses how to implement a secure hybrid network architecture in Azure.</span></span>
