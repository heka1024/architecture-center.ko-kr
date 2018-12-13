---
title: Azure의 은행 간에 분산 트러스트
description: 중앙 집중식 데이터베이스로 재분류하지 않고 통신 및 정보 공유에 대해 신뢰할 수 있는 환경을 설정합니다.
author: vitoc
ms.date: 09/09/2018
ms.custom: csa-team
ms.openlocfilehash: 91c41f7bd6bd6f4eb8cd00859f7ce9065f8a86be
ms.sourcegitcommit: a0e8d11543751d681953717f6e78173e597ae207
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/06/2018
ms.locfileid: "53004731"
---
# <a name="decentralized-trust-between-banks-on-azure"></a><span data-ttu-id="a34cd-103">Azure의 은행 간에 분산 트러스트</span><span class="sxs-lookup"><span data-stu-id="a34cd-103">Decentralized trust between banks on Azure</span></span>

<span data-ttu-id="a34cd-104">이 예제 시나리오는 중앙 집중식 데이터베이스에 의존하지 않고 정보 공유를 위한 신뢰할 수 있는 환경을 구축하려는 은행 또는 기타 기관에 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-104">This example scenario is useful for banks or any other institutions that want to establish a trusted environment for information sharing without resorting to a centralized database.</span></span> <span data-ttu-id="a34cd-105">이 예제의 목적을 달성하기 위해, 은행 간 크레딧 점수 정보 유지의 관점에서 시나리오를 살펴볼 것이지만, 어느 한 당사자가 사용하는 중앙 시스템에 의존하지 않고 조직 컨소시엄에서 유효성이 검증된 정보를 서로 공유하려는 모든 시나리오에 이 아키텍처를 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-105">For the purpose of this example, we will describe the scenario in the context of maintaining credit score information between banks, but the architecture can be applied to any scenario where a consortium of organizations want to share validated information with one another without resorting to the use of a central system ran by one single party.</span></span>

<span data-ttu-id="a34cd-106">전통적으로 금융 시스템 내의 은행은 개인의 신용 점수 및 기록에 대한 정보를 얻기 위해 신용 조사 기관 같은 중앙 집중식 소스에 의존합니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-106">Traditionally, banks within a financial system rely on centralized sources such as credit bureaus for information on an individual's credit score and history.</span></span> <span data-ttu-id="a34cd-107">중앙 집중식 방법은 운영 위험이 집중되고 때로는 불필요한 제3자가 개입됩니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-107">A centralized approach presents a concentration of operational risk and sometimes an unnecessary third party.</span></span>

<span data-ttu-id="a34cd-108">DLT(분산된 원장 기술)를 사용하면 은행 컨소시엄은 보다 효율적이고, 공격에 대한 내성이 강하고, 혁신적인 구조체를 구현하여 개인 정보 보호, 속도 및 비용과 관련된 전통적인 과제를 해결하는 새로운 플랫폼 역할을 할 수 있는 분산 시스템을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-108">With DLTs (distributed ledger technology), a consortium of banks can establish a decentralized system that can be more efficient, less susceptible to attack, and serve as a new platform where innovative structures can be implemented to solve traditional challenges with privacy, speed, and cost.</span></span>

<span data-ttu-id="a34cd-109">이 예제에서는 가상 머신 확장 집합, Virtual Network, Key Vault, Storage, Load Balancer, Monitor 등의 Azure 서비스를 신속하게 프로비전하여 멤버 은행이 고유의 노드를 설정할 수 있는 효율적인 비공개 Ethereum PoA 블록체인을 배포하는 방법을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-109">This example will show you how Azure services such as virtual machine scale sets, Virtual Network, Key Vault, Storage, Load Balancer, and Monitor can be quickly provisioned for the deployment of an efficient private Ethereum PoA blockchain where member banks can establish their own nodes.</span></span>

## <a name="relevant-use-cases"></a><span data-ttu-id="a34cd-110">관련 사용 사례</span><span class="sxs-lookup"><span data-stu-id="a34cd-110">Relevant use cases</span></span>

<span data-ttu-id="a34cd-111">관련된 다른 사용 사례는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-111">Other relevant use cases include:</span></span>

* <span data-ttu-id="a34cd-112">다국적 회사의 여러 사업부 간에 할당된 예산 이동</span><span class="sxs-lookup"><span data-stu-id="a34cd-112">Movement of allocated budgets between different business units of a multinational corporation</span></span>
* <span data-ttu-id="a34cd-113">국가 간 결제</span><span class="sxs-lookup"><span data-stu-id="a34cd-113">Cross-border payments</span></span>
* <span data-ttu-id="a34cd-114">무역 금융 시나리오</span><span class="sxs-lookup"><span data-stu-id="a34cd-114">Trade finance scenarios</span></span>
* <span data-ttu-id="a34cd-115">여러 회사가 관련된 로열티 시스템</span><span class="sxs-lookup"><span data-stu-id="a34cd-115">Loyalty systems involving different companies</span></span>
* <span data-ttu-id="a34cd-116">공급망 에코시스템</span><span class="sxs-lookup"><span data-stu-id="a34cd-116">Supply chain ecosystems</span></span>

## <a name="architecture"></a><span data-ttu-id="a34cd-117">아키텍처</span><span class="sxs-lookup"><span data-stu-id="a34cd-117">Architecture</span></span>

![분산된 은행 트러스트 아키텍처 다이어그램](./media/architecture-decentralized-trust.png)

<span data-ttu-id="a34cd-119">이 시나리오에서는 둘 이상의 멤버로 구성된 컨소시엄 내부에 확장 가능하고 안전하고 모니터링되는 비공개 엔터프라이즈 블록체인 네트워크를 만드는 데 필요한 백 엔드 구성 요소를 다룹니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-119">This scenario covers the back-end components that are necessary to create a scalable, secure, and monitored private, enterprise blockchain network within a consortium of two or more members.</span></span> <span data-ttu-id="a34cd-120">이러한 구성 요소를 프로비전하는 방법(즉, 서로 다른 구독 및 리소스 그룹에 배치) 및 연결 요구 사항(즉, VPN 또는 ExpressRoute)에 대한 구체적인 내용은 조직의 정책 요구 사항에 따라 여러분이 고민해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-120">Details of how these components are provisioned (that is, within different subscriptions and resource groups) as well as the connectivity requirements (that is, VPN or ExpressRoute) are left for your consideration based on your organization's policy requirements.</span></span> <span data-ttu-id="a34cd-121">다음은 데이터 흐름입니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-121">Here's how data flows:</span></span>

1. <span data-ttu-id="a34cd-122">A 은행은 JSON-RPC를 통해 블록체인 네트워크로 트랜잭션을 전송하여 개인의 신용 레코드를 생성/업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-122">Bank A creates/updates an individual's credit record by sending a transaction to the blockchain network via JSON-RPC.</span></span>
2. <span data-ttu-id="a34cd-123">데이터는 A 은행의 비공개 응용 프로그램 서버에서 Azure 부하 분산 장치를 거쳐 가상 머신 확장 집합의 유효성 검사 노드 VM으로 흐릅니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-123">Data flows from Bank A's private application server to the Azure load balancer and subsequently to a validating node VM on the virtual machine scale set.</span></span>
3. <span data-ttu-id="a34cd-124">Ethereum PoA 네트워크는 미리 정해진 시간(이 시나리오에서는 2초)에 블록을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-124">The Ethereum PoA network creates a block at a preset time (2 seconds for this scenario).</span></span>
4. <span data-ttu-id="a34cd-125">트랜잭션은 생성된 블록에 번들되고 블록체인 네트워크에서 유효성이 검사됩니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-125">The transaction is bundled into the created block and validated across the blockchain network.</span></span>
5. <span data-ttu-id="a34cd-126">B 은행은 마찬가지로 JSON-RPC를 통해 자체 노드와 통신하여 A 은행에서 만든 신용 레코드를 읽을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-126">Bank B can read the credit record created by bank A by communicating with its own node similarly via JSON-RPC.</span></span>

### <a name="components"></a><span data-ttu-id="a34cd-127">구성 요소</span><span class="sxs-lookup"><span data-stu-id="a34cd-127">Components</span></span>

* <span data-ttu-id="a34cd-128">가상 머신 확장 집합 내의 가상 머신은 블록체인에 대한 유효성 검사기 프로세스를 호스트하는 주문형 계산 시설을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-128">Virtual machines within virtual machine scale sets provides the on-demand compute facility to host the validator processes for the blockchain</span></span>
* <span data-ttu-id="a34cd-129">Key Vault는 각 유효성 검사기의 개인 키에 대한 보안 저장소 시설로 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-129">Key Vault is used as the secure storage facility for the private keys of each validator</span></span>
* <span data-ttu-id="a34cd-130">Load Balancer는 RPC, 피어링 및 거버넌스 DApp 요청을 분산합니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-130">Load Balancer spreads the RPC, peering, and Governance DApp requests</span></span>
* <span data-ttu-id="a34cd-131">지속적인 네트워크 정보를 호스팅하고 임대를 조정하는 Storage</span><span class="sxs-lookup"><span data-stu-id="a34cd-131">Storage hosting persistent network information and coordinating leasing</span></span>
* <span data-ttu-id="a34cd-132">Operations Management Suite(몇 가지 Azure 서비스 번들)는 사용 가능한 노드, 분당 트랜잭션 수 및 컨소시엄 멤버에 대한 인사이트를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-132">Operations Management Suite (a bundling of a few Azure services) provides insight into available nodes, transactions per minute and consortium members</span></span>

### <a name="alternatives"></a><span data-ttu-id="a34cd-133">대안</span><span class="sxs-lookup"><span data-stu-id="a34cd-133">Alternatives</span></span>

<span data-ttu-id="a34cd-134">이 예제에서는 신뢰할 수 있고 분산되고 이해하기 쉬운 방법으로 정보를 서로 교환 및 공유할 수 있는 환경을 만들려는 조직 컨소시엄을 위한 좋은 진입점인 Ethereum PoA 접근 방식을 선택했습니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-134">The Ethereum PoA approach is chosen for this example because it is a good entry point for a consortium of organizations that want to create an environment where information can be exchanged and shared with one another easily in a trusted, decentralized, and easy to understand way.</span></span> <span data-ttu-id="a34cd-135">제공되는 Azure 솔루션 템플릿도 컨소시엄 리더가 Ethereum PoA 블록체인을 시작하고 컨소시엄의 멤버 조직이 자체 리소스 그룹 및 구독 내에서 자체 Azure 리소스를 가동하여 기존 네트워크에 조인할 수 있는 빠르고 간편한 방법을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-135">The available Azure solution templates also provide a fast and convenient way not just for a consortium leader to start an Ethereum PoA blockchain, but also for member organizations in the consortium to spin up their own Azure resources within their own resource group and subscription to join an existing network.</span></span>

<span data-ttu-id="a34cd-136">기타 확장 시나리오 또는 다른 시나리오의 경우 트랜잭션 개인 정보 보호 같은 우려가 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-136">For other extended or different scenarios, concerns such as transaction privacy may arise.</span></span> <span data-ttu-id="a34cd-137">예를 들어 보안 전송 시나리오에서 컨소시엄의 멤버가 다른 멤버에게도 트랜잭션을 보여주려 하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-137">For example, in a securities transfer scenario, members in a consortium may not want their transactions to be visible even to other members.</span></span> <span data-ttu-id="a34cd-138">고유한 방식으로 이러한 문제를 해결하는 Ethereum PoA의 대안이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-138">Other alternatives to Ethereum PoA exist that addresses these concerns in their own way:</span></span>

* <span data-ttu-id="a34cd-139">Corda</span><span class="sxs-lookup"><span data-stu-id="a34cd-139">Corda</span></span>
* <span data-ttu-id="a34cd-140">Quorum</span><span class="sxs-lookup"><span data-stu-id="a34cd-140">Quorum</span></span>
* <span data-ttu-id="a34cd-141">Hyperledger</span><span class="sxs-lookup"><span data-stu-id="a34cd-141">Hyperledger</span></span>

## <a name="considerations"></a><span data-ttu-id="a34cd-142">고려 사항</span><span class="sxs-lookup"><span data-stu-id="a34cd-142">Considerations</span></span>

### <a name="availability"></a><span data-ttu-id="a34cd-143">가용성</span><span class="sxs-lookup"><span data-stu-id="a34cd-143">Availability</span></span>

<span data-ttu-id="a34cd-144">[Azure Monitor][monitor]는 가용성을 보장하기 위해 문제에 대한 블록체인 네트워크를 지속적으로 모니터링하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-144">[Azure Monitor][monitor] is used to continuously monitor the blockchain network for issues to ensure availability.</span></span> <span data-ttu-id="a34cd-145">이 시나리오에 사용되는 블록체인 솔루션 템플릿이 성공적으로 배포되는 즉시 Azure Monitor 기반의 사용자 지정 모니터링 대시보드에 대한 링크가 전송됩니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-145">A link to a custom monitoring dashboard based on Azure Monitor will be sent to you upon successful deployment of the blockchain solution template used in this scenario.</span></span> <span data-ttu-id="a34cd-146">대시보드는 지난 30분의 하트비트와 기타 유용한 통계를 보고하는 노드를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-146">The dashboard shows nodes that are reporting heartbeats in the past 30 minutes as well as other useful statistics.</span></span> 

<span data-ttu-id="a34cd-147">다른 가용성 항목에 대해서는 Azure 아키텍처 센터의 [가용성 검사 목록][availability]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="a34cd-147">For other availability topics, see the [availability checklist][availability] in the Azure Architecture Center.</span></span>

### <a name="scalability"></a><span data-ttu-id="a34cd-148">확장성</span><span class="sxs-lookup"><span data-stu-id="a34cd-148">Scalability</span></span>

<span data-ttu-id="a34cd-149">블록체인에 대한 일반적인 우려는 미리 정해진 시간 내에 블록체인이 포함할 수 있는 트랜잭션의 수입니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-149">A popular concern for blockchain is the number of transactions that a blockchain can include within a preset amount of time.</span></span> <span data-ttu-id="a34cd-150">이 시나리오에서는 이러한 확장성을 작업 증명보다 잘 관리할 수 있는 인증 증명을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-150">This scenario uses Proof-of-Authority where such scalability can be better managed than Proof-of-Work.</span></span> <span data-ttu-id="a34cd-151">인증 증명&ndash;기반 네트워크에서는 합의 참가자가 알려져 있고 관리되기 때문에 서로 알고 있는 조직 컨소시엄을 위한 비공개 블록체인에 더 적합합니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-151">In Proof-of-Authority&ndash;based networks, consensus participants are known and managed, making it more suitable for private blockchain for a consortium of organization that knows one another.</span></span> <span data-ttu-id="a34cd-152">사용자 지정 대시보드를 통해 평균 블록 시간, 분당 트랜잭션, 계산 리소스 사용량 등의 매개 변수를 간편하게 모니터링할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-152">Parameters such as average block time, transactions per minute and compute resource consumption can be easily monitored via the custom dashboard.</span></span> <span data-ttu-id="a34cd-153">규모 요구 사항에 따라 리소스를 조정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-153">Resources can then be adjusted accordingly based on scale requirements.</span></span>

<span data-ttu-id="a34cd-154">확장 가능한 솔루션 설계에 대한 일반적인 지침은 Azure 아키텍처 센터의 [확장성 검사 목록][scalability]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="a34cd-154">For general guidance on designing scalable solutions, see the [scalability checklist][scalability] in the Azure Architecture Center.</span></span>

### <a name="security"></a><span data-ttu-id="a34cd-155">보안</span><span class="sxs-lookup"><span data-stu-id="a34cd-155">Security</span></span>

<span data-ttu-id="a34cd-156">[Azure Key Vault][vault]는 유효성 검사기의 개인 키를 간편하게 저장하고 관리하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-156">[Azure Key Vault][vault] is used to easily store and manage the private keys of validators.</span></span> <span data-ttu-id="a34cd-157">이 예제의 기본 배포는 인터넷을 통해 액세스할 수 있는 블록체인 네트워크를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-157">The default deployment in this example creates a blockchain network that is accessible via the internet.</span></span> <span data-ttu-id="a34cd-158">비공개 네트워크를 원하는 프로덕션 시나리오의 경우 VNet-VNet VPN 게이트웨이 연결을 통해 멤버를 서로 연결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-158">For production scenario where a private network is desired, members can be connected to each other via VNet-to-VNet VPN gateway connections.</span></span> <span data-ttu-id="a34cd-159">VPN을 구성하는 단계는 아래의 관련 리소스 섹션에 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-159">The steps for configuring a VPN are included in the related resources section below.</span></span>

<span data-ttu-id="a34cd-160">보안 솔루션 설계에 대한 일반적인 지침은 [Azure 보안 설명서][security]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="a34cd-160">For general guidance on designing secure solutions, see the [Azure Security Documentation][security].</span></span>

### <a name="resiliency"></a><span data-ttu-id="a34cd-161">복원력</span><span class="sxs-lookup"><span data-stu-id="a34cd-161">Resiliency</span></span>

<span data-ttu-id="a34cd-162">유효성 검사기 노드를 다른 지역에 배포할 수 있으므로 Ethereum PoA 블록체인은 그 자체로 일정 수준의 복원력을 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-162">The Ethereum PoA blockchain can itself provide some degree of resilience as the validator nodes can be deployed in different regions.</span></span> <span data-ttu-id="a34cd-163">Azure는 전 세계의 54개가 넘는 지역에 배포할 수 있는 옵션을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-163">Azure has options for deployments in over 54 regions worldwide.</span></span> <span data-ttu-id="a34cd-164">이 시나리오와 비슷한 블록체인은 협력을 통해 복원력을 높이는 고유하고 신선한 가능성을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-164">A blockchain such as the one in this scenario provides unique and refreshing possibilities of cooperation to increase resilience.</span></span> <span data-ttu-id="a34cd-165">네트워크 복원력은 중앙 집중식 단일 멤버가 아닌 컨소시엄의 모든 멤버를 통해 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-165">The resilience of the network is not just provided for by a single centralized party but all members of the consortium.</span></span> <span data-ttu-id="a34cd-166">인증 증명&ndash;기반 블록체인을 사용하면 네트워크를 훨씬 계획적이고 의도적으로 복원할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-166">A proof-of-authority&ndash;based blockchain allows network resilience to be even more planned and deliberate.</span></span>

<span data-ttu-id="a34cd-167">복원력 있는 솔루션 설계에 대한 일반적인 지침은 [복원력 있는 Azure 응용 프로그램 디자인][resiliency]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="a34cd-167">For general guidance on designing resilient solutions, see [Designing resilient applications for Azure][resiliency].</span></span>

## <a name="pricing"></a><span data-ttu-id="a34cd-168">가격</span><span class="sxs-lookup"><span data-stu-id="a34cd-168">Pricing</span></span>

<span data-ttu-id="a34cd-169">이 시나리오를 실행하는 데 들어가는 비용을 알아보기 위해 모든 서비스가 비용 계산기에서 미리 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-169">To explore the cost of running this scenario, all of the services are pre-configured in the cost calculator.</span></span> <span data-ttu-id="a34cd-170">특정 사용 사례에 대한 가격 변동을 확인하려면 예상 성능 및 가용성 요구 사항에 맞게 변수를 적절하게 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-170">To see how the pricing would change for your particular use case, change the appropriate variables to match your expected performance and availability requirements.</span></span>

<span data-ttu-id="a34cd-171">응용 프로그램을 실행하는 확장 집합 VM 인스턴스의 수를 기준으로 세 가지 샘플 비용 프로필이 제공되었습니다(인스턴스가 다른 지역에 상주할 수 있음).</span><span class="sxs-lookup"><span data-stu-id="a34cd-171">We have provided three sample cost profiles based on the number of scale set VM instances that run your applications (the instances can reside in different regions).</span></span>

* <span data-ttu-id="a34cd-172">[소형][small-pricing]: 이 가격 책정 예제는 매월 모니터링이 꺼진 VM 2대와 관련이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-172">[Small][small-pricing]: this pricing example correlates to 2 VMs per month with monitoring turned off</span></span>
* <span data-ttu-id="a34cd-173">[중형][medium-pricing]: 이 가격 책정 예제는 매월 모니터링이 켜진 VM 7대와 관련이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-173">[Medium][medium-pricing]: this pricing example correlates to 7 VMs per month with monitoring turned on</span></span>
* <span data-ttu-id="a34cd-174">[대형][large-pricing]: 이 가격 책정 예제는 매월 모니터링이 켜진 VM 15대와 관련이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-174">[Large][large-pricing]: this pricing example correlates to 15 VMs per month with monitoring turned on</span></span>

<span data-ttu-id="a34cd-175">위의 가격 책정은 한 컨소시엄 멤버가 블록체인 네트워크를 시작 또는 조인하기 위한 것입니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-175">The above pricing is for one consortium member to start or join a blockchain network.</span></span> <span data-ttu-id="a34cd-176">일반적으로 여러 회사 또는 조직이 관련되는 컨소시엄의 각 멤버는 고유의 Azure 구독을 받습니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-176">Typically in a consortium where there are multiple companies or organizations involved, each member will get their own Azure subscription.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a34cd-177">다음 단계</span><span class="sxs-lookup"><span data-stu-id="a34cd-177">Next Steps</span></span>

<span data-ttu-id="a34cd-178">이 시나리오의 예제를 보려면 Azure에 [Ethereum PoA 블록체인 데모 애플리케이션][deploy]을 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-178">To see an example of this scenario, deploy the [Ethereum PoA blockchain demo application][deploy] on Azure.</span></span> <span data-ttu-id="a34cd-179">그런 다음, [시나리오 소스 코드의 추가 정보][source]를 검토합니다.</span><span class="sxs-lookup"><span data-stu-id="a34cd-179">Then review the [README of the scenario source code][source].</span></span>

## <a name="related-resources"></a><span data-ttu-id="a34cd-180">관련 리소스</span><span class="sxs-lookup"><span data-stu-id="a34cd-180">Related resources</span></span>

<span data-ttu-id="a34cd-181">Azure에 Ethereum 인증 증명 솔루션 템플릿을 사용하는 방법에 대한 자세한 내용은 이 [사용 가이드][guide]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="a34cd-181">For more information on using the Ethereum Proof-of-Authority solution template for Azure, review this [usage guide][guide].</span></span>

<!-- links -->
[small-pricing]: https://azure.com/e/4e429d721eb54adc9a1558fae3e67990
[medium-pricing]: https://azure.com/e/bb42cd77437744be8ed7064403bfe2ef
[large-pricing]: https://azure.com/e/e205b443de3e4adfadf4e09ffee30c56
[guide]: /azure/blockchain-workbench/ethereum-poa-deployment
[deploy]: https://portal.azure.com/?pub_source=email&pub_status=success#create/microsoft-azure-blockchain.azure-blockchain-ethereumethereum-poa-consortium
[source]: https://github.com/vitoc/creditscoreblockchain
[monitor]: /azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor
[availability]: /azure/architecture/checklist/availability
[scalability]: /azure/architecture/checklist/scalability
[resiliency]: ../../resiliency/index.md
[security]: /azure/security/
[vault]: https://azure.microsoft.com/services/key-vault/
