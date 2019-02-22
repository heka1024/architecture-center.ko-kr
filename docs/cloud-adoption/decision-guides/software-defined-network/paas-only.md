---
title: 'CAF: 소프트웨어 정의 네트워크 - PaaS 전용'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 클라우드 기반 네트워킹 기능을 위한 PaaS 전용 모델에 대한 설명
author: rotycenh
ms.openlocfilehash: 2f3f82d781ddb6544721e82e7b7d795222a2f8ff
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901727"
---
# <a name="software-defined-networks-paas-only"></a><span data-ttu-id="e2969-103">소프트웨어 정의 네트워크: PaaS 전용</span><span class="sxs-lookup"><span data-stu-id="e2969-103">Software Defined Networks: PaaS-only</span></span>

<span data-ttu-id="e2969-104">PaaS(Platform as a Service) 리소스를 구현할 때, 배포 프로세스는 네트워크를 통해 부하 분산, 포트 차단 및 다른 PaaS 서비스에 대한 연결을 비롯한 제한된 수의 컨트롤을 사용하여 가정된 기본 네트워크를 자동으로 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="e2969-104">When you implement a platform as a service (PaaS) resource, the deployment process automatically creates an assumed underlying network with a limited number of controls over that network, including load balancing, port blocking, and connections to other PaaS services.</span></span>

<span data-ttu-id="e2969-105">Azure에서는 몇 가지 PaaS 리소스 유형을 가상 네트워크에 [배포](/azure/virtual-network/virtual-network-for-azure-services)하거나 [연결](/azure/virtual-network/virtual-network-service-endpoints-overview)하기 때문에 이러한 리소스를 기존 가상 네트워킹 인프라와 통합할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e2969-105">In Azure, several PaaS resource types can be [deployed into](/azure/virtual-network/virtual-network-for-azure-services) or [connected to](/azure/virtual-network/virtual-network-service-endpoints-overview) a virtual network, allowing these resources to integrate with your existing virtual networking infrastructure.</span></span> <span data-ttu-id="e2969-106">하지만, 대부분의 경우 PaaS 리소스가 기본적으로 제공하는 기본 네트워킹 기능에만 의존하는 PaaS 전용 네트워킹 아키텍처는 워크로드 요구 사항을 충족하기에 충분합니다.</span><span class="sxs-lookup"><span data-stu-id="e2969-106">However, in many cases a PaaS only networking architecture, relying only on these default networking capabilities natively provided by PaaS resources, is sufficient to meet workload requirements.</span></span>

<span data-ttu-id="e2969-107">PaaS 전용 네트워킹 아키텍처를 고려하는 경우에는 필요한 가정이 요구 사항과 일치하는지 확인해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e2969-107">If you are considering a PaaS only networking architecture, be sure you validate that the required assumptions align with your requirements.</span></span>

## <a name="paas-only-assumptions"></a><span data-ttu-id="e2969-108">PaaS 전용 가정</span><span class="sxs-lookup"><span data-stu-id="e2969-108">PaaS-only assumptions</span></span>

<span data-ttu-id="e2969-109">PaaS 전용 네트워킹 아키텍처 배포의 경우 다음 사항을 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="e2969-109">Deploying a PaaS-only networking architecture assumes the following:</span></span>

- <span data-ttu-id="e2969-110">배포되는 애플리케이션이 독립 실행형 애플리케이션이거나 다른 PaaS 리소스에만 종속됩니다.</span><span class="sxs-lookup"><span data-stu-id="e2969-110">The application being deployed is a standalone application OR is dependent on only other PaaS resources.</span></span>
- <span data-ttu-id="e2969-111">IT 운영 팀이 독립 실행형 PaaS 애플리케이션의 관리, 구성 및 배포를 지원하도록 툴, 교육 및 프로세스를 업데이트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e2969-111">Your IT operations teams can update their tools, training, and processes to support management, configuration, and deployment of standalone PaaS applications.</span></span>
- <span data-ttu-id="e2969-112">PaaS 애플리케이션이 IaaS 리소스를 포함하는 광범위한 클라우드 마이그레이션 작업에 속하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e2969-112">The PaaS application is not part of a broader cloud migration effort that will include IaaS resources.</span></span>

<span data-ttu-id="e2969-113">이러한 가정은 PaaS 전용 네트워크를 배포하는 데 필요한 최소 한정자입니다.</span><span class="sxs-lookup"><span data-stu-id="e2969-113">These assumptions are minimum qualifiers aligned to deploying a PaaS-only network.</span></span> <span data-ttu-id="e2969-114">이러한 접근 방식이 단일 애플리케이션 배포의 요구 사항에 맞을 수 있지만, 클라우드 도입 팀은 다음과 같은 장기적인 질문을 검토해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e2969-114">While this approach may align with the requirements of a single application deployment, your Cloud Adoption Team should examine these long-term questions:</span></span>

- <span data-ttu-id="e2969-115">배포의 범위가 확장되거나 배포 규모가 확장되어 PaaS 이외의 다른 리소스에 대한 액세스가 필요하게 될까요?</span><span class="sxs-lookup"><span data-stu-id="e2969-115">Will this deployment expand in scope or scale to require access to other non-PaaS resources?</span></span>
- <span data-ttu-id="e2969-116">현재 솔루션 이외에 다른 PaaS 배포가 계획되어 있나요?</span><span class="sxs-lookup"><span data-stu-id="e2969-116">Are other PaaS deployments planned beyond the current solution?</span></span>
- <span data-ttu-id="e2969-117">조직에 향후 다른 클라우드 마이그레이션에 대한 계획이 있나요?</span><span class="sxs-lookup"><span data-stu-id="e2969-117">Does the organization have plans for other future cloud migrations?</span></span>

<span data-ttu-id="e2969-118">이러한 질문에 대한 답변 때문에 팀이 PaaS 전용 옵션을 선택하지 않을 리는 없지만 최종 결정을 내리기 전에 이러한 내용을 고려해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e2969-118">The answers to these questions would not preclude a team from choosing a PaaS only option but should be considered before making a final decision.</span></span>
