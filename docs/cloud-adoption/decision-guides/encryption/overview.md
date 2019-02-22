---
title: 'CAF: 암호화'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Azure 마이그레이션의 핵심 서비스인 암호화에 대해 알아봅니다.
author: rotycenh
ms.openlocfilehash: 660206d57ded9a93d73c57ba9cb8058020d87525
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901455"
---
# <a name="encryption-decision-guide"></a><span data-ttu-id="58581-103">암호화 결정 가이드</span><span class="sxs-lookup"><span data-stu-id="58581-103">Encryption decision guide</span></span>

<span data-ttu-id="58581-104">데이터 암호화는 무단 액세스로부터 데이터를 보호합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-104">Encrypting data protects it against unauthorized access.</span></span> <span data-ttu-id="58581-105">올바르게 구현된 암호화 정책은 클라우드 기반 워크로드에 대해 추가 계층 보안 및 조직과 네트워크 내외부의 공격자와 기타 무단 사용자에 대한 보호를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-105">Properly implemented encryption policy provides additional layers of security for your cloud-based workloads and guards against attackers and other unauthorized users from both inside and outside your organization and networks.</span></span>

<span data-ttu-id="58581-106">리소스 암호화가 일반적으로 바람직하지만 암호화는 대기 시간 및 전체 리소스 사용량을 증가시킬 수 있는 비용이 수반됩니다.</span><span class="sxs-lookup"><span data-stu-id="58581-106">While encrypting resources is generally desirable, encryption does have costs that can increase latency and overall resource usage.</span></span> <span data-ttu-id="58581-107">까다로운 워크로드의 경우 암호화와 성능 사이의 적절한 균형은 아주 중요합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-107">For demanding workloads, striking the correct balance between encryption and performance is essential.</span></span>

![암호화 옵션 그림(복잡성 낮은 항목에서 높은 항목 순, 하단에 각 항목으로 이동하는 링크 포함)](../../_images/discovery-guides/discovery-guide-encryption.png)

<span data-ttu-id="58581-109">이동: [키 관리](#key-management) | [데이터 암호화](#data-encryption) | [자세한 정보](#learn-more)</span><span class="sxs-lookup"><span data-stu-id="58581-109">Jump to: [Key management](#key-management) | [Data encryption](#data-encryption) | [Learn more](#learn-more)</span></span>

<span data-ttu-id="58581-110">클라우드 암호화 전략을 결정하는 경우의 변곡점은 회사 정책 및 규정 준수 의무에 중점을 둡니다.</span><span class="sxs-lookup"><span data-stu-id="58581-110">The inflection point when determining a cloud encryption strategy focuses on corporate policy and compliance mandates.</span></span>

<span data-ttu-id="58581-111">다양한 비용 및 복잡성을 사용하여 클라우드 환경에서 암호화를 구현하는 방법은 여러 가지입니다.</span><span class="sxs-lookup"><span data-stu-id="58581-111">There are multiple ways to implement encryption in a cloud environment, with varying cost and complexity.</span></span> <span data-ttu-id="58581-112">회사 정책 및 타사 규정 준수 암호화는 암호화 전략을 계획할 때 가장 중요한 동인입니다.</span><span class="sxs-lookup"><span data-stu-id="58581-112">Corporate policy and third-party compliance are the biggest drivers when planning an encryption strategy.</span></span> <span data-ttu-id="58581-113">대부분의 클라우드 기반 솔루션은 미사용이든 전송 중이든 관계 없이 데이터를 암호화하기 위한 표준 메커니즘을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-113">Most cloud-based solutions provide standard mechanisms for encrypting data, whether at rest or in transit.</span></span> <span data-ttu-id="58581-114">그러나 표준화된 비밀 및 키 관리, 사용 중인 암호화 또는 데이터 특정 암호화와 같이 엄격한 컨트롤을 요구하는 정책 및 규정 준수 요구 사항의 경우 복잡한 솔루션을 구현해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="58581-114">However, for policies and compliance requirements that demand tighter controls, such as standardized secrets and key management, encryption in-use, or data specific encryption, you will likely need to implement a complex solution.</span></span>

## <a name="key-management"></a><span data-ttu-id="58581-115">키 관리</span><span class="sxs-lookup"><span data-stu-id="58581-115">Key management</span></span>

<span data-ttu-id="58581-116">최신 키 관리 시스템은 HSM(하드웨어 보안 모듈)을 사용하여 키를 저장하는 것을 지원해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-116">Modern key management systems should offer support for storing keys using hardware security modules (HSMs) for increased protection.</span></span> <span data-ttu-id="58581-117">따라서 키 관리 시스템은 암호화 키, 중요한 암호, 연결 문자열 및 기타 IT 기밀 정보를 만들고 저장하는 조직의 능력에서 중요한 위치를 차지합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-117">Thus, a key management system is critical to your organization's ability to create and store cryptographic keys, important passwords, connection strings, and other IT confidential information.</span></span>

<span data-ttu-id="58581-118">클라우드 마이그레이션을 계획하는 경우 다음 표에서는 안전하고 관리 가능한 클라우드 배포를 만드는 데 있어 중요한 암호화 키, 인증서 및 비밀을 저장 및 관리하는 방법을 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-118">When planning a cloud migration, the following table describes how you can store and manage encryption keys, certificates, and secrets, which are critical for creating secure and manageable cloud deployments:</span></span>

| <span data-ttu-id="58581-119">질문</span><span class="sxs-lookup"><span data-stu-id="58581-119">Question</span></span> | <span data-ttu-id="58581-120">클라우드 네이티브</span><span class="sxs-lookup"><span data-stu-id="58581-120">Cloud Native</span></span> | <span data-ttu-id="58581-121">하이브리드</span><span class="sxs-lookup"><span data-stu-id="58581-121">Hybrid</span></span> | <span data-ttu-id="58581-122">온-프레미스</span><span class="sxs-lookup"><span data-stu-id="58581-122">On-premises</span></span> |
|---------------------------------------------------------------------------------------------------------------------------------------|--------------|--------|-------------|
| <span data-ttu-id="58581-123">조직에 중앙 집중식 키 및 비밀 관리가 부족하나요?</span><span class="sxs-lookup"><span data-stu-id="58581-123">Does your organization lack centralized key and secret management?</span></span>                                                                    | <span data-ttu-id="58581-124">예</span><span class="sxs-lookup"><span data-stu-id="58581-124">Yes</span></span>          | <span data-ttu-id="58581-125">아니요</span><span class="sxs-lookup"><span data-stu-id="58581-125">No</span></span>     | <span data-ttu-id="58581-126">아니요</span><span class="sxs-lookup"><span data-stu-id="58581-126">No</span></span>          |
| <span data-ttu-id="58581-127">클라우드에서 이러한 키를 사용하는 동안 디바이스의 키와 비밀 생성을 온-프레미스 하드웨어로 제한해야 하나요?</span><span class="sxs-lookup"><span data-stu-id="58581-127">Will you need to limit the creation of keys and secrets to devices to your on-premises hardware, while using these keys in the cloud?</span></span> | <span data-ttu-id="58581-128">아니요</span><span class="sxs-lookup"><span data-stu-id="58581-128">No</span></span>           | <span data-ttu-id="58581-129">예</span><span class="sxs-lookup"><span data-stu-id="58581-129">Yes</span></span>    | <span data-ttu-id="58581-130">아니요</span><span class="sxs-lookup"><span data-stu-id="58581-130">No</span></span>          |
| <span data-ttu-id="58581-131">조직에는 키 및 비밀을 오프사이트에 저장하는 것을 방지하는 규칙 또는 정책이 정착됐나요?</span><span class="sxs-lookup"><span data-stu-id="58581-131">Does your organization have rules or policies in place that would prevent keys and secrets from being stored offsite?</span></span>                | <span data-ttu-id="58581-132">아니요</span><span class="sxs-lookup"><span data-stu-id="58581-132">No</span></span>           | <span data-ttu-id="58581-133">아니요</span><span class="sxs-lookup"><span data-stu-id="58581-133">No</span></span>     | <span data-ttu-id="58581-134">예</span><span class="sxs-lookup"><span data-stu-id="58581-134">Yes</span></span>         |

### <a name="cloud-native"></a><span data-ttu-id="58581-135">네이티브 클라우드</span><span class="sxs-lookup"><span data-stu-id="58581-135">Cloud native</span></span>

<span data-ttu-id="58581-136">클라우드 네이티브 키 관리를 사용하여 클라우드 기반 자격 증명 모음에서 모든 키와 비밀을 생성, 관리 및 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-136">With cloud native key management, all keys and secrets are generated, managed, and stored in a cloud-based vault.</span></span> <span data-ttu-id="58581-137">이 방법은 키 관리와 관련된 여러 IT 작업을 간소화합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-137">This approach simplifies many IT tasks related to key management.</span></span>

<span data-ttu-id="58581-138">클라우드 네이티브 키 관리 가정: 클라우드 네이티브 키 관리 시스템을 사용하여 다음을 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-138">Cloud native key management assumptions: Using a cloud native key management system assumes the following:</span></span>

- <span data-ttu-id="58581-139">조직의 비밀 및 키를 만들고, 관리하고 호스트하는 클라우드 키 관리 솔루션을 신뢰합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-139">You trust the cloud key management solution with creating, managing, and hosting your organization's secrets and keys.</span></span>
- <span data-ttu-id="58581-140">암호화 서비스 또는 비밀에 액세스를 사용하여 클라우드 키 관리 시스템에 액세스하는 모든 온-프레미스 애플리케이션 및 서비스를 사용하도록 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-140">You enable all on-premises applications and services that rely on accessing encryption services or secrets to access the cloud key management system.</span></span>

### <a name="hybrid-bring-your-own-key"></a><span data-ttu-id="58581-141">하이브리드(Bring Your Own Key)</span><span class="sxs-lookup"><span data-stu-id="58581-141">Hybrid (bring your own key)</span></span>

<span data-ttu-id="58581-142">Bring Your Own Key 방식을 사용하여 온-프레미스 환경 내의 전용 HSM 하드웨어에서 키를 생성한 다음, 클라우드 리소스를 사용하기 위해 보안 클라우드 키 관리 시스템에 키를 전송합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-142">With a bring-your-own-key approach, you generate keys on dedicated HSM hardware within your on-premises environment, then transfer the keys to a secure cloud key management system for use with cloud resources.</span></span>

<span data-ttu-id="58581-143">하이브리드 키 관리 가정: 하이브리드 키 관리 시스템을 사용하여 다음을 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-143">Hybrid key management assumptions: Using a hybrid key management system assumes the following:</span></span>

- <span data-ttu-id="58581-144">키 및 비밀을 호스트하고 사용하기 위해 클라우드 플랫폼의 기본 보안 및 액세스 제어 인프라를 신뢰합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-144">You trust the underlying security and access control infrastructure of the cloud platform for hosting and using your keys and secrets.</span></span>
- <span data-ttu-id="58581-145">온-프레미스에서 조직의 비밀 및 키의 생성과 관리를 유지하려면 규정 정책 또는 조직 정책이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-145">You are required by regulatory or organizational policy to keep the creation and management of your organization's secrets and keys on-premises.</span></span>

### <a name="on-premises-hold-your-own-key"></a><span data-ttu-id="58581-146">온-프레미스(사용자 고유의 키 보유)</span><span class="sxs-lookup"><span data-stu-id="58581-146">On-premises (hold your own key)</span></span>

<span data-ttu-id="58581-147">특정 시나리오에서 공용 클라우드 서비스가 제공하는 키 관리 시스템에 키를 저장할 수 없는 규정, 정책 또는 기술적 이유가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="58581-147">In certain scenarios, there may be regulatory, policy, or technical reasons why you can't store keys on a key management system provided by a public cloud service.</span></span> <span data-ttu-id="58581-148">이러한 경우, 온-프레미스 하드웨어를 사용하여 키를 유지 관리하고, 클라우드 기반 리소스에서 암호화의 목적으로 이러한 키에 액세스하도록 허용하는 메커니즘을 프로비전해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-148">In these cases, you must maintain keys using on-premises hardware, and provision a mechanism to allow cloud-based resource to access these keys for encryption purposes.</span></span> <span data-ttu-id="58581-149">사용자 고유의 키 보유 방법은 모든 클라우드 서비스와 호환되지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="58581-149">Note that a hold your own key approach may not be compatible with all cloud services.</span></span>

<span data-ttu-id="58581-150">온-프레미스 키 관리 가정: 온-프레미스 키 관리 시스템을 사용하여 다음을 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-150">On-premises key management assumptions: Using an on-premises key management system assumes the following:</span></span>

- <span data-ttu-id="58581-151">온-프레미스에서 조직의 비밀 및 키의 생성, 관리 및 호스팅을 유지하려면 규정 정책 또는 조직 정책이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-151">You are required by regulatory or organizational policy to keep the creation, management, and hosting of your organization's secrets and keys on-premises.</span></span>
- <span data-ttu-id="58581-152">암호화 서비스 또는 비밀에 액세스를 사용하는 모든 클라우드 키 애플리케이션 또는 서비스는 온-프레미스 키 관리 시스템에 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="58581-152">Any cloud-based applications or services that rely on accessing encryption services or secrets can access the on-premises key management system.</span></span>

## <a name="data-encryption"></a><span data-ttu-id="58581-153">데이터 암호화.</span><span class="sxs-lookup"><span data-stu-id="58581-153">Data encryption</span></span>

<span data-ttu-id="58581-154">암호화 정책을 계획할 때 고려해야 할 다양한 암호화 요구가 있는 여러 다양한 상태의 데이터가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="58581-154">There are several different states of data with different encryption needs to consider when planning your encryption policy:</span></span>

| <span data-ttu-id="58581-155">데이터 상태</span><span class="sxs-lookup"><span data-stu-id="58581-155">Data state</span></span> | <span data-ttu-id="58581-156">Data</span><span class="sxs-lookup"><span data-stu-id="58581-156">Data</span></span> |
|-----|-----|
| <span data-ttu-id="58581-157">전송 중 데이터</span><span class="sxs-lookup"><span data-stu-id="58581-157">Data in transit</span></span> | <span data-ttu-id="58581-158">내부 네트워크 트래픽, 인터넷 연결, 데이터 센터 또는 가상 네트워크 간의 연결</span><span class="sxs-lookup"><span data-stu-id="58581-158">Internal network traffic, internet connections, connections between datacenters or virtual networks</span></span> |
| <span data-ttu-id="58581-159">미사용 데이터</span><span class="sxs-lookup"><span data-stu-id="58581-159">Data at rest</span></span>    | <span data-ttu-id="58581-160">데이터베이스, 파일, 가상 드라이브, PaaS 스토리지</span><span class="sxs-lookup"><span data-stu-id="58581-160">Databases, files, virtual drives, PaaS storage</span></span> |
| <span data-ttu-id="58581-161">사용 중인 데이터</span><span class="sxs-lookup"><span data-stu-id="58581-161">Data in use</span></span>     | <span data-ttu-id="58581-162">RAM 또는 CPU 캐시에 로드된 데이터</span><span class="sxs-lookup"><span data-stu-id="58581-162">Data loaded in RAM or in CPU caches</span></span> |

### <a name="data-in-transit"></a><span data-ttu-id="58581-163">전송 중 데이터</span><span class="sxs-lookup"><span data-stu-id="58581-163">Data in transit</span></span>

<span data-ttu-id="58581-164">전송 중인 데이터란 내부의 리소스 간에, 데이터 센터 또는 외부 네트워크 간에 또는 인터넷을 통해 이동하는 데이터를 말합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-164">Data in transit is data moving between resources on the internal, between datacenters or external networks, or over the internet.</span></span>

<span data-ttu-id="58581-165">전송 중인 데이터 암호화는 일반적으로 트래픽에 SSL/TLS 프로토콜을 요구하여 수행됩니다.</span><span class="sxs-lookup"><span data-stu-id="58581-165">Encrypting data in transit is usually done by requiring SSL/TLS protocols for traffic.</span></span> <span data-ttu-id="58581-166">클라우드에서 외부 네트워크 또는 공용 인터넷으로 호스팅되는 리소스 간 전송되는 트래픽은 항상 암호화되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-166">Traffic transiting between your cloud-hosted resources to external network or the public internet should always be encrypted.</span></span> <span data-ttu-id="58581-167">일반적으로 PaaS 리소스는 기본적으로 트래픽에 SSL/TLS 암호화를 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-167">PaaS resources generally also enforce SSL/TLS encryption to traffic by default.</span></span> <span data-ttu-id="58581-168">가상 네트워크 내에서 호스팅되는 IaaS 리소스 간의 트래픽에 암호화를 적용할지 여부는 일반적으로 클라우드 도입 팀 및 워크로드 소유자가 결정하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="58581-168">Whether you enforce encryption for traffic between IaaS resources hosted inside your virtual networks is a decision for your Cloud Adoption Team and workload owner and is generally recommended.</span></span>

<span data-ttu-id="58581-169">**전송 중 데이터 암호화 가정**.</span><span class="sxs-lookup"><span data-stu-id="58581-169">**Encrypting data in transit assumptions**.</span></span> <span data-ttu-id="58581-170">전송 중 데이터에 적절한 암호화 정책의 구현은 다음을 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-170">Implementing proper encryption policy for data in transit assumes the following:</span></span>

- <span data-ttu-id="58581-171">클라우드 환경에서 공개적으로 액세스할 수 있는 모든 엔드포인트는 SSL/TLS 프로토콜을 사용하여 공용 인터넷과 통신합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-171">All publicly accessible endpoints in your cloud environment will communicate with the public internet using SSL/TLS protocols.</span></span>
- <span data-ttu-id="58581-172">클라우드 네트워크와 온-프레미스 또는 공용 인터넷을 통해 기타 외부 네트워크를 연결하는 경우 암호화된 VPN 프로토콜을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-172">When connecting cloud networks with on-premises or other external network over the public internet, use encrypted VPN protocols.</span></span>
- <span data-ttu-id="58581-173">ExpressRoute와 같은 전용 WAN 연결을 사용하여 클라우드 네트워크와 온-프레미스 또는 기타 외부 네트워크를 연결하는 경우 클라우드 네트워크에 배포된 해당 가상 VPN 또는 암호화 어플라이언스와 쌍을 이룬 VPN 또는 기타 암호화 어플라이언스 온-프레미스를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-173">When connecting cloud networks with on-premises or other external network using a dedicated WAN connection such as ExpressRoute, you will use a VPN or other encryption appliance on-premises paired with a corresponding virtual VPN or encryption appliance deployed to your cloud network.</span></span>
- <span data-ttu-id="58581-174">트래픽 로그 또는 IT 직원에게 표시되는 기타 진단 보고서에 포함되지 않아야 하는 중요한 데이터가 있는 경우 가상 네트워크의 리소스 간 모든 트래픽을 암호화합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-174">If you have sensitive data that shouldn't be included in traffic logs or other diagnostics reports visible to IT staff, you will encrypt all traffic between resources in your virtual network.</span></span>

### <a name="data-at-rest"></a><span data-ttu-id="58581-175">미사용 데이터</span><span class="sxs-lookup"><span data-stu-id="58581-175">Data at rest</span></span>

<span data-ttu-id="58581-176">미사용 데이터는 파일, 데이터베이스, 가상 머신 드라이브, PaaS 스토리지 계정 또는 비슷한 자산을 포함하여 적극적으로 이동하거나 처리되지 않는 모든 데이터를 말합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-176">Data at rest represents any data not being actively moved or processed, including files, databases, virtual machine drives, PaaS storage accounts, or similar assets.</span></span> <span data-ttu-id="58581-177">저장된 데이터를 암호화하면 외부 네트워크 침입, 악의적인 내부 사용자 또는 실수로 인한 릴리스에서 무단 액세스로부터 가상 디바이스 또는 파일을 보호합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-177">Encrypting stored data protects virtual devices or files against unauthorized access either from external network penetration, rogue internal users, or accidental releases.</span></span>

<span data-ttu-id="58581-178">일반적으로 PaaS 스토리지 및 데이터베이스 리소스는 기본적으로 암호화를 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-178">PaaS storage and database resources generally enforce encryption by default.</span></span> <span data-ttu-id="58581-179">키 관리 시스템에 저장된 암호화 키를 사용하여 가상 디스크 암호화를 통해 IaaS 가상 리소스를 보호할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="58581-179">IaaS virtual resources can be secured through virtual disk encryption using cryptographic keys stored in your key management system.</span></span>

<span data-ttu-id="58581-180">미사용 데이터의 암호화는 또한 열 수준 및 행 수준 암호화와 같은 고급 데이터베이스 암호화 기술을 포함하고, 정확하게 보호되는 데이터에 대한 훨씬 강화된 제어 기능을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-180">Encryption for data at rest also encompasses more advanced database encryption techniques, such as column-level and row level encryption, which provides much more control over exactly what data is being secured.</span></span>

<span data-ttu-id="58581-181">전체 정책 및 규정 준수 요구 사항, 저장되는 데이터의 민감도 및 워크로드의 성능 요구 사항은 암호화가 필요한 자산을 결정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-181">Your overall policy and compliance requirements, the sensitivity of the data being stored, and the performance requirements of your workloads should determine which assets require encryption.</span></span>

<span data-ttu-id="58581-182">**미사용 데이터 암호화 가정**.</span><span class="sxs-lookup"><span data-stu-id="58581-182">**Encrypting Data at Rest Assumptions**.</span></span> <span data-ttu-id="58581-183">미사용 데이터 암호화는 다음을 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-183">Encrypting data at rest assumes the following:</span></span>

- <span data-ttu-id="58581-184">공개 사용으로 계획되지 않은 데이터를 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-184">You are storing data that is not meant for public consumption.</span></span>
- <span data-ttu-id="58581-185">워크로드는 디스크 암호화의 추가된 대기 시간 비용을 수락할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="58581-185">Your workloads can accept the added latency cost of disk encryption.</span></span>

### <a name="data-in-use"></a><span data-ttu-id="58581-186">사용 중인 데이터</span><span class="sxs-lookup"><span data-stu-id="58581-186">Data in use</span></span>

<span data-ttu-id="58581-187">사용 중인 데이터의 암호화에는 RAM 또는 CPU 캐시와 같은 비영구 스토리지에서 데이터 보안이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="58581-187">Encryption for data in use involves securing data in nonpersistent storage, such as RAM or CPU caches.</span></span> <span data-ttu-id="58581-188">Intel의 SGX(Secure Guard Extensions)와 같은 Enclave 기술 및 전체 메모리 암호화와 같은 기술을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-188">Use of technologies such as full memory encryption, enclave technologies, such as Intel's Secure Guard Extensions (SGX).</span></span> <span data-ttu-id="58581-189">또한 안전하고 신뢰할 수 있는 실행 환경을 만드는 데 사용할 수 있는 준동형 암호화와 같은 암호화 기술도 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="58581-189">This also includes cryptographic techniques, such as homomorphic encryption that can be used to create secure, trusted execution environments.</span></span>

<span data-ttu-id="58581-190">**사용 중인 데이터 암호화 가정**.</span><span class="sxs-lookup"><span data-stu-id="58581-190">**Encrypting data in use assumptions**.</span></span> <span data-ttu-id="58581-191">사용 중인 데이터 암호화는 다음을 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-191">Encrypting data in use assumes the following:</span></span>

- <span data-ttu-id="58581-192">RAM 및 CPU 수준에서 언제든 기본 클라우드 플랫폼과 별도로 데이터 소유권을 유지 관리할 필요가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="58581-192">You are required to maintain data ownership separate from the underlying cloud platform at all times, even at the RAM and CPU level.</span></span>

## <a name="learn-more"></a><span data-ttu-id="58581-193">자세한 정보</span><span class="sxs-lookup"><span data-stu-id="58581-193">Learn more</span></span>

<span data-ttu-id="58581-194">Azure 플랫폼에서 암호화 및 키 관리에 대한 자세한 내용은 다음을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="58581-194">See the following for more information about encryption and key management in the Azure platform.</span></span>

- <span data-ttu-id="58581-195">[Azure 암호화 개요](/azure/security/security-azure-encryption-overview).</span><span class="sxs-lookup"><span data-stu-id="58581-195">[Azure encryption overview](/azure/security/security-azure-encryption-overview).</span></span> <span data-ttu-id="58581-196">Azure에서 미사용 데이터와 전송 중인 데이터 모두를 보안 조치하기 위해 암호화를 사용하는 방법에 대한 자세한 설명입니다.</span><span class="sxs-lookup"><span data-stu-id="58581-196">A detailed description of how Azure uses encryption to secure both data at rest and data in transit.</span></span>
- <span data-ttu-id="58581-197">[Azure Key Vault](/azure/key-vault/key-vault-overview)</span><span class="sxs-lookup"><span data-stu-id="58581-197">[Azure Key Vault](/azure/key-vault/key-vault-overview).</span></span> <span data-ttu-id="58581-198">Key Vault는 Azure 내에 암호화 키, 비밀 및 인증서를 저장하고 관리하기 위한 기본 키 관리 시스템입니다.</span><span class="sxs-lookup"><span data-stu-id="58581-198">Key Vault is the primary key management system for storing and managing cryptographic keys, secrets, and certificates within Azure.</span></span>
- <span data-ttu-id="58581-199">[Azure의 기밀 컴퓨팅](/solutions/confidential-compute).</span><span class="sxs-lookup"><span data-stu-id="58581-199">[Confidential computing in Azure](/solutions/confidential-compute).</span></span> <span data-ttu-id="58581-200">Azure의 기밀 컴퓨팅 이니셔티브는 신뢰할 수 있는 실행 환경을 만드는 도구 및 기술 또는 사용 중인 데이터를 보호하는 기타 암호화 메커니즘을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="58581-200">Azure's confidential computing initiative provides tools and technology to create trusted execution environments or other encryption mechanisms to secure data in use.</span></span>

## <a name="next-steps"></a><span data-ttu-id="58581-201">다음 단계</span><span class="sxs-lookup"><span data-stu-id="58581-201">Next steps</span></span>

<span data-ttu-id="58581-202">소프트웨어 정의 네트워크에서 클라우드 배포를 위해 가상화된 네트워킹 기능을 제공하는 방법을 알아봅니다.</span><span class="sxs-lookup"><span data-stu-id="58581-202">Learn how Software Defined Networks provide virtualized networking capabilities for cloud deployments.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="58581-203">어떤 소프트웨어 정의 네트워크 패턴이 내 배포에 가장 적합한가요?</span><span class="sxs-lookup"><span data-stu-id="58581-203">Which Software Defined Network pattern is best for my deployment?</span></span>](../software-defined-network/overview.md)
