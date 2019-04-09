---
title: 'CAF: 클라우드 리소스 거버넌스란?'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.date: 02/11/2019
description: Azure에서 클라우드 리소스 거버넌스 설명
author: petertaylor9999
ms.openlocfilehash: 0989a5aad8a6290cce07fd71771c690bbd615e0d
ms.sourcegitcommit: 0a8a60d782facc294f7f78ec0e9033e3ee16bf4a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068857"
---
<!-- markdownlint-disable MD026 -->

# <a name="what-is-cloud-resource-governance"></a><span data-ttu-id="edf80-103">클라우드 리소스 거버넌스란?</span><span class="sxs-lookup"><span data-stu-id="edf80-103">What is cloud resource governance?</span></span>

<span data-ttu-id="edf80-104">[Azure의 작동?](what-is-azure.md), Azure 서버의 컬렉션 임을 학습 및 네트워킹 하드웨어 사용자를 대신해 서 가상화 된 하드웨어 및 소프트웨어를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="edf80-104">In [How does Azure work?](what-is-azure.md), you learned that Azure is a collection of servers and networking hardware running virtualized hardware and software on behalf of users.</span></span> <span data-ttu-id="edf80-105">Azure는 귀사의 응용 프로그램 개발 및 IT 부서에서 쉽게 만들기, 읽기, 업데이트 및 필요에 따라 리소스를 삭제 하 여 agile 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="edf80-105">Azure enables your organization's application development and IT departments to be agile by making it easy to create, read, update, and delete resources as needed.</span></span>

<span data-ttu-id="edf80-106">그러나 리소스에 대 한 무제한 액세스를 만들 수 개발자가 매우 민첩 하는 동안 예기치 않은 비용에도 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="edf80-106">However, while unrestricted access to resources can make developers very agile, it can also lead to unexpected costs.</span></span> <span data-ttu-id="edf80-107">예를 들어 개발 팀은 테스트를 위한 리소스 집합을 배포하도록 승인받지만 테스트를 완료하는 경우 리소스 집합을 삭제하는 것을 잊어버릴 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="edf80-107">For example, a development team might be approved to deploy a set of resources for testing but forget to delete them when testing is complete.</span></span> <span data-ttu-id="edf80-108">이러한 리소스는 계속 승인 하거나 필요할 때 더 이상에 비용을 계산 합니다.</span><span class="sxs-lookup"><span data-stu-id="edf80-108">These resources will continue to accrue costs even though they are no longer approved or necessary.</span></span>

<span data-ttu-id="edf80-109">이 방법은 리소스 액세스 거 버 넌 스입니다.</span><span class="sxs-lookup"><span data-stu-id="edf80-109">The solution is resource access governance.</span></span> <span data-ttu-id="edf80-110">**거 버 넌 스** 관리, 모니터링 및 조직의 요구 사항에 맞게 Azure 리소스의 사용을 감사 하는 지속적인 프로세스입니다.</span><span class="sxs-lookup"><span data-stu-id="edf80-110">**Governance** is the ongoing process of managing, monitoring, and auditing the use of Azure resources to meet the requirements of your organization.</span></span>

<!-- markdownlint-disable MD034 -->

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2ii94]

<!-- markdownlint-enable MD034 -->

<span data-ttu-id="edf80-111">이러한 요구 사항은 각 조직에 고유한 되므로 보편적으로 적용할 거 버 넌 스는 도움이 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="edf80-111">These requirements are unique to each organization, so a one-size-fits-all approach to governance isn't helpful.</span></span> <span data-ttu-id="edf80-112">대신에 달려 각 조직에 Azure의 두 가지 기본 관리 도구를 사용 하 여 해당 거 버 넌 스 모델 디자인: **역할 기반 액세스 제어 (RBAC)** 하 고 **리소스 정책**합니다.</span><span class="sxs-lookup"><span data-stu-id="edf80-112">Instead, it's up to each organization to design their governance model using Azure's two primary governance tools: **role-based access control (RBAC)** and **resource policy**.</span></span>

<span data-ttu-id="edf80-113">해당 역할을 할당 하는 각 사용자의 기능을 정의 하는 역할 및 RBAC 역할을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="edf80-113">RBAC defines roles, and roles define the capabilities of each user assigned that role.</span></span> <span data-ttu-id="edf80-114">예를 들어를 **소유자** 역할이 모든 기능을 허용 (만들기, 읽기, 업데이트 및 삭제) 리소스에 대 한 동안는 **판독기** 역할에서 읽기 기능만 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="edf80-114">For example, the **owner** role allows all capabilites (create, read, update, and delete) for a resource, while the  **reader** role allows only the read capability.</span></span> <span data-ttu-id="edf80-115">여러 리소스 유형에 적용 되는 광범위 한 범위 또는 일부에 적용 되는 좁은 범위를 사용 하 여 역할을 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="edf80-115">Roles can be defined with a broad scope that applies to many resource types, or a narrow scope that applies to a few.</span></span>

<span data-ttu-id="edf80-116">리소스 정책은 리소스 만들기에 대한 규칙을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="edf80-116">Resource policies define rules for resource creation.</span></span> <span data-ttu-id="edf80-117">예를 들어, 리소스 정책을 특정 사전 승인 된 크기로 가상 컴퓨터의 SKU를 제한할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="edf80-117">For example, a resource policy can limit the SKU of a virtual machine to a particular pre-approved size.</span></span> <span data-ttu-id="edf80-118">다른 리소스 정책 리소스를 만들 요청 될 때는 할당 된 비용 센터에 대 한 태그의 응용 프로그램을 적용 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="edf80-118">Another resource policy could enforce the application of a tag for an assigned cost center when the request is made to create the resource.</span></span>

<span data-ttu-id="edf80-119">이러한 도구를 구성할 때에 거 버 넌 스 및 조직의 생산성 균형을 유지 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="edf80-119">When configuring these tools, it is important to balance governance and organizational agility.</span></span> <span data-ttu-id="edf80-120">더 제한적인 거 버 넌 스 정책 덜 민첩 한 개발자 및 IT 근로자 업체 됩니다.</span><span class="sxs-lookup"><span data-stu-id="edf80-120">The more restrictive your governance policy, the less agile your developers and IT workers will be.</span></span> <span data-ttu-id="edf80-121">제한적인 거 버 넌 스 정책의 양식을 작성 하거나 리소스를 수동으로 만들려면 거 버 넌 스 팀의 구성원 전자 메일 보내기 개발자 요구와 같은 더 많은 수동 단계가 필요할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="edf80-121">A restrictive governance policy may require more manual steps like requiring a developer to fill out a form or send an email to a member of the governance team to manually create a resource.</span></span> <span data-ttu-id="edf80-122">거 버 넌 스 팀 유한한 용량 있고 해당 리소스 삭제 되기 전에 비용을 발생 하는 만들거나 불필요 한 리소스를 많이 대기 하는 개발 팀에 병목 상태가 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="edf80-122">The governance team has finite capacity and may become a bottleneck, resulting in development teams waiting unproductively for their resources to be created or unneeded resources accruing costs before they are deleted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="edf80-123">다음 단계</span><span class="sxs-lookup"><span data-stu-id="edf80-123">Next steps</span></span>

<span data-ttu-id="edf80-124">클라우드 리소스 거 버 넌 스의 개념을 이해 했으므로 Azure에서 리소스 액세스를 관리 하는 방법을 자세히 알아봅니다.</span><span class="sxs-lookup"><span data-stu-id="edf80-124">Now that you understand the concept of cloud resource governance, learn more about how resource access is managed in Azure.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="edf80-125">Azure의 리소스 액세스에 알아봅니다</span><span class="sxs-lookup"><span data-stu-id="edf80-125">Learn about resource access in Azure</span></span>](azure-resource-access.md)
