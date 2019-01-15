---
title: 게이트웨이 오프로딩 패턴
titleSuffix: Cloud Design Patterns
description: 공유 또는 특수 서비스 기능을 게이트웨이 프록시에 오프로드합니다.
keywords: 디자인 패턴
author: dragon119
ms.date: 06/23/2017
ms.custom: seodec18
ms.openlocfilehash: 50af3d8593279986ed6efee55667187424c18e56
ms.sourcegitcommit: 680c9cef945dff6fee5e66b38e24f07804510fa9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54010217"
---
# <a name="gateway-offloading-pattern"></a><span data-ttu-id="95e28-104">게이트웨이 오프로딩 패턴</span><span class="sxs-lookup"><span data-stu-id="95e28-104">Gateway Offloading pattern</span></span>

<span data-ttu-id="95e28-105">공유 또는 특수 서비스 기능을 게이트웨이 프록시에 오프로드합니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-105">Offload shared or specialized service functionality to a gateway proxy.</span></span> <span data-ttu-id="95e28-106">이 패턴은 SSL 인증서 사용과 같은 공유 서비스 기능을 애플리케이션의 다른 부분에서 게이트웨이로 이동하여 애플리케이션 개발을 간소화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-106">This pattern can simplify application development by moving shared service functionality, such as the use of SSL certificates, from other parts of the application into the gateway.</span></span>

## <a name="context-and-problem"></a><span data-ttu-id="95e28-107">컨텍스트 및 문제점</span><span class="sxs-lookup"><span data-stu-id="95e28-107">Context and problem</span></span>

<span data-ttu-id="95e28-108">일부 기능은 여러 서비스에서 공통적으로 사용되며, 이러한 기능은 구성, 관리 및 유지 관리가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-108">Some features are commonly used across multiple services, and these features require configuration, management, and maintenance.</span></span> <span data-ttu-id="95e28-109">모든 애플리케이션 배포에서 분산되는 공유 또는 특수 서비스는 관리 오버헤드를 높이며, 배포 오류의 발생 가능성을 높입니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-109">A shared or specialized service that is distributed with every application deployment increases the administrative overhead and increases the likelihood of deployment error.</span></span> <span data-ttu-id="95e28-110">공유 기능에 대한 업데이트는 해당 기능을 공유하는 모든 서비스에 배포해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-110">Any updates to a shared feature must be deployed across all services that share that feature.</span></span>

<span data-ttu-id="95e28-111">보안 문제(토큰 유효성 검사, 암호화, SSL 인증서 관리) 및 기타 복잡한 작업을 제대로 처리하려면 팀원들에게 매우 전문적인 기술이 필요할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-111">Properly handling security issues (token validation, encryption, SSL certificate management) and other complex tasks can require team members to have highly specialized skills.</span></span> <span data-ttu-id="95e28-112">예를 들어 애플리케이션에 필요한 인증서를 모든 애플리케이션 인스턴스에서 구성 및 배포해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-112">For example, a certificate needed by an application must be configured and deployed on all application instances.</span></span> <span data-ttu-id="95e28-113">각각의 새로운 배포에서 인증서가 만료되지 않도록 관리해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-113">With each new deployment, the certificate must be managed to ensure that it does not expire.</span></span> <span data-ttu-id="95e28-114">모든 애플리케이션 배포에서 만료 예정인 모든 공용 인증서를 업데이트하고 테스트하고 확인해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-114">Any common certificate that is due to expire must be updated, tested, and verified on every application deployment.</span></span>

<span data-ttu-id="95e28-115">인증, 권한 부여, 로깅, 모니터링 또는 [제한](./throttling.md)과 같은 기타 공통 서비스를 많은 수의 배포에서 구현하고 관리하는 것은 어려울 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-115">Other common services such as authentication, authorization, logging, monitoring, or [throttling](./throttling.md) can be difficult to implement and manage across a large number of deployments.</span></span> <span data-ttu-id="95e28-116">오버헤드와 오류 가능성을 줄이기 위해서는 이러한 유형의 기능을 통합하는 것이 더 나을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-116">It may be better to consolidate this type of functionality, in order to reduce overhead and the chance of errors.</span></span>

## <a name="solution"></a><span data-ttu-id="95e28-117">해결 방법</span><span class="sxs-lookup"><span data-stu-id="95e28-117">Solution</span></span>

<span data-ttu-id="95e28-118">일부 기능, 특히 인증서 관리, 인증, SSL 종료, 모니터링, 프로토콜 변환 또는 제한과 같이 교차 적용되는 문제를 API 게이트웨이에 오프로드합니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-118">Offload some features into an API gateway, particularly cross-cutting concerns such as certificate management, authentication, SSL termination, monitoring, protocol translation, or throttling.</span></span>

<span data-ttu-id="95e28-119">다음 다이어그램에서는 인바운드 SSL 연결을 종료하는 API 게이트웨이를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-119">The following diagram shows an API gateway that terminates inbound SSL connections.</span></span> <span data-ttu-id="95e28-120">여기서는 원래 요청자 대신, API 게이트웨이의 업스트림 HTTP 서버에서 데이터를 요청합니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-120">It requests data on behalf of the original requestor from any HTTP server upstream of the API gateway.</span></span>

 ![게이트웨이 오프로딩 패턴의 다이어그램](./_images/gateway-offload.png)

<span data-ttu-id="95e28-122">이 패턴의 이점은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-122">Benefits of this pattern include:</span></span>

- <span data-ttu-id="95e28-123">웹 서버 인증서 및 보안 웹 사이트의 구성 등과 같은 지원 리소스를 배포하고 유지 관리할 필요를 없앰으로써 서비스의 개발을 단순화합니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-123">Simplify the development of services by removing the need to distribute and maintain supporting resources, such as web server certificates and configuration for secure websites.</span></span> <span data-ttu-id="95e28-124">구성이 좀 더 단순해지면 관리 및 확장이 용이해지며, 서비스 업그레이드가 더 간단해집니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-124">Simpler configuration results in easier management and scalability and makes service upgrades simpler.</span></span>

- <span data-ttu-id="95e28-125">보안처럼 전문 지식이 필요한 기능은 전담 팀에서 구현하도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-125">Allow dedicated teams to implement features that require specialized expertise, such as security.</span></span> <span data-ttu-id="95e28-126">이렇게 하면 이러한 전문적이면서도 교차 적용되는 문제를 관련 전문가에게 맡기고, 핵심 팀은 애플리케이션 기능에 주력할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-126">This allows your core team to focus on the application functionality, leaving these specialized but cross-cutting concerns to the relevant experts.</span></span>

- <span data-ttu-id="95e28-127">요청 및 응답 로깅/모니터링에 대해 일관성을 유지합니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-127">Provide some consistency for request and response logging and monitoring.</span></span> <span data-ttu-id="95e28-128">서비스가 올바르게 계측되지 않더라도, 최소 수준의 모니터링 및 로깅을 보장하도록 게이트웨이를 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-128">Even if a service is not correctly instrumented, the gateway can be configured to ensure a minimum level of monitoring and logging.</span></span>

## <a name="issues-and-considerations"></a><span data-ttu-id="95e28-129">문제 및 고려 사항</span><span class="sxs-lookup"><span data-stu-id="95e28-129">Issues and considerations</span></span>

- <span data-ttu-id="95e28-130">API 게이트웨이가 높은 가용성을 유지하고 오류에 대해 복원력을 가지는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-130">Ensure the API gateway is highly available and resilient to failure.</span></span> <span data-ttu-id="95e28-131">API 게이트웨이의 여러 인스턴스를 실행하여 단일 실패 지점을 방지합니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-131">Avoid single points of failure by running multiple instances of your API gateway.</span></span>
- <span data-ttu-id="95e28-132">게이트웨이가 애플리케이션 및 엔드포인트의 용량 및 크기 조정 요구 사항에 맞게 디자인되었는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-132">Ensure the gateway is designed for the capacity and scaling requirements of your application and endpoints.</span></span> <span data-ttu-id="95e28-133">게이트웨이가 애플리케이션에서 병목 현상을 유발하는 요인이 되지 않으면서 충분히 확장 가능하도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-133">Make sure the gateway does not become a bottleneck for the application and is sufficiently scalable.</span></span>
- <span data-ttu-id="95e28-134">보안 또는 데이터 전송 등의 전체 애플리케이션에서 사용되는 기능만 오프로드합니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-134">Only offload features that are used by the entire application, such as security or data transfer.</span></span>
- <span data-ttu-id="95e28-135">비즈니스 논리는 API 게이트웨이로 절대 오프로드하지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-135">Business logic should never be offloaded to the API gateway.</span></span>
- <span data-ttu-id="95e28-136">트랜잭션을 추적해야 할 경우 로깅을 위한 상관 관계 ID를 생성하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-136">If you need to track transactions, consider generating correlation IDs for logging purposes.</span></span>

## <a name="when-to-use-this-pattern"></a><span data-ttu-id="95e28-137">이 패턴을 사용해야 하는 경우</span><span class="sxs-lookup"><span data-stu-id="95e28-137">When to use this pattern</span></span>

<span data-ttu-id="95e28-138">다음의 경우에 이 패턴을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-138">Use this pattern when:</span></span>

- <span data-ttu-id="95e28-139">애플리케이션 배포에 SSL 인증서 또는 암호화와 같은 공유 문제가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-139">An application deployment has a shared concern such as SSL certificates or encryption.</span></span>
- <span data-ttu-id="95e28-140">메모리 리소스, 저장소 용량 또는 네트워크 연결 등의 다른 리소스 요구 사항이 있을 수 있는 여러 애플리케이션 배포에서 공통되는 기능이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-140">A feature that is common across application deployments that may have different resource requirements, such as memory resources, storage capacity or network connections.</span></span>
- <span data-ttu-id="95e28-141">네트워크 보안, 제한 또는 기타 네트워크 경계 관련 문제 등에 대한 책임을 좀 더 전문적인 팀으로 전환하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-141">You wish to move the responsibility for issues such as network security, throttling, or other network boundary concerns to a more specialized team.</span></span>

<span data-ttu-id="95e28-142">이러한 패턴은 서비스 간에 결합을 발생시킬 경우 적합하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-142">This pattern may not be suitable if it introduces coupling across services.</span></span>

## <a name="example"></a><span data-ttu-id="95e28-143">예</span><span class="sxs-lookup"><span data-stu-id="95e28-143">Example</span></span>

<span data-ttu-id="95e28-144">Nginx를 SSL 오프로드 어플라이언스로 사용할 경우 다음 구성은 인바운드 SSL 연결을 종료하고 업스트림 HTTP 서버 3개 중 하나로 연결을 분산합니다.</span><span class="sxs-lookup"><span data-stu-id="95e28-144">Using Nginx as the SSL offload appliance, the following configuration terminates an inbound SSL connection and distributes the connection to one of three upstream HTTP servers.</span></span>

```console
upstream iis {
        server  10.3.0.10    max_fails=3    fail_timeout=15s;
        server  10.3.0.20    max_fails=3    fail_timeout=15s;
        server  10.3.0.30    max_fails=3    fail_timeout=15s;
}

server {
        listen 443;
        ssl on;
        ssl_certificate /etc/nginx/ssl/domain.cer;
        ssl_certificate_key /etc/nginx/ssl/domain.key;

        location / {
                set $targ iis;
                proxy_pass http://$targ;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto https;
proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header Host $host;
        }
}
```

## <a name="related-guidance"></a><span data-ttu-id="95e28-145">관련 지침</span><span class="sxs-lookup"><span data-stu-id="95e28-145">Related guidance</span></span>

- [<span data-ttu-id="95e28-146">프런트 엔드에 대한 백 엔드 패턴</span><span class="sxs-lookup"><span data-stu-id="95e28-146">Backends for Frontends pattern</span></span>](./backends-for-frontends.md)
- [<span data-ttu-id="95e28-147">게이트웨이 집계 패턴</span><span class="sxs-lookup"><span data-stu-id="95e28-147">Gateway Aggregation pattern</span></span>](./gateway-aggregation.md)
- [<span data-ttu-id="95e28-148">게이트웨이 라우팅 패턴</span><span class="sxs-lookup"><span data-stu-id="95e28-148">Gateway Routing pattern</span></span>](./gateway-routing.md)
