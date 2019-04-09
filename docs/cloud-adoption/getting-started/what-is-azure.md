---
title: 'CAF: Azure의 작동 방식'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
description: Azure의 내부 작동 방식 설명
author: petertaylor9999
ms.date: 02/11/2019
ms.openlocfilehash: 215e4e4954a2f3e722e3ac865fea19508f6edadd
ms.sourcegitcommit: 0a8a60d782facc294f7f78ec0e9033e3ee16bf4a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068823"
---
<!-- markdownlint-disable MD026 -->

# <a name="how-does-azure-work"></a><span data-ttu-id="ce97c-103">Azure의 작동 방식</span><span class="sxs-lookup"><span data-stu-id="ce97c-103">How does Azure work?</span></span>

<span data-ttu-id="ce97c-104">Azure는 Microsoft의 공용 클라우드 플랫폼입니다.</span><span class="sxs-lookup"><span data-stu-id="ce97c-104">Azure is Microsoft's public cloud platform.</span></span> <span data-ttu-id="ce97c-105">Azure는 대규모 platform-as-a-service (PaaS), 인프라 (IaaS) 서비스 (DBaaS) 기능으로 데이터베이스 서비스를 비롯 하 여 서비스의 컬렉션을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="ce97c-105">Azure offers a large collection of services including platform as a service (PaaS), infrastructure as a service (IaaS), database as a service (DBaaS) capabilities.</span></span> <span data-ttu-id="ce97c-106">하지만 Azure란 정확히 무엇이며 어떤 방식으로 작동할까요?</span><span class="sxs-lookup"><span data-stu-id="ce97c-106">But what exactly is Azure, and how does it work?</span></span>

<!-- markdownlint-disable MD034 -->

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2ixGo]

<!-- markdownlint-enable MD034 -->

<span data-ttu-id="ce97c-107">다른 클라우드 플랫폼과 마찬가지로, Azure는 **가상화**라는 기술에 의존합니다.</span><span class="sxs-lookup"><span data-stu-id="ce97c-107">Azure, like other cloud platforms, relies on a technology known as **virtualization**.</span></span> <span data-ttu-id="ce97c-108">대부분의 컴퓨터 하드웨어는 간단히 말해서 실리콘에 영구적으로 또는 반영구적으로 인코딩되는 명령 집합이므로, 소프트웨어에 에뮬레이트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ce97c-108">Most computer hardware can be emulated in software, because most computer hardware is simply a set of instructions permanently or semi-permanently encoded in silicon.</span></span> <span data-ttu-id="ce97c-109">소프트웨어 명령을 하드웨어의 명령에 매핑하는 에뮬레이션 계층을 사용하여, 가상화된 하드웨어는 마치 실제 하드웨어인 것처럼 소프트웨어에서 실행될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ce97c-109">Using an emulation layer that maps software instructions to hardware instructions, virtualized hardware can execute in software as if it were the actual hardware itself.</span></span>

<span data-ttu-id="ce97c-110">기본적으로, 클라우드는 고객을 대신해서 가상화된 하드웨어를 실행하는 하나 이상의 데이터 센터의 물리적 서버 집합입니다.</span><span class="sxs-lookup"><span data-stu-id="ce97c-110">Essentially, the cloud is a set of physical servers in one or more datacenters that execute virtualized hardware on behalf of customers.</span></span> <span data-ttu-id="ce97c-111">그렇다면 클라우드는 어떻게 수백만 명의 고객을 위해 수백만 개의 가상화된 하드웨어 인스턴스를 동시에 만들고 시작하고 중지하고 삭제할 수 있을까요?</span><span class="sxs-lookup"><span data-stu-id="ce97c-111">So how does the cloud create, start, stop, and delete millions of instances of virtualized hardware for millions of customers simultaneously?</span></span>

<span data-ttu-id="ce97c-112">이를 이해하기 위해 데이터 센터의 하드웨어 아키텍처를 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="ce97c-112">To understand this, let's look at the architecture of the hardware in the datacenter.</span></span>  <span data-ttu-id="ce97c-113">각 데이터 센터 내에서 컬렉션인 서버 랙에 앉아 서버.</span><span class="sxs-lookup"><span data-stu-id="ce97c-113">Inside each datacenter is a collection of servers sitting in server racks.</span></span> <span data-ttu-id="ce97c-114">각 서버 랙에는 네트워크 연결 및 전력을 공급하는 PDU(전원 분배 장치)를 제공하는 네트워크 스위치와 함께 많은 서버 **블레이드**가 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ce97c-114">Each server rack contains many server **blades** as well as a network switch providing network connectivity and a power distribution unit (PDU) providing power.</span></span> <span data-ttu-id="ce97c-115">경우에 따라 랙은 **클러스터**라는 좀 더 큰 단위로 함께 그룹화됩니다.</span><span class="sxs-lookup"><span data-stu-id="ce97c-115">Racks are sometimes grouped together in larger units known as **clusters**.</span></span>

<span data-ttu-id="ce97c-116">각 랙 또는 클러스터 내에서, 대부분의 서버는 사용자 대신 이러한 가상화된 하드웨어 인스턴스를 실행하도록 지정됩니다.</span><span class="sxs-lookup"><span data-stu-id="ce97c-116">Within each rack or cluster, most of the servers are designated to run these virtualized hardware instances on behalf of the user.</span></span> <span data-ttu-id="ce97c-117">그러나 패브릭 컨트롤러 라고 하는 클라우드 관리 소프트웨어를 실행할 서버 중 일부입니다.</span><span class="sxs-lookup"><span data-stu-id="ce97c-117">However, some of the servers run cloud management software known as a fabric controller.</span></span> <span data-ttu-id="ce97c-118">**패브릭 컨트롤러**는 많은 책임을 담당하는 분산된 애플리케이션입니다.</span><span class="sxs-lookup"><span data-stu-id="ce97c-118">The **fabric controller** is a distributed application with many responsibilities.</span></span> <span data-ttu-id="ce97c-119">서비스를 할당하고, 서버와 해당 서버에서 실행되는 서비스의 상태를 모니터링하고, 실패할 경우 서버를 복구합니다.</span><span class="sxs-lookup"><span data-stu-id="ce97c-119">It allocates services, monitors the health of the server and the services running on it, and heals servers when they fail.</span></span>

<span data-ttu-id="ce97c-120">패브릭 컨트롤러의 각 인스턴스는 클라우드 오케스트레이션 소프트웨어를 실행하는 **프런트 엔드**라고도 하는 또 다른 서버 집합에 연결됩니다.</span><span class="sxs-lookup"><span data-stu-id="ce97c-120">Each instance of the fabric controller is connected to another set of servers running cloud orchestration software, typically known as a **front end**.</span></span> <span data-ttu-id="ce97c-121">프런트 엔드는 웹 서비스, RESTful API 및 클라우드가 수행하는 모든 기능에 사용되는 내부 Azure Database를 호스트합니다.</span><span class="sxs-lookup"><span data-stu-id="ce97c-121">The front end hosts the web services, RESTful APIs, and internal Azure databases used for all functions the cloud performs.</span></span>

<span data-ttu-id="ce97c-122">프런트 엔드와 같은 Azure 리소스를 할당할 고객 요청을 처리 하는 서비스를 호스트 하는 예를 들어 [가상 네트워크](/azure/virtual-network/virtual-networks-overview)를 [가상 머신](/azure/virtual-machines), 및와 같은 서비스 [CosmosDB](/azure/cosmos-db/introduction).</span><span class="sxs-lookup"><span data-stu-id="ce97c-122">For example, the front end hosts the services that handle customer requests to allocate Azure resources such as [virtual networks](/azure/virtual-network/virtual-networks-overview), [virtual machines](/azure/virtual-machines), and services like [Cosmos DB](/azure/cosmos-db/introduction).</span></span> <span data-ttu-id="ce97c-123">첫째, 프런트 엔드는 사용자의 유효성을 검사하고 사용자가 요청된 리소스를 할당할 수 있도록 허가되는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="ce97c-123">First, the front end validates the user and verifies the user is authorized to allocate the requested resources.</span></span> <span data-ttu-id="ce97c-124">그렇다면 프런트 엔드는 충분 한 용량이 있는 서버 랙을 찾은 데이터베이스를 검사 하 고 패브릭 컨트롤러는 리소스를 할당 하는 랙에 지시 합니다.</span><span class="sxs-lookup"><span data-stu-id="ce97c-124">If so, the front end checks a database to locate a server rack with sufficient capacity and then instructs the fabric controller on that rack to allocate the resource.</span></span>

<span data-ttu-id="ce97c-125">따라서 기본적으로 Azure는 서버 및 해당 서버에서 가상화 된 하드웨어 및 소프트웨어의 작업 및 복잡 한 집합이 구성을 오케스트레이션 하는 분산된 응용 프로그램을 실행 하는 네트워킹 하드웨어의 방대한 컬렉션입니다.</span><span class="sxs-lookup"><span data-stu-id="ce97c-125">So fundamentally, Azure is a huge collection of servers and networking hardware running a complex set of distributed applications to orchestrate the configuration and operation of the virtualized hardware and software on those servers.</span></span> <span data-ttu-id="ce97c-126">또한 이러한 오케스트레이션을 통해 Azure가 강력한 &mdash; 사용자는 더 이상 유지 관리 하 고 Azure가 백그라운드에서 이러한 모든 않기 때문에 하드웨어를 업그레이드 하는 일을 담당 합니다.</span><span class="sxs-lookup"><span data-stu-id="ce97c-126">It is this orchestration that makes Azure so powerful &mdash; users are no longer responsible for maintaining and upgrading hardware because Azure does all this behind the scenes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ce97c-127">다음 단계</span><span class="sxs-lookup"><span data-stu-id="ce97c-127">Next steps</span></span>

<span data-ttu-id="ce97c-128">Azure 내부 기능을 이해 했으므로 클라우드 리소스 거 버 넌 스에 알아봅니다.</span><span class="sxs-lookup"><span data-stu-id="ce97c-128">Now that you understand Azure internals, learn about cloud resource governance.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ce97c-129">리소스 거 버 넌 스에 알아봅니다</span><span class="sxs-lookup"><span data-stu-id="ce97c-129">Learn about resource governance</span></span>](what-is-governance.md)

<!-- Links -->

[docs-add-users-to-aad]: /azure/active-directory/add-users-azure-active-directory?toc=/azure/architecture/cloud-adoption-guide/toc.json
