---
title: 서버리스 웹 응용 프로그램
description: 서버리스 웹 응용 프로그램 및 웹 API를 보여 주는 참조 아키텍처입니다.
author: MikeWasson
ms.date: 10/16/2018
ms.openlocfilehash: d1af03811bda6267fd40ee17823ac8357829f988
ms.sourcegitcommit: 949b9d3e5a9cdee1051e6be700ed169113e914ae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2018
ms.locfileid: "50983399"
---
# <a name="serverless-web-application"></a><span data-ttu-id="81e73-103">서버리스 웹 응용 프로그램</span><span class="sxs-lookup"><span data-stu-id="81e73-103">Serverless web application</span></span> 

<span data-ttu-id="81e73-104">이 참조 아키텍처에서는 서버리스 웹 응용 프로그램을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-104">This reference architecture shows a serverless web application.</span></span> <span data-ttu-id="81e73-105">이 응용 프로그램은 Azure Blob Storage의 정적 콘텐츠를 제공하고 Azure Functions를 사용하여 API를 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-105">The application serves static content from Azure Blob Storage, and implements an API using Azure Functions.</span></span> <span data-ttu-id="81e73-106">API는 Cosmos DB에서 데이터를 읽고 결과를 웹앱에 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-106">The API reads data from Cosmos DB and returns the results to the web app.</span></span> <span data-ttu-id="81e73-107">이 아키텍처에 대한 참조 구현은 [GitHub][github]에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-107">A reference implementation for this architecture is available on [GitHub][github].</span></span>

![](./_images/serverless-web-app.png)
 
<span data-ttu-id="81e73-108">'서버리스'라는 용어에는 별도의 관련된 두 가지 의미가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-108">The term serverless has two distinct but related meanings:</span></span>

- <span data-ttu-id="81e73-109">**BaaS(Backend as a Service)**.</span><span class="sxs-lookup"><span data-stu-id="81e73-109">**Backend as a service** (BaaS).</span></span> <span data-ttu-id="81e73-110">데이터베이스 및 저장소와 같은 백 엔드 클라우드 서비스는 클라이언트 응용 프로그램에서 이러한 서비스에 직접 연결할 수 있게 하는 API를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-110">Backend cloud services, such as databases and storage, provide APIs that enable client applications to connect directly to these services.</span></span> 
- <span data-ttu-id="81e73-111">**FaaS(Functions as a Service)**.</span><span class="sxs-lookup"><span data-stu-id="81e73-111">**Functions as a service** (FaaS).</span></span> <span data-ttu-id="81e73-112">이 모델에서 "함수"는 클라우드에 배포되고, 코드를 실행하는 서버를 완전히 추상화하는 호스팅 환경 내에서 실행되는 코드 조각입니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-112">In this model, a "function" is a piece of code that is deployed to the cloud and runs inside a hosting environment that completely abstracts the servers that run the code.</span></span> 

<span data-ttu-id="81e73-113">두 정의 모두에는 개발자와 DevOps 직원이 서버를 배포, 구성 또는 관리할 필요가 없다는 일반적인 개념이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-113">Both definitions have in common the idea that developers and DevOps personnel don't need to deploy, configure, or manage servers.</span></span> <span data-ttu-id="81e73-114">이 참조 아키텍처는 Azure Functions를 사용하는 FaaS에 중점을 두고 있지만, Azure Blob Storage의 웹 콘텐츠를 제공하는 BaaS의 예입니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-114">This reference architecture focuses on FaaS using Azure Functions, although serving web content from Azure Blob Storage is an example of BaaS.</span></span> <span data-ttu-id="81e73-115">FaaS에 대한 몇 가지 중요한 특징은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-115">Some important characteristics of FaaS are:</span></span>

1. <span data-ttu-id="81e73-116">계산 리소스는 필요에 따라 플랫폼별로 동적으로 할당됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-116">Compute resources are allocated dynamically as needed by the platform.</span></span>
1. <span data-ttu-id="81e73-117">사용량 기반 가격 책정: 코드를 실행하는 데 사용된 계산 리소스에 대한 요금만 부과됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-117">Consumption-based pricing: You are charged only for the compute resources used to execute your code.</span></span>
1. <span data-ttu-id="81e73-118">계산 리소스는 트래픽 기반 요청에 따라 크기 조정되며, 개발자가 구성할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-118">The compute resources scale on demand based on traffic, without the developer needing to do any configuration.</span></span>

<span data-ttu-id="81e73-119">Functions는 HTTP 요청 또는 큐에 도착하는 메시지와 같은 외부 트리거가 발생하면 실행됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-119">Functions are executed when an external trigger occurs, such as an HTTP request or a message arriving on a queue.</span></span> <span data-ttu-id="81e73-120">그러면 서버리스 아키텍처에서 [이벤트 구동 아키텍처 스타일][event-driven]이 자연스럽게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-120">This makes an [event-driven architecture style][event-driven] natural for serverless architectures.</span></span> <span data-ttu-id="81e73-121">아키텍처에서 구성 요소 간의 작업을 조정하려면 메시지 broker 또는 pub/sub 패턴을 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-121">To coordinate work between components in the architecture, consider using message brokers or pub/sub patterns.</span></span> <span data-ttu-id="81e73-122">Azure에서 메시지 기술을 선택하는 데 도움이 필요하면[메시지를 배달하는 Azure 서비스 중에서 선택][azure-messaging]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="81e73-122">For help choosing between messaging technologies in Azure, see [Choose between Azure services that deliver messages][azure-messaging].</span></span>

## <a name="architecture"></a><span data-ttu-id="81e73-123">아키텍처</span><span class="sxs-lookup"><span data-stu-id="81e73-123">Architecture</span></span>
<span data-ttu-id="81e73-124">이 아키텍처는 다음 구성 요소로 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-124">The architecture consists of the following components.</span></span>

<span data-ttu-id="81e73-125">**Blob Storage**</span><span class="sxs-lookup"><span data-stu-id="81e73-125">**Blob Storage**.</span></span> <span data-ttu-id="81e73-126">HTML, CSS 및 JavaScript 파일과 같은 정적 웹 콘텐츠는 Azure Blob Storage에 저장되며, [정적 웹 사이트 호스팅][static-hosting]을 사용하여 클라이언트에 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-126">Static web content, such as HTML, CSS, and JavaScript files, are stored in Azure Blob Storage and served to clients by using [static website hosting][static-hosting].</span></span> <span data-ttu-id="81e73-127">모든 동적 상호 작용은 백 엔드 API를 호출하는 JavaScript 코드를 통해 수행됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-127">All dynamic interaction happens through JavaScript code making calls to the backend APIs.</span></span> <span data-ttu-id="81e73-128">웹 페이지를 렌더링하는 서버 쪽 코드는 없습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-128">There is no server-side code to render the web page.</span></span> <span data-ttu-id="81e73-129">정적 웹 사이트 호스팅은 인덱스 문서 및 404 사용자 지정 오류 페이지를 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-129">Static website hosting supports index documents and custom 404 error pages.</span></span>

> [!NOTE]
> <span data-ttu-id="81e73-130">정적 웹 사이트 호스팅은 현재 [미리 보기][static-hosting-preview]로 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-130">Static website hosting is currently in [preview][static-hosting-preview].</span></span>

<span data-ttu-id="81e73-131">**CDN**.</span><span class="sxs-lookup"><span data-stu-id="81e73-131">**CDN**.</span></span> <span data-ttu-id="81e73-132">HTTPS 엔드포인트를 제공할 뿐만 아니라 대기 시간을 줄이고 콘텐츠를 빠르게 전송하기 위해 콘텐츠를 캐시하려면 [Azure CDN(Content Delivery Network)][cdn]을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-132">Use [Azure Content Delivery Network][cdn] (CDN) to cache content for lower latency and faster delivery of content, as well as providing an HTTPS endpoint.</span></span>

<span data-ttu-id="81e73-133">**함수 앱**.</span><span class="sxs-lookup"><span data-stu-id="81e73-133">**Function Apps**.</span></span> <span data-ttu-id="81e73-134">[Azure Functions][functions]는 서버리스 계산 옵션입니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-134">[Azure Functions][functions] is a serverless compute option.</span></span> <span data-ttu-id="81e73-135">트리거를 통해 코드("함수")가 호출되는 이벤트 구동 모델을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-135">It uses an event-driven model, where a piece of code (a "function") is invoked by a trigger.</span></span> <span data-ttu-id="81e73-136">이 아키텍처에서 함수는 클라이언트에서 HTTP를 요청할 때 호출됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-136">In this architecture, the function is invoked when a client makes an HTTP request.</span></span> <span data-ttu-id="81e73-137">요청은 항상 아래에 설명된 API 게이트웨이를 통해 라우팅됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-137">The request is always routed through an API gateway, described below.</span></span>

<span data-ttu-id="81e73-138">**API Management**.</span><span class="sxs-lookup"><span data-stu-id="81e73-138">**API Management**.</span></span> <span data-ttu-id="81e73-139">[API Management][apim]는 HTTP 함수 앞에 있는 API 게이트웨이를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-139">[API Management][apim] provides a API gateway that sits in front of the HTTP function.</span></span> <span data-ttu-id="81e73-140">API Management를 사용하여 클라이언트 응용 프로그램에서 사용하는 API를 게시하고 관리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-140">You can use API Management to publish and manage APIs used by client applications.</span></span> <span data-ttu-id="81e73-141">게이트웨이를 사용하면 프런트 엔드 응용 프로그램을 백 엔드 API에서 분리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-141">Using a gateway helps to decouple the front-end application from the back-end APIs.</span></span> <span data-ttu-id="81e73-142">예를 들어 API Management는 URL을 다시 쓰거나, 백 엔드에 도달하기 전에 요청을 변환하거나, 요청 또는 응답 헤더를 설정하는 등의 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-142">For example, API Management can rewrite URLs, transform requests before they reach the backend, set request or response headers, and so forth.</span></span>

<span data-ttu-id="81e73-143">API Management는 다음과 같은 교차 편집 문제를 구현하는 데에도 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-143">API Management can also be used to implement cross-cutting concerns such as:</span></span>

- <span data-ttu-id="81e73-144">사용 할당량 및 속도 제한 적용</span><span class="sxs-lookup"><span data-stu-id="81e73-144">Enforcing usage quotas and rate limits</span></span>
- <span data-ttu-id="81e73-145">인증을 위한 OAuth 토큰 유효성 검사</span><span class="sxs-lookup"><span data-stu-id="81e73-145">Validating OAuth tokens for authentication</span></span>
- <span data-ttu-id="81e73-146">CORS(원본 간 요청) 사용</span><span class="sxs-lookup"><span data-stu-id="81e73-146">Enabling cross-origin requests (CORS)</span></span>
- <span data-ttu-id="81e73-147">응답 캐싱</span><span class="sxs-lookup"><span data-stu-id="81e73-147">Caching responses</span></span>
- <span data-ttu-id="81e73-148">요청 모니터링 및 로깅</span><span class="sxs-lookup"><span data-stu-id="81e73-148">Monitoring and logging requests</span></span>  

<span data-ttu-id="81e73-149">API Management에서 제공하는 기능 중 일부만 필요한 경우 다른 옵션으로 [Functions 프록시][functions-proxy]를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-149">If you don't need all of the functionality provided by API Management, another option is to use [Functions Proxies][functions-proxy].</span></span> <span data-ttu-id="81e73-150">Azure Functions의 이 기능을 사용하면 백 엔드 함수에 대한 경로를 만들어 여러 함수 앱에 대해 단일 API 화면을 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-150">This feature of Azure Functions lets you define a single API surface for multiple function apps, by creating routes to back-end functions.</span></span> <span data-ttu-id="81e73-151">Function 프록시는 HTTP 요청 및 응답에 대해 제한된 변환을 수행할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-151">Function proxies can also perform limited transformations on the HTTP request and response.</span></span> <span data-ttu-id="81e73-152">그러나 API Management의 다양한 정책 기반 기능을 동일하게 제공하지는 않습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-152">However, they don't provide the same rich policy-based capabilities of API Management.</span></span>

<span data-ttu-id="81e73-153">**Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="81e73-153">**Cosmos DB**.</span></span> <span data-ttu-id="81e73-154">[Cosmos DB][cosmosdb]는 다중 모델 데이터베이스 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-154">[Cosmos DB][cosmosdb] is a multi-model database  service.</span></span> <span data-ttu-id="81e73-155">이 시나리오의 경우 함수 응용 프로그램은 클라이언트의 HTTP GET 요청에 대한 응답으로 Cosmos DB에서 문서를 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-155">For this scenario, the function application fetches documents from Cosmos DB in response to HTTP GET requests from the client.</span></span>

<span data-ttu-id="81e73-156">**Azure AD(Azure Active Directory)**.</span><span class="sxs-lookup"><span data-stu-id="81e73-156">**Azure Active Directory** (Azure AD).</span></span> <span data-ttu-id="81e73-157">사용자는 Azure AD 자격 증명을 사용하여 웹 응용 프로그램에 로그인합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-157">Users sign into the web application by using their Azure AD credentials.</span></span> <span data-ttu-id="81e73-158">Azure AD는 웹 응용 프로그램에서 API 요청을 인증하는 데 사용하는 API에 대한 액세스 토큰을 반환합니다([인증](#authentication) 참조).</span><span class="sxs-lookup"><span data-stu-id="81e73-158">Azure AD returns an access token for the API, which the web application uses to authenticate API requests (see [Authentication](#authentication)).</span></span>

<span data-ttu-id="81e73-159">**Azure Monitor**</span><span class="sxs-lookup"><span data-stu-id="81e73-159">**Azure Monitor**.</span></span> <span data-ttu-id="81e73-160">[Monitor][monitor]는 솔루션에 배포된 Azure 서비스에 대한 성능 메트릭을 수집합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-160">[Monitor][monitor] collects performance metrics about the Azure services deployed in the solution.</span></span> <span data-ttu-id="81e73-161">이러한 메트릭을 대시보드에서 시각화하여 솔루션의 상태를 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-161">By visualizing these in a dashboard, you can get visibility into the health of the solution.</span></span> <span data-ttu-id="81e73-162">또한 응용 프로그램 로그도 수집합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-162">It also collected application logs.</span></span>

<span data-ttu-id="81e73-163">**Azure Pipelines**.</span><span class="sxs-lookup"><span data-stu-id="81e73-163">**Azure Pipelines**.</span></span> <span data-ttu-id="81e73-164">[Pipelines][pipelines]는 응용 프로그램을 빌드, 테스트 및 배포하는 CI(지속적인 통합) 및 CD(지속적인 업데이트) 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-164">[Pipelines][pipelines] is a continuous integration (CI) and continuous delivery (CD) service that builds, tests, and deploys the application.</span></span>

## <a name="recommendations"></a><span data-ttu-id="81e73-165">권장 사항</span><span class="sxs-lookup"><span data-stu-id="81e73-165">Recommendations</span></span>

### <a name="function-app-plans"></a><span data-ttu-id="81e73-166">함수 앱 계획</span><span class="sxs-lookup"><span data-stu-id="81e73-166">Function App plans</span></span>

<span data-ttu-id="81e73-167">Azure Functions는 두 가지 호스팅 모델을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-167">Azure Functions supports two hosting models.</span></span> <span data-ttu-id="81e73-168">**사용 계획**을 사용하면 코드가 실행될 때 계산 성능이 자동으로 할당됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-168">With the **consumption plan**, compute power is automatically allocated when your code is running.</span></span>  <span data-ttu-id="81e73-169">**App Service** 계획을 사용하면 코드에 일단의 VM이 할당됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-169">With the **App Service** plan, a set of VMs are allocated for your code.</span></span> <span data-ttu-id="81e73-170">App Service 계획은 VM의 수와 크기를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-170">The App Service plan defines the number of VMs and the VM size.</span></span> 

<span data-ttu-id="81e73-171">위에서 설명한 정의에 따라 App Service 계획은 엄격히 *서버리스*가 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-171">Note that the App Service plan is not strictly *serverless*, according to the definition given above.</span></span> <span data-ttu-id="81e73-172">프로그래밍 모델은 동일하지만, 동일한 함수 코드가 사용 계획과 App Service 계획 모두에서 실행될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-172">The programming model is the same, however &mdash; the same function code can run in both a consumption plan and an App Service plan.</span></span>

<span data-ttu-id="81e73-173">사용할 계획 유형을 선택할 때 고려해야 할 몇 가지 요소는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-173">Here are some factors to consider when choosing which type of plan to use:</span></span>

- <span data-ttu-id="81e73-174">**콜드 부팅**.</span><span class="sxs-lookup"><span data-stu-id="81e73-174">**Cold start**.</span></span> <span data-ttu-id="81e73-175">사용 계획에서 최근에 호출되지 않은 함수는 다음에 실행될 때 약간의 대기 시간이 추가로 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-175">With the consumption plan, a function that hasn't been invoked recently will incur some additional latency the next time it runs.</span></span> <span data-ttu-id="81e73-176">이 추가 대기 시간은 런타임 환경을 할당하고 준비하기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-176">This additional latency is due to allocating and preparing the runtime environment.</span></span> <span data-ttu-id="81e73-177">일반적으로 초 단위이지만 로드해야 하는 종속성 수를 포함하여 몇 가지 요인에 따라 달라집니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-177">It is usually on the order of seconds but depends on several factors, including the number of dependencies that need to be loaded.</span></span> <span data-ttu-id="81e73-178">자세한 내용은 [서버리스 콜드 부팅 이해][functions-cold-start]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="81e73-178">For more information, see [Understanding Serverless Cold Start][functions-cold-start].</span></span> <span data-ttu-id="81e73-179">추가 대기 시간은 사용자가 직접 관찰하므로 콜드 부팅은 일반적으로 비동기 메시지 구동 워크로드(큐 또는 이벤트 허브 트리거)보다 대화형 워크로드(HTTP 트리거)에서 더 큰 문제가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-179">Cold start is usually more of a concern for interactive workloads (HTTP triggers) than asynchronous message-driven workloads (queue or event hubs triggers), because the additional latency is directly observed by users.</span></span>
- <span data-ttu-id="81e73-180">**제한 시간**.</span><span class="sxs-lookup"><span data-stu-id="81e73-180">**Timeout period**.</span></span>  <span data-ttu-id="81e73-181">사용 계획에서 [구성 가능한][functions-timeout] 기간(최대 10분) 후에는 함수 실행 시간이 초과됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-181">In the consumption plan, a function execution times out after a [configurable][functions-timeout] period of time (to a maximum of 10 minutes)</span></span>
- <span data-ttu-id="81e73-182">**가상 네트워크 격리**.</span><span class="sxs-lookup"><span data-stu-id="81e73-182">**Virtual network isolation**.</span></span> <span data-ttu-id="81e73-183">App Service 계획을 사용하면 격리된 전용 호스팅 환경인 [App Service Environment][ase] 내에서 함수가 실행될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-183">Using an App Service plan allows functions to run inside of an [App Service Environment][ase], which is a dedicated and isolated hosting environment.</span></span>
- <span data-ttu-id="81e73-184">**가격 책정 모델**.</span><span class="sxs-lookup"><span data-stu-id="81e73-184">**Pricing model**.</span></span> <span data-ttu-id="81e73-185">사용 계획은 실행 횟수와 리소스 사용량 따라 청구됩니다(메모리 &times; 실행 시간)에.</span><span class="sxs-lookup"><span data-stu-id="81e73-185">The consumption plan is billed by the number of executions and resource consumption (memory &times; execution time).</span></span> <span data-ttu-id="81e73-186">App Service 계획은 VM 인스턴스 SKU에 따라 시간당 기준으로 청구됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-186">The App Service plan is billed hourly based on VM instance SKU.</span></span> <span data-ttu-id="81e73-187">사용하는 계산 리소스에 대해서만 비용을 지불하므로 사용 계획이 App Service 계획보다 저렴한 경우가 많습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-187">Often, the consumption plan can be cheaper than an App Service plan, because you pay only for the compute resources that you use.</span></span> <span data-ttu-id="81e73-188">트래픽이 최고점 및 최저점에 있는 경우 특히 그렇습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-188">This is especially true if your traffic experiences peaks and troughs.</span></span> <span data-ttu-id="81e73-189">그러나 응용 프로그램에서 지속적으로 대량의 처리량이 발생하는 경우 App Service 계획이 사용 계획보다 비용이 저렴할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-189">However, if an application experiences constant high-volume throughput, an App Service plan may cost less than the consumption plan.</span></span>
- <span data-ttu-id="81e73-190">**크기 조정**.</span><span class="sxs-lookup"><span data-stu-id="81e73-190">**Scaling**.</span></span> <span data-ttu-id="81e73-191">소비 모델의 큰 장점은 들어오는 트래픽을 기반으로 하여 필요에 따라 동적으로 크기가 조정된다는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-191">A big advantage of the consumption model is that it scales dynamically as needed, based on the incoming traffic.</span></span> <span data-ttu-id="81e73-192">이 크기 조정은 빠르게 수행되지만 램프 업(ramp-up) 기간이 여전히 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-192">While this scaling occurs quickly, there is still a ramp-up period.</span></span> <span data-ttu-id="81e73-193">일부 워크로드의 경우 VM을 의도적으로 과도하게 프로비전하여 램프 업 시간 없이 트래픽 급증을 처리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-193">For some workloads, you might want to deliberately overprovision the VMs, so that you can handle bursts of traffic with zero ramp-up time.</span></span> <span data-ttu-id="81e73-194">이 경우 App Service 계획을 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-194">In that case, consider an App Service plan.</span></span>

### <a name="function-app-boundaries"></a><span data-ttu-id="81e73-195">함수 앱 경계</span><span class="sxs-lookup"><span data-stu-id="81e73-195">Function App boundaries</span></span>

<span data-ttu-id="81e73-196">*함수 앱*은 실행할 하나 이상의 *함수*를 호스팅합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-196">A *function app* hosts the execution of one or more *functions*.</span></span> <span data-ttu-id="81e73-197">함수 앱을 사용하여 여러 함수를 논리적 단위로 그룹화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-197">You can use a function app to group several functions together as a logical unit.</span></span> <span data-ttu-id="81e73-198">함수 앱 내에서 함수는 동일한 응용 프로그램 설정, 호스팅 계획 및 배포 수명 주기를 공유합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-198">Within a function app, the functions share the same application settings, hosting plan, and deployment lifecycle.</span></span> <span data-ttu-id="81e73-199">각 함수 앱에는 자체의 호스트 이름이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-199">Each function app has its own hostname.</span></span>  

<span data-ttu-id="81e73-200">함수 앱을 사용하여 동일한 수명 주기와 설정을 공유하는 함수를 그룹화합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-200">Use function apps to group functions that share the same lifecycle and settings.</span></span> <span data-ttu-id="81e73-201">동일한 수명 주기를 공유하지 않는 함수는 다른 함수 앱에서 호스팅해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-201">Functions that don't share the same lifecycle should be hosted in different function apps.</span></span> 

<span data-ttu-id="81e73-202">마이크로 서비스 접근 방식을 사용하는 것이 좋습니다. 여기서는 각 함수 앱이 하나의 마이크로 서비스를 나타내며, 몇 가지 관련된 함수로 구성될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-202">Consider taking a microservices approach, where each function app represents one microservice, possibly consisting of several related functions.</span></span> <span data-ttu-id="81e73-203">마이크로 서비스 아키텍처의 경우 서비스에는 느슨한 결합과 높은 기능적 응집력이 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-203">In a microservices architecture, services should have loose coupling and high functional cohesion.</span></span> <span data-ttu-id="81e73-204">*느슨한 결합*은 다른 서비스를 동시에 업데이트하지 않고도 하나의 서비스를 변경할 수 있다는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-204">*Loosely* coupled means you can change one service without requiring other services to be updated at the same time.</span></span> <span data-ttu-id="81e73-205">*응집력*은 서비스에 잘 정의된 단일 용도가 있다는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-205">*Cohesive* means a service has a single, well-defined purpose.</span></span> <span data-ttu-id="81e73-206">이러한 개념에 대한 자세한 내용은 [마이크로 서비스 설계: 도메인 분석][microservices-domain-analysis]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="81e73-206">For more discussion of these ideas, see [Designing microservices: Domain analysis][microservices-domain-analysis].</span></span>

### <a name="function-bindings"></a><span data-ttu-id="81e73-207">Functions 바인딩</span><span class="sxs-lookup"><span data-stu-id="81e73-207">Function bindings</span></span>

<span data-ttu-id="81e73-208">가능한 경우 Functions [바인딩][functions-bindings]을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-208">Use Functions [bindings][functions-bindings] when possible.</span></span> <span data-ttu-id="81e73-209">바인딩은 코드를 데이터에 연결하고 다른 Azure 서비스와 통합하는 선언적 방법을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-209">Bindings provide a declarative way to connect your code to data and integrate with other Azure services.</span></span> <span data-ttu-id="81e73-210">입력 바인딩은 외부 데이터 원본의 입력 매개 변수를 채웁니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-210">An input binding populates an input parameter from an external data source.</span></span> <span data-ttu-id="81e73-211">출력 바인딩은 함수의 반환 값을 큐 또는 데이터베이스와 같은 데이터 싱크로 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-211">An output binding sends the function's return value to a data sink, such as a queue or database.</span></span>

<span data-ttu-id="81e73-212">예를 들어 참조 구현의 `GetStatus` 함수는 Cosmos DB [입력 바인딩][cosmosdb-input-binding]을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-212">For example, the `GetStatus` function in the reference implementation uses the Cosmos DB [input binding][cosmosdb-input-binding].</span></span> <span data-ttu-id="81e73-213">다음 바인딩은 HTTP 요청의 쿼리 문자열에서 가져온 쿼리 매개 변수를 사용하여 Cosmos DB에서 문서를 조회하도록 구성되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-213">This binding is configured to look up a document in Cosmos DB, using query parameters that are taken from the query string in the HTTP request.</span></span> <span data-ttu-id="81e73-214">문서가 있으면 매개 변수로 함수에 전달됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-214">If the document is found, it is passed to the function as a parameter.</span></span>

```csharp
[FunctionName("GetStatusFunction")]
public static Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", Route = null)] HttpRequest req, 
    [CosmosDB(
        databaseName: "%COSMOSDB_DATABASE_NAME%",
        collectionName: "%COSMOSDB_DATABASE_COL%",
        ConnectionStringSetting = "COSMOSDB_CONNECTION_STRING",
        Id = "{Query.deviceId}",
        PartitionKey = "{Query.deviceId}")] dynamic deviceStatus, 
    ILogger log)
{
    ...
}
```

<span data-ttu-id="81e73-215">바인딩을 사용하면 서비스와 직접 통신하는 코드를 작성할 필요가 없으므로 함수 코드가 더 간단해지고 데이터 원본 또는 싱크의 세부 정보가 추상화됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-215">By using bindings, you don't need to write code that talks directly to the service, which makes the function code simpler and also abstracts the details of the data source or sink.</span></span> <span data-ttu-id="81e73-216">그러나 경우에 따라 바인딩에서 제공하는 것보다 더 복잡한 논리가 필요할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-216">In some cases, however, you may need more complex logic than the binding provides.</span></span> <span data-ttu-id="81e73-217">이 경우 Azure 클라이언트 SDK를 직접 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-217">In that case, use the Azure client SDKs directly.</span></span>

## <a name="scalability-considerations"></a><span data-ttu-id="81e73-218">확장성 고려 사항</span><span class="sxs-lookup"><span data-stu-id="81e73-218">Scalability considerations</span></span>

<span data-ttu-id="81e73-219">**Functions**.</span><span class="sxs-lookup"><span data-stu-id="81e73-219">**Functions**.</span></span> <span data-ttu-id="81e73-220">사용 계획의 경우 HTTP 트리거는 트래픽에 따라 크기 조정됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-220">For the consumption plan, the HTTP trigger scales based on the traffic.</span></span> <span data-ttu-id="81e73-221">동시 함수 인스턴스의 수는 제한되지만, 각 인스턴스는 한 번에 둘 이상의 요청을 처리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-221">There is a limit to the number of concurrent function instances, but each instance can process more than one request at a time.</span></span> <span data-ttu-id="81e73-222">App Service 계획의 경우 HTTP 트리거는 VM 인스턴스의 수에 따라 크기 조정됩니다. 즉 VM 인스턴스의 수가 고정된 값이거나 자동 크기 조정 규칙 집합을 기반으로 하여 자동으로 크기 조정될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-222">For an App Service plan, the HTTP trigger scales according to the number of VM instances, which can be a fixed value or can autoscale based on a set of autoscaling rules.</span></span> <span data-ttu-id="81e73-223">자세한 내용은 [Azure Functions 크기 조정 및 호스팅][functions-scale]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="81e73-223">For information, see [Azure Functions scale and hosting][functions-scale].</span></span> 

<span data-ttu-id="81e73-224">**Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="81e73-224">**Cosmos DB**.</span></span> <span data-ttu-id="81e73-225">Cosmos DB의 처리 용량은 [RU(요청 단위)][ru]로 측정됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-225">Throughput capacity for Cosmos DB is measured in [Request Units][ru] (RU).</span></span> <span data-ttu-id="81e73-226">1RU 처리량은 1KB 문서를 가져오는 데 필요한 GET 처리량에 해당합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-226">A 1-RU throughput corresponds to the throughput need to GET a 1KB document.</span></span> <span data-ttu-id="81e73-227">10,000RU를 초과하는 Cosmos DB 컨테이너의 크기를 조정하려면 컨테이너를 만들 때 [파티션 키][partition-key]를 지정하고, 만드는 모든 문서에 파티션 키가 포함되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-227">In order to scale a Cosmos DB container past 10,000 RU, you must specify a [partition key][partition-key] when you create the container and include the partition key in every document that you create.</span></span> <span data-ttu-id="81e73-228">파티션 키에 대한 자세한 내용은 [Azure Cosmos DB의 파티션 및 확장][cosmosdb-scale]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="81e73-228">For more information about partition keys, see [Partition and scale in Azure Cosmos DB][cosmosdb-scale].</span></span>

<span data-ttu-id="81e73-229">**API Management**.</span><span class="sxs-lookup"><span data-stu-id="81e73-229">**API Management**.</span></span> <span data-ttu-id="81e73-230">API Management는 규모를 확장하고 규칙 기반 자동 크기 조정을 지원할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-230">API Management can scale out and supports rule-based autoscaling.</span></span> <span data-ttu-id="81e73-231">크기 조정 프로세스에는 20분 이상이 걸립니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-231">Note that the scaling process takes at least 20 minutes.</span></span> <span data-ttu-id="81e73-232">트래픽이 급증하면 예상되는 최대 버스트 트래픽을 제공해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-232">If your traffic is bursty, you should provision for the maximum burst traffic that you expect.</span></span> <span data-ttu-id="81e73-233">그러나 자동 크기 조정은 시간별 또는 일별 트래픽 변동을 처리하는 데 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-233">However, autoscaling is useful for handling hourly or daily variations in traffic.</span></span> <span data-ttu-id="81e73-234">자세한 내용은 [Azure API Management 인스턴스자동 크기 조정][apim-scale]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="81e73-234">For more information, see [Automatically scale an Azure API Management instance][apim-scale].</span></span>

## <a name="disaster-recovery-considerations"></a><span data-ttu-id="81e73-235">재해 복구 고려 사항</span><span class="sxs-lookup"><span data-stu-id="81e73-235">Disaster recovery considerations</span></span>

<span data-ttu-id="81e73-236">여기에 표시된 배포는 단일 Azure 지역에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-236">The deployment shown here resides in a single Azure region.</span></span> <span data-ttu-id="81e73-237">재해 복구에 더 탄력적인 방법을 적용하려면 다양한 서비스의 지리적 배포 기능을 활용합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-237">For a more resilient approach to disaster-recovery, take advantage of the geo-distribution features in the various services:</span></span>

- <span data-ttu-id="81e73-238">API Management는 여러 Azure 지역에 걸쳐 단일 API Management 인스턴스를 배포하는 데 사용할 수 있는 다중 지역 배포를 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-238">API Management supports multi-region deployment, which can be used to distribute a single API Management instance across any number of Azure regions.</span></span> <span data-ttu-id="81e73-239">자세한 내용은 [여러 Azure 지역에 Azure API Management 서비스 인스턴스를 배포하는 방법][api-geo]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="81e73-239">For more information, see [How to deploy an Azure API Management service instance to multiple Azure regions][api-geo].</span></span>

- <span data-ttu-id="81e73-240">[Traffic Manager][tm]를 사용하여 HTTP 요청을 주 지역으로 라우팅합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-240">Use [Traffic Manager][tm] to route HTTP requests to the primary region.</span></span> <span data-ttu-id="81e73-241">해당 지역에서 실행하는 함수 앱을 사용할 수 없게 되면 Traffic Manager에서 보조 지역으로 조치 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-241">If the Function App running in that region becomes unavailable, Traffic Manager can fail over to a secondary region.</span></span>

- <span data-ttu-id="81e73-242">Cosmos DB는 [다중 마스터 지역][cosmosdb-geo]을 지원하므로 Cosmos DB 계정에 추가하는 모든 지역에 쓸 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-242">Cosmos DB supports [multiple master regions][cosmosdb-geo], which enables writes to any region that you add to your Cosmos DB account.</span></span> <span data-ttu-id="81e73-243">다중 마스터를 사용하도록 설정하지 않은 경우에도 주 쓰기 지역을 장애 조치할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-243">If you don't enable multi-master, you can still fail over the primary write region.</span></span> <span data-ttu-id="81e73-244">Cosmos DB 클라이언트 SDK 및 Azure Function 바인딩은 장애 조치를 자동으로 처리하므로 응용 프로그램 구성 설정을 업데이트할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-244">The Cosmos DB client SDKs and the Azure Function bindings automatically handle the failover, so you don't need to update any application configuration settings.</span></span>

## <a name="security-considerations"></a><span data-ttu-id="81e73-245">보안 고려 사항</span><span class="sxs-lookup"><span data-stu-id="81e73-245">Security considerations</span></span>

### <a name="authentication"></a><span data-ttu-id="81e73-246">인증</span><span class="sxs-lookup"><span data-stu-id="81e73-246">Authentication</span></span>

<span data-ttu-id="81e73-247">참조 구현의 `GetStatus` API는 Azure AD를 사용하여 요청을 인증합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-247">The `GetStatus` API in the reference implementation uses Azure AD to authenticate requests.</span></span> <span data-ttu-id="81e73-248">Azure AD는 OAuth 2 프로토콜에 기반한 인증 프로토콜인 Open ID Connect 프로토콜을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-248">Azure AD supports the Open ID Connect protocol, which is an authentication protocol built on top of the OAuth 2 protocol.</span></span>

<span data-ttu-id="81e73-249">이 아키텍처에서 클라이언트 응용 프로그램은 브라우저에서 실행되는 SPA(단일 페이지 응용 프로그램)입니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-249">In this architecture, the client application is a single-page application (SPA) that runs in the browser.</span></span> <span data-ttu-id="81e73-250">이 유형의 클라이언트 응용 프로그램은 클라이언트 비밀 또는 권한 부여 코드를 숨길 수 없으므로 암시적 허용 흐름이 적절합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-250">This type of client application cannot keep a client secret or an authorization code hidden, so the implicit grant flow is appropriate.</span></span> <span data-ttu-id="81e73-251">[어떤 OAuth 2.0 흐름을 사용해야 합니까?][oauth-flow]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="81e73-251">(See [Which OAuth 2.0 flow should I use?][oauth-flow]).</span></span> <span data-ttu-id="81e73-252">전체 흐름은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-252">Here's the overall flow:</span></span>

1. <span data-ttu-id="81e73-253">사용자가 웹 응용 프로그램에서 "로그인" 링크를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-253">The user clicks the "Sign in" link in the web application.</span></span>
1. <span data-ttu-id="81e73-254">브라우저가 Azure AD 로그인 페이지로 리디렉션됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-254">The browser is redirected the Azure AD sign in page.</span></span> 
1. <span data-ttu-id="81e73-255">사용자가 로그인합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-255">The user signs in.</span></span>
1. <span data-ttu-id="81e73-256">Azure AD에서 URL 조각의 액세스 토큰을 포함하여 클라이언트 응용 프로그램으로 다시 리디렉션합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-256">Azure AD redirects back to the client application, including an access token in the URL fragment.</span></span>
1. <span data-ttu-id="81e73-257">웹 응용 프로그램에서 API를 호출하면 인증 헤더에 액세스 토큰이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-257">When the web application calls the API, it includes the access token in the Authentication header.</span></span> <span data-ttu-id="81e73-258">응용 프로그램 ID는 액세스 토큰의 대상 그룹('aud') 클레임으로 보내집니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-258">The application ID is sent as the audience ('aud') claim in the access token.</span></span> 
1. <span data-ttu-id="81e73-259">백 엔드 API에서 액세스 토큰의 유효성을 검사합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-259">The backend API validates the access token.</span></span>

<span data-ttu-id="81e73-260">인증을 구성하려면</span><span class="sxs-lookup"><span data-stu-id="81e73-260">To configure authentication:</span></span>

- <span data-ttu-id="81e73-261">Azure AD 테넌트에 응용 프로그램을 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-261">Register an application in your Azure AD tenant.</span></span> <span data-ttu-id="81e73-262">그러면 클라이언트에서 로그인 URL에 포함하는 응용 프로그램 ID가 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-262">This generates an application ID, which the client includes with the login URL.</span></span>

- <span data-ttu-id="81e73-263">함수 앱 내에서 Azure AD 인증을 사용하도록 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-263">Enable Azure AD authentication inside the Function App.</span></span> <span data-ttu-id="81e73-264">자세한 내용은 [Azure App Service의 인증 및 권한 부여][app-service-auth]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="81e73-264">For more information, see [Authentication and authorization in Azure App Service][app-service-auth].</span></span>

- <span data-ttu-id="81e73-265">액세스 토큰의 유효성을 검사하여 요청을 사전 승인하는 [validate-jwt policy][apim-validate-jwt] 정책을 API Management에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-265">Add the [validate-jwt policy][apim-validate-jwt] to API Management to pre-authorize the request by validating the access token.</span></span>

<span data-ttu-id="81e73-266">자세한 내용은 [GitHub 추가 정보][readme]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="81e73-266">For more details, see the [GitHub readme][readme].</span></span>

<span data-ttu-id="81e73-267">클라이언트 응용 프로그램 및 백 엔드 API의 경우 Azure AD에서 별도 앱 등록을 만드는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-267">It's recommended to create separate app registrations in Azure AD for the client application and the backend API.</span></span> <span data-ttu-id="81e73-268">클라이언트 응용 프로그램에 API를 호출하는 권한을 부여합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-268">Grant the client application permission to call the API.</span></span> <span data-ttu-id="81e73-269">이 방법은 여러 API 및 클라이언트를 정의하고 각각에 대한 사용 권한을 제어할 유연성을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-269">This approach gives you the flexibility to define multiple APIs and clients and control the permissions for each.</span></span> 

<span data-ttu-id="81e73-270">API 내에서 [범위][scopes]를 사용하여 응용 프로그램이 사용자로부터 요청한 사용 권한을 세부적으로 제어할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-270">Within an API, use [scopes][scopes] to give applications fine-grained control over what permissions they request from a user.</span></span> <span data-ttu-id="81e73-271">예를 들어 API에는 `Read` 및 `Write` 범위가 포함될 수 있고, 특정 클라이언트 앱은 `Read` 권한만을 부여하도록 사용자에게 요청할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-271">For example, an API might have `Read` and `Write` scopes, and a particular client app might ask the user to authorize `Read` permissions only.</span></span>

### <a name="authorization"></a><span data-ttu-id="81e73-272">권한 부여</span><span class="sxs-lookup"><span data-stu-id="81e73-272">Authorization</span></span>

<span data-ttu-id="81e73-273">많은 응용 프로그램에서 백 엔드 API는 사용자에게 지정된 작업을 수행할 권한이 있는지 여부를 확인해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-273">In many applications, the backend API must check whether a user has permission to perform a given action.</span></span> <span data-ttu-id="81e73-274">[클레임 기반 권한 부여][claims]를 사용하는 것이 좋습니다. 여기서 사용자에 대한 정보는 ID 공급자(이 경우 Azure AD)에 전달되고 권한 부여를 결정하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-274">It's recommended to use [claims-based authorization][claims], where information about the user is conveyed by the identity provider (in this case, Azure AD) and used to make authorization decisions.</span></span> 

<span data-ttu-id="81e73-275">일부 클레임은 Azure AD에서 클라이언트에 반환하는 ID 토큰 내에 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-275">Some claims are provided inside the ID token that Azure AD returns to the client.</span></span> <span data-ttu-id="81e73-276">요청의 X-MS-CLIENT-PRINCIPAL 헤더를 검사하여 함수 앱 내에서 이러한 클레임을 가져올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-276">You can get these claims from within the function app by examining the X-MS-CLIENT-PRINCIPAL header in the request.</span></span> <span data-ttu-id="81e73-277">다른 클레임의 경우 [Microsoft Graph][graph]를 사용하여 Azure AD를 쿼리합니다(로그인 중에 사용자 동의 필요).</span><span class="sxs-lookup"><span data-stu-id="81e73-277">For other claims, use [Microsoft Graph][graph] to query Azure AD (requires user consent during sign-in).</span></span> 

<span data-ttu-id="81e73-278">예를 들어 Azure AD에서 응용 프로그램을 등록할 때 응용 프로그램 역할 집합을 응용 프로그램의 등록 매니페스트에 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-278">For example, when you register an application in Azure AD, you can define a set of application roles in the application's registration manifest.</span></span> <span data-ttu-id="81e73-279">사용자가 응용 프로그램에 로그인하면 Azure AD에는 사용자가 부여한 각 역할에 대한 "역할" 클레임이 포함됩니다(그룹 멤버 자격을 통해 상속된 역할 포함).</span><span class="sxs-lookup"><span data-stu-id="81e73-279">When a user signs into the application, Azure AD includes a "roles" claim for each role that the user has been granted (including roles that are inherited through group membership).</span></span> 

<span data-ttu-id="81e73-280">참조 구현에서 함수는 인증된 사용자가 `GetStatus` 응용 프로그램 역할의 멤버인지 여부를 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-280">In the reference implementation, the function checks whether the authenticated user is a member of the `GetStatus` application role.</span></span> <span data-ttu-id="81e73-281">그렇지 않은 경우 함수에서 HTTP 권한 없음(401) 응답을 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-281">If not, the function returns an HTTP Unauthorized (401) response.</span></span> 

```csharp
[FunctionName("GetStatusFunction")]
public static Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, 
    [CosmosDB(
        databaseName: "%COSMOSDB_DATABASE_NAME%",
        collectionName: "%COSMOSDB_DATABASE_COL%",
        ConnectionStringSetting = "COSMOSDB_CONNECTION_STRING",
        Id = "{Query.deviceId}",
        PartitionKey = "{Query.deviceId}")] dynamic deviceStatus, 
    ILogger log)
{
    log.LogInformation("Processing GetStatus request.");

    return req.HandleIfAuthorizedForRoles(new[] { GetDeviceStatusRoleName },
        async () =>
        {
            string deviceId = req.Query["deviceId"];
            if (deviceId == null)
            {
                return new BadRequestObjectResult("Missing DeviceId");
            }

            return await Task.FromResult<IActionResult>(deviceStatus != null
                    ? (ActionResult)new OkObjectResult(deviceStatus)
                    : new NotFoundResult());
        },
        log);
}
```

<span data-ttu-id="81e73-282">이 코드 예제에서 `HandleIfAuthorizedForRoles`는 역할 클레임을 확인하고, 해당 클레임이 없는 경우 HTTP 401을 반환하는 확장 메서드입니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-282">In this code example, `HandleIfAuthorizedForRoles` is an extension method that checks for the role claim and returns HTTP 401 if the claim isn't found.</span></span> <span data-ttu-id="81e73-283">소스 코드는 [여기][HttpRequestAuthorizationExtensions]서 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-283">You can find the source code [here][HttpRequestAuthorizationExtensions].</span></span> <span data-ttu-id="81e73-284">`HandleIfAuthorizedForRoles`에는 `ILogger` 매개 변수가 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-284">Notice that `HandleIfAuthorizedForRoles` takes an `ILogger` parameter.</span></span> <span data-ttu-id="81e73-285">감사 추적을 수행하고 필요에 따라 문제를 진단할 수 있도록 권한이 없는 요청을 기록해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-285">You should log unauthorized requests so that you have an audit trail and can diagnose issues if needed.</span></span> <span data-ttu-id="81e73-286">동시에 HTTP 401 응답 내에 자세한 정보가 누출되지 않도록 방지합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-286">At the same time, avoid leaking any detailed information inside the HTTP 401 response.</span></span>

### <a name="cors"></a><span data-ttu-id="81e73-287">CORS</span><span class="sxs-lookup"><span data-stu-id="81e73-287">CORS</span></span>

<span data-ttu-id="81e73-288">이 참조 아키텍처에서 웹 응용 프로그램과 API는 동일한 원본을 공유하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-288">In this reference architecture, the web application and the API do not share the same origin.</span></span> <span data-ttu-id="81e73-289">즉 응용 프로그램이 API를 호출할 때의 원본 간 요청입니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-289">That means when the application calls the API, it is a cross-origin request.</span></span> <span data-ttu-id="81e73-290">브라우저 보안은 웹 페이지에서 다른 도메인으로 AJAX 요청을 수행하지 못하도록 방지합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-290">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="81e73-291">이 제한은 *동일 원본 정책*이라고 하며, 악성 사이트에서 다른 사이트의 중요한 데이터를 읽지 못하도록 차단합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-291">This restriction is called the *same-origin policy* and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="81e73-292">원본 간 요청을 사용하려면 CORS(원본 간 요청) [정책][cors-policy]을 API Management 게이트웨이에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-292">To enable a cross-origin request, add a Cross-Origin Resource Sharing (CORS) [policy][cors-policy] to the API Management gateway:</span></span>

```xml
<cors allow-credentials="true">
    <allowed-origins>
        <origin>[Website URL]</origin>
    </allowed-origins>
    <allowed-methods>
        <method>GET</method>
    </allowed-methods>
    <allowed-headers>
        <header>*</header>
    </allowed-headers>
</cors>
```

<span data-ttu-id="81e73-293">이 예제에서 **allow-credentials** 특성은 **true**입니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-293">In this example, the **allow-credentials** attribute is **true**.</span></span> <span data-ttu-id="81e73-294">그러면 요청과 함께 자격 증명(쿠키 포함)을 보낼 수 있는 권한을 브라우저에 부여합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-294">This authorizes the browser to send credentials (including cookies) with the request.</span></span> <span data-ttu-id="81e73-295">그렇지 않으면 브라우저에서 기본적으로 원본 간 요청을 사용하여 자격 증명을 보내지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-295">Otherwise, by default the browser does not send credentials with a cross-origin request.</span></span>

> [!NOTE] 
> <span data-ttu-id="81e73-296">**allow-credentials**는 매우 신중하게 **true**로 설정해야 합니다. 이렇게 하면 사용자가 알지 못하는 사이에 웹 사이트에서 해당 사용자를 대신하여 사용자의 자격 증명을 API로 보낼 수 있기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-296">Be very careful about setting **allow-credentials** to **true**, because it means a website can send the user's credentials to your API on the user's behalf, without the user being aware.</span></span> <span data-ttu-id="81e73-297">허용된 원본을 신뢰해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-297">You must trust the allowed origin.</span></span>

### <a name="enforce-https"></a><span data-ttu-id="81e73-298">HTTPS 적용</span><span class="sxs-lookup"><span data-stu-id="81e73-298">Enforce HTTPS</span></span>

<span data-ttu-id="81e73-299">보안을 극대화하려면 요청 파이프라인 전체에 HTTPS가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-299">For maximum security, require HTTPS throughout the request pipeline:</span></span>

- <span data-ttu-id="81e73-300">**CDN**.</span><span class="sxs-lookup"><span data-stu-id="81e73-300">**CDN**.</span></span> <span data-ttu-id="81e73-301">Azure CDN은 기본적으로 `*.azureedge.net` 하위 도메인에서 HTTPS를 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-301">Azure CDN supports HTTPS on the `*.azureedge.net` subdomain by default.</span></span> <span data-ttu-id="81e73-302">사용자 지정 도메인 이름에 대해 CDN에서 HTTPS를 사용하도록 설정하려면 [자습서: Azure CDN 사용자 지정 도메인에서 HTTPS 구성][cdn-https]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="81e73-302">To enable HTTPS in the CDN for custom domain names, see [Tutorial: Configure HTTPS on an Azure CDN custom domain][cdn-https].</span></span> 

- <span data-ttu-id="81e73-303">**정적 웹 사이트 호스팅**.</span><span class="sxs-lookup"><span data-stu-id="81e73-303">**Static website hosting**.</span></span> <span data-ttu-id="81e73-304">저장소 계정에서 "[보안 전송 필요][storage-https]" 옵션을 사용하도록 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-304">Enable the "[Secure transfer required][storage-https]" option on the Storage account.</span></span> <span data-ttu-id="81e73-305">이 옵션을 사용하도록 설정하면 저장소 계정에서 보안 HTTPS 연결의 요청만 허용합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-305">When this option is enabled, the storage account only allows requests from secure HTTPS connections.</span></span> 

- <span data-ttu-id="81e73-306">**API Management**.</span><span class="sxs-lookup"><span data-stu-id="81e73-306">**API Management**.</span></span> <span data-ttu-id="81e73-307">HTTPS 프로토콜만 사용하도록 API를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-307">Configure the APIs to use HTTPS protocol only.</span></span> <span data-ttu-id="81e73-308">Azure Portal 또는 Resource Manager 템플릿을 통해 이를 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-308">You can configure this in the Azure portal or through a Resource Manager template:</span></span>

    ```json
    {
        "apiVersion": "2018-01-01",
        "type": "apis",
        "name": "dronedeliveryapi",
        "dependsOn": [
            "[concat('Microsoft.ApiManagement/service/', variables('apiManagementServiceName'))]"
        ],
        "properties": {
            "displayName": "Drone Delivery API",
            "description": "Drone Delivery API",
            "path": "api",
            "protocols": [ "HTTPS" ]
        },
        ...
    }
    ```

- <span data-ttu-id="81e73-309">**Azure Functions**.</span><span class="sxs-lookup"><span data-stu-id="81e73-309">**Azure Functions**.</span></span> <span data-ttu-id="81e73-310">"[HTTPS만][functions-https]" 설정을 사용하도록 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-310">Enable the "[HTTPS Only][functions-https]" setting.</span></span> 

### <a name="lock-down-the-function-app"></a><span data-ttu-id="81e73-311">함수 앱 잠금</span><span class="sxs-lookup"><span data-stu-id="81e73-311">Lock down the function app</span></span>

<span data-ttu-id="81e73-312">모든 함수 호출은 API 게이트웨이를 통해 이동해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-312">All calls to the function should go through the API gateway.</span></span> <span data-ttu-id="81e73-313">이 작업은 다음과 같이 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-313">You can achieve this as follows:</span></span>

- <span data-ttu-id="81e73-314">함수 키를 요구하도록 함수 앱을 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-314">Configure the function app to require a function key.</span></span> <span data-ttu-id="81e73-315">함수 앱을 호출하면 API Management 게이트웨이에 함수 키가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-315">The API Management gateway will include the function key when it calls the function app.</span></span> <span data-ttu-id="81e73-316">이렇게 하면 클라이언트에서 게이트웨이를 무시하여 직접 함수를 호출하지 못하도록 차단합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-316">This prevents clients from calling the function directly, bypassing the gateway.</span></span> 

- <span data-ttu-id="81e73-317">API Management 게이트웨이에는 [고정 IP 주소][apim-ip]가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-317">The API Management gateway has a [static IP address][apim-ip].</span></span> <span data-ttu-id="81e73-318">Azure Function을 제한하여 고정 IP 주소의 호출만 허용합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-318">Restrict the Azure Function to allow only calls from that static IP address.</span></span> <span data-ttu-id="81e73-319">자세한 내용은 [Azure App Service 고정 IP 제한][app-service-ip-restrictions]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="81e73-319">For more information, see [Azure App Service Static IP Restrictions][app-service-ip-restrictions].</span></span> <span data-ttu-id="81e73-320">이 기능은 표준 계층 서비스에서만 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-320">(This feature is available for Standard tier services only.)</span></span> 

### <a name="protect-application-secrets"></a><span data-ttu-id="81e73-321">응용 프로그램 비밀 보호</span><span class="sxs-lookup"><span data-stu-id="81e73-321">Protect application secrets</span></span>

<span data-ttu-id="81e73-322">응용 프로그램 비밀(예: 데이터베이스 자격 증명)은 코드 또는 구성 파일에 저장하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-322">Don't store application secrets, such as database credentials, in your code or configuration files.</span></span> <span data-ttu-id="81e73-323">대신, Azure에서 암호화되어 저장되는 [앱 설정]을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-323">Instead, use App settings, which are stored encrypted in Azure.</span></span> <span data-ttu-id="81e73-324">자세한 내용은 [Azure App Service 및 Azure Functions의 보안][app-service-security]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="81e73-324">For more information, see [Security in Azure App Service and Azure Functions][app-service-security].</span></span>

<span data-ttu-id="81e73-325">또는 Key Vault에 응용 프로그램 비밀을 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-325">Alternatively, you can store application secrets in Key Vault.</span></span> <span data-ttu-id="81e73-326">이렇게 하면 비밀 저장소를 중앙 집중화하고, 해당 배포를 제어하며, 비밀에 액세스하는 방법과 시기를 모니터링할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-326">This allows you to centralize the storage of secrets, control their distribution, and monitor how and when secrets are being accessed.</span></span> <span data-ttu-id="81e73-327">자세한 내용은 [자습서: Key Vault에서 비밀을 읽도록 Azure 웹 응용 프로그램 구성][key-vault-web-app]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="81e73-327">For more information, see [Configure an Azure web application to read a secret from Key Vault][key-vault-web-app].</span></span> <span data-ttu-id="81e73-328">그러나 Functions 트리거와 바인딩은 앱 설정에서 해당 구성 설정을 로드합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-328">However, note that Functions triggers and bindings load their configuration settings from app settings.</span></span> <span data-ttu-id="81e73-329">Key Vault 비밀을 사용하도록 트리거와 바인딩을 구성하는 기본 제공 방법은 없습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-329">There is no built-in way to configure the triggers and bindings to use Key Vault secrets.</span></span>

## <a name="devops-considerations"></a><span data-ttu-id="81e73-330">DevOps 고려 사항</span><span class="sxs-lookup"><span data-stu-id="81e73-330">DevOps considerations</span></span>

### <a name="deployment"></a><span data-ttu-id="81e73-331">배포</span><span class="sxs-lookup"><span data-stu-id="81e73-331">Deployment</span></span>

<span data-ttu-id="81e73-332">함수 앱을 배포하려면 [패키지 파일][functions-run-from-package]("패키지에서 실행")을 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-332">To deploy the function app, we recommend using [package files][functions-run-from-package] ("Run from package").</span></span> <span data-ttu-id="81e73-333">이 방법을 사용하면 Blob Storage 컨테이너에 zip 파일을 업로드하고, Functions 런타임은 읽기 전용 파일 시스템으로 zip 파일을 탑재합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-333">Using this approach, you upload a zip file to a Blob Storage container and the Functions runtime mounts the zip file as a read-only file system.</span></span> <span data-ttu-id="81e73-334">실패한 배포로 인해 응용 프로그램이 일관성이 없는 상태로 남겨질 가능성을 줄일 수 있는 원자성 조작입니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-334">This is an atomic operation, which reduces the chance that a failed deployment will leave the application in an inconsistent state.</span></span> <span data-ttu-id="81e73-335">한 번에 모든 파일이 교환되기 때문에 특히 Node.js 앱의 경우 콜드 시작 시간을 개선할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-335">It can also improve cold start times, especially for Node.js apps, because all of the files are swapped at once.</span></span>

### <a name="api-versioning"></a><span data-ttu-id="81e73-336">API 버전 관리</span><span class="sxs-lookup"><span data-stu-id="81e73-336">API versioning</span></span>

<span data-ttu-id="81e73-337">API는 서비스와 클라이언트 간의 계약입니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-337">An API is a contract between a service and clients.</span></span> <span data-ttu-id="81e73-338">이 아키텍처에서 API 계약은 API Management 계층에서 정의됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-338">In this architecture, the API contract is defined at the API Management layer.</span></span> <span data-ttu-id="81e73-339">API Management는 각각 별개이지만 상호 보완 관계인 두 가지 [버전 관리 개념][apim-versioning]을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-339">API Management supports two distinct but complementary [versioning concepts][apim-versioning]:</span></span>

- <span data-ttu-id="81e73-340">*버전*을 사용하면 API 소비자가 요구 사항에 따라 API 버전을 선택할 수 있습니다(예: v1 및 v2).</span><span class="sxs-lookup"><span data-stu-id="81e73-340">*Versions* allow API consumers to choose an API version based on their needs, such as v1 versus v2.</span></span> 

- <span data-ttu-id="81e73-341">*수정 버전*을 통해 API 관리자는 API에서 호환성이 손상되지 않는 변경 작업을 수행하고 변경 내용을 배포할 수 있으며, 변경 로그를 통해 API 소비자에게 변경 관련 정보를 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-341">*Revisions* allow API administrators to make non-breaking changes in an API and deploy those changes, along with a change log to inform API consumers about the changes.</span></span>

<span data-ttu-id="81e73-342">API에서 호환성이 손상되는 변경을 수행하는 경우 API Management에서 새 버전을 게시합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-342">If you make a breaking change in an API, publish a new version in API Management.</span></span> <span data-ttu-id="81e73-343">새 버전은 원래 버전과 함께 별도의 함수 앱에 배포됩니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-343">Deploy the new version side-by-side with the original version, in a separate Function App.</span></span> <span data-ttu-id="81e73-344">이렇게 하면 클라이언트 응용 프로그램을 중단하지 않고 기존 클라이언트를 새 API로 마이그레이션할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-344">This lets you migrate existing clients to the new API without breaking client applications.</span></span> <span data-ttu-id="81e73-345">결국에는 이전 버전을 더 이상 사용하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-345">Eventually, you can deprecate the previous version.</span></span> <span data-ttu-id="81e73-346">API Management는 URL 경로, HTTP 헤더 또는 쿼리 문자열과 같은 여러 [버전 관리 체계][apim-versioning-schemes]를 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-346">API Management supports several [versioning schemes][apim-versioning-schemes]: URL path, HTTP header, or query string.</span></span> <span data-ttu-id="81e73-347">일반적인 API 버전 관리에 대한 자세한 내용은 [RESTful 웹 API 버전 관리][api-versioning]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="81e73-347">For more information about API versioning in general, see [Versioning a RESTful web API][api-versioning].</span></span>

<span data-ttu-id="81e73-348">API 변경을 차단하지 않는 업데이트의 경우 동일한 함수 앱의 스테이징 슬롯에 새 버전을 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-348">For updates that are not breaking API changes, deploy the new version to a staging slot in the same Function App.</span></span> <span data-ttu-id="81e73-349">배포가 성공했는지 확인한 다음, 준비된 버전을 프로덕션 버전으로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-349">Verify the deployment succeeded and then swap the staged version with the production version.</span></span> <span data-ttu-id="81e73-350">API Management에서 수정 버전을 게시합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-350">Publish a revision in API Management.</span></span>

## <a name="deploy-the-solution"></a><span data-ttu-id="81e73-351">솔루션 배포</span><span class="sxs-lookup"><span data-stu-id="81e73-351">Deploy the solution</span></span>

<span data-ttu-id="81e73-352">이 참조 아키텍처를 배포하기 위해 [GitHub 추가 정보][readme]를 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="81e73-352">To deploy this reference architecture, view the [GitHub readme][readme].</span></span> 

<!-- links -->

[api-versioning]: ../../best-practices/api-design.md#versioning-a-restful-web-api
[apim]: /azure/api-management/api-management-key-concepts
[apim-ip]: /azure/api-management/api-management-faq#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules
[api-geo]: /azure/api-management/api-management-howto-deploy-multi-region
[apim-scale]: /azure/api-management/api-management-howto-autoscale
[apim-validate-jwt]: /azure/api-management/api-management-access-restriction-policies#ValidateJWT
[apim-versioning]: /azure/api-management/api-management-get-started-publish-versions
[apim-versioning-schemes]: /azure/api-management/api-management-get-started-publish-versions#choose-a-versioning-scheme
[app-service-auth]: /azure/app-service/app-service-authentication-overview
[app-service-ip-restrictions]: /azure/app-service/app-service-ip-restrictions
[app-service-security]: /azure/app-service/app-service-security
[ase]: /azure/app-service/environment/intro
[azure-messaging]: /azure/event-grid/compare-messaging-services
[claims]: https://en.wikipedia.org/wiki/Claims-based_identity
[cdn]: https://azure.microsoft.com/services/cdn/
[cdn-https]: /azure/cdn/cdn-custom-ssl
[cors-policy]: /azure/api-management/api-management-cross-domain-policies
[cosmosdb]: /azure/cosmos-db/introduction
[cosmosdb-geo]: /azure/cosmos-db/distribute-data-globally
[cosmosdb-input-binding]: /azure/azure-functions/functions-bindings-cosmosdb-v2#input
[cosmosdb-scale]: /azure/cosmos-db/partition-data
[event-driven]: ../../guide/architecture-styles/event-driven.md
[functions]: /azure/azure-functions/functions-overview
[functions-bindings]: /azure/azure-functions/functions-triggers-bindings
[functions-cold-start]: https://blogs.msdn.microsoft.com/appserviceteam/2018/02/07/understanding-serverless-cold-start/
[functions-https]: /azure/app-service/app-service-web-tutorial-custom-ssl#enforce-https
[functions-proxy]: /azure/azure-functions/functions-proxies
[functions-run-from-package]: /azure/azure-functions/run-functions-from-deployment-package
[functions-scale]: /azure/azure-functions/functions-scale
[functions-timeout]: /azure/azure-functions/functions-scale#consumption-plan
[functions-zip-deploy]: /azure/azure-functions/deployment-zip-push
[graph]: https://developer.microsoft.com/graph/docs/concepts/overview
[key-vault-web-app]: /azure/key-vault/tutorial-web-application-keyvault
[microservices-domain-analysis]: ../../microservices/domain-analysis.md
[monitor]: /azure/azure-monitor/overview
[oauth-flow]: https://auth0.com/docs/api-auth/which-oauth-flow-to-use
[partition-key]: /azure/cosmos-db/partition-data
[pipelines]: /azure/devops/pipelines/index
[ru]: /azure/cosmos-db/request-units
[scopes]: /azure/active-directory/develop/v2-permissions-and-consent
[static-hosting]: /azure/storage/blobs/storage-blob-static-website
[static-hosting-preview]: https://azure.microsoft.com/blog/azure-storage-static-web-hosting-public-preview/
[storage-https]: /azure/storage/common/storage-require-secure-transfer
[tm]: /azure/traffic-manager/traffic-manager-overview

[github]: https://github.com/mspnp/serverless-reference-implementation
[HttpRequestAuthorizationExtensions]: https://github.com/mspnp/serverless-reference-implementation/blob/master/src/DroneStatus/dotnet/DroneStatusFunctionApp/HttpRequestAuthorizationExtensions.cs
[readme]: https://github.com/mspnp/serverless-reference-implementation/blob/master/README.md
