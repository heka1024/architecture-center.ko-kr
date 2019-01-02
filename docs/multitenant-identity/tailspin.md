---
title: Tailspin 설문 조사 애플리케이션 정보
description: Tailspin 설문 조사 애플리케이션 개요
author: MikeWasson
ms.date: 07/21/2017
pnp.series.title: Manage Identity in Multitenant Applications
pnp.series.prev: index
pnp.series.next: authenticate
ms.openlocfilehash: a1c357bd1b5306d1255c66aaea96d86be55e7b77
ms.sourcegitcommit: e7e0e0282fa93f0063da3b57128ade395a9c1ef9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52902071"
---
# <a name="the-tailspin-scenario"></a><span data-ttu-id="dcd19-103">Tailspin 시나리오</span><span class="sxs-lookup"><span data-stu-id="dcd19-103">The Tailspin scenario</span></span>

<span data-ttu-id="dcd19-104">[![GitHub](../_images/github.png) 샘플 코드][sample application]</span><span class="sxs-lookup"><span data-stu-id="dcd19-104">[![GitHub](../_images/github.png) Sample code][sample application]</span></span>

<span data-ttu-id="dcd19-105">Tailspin은 설문 조사라는 SaaS 애플리케이션을 개발하는 가상 회사입니다.</span><span class="sxs-lookup"><span data-stu-id="dcd19-105">Tailspin is a fictitious company that is developing a SaaS application named Surveys.</span></span> <span data-ttu-id="dcd19-106">이 애플리케이션을 사용하면 조직에서 온라인 설문 조사를 만들어 게시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dcd19-106">This application enables organizations to create and publish online surveys.</span></span>

* <span data-ttu-id="dcd19-107">조직은 애플리케이션을 등록할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dcd19-107">An organization can sign up for the application.</span></span>
* <span data-ttu-id="dcd19-108">조직이 등록한 후 사용자는 조직 자격 증명으로 애플리케이션에 로그인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dcd19-108">After the organization is signed up, users can sign into the application with their organizational credentials.</span></span>
* <span data-ttu-id="dcd19-109">사용자는 설문 조사를 만들고, 편집하고, 게시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dcd19-109">Users can create, edit, and publish surveys.</span></span>

> [!NOTE]
> <span data-ttu-id="dcd19-110">애플리케이션을 시작하려면 [설문 조사 애플리케이션 실행]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="dcd19-110">To get started with the application, see [Run the Surveys application].</span></span>
> 
> 

## <a name="users-can-create-edit-and-view-surveys"></a><span data-ttu-id="dcd19-111">사용자는 설문 조사를 만들고, 편집하고, 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dcd19-111">Users can create, edit, and view surveys</span></span>
<span data-ttu-id="dcd19-112">인증된 사용자는 자신이 만들었거나 참가자 권한이 있는 모든 설문 조사를 볼 수 있고, 새 설문 조사를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dcd19-112">An authenticated user can view all the surveys that he or she has created or has contributor rights to, and create new surveys.</span></span> <span data-ttu-id="dcd19-113">사용자가 조직 ID, `bob@contoso.com`을 사용하여 로그인합니다.</span><span class="sxs-lookup"><span data-stu-id="dcd19-113">Notice that the user is signed in with his organizational identity, `bob@contoso.com`.</span></span>

![설문 조사 앱](./images/surveys-screenshot.png)

<span data-ttu-id="dcd19-115">이 스크린샷은 설문 조사 편집 페이지를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="dcd19-115">This screenshot shows the Edit Survey page:</span></span>

![설문 조사 편집](./images/edit-survey.png)

<span data-ttu-id="dcd19-117">또한 사용자는 동일한 테넌트 내에서 다른 사용자가 만든 설문 조사를 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dcd19-117">Users can also view any surveys created by other users within the same tenant.</span></span>

![테넌트 설문 조사](./images/tenant-surveys.png)

## <a name="survey-owners-can-invite-contributors"></a><span data-ttu-id="dcd19-119">설문 조사 소유자는 참가자를 초대할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dcd19-119">Survey owners can invite contributors</span></span>
<span data-ttu-id="dcd19-120">사용자는 설문 조사를 만들 때 다른 사람을 설문 조사의 참가자로 초대할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dcd19-120">When a user creates a survey, he or she can invite other people to be contributors on the survey.</span></span> <span data-ttu-id="dcd19-121">참가자는 설문 조사를 편집할 수 있으나 삭제하거나 게시할 수는 없습니다.</span><span class="sxs-lookup"><span data-stu-id="dcd19-121">Contributors can edit the survey, but cannot delete or publish it.</span></span>  

![참여자 추가](./images/add-contributor.png)

<span data-ttu-id="dcd19-123">사용자는 다른 테넌트에서 참가자를 추가하여 테넌트 간에 리소스를 공유할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dcd19-123">A user can add contributors from other tenants, which enables cross-tenant sharing of resources.</span></span> <span data-ttu-id="dcd19-124">이 스크린샷에서 Bob(`bob@contoso.com`)은 자신이 만든 설문 조사에 참가자로 Alice(`alice@fabrikam.com`)를 추가하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dcd19-124">In this screenshot, Bob (`bob@contoso.com`) is adding Alice (`alice@fabrikam.com`) as a contributor to a survey that Bob created.</span></span>

<span data-ttu-id="dcd19-125">Alice가 로그인할 때 "Surveys I can contribute to" 아래에 나열된 설문 조사를 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="dcd19-125">When Alice logs in, she sees the survey listed under "Surveys I can contribute to".</span></span>

![설문 조사 참가자](./images/contributor.png)

<span data-ttu-id="dcd19-127">Alice는 Contoso 테넌트의 게스트가 아니라 자신의 테넌트로 로그인합니다.</span><span class="sxs-lookup"><span data-stu-id="dcd19-127">Note that Alice signs into her own tenant, not as a guest of the Contoso tenant.</span></span> <span data-ttu-id="dcd19-128">Alice는 해당 설문 조사에 대해서만 참가자 권한을 가지며 Contoso 테넌트의 다른 설문 조사는 볼 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="dcd19-128">Alice has contributor permissions only for that survey &mdash; she cannot view other surveys from the Contoso tenant.</span></span>

## <a name="architecture"></a><span data-ttu-id="dcd19-129">아키텍처</span><span class="sxs-lookup"><span data-stu-id="dcd19-129">Architecture</span></span>
<span data-ttu-id="dcd19-130">설문 조사 애플리케이션은 웹 프런트 엔드 및 Web API 백 엔드로 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="dcd19-130">The Surveys application consists of a web front end and a web API backend.</span></span> <span data-ttu-id="dcd19-131">둘 다 [ASP.NET Core]를 사용하여 구현됩니다.</span><span class="sxs-lookup"><span data-stu-id="dcd19-131">Both are implemented using [ASP.NET Core].</span></span>

<span data-ttu-id="dcd19-132">웹 애플리케이션은 Azure AD(Azure Active Directory)를 사용하여 사용자를 인증합니다.</span><span class="sxs-lookup"><span data-stu-id="dcd19-132">The web application uses Azure Active Directory (Azure AD) to authenticate users.</span></span> <span data-ttu-id="dcd19-133">또한 웹 애플리케이션은 Azure AD를 호출하여 Web API에 대한 OAuth 2 액세스 토큰을 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="dcd19-133">The web application also calls Azure AD to get OAuth 2 access tokens for the Web API.</span></span> <span data-ttu-id="dcd19-134">액세스 토큰은 Azure Redis Cache에 캐시됩니다.</span><span class="sxs-lookup"><span data-stu-id="dcd19-134">Access tokens are cached in Azure Redis Cache.</span></span> <span data-ttu-id="dcd19-135">캐시는 여러 인스턴스가 동일한 토큰 캐시를 공유할 수 있도록 해줍니다(예: 서버 팜에서).</span><span class="sxs-lookup"><span data-stu-id="dcd19-135">The cache enables multiple instances to share the same token cache (e.g., in a server farm).</span></span>

![아키텍처](./images/architecture.png)

<span data-ttu-id="dcd19-137">[**다음**][authentication]</span><span class="sxs-lookup"><span data-stu-id="dcd19-137">[**Next**][authentication]</span></span>

<!-- Links -->

[authentication]: authenticate.md

[설문 조사 애플리케이션 실행]: ./run-the-app.md
[Run the Surveys application]: ./run-the-app.md
[ASP.NET Core]: /aspnet/core
[sample application]: https://github.com/mspnp/multitenant-saas-guidance
