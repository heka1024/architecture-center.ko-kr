---
title: 합리화의 5가지 R
titleSuffix: Enterprise Cloud Adoption
description: 디지털 자산을 합리화하는 경우에 사용할 수 있는 옵션을 설명합니다.
author: BrianBlanchard
ms.date: 12/10/2018
ms.openlocfilehash: 66eee782b63210bcba3022e3be5b8fdee11716ec
ms.sourcegitcommit: bb7fcffbb41e2c26a26f8781df32825eb60df70c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/20/2018
ms.locfileid: "53643682"
---
# <a name="enterprise-cloud-adoption-the-5-rs-of-rationalization"></a><span data-ttu-id="d206f-103">엔터프라이즈 클라우드 채택: 합리화의 5가지 R</span><span class="sxs-lookup"><span data-stu-id="d206f-103">Enterprise Cloud Adoption: The 5 Rs of rationalization</span></span>

<span data-ttu-id="d206f-104">클라우드 합리화는 자산을 평가하여 클라우드의 각 자산을 마이그레이션하거나 현대화할 때 사용하면 가장 좋은 접근 방식을 결정하는 프로세스입니다.</span><span class="sxs-lookup"><span data-stu-id="d206f-104">Cloud Rationalization is the process of evaluating assets to determine the best approach to migrating or modernizing each asset in the cloud.</span></span> <span data-ttu-id="d206f-105">합리화 프로세스에 대한 자세한 내용은 [디지털 자산이란?](overview.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="d206f-105">For more information about the process of rationalization, see [What is a digital estate?](overview.md)</span></span>

<span data-ttu-id="d206f-106">여기에 나열된 "합리화의 5가지 R"은 가장 일반적인 합리화 옵션을 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="d206f-106">The "5 Rs of rationalization" listed here describe the most common options for rationalization.</span></span>

## <a name="rehost"></a><span data-ttu-id="d206f-107">다시 호스트</span><span class="sxs-lookup"><span data-stu-id="d206f-107">Rehost</span></span>

<span data-ttu-id="d206f-108">"리프트 앤 시프트"라고도 하는 다시 호스트 작업은 전체 아키텍처를 최소한만 변경하여 현재 상태 자산을 선택된 클라우드 공급자로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="d206f-108">Also known as "lift and shift," a rehost effort moves the current state asset to the chosen cloud provider, with minimal change to overall architecture.</span></span>

<span data-ttu-id="d206f-109">일반적인 동기는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="d206f-109">Common drivers could include:</span></span>

* <span data-ttu-id="d206f-110">CapEx 절감</span><span class="sxs-lookup"><span data-stu-id="d206f-110">Reduce CapEx</span></span>
* <span data-ttu-id="d206f-111">데이터 센터 공간 확보</span><span class="sxs-lookup"><span data-stu-id="d206f-111">Free up datacenter space</span></span>
* <span data-ttu-id="d206f-112">빠른 클라우드 ROI</span><span class="sxs-lookup"><span data-stu-id="d206f-112">Quick cloud ROI</span></span>

<span data-ttu-id="d206f-113">정량적 분석 요소:</span><span class="sxs-lookup"><span data-stu-id="d206f-113">Quantitative analysis factors:</span></span>

* <span data-ttu-id="d206f-114">VM 크기(CPU, 메모리, 스토리지)</span><span class="sxs-lookup"><span data-stu-id="d206f-114">VM size (CPU, memory, storage)</span></span>
* <span data-ttu-id="d206f-115">종속성(네트워크 트래픽)</span><span class="sxs-lookup"><span data-stu-id="d206f-115">Dependencies (network traffic)</span></span>
* <span data-ttu-id="d206f-116">자산 호환성</span><span class="sxs-lookup"><span data-stu-id="d206f-116">Asset compatibility</span></span>

<span data-ttu-id="d206f-117">정성적 분석 요소:</span><span class="sxs-lookup"><span data-stu-id="d206f-117">Qualitative analysis factors:</span></span>

* <span data-ttu-id="d206f-118">변경 허용 오차</span><span class="sxs-lookup"><span data-stu-id="d206f-118">Tolerance for change</span></span>
* <span data-ttu-id="d206f-119">비즈니스 우선 순위</span><span class="sxs-lookup"><span data-stu-id="d206f-119">Business priorities</span></span>
* <span data-ttu-id="d206f-120">중요한 비즈니스 이벤트</span><span class="sxs-lookup"><span data-stu-id="d206f-120">Critical business events</span></span>
* <span data-ttu-id="d206f-121">프로세스 종속성</span><span class="sxs-lookup"><span data-stu-id="d206f-121">Process dependencies</span></span>

## <a name="refactor"></a><span data-ttu-id="d206f-122">리팩터링</span><span class="sxs-lookup"><span data-stu-id="d206f-122">Refactor</span></span>

<span data-ttu-id="d206f-123">PaaS(Platform as a Service) 옵션은 여러 애플리케이션과 관련된 운영 비용을 줄일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d206f-123">Platform as a Service (PaaS) options can reduce operational costs associated with many applications.</span></span> <span data-ttu-id="d206f-124">PaaS 기반 모델에 맞게 애플리케이션을 약간 리팩터링하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="d206f-124">It can be prudent to slightly refactor an application to fit a PaaS based model.</span></span>

<span data-ttu-id="d206f-125">또한 애플리케이션이 새로운 비즈니스 기회를 이행할 수 있도록 코드를 리팩터링하는 애플리케이션 개발 프로세스도 리팩터링이라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="d206f-125">Refactor also refers to the application development process of refactoring code to allow an application to deliver on new business opportunities.</span></span>

<span data-ttu-id="d206f-126">일반적인 동기는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="d206f-126">Common drivers could include:</span></span>

* <span data-ttu-id="d206f-127">더 빠르고, 더 짧은 업데이트</span><span class="sxs-lookup"><span data-stu-id="d206f-127">Faster, shorter, updates</span></span>
* <span data-ttu-id="d206f-128">코드 이식성</span><span class="sxs-lookup"><span data-stu-id="d206f-128">Code portability</span></span>
* <span data-ttu-id="d206f-129">더 높은 클라우드 효율성(리소스, 속도, 비용)</span><span class="sxs-lookup"><span data-stu-id="d206f-129">Greater cloud efficiency (resources, speed, cost)</span></span>

<span data-ttu-id="d206f-130">정량적 분석 요소:</span><span class="sxs-lookup"><span data-stu-id="d206f-130">Quantitative analysis factors:</span></span>

* <span data-ttu-id="d206f-131">애플리케이션 자산 크기(CPU, 메모리, 스토리지)</span><span class="sxs-lookup"><span data-stu-id="d206f-131">Application asset size (CPU, memory, storage)</span></span>
* <span data-ttu-id="d206f-132">종속성(네트워크 트래픽)</span><span class="sxs-lookup"><span data-stu-id="d206f-132">Dependencies (network traffic)</span></span>
* <span data-ttu-id="d206f-133">사용자 트래픽(페이지 보기, 페이지의 시간, 로드 시간)</span><span class="sxs-lookup"><span data-stu-id="d206f-133">User traffic (page views, time on page, load time)</span></span>
* <span data-ttu-id="d206f-134">개발 플랫폼(언어, 데이터 플랫폼, 중간 계층 서비스)</span><span class="sxs-lookup"><span data-stu-id="d206f-134">Development platform (languages, data platform, middle tier services)</span></span>

<span data-ttu-id="d206f-135">정성적 분석 요소:</span><span class="sxs-lookup"><span data-stu-id="d206f-135">Qualitative analysis factors:</span></span>

* <span data-ttu-id="d206f-136">지속적인 비즈니스 투자</span><span class="sxs-lookup"><span data-stu-id="d206f-136">Continued business investments</span></span>
* <span data-ttu-id="d206f-137">버스팅 옵션/타임라인</span><span class="sxs-lookup"><span data-stu-id="d206f-137">Bursting options/timelines</span></span>
* <span data-ttu-id="d206f-138">비즈니스 프로세스 종속성</span><span class="sxs-lookup"><span data-stu-id="d206f-138">Business process dependencies</span></span>

## <a name="rearchitect"></a><span data-ttu-id="d206f-139">아키텍처 변경</span><span class="sxs-lookup"><span data-stu-id="d206f-139">Rearchitect</span></span>

<span data-ttu-id="d206f-140">일부 오래된 애플리케이션은 애플리케이션이 구축될 당시에 아키텍처와 관련하여 결정된 사항으로 인해 클라우드 공급자와 호환되지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d206f-140">Some aging applications aren't compatible with cloud providers because of the architectural decisions made when the application was built.</span></span> <span data-ttu-id="d206f-141">이러한 경우 전환 전에 애플리케이션의 아키텍처를 변경해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d206f-141">In these cases, the application may need to be rearchitected prior to transformation.</span></span>

<span data-ttu-id="d206f-142">다른 경우, 클라우드와 호환되지만 클라우드 네이티브의 이점이 없는 애플리케이션은 솔루션의 아키텍처를 클라우드 네이티브 애플리케이션으로 변경하여 비용 효율과 운영 효율을 창출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d206f-142">In other cases, applications that are cloud compatible, but not cloud native benefits, may produce cost efficiencies and operational efficiencies by rearchitecting the solution to be a cloud native application.</span></span>

<span data-ttu-id="d206f-143">일반적인 동기는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="d206f-143">Common drivers could include:</span></span>

* <span data-ttu-id="d206f-144">애플리케이션 규모 및 민첩성</span><span class="sxs-lookup"><span data-stu-id="d206f-144">Application scale and agility</span></span>
* <span data-ttu-id="d206f-145">새로운 클라우드 기능을 더욱 쉽게 채택</span><span class="sxs-lookup"><span data-stu-id="d206f-145">Easier adoption of new cloud capabilities</span></span>
* <span data-ttu-id="d206f-146">기술 스택 혼합</span><span class="sxs-lookup"><span data-stu-id="d206f-146">Mix of technology stacks</span></span>

<span data-ttu-id="d206f-147">정량적 분석 요소:</span><span class="sxs-lookup"><span data-stu-id="d206f-147">Quantitative analysis factors:</span></span>

* <span data-ttu-id="d206f-148">애플리케이션 자산 크기(CPU, 메모리, 스토리지)</span><span class="sxs-lookup"><span data-stu-id="d206f-148">Application asset size (CPU, memory, storage)</span></span>
* <span data-ttu-id="d206f-149">종속성(네트워크 트래픽)</span><span class="sxs-lookup"><span data-stu-id="d206f-149">Dependencies (network traffic)</span></span>
* <span data-ttu-id="d206f-150">사용자 트래픽(페이지 보기, 페이지의 시간, 로드 시간)</span><span class="sxs-lookup"><span data-stu-id="d206f-150">User traffic (page views, time on page, load time)</span></span>
* <span data-ttu-id="d206f-151">개발 플랫폼(언어, 데이터 플랫폼, 중간 계층 서비스)</span><span class="sxs-lookup"><span data-stu-id="d206f-151">Development platform (languages, data platform, middle tier services)</span></span>

<span data-ttu-id="d206f-152">정성적 분석 요소:</span><span class="sxs-lookup"><span data-stu-id="d206f-152">Qualitative analysis factors:</span></span>

* <span data-ttu-id="d206f-153">늘어나는 비즈니스 투자</span><span class="sxs-lookup"><span data-stu-id="d206f-153">Growing business investments</span></span>
* <span data-ttu-id="d206f-154">운영 비용</span><span class="sxs-lookup"><span data-stu-id="d206f-154">Operational costs</span></span>
* <span data-ttu-id="d206f-155">잠재적인 피드백 루프 및 DevOps 투자</span><span class="sxs-lookup"><span data-stu-id="d206f-155">Potential feedback loops and DevOps investments</span></span>

## <a name="rebuild"></a><span data-ttu-id="d206f-156">다시 빌드</span><span class="sxs-lookup"><span data-stu-id="d206f-156">Rebuild</span></span>

<span data-ttu-id="d206f-157">일부 시나리오에서는 애플리케이션을 추진하기 위해 극복해야 하는 델타가 너무 커서 추가 투자가 정당화되지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d206f-157">In some scenarios, the delta that must be overcome to carry forward an application can be too large to justify further investment.</span></span> <span data-ttu-id="d206f-158">비즈니스 요구 사항을 충족했었으나 이제는 비즈니스 프로세스가 수행되는 방식과 맞지 않거나 지원되지 않는 애플리케이션의 경우에 특히 그렇습니다.</span><span class="sxs-lookup"><span data-stu-id="d206f-158">This is especially true for applications that used to meet the needs of the business, but are now unsupported or misaligned with how the business processes are executed today.</span></span> <span data-ttu-id="d206f-159">이 경우 새 코드 베이스를 만들어 클라우드 네이티브 방식과 맞출 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d206f-159">In this case, a new code base is created to align with a cloud native approach.</span></span>

<span data-ttu-id="d206f-160">일반적인 동기는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="d206f-160">Common drivers could include:</span></span>

* <span data-ttu-id="d206f-161">혁신 가속화</span><span class="sxs-lookup"><span data-stu-id="d206f-161">Accelerate innovation</span></span>
* <span data-ttu-id="d206f-162">더 빠르게 앱 빌드</span><span class="sxs-lookup"><span data-stu-id="d206f-162">Build apps faster</span></span>
* <span data-ttu-id="d206f-163">운영 비용 절감</span><span class="sxs-lookup"><span data-stu-id="d206f-163">Reduce operational cost</span></span>

<span data-ttu-id="d206f-164">정량적 분석 요소:</span><span class="sxs-lookup"><span data-stu-id="d206f-164">Quantitative analysis factors:</span></span>

* <span data-ttu-id="d206f-165">애플리케이션 자산 크기(CPU, 메모리, 스토리지)</span><span class="sxs-lookup"><span data-stu-id="d206f-165">Application asset size (CPU, memory, storage)</span></span>
* <span data-ttu-id="d206f-166">종속성(네트워크 트래픽)</span><span class="sxs-lookup"><span data-stu-id="d206f-166">Dependencies (network traffic)</span></span>
* <span data-ttu-id="d206f-167">사용자 트래픽(페이지 보기, 페이지의 시간, 로드 시간)</span><span class="sxs-lookup"><span data-stu-id="d206f-167">User traffic (page views, time on page, load time)</span></span>
* <span data-ttu-id="d206f-168">개발 플랫폼(언어, 데이터 플랫폼, 중간 계층 서비스)</span><span class="sxs-lookup"><span data-stu-id="d206f-168">Development platform (languages, data platform, middle tier services)</span></span>

<span data-ttu-id="d206f-169">정성적 분석 요소:</span><span class="sxs-lookup"><span data-stu-id="d206f-169">Qualitative analysis Factors:</span></span>

* <span data-ttu-id="d206f-170">최종 사용자의 만족도 감소</span><span class="sxs-lookup"><span data-stu-id="d206f-170">Declining end user satisfaction</span></span>
* <span data-ttu-id="d206f-171">기능의 제한을 받는 비즈니스 프로세스</span><span class="sxs-lookup"><span data-stu-id="d206f-171">Business processes limited by functionality</span></span>
* <span data-ttu-id="d206f-172">잠재적 비용, 환경 또는 수익 향상</span><span class="sxs-lookup"><span data-stu-id="d206f-172">Potential cost, experience, or revenue gains</span></span>

## <a name="replace"></a><span data-ttu-id="d206f-173">Replace</span><span class="sxs-lookup"><span data-stu-id="d206f-173">Replace</span></span>

<span data-ttu-id="d206f-174">솔루션은 일반적으로 당시에 사용 가능한 최고의 기술과 접근 방식을 사용하여 구현됩니다.</span><span class="sxs-lookup"><span data-stu-id="d206f-174">Solutions are generally implemented using the best technology and approach available at the time.</span></span> <span data-ttu-id="d206f-175">일부 경우에는 SaaS(Software as a Service) 애플리케이션이 호스팅되는 애플리케이션에 필요한 모든 기능을 충족할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d206f-175">In some cases, Software as a Service (SaaS) applications can meet all of the functionality required of the hosted application.</span></span> <span data-ttu-id="d206f-176">이러한 시나리오에서는 향후 교체를 위해 워크로드를 계획하여 전환 활동에서 효과적으로 제외할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d206f-176">In these scenarios, a workload could be slated for future replacement, effectively removing it from the transformation effort.</span></span>

<span data-ttu-id="d206f-177">일반적인 동기는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="d206f-177">Common drivers could include:</span></span>

* <span data-ttu-id="d206f-178">업계 모범 사례를 기준으로 표준화</span><span class="sxs-lookup"><span data-stu-id="d206f-178">Standardize around industry-best practices</span></span>
* <span data-ttu-id="d206f-179">비즈니스 프로세스 중심의 접근 방식 채택 가속화</span><span class="sxs-lookup"><span data-stu-id="d206f-179">Accelerate adoption of business process driven approaches</span></span>
* <span data-ttu-id="d206f-180">경쟁력 있는 차별점이나 경쟁 우위를 창출하는 애플리케이션에 개발 투자 비용 재할당</span><span class="sxs-lookup"><span data-stu-id="d206f-180">Reallocate development investments into applications that create competitive differentiation or advantages</span></span>

<span data-ttu-id="d206f-181">정량적 분석 요소:</span><span class="sxs-lookup"><span data-stu-id="d206f-181">Quantitative analysis factors:</span></span>

* <span data-ttu-id="d206f-182">일반 운영 비용 절감</span><span class="sxs-lookup"><span data-stu-id="d206f-182">General operating cost reductions</span></span>
* <span data-ttu-id="d206f-183">VM 크기(CPU, 메모리, 스토리지)</span><span class="sxs-lookup"><span data-stu-id="d206f-183">VM size (CPU, memory, storage)</span></span>
* <span data-ttu-id="d206f-184">종속성(네트워크 트래픽)</span><span class="sxs-lookup"><span data-stu-id="d206f-184">Dependencies (network traffic)</span></span>
* <span data-ttu-id="d206f-185">사용 중지되어야 하는 자산</span><span class="sxs-lookup"><span data-stu-id="d206f-185">Assets to be retired</span></span>

<span data-ttu-id="d206f-186">정성적 분석 요소:</span><span class="sxs-lookup"><span data-stu-id="d206f-186">Qualitative analysis factors:</span></span>

* <span data-ttu-id="d206f-187">현재 아키텍처와 SaaS 솔루션의 비용 이점 분석</span><span class="sxs-lookup"><span data-stu-id="d206f-187">Cost benefit analysis of current architecture vs SaaS solution</span></span>
* <span data-ttu-id="d206f-188">비즈니스 프로세스 맵</span><span class="sxs-lookup"><span data-stu-id="d206f-188">Business process maps</span></span>
* <span data-ttu-id="d206f-189">데이터 스키마</span><span class="sxs-lookup"><span data-stu-id="d206f-189">Data schemas</span></span>
* <span data-ttu-id="d206f-190">사용자 지정 또는 자동화된 프로세스</span><span class="sxs-lookup"><span data-stu-id="d206f-190">Custom or automated processes</span></span>

## <a name="next-steps"></a><span data-ttu-id="d206f-191">다음 단계</span><span class="sxs-lookup"><span data-stu-id="d206f-191">Next steps</span></span>

<span data-ttu-id="d206f-192">합리화의 5가지 R을 전체적으로 디지털 자산에 적용하여 각 애플리케이션의 향후 상태와 관련된 합리화 결정을 내릴 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d206f-192">Collectively, these 5 Rs of Rationalization can be applied to a digital estate to make rationalization decisions regarding the future state of each application.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d206f-193">디지털 자산이란?</span><span class="sxs-lookup"><span data-stu-id="d206f-193">What is a digital estate?</span></span>](overview.md)
