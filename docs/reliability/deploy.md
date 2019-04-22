---
title: 복원 력 및 가용성을 위해 Azure 응용 프로그램 배포
description: Azure에서 신뢰할 수 있는 배포를 만들기 위한 기술 개요
author: MikeWasson
ms.date: 04/10/2019
ms.topic: article
ms.service: architecture-center
ms.subservice: cloud-design-principles
ms.openlocfilehash: d8df3fe1c3353c60462959a625ba13ae47b96cf8
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59646644"
---
# <a name="deploying-azure-applications-for-resiliency-and-availability"></a><span data-ttu-id="3a0ac-103">복원 력 및 가용성을 위해 Azure 응용 프로그램 배포</span><span class="sxs-lookup"><span data-stu-id="3a0ac-103">Deploying Azure applications for resiliency and availability</span></span>

<span data-ttu-id="3a0ac-104">프로 비전 하 고 Azure 리소스, 응용 프로그램 코드 및 구성 설정을 업데이트 하는 대로 오류 및 가동 중지 시간을 방지 하는 반복 가능 하 고 예측 가능한 프로세스가 도와줍니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-104">As you provision and update Azure resources, application code, and configuration settings, a repeatable and predictable process will help you avoid errors and downtime.</span></span> <span data-ttu-id="3a0ac-105">요청 시 실행 하 고 실패 하면 다시 실행할 수 있는 배포에 대 한 자동화 된 프로세스를 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-105">We recommend automated processes for deployment that you can run on demand and rerun if something fails.</span></span> <span data-ttu-id="3a0ac-106">배포 프로세스에 원활 하 게 실행 된 후 프로세스 문서 수 유지 이렇게 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-106">After your deployment processes are running smoothly, process documentation can keep them that way.</span></span>

## <a name="automate-processes"></a><span data-ttu-id="3a0ac-107">프로세스 자동화</span><span class="sxs-lookup"><span data-stu-id="3a0ac-107">Automate processes</span></span>

<span data-ttu-id="3a0ac-108">리소스를 요청 시 활성화, 솔루션을 신속 하 게 배포할 인적 오류를 최소화 및 일관성 있고 반복 가능한 결과 생성, 배포 및 업데이트를 자동화 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-108">To activate resources on demand, deploy solutions rapidly, minimize human error, and produce consistent and repeatable results, be sure to automate deployments and updates.</span></span>

### <a name="automate-as-many-processes-as-possible"></a><span data-ttu-id="3a0ac-109">최대한 많은 프로세스를 자동화 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-109">Automate as many processes as possible</span></span>

<span data-ttu-id="3a0ac-110">가장 신뢰할 수 있는 배포 프로세스가 자동화 됩니다 및 *idempotent* &mdash; 즉, 동일한 결과 생성 하는 반복이 가능 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-110">The most reliable deployment processes are automated and *idempotent* &mdash; that is, repeatable to produce the same results.</span></span>

- <span data-ttu-id="3a0ac-111">Azure 리소스의 프로 비전을 자동화 하려면 사용할 수 있습니다 [Terraform](/azure/virtual-machines/windows/infrastructure-automation#terraform), [Ansible](/azure/virtual-machines/windows/infrastructure-automation#ansible), [Chef](/azure/virtual-machines/windows/infrastructure-automation#chef)하십시오 [Puppet](/azure/virtual-machines/windows/infrastructure-automation#puppet), [Azure PowerShell](/powershell/azure/overview)하십시오 [Azure 명령줄 인터페이스 (CLI)](/cli/azure), 또는 [Azure Resource Manager 템플릿](/azure/azure-resource-manager/resource-group-overview#template-deployment)합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-111">To automate provisioning of Azure resources, you can use [Terraform](/azure/virtual-machines/windows/infrastructure-automation#terraform),   [Ansible](/azure/virtual-machines/windows/infrastructure-automation#ansible), [Chef](/azure/virtual-machines/windows/infrastructure-automation#chef), [Puppet](/azure/virtual-machines/windows/infrastructure-automation#puppet),   [Azure PowerShell](/powershell/azure/overview), [Azure command-line interface (CLI)](/cli/azure), or [Azure Resource Manager templates](/azure/azure-resource-manager/resource-group-overview#template-deployment).</span></span>
- <span data-ttu-id="3a0ac-112">Vm을 구성 하려면 사용할 수 있습니다 [에서 cloud-init](/azure/virtual-machines/windows/infrastructure-automation#cloud-init) (의 Linux Vm) 또는 [Azure Automation 상태 구성](/azure/automation/automation-dsc-overview) (DSC).</span><span class="sxs-lookup"><span data-stu-id="3a0ac-112">To configure VMs, you can use [cloud-init](/azure/virtual-machines/windows/infrastructure-automation#cloud-init) (for Linux VMs) or [Azure Automation State Configuration](/azure/automation/automation-dsc-overview) (DSC).</span></span>
- <span data-ttu-id="3a0ac-113">응용 프로그램 배포를 자동화 하기 위해 사용할 수 있습니다 [Azure DevOps 서비스](/azure/virtual-machines/windows/infrastructure-automation#azure-devops-services)하십시오 [Jenkins](/azure/virtual-machines/windows/infrastructure-automation#jenkins), 또는 다른 CI/CD 솔루션입니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-113">To automate application deployment, you can use [Azure DevOps Services](/azure/virtual-machines/windows/infrastructure-automation#azure-devops-services), [Jenkins](/azure/virtual-machines/windows/infrastructure-automation#jenkins), or other CI/CD solutions.</span></span>

<span data-ttu-id="3a0ac-114">모범 사례로, 매개 변수 및 스크립트 사용 예에 대 한 설명은 문서화 빠른 액세스를 위해 범주별로 분류 자동화 스크립트의 리포지토리를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-114">As a best practice, create a repository of categorized automation scripts for quick access, documented with explanations of parameters and examples of script use.</span></span> <span data-ttu-id="3a0ac-115">이 설명서를 Azure 배포를 사용 하 여 동기화 된 상태로 유지 하 고 리포지토리를 관리 하려면 기본 사용자를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-115">Keep this documentation in sync with your Azure deployments, and designate a primary person to manage the repository.</span></span>

<span data-ttu-id="3a0ac-116">Automation 스크립트는 재해 복구에 대 한 주문형 리소스를 활성화할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-116">Automation scripts can also activate resources on demand for disaster recovery.</span></span>

### <a name="use-code-to-provision-and-configure-infrastructure"></a><span data-ttu-id="3a0ac-117">프로 비전 하는 코드를 사용 하 고 인프라를 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-117">Use code to provision and configure infrastructure</span></span>

<span data-ttu-id="3a0ac-118">이 연습에서는 호출 *코드로 인프라* 선언적 방법 또는 명령적 방법 (또는 둘의 조합)을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-118">This practice, called *infrastructure as code,* may use a declarative approach or an imperative approach (or a combination of both).</span></span>

- <span data-ttu-id="3a0ac-119">[Resource Manager 템플릿](/azure/azure-resource-manager/resource-group-overview#template-deployment) 은 선언적 방법의 예입니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-119">[Resource Manager templates](/azure/azure-resource-manager/resource-group-overview#template-deployment) are an example of a declarative approach.</span></span>
- <span data-ttu-id="3a0ac-120">[PowerShell](/powershell/azure/overview) 스크립트는 명령적 방법의 예입니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-120">[PowerShell](/powershell/azure/overview) scripts are an example of an imperative approach.</span></span>

### <a name="practice-immutable-infrastructure"></a><span data-ttu-id="3a0ac-121">실제로 변경이 불가능 한 인프라</span><span class="sxs-lookup"><span data-stu-id="3a0ac-121">Practice immutable infrastructure</span></span>

<span data-ttu-id="3a0ac-122">즉, 하지 프로덕션에 배포 된 후 인프라를 수정 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-122">In other words, don't modify infrastructure after it's deployed to production.</span></span> <span data-ttu-id="3a0ac-123">임시 변경 적용 된 후 모를 수도 있습니다 정확 하 게 변경 된 사항, 시스템 문제를 해결 하기 어려울 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-123">After ad hoc changes have been applied, you might not know exactly what has changed, so it can be difficult to troubleshoot the system.</span></span>

### <a name="automate-and-test-deployment-and-maintenance-tasks"></a><span data-ttu-id="3a0ac-124">자동화 및 테스트 배포 및 유지 관리 작업</span><span class="sxs-lookup"><span data-stu-id="3a0ac-124">Automate and test deployment and maintenance tasks</span></span>

<span data-ttu-id="3a0ac-125">분산된 애플리케이션은 함께 작동해야 하는 여러 부분으로 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-125">Distributed applications consist of multiple parts that must work together.</span></span> <span data-ttu-id="3a0ac-126">배포의 업데이트 및 구성의 유효성을 검사 하 고 배포 프로세스를 자동화할 수 있는 스크립트와 같은 입증 된 메커니즘을 활용 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-126">Deployment should take advantage of proven mechanisms, such as scripts, that can update and validate configuration and automate the deployment process.</span></span> <span data-ttu-id="3a0ac-127">완벽 하 게 되도록 오류 작동 중단 시간이 추가로 발생 하지는 모든 프로세스를 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-127">Test all processes fully to ensure that errors don't cause additional downtime.</span></span>

### <a name="implement-deployment-security-measures"></a><span data-ttu-id="3a0ac-128">배포 보안 조치를 구현 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-128">Implement deployment security measures</span></span>

<span data-ttu-id="3a0ac-129">모든 배포 도구는 배포 된 응용 프로그램을 보호 하기 위해 보안 제한 사항을 통합 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-129">All deployment tools must incorporate security restrictions to protect the deployed application.</span></span> <span data-ttu-id="3a0ac-130">정의 및 배포 정책을 신중 하 게 적용 하 고 사용자의 개입의 필요성을 최소화 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-130">Define and enforce deployment policies carefully, and minimize the need for human intervention.</span></span>

## <a name="deploy-to-multiple-regions-and-instances"></a><span data-ttu-id="3a0ac-131">여러 지역 및 여러 인스턴스 배포</span><span class="sxs-lookup"><span data-stu-id="3a0ac-131">Deploy to multiple regions and instances</span></span>

<span data-ttu-id="3a0ac-132">서비스의 단일 인스턴스에 종속 된 응용 프로그램에 단일 실패 지점을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-132">An application that depends on a single instance of a service creates a single point of failure.</span></span> <span data-ttu-id="3a0ac-133">복원 력 및 확장성 향상을 위해 여러 인스턴스를 프로 비전 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-133">To improve resiliency and scalability, provision multiple instances.</span></span>

- <span data-ttu-id="3a0ac-134">[Azure App Service](/azure/app-service/app-service-value-prop-what-is/)의 경우 여러 인스턴스를 제공하는 [App Service 계획](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview/)을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-134">For [Azure App Service](/azure/app-service/app-service-value-prop-what-is/), select an [App Service plan](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview/) that offers multiple instances.</span></span>
- <span data-ttu-id="3a0ac-135">에 대 한 [Azure Cloud Services](/azure/cloud-services/cloud-services-choose-me)를 사용 하도록 역할의 각 구성 [인스턴스를 여러 개](/azure/cloud-services/cloud-services-choose-me/#scaling-and-management)합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-135">For [Azure Cloud Services](/azure/cloud-services/cloud-services-choose-me), configure each of your roles to use [multiple instances](/azure/cloud-services/cloud-services-choose-me/#scaling-and-management).</span></span>
- <span data-ttu-id="3a0ac-136">에 대 한 [Azure Virtual Machines](/azure/virtual-machines/virtual-machines-windows-about/?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), 아키텍처 둘 이상의 VM을 포함 하 고 각 VM에 포함 되어 있는지 확인 한 [가용성 집합](/azure/virtual-machines/virtual-machines-windows-manage-availability/)합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-136">For [Azure Virtual Machines](/azure/virtual-machines/virtual-machines-windows-about/?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), ensure that your architecture includes more than one VM and that each VM is included in an [availability set](/azure/virtual-machines/virtual-machines-windows-manage-availability/).</span></span>

### <a name="consider-deploying-across-multiple-regions"></a><span data-ttu-id="3a0ac-137">여러 지역에 걸쳐 배포 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-137">Consider deploying across multiple regions</span></span>

<span data-ttu-id="3a0ac-138">모든 배포 하는 것이 좋습니다 하지만 가장 중요 한 응용 프로그램 및 여러 지역에서 응용 프로그램 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-138">We recommend deploying all but the least critical applications and application services across multiple regions.</span></span> <span data-ttu-id="3a0ac-139">응용 프로그램을 단일 지역에 배포 하는 경우 드문 경우에 전체 지역이 사용할 수 없게 응용 프로그램도 사용할 수 없게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-139">If your application is deployed to a single region, in the rare event that the entire region becomes unavailable, the application will also be unavailable.</span></span>

<span data-ttu-id="3a0ac-140">단일 지역에 배포 하려는 경우에 보조 지역에 예기치 않은 오류에 대 한 응답으로 다시 배포 하려면 준비 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-140">If you choose to deploy to a single region, consider preparing to redeploy to a secondary region as a response to an unexpected failure.</span></span>

### <a name="redeploy-to-a-secondary-region"></a><span data-ttu-id="3a0ac-141">보조 지역에 다시 배포</span><span class="sxs-lookup"><span data-stu-id="3a0ac-141">Redeploy to a secondary region</span></span>

<span data-ttu-id="3a0ac-142">복제 하지 않고 단일, 주 지역의 응용 프로그램 및 데이터베이스를 실행 하는 경우 다른 지역에 다시 배포 하려면 복구 전략 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-142">If you run applications and databases in a single, primary region with no replication, your recovery strategy might be to redeploy to another region.</span></span> <span data-ttu-id="3a0ac-143">이 방법은 저렴 하지만 가장 긴 복구 시간을 허용할 수 있는 중요 하지 않은 응용 프로그램에 적합 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-143">This solution is affordable but most appropriate for non-critical applications that can tolerate longer recovery times.</span></span>

<span data-ttu-id="3a0ac-144">이 전략을 선택 하면 가능한 한 재배포 프로세스를 자동화 하 고 재해 응답 테스트에 다시 배포 시나리오를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-144">If you choose this strategy, automate the redeployment process as much as possible and include redeployment scenarios in your disaster response testing.</span></span>

<span data-ttu-id="3a0ac-145">재배포 프로세스를 자동화 하려면 사용을 고려 [Azure Site Recovery](/azure/site-recovery/)합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-145">To automate your redeployment process, consider using [Azure Site Recovery](/azure/site-recovery/).</span></span>

## <a name="use-staging-and-production-environments"></a><span data-ttu-id="3a0ac-146">스테이징 및 프로덕션 환경을 사용 하 여</span><span class="sxs-lookup"><span data-stu-id="3a0ac-146">Use staging and production environments</span></span>

<span data-ttu-id="3a0ac-147">스테이징 및 프로덕션 환경에 적절 하 게 사용을 사용 하 여 매우 제어 된 방식으로 프로덕션 환경에 업데이트를 푸시할 수 있으며 중단을 예기치 않은 배포 문제를 최소화 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-147">With good use of staging and production environments, you can push updates to the production environment in a highly controlled way and minimize disruption from unanticipated deployment issues.</span></span>

- <span data-ttu-id="3a0ac-148">[*청록색 배포*](https://martinfowler.com/bliki/BlueGreenDeployment.html) 라이브 응용 프로그램에서 분리 된 프로덕션 환경에 대 한 업데이트 배포를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-148">[*Blue-green deployment*](https://martinfowler.com/bliki/BlueGreenDeployment.html) involves deploying an update into a production environment that's separate from the live application.</span></span> <span data-ttu-id="3a0ac-149">배포의 유효성을 검사한 후에는 업데이트된 버전으로 트래픽 라우팅을 전환합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-149">After you validate the deployment, switch the traffic routing to the updated version.</span></span> <span data-ttu-id="3a0ac-150">작업을 수행 하는 한 가지 방법은 사용 하는 것은 [스테이징 슬롯](/azure/app-service/web-sites-staged-publishing) 프로덕션으로 전환 하기 전에 배포에 Azure App Service에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-150">One way to do this is to use the [staging slots](/azure/app-service/web-sites-staged-publishing) available in Azure App Service to stage a deployment before moving it to production.</span></span>
- <span data-ttu-id="3a0ac-151">[*카나리아 릴리스*](https://martinfowler.com/bliki/CanaryRelease.html) 청록색 배포와 비슷합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-151">[*Canary releases*](https://martinfowler.com/bliki/CanaryRelease.html) are similar to blue-green deployments.</span></span> <span data-ttu-id="3a0ac-152">업데이트 된 응용 프로그램에 모든 트래픽을 전환 하는 대신 새 배포로 트래픽의 작은 부분만을 라우팅합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-152">Instead of switching all traffic to the updated application, you route only a small portion of the traffic to the new deployment.</span></span> <span data-ttu-id="3a0ac-153">문제가 있으면 이전 배포를 되돌립니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-153">If there's a problem, revert to the old deployment.</span></span> <span data-ttu-id="3a0ac-154">그렇지 않은 경우 점진적으로 더 많은 트래픽을 새 버전으로 라우팅합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-154">If not, gradually route more traffic to the new version.</span></span> <span data-ttu-id="3a0ac-155">Azure App Service를 사용 하는 경우 프로덕션 기능에 카나리아 릴리스를 관리 하는 테스트를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-155">If you're using Azure App Service, you can use the Testing in production feature to manage a canary release.</span></span>

## <a name="create-a-rollback-plan-for-deployment-and-updates"></a><span data-ttu-id="3a0ac-156">배포 및 업데이트에 대 한 롤백 계획 만들기</span><span class="sxs-lookup"><span data-stu-id="3a0ac-156">Create a rollback plan for deployment and updates</span></span>

<span data-ttu-id="3a0ac-157">배포에 실패 하면 응용 프로그램이 사용할 수 없게 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-157">If a deployment fails, your application could become unavailable.</span></span> <span data-ttu-id="3a0ac-158">가동 중지 시간을 최소화 하려면 마지막으로 알려진 적합 한 버전으로 다시 이동 하도록 롤백 프로세스를 디자인 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-158">To minimize downtime, design a rollback process to go back to a last-known good version.</span></span> <span data-ttu-id="3a0ac-159">데이터베이스 및 응용 프로그램이 종속 다른 서비스에 변경 내용을 롤백하려면 전략을 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-159">Include a strategy to roll back changes to databases and any other services your app depends on.</span></span>

<span data-ttu-id="3a0ac-160">Azure App Service를 사용 하는 경우 마지막으로 알려진 좋은 사이트 슬롯을 설정 하 고 사용 하 여 웹 또는 API 앱 배포에서 롤백할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-160">If you're using Azure App Service, you can set up a last-known good site slot and use it to roll back from a web or API app deployment.</span></span>

## <a name="log-and-audit-deployments"></a><span data-ttu-id="3a0ac-161">로그 및 감사 배포</span><span class="sxs-lookup"><span data-stu-id="3a0ac-161">Log and audit deployments</span></span>

<span data-ttu-id="3a0ac-162">가능한 훨씬 버전 관련 정보를 캡처하려면 강력한 로깅 전략을 구현 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-162">To capture as much version-specific information as possible, implement a robust logging strategy.</span></span> <span data-ttu-id="3a0ac-163">스테이징 된 배포 기술에서 사용 하는 경우 프로덕션 환경에서 응용 프로그램의 둘 이상의 버전이 실행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-163">If you use staged deployment techniques, more than one version of your application will be running in production.</span></span> <span data-ttu-id="3a0ac-164">문제가 발생 하는 경우 버전이 충돌을 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-164">If a problem occurs, determine which version is causing it.</span></span>

## <a name="document-release-processes"></a><span data-ttu-id="3a0ac-165">문서 릴리스 프로세스</span><span class="sxs-lookup"><span data-stu-id="3a0ac-165">Document release processes</span></span>

<span data-ttu-id="3a0ac-166">자세한 릴리스 프로세스 문서가 없으면 운영자는 잘못 된 업데이트를 배포할 수 있습니다 또는 응용 프로그램에 대 한 설정을 부적절 하 게 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-166">Without detailed release process documentation, an operator might deploy a bad update or might improperly configure settings for your application.</span></span> <span data-ttu-id="3a0ac-167">릴리스 프로세스를 명확히 정의 및 문서화하고 전체 작업 팀에 사용할 수 있도록 했는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-167">Clearly define and document your release process, and ensure that it's available to the entire operations team.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3a0ac-168">다음 단계</span><span class="sxs-lookup"><span data-stu-id="3a0ac-168">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="3a0ac-169">안정성에 대 한 응용 프로그램 상태 모니터링</span><span class="sxs-lookup"><span data-stu-id="3a0ac-169">Monitor application health for reliability</span></span>](./monitoring.md)