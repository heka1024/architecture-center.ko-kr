---
title: 'CAF: Azure에서 리소스 액세스 관리'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 리소스 액세스 관리의 설명으로 Azure에서 다음을 생성합니다. Azure Resource Manager, 구독, 리소스 그룹 및 리소스
author: petertaylor9999
ms.openlocfilehash: b98cdc94d6d3a37c1e65da1d4de35d5d9520d6eb
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55902152"
---
# <a name="resource-access-management-in-azure"></a><span data-ttu-id="29a3a-103">Azure에서 리소스 액세스 관리</span><span class="sxs-lookup"><span data-stu-id="29a3a-103">Resource access management in Azure</span></span>

<span data-ttu-id="29a3a-104">[클라우드 거버넌스](../overview.md)에서는 리소스 관리를 포함하는 5개 클라우드 거버넌스 분야에 대해 간략하게 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-104">[Cloud Governance](../overview.md) outlines the five disciplines of Cloud Governance, which includes Resource Management.</span></span>  <span data-ttu-id="29a3a-105">[리소스 액세스 거버넌스란?](overview.md)에서는 리소스 액세스 관리가 리소스 관리 분야에 적용되는 방법에 대해 자세히 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-105">[What is resource access governance](overview.md) furthers explains how resource access management fits into the resource management discipline.</span></span> <span data-ttu-id="29a3a-106">거버넌스 모델을 디자인하는 방법을 알아보기 위해 이동하려면 먼저 Azure에서 리소스 액세스 관리 제어를 이해해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-106">Before you move on to learn how to design a governance model, it's important to understand the resource access management controls in Azure.</span></span> <span data-ttu-id="29a3a-107">이러한 리소스 액세스 관리 제어의 구성은 거버넌스 모델의 기본을 형성합니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-107">The configuration of these resource access management controls forms the basis of your governance model.</span></span>

<span data-ttu-id="29a3a-108">Azure에서 리소스가 배포되는 방법을 더 자세히 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-108">Begin by taking a closer look at how resources are deployed in Azure.</span></span>

<!-- markdownlint-disable MD026 -->

## <a name="what-is-an-azure-resource"></a><span data-ttu-id="29a3a-109">Azure 리소스란?</span><span class="sxs-lookup"><span data-stu-id="29a3a-109">What is an Azure resource?</span></span>

<span data-ttu-id="29a3a-110">Azure에서 **리소스**라는 용어는 Azure에서 관리하는 엔터티를 가리킵니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-110">In Azure, the term **resource** refers to an entity managed by Azure.</span></span> <span data-ttu-id="29a3a-111">예를 들어 가상 머신, 가상 네트워크 및 저장소 계정은 모두 Azure 리소스를 가리킵니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-111">For example, virtual machines, virtual networks, and storage accounts are all referred to as Azure resources.</span></span>

<span data-ttu-id="29a3a-112">![리소스의 다이어그램](../../_images/governance-1-9.png)
*그림 1. 리소스*</span><span class="sxs-lookup"><span data-stu-id="29a3a-112">![Diagram of a resource](../../_images/governance-1-9.png)
*Figure 1. A resource.*</span></span>

## <a name="what-is-an-azure-resource-group"></a><span data-ttu-id="29a3a-113">Azure 리소스 그룹이란?</span><span class="sxs-lookup"><span data-stu-id="29a3a-113">What is an Azure resource group?</span></span>

<span data-ttu-id="29a3a-114">Azure의 각 리소스는 [리소스 그룹](/azure/azure-resource-manager/resource-group-overview#resource-groups)에 속해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-114">Each resource in Azure must belong to a [resource group](/azure/azure-resource-manager/resource-group-overview#resource-groups).</span></span> <span data-ttu-id="29a3a-115">리소스 그룹은 단순히 단일 엔터티로 관리될 수 있도록 여러 리소스를 그룹화하는 논리적 구문입니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-115">A resource group is simply a logical construct that groups multiple resources together so they can be managed as a single entity.</span></span> <span data-ttu-id="29a3a-116">예를 들어 [n 계층 애플리케이션](/azure/architecture/guide/architecture-styles/n-tier)에 대한 리소스와 같은 비슷한 수명 주기를 공유하는 리소스를 그룹으로 만들거나 삭제할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-116">For example, resources that share a similar lifecycle, such as the resources for an [n-tier application](/azure/architecture/guide/architecture-styles/n-tier) may be created or deleted as a group.</span></span>

<span data-ttu-id="29a3a-117">![리소스가 포함된 리소스 그룹의 다이어그램](../../_images/governance-1-10.png)
*그림 2. 리소스 그룹에는 리소스가 포함됩니다.*</span><span class="sxs-lookup"><span data-stu-id="29a3a-117">![Diagram of a resource group containing a resource](../../_images/governance-1-10.png)
*Figure 2. A resource group contains a resource.*</span></span>

<span data-ttu-id="29a3a-118">리소스 그룹 및 여기 포함된 리소스는 Azure **구독**과 연결됩니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-118">Resource groups and the resources they contain are associated with an Azure **subscription**.</span></span>

## <a name="what-is-an-azure-subscription"></a><span data-ttu-id="29a3a-119">Azure 구독이란?</span><span class="sxs-lookup"><span data-stu-id="29a3a-119">What is an Azure subscription?</span></span>

<span data-ttu-id="29a3a-120">Azure 구독은 리소스 그룹 및 해당 리소스를 함께 그룹화하는 논리적 구문에서 리소스 그룹과 비슷합니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-120">An Azure subscription is similar to a resource group in that it's a logical construct that groups together resource groups and their resources.</span></span> <span data-ttu-id="29a3a-121">그러나 Azure 구독은 Azure Resource Manager에서 사용되는 컨트롤과도 연결됩니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-121">However, an Azure subscription is also associated with the controls used by Azure Resource Manager.</span></span> <span data-ttu-id="29a3a-122">무슨 의미인가요?</span><span class="sxs-lookup"><span data-stu-id="29a3a-122">What does this mean?</span></span> <span data-ttu-id="29a3a-123">Azure Resource Manager를 더 자세히 살펴보면서 Azure 구독과의 관계에 대해 자세히 알아보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-123">Take a closer look at Azure Resource Manager to learn about the relationship between it and an Azure subscription.</span></span>

<span data-ttu-id="29a3a-124">![Azure 구독의 다이어그램](../../_images/governance-1-11.png)
*그림 3. Azure 구독*</span><span class="sxs-lookup"><span data-stu-id="29a3a-124">![Diagram of an Azure subscription](../../_images/governance-1-11.png)
*Figure 3. An Azure subscription.*</span></span>

## <a name="what-is-azure-resource-manager"></a><span data-ttu-id="29a3a-125">Azure Resource Manager란?</span><span class="sxs-lookup"><span data-stu-id="29a3a-125">What is Azure Resource Manager?</span></span>

<span data-ttu-id="29a3a-126">[Azure의 작동 방식](../../getting-started/what-is-azure.md)에서 Azure에는 Azure의 모든 기능을 오케스트레이션하는 여러 서비스를 사용하는 "프런트 엔드"가 포함된다는 사실을 알아보았습니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-126">In [how does Azure work?](../../getting-started/what-is-azure.md) you learned that Azure includes a "front end" with many services that orchestrate all the functions of Azure.</span></span> <span data-ttu-id="29a3a-127">[Azure Resource Manager](/azure/azure-resource-manager/)는 이러한 서비스 중 하나이며, 클라이언트에서 리소스를 관리하는 데 사용되는 RESTful API를 호스팅합니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-127">One of these services is [Azure Resource Manager](/azure/azure-resource-manager/), and this service hosts the RESTful API used by clients to manage resources.</span></span>

<span data-ttu-id="29a3a-128">![Azure Resource Manager의 다이어그램](../../_images/governance-1-12.png)
*그림 4. Azure Resource Manager*</span><span class="sxs-lookup"><span data-stu-id="29a3a-128">![Diagram of Azure Resource Manager](../../_images/governance-1-12.png)
*Figure 4. Azure Resource Manager.*</span></span>

<span data-ttu-id="29a3a-129">다음 그림에서는 [PowerShell](/powershell/azure/overview), [Azure Portal](https://portal.azure.com) 및 [Azure CLI(명령줄 인터페이스)](/cli/azure)의 세 가지 클라이언트를 보여줍니다.:</span><span class="sxs-lookup"><span data-stu-id="29a3a-129">The following figure shows three clients: [PowerShell](/powershell/azure/overview), the [Azure portal](https://portal.azure.com), and the [Azure command-line interface (CLI)](/cli/azure):</span></span>

<span data-ttu-id="29a3a-130">![Azure Resource Manager API에 연결하는 Azure 클라이언트 다이어그램](../../_images/governance-1-13.png)
*그림 5. Azure 클라이언트에서 Azure Resource Manager RESTful API에 연결합니다.*</span><span class="sxs-lookup"><span data-stu-id="29a3a-130">![Diagram of Azure clients connecting to the Azure Resource Manager API](../../_images/governance-1-13.png)
*Figure 5. Azure clients connect to the Azure Resource Manager RESTful API.*</span></span>

<span data-ttu-id="29a3a-131">이러한 클라이언트에서 RESTful API를 사용하여 Azure Resource Manager에 연결하는 한편, Azure Resource Manager에는 리소스를 직접 관리하는 기능이 포함되어 있지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-131">While these clients connect to Azure Resource Manager using the RESTful API, Azure Resource Manager does not include functionality to manage resources directly.</span></span> <span data-ttu-id="29a3a-132">대신 Azure에서 대부분의 리소스 종류에는 고유한 [**리소스 공급자**](/azure/azure-resource-manager/resource-group-overview#terminology)가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-132">Rather, most resource types in Azure have their own [**resource provider**](/azure/azure-resource-manager/resource-group-overview#terminology).</span></span>

<span data-ttu-id="29a3a-133">![Azure 리소스 공급자](../../_images/governance-1-14.png)
*그림 6. Azure 리소스 공급자.*</span><span class="sxs-lookup"><span data-stu-id="29a3a-133">![Azure resource providers](../../_images/governance-1-14.png)
*Figure 6. Azure resource providers.*</span></span>

<span data-ttu-id="29a3a-134">클라이언트에서 특정 리소스를 관리하도록 요청하면 Azure Resource Manager는 해당 리소스 종류에 대한 리소스 공급자에 연결하여 요청을 완료합니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-134">When a client makes a request to manage a specific resource, Azure Resource Manager connects to the resource provider for that resource type to complete the request.</span></span> <span data-ttu-id="29a3a-135">예를 들어 클라이언트에서 가상 머신 리소스를 관리하도록 요청하는 경우 Azure Resource Manager는 **Microsoft.Compute** 리소스 공급자에 연결합니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-135">For example, if a client makes a request to manage a virtual machine resource, Azure Resource Manager connects to the **Microsoft.Compute** resource provider.</span></span>

<span data-ttu-id="29a3a-136">![Microsoft.Compute 리소스 공급자에 연결하는 Azure Resource Manager](../../_images/governance-1-15.png)
*그림 7. Azure Resource Manager에서 **Microsoft.Compute** 리소스 공급자에 연결하여 클라이언트 요청에 지정된 리소스를 관리합니다.*</span><span class="sxs-lookup"><span data-stu-id="29a3a-136">![Azure Resource Manager connecting to the Microsoft.Compute resource provider](../../_images/governance-1-15.png)
*Figure 7. Azure Resource Manager connects to the **Microsoft.Compute** resource provider to manage the resource specified in the client request.*</span></span>

<span data-ttu-id="29a3a-137">Azure Resource Manager를 사용하려면 클라이언트에서 가상 머신 리소스를 관리하기 위해 구독 및 리소스 그룹 모두에 대한 식별자를 지정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-137">Azure Resource Manager requires the client to specify an identifier for both the subscription and the resource group in order to manage the virtual machine resource.</span></span>

<span data-ttu-id="29a3a-138">이제 Azure Resource Manager의 작동 방식을 이해했으므로 Azure Resource Manager에서 사용되는 컨트롤과 Azure 구독을 연결하는 방법에 대해 다시 설명하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-138">Now that you have an understanding of how Azure Resource Manager works, return to the discussion of how an Azure subscription is associated with the controls used by Azure Resource Manager.</span></span> <span data-ttu-id="29a3a-139">Azure Resource Manager에서 리소스 관리 요청을 실행하기 전에 컨트롤 세트를 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-139">Before any resource management request can be executed by Azure Resource Manager, a set of controls are checked.</span></span>

<span data-ttu-id="29a3a-140">첫 번째 컨트롤은 유효성이 검사된 사용자가 요청해야 한다는 것이며, 사용자 ID 기능을 제공하기 위해 Azure Resource Manager에는 [Azure AD(Azure Active Directory)](/azure/active-directory/)와 신뢰할 수 있는 관계가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-140">The first control is that a request must be made by a validated user, and Azure Resource Manager has a trusted relationship with [Azure Active Directory (Azure AD)](/azure/active-directory/) to provide user identity functionality.</span></span>

<span data-ttu-id="29a3a-141">![Azure Active Directory](../../_images/governance-1-16.png)
*그림 8. Azure Active Directory*</span><span class="sxs-lookup"><span data-stu-id="29a3a-141">![Azure Active Directory](../../_images/governance-1-16.png)
*Figure 8. Azure Active Directory.*</span></span>

<span data-ttu-id="29a3a-142">Azure AD에서 사용자는 **테넌트**로 조각화됩니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-142">In Azure AD, users are segmented into **tenants**.</span></span> <span data-ttu-id="29a3a-143">테넌트는 일반적으로 조직과 연결된 Azure AD의 안전한 전용 인스턴스를 나타내는 논리적 구문입니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-143">A tenant is a logical construct that represents a secure, dedicated instance of Azure AD typically associated with an organization.</span></span> <span data-ttu-id="29a3a-144">각 구독은 Azure AD 테넌트와 연결됩니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-144">Each subscription is associated with an Azure AD tenant.</span></span>

<span data-ttu-id="29a3a-145">![구독과 연결된 Azure AD 테넌트](../../_images/governance-1-17.png)
*그림 9. 구독과 연결된 Azure AD 테넌트.*</span><span class="sxs-lookup"><span data-stu-id="29a3a-145">![An Azure AD tenant associated with a subscription](../../_images/governance-1-17.png)
*Figure 9. An Azure AD tenant associated with a subscription.*</span></span>

<span data-ttu-id="29a3a-146">특정 구독에서 리소스를 관리하는 각 클라이언트 요청에는 연결된 Azure AD 테넌트에 사용자가 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-146">Each client request to manage a resource in a particular subscription requires that the user has an account in the associated Azure AD tenant.</span></span>

<span data-ttu-id="29a3a-147">다음 컨트롤은 사용자에게 요청할 충분한 권한이 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-147">The next control is a check that the user has sufficient permission to make the request.</span></span> <span data-ttu-id="29a3a-148">[RBAC(역할 기반 액세스 제어)](/azure/role-based-access-control/)를 사용하여 사용자에 대한 보안 권한을 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-148">Permissions are assigned to users using [role-based access control (RBAC)](/azure/role-based-access-control/).</span></span>

<span data-ttu-id="29a3a-149">![RBAC 역할에 할당된 사용자](../../_images/governance-1-18.png)
*그림 10. 테넌트의 각 사용자에게는 하나 이상의 RBAC 역할이 할당됩니다.*</span><span class="sxs-lookup"><span data-stu-id="29a3a-149">![Users assigned to RBAC roles](../../_images/governance-1-18.png)
*Figure 10. Each user in the tenant is assigned one or more RBAC roles.*</span></span>

<span data-ttu-id="29a3a-150">RBAC 역할은 사용자가 특정 리소스에 대해 수행할 수 있는 일련의 사용 권한을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-150">An RBAC role specifies a set of permissions a user may take on a specific resource.</span></span> <span data-ttu-id="29a3a-151">역할을 사용자에게 할당하면 해당 사용 권한이 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-151">When the role is assigned to the user, those permissions are applied.</span></span> <span data-ttu-id="29a3a-152">예를 들어 [기본 제공 **소유자** 역할](/azure/role-based-access-control/built-in-roles#owner)을 사용하면 사용자가 리소스에서 모든 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-152">For example, the [built-in **owner** role](/azure/role-based-access-control/built-in-roles#owner) allows a user to perform any action on a resource.</span></span>

<span data-ttu-id="29a3a-153">다음 컨트롤은 [Azure 리소스 정책](/azure/governance/policy/)에 지정된 설정에서 요청이 허용되는지를 검사합니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-153">The next control is a check that the request is allowed under the settings specified for [Azure resource policy](/azure/governance/policy/).</span></span> <span data-ttu-id="29a3a-154">Azure 리소스 정책은 특정 리소스에 허용되는 작업을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-154">Azure resource policies specify the operations allowed for a specific resource.</span></span> <span data-ttu-id="29a3a-155">예를 들어 Azure 리소스 정책은 사용자가 특정 형식의 가상 머신만을 배포할 수 있도록 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-155">For example, an Azure resource policy can specify that users are only allowed to deploy a specific type of virtual machine.</span></span>

<span data-ttu-id="29a3a-156">![Azure 리소스 정책](../../_images/governance-1-19.png)
*그림 11. Azure 리소스 정책.*</span><span class="sxs-lookup"><span data-stu-id="29a3a-156">![Azure resource policy](../../_images/governance-1-19.png)
*Figure 11. Azure resource policy.*</span></span>

<span data-ttu-id="29a3a-157">다음 컨트롤은 요청이 [Azure 구독 제한](/azure/azure-subscription-service-limits)을 초과하지 않는지 검사합니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-157">The next control is a check that the request does not exceed an [Azure subscription limit](/azure/azure-subscription-service-limits).</span></span> <span data-ttu-id="29a3a-158">예를 들어 각 구독에는 구독당 980개의 리소스 그룹이라는 제한이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-158">For example, each subscription has a limit of 980 resource groups per subscription.</span></span> <span data-ttu-id="29a3a-159">제한에 도달하면 추가 리소스 그룹을 배포하는 요청이 수신된 경우 해당 요청이 거부됩니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-159">If a request is received to deploy an additional resource group once the limit has been reached, it is denied.</span></span>

<span data-ttu-id="29a3a-160">![Azure 리소스 제한](../../_images/governance-1-20.png)
*그림 12. Azure 리소스 제한.*</span><span class="sxs-lookup"><span data-stu-id="29a3a-160">![Azure resource limits](../../_images/governance-1-20.png)
*Figure 12. Azure resource limits.*</span></span>

<span data-ttu-id="29a3a-161">최종 컨트롤은 요청이 구독과 연결된 재무 약정 내에 있는지를 검사합니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-161">The final control is a check that the request is within the financial commitment associated with the subscription.</span></span> <span data-ttu-id="29a3a-162">예를 들어 가상 머신을 배포하도록 요구하는 요청인 경우 Azure Resource Manager는 구독에 충분한 결제 정보가 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-162">For example, if the request is to deploy a virtual machine, Azure Resource Manager verifies that the subscription has sufficient payment information.</span></span>

<span data-ttu-id="29a3a-163">![구독과 연결된 재무 약정](../../_images/governance-1-21.png)
*그림 13. 재무 약정이 구독과 연결됩니다.*</span><span class="sxs-lookup"><span data-stu-id="29a3a-163">![A financial commitment associated with a subscription](../../_images/governance-1-21.png)
*Figure 13. A financial commitment is associated with a subscription.*</span></span>

## <a name="summary"></a><span data-ttu-id="29a3a-164">요약</span><span class="sxs-lookup"><span data-stu-id="29a3a-164">Summary</span></span>

<span data-ttu-id="29a3a-165">이 문서에서는 Azure Resource Manager를 사용하여 Azure에서 리소스 액세스를 관리하는 방법에 대해 알아보았습니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-165">In this article, you learned about how resource access is managed in Azure using Azure Resource Manager.</span></span>

## <a name="next-steps"></a><span data-ttu-id="29a3a-166">다음 단계</span><span class="sxs-lookup"><span data-stu-id="29a3a-166">Next steps</span></span>

<span data-ttu-id="29a3a-167">이제 Azure에서 리소스 액세스를 관리하는 방법을 이해했으므로 이러한 서비스를 사용하여 [간단한 워크로드](governance-simple-workload.md) 또는 [여러 팀](governance-multiple-teams.md)에 대한 거버넌스 모델을 설계하는 방법을 알아봅니다.</span><span class="sxs-lookup"><span data-stu-id="29a3a-167">Now that you understand how to manage resource access in Azure, move on to learn how to design a governance model [for a simple workload](governance-simple-workload.md) or for [multiple teams](governance-multiple-teams.md) using these services.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="29a3a-168">거버넌스에 대한 개요</span><span class="sxs-lookup"><span data-stu-id="29a3a-168">An overview of governance</span></span>](../overview.md)
