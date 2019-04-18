---
title: 복원 력 및 가용성을 위해 Azure 응용 프로그램 테스트
description: Azure에서 응용 프로그램의 안정성을 테스트 하는 방법
author: MikeWasson
ms.date: 04/10/2019
ms.topic: article
ms.service: architecture-center
ms.subservice: cloud-design-principles
ms.openlocfilehash: cf33a859540810279b1cb85be5a6fb2ebe4500dc
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59646641"
---
# <a name="testing-azure-applications-for-resiliency-and-availability"></a><span data-ttu-id="913f3-103">복원 력 및 가용성을 위해 Azure 응용 프로그램 테스트</span><span class="sxs-lookup"><span data-stu-id="913f3-103">Testing Azure applications for resiliency and availability</span></span>

<span data-ttu-id="913f3-104">복원 력 테스트를 엔드-투-엔드 워크 로드를 일시적인 오류 조건에서 수행 하는 방법을 확인 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-104">To test resiliency, you should verify how the end-to-end workload performs under intermittent failure conditions.</span></span>

<span data-ttu-id="913f3-105">가상 및 실제 사용자 데이터 모두를 사용 하 여 프로덕션에서 테스트를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-105">Run tests in production using both synthetic and real user data.</span></span> <span data-ttu-id="913f3-106">테스트 및 프로덕션은 거의 동일 하므로 사용 하 여 프로덕션에서 응용 프로그램의 유효성을 검사 해야는 [청록색](https://martinfowler.com/bliki/BlueGreenDeployment.html) 하거나 [카나리아 배포](https://martinfowler.com/bliki/CanaryRelease.html)합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-106">Test and production are rarely identical, so it's important to validate your application in production using a [blue-green](https://martinfowler.com/bliki/BlueGreenDeployment.html) or [canary deployment](https://martinfowler.com/bliki/CanaryRelease.html).</span></span> <span data-ttu-id="913f3-107">이러한 방식으로 테스트 하는 실제 상황에서는 응용 프로그램이 완벽 하 게 배포 하는 경우 예상 대로 작동 하는지 확인할 수 있도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-107">This way, you're testing the application under real conditions, so you can be sure that it will function as expected when fully deployed.</span></span>

<span data-ttu-id="913f3-108">테스트 계획의 일환으로, 다음을 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-108">As part of your test plan, include:</span></span>

- <span data-ttu-id="913f3-109">자동화 된 배포 전 테스트</span><span class="sxs-lookup"><span data-stu-id="913f3-109">Automated predeployment testing</span></span>
- <span data-ttu-id="913f3-110">오류 주입 테스트</span><span class="sxs-lookup"><span data-stu-id="913f3-110">Fault injection testing</span></span>
- <span data-ttu-id="913f3-111">최대 부하 테스트</span><span class="sxs-lookup"><span data-stu-id="913f3-111">Peak load testing</span></span>
- <span data-ttu-id="913f3-112">재해 복구 테스트</span><span class="sxs-lookup"><span data-stu-id="913f3-112">Disaster recovery testing</span></span>
- <span data-ttu-id="913f3-113">타사 서비스 테스트</span><span class="sxs-lookup"><span data-stu-id="913f3-113">Third-party service testing</span></span>

<span data-ttu-id="913f3-114">테스트는 반복적인 프로세스입니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-114">Testing is an iterative process.</span></span> <span data-ttu-id="913f3-115">애플리케이션을 테스트하고, 결과를 측정하고, 결과를 분석 및 해결하고, 다시 프로세스를 반복합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-115">Test the application, measure the outcome, analyze and address any failures that result, and repeat the process.</span></span>

## <a name="perform-fault-injection-testing"></a><span data-ttu-id="913f3-116">오류 주입 테스트 수행</span><span class="sxs-lookup"><span data-stu-id="913f3-116">Perform fault injection testing</span></span>

<span data-ttu-id="913f3-117">오류 주입 테스트 또는 실제 오류를 트리거하거나 시뮬레이션 하 여 시스템의 복원 력을 실패 하는 동안 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-117">For fault injection testing, check the resiliency of the system during failures, either by triggering actual failures or by simulating them.</span></span> <span data-ttu-id="913f3-118">다음은 오류를 유도 하기 위한 몇 가지 전략입니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-118">Here are some strategies to induce failures:</span></span>

- <span data-ttu-id="913f3-119">가상 머신 (VM) 인스턴스를 종료 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-119">Shut down virtual machine (VM) instances.</span></span>
- <span data-ttu-id="913f3-120">프로세스가 충돌합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-120">Crash processes.</span></span>
- <span data-ttu-id="913f3-121">인증서가 만료됩니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-121">Expire certificates.</span></span>
- <span data-ttu-id="913f3-122">액세스 키가 변경됩니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-122">Change access keys.</span></span>
- <span data-ttu-id="913f3-123">도메인 컨트롤러에서 DNS 서비스를 중단합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-123">Shut down the DNS service on domain controllers.</span></span>
- <span data-ttu-id="913f3-124">RAM 또는 스레드 수 같은 가용 시스템 리소스를 제한합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-124">Limit available system resources, such as RAM or number of threads.</span></span>
- <span data-ttu-id="913f3-125">디스크를 탑재 해제합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-125">Unmount disks.</span></span>
- <span data-ttu-id="913f3-126">VM을 다시 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-126">Redeploy a VM.</span></span>

<span data-ttu-id="913f3-127">테스트 계획 일반적인 오류 시나리오 외에도 디자인 단계에서 식별 가능한 오류 지점을 통합 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-127">Your test plan should incorporate possible failure points identified during the design phase, in addition to common failure scenarios:</span></span>

- <span data-ttu-id="913f3-128">프로덕션 최대한 가까운 환경에서 응용 프로그램을 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-128">Test your application in an environment as close to production as possible.</span></span>
- <span data-ttu-id="913f3-129">조합 하 여 오류를 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-129">Test failures in combination.</span></span>
- <span data-ttu-id="913f3-130">복구 시간을 측정 하 고 비즈니스 요구 사항을 충족 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-130">Measure the recovery times, and be sure that your business requirements are met.</span></span>
- <span data-ttu-id="913f3-131">확인 오류 계단식으로 배열 되지 한 격리 된 방식으로 처리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-131">Verify that failures don't cascade and are handled in an isolated way.</span></span>

<span data-ttu-id="913f3-132">오류 시나리오에 대 한 자세한 내용은 참조 하세요. [Azure 응용 프로그램에 대 한 오류 및 재해 복구](./disaster-recovery.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-132">For more information about failure scenarios, see [Failure and disaster recovery for Azure applications](./disaster-recovery.md).</span></span>

## <a name="test-under-peak-loads"></a><span data-ttu-id="913f3-133">최대 부하 테스트</span><span class="sxs-lookup"><span data-stu-id="913f3-133">Test under peak loads</span></span>

<span data-ttu-id="913f3-134">부하 테스트 하는 것은 중요 서비스 제한 또는 부하가 과부하가 백 엔드 데이터베이스와 같은 때에 발생 하는 오류를 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-134">Load testing is crucial for identifying failures that only happen under load, such as the back-end database being overwhelmed or service throttling.</span></span> <span data-ttu-id="913f3-135">최대 부하 및 최대 부하를 프로덕션 데이터 또는 프로덕션 데이터와 최대한 가까운 가상 데이터를 사용 하 여 예상 된 증가 대 한 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-135">Test for peak load and anticipated increase in peak load, using production data or synthetic data that is as close to production data as possible.</span></span> <span data-ttu-id="913f3-136">목표는 응용 프로그램이 실제 조건에서 어떻게 동작 하는지 확인 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-136">Your goal is to see how the application behaves under real-world conditions.</span></span>

## <a name="conduct-disaster-recovery-drills"></a><span data-ttu-id="913f3-137">재해 복구 훈련 수행</span><span class="sxs-lookup"><span data-stu-id="913f3-137">Conduct disaster recovery drills</span></span>

<span data-ttu-id="913f3-138">좋은 재해 복구 계획을 시작 하 고 작동 하는지 정기적으로 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-138">Start with a good disaster recovery plan, and test it periodically to make sure it works.</span></span>

<span data-ttu-id="913f3-139">Azure Virtual Machines에 대해 사용할 수 [Azure Site Recovery](/azure/site-recovery/azure-to-azure-quickstart/) 복제를 수행할 [재해 복구 훈련](/azure/site-recovery/azure-to-azure-tutorial-dr-drill/) 프로덕션 응용 프로그램 또는 진행 중인 복제에 영향을 주지 않고 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-139">For Azure Virtual Machines, you can use [Azure Site Recovery](/azure/site-recovery/azure-to-azure-quickstart/) to replicate and perform [disaster recovery drills](/azure/site-recovery/azure-to-azure-tutorial-dr-drill/) without impacting production applications or ongoing replication.</span></span>

### <a name="failover-and-failback-testing"></a><span data-ttu-id="913f3-140">장애 조치 및 장애 복구 테스트</span><span class="sxs-lookup"><span data-stu-id="913f3-140">Failover and failback testing</span></span>

<span data-ttu-id="913f3-141">장애 조치 및 장애 복구는 응용 프로그램의 종속 서비스 돌아와야 동기화 된 방식으로 재해 복구 하는 동안 확인을 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-141">Test failover and failback to verify that your application's dependent services come back up in a synchronized manner during disaster recovery.</span></span> <span data-ttu-id="913f3-142">시스템 및 작업 변경에는 장애 조치 및 장애 복구 기능을 영향을 줄 수 있지만 주 시스템이 실패 하거나 과부하 될 때까지 영향이 검색 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-142">Changes to systems and operations may affect failover and failback functions, but the impact may not be detected until the main system fails or becomes overloaded.</span></span> <span data-ttu-id="913f3-143">장애 조치 기능을 테스트 *하기 전에* 라이브 문제에 대 한 보정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-143">Test failover capabilities *before* they are required to compensate for a live problem.</span></span> <span data-ttu-id="913f3-144">또한 종속 서비스가 장애 조치 하 고 올바른 순서로 실패 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-144">Also be sure that dependent services fail over and fail back in the correct order.</span></span>

<span data-ttu-id="913f3-145">사용 중인 경우 [Azure Site Recovery](/azure/site-recovery/) Vm을 복제, 재해 복구 훈련을 실행할 정기적으로 테스트 장애 조치 복제 전략의 유효성을 검사를 수행 하 여 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-145">If you are using [Azure Site Recovery](/azure/site-recovery/) to replicate VMs, run disaster recovery drills periodically by doing test failovers to validate your replication strategy.</span></span> <span data-ttu-id="913f3-146">진행 중인 VM 복제 또는 프로덕션 환경에는 테스트 장애 조치를 적용 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-146">A test failover does not affect the ongoing VM replication or your production environment.</span></span> <span data-ttu-id="913f3-147">자세한 내용은 [Azure로 재해 복구 훈련 실행](/azure/site-recovery/site-recovery-test-failover-to-azure)합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-147">For more information, see [Run a disaster recovery drill to Azure](/azure/site-recovery/site-recovery-test-failover-to-azure).</span></span>

### <a name="simulation-testing"></a><span data-ttu-id="913f3-148">시뮬레이션 테스트</span><span class="sxs-lookup"><span data-stu-id="913f3-148">Simulation testing</span></span>

<span data-ttu-id="913f3-149">시뮬레이션 테스트에서는 소규모의 실제 상황을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-149">Simulation testing involves creating small, real-life situations.</span></span> <span data-ttu-id="913f3-150">시뮬레이션에는 적절 하 게 처리 되지 않은 모든 문제를 강조 표시 및 복구 계획에 솔루션의 효과 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-150">Simulations demonstrate the effectiveness of the solutions in the recovery plan and highlight any issues that weren't adequately addressed.</span></span>

<span data-ttu-id="913f3-151">시뮬레이션 테스트를 수행 하면 모범 사례를 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-151">As you perform simulation testing, follow best practices:</span></span>

- <span data-ttu-id="913f3-152">실제 비즈니스를 중단 하지 않습니다 하지만 실제 상황을 찾기란 하는 방식으로 시뮬레이션을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-152">Conduct simulations in a manner that doesn't disrupt actual business but feels like a real situation.</span></span>
- <span data-ttu-id="913f3-153">시뮬레이션 된 시나리오는 완전히 제어할 수 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-153">Make sure that simulated scenarios are completely controllable.</span></span> <span data-ttu-id="913f3-154">복구 계획 실패 한 것 처럼 보이지만, 하는 경우 손상 없이 상황을 정상 상태로 복원할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-154">If the recovery plan seems to be failing, you can restore the situation back to normal without causing damage.</span></span>
- <span data-ttu-id="913f3-155">시뮬레이션 연습을 수행할 시기와 방법에 대 한 관리를 알립니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-155">Inform management about when and how the simulation exercises will be conducted.</span></span> <span data-ttu-id="913f3-156">계획에는 시간 프레임 및 시뮬레이션 중 영향을 받는 리소스를 자세히 설명 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-156">Your plan should detail the time frame and the resources affected during the simulation.</span></span>

## <a name="test-health-probes-for-load-balancers-and-traffic-managers"></a><span data-ttu-id="913f3-157">부하 분산 장치 및 traffic manager에 대 한 상태 프로브를 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-157">Test health probes for load balancers and traffic managers</span></span>

<span data-ttu-id="913f3-158">구성 하 고 부하 분산 장치 및 traffic manager 상태 프로브를 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-158">Configure and test health probes for your load balancers and traffic managers.</span></span> <span data-ttu-id="913f3-159">상태 끝점 시스템의 중요 한 부분을 확인 하 고 적절 하 게 응답을 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-159">Ensure that your health endpoint checks the critical parts of the system and responds appropriately.</span></span>

- <span data-ttu-id="913f3-160">에 대 한 [Azure Traffic Manager](/azure/traffic-manager/traffic-manager-overview/), 상태 프로브를 다른 지역으로 장애 조치할 지 여부를 결정 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-160">For [Azure Traffic Manager](/azure/traffic-manager/traffic-manager-overview/), the health probe determines whether to fail over to another region.</span></span> <span data-ttu-id="913f3-161">상태 끝점에는 동일한 지역 내에서 배포 되는 모든 중요 한 종속성을 확인 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-161">Your health endpoint should check any critical dependencies that are deployed within the same region.</span></span>
- <span data-ttu-id="913f3-162">에 대 한 [Azure Load Balancer](/azure/load-balancer/load-balancer-overview/), 상태 프로브 VM을 윤 번에서 제거할지 여부를 결정 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-162">For [Azure Load Balancer](/azure/load-balancer/load-balancer-overview/), the health probe determines whether to remove a VM from rotation.</span></span> <span data-ttu-id="913f3-163">상태 끝점은 VM의 상태를 보고 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-163">The health endpoint should report the health of the VM.</span></span> <span data-ttu-id="913f3-164">다른 계층 또는 외부 서비스를 포함하지 마세요.</span><span class="sxs-lookup"><span data-stu-id="913f3-164">Don't include other tiers or external services.</span></span> <span data-ttu-id="913f3-165">그렇지 않으면 VM 외부에서 발생하는 실패 때문에 부하 분산 장치가 VM을 윤번에서 제거하게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-165">Otherwise, a failure that occurs outside the VM will cause the load balancer to remove the VM from rotation.</span></span>

<span data-ttu-id="913f3-166">애플리케이션의 상태 모니터링 구현에 대한 지침은 [상태 엔드포인트 모니터링 패턴](../patterns/health-endpoint-monitoring.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="913f3-166">For guidance on implementing health monitoring in your application, see [Health Endpoint Monitoring pattern](../patterns/health-endpoint-monitoring.md).</span></span>

## <a name="test-monitoring-systems"></a><span data-ttu-id="913f3-167">모니터링 시스템 테스트</span><span class="sxs-lookup"><span data-stu-id="913f3-167">Test monitoring systems</span></span>

<span data-ttu-id="913f3-168">테스트 계획에서 시스템 모니터링이 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-168">Include monitoring systems in your test plan.</span></span> <span data-ttu-id="913f3-169">장애 조치 및 장애 복구에 대 한 자동화 된 시스템 모니터링 및 계측의 올바른 작동에 따라 달라 집니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-169">Automated failover and failback systems depend on the correct functioning of monitoring and instrumentation.</span></span> <span data-ttu-id="913f3-170">시스템 상태 및 연산자 경고를 시각화 하는 대시보드도 정확 하 게 모니터링 및 계측에 따라 달라 집니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-170">Dashboards to visualize system health and operator alerts also depend on having accurate monitoring and instrumentation.</span></span> <span data-ttu-id="913f3-171">이러한 요소에 오류가 발생하고 중요한 정보를 누락되거나 부정확한 데이터가 보고된 경우 운영자는 시스템이 비정상 또는 실패임을 모를 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-171">If these elements fail, miss critical information, or report inaccurate data, an operator might not realize that the system is unhealthy or failing.</span></span>

## <a name="include-third-party-services-in-test-scenarios"></a><span data-ttu-id="913f3-172">테스트 시나리오에서 타사 서비스를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-172">Include third-party services in test scenarios</span></span>

<span data-ttu-id="913f3-173">응용 프로그램이 타사 서비스에 의존 하는 경우 위치를 식별 하 고 이러한 서비스가 실패할 수 있습니다 이러한 오류를 모르겠으면 방법과 응용 프로그램에 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-173">If your application has dependencies on third-party services, identify where and how these services can fail and what effect those failures will have on your application.</span></span> <span data-ttu-id="913f3-174">항상 염두에서에 서비스 수준 계약 (SLA) 제 3 자 서비스 및 효과 대 한 재해 복구 계획을 가질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-174">Keep in mind the service-level agreement (SLA) for the third-party service and the effect it might have on your disaster recovery plan.</span></span>

<span data-ttu-id="913f3-175">하지는 타사 서비스는 그 중에 호출을 기록 하 고 응용 프로그램의 상태를 사용 하 여 상관 관계 및 고유 식별자를 사용 하 여 로깅을 진단 되므로 모니터링 및 진단 기능을 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-175">A third-party service might not provide monitoring and diagnostics capabilities, so it's important to log your invocations of them and to correlate them with your application's health and diagnostic logging using a unique identifier.</span></span> <span data-ttu-id="913f3-176">모니터링 및 진단에 대 한 검증 된 사례에 대 한 자세한 내용은 참조 하세요. [모니터링 및 진단 지침](../best-practices/monitoring.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="913f3-176">For more information on proven practices for monitoring and diagnostics, see [Monitoring and diagnostics guidance](../best-practices/monitoring.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="913f3-177">다음 단계</span><span class="sxs-lookup"><span data-stu-id="913f3-177">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="913f3-178">복원 력 및 가용성에 대 한 배포</span><span class="sxs-lookup"><span data-stu-id="913f3-178">Deploy for resiliency and availability</span></span>](./deploy.md)
