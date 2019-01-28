---
title: '엔터프라이즈 클라우드 채택: 단일 워크로드를 위한 거버넌스 디자인'
description: 사용자가 간단한 워크로드를 배포할 수 있도록 Azure 거버넌스 컨트롤을 구성하기 위한 지침
author: petertaylor9999
ms.date: 09/10/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.openlocfilehash: f87584b4182e5eca143f6429220c822b4be2ed70
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54481731"
---
# <a name="enterprise-cloud-adoption-governance-design-for-a-simple-workload"></a><span data-ttu-id="b21bb-103">엔터프라이즈 클라우드 채택: 단일 워크로드를 위한 거버넌스 디자인</span><span class="sxs-lookup"><span data-stu-id="b21bb-103">Enterprise Cloud Adoption: Governance design for a simple workload</span></span>

<span data-ttu-id="b21bb-104">이 지침의 목적은 단일 팀 및 간단한 워크로드를 지원하기 위해 Azure에서 리소스 거버넌스 모델을 디자인하는 프로세스를 알아보는 데 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-104">The goal of this guidance is to help you learn the process for designing a resource governance model in Azure to support a single team and a simple workload.</span></span>  <span data-ttu-id="b21bb-105">가상의 거버넌스 요구 사항 집합을 살펴본 다음, 해당 요구 사항을 충족하는 몇 가지 예제 구현을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-105">We'll look at a set of hypothetical governance requirements, then go through several example implementations that satisfy those requirements.</span></span> 

<span data-ttu-id="b21bb-106">기초 채택 단계에서는 간단한 워크로드를 Azure에 배포하는 것이 목표입니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-106">In the foundational adoption stage, our goal is to deploy a simple workload to Azure.</span></span> <span data-ttu-id="b21bb-107">그러면 다음과 같은 요구 사항이 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-107">This results in the following requirements:</span></span>
* <span data-ttu-id="b21bb-108">간단한 워크로드를 배포하고 유지 관리하는 작업을 담당하는 단일 **워크로드 소유자**의 ID 관리</span><span class="sxs-lookup"><span data-stu-id="b21bb-108">Identity management for a single **workload owner** who is responsible for deploying and maintaining the simple workload.</span></span> <span data-ttu-id="b21bb-109">워크로드 소유자에게는 ID 관리 시스템의 다른 사용자에게 이러한 권한을 위임하는 사용 권한뿐만 아니라 리소스를 만들고, 읽고, 업데이트하고, 삭제할 수 있는 사용 권한이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-109">The workload owner requires permission to create, read, update, and delete resources as well as permission to delegate these rights to other users in the identity management system.</span></span>
* <span data-ttu-id="b21bb-110">간단한 워크로드의 모든 리소스를 단일 관리 단위로 관리합니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-110">Manage all resources for the simple workload as a single management unit.</span></span>

## <a name="licensing-azure"></a><span data-ttu-id="b21bb-111">Azure 라이선싱</span><span class="sxs-lookup"><span data-stu-id="b21bb-111">Licensing Azure</span></span>

<span data-ttu-id="b21bb-112">거버넌스 모델을 디자인하기 전에 Azure의 사용을 허가하는 방법을 이해해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-112">Before we begin designing our governance model, it's important to understand how Azure is licensed.</span></span> <span data-ttu-id="b21bb-113">Azure 라이선스와 연결된 관리자 계정에 모든 Azure 리소스에 대한 가장 높은 수준의 액세스 권한이 있기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-113">This is because the administrative accounts associated with your Azure license have the highest level of access to all of your Azure resources.</span></span> <span data-ttu-id="b21bb-114">이러한 관리 계정은 거버넌스 모델의 기본을 형성합니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-114">These administrative accounts form the basis of your governance model.</span></span>  

> [!NOTE]
> <span data-ttu-id="b21bb-115">조직에 Azure가 포함되지 않은 기존 [Microsoft 기업계약](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx)이 있는 경우 선불 현금 약정 금액을 만들어서 Azure를 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-115">If your organization has an existing [Microsoft Enterprise Agreement](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx) that does not include Azure, Azure can be added by making an upfront monetary commitment.</span></span> <span data-ttu-id="b21bb-116">자세한 내용은 [엔터프라이즈용 Azure 라이선스](https://azure.microsoft.com/pricing/enterprise-agreement/)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="b21bb-116">See [licensing Azure for the enterprise](https://azure.microsoft.com/pricing/enterprise-agreement/) for more information.</span></span> 

<span data-ttu-id="b21bb-117">Azure를 조직의 기업계약에 추가하는 경우 조직에는 **Azure 계정**을 만들라는 메시지가 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-117">When Azure was added to your organization's Enterprise Agreement, your organization was prompted to create an **Azure account**.</span></span> <span data-ttu-id="b21bb-118">계정 생성 프로세스 중에 **Azure 계정 소유자**뿐만 아니라 **전역 관리자** 계정을 사용하는 Azure AD(Azure Active Directory) 테넌트가 생성되었습니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-118">During the account creation process, an **Azure account owner** was created, as well as an Azure Active Directory (Azure AD) tenant with a **global administrator** account.</span></span> <span data-ttu-id="b21bb-119">Azure AD 테넌트는 Azure AD의 안전한 전용 인스턴스를 나타내는 논리적 구문입니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-119">An Azure AD tenant is a logical construct that represents a secure, dedicated instance of Azure AD.</span></span>

<span data-ttu-id="b21bb-120">![Azure 계정 관리자 및 Azure AD 전역 관리자 권한이 있는 Azure 계정](../_images/governance-3-0.png)
*그림 1. Azure 계정 관리자 및 Azure AD 전역 관리자가 있는 Azure 계정.*</span><span class="sxs-lookup"><span data-stu-id="b21bb-120">![Azure account with Azure Account Manager and Azure AD global administrator](../_images/governance-3-0.png)
*Figure 1. An Azure account with an Account Manager and Azure AD Global Administrator.*</span></span>

## <a name="identity-management"></a><span data-ttu-id="b21bb-121">ID 관리</span><span class="sxs-lookup"><span data-stu-id="b21bb-121">Identity management</span></span>

<span data-ttu-id="b21bb-122">Azure에서는 [Azure AD](/azure/active-directory)를 신뢰하여 사용자를 인증하고 사용자에게 리소스에 대한 액세스 권한을 부여합니다. 따라서 Azure AD는 ID 관리 시스템입니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-122">Azure only trusts [Azure AD](/azure/active-directory) to authenticate users and authorize user access to resources, so Azure AD is our identity management system.</span></span> <span data-ttu-id="b21bb-123">Azure AD 전역 관리자에게는 가장 높은 수준의 사용 권한이 있으며 사용자 생성 및 사용 권한 할당을 비롯하여 ID와 관련된 모든 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-123">The Azure AD global administrator has the highest level of permissions and can perform all actions related to identity, including creating users and assigning permissions.</span></span> 

<span data-ttu-id="b21bb-124">요구 사항은 간단한 워크로드를 배포하고 유지 관리하는 작업을 담당하는 단일 **워크로드 소유자**의 ID 관리입니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-124">Our requirement is identity management for a single **workload owner** who is responsible for deploying and maintaining the simple workload.</span></span> <span data-ttu-id="b21bb-125">워크로드 소유자에게는 ID 관리 시스템의 다른 사용자에게 이러한 권한을 위임하는 사용 권한뿐만 아니라 리소스를 만들고, 읽고, 업데이트하고, 삭제할 수 있는 사용 권한이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-125">The workload owner requires permission to create, read, update, and delete resources as well as permission to delegate these rights to other users in the identity management system.</span></span>

<span data-ttu-id="b21bb-126">이 Azure AD 전역 관리자는 **워크로드 소유자**에 대해 **워크로드 소유자** 계정을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-126">Our Azure AD global administrator will create the **workload owner** account for the **workload owner**:</span></span>

<span data-ttu-id="b21bb-127">![Azure AD 전역 관리자는 워크로드 소유자 계정을 만듭니다.](../_images/governance-1-2.png)
*그림 2. Azure AD 전역 관리자는 워크로드 소유 사용자 계정을 만듭니다.*</span><span class="sxs-lookup"><span data-stu-id="b21bb-127">![The Azure AD global administrator creates the workload owner account](../_images/governance-1-2.png)
*Figure 2. The Azure AD global administrator creates the workload owner user account.*</span></span>

<span data-ttu-id="b21bb-128">이 사용자가 **구독**에 추가될 때까지 리소스 액세스 권한을 할당할 수 없습니다. 따라서 다음 두 섹션에서 해당 작업을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-128">We aren't able to assign resource access permission until this user is added to a **subscription**, so we'll do that in the next two sections.</span></span> 

## <a name="resource-management-scope"></a><span data-ttu-id="b21bb-129">리소스 관리 범위</span><span class="sxs-lookup"><span data-stu-id="b21bb-129">Resource management scope</span></span>

<span data-ttu-id="b21bb-130">조직에서 배포한 리소스 수가 증가함에 따라 해당 리소스를 관리하는 복잡성도 증가합니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-130">As the number of resources deployed by your organization grows, the complexity of governing those resources grows as well.</span></span> <span data-ttu-id="b21bb-131">Azure는 논리 컨테이너 계층 구조를 구현하여 조직이 **범위**라고도 하는 다양한 수준의 세분성으로 그룹의 리소스를 관리할 수 있도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-131">Azure implements a logical container hierarchy to enable your organization to manage your resources in groups at various levels of granularity, also known as **scope**.</span></span> 

<span data-ttu-id="b21bb-132">상위 수준의 리소스 관리 범위는 **구독** 수준입니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-132">The top level of resource management scope is the **subscription** level.</span></span> <span data-ttu-id="b21bb-133">구독은 Azure **계정 소유자**에 의해 성성됩니다. 이 사용자는 재정 약정을 설정하고 구독과 연결된 모든 Azure 리소스에 대해 요금을 지불해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-133">A subscription is created by the Azure **account owner**, who establishes the financial commitment and is responsible for paying for all Azure resources associated with the subscription:</span></span>

<span data-ttu-id="b21bb-134">![Azure 계정 소유자가 구독을 만듭니다.](../_images/governance-1-3.png)
*그림 3. Azure 계정 소유자가 구독을 만듭니다.*</span><span class="sxs-lookup"><span data-stu-id="b21bb-134">![The Azure account owner creates a subscription](../_images/governance-1-3.png)
*Figure 3. The Azure account owner creates a subscription.*</span></span>

<span data-ttu-id="b21bb-135">구독을 만들면 Azure **계정 소유자**는 구독과 Azure AD 테넌트를 연결하고 이 Azure AD 테넌트는 사용자를 인증하고 권한을 부여하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-135">When the subscription is created, the Azure **account owner** associates an Azure AD tenant with the subscription, and this Azure AD tenant is used for authenticating and authorizing users:</span></span>

<span data-ttu-id="b21bb-136">![Azure 계정 소유자는 구독과 Azure AD 테넌트를 연결합니다.](../_images/governance-1-4.png)
*그림 4. Azure 계정 소유자는 구독과 Azure AD 테넌트를 연결합니다.*</span><span class="sxs-lookup"><span data-stu-id="b21bb-136">![The Azure account owner associates the Azure AD tenant with the subscription](../_images/governance-1-4.png)
*Figure 4. The Azure account owner associates the Azure AD tenant with the subscription.*</span></span>

<span data-ttu-id="b21bb-137">현재 이 구독과 연결된 사용자가 없다는 것을 알 수 있습니다. 즉, 누구에게도 리소스를 관리할 사용 권한이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-137">You may have noticed that there is currently no user associated with the subscription, which means that no one has permission to manage resources.</span></span> <span data-ttu-id="b21bb-138">실제로 **계정 소유자**는 구독 소유자이며 구독의 리소스에 대해 조치를 취할 수 있는 사용 권한이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-138">In reality, the **account owner** is the owner of the subscription and has permission to take any action on a resource in the subscription.</span></span> <span data-ttu-id="b21bb-139">그러나 실용적인 용어로 **계정 소유자**는 조직의 재무 담당자 이상이며 이러한 리소스를 만들고, 읽고, 업데이트하고, 삭제하는 작업을 담당하지 않습니다. 해당 작업은 **워크로드 소유자**가 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-139">However, in practical terms the **account owner** is more than likely a finance person in your organization and is not responsible for creating, reading, updating, and deleting resources - those tasks will be performed by the **workload owner**.</span></span> <span data-ttu-id="b21bb-140">따라서 **워크로드 소유자**를 구독에 추가하고 권한을 할당해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-140">Therefore, we need to add the **workload owner** to the subscription and assign permissions.</span></span>

<span data-ttu-id="b21bb-141">**계정 소유자**가 현재 구독에 **워크로드 소유자**를 추가할 수 있는 사용 권한을 가진 유일한 사용자이므로 **워크로드 소유자**를 구독에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-141">Since the **account owner** is currently the only user with permission to add the **workload owner** to the subscription, they add the **workload owner** to the subscription:</span></span>

<span data-ttu-id="b21bb-142">![Azure 계정 소유자는 **워크로드 소유자**를 구독에 추가합니다.](../_images/governance-1-5.png)
*그림 5. Azure 계정 소유자는 워크로드 소유자를 구독에 추가합니다.*</span><span class="sxs-lookup"><span data-stu-id="b21bb-142">![The Azure account owner adds the **workload owner** to the subscription](../_images/governance-1-5.png)
*Figure 5. The Azure account owner adds the workload owner to the subscription.*</span></span>

<span data-ttu-id="b21bb-143">Azure **계정 소유자**는 [RBAC(역할 기반 액세스 제어)](/azure/role-based-access-control/) 역할을 할당하여 **워크로드 소유자**에게 사용 권한을 부여합니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-143">The Azure **account owner** grants permissions to the **workload owner** by assigning a [role-based access control (RBAC)](/azure/role-based-access-control/) role.</span></span> <span data-ttu-id="b21bb-144">RBAC 역할은 **워크로드 소유자**에게 개별 리소스 종류 또는 일련의 리소스 종류에 대한 사용 권한 집합을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-144">The RBAC role specifies a set of permissions that the **workload owner** has for an individual resource type or a set of resource types.</span></span>

<span data-ttu-id="b21bb-145">이 예제에서 **계정 소유자**에게는 [기본 제공 **소유자** 역할](/azure/role-based-access-control/built-in-roles#owner)을 부여했습니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-145">Notice that in this example, the **account owner** has assigned the [built-in **owner** role](/azure/role-based-access-control/built-in-roles#owner):</span></span> 

<span data-ttu-id="b21bb-146">![**워크로드 소유자**에게는 기본 제공 소유자 역할이 할당되었습니다.](../_images/governance-1-6.png)
*그림 6. 워크로드 소유자에게는 기본 제공 소유자 역할이 할당되었습니다.*</span><span class="sxs-lookup"><span data-stu-id="b21bb-146">![The **workload owner** was assigned the built-in owner role](../_images/governance-1-6.png)
*Figure 6. The workload owner was assigned the built-in owner role.*</span></span>

<span data-ttu-id="b21bb-147">기본 제공 **소유자** 역할은 구독 범위에서 **워크로드 소유자**에게 모든 사용 권한을 부여합니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-147">The built-in **owner** role grants all permissions to the **workload owner** at the subscription scope.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="b21bb-148">Azure **계정 소유자**는 구독과 연결된 재무 약정을 담당하지만 **워크로드 소유자**에게도 동일한 사용 권한이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-148">The Azure **acount owner** is responsible for the financial committment associated with the subscription, but the **workload owner** has the same permissions.</span></span> <span data-ttu-id="b21bb-149">**계정 소유자**는 **워크로드 소유자**를 신뢰하여 구독 예산 내에 있는 리소스를 배포해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-149">The **account owner** must trust the **workload owner** to deploy resources that are within the subscription budget.</span></span>

<span data-ttu-id="b21bb-150">높은 수준의 관리 범위는 **리소스 그룹** 수준입니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-150">The next level of management scope is the **resource group** level.</span></span> <span data-ttu-id="b21bb-151">리소스 그룹은 리소스의 논리 컨테이너입니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-151">A resource group is a logical container for resources.</span></span> <span data-ttu-id="b21bb-152">리소스 그룹 수준에서 적용된 작업은 그룹의 모든 리소스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-152">Operations applied at the resource group level apply to all resources in a group.</span></span> <span data-ttu-id="b21bb-153">또한 각 사용자에 대한 사용 권한이 해당 범위에서 명시적으로 변경되지 않으면 높은 수준에서 상속됩니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-153">Also, it's important to note that permissions for each user are inherited from the next level up unless they are explicitly changed at that scope.</span></span> 

<span data-ttu-id="b21bb-154">예를 들어 **워크로드 소유자**가 리소스 그룹을 만드는 경우 어떤 상황이 발생하는지를 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-154">To illustrate this, let's look at what happens when the **workload owner** creates a resource group:</span></span>

<span data-ttu-id="b21bb-155">![**워크로드 소유자**는 리소스 그룹을 만듭니다.](../_images/governance-1-7.png)
*그림 7. 워크로드 소유자는 리소스 그룹을 만들고 리소스 그룹 범위에서 기본 제공 소유자 역할을 상속합니다.*</span><span class="sxs-lookup"><span data-stu-id="b21bb-155">![The **workload owner** creates a resource group](../_images/governance-1-7.png)
*Figure 7. The workload owner creates a resource group and inherits the built-in owner role at the resource group scope.*</span></span>

<span data-ttu-id="b21bb-156">다시 기본 제공 **소유자** 역할은 리소스 그룹 범위에서 **워크로드 소유자**에게 모든 사용 권한을 부여합니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-156">Again, the built-in **owner** role grants all permissions to the **workload owner** at the resource group scope.</span></span> <span data-ttu-id="b21bb-157">앞에서 설명한 대로 이 역할은 구독 수준에서 상속됩니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-157">As we discussed earlier, this role is inherited from the subscription level.</span></span> <span data-ttu-id="b21bb-158">이 범위에서 다른 역할이 이 사용자에게 할당된 경우 이 범위에만 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-158">If a different role is assigned to this user at this scope, it applies to this scope only.</span></span>

<span data-ttu-id="b21bb-159">가장 낮은 수준의 관리 범위는 **리소스** 수준입니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-159">The lowest level of management scope is at the **resource** level.</span></span> <span data-ttu-id="b21bb-160">리소스 수준에서 적용된 작업음 리소스 자체에만 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-160">Operations applied at the resource level apply only to the resource itself.</span></span> <span data-ttu-id="b21bb-161">또한 다시 한 번 리소스 수준의 사용 권한은 리소스 그룹 범위에서 상속됩니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-161">And once again, permissions at the resource level are inherited from resource group scope.</span></span> <span data-ttu-id="b21bb-162">예를 들어 **워크로드 소유자**가 [가상 네트워크](/azure/virtual-network/virtual-networks-overview)를 리소스 그룹에 배포한 경우 어떤 상황이 발생하는지를 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-162">For example, let's look at what happens if the **workload owner** deploys a [virtual network](/azure/virtual-network/virtual-networks-overview) into the resource group:</span></span>

<span data-ttu-id="b21bb-163">![**워크로드 소유자**는 리소스를 만듭니다.](../_images/governance-1-8.png)
*그림 8. 워크로드 소유자는 리소스를 만들고 리소스 범위에서 기본 제공 소유자 역할을 상속합니다.*</span><span class="sxs-lookup"><span data-stu-id="b21bb-163">![The **workload owner** creates a resource](../_images/governance-1-8.png)
*Figure 8. The workload owner creates a resource and inherits the built-in owner role at the resource scope.*</span></span>

<span data-ttu-id="b21bb-164">**워크로드 소유자**는 리소스 범위에서 소유자 역할을 상속합니다. 즉, 워크로드 소유자에게는 가상 네트워크에 대한 모든 권한이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-164">The **workload owner** inherits the owner role at the resource scope, which means the workload owner has all permissions for the virtual network.</span></span>

## <a name="implementing-the-basic-resource-access-management-model"></a><span data-ttu-id="b21bb-165">기본 리소스 액세스 관리 모델 구현</span><span class="sxs-lookup"><span data-stu-id="b21bb-165">Implementing the basic resource access management model</span></span>

<span data-ttu-id="b21bb-166">이전에 디자인된 거버넌스 모델을 구현하는 방법을 알아보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-166">Let's move on to learn how to implement the governance model designed earlier.</span></span> 

<span data-ttu-id="b21bb-167">시작하기 위해 조직에는 Azure 계정이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-167">To begin, your organization requires an Azure account.</span></span> <span data-ttu-id="b21bb-168">조직에 Azure가 포함되지 않은 기존 [Microsoft 기업계약](https://www.microsoft.com/licensing/licensing-programs/enterprise.aspx)이 있는 경우 선불 현금 약정 금액을 만들어서 Azure를 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-168">If your organization has an existing [Microsoft Enterprise Agreement](https://www.microsoft.com/licensing/licensing-programs/enterprise.aspx) that does not include Azure, Azure can be added by making an upfront monetary commitment.</span></span> <span data-ttu-id="b21bb-169">자세한 내용은 [엔터프라이즈용 Azure 라이선스](https://azure.microsoft.com/pricing/enterprise-agreement/)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="b21bb-169">See [licensing Azure for the enterprise](https://azure.microsoft.com/pricing/enterprise-agreement/) for more information.</span></span> 

<span data-ttu-id="b21bb-170">Azure 계정의 만들어지면 조직의 사용자를 Azure **계정 소유자**로 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-170">When your Azure account is created, you specify a person in your organization to be the Azure **account owner**.</span></span> <span data-ttu-id="b21bb-171">기본적으로 그런 다음, Azure AD(Azure Active Directory) 테넌트가 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-171">An Azure Active Directory (Azure AD) tenant is then created by default.</span></span> <span data-ttu-id="b21bb-172">Azure **계정 소유자**는 **워크로드 소유자**인 조직의 사용자에 대한 [사용자 계정을 만들](/azure/active-directory/add-users-azure-active-directory)어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-172">Your Azure **account owner** must [create the user account](/azure/active-directory/add-users-azure-active-directory) for the person in your organization who is the **workload owner**.</span></span> 

<span data-ttu-id="b21bb-173">다음으로, Azure **계정 소유자**는 [구독을 만들고](https://docs.microsoft.com/partner-center/create-a-new-subscription) 여기에 [Azure AD 테넌트를 연결](/azure/active-directory/fundamentals/active-directory-how-subscriptions-associated-directory)해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-173">Next, your Azure **account owner** must [create a subscription](https://docs.microsoft.com/partner-center/create-a-new-subscription) and [associate the Azure AD tenant](/azure/active-directory/fundamentals/active-directory-how-subscriptions-associated-directory) with it.</span></span>

<span data-ttu-id="b21bb-174">[이제 구독을 만들고 Azure AD 테넌트와 연결했으므로 마지막으로, 기본 제공 **소유자** 역할](/azure/billing/billing-add-change-azure-subscription-administrator#add-an-rbac-owner-for-a-subscription-in-azure-portal)을 포함하는 구독에 **워크로드 소유자**를 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b21bb-174">Finally, now that the subscription is created and your Azure AD tenant is associated with it, you can [add the **workload owner** to the subscription with the built-in **owner** role](/azure/billing/billing-add-change-azure-subscription-administrator#add-an-rbac-owner-for-a-subscription-in-azure-portal).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b21bb-175">다음 단계</span><span class="sxs-lookup"><span data-stu-id="b21bb-175">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="b21bb-176">Azure에 기본 워크로드 배포</span><span class="sxs-lookup"><span data-stu-id="b21bb-176">Deploy a basic workload to Azure</span></span>](../infrastructure/basic-workload.md)
> [!div class="nextstepaction"]
> [<span data-ttu-id="b21bb-177">여러 팀에 대한 리소스 액세스 알아보기</span><span class="sxs-lookup"><span data-stu-id="b21bb-177">Learn about resource access for multiple teams</span></span>](governance-multiple-teams.md)
