---
title: Azure 복원 력 있는 응용 프로그램에 대 한 요구 사항 정의
description: Azure 응용 프로그램의 가용성과 복원 력 요구 사항 수집 개요
author: MikeWasson
ms.date: 04/10/2019
ms.topic: article
ms.service: architecture-center
ms.subservice: cloud-design-principles
ms.openlocfilehash: 2af2ce4200c285abfda1395862d21efc3c17e577
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59646630"
---
# <a name="developing-requirements-for-resilient-azure-applications"></a><span data-ttu-id="9293d-103">Azure 복원 력 있는 응용 프로그램에 대 한 요구 사항 개발</span><span class="sxs-lookup"><span data-stu-id="9293d-103">Developing requirements for resilient Azure applications</span></span>

<span data-ttu-id="9293d-104">구축 *복원 력* (오류 로부터 복구) 및 *가용성* (상당한 가동 중지 없이 정상 상태에서 실행)를 앱에 요구 사항 수집으로 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-104">Building *resiliency* (recovering from failures) and *availability* (running in a healthy state without significant downtime) into your apps begins with gathering requirements.</span></span> <span data-ttu-id="9293d-105">예를 들어 가동 중지 시간이 얼마나 적합?</span><span class="sxs-lookup"><span data-stu-id="9293d-105">For example, how much downtime is acceptable?</span></span> <span data-ttu-id="9293d-106">잠재적인 가동 중지 시간에 비즈니스가 비용은 얼마 인가요?</span><span class="sxs-lookup"><span data-stu-id="9293d-106">How much does potential downtime cost your business?</span></span> <span data-ttu-id="9293d-107">고객의 가용성 요구 사항은 무엇입니까?</span><span class="sxs-lookup"><span data-stu-id="9293d-107">What are your customer's availability requirements?</span></span> <span data-ttu-id="9293d-108">응용 프로그램을 항상 사용할 수 있도록에 얼마나 투자 수행?</span><span class="sxs-lookup"><span data-stu-id="9293d-108">How much do you invest in making your application highly available?</span></span> <span data-ttu-id="9293d-109">비용과 위험 란?</span><span class="sxs-lookup"><span data-stu-id="9293d-109">What is the risk versus the cost?</span></span>

## <a name="identify-distinct-workloads"></a><span data-ttu-id="9293d-110">고유한 워크 로드 식별</span><span class="sxs-lookup"><span data-stu-id="9293d-110">Identify distinct workloads</span></span>

<span data-ttu-id="9293d-111">일반적으로 여러 응용 프로그램 클라우드 솔루션에 이루어진 *워크 로드*합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-111">Cloud solutions typically consist of multiple application *workloads*.</span></span> <span data-ttu-id="9293d-112">워크 로드에는 고유한 기능 또는 비즈니스 논리와 데이터 저장소 요구 사항 측면에서 다른 태스크와에서 논리적으로 분리 된 작업입니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-112">A workload is a distinct capability or task that is logically separated from other tasks in terms of business logic and data storage requirements.</span></span> <span data-ttu-id="9293d-113">예를 들어 전자 상거래 앱에는 다음 워크 로드는 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-113">For example, an e-commerce app might have the following workloads:</span></span>

- <span data-ttu-id="9293d-114">제품 카탈로그를 찾아보고 검색합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-114">Browse and search a product catalog.</span></span>
- <span data-ttu-id="9293d-115">주문을 작성하고 추적합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-115">Create and track orders.</span></span>
- <span data-ttu-id="9293d-116">추천 제품을 살펴봅니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-116">View recommendations.</span></span>

<span data-ttu-id="9293d-117">각 워크 로드에는 가용성, 확장성, 데이터 일관성 및 재해 복구에 대 한 다른 요구 사항이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-117">Each workload has different requirements for availability, scalability, data consistency, and disaster recovery.</span></span> <span data-ttu-id="9293d-118">각 워크 로드에 대 한 위험 및 비용을 분산 하 여 비즈니스 결정을 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-118">Make your business decisions by balancing cost versus risk for each workload.</span></span>

<span data-ttu-id="9293d-119">또한 서비스 수준 목표 워크 로드를 분해 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-119">Also decompose workloads by service-level objective.</span></span> <span data-ttu-id="9293d-120">서비스 구성 된 경우 중요 하 고 덜 중요 워크 로드를 다르게 관리 하도록 하 고 서비스 기능 및 가용성 요구 사항을 충족 하는 데 필요한 인스턴스 수를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-120">If a service is composed of critical and less-critical workloads, manage them differently and specify the service features and number of instances needed to meet their availability requirements.</span></span>

## <a name="plan-for-usage-patterns"></a><span data-ttu-id="9293d-121">사용 패턴에 대 한 계획</span><span class="sxs-lookup"><span data-stu-id="9293d-121">Plan for usage patterns</span></span>

<span data-ttu-id="9293d-122">위험 및 중요 하지 않은 기간 동안 요구 사항의 차이점을 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-122">Identify differences in requirements during critical and non-critical periods.</span></span> <span data-ttu-id="9293d-123">시스템이 반드시 작동해야 하는 특정 중요 시간대가 있습니까?</span><span class="sxs-lookup"><span data-stu-id="9293d-123">Are there certain critical periods when the system must be available?</span></span> <span data-ttu-id="9293d-124">예를 들어 세금 신고 응용 프로그램을 신고 마감 하는 동안 실패 수 없습니다 하 고 비디오 스트리밍 서비스는 라이브 이벤트 동안 지연 해서는 안 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-124">For example, a tax-filing application can't fail during a filing deadline and a video streaming service shouldn't lag during a live event.</span></span> <span data-ttu-id="9293d-125">이러한 상황에서 하는 비용과 위험 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-125">In these situations, weigh the cost against the risk.</span></span>

- <span data-ttu-id="9293d-126">가동 시간을 보장 하 고 중요 한 기간에 서비스 수준 계약 (Sla)을 충족 하려면 중복성을 계획 여러 지역에 걸쳐 하나가 실패할 경우 많은 비용이 소요 하는 경우에 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-126">To ensure uptime and meet service-level agreements (SLAs) in critical periods, plan redundancy across several regions in case one fails, even if it costs more.</span></span>
- <span data-ttu-id="9293d-127">반대로, 중요 하지 않은 기간 동안 비용을 최소화 하기 위해 단일 지역에서 응용 프로그램을 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-127">Conversely, during non-critical periods, run your application in a single region to minimize costs.</span></span>
- <span data-ttu-id="9293d-128">경우에 따라 사용량 기반 요금 청구는 최신 서버 리스 기술을 사용 하 여 추가 비용을 줄일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-128">In some cases, you can mitigate additional expenses by using modern serverless techniques that have consumption-based billing.</span></span>

## <a name="establish-availability-and-recovery-metrics"></a><span data-ttu-id="9293d-129">가용성 및 복구 메트릭을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-129">Establish availability and recovery metrics</span></span>

<span data-ttu-id="9293d-130">요구 사항 프로세스의 일부로 메트릭의 두 집합에 대 한 기준 수치를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-130">Create baseline numbers for two sets of metrics as part of the requirements process.</span></span> <span data-ttu-id="9293d-131">첫 번째 집합을 사용 하 여 클라우드 서비스에 중복성을 추가 하는 위치와 고객에 게 제공 하는 Sla를 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-131">The first set helps you determine where to add redundancy to cloud services and which SLAs to provide to customers.</span></span> <span data-ttu-id="9293d-132">두 번째 재해 복구를 계획할 수 있습니다 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-132">The second set helps you plan your disaster recovery.</span></span>

### <a name="availability-metrics"></a><span data-ttu-id="9293d-133">가용성 메트릭</span><span class="sxs-lookup"><span data-stu-id="9293d-133">Availability metrics</span></span>

<span data-ttu-id="9293d-134">이러한 측정값을 사용 하 여 중복 계획 하 고 고객 Sla 확인.</span><span class="sxs-lookup"><span data-stu-id="9293d-134">Use these measures to plan for redundancy and determine customer SLAs.</span></span>

- <span data-ttu-id="9293d-135">**Mttr (평균)를 복구 하는 평균 시간은** 는 평균 시간 오류가 발생 한 후 구성 요소를 복원 하는 데 걸리는 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-135">**Mean time to recover (MTTR)** is the average time it takes to restore a component after a failure.</span></span>
- <span data-ttu-id="9293d-136">**(Mtbf) 간 평균 시간** 는 방법이 긴 구성 요소를 예상할 수 중단 간에 지속 되도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-136">**Mean time between failures (MTBF)** is the how long a component can reasonably expect to last between outages.</span></span>

### <a name="recovery-metrics"></a><span data-ttu-id="9293d-137">복구 메트릭</span><span class="sxs-lookup"><span data-stu-id="9293d-137">Recovery metrics</span></span>

<span data-ttu-id="9293d-138">위험 평가 수행 하 여 이러한 값을 파생 하 고 비용 및 가동 중지 시간과 데이터 손실의 위험을 이해 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-138">Derive these values by conducting a risk assessment, and make sure you understand the cost and risk of downtime and data loss.</span></span> <span data-ttu-id="9293d-139">이러한 시스템의 비기능적 요구 사항이 되며 비즈니스 요구 사항에 따라 결정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-139">These are nonfunctional requirements of a system and should be dictated by business requirements.</span></span>

- <span data-ttu-id="9293d-140">**복구 시간 목표 (RTO)** 사고 발생 후 응용 프로그램을 사용할 수 없는 최대 허용 가능한 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-140">**Recovery time objective (RTO)** is the maximum acceptable time an application is unavailable after an incident.</span></span>
- <span data-ttu-id="9293d-141">**복구 지점 목표 (RPO)** 재해 발생 시 허용 되는 데이터 손실의 최대 지속 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-141">**Recovery point objective (RPO)** is the maximum duration of data loss that's acceptable during a disaster.</span></span>

<span data-ttu-id="9293d-142">경우 MTTR 값 *모든* 시스템 RTO를 초과 하는 항상 사용 가능한 설치의 핵심 구성 요소, 시스템 오류는 허용 되지 않는 업무 중단이 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-142">If the MTTR value of *any* critical component in a highly available setup exceeds the system RTO, a failure in the system might cause an unacceptable business disruption.</span></span> <span data-ttu-id="9293d-143">즉, 정의 된 RTO 내에서 시스템을 복원할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-143">That is, you can't restore the system within the defined RTO.</span></span>

## <a name="determine-workload-availability-targets"></a><span data-ttu-id="9293d-144">워크 로드 가용성 목표를 결정 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-144">Determine workload availability targets</span></span>

<span data-ttu-id="9293d-145">아키텍처는 비즈니스 요구 사항을 충족 하는지 확인할 수 있도록 솔루션에서 각 워크 로드에 대 한 고유의 목표 Sla를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-145">Define your own target SLAs for each workload in your solution so you can determine whether the architecture meets the business requirements.</span></span>

### <a name="consider-cost-and-complexity"></a><span data-ttu-id="9293d-146">비용 및 복잡성을 고려</span><span class="sxs-lookup"><span data-stu-id="9293d-146">Consider cost and complexity</span></span>

<span data-ttu-id="9293d-147">다른 모든 항목을 더 높은 가용성 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-147">Everything else being equal, higher availability is better.</span></span> <span data-ttu-id="9293d-148">그러나 비용 및 복잡성이 증가 자세한 99.999 9에 따른 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-148">But as you strive for more nines, the cost and complexity grow.</span></span> <span data-ttu-id="9293d-149">가동 시간 99.99%의 월간 총 가동 중지 시간을 약 5 분으로 변환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-149">An uptime of 99.99% translates to about five minutes of total downtime per month.</span></span> <span data-ttu-id="9293d-150">복잡성과 99.999 도달 하는 비용을 들일 가치가 있습니까?</span><span class="sxs-lookup"><span data-stu-id="9293d-150">Is it worth the additional complexity and cost to reach five nines?</span></span> <span data-ttu-id="9293d-151">대답은 비즈니스 요구 사항에 따라 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-151">The answer depends on the business requirements.</span></span>

<span data-ttu-id="9293d-152">다음은 SLA를 정의할 때 고려해야 할 다른 사항입니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-152">Here are some other considerations when defining an SLA:</span></span>

- <span data-ttu-id="9293d-153">4 개의 9 (99.99%)를 위해 오류 로부터 복구 하려면 수동 개입에 사용할 수 없게 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-153">To achieve four nines (99.99%), you can't rely on manual intervention to recover from failures.</span></span> <span data-ttu-id="9293d-154">애플리케이션이 자체적으로 진단하고 자체적으로 복구해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-154">The application must be self-diagnosing and self-healing.</span></span>
- <span data-ttu-id="9293d-155">99.99%를 초과 SLA를 충족할 만큼 신속 하 게 가동 중단을 감지 하 어렵습니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-155">Beyond four nines, it's challenging to detect outages quickly enough to meet the SLA.</span></span>
- <span data-ttu-id="9293d-156">SLA가 측정되는 시간을 생각해 보세요.</span><span class="sxs-lookup"><span data-stu-id="9293d-156">Think about the time window that your SLA is measured against.</span></span> <span data-ttu-id="9293d-157">시간이 짧을수록 허용 오차도 작습니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-157">The smaller the window, the tighter the tolerances.</span></span> <span data-ttu-id="9293d-158">시간별 또는 일별 작동 시간 측면에서 SLA를 정의 하려면 적합 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-158">It doesn't make sense to define your SLA in terms of hourly or daily uptime.</span></span>
- <span data-ttu-id="9293d-159">MTBF 및 MTTR 측정값을 고려해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-159">Consider the MTBF and MTTR measurements.</span></span> <span data-ttu-id="9293d-160">높을수록 SLA, 덜 자주 서비스가 중단 될 수 있습니다 하 고 서비스를 더 빠르게 복구 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-160">The higher your SLA, the less frequently the service can go down and the quicker the service must recover.</span></span>
- <span data-ttu-id="9293d-161">응용 프로그램 각 부분의 가용성 목표에 대 한 고객의 계약 가져오기 및 문서화 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-161">Get agreement from your customers for the availability targets of each piece of your application, and document it.</span></span> <span data-ttu-id="9293d-162">이 고, 그렇지 디자인 고객의 기대를 충족 하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-162">Otherwise, your design may not meet the customers' expectations.</span></span>

### <a name="identify-dependencies"></a><span data-ttu-id="9293d-163">종속성 확인</span><span class="sxs-lookup"><span data-stu-id="9293d-163">Identify dependencies</span></span>

<span data-ttu-id="9293d-164">내부 및 외부 종속성을 식별 하려면 종속성 매핑을 연습을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-164">Perform dependency-mapping exercises to identify internal and external dependencies.</span></span> <span data-ttu-id="9293d-165">예제에서는 보안 또는 Active Directory 또는 결제 공급자와 같은 타사 서비스와 같은 id와 관련 된 종속성을 포함 또는 전자 메일 메시징 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-165">Examples include dependencies relating to security or identity, such as Active Directory, or third-party services such as a payment provider or e-mail messaging service.</span></span>

<span data-ttu-id="9293d-166">오류의 단일 지점 또는 병목 현상이 발생할 수 있는 외부 종속성에 특히 주의 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-166">Pay particular attention to external dependencies that might be a single point of failure or cause bottlenecks.</span></span> <span data-ttu-id="9293d-167">작업 필요 99.99% 가동 시간 99.9 %SLA 사용 하 여 서비스에 따라 달라 집니다 하지만 해당 서비스 시스템을 단일 실패 지점이 일 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-167">If a workload requires 99.99% uptime but depends on a service with a 99.9% SLA, that service can't be a single point of failure in the system.</span></span> <span data-ttu-id="9293d-168">한 가지 해결책은 대체 경로 서비스가 실패 하는 경우입니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-168">One remedy is to have a fallback path in case the service fails.</span></span> <span data-ttu-id="9293d-169">또는 해당 서비스의 오류를 복구 하려면 다른 조치를 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-169">Alternatively, take other measures to recover from a failure in that service.</span></span>

<span data-ttu-id="9293d-170">다음 표는 다양한 SLA 수준의 잠재적인 누적 가동 중지 시간을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-170">The following table shows the potential cumulative downtime for various SLA levels.</span></span>

| <span data-ttu-id="9293d-171">**SLA**</span><span class="sxs-lookup"><span data-stu-id="9293d-171">**SLA**</span></span> | <span data-ttu-id="9293d-172">**주간 가동 중지 시간**</span><span class="sxs-lookup"><span data-stu-id="9293d-172">**Downtime per week**</span></span> | <span data-ttu-id="9293d-173">**월간 가동 중지 시간**</span><span class="sxs-lookup"><span data-stu-id="9293d-173">**Downtime per month**</span></span> | <span data-ttu-id="9293d-174">**연간 가동 중지 시간**</span><span class="sxs-lookup"><span data-stu-id="9293d-174">**Downtime per year**</span></span> |
|---------|-----------------------|------------------------|-----------------------|
| <span data-ttu-id="9293d-175">99%</span><span class="sxs-lookup"><span data-stu-id="9293d-175">99%</span></span>     | <span data-ttu-id="9293d-176">1.68시간</span><span class="sxs-lookup"><span data-stu-id="9293d-176">1.68 hours</span></span>            | <span data-ttu-id="9293d-177">7.2시간</span><span class="sxs-lookup"><span data-stu-id="9293d-177">7.2 hours</span></span>              | <span data-ttu-id="9293d-178">3.65일</span><span class="sxs-lookup"><span data-stu-id="9293d-178">3.65 days</span></span>             |
| <span data-ttu-id="9293d-179">99.9%</span><span class="sxs-lookup"><span data-stu-id="9293d-179">99.9%</span></span>   | <span data-ttu-id="9293d-180">10.1분</span><span class="sxs-lookup"><span data-stu-id="9293d-180">10.1 minutes</span></span>          | <span data-ttu-id="9293d-181">43.2분</span><span class="sxs-lookup"><span data-stu-id="9293d-181">43.2 minutes</span></span>           | <span data-ttu-id="9293d-182">8.76시간</span><span class="sxs-lookup"><span data-stu-id="9293d-182">8.76 hours</span></span>            |
| <span data-ttu-id="9293d-183">99.95%</span><span class="sxs-lookup"><span data-stu-id="9293d-183">99.95%</span></span>  | <span data-ttu-id="9293d-184">5분</span><span class="sxs-lookup"><span data-stu-id="9293d-184">5 minutes</span></span>             | <span data-ttu-id="9293d-185">21.6분</span><span class="sxs-lookup"><span data-stu-id="9293d-185">21.6 minutes</span></span>           | <span data-ttu-id="9293d-186">4.38시간</span><span class="sxs-lookup"><span data-stu-id="9293d-186">4.38 hours</span></span>            |
| <span data-ttu-id="9293d-187">99.99%</span><span class="sxs-lookup"><span data-stu-id="9293d-187">99.99%</span></span>  | <span data-ttu-id="9293d-188">1.01분</span><span class="sxs-lookup"><span data-stu-id="9293d-188">1.01 minutes</span></span>          | <span data-ttu-id="9293d-189">4.32분</span><span class="sxs-lookup"><span data-stu-id="9293d-189">4.32 minutes</span></span>           | <span data-ttu-id="9293d-190">52.56분</span><span class="sxs-lookup"><span data-stu-id="9293d-190">52.56 minutes</span></span>         |
| <span data-ttu-id="9293d-191">99.999%</span><span class="sxs-lookup"><span data-stu-id="9293d-191">99.999%</span></span> | <span data-ttu-id="9293d-192">6초</span><span class="sxs-lookup"><span data-stu-id="9293d-192">6 seconds</span></span>             | <span data-ttu-id="9293d-193">25.9초</span><span class="sxs-lookup"><span data-stu-id="9293d-193">25.9 seconds</span></span>           | <span data-ttu-id="9293d-194">5.26분</span><span class="sxs-lookup"><span data-stu-id="9293d-194">5.26 minutes</span></span>          |

## <a name="understand-service-level-agreements"></a><span data-ttu-id="9293d-195">서비스 수준 계약을 이해</span><span class="sxs-lookup"><span data-stu-id="9293d-195">Understand service-level agreements</span></span>

<span data-ttu-id="9293d-196">Azure에서의 [서비스 수준 계약](https://azure.microsoft.com/en-us/support/legal/sla/) 작동 시간 및 연결에 대 한 Microsoft의 약속을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-196">In Azure, the [Service Level Agreement](https://azure.microsoft.com/en-us/support/legal/sla/) describes Microsoft's commitments for uptime and connectivity.</span></span> <span data-ttu-id="9293d-197">특정 서비스에 대 한 SLA는 99.9% 인 경우에 99.9%의 시간 동안 사용 가능 하도록 서비스를 하시면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-197">If the SLA for a particular service is 99.9%, you should expect the service to be available 99.9% of the time.</span></span> <span data-ttu-id="9293d-198">다른 서비스에 다른 Sla.</span><span class="sxs-lookup"><span data-stu-id="9293d-198">Different services have different SLAs.</span></span>

<span data-ttu-id="9293d-199">Azure SLA에 또한 서비스 크레딧의 구체적인 정의와 함께 SLA가 충족 되지 않으면 가져오기에 대 한 프로 비전 *가용성* 각 서비스에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-199">The Azure SLA also includes provisions for obtaining a service credit if the SLA is not met, along with specific definitions of *availability* for each service.</span></span> <span data-ttu-id="9293d-200">SLA의 이러한 측면은 적용 정책의 역할을 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-200">That aspect of the SLA acts as an enforcement policy.</span></span>

### <a name="composite-slas"></a><span data-ttu-id="9293d-201">복합 SLA</span><span class="sxs-lookup"><span data-stu-id="9293d-201">Composite SLAs</span></span>

<span data-ttu-id="9293d-202">*복합 Sla* 서로 다른 수준의 가용성을 사용 하 여 각 응용 프로그램을 지 원하는 여러 서비스가 관련 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-202">*Composite SLAs* involve multiple services supporting an application, each with differing levels of availability.</span></span> <span data-ttu-id="9293d-203">Azure SQL Database에 쓰는 App Service 웹 앱을 예로 들어 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-203">For example, consider an App Service web app that writes to Azure SQL Database.</span></span> <span data-ttu-id="9293d-204">이 문서가 작성된 시점에 이러한 Azure 서비스의 SLA는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-204">At the time of this writing, these Azure services have the following SLAs:</span></span>

- <span data-ttu-id="9293d-205">App Service 웹 앱 = 99.95%</span><span class="sxs-lookup"><span data-stu-id="9293d-205">App Service web apps = 99.95%</span></span>
- <span data-ttu-id="9293d-206">SQL Database = 99.99%</span><span class="sxs-lookup"><span data-stu-id="9293d-206">SQL Database = 99.99%</span></span>

<span data-ttu-id="9293d-207">이 애플리케이션에 기대하는 최대 가동 중지 시간이 얼마입니까?</span><span class="sxs-lookup"><span data-stu-id="9293d-207">What is the maximum downtime you would expect for this application?</span></span> <span data-ttu-id="9293d-208">서비스 중 하나가 중단되면 전체 애플리케이션이 중단됩니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-208">If either service fails, the whole application fails.</span></span> <span data-ttu-id="9293d-209">각 서비스 실패의 확률은 독립적 이므로이 응용 프로그램에 대 한 복합 SLA는 99.95% × 99.99% = 99.94%입니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-209">The probability of each service failing is independent, so the composite SLA for this application is 99.95% × 99.99% = 99.94%.</span></span> <span data-ttu-id="9293d-210">여러 서비스를 사용 하는 응용 프로그램은 잠재적 오류 지점이 더 때문에 놀라운 아닌 개별 Sla 보다 작습니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-210">That's lower than the individual SLAs, which isn't surprising because an application that relies on multiple services has more potential failure points.</span></span>

<span data-ttu-id="9293d-211">독립적인 대체 경로 만들어서 복합 SLA를 높일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-211">You can improve the composite SLA by creating independent fallback paths.</span></span> <span data-ttu-id="9293d-212">예를 들어 SQL Database를 사용할 수 없는 트랜잭션을 나중에 처리 되도록 큐에 배치 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-212">For example, if SQL Database is unavailable, put transactions into a queue to be processed later.</span></span>

![복합 SLA](_images/composite-sla.png)

<span data-ttu-id="9293d-214">이 디자인에서는 애플리케이션이 데이터베이스에 연결할 수 없는 경우에도 계속 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-214">With this design, the application is still available even if it can't connect to the database.</span></span> <span data-ttu-id="9293d-215">그러나 데이터베이스와 큐가 동시에 중단되면 응용 프로그램도 중단됩니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-215">However, it fails if the database and the queue both fail at the same time.</span></span> <span data-ttu-id="9293d-216">예상 되는 동시에 오류에 대 한 시간 비율은 0.0001 × 0.001 이므로이 복합 경로의 복합 SLA는:</span><span class="sxs-lookup"><span data-stu-id="9293d-216">The expected percentage of time for a simultaneous failure is 0.0001 × 0.001, so the composite SLA for this combined path is:</span></span>

- <span data-ttu-id="9293d-217">데이터베이스 *또는* 큐 = 1.0 − (0.0001 × 0.001) = 99.99999%</span><span class="sxs-lookup"><span data-stu-id="9293d-217">Database *or* queue = 1.0 − (0.0001 × 0.001) = 99.99999%</span></span>

<span data-ttu-id="9293d-218">총 복합 SLA는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-218">The total composite SLA is:</span></span>

- <span data-ttu-id="9293d-219">웹 앱 *하 고* (데이터베이스 *하거나* 큐) = 99.95% × 99.99999% = \~99.95%</span><span class="sxs-lookup"><span data-stu-id="9293d-219">Web app *and* (database *or* queue) = 99.95% × 99.99999% = \~99.95%</span></span>

<span data-ttu-id="9293d-220">도이 방식의 장단점이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-220">There are tradeoffs to this approach.</span></span> <span data-ttu-id="9293d-221">응용 프로그램 논리는 복잡 하 고 큐에 대해 지불 하는 데이터 일관성 문제를 고려해 야 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-221">The application logic is more complex, you are paying for the queue, and you need to consider data consistency issues.</span></span>

### <a name="slas-for-multiregion-deployments"></a><span data-ttu-id="9293d-222">다중 지역 배포에 대 한 Sla</span><span class="sxs-lookup"><span data-stu-id="9293d-222">SLAs for multiregion deployments</span></span>

<span data-ttu-id="9293d-223">*다중 지역 배포에 대 한 Sla* 둘 이상의 지역에서 응용 프로그램을 배포 하 고 장애 조치 한 지역의 응용 프로그램이 실패 하는 경우 Azure Traffic Manager를 사용 하는 고가용성 기술을 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-223">*SLAs for multiregion deployments* involve a high-availability technique to deploy the application in more than one region and use Azure Traffic Manager to fail over if the application fails in one region.</span></span>

<span data-ttu-id="9293d-224">다중 지역 배포의 복합 SLA는 다음과 같이 계산 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-224">The composite SLA for a multiregion deployment is calculated as follows:</span></span>

- <span data-ttu-id="9293d-225">*N* 한 지역에서 배포 응용 프로그램에 대 한 복합 SLA는 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-225">*N* is the composite SLA for the application deployed in one region.</span></span>
- <span data-ttu-id="9293d-226">*R* 응용 프로그램이 배포 되는 지역입니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-226">*R* is the number of regions where the application is deployed.</span></span>

<span data-ttu-id="9293d-227">응용 프로그램이 모든 지역에서 동시에 실패 하는 예상된 가능성은 ((1 − N) \^ R).</span><span class="sxs-lookup"><span data-stu-id="9293d-227">The expected chance that the application fails in all regions at the same time is ((1 − N) \^ R).</span></span> <span data-ttu-id="9293d-228">예를 들어, 단일 지역 SLA가 99.95%입니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-228">For example, if the single-region SLA is 99.95%:</span></span>

- <span data-ttu-id="9293d-229">두 지역에 대 한 결합 된 SLA = ((1 − 0.9995) 1 − \^ 2) = 99.999975%</span><span class="sxs-lookup"><span data-stu-id="9293d-229">The combined SLA for two regions = (1 − (1 − 0.9995) \^ 2) = 99.999975%</span></span>
- <span data-ttu-id="9293d-230">4 개 지역에 대 한 결합 된 SLA = ((1 − 0.9995) 1 − \^ 4) 99.999999% =</span><span class="sxs-lookup"><span data-stu-id="9293d-230">The combined SLA for four regions =  (1 − (1 − 0.9995) \^ 4)  = 99.999999%</span></span>

<span data-ttu-id="9293d-231">합니다 [Traffic Manager SLA](https://azure.microsoft.com/en-us/support/legal/sla/traffic-manager/v1_0/) 요소 이기도 합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-231">The [SLA for Traffic Manager](https://azure.microsoft.com/en-us/support/legal/sla/traffic-manager/v1_0/) is also a factor.</span></span> <span data-ttu-id="9293d-232">장애 조치 하는 동안 가동 중지 시간이 발생할 수 있는 활성-수동 구성에서 장애 조치 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-232">Failing over is not instantaneous in active-passive configurations, which can result in downtime during a failover.</span></span> <span data-ttu-id="9293d-233">참조 [Traffic Manager 끝점 모니터링 및 장애 조치](/azure/traffic-manager/traffic-manager-monitoring)합니다.</span><span class="sxs-lookup"><span data-stu-id="9293d-233">See [Traffic Manager endpoint monitoring and failover](/azure/traffic-manager/traffic-manager-monitoring).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9293d-234">다음 단계</span><span class="sxs-lookup"><span data-stu-id="9293d-234">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="9293d-235">복원 력 및 가용성에 대 한 설계자</span><span class="sxs-lookup"><span data-stu-id="9293d-235">Architect for resiliency and availability</span></span>](./architect.md)