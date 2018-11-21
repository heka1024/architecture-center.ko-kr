---
title: Azure DevOps를 사용한 CI/CD 파이프라인
description: Azure DevOps를 사용하여 Azure Web Apps에 .NET 앱을 빌드하고 릴리스합니다.
author: christianreddington
ms.date: 07/11/18
ms.openlocfilehash: 97f16b2d3d9c15bc6f5db6fad4c9d8097243ad3d
ms.sourcegitcommit: 0a31fad9b68d54e2858314ca5fe6cba6c6b95ae4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51610790"
---
# <a name="cicd-pipeline-with-azure-devops"></a><span data-ttu-id="ebafa-103">Azure DevOps를 사용한 CI/CD 파이프라인</span><span class="sxs-lookup"><span data-stu-id="ebafa-103">CI/CD pipeline with Azure DevOps</span></span>

<span data-ttu-id="ebafa-104">DevOps는 개발, 품질 보증 및 IT 운영의 통합입니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-104">DevOps is the integration of development, quality assurance, and IT operations.</span></span> <span data-ttu-id="ebafa-105">DevOps에는 소프트웨어를 제공하기 위한 통합 문화권 및 강력한 프로세스 집합이 모두 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-105">DevOps requires both unified culture and a strong set of processes for delivering software.</span></span>

<span data-ttu-id="ebafa-106">이 예제 시나리오에서는 개발 팀이 Azure DevOps를 사용하여 .NET 2계층 웹 응용 프로그램을 Azure App Service에 배포하는 방법을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-106">This example scenario demonstrates how development teams can use Azure DevOps to deploy a .NET two-tier web application to Azure App Service.</span></span> <span data-ttu-id="ebafa-107">웹 응용 프로그램은 Azure PaaS(Platform as a Service) 서비스에 따라 달라집니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-107">The Web Application depends on downstream Azure platform as a service (PaaS) services.</span></span> <span data-ttu-id="ebafa-108">또한 이 문서에서는 Azure PaaS를 사용하여 이러한 시나리오를 설계할 때 고려해야 할 몇 가지 사항에 대해서도 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-108">This document also points out some considerations that you should make when designing such a scenario using Azure PaaS.</span></span>

<span data-ttu-id="ebafa-109">CI(지속적인 통합) 및 CD(지속적인 배포)를 사용하는 최신 응용 프로그램 개발 방식을 채택하면 강력한 빌드, 테스트, 배포 및 모니터링 서비스를 통해 사용자에게 가치 전달을 가속화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-109">Adopting a modern approach to application development using Continuous Integration and Continuous Deployment (CI/CD), helps you to accelerate the delivery of value to your users through a robust build, test, deployment, and monitoring service.</span></span> <span data-ttu-id="ebafa-110">Azure DevOps 같은 플랫폼을 App Service 같은 Azure 서비스와 함께 사용하면 지원 인프라 관리 대신 시나리오 개발에 집중할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-110">By using a platform such as Azure DevOps along with Azure services such as App Service, organizations can focus on the development of their scenario rather than the management of the supporting infrastructure.</span></span>

## <a name="relevant-use-cases"></a><span data-ttu-id="ebafa-111">관련 사용 사례</span><span class="sxs-lookup"><span data-stu-id="ebafa-111">Relevant use cases</span></span>

<span data-ttu-id="ebafa-112">DevOps에 적합한 사용 사례는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-112">Consider DevOps for the following use cases:</span></span>

* <span data-ttu-id="ebafa-113">응용 프로그램 개발 및 개발 수명 주기 가속화</span><span class="sxs-lookup"><span data-stu-id="ebafa-113">Accelerating application development and development life cycles</span></span>
* <span data-ttu-id="ebafa-114">자동화된 빌드 및 릴리스 프로세스에 대한 품질 및 일관성 구축</span><span class="sxs-lookup"><span data-stu-id="ebafa-114">Building quality and consistency into an automated build and release process</span></span>

## <a name="architecture"></a><span data-ttu-id="ebafa-115">아키텍처</span><span class="sxs-lookup"><span data-stu-id="ebafa-115">Architecture</span></span>

![Azure DevOps 및 Azure App Service를 사용하는 DevOps 시나리오와 관련된 Azure 구성 요소에 대한 아키텍처 개요][architecture]

<span data-ttu-id="ebafa-117">이 시나리오에서는 Azure DevOps를 사용하는 .NET 웹 응용 프로그램에 대한 CI/CD 파이프라인을 다룹니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-117">This scenario covers a CI/CD pipeline for a .NET web application using Azure DevOps.</span></span> <span data-ttu-id="ebafa-118">시나리오를 통한 데이터 흐름은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-118">The data flows through the scenario as follows:</span></span>

1. <span data-ttu-id="ebafa-119">응용 프로그램 소스 코드를 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-119">Change application source code.</span></span>
2. <span data-ttu-id="ebafa-120">응용 프로그램 코드 및 Web Apps web.config 파일을 커밋합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-120">Commit application code and Web Apps web.config file.</span></span>
3. <span data-ttu-id="ebafa-121">지속적인 통합에서 응용 프로그램 빌드 및 단위 테스트를 트리거합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-121">Continuous integration triggers application build and unit tests.</span></span>
4. <span data-ttu-id="ebafa-122">지속적인 배포 트리거에서 *매개 변수화된 환경 특정 구성 값*을 사용하여 응용 프로그램 아티팩트의 배포를 오케스트레이션합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-122">Continuous deployment trigger orchestrates deployment of application artifacts *with environment-specific parameterized configuration values*.</span></span>
5. <span data-ttu-id="ebafa-123">Azure App Service에 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-123">Deployment to Azure App Service.</span></span>
6. <span data-ttu-id="ebafa-124">Azure Application Insights에서 상태, 성능 및 사용량 데이터를 수집합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-124">Azure Application Insights collects and analyzes health, performance, and usage data.</span></span>
7. <span data-ttu-id="ebafa-125">상태, 성능 및 사용량 정보를 검토합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-125">Review health, performance, and usage information.</span></span>

### <a name="components"></a><span data-ttu-id="ebafa-126">구성 요소</span><span class="sxs-lookup"><span data-stu-id="ebafa-126">Components</span></span>

* <span data-ttu-id="ebafa-127">[Azure DevOps][vsts]는 계획 및 프로젝트 관리부터 코드 관리, 지속적인 빌드 및 릴리스까지 모든 개발 수명 주기를 관리할 수 있는 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-127">[Azure DevOps][vsts] is a service for managing your development life cycle end-to-end &mdash; from planning and project management, to code management, and continuing to build and release.</span></span>
* <span data-ttu-id="ebafa-128">[Azure Web Apps][web-apps]는 웹 응용 프로그램, REST API 및 모바일 백 엔드를 호스팅하는 PaaS 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-128">[Azure Web Apps][web-apps] is a PaaS service for hosting web applications, REST APIs, and mobile back ends.</span></span> <span data-ttu-id="ebafa-129">이 문서에서는 .NET에 집중하고 있지만 몇 가지 추가 개발 플랫폼 옵션이 지원됩니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-129">While this article focuses on .NET, there are several additional development platform options supported.</span></span>
* <span data-ttu-id="ebafa-130">[Application Insights][application-insights]는 웹 개발자를 위해 여러 플랫폼에서 확장 가능한 자체 APM(Application Performance Management) 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-130">[Application Insights][application-insights] is a first-party, extensible Application Performance Management (APM) service for web developers on multiple platforms.</span></span>

### <a name="alternative-devops-tooling-options"></a><span data-ttu-id="ebafa-131">대체 DevOps 도구 옵션</span><span class="sxs-lookup"><span data-stu-id="ebafa-131">Alternative DevOps tooling options</span></span>

<span data-ttu-id="ebafa-132">이 문서는 Azure DevOps 중심이지만, [Team Foundation Server][team-foundation-server]를 온-프레미스 대체 도구로 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-132">While this article focuses on Azure DevOps, [Team Foundation Server][team-foundation-server] could be used as on-premises substitute.</span></span> <span data-ttu-id="ebafa-133">또는 [Jenkins][jenkins-on-azure]를 사용하는 오픈 소스 개발 파이프라인에 대한 기술 집합을 사용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-133">Alternatively, you could also use a set of technologies for an open source development pipeline using [Jenkins][jenkins-on-azure].</span></span>

<span data-ttu-id="ebafa-134">코드로써의 인프라(Infrastructure as Code) 관점에서, [Azure Resource Manager 템플릿][arm-templates]은 Azure DevOps 프로젝트의 일부로 포함되지만, [Terraform][terraform] 또는 [Chef][chef]를 고려할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-134">From an infrastructure-as-code perspective, [Azure Resource Manager Templates][arm-templates] are included as part of the Azure DevOps project, but you could consider [Terraform][terraform] or [Chef][chef].</span></span> <span data-ttu-id="ebafa-135">IaaS(Infrastructure as a Service) 기반 배포를 선호하고 구성 관리가 필요한 경우 [Azure Automation State Configuration][desired-state-configuration], [Ansible][ansible] 또는 [Chef][chef] 중 하나를 고려할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-135">If you prefer an infrastructure-as-a-service (IaaS)-based deployment and require configuration management, you could consider either [Azure Automation State Configuration][desired-state-configuration], [Ansible][ansible], or [Chef][chef].</span></span>

### <a name="alternatives-to-azure-web-apps"></a><span data-ttu-id="ebafa-136">Azure Web Apps의 대안</span><span class="sxs-lookup"><span data-stu-id="ebafa-136">Alternatives to Azure Web Apps</span></span>

<span data-ttu-id="ebafa-137">Azure Web Apps에 호스팅하는 대안은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-137">You could consider these alternatives to hosting in Azure Web Apps:</span></span>

* <span data-ttu-id="ebafa-138">[Azure Virtual Machines][compare-vm-hosting] &mdash; 높은 수준의 제어가 필요하거나 Web Apps에서 가능하지 않은 OS 구성 요소 및 서비스를 사용하는 워크로드의 경우(예: Windows GAC 또는 COM) 이 대안을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-138">[Azure Virtual Machines][compare-vm-hosting] &mdash; For workloads that require a high degree of control, or depend on OS components and services that are not possible with Web Apps (for example, the Windows GAC, or COM).</span></span>
* <span data-ttu-id="ebafa-139">[Service Fabric][service-fabric] &mdash; 워크로드 아키텍처가 높은 수준의 제어를 통해 클러스터 전체에 배포되고 실행되는 이점이 있는 분산된 구성 요소에 집중되는 경우에 좋은 옵션입니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-139">[Service Fabric][service-fabric] &mdash; a good option if the workload architecture is focused around distributed components that benefit from being deployed and run across a cluster with a high degree of control.</span></span> <span data-ttu-id="ebafa-140">Service Fabric은 컨테이너를 호스팅하는 데도 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-140">Service Fabric can also be used to host containers.</span></span>
* <span data-ttu-id="ebafa-141">[Azure Functions][azure-functions] - 워크로드 아키텍처가 세분화된 분산된 구성 요소에 집중되고, 최소한의 종속성이 필요하며, 개별 구성 요소가 요청 시에만 실행되고(불연속) 구성 요소의 오케스트레이션이 필요하지 않은 경우에 좋은 옵션입니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-141">[Azure Functions][azure-functions] - an effective serverless approach if the workload architecture is centered around fine grained distributed components, requiring minimal dependencies, where individual components are only required to run on demand (not continuously) and orchestration of components is not required.</span></span>

<span data-ttu-id="ebafa-142">이 [의사 결정 트리](/azure/architecture/guide/technology-choices/compute-decision-tree)는 올바른 마이그레이션 수행 경로를 선택할 때 도움이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-142">This [decision tree](/azure/architecture/guide/technology-choices/compute-decision-tree) may help when choosing the right path to take for a migration.</span></span>

### <a name="devops"></a><span data-ttu-id="ebafa-143">DevOps</span><span class="sxs-lookup"><span data-stu-id="ebafa-143">DevOps</span></span>

<span data-ttu-id="ebafa-144">**[CI(지속적인 통합)][continuous-integration]** - 안정적인 빌드를 유지 관리하며, 여러 개발자가 정기적으로 공유 코드 베이스 변경을 조금씩, 자주 커밋합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-144">**[Continuous Integration (CI)][continuous-integration]** maintains a stable build, with multiple developers regularly committing small, frequent changes to the shared codebase.</span></span> <span data-ttu-id="ebafa-145">지속적인 통합 파이프라인의 일환으로 다음을 수행해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-145">As part of your continuous integration pipeline, you should:</span></span>
* <span data-ttu-id="ebafa-146">작은 코드 변경을 자주 커밋합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-146">Frequently commit smaller code changes.</span></span> <span data-ttu-id="ebafa-147">성공적으로 병합하기 어려울 수 있는 큰 또는 복잡한 변경 내용을 일괄 처리하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-147">Avoid batching up larger or more complex changes that may be more difficult to merge successfully.</span></span>
* <span data-ttu-id="ebafa-148">불편한 경로 테스트를 포함하여 충분한 코드 범위를 사용하여 응용 프로그램 구성 요소에 대한 단위 테스트를 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-148">Conduct unit testing of your application components with sufficient code coverage, including testing the unhappy paths.</span></span>
* <span data-ttu-id="ebafa-149">빌드가 공유 마스터(또는 트렁크) 분기에 대해 실행되는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-149">Ensure the build is run against the shared master (or trunk) branch.</span></span> <span data-ttu-id="ebafa-150">이 분기는 안정적이어야 하며 "배포 준비 완료" 상태로 유지되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-150">This branch should be stable and maintained as "deployment ready".</span></span> <span data-ttu-id="ebafa-151">불완전하거나 작업 진행 중인 변경은 나중에 충돌하지 않도록 방지하기 위해 빈번한 "정방향 통합" 병합을 통해 별도의 분기에서 격리되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-151">Incomplete or work-in-progress changes should be isolated in a separate branch with frequent "forward integration" merges to avoid conflicts later.</span></span>

<span data-ttu-id="ebafa-152">**[CD(지속적인 업데이트)][continuous-delivery]** 는 안정적인 빌드뿐 아니라 안정적인 배포를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-152">**[Continuous Delivery (CD)][continuous-delivery]** demonstrates not just a stable build but a stable deployment.</span></span> <span data-ttu-id="ebafa-153">이에 따라 CD를 인식하기가 좀 더 어렵고, 환경 관련 구성이 필요하며, 이러한 값을 올바르게 설정하는 메커니즘이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-153">This makes realizing CD a little more difficult, requiring environment-specific configuration and a mechanism for setting those values correctly.</span></span> <span data-ttu-id="ebafa-154">다른 CD 고려 사항은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-154">Other CD considerations include the following:</span></span>
* <span data-ttu-id="ebafa-155">다양한 구성 요소가 종단 간에 올바르게 구성되고 작동하는지 확인하려면 충분한 통합 테스트 범위가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-155">Sufficient integration testing coverage is required to validate that the various components are configured and working correctly end-to-end.</span></span>
* <span data-ttu-id="ebafa-156">CD는 환경 관련 데이터를 설정 및 재설정하고 데이터베이스 스키마 버전을 관리해야 할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-156">CD may also require setting up and resetting environment-specific data and managing database schema versions.</span></span>
* <span data-ttu-id="ebafa-157">또한 지속적인 업데이트를 부하 테스트 및 사용자 승인 테스트 환경으로 확장해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-157">Continuous delivery should also extend to load testing and user acceptance testing environments.</span></span>
* <span data-ttu-id="ebafa-158">지속적인 업데이트에 가장 좋은 것은 모든 환경을 지속적으로 모니터링하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-158">Continuous delivery benefits from continuous monitoring, ideally across all environments.</span></span>
* <span data-ttu-id="ebafa-159">호스팅 인프라의 생성 및 구성을 스크립팅하여 환경에서 수행되는 개발 및 통합 테스트의 일관성과 안정성을 손쉽게 높일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-159">The consistency and reliability of deployments and integration testing across environments is made easier by scripting the creation and configuration of the hosting infrastructure.</span></span> <span data-ttu-id="ebafa-160">클라우드 기반 워크로드의 경우 이것이 매우 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-160">This is considerably easier for cloud-based workloads.</span></span> <span data-ttu-id="ebafa-161">자세한 내용은 [코드로써의 인프라][infra-as-code]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="ebafa-161">For more information, see [Infrastructure as Code][infra-as-code].</span></span>
* <span data-ttu-id="ebafa-162">프로젝트 수명 주기에서 최대한 빨리 지속적인 업데이트를 시작하세요.</span><span class="sxs-lookup"><span data-stu-id="ebafa-162">Begin continuous delivery as early as possible in the project lifecycle.</span></span> <span data-ttu-id="ebafa-163">늦게 시작할수록 통합하기가 어렵습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-163">The later you begin, the more difficult it will be to incorporate.</span></span>
* <span data-ttu-id="ebafa-164">통합과 단위 테스트에 프로젝트 기능과 동일한 우선 순위를 부여해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-164">Integration and unit tests should be given the same priority as application features.</span></span>
* <span data-ttu-id="ebafa-165">환경에 독립적인 배포 패키지를 사용하고, 릴리스 프로세스를 통해 환경 특정 구성을 관리합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-165">Use environment-agnostic deployment packages and manage environment-specific configuration via the release process.</span></span>
* <span data-ttu-id="ebafa-166">릴리스 프로세스 중에 릴리스 관리 도구를 사용하여 또는 HSM(하드웨어 보안 모듈)이나 [Azure Key Vault][azure-key-vault]를 호출하여 중요한 구성을 보호합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-166">Protect sensitive configuration using the release management tooling, or by calling out to a Hardware-security-module (HSM) or [Azure Key Vault][azure-key-vault] during the release process.</span></span> <span data-ttu-id="ebafa-167">소스 제어 내에는 중요한 구성을 저장하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-167">Do not store sensitive configuration within source control.</span></span>

<span data-ttu-id="ebafa-168">**지속적인 학습**.</span><span class="sxs-lookup"><span data-stu-id="ebafa-168">**Continuous Learning**.</span></span> <span data-ttu-id="ebafa-169">CD 환경을 가장 효과적으로 모니터링하는 방법은 [Application Insights][application-insights] 같은 APM(응용 프로그램 성능 모니터링) 도구를 사용하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-169">The most effective monitoring of a CD environment is provided by application performance monitoring (APM) tools such as [Application Insights][application-insights].</span></span> <span data-ttu-id="ebafa-170">응용 프로그램 워크로드에 대한 충분한 모니터링은 버그 또는 로드 시 성능을 파악하는 데 중요합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-170">Sufficient depth of monitoring for an application workload is critical to understand bugs or performance under load.</span></span> <span data-ttu-id="ebafa-171">Application Insights를 VSTS에 통합하여 [CD 파이프라인을 지속적으로 모니터링할 수 있습니다][app-insights-cd-monitoring].</span><span class="sxs-lookup"><span data-stu-id="ebafa-171">Application Insights can be integrated into VSTS to enable [continuous monitoring of the CD pipeline][app-insights-cd-monitoring].</span></span> <span data-ttu-id="ebafa-172">이를 통해 사용자 개입 없이 자동으로 다음 단계로 진행하거나 경고가 검색되면 롤백할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-172">This could be used to enable automatic progression to the next stage, without human intervention, or rollback if an alert is detected.</span></span>

## <a name="considerations"></a><span data-ttu-id="ebafa-173">고려 사항</span><span class="sxs-lookup"><span data-stu-id="ebafa-173">Considerations</span></span>

### <a name="availability"></a><span data-ttu-id="ebafa-174">가용성</span><span class="sxs-lookup"><span data-stu-id="ebafa-174">Availability</span></span>

<span data-ttu-id="ebafa-175">클라우드 응용 프로그램을 구축하는 경우 [일반적인 가용성 디자인 패턴][design-patterns-availability]을 활용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-175">Consider leveraging the [typical design patterns for availability][design-patterns-availability] when building your cloud application.</span></span>

<span data-ttu-id="ebafa-176">적절한 [App Service 웹 응용 프로그램 참조 아키텍처][app-service-reference-architecture]의 가용성 고려 사항을 검토합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-176">Review the availability considerations in the appropriate [App Service web application reference architecture][app-service-reference-architecture]</span></span>

<span data-ttu-id="ebafa-177">다른 가용성 항목에 대해서는 Azure 아키텍처 센터의 [가용성 검사 목록][availability]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="ebafa-177">For other availability topics, see the [availability checklist][availability] in the Azure Architecture Center.</span></span>

### <a name="scalability"></a><span data-ttu-id="ebafa-178">확장성</span><span class="sxs-lookup"><span data-stu-id="ebafa-178">Scalability</span></span>

<span data-ttu-id="ebafa-179">클라우드 응용 프로그램을 구축하는 경우 [일반적인 확장성 디자인 패턴][design-patterns-scalability]에 대해 알고 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-179">When building a cloud application be aware of the [typical design patterns for scalability][design-patterns-scalability].</span></span>

<span data-ttu-id="ebafa-180">적절한 [App Service 웹 응용 프로그램 참조 아키텍처][app-service-reference-architecture]의 확장성 고려 사항을 검토합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-180">Review the scalability considerations in the appropriate [App Service web application reference architecture][app-service-reference-architecture]</span></span>

<span data-ttu-id="ebafa-181">다른 확장성 항목에 대해서는 Azure 아키텍처 센터의 [확장성 검사 목록][scalability]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="ebafa-181">For other scalability topics, see the [scalability checklist][scalability] in the Azure Architecture Center.</span></span>

### <a name="security"></a><span data-ttu-id="ebafa-182">보안</span><span class="sxs-lookup"><span data-stu-id="ebafa-182">Security</span></span>

<span data-ttu-id="ebafa-183">적절한 경우 [일반적인 보안 디자인 패턴][design-patterns-security]을 활용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-183">Consider leveraging the [typical design patterns for security][design-patterns-security] where appropriate.</span></span>

<span data-ttu-id="ebafa-184">적절한 [App Service 웹 응용 프로그램 참조 아키텍처][app-service-reference-architecture]의 보안 고려 사항을 검토합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-184">Review the security considerations in the appropriate [App Service web application reference architecture][app-service-reference-architecture].</span></span>

<span data-ttu-id="ebafa-185">보안 솔루션 설계에 대한 일반적인 지침은 [Azure 보안 설명서][security]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="ebafa-185">For general guidance on designing secure solutions, see the [Azure Security Documentation][security].</span></span>

### <a name="resiliency"></a><span data-ttu-id="ebafa-186">복원력</span><span class="sxs-lookup"><span data-stu-id="ebafa-186">Resiliency</span></span>

<span data-ttu-id="ebafa-187">적절할 경우 [일반적인 복원력 디자인 패턴][design-patterns-resiliency]을 구현하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-187">Consider implementing the [typical design patterns for resiliency][design-patterns-resiliency] where appropriate.</span></span>

<span data-ttu-id="ebafa-188">Azure 아키텍처 센터에서 [App Service에 대한 다양한 권장 사례][resiliency-app-service]를 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-188">You can find a number of [recommended practices for App Service][resiliency-app-service] in the Azure Architecture Center.</span></span>

<span data-ttu-id="ebafa-189">복원력 있는 솔루션 설계에 대한 일반적인 지침은 [복원력 있는 Azure 응용 프로그램 디자인][resiliency]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="ebafa-189">For general guidance on designing resilient solutions, see [Designing resilient applications for Azure][resiliency].</span></span>

## <a name="deploy-the-scenario"></a><span data-ttu-id="ebafa-190">시나리오 배포</span><span class="sxs-lookup"><span data-stu-id="ebafa-190">Deploy the scenario</span></span>

### <a name="prerequisites"></a><span data-ttu-id="ebafa-191">필수 조건</span><span class="sxs-lookup"><span data-stu-id="ebafa-191">Prerequisites</span></span>

* <span data-ttu-id="ebafa-192">기존 Azure 계정이 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-192">You must have an existing Azure account.</span></span> <span data-ttu-id="ebafa-193">Azure 구독이 없는 경우 시작하기 전에 [체험 계정][azure-free-account]을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-193">If you don't have an Azure subscription, create a [free account][azure-free-account] before you begin.</span></span>
* <span data-ttu-id="ebafa-194">Azure DevOps 조직에 가입해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-194">You must sign up for an Azure DevOps organization.</span></span> <span data-ttu-id="ebafa-195">자세한 내용은 [빠른 시작: 조직 만들기][vsts-account-create]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="ebafa-195">For more information, see [Quickstart: Create your organization][vsts-account-create].</span></span>

### <a name="walk-through"></a><span data-ttu-id="ebafa-196">연습</span><span class="sxs-lookup"><span data-stu-id="ebafa-196">Walk-through</span></span>

<span data-ttu-id="ebafa-197">이 시나리오에서는 Azure DevOps 프로젝트를 사용하여 CI/CD 파이프라인을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-197">In this scenario, you'll use the Azure DevOps project to create your CI/CD pipeline.</span></span>

<span data-ttu-id="ebafa-198">Azure DevOps 프로젝트는 고객을 대신하여 App Service 계획, App Service 및 App Insights 리소스를 배포하고, 고객을 대신하여 Azure DevOps 프로젝트를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-198">The Azure DevOps project will deploy an App Service Plan, App Service, and an App Insights resource for you, as well as configure the Azure DevOps project for you.</span></span>

<span data-ttu-id="ebafa-199">Azure DevOps 프로젝트를 개발하고 빌드가 완료되면 관련 코드 변경 사항, 작업 항목 및 테스트 결과를 검토합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-199">Once you've deployed the Azure DevOps project and the build is completed, review the associated code changes, work items, and test results.</span></span> <span data-ttu-id="ebafa-200">코드에 실행할 테스트가 포함되어 있지 않으므로 테스트 결과가 표시되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-200">You will notice that no test results are displayed, because the code does not contain any tests to run.</span></span>

<span data-ttu-id="ebafa-201">릴리스 정의를 검토합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-201">Review the release definitions.</span></span> <span data-ttu-id="ebafa-202">릴리스 파이프라인이 설정되어 응용 프로그램이 개발 환경으로 릴리스되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-202">Notice that a release pipeline has been set up, releasing our application into the Dev environment.</span></span> <span data-ttu-id="ebafa-203">**삭제** 빌드 아티팩트에서 **지속적인 배포 트리거**가 설정되어 있고, 개발 환경으로 자동 릴리스되는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-203">Observe that there is a **continuous deployment trigger** set from the **Drop** build artifact, with automatic releases into the Dev environment.</span></span> <span data-ttu-id="ebafa-204">지속적인 배포 프로세스의 일부로 릴리스가 여러 환경에 걸쳐 있음을 알 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-204">As part of a continuous deployment process, you may see releases that span multiple environments.</span></span> <span data-ttu-id="ebafa-205">릴리스가 두 인프라를 모두 아우르고(코드로써의 인프라 같은 기술을 사용하여), 구성 후 작업과 함께 필요한 응용 프로그램 패키지를 배포할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-205">A release can span both infrastructure (using techniques such as infrastructure-as-code), and can also deploy the application packages required along with any post-configuration tasks.</span></span>

## <a name="additional-considerations"></a><span data-ttu-id="ebafa-206">추가 고려 사항</span><span class="sxs-lookup"><span data-stu-id="ebafa-206">Additional considerations</span></span>

* <span data-ttu-id="ebafa-207">VSTS 마켓플레이스에 제공되는 [토큰화 작업][vsts-tokenization] 중 하나를 활용하는 방안을 고려해 봅니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-207">Consider leveraging one of the [tokenization tasks][vsts-tokenization] available in the VSTS marketplace.</span></span>
* <span data-ttu-id="ebafa-208">[배포: Azure Key Vault][download-keyvault-secrets] VSTS 작업을 사용하여 Azure Key Vault에서 비밀을 다운로드하는 방안을 고려해 봅니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-208">Consider using the [Deploy: Azure Key Vault][download-keyvault-secrets] VSTS task to download secrets from an Azure Key Vault into your release.</span></span> <span data-ttu-id="ebafa-209">그런 다음, 릴리스 정의에서 이러한 비밀을 변수로 사용하면 원본 제어에 저장되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-209">You can then use those secrets as variables in your release definition, so you can avoid storing them in source control.</span></span>
* <span data-ttu-id="ebafa-210">릴리스 환경에서 [릴리스 변수][vsts-release-variables]를 사용하여 환경 구성을 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-210">Consider using [release variables][vsts-release-variables] in your release definitions to drive configuration changes of your environments.</span></span> <span data-ttu-id="ebafa-211">릴리스 변수의 범위는 전체 릴리스 또는 지정된 환경으로 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-211">Release variables can be scoped to an entire release or a given environment.</span></span> <span data-ttu-id="ebafa-212">변수를 비밀 정보에 사용하는 경우 자물쇠 아이콘을 선택해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-212">When using variables for secret information, ensure that you select the padlock icon.</span></span>
* <span data-ttu-id="ebafa-213">릴리스 파이프라인에서 [배포 게이트][vsts-deployment-gates]를 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-213">Consider using [deployment gates][vsts-deployment-gates] in your release pipeline.</span></span> <span data-ttu-id="ebafa-214">이렇게 하면 외부 시스템(예: 인시던트 관리 또는 추가 맞춤형 시스템)과 공동으로 모니터링 데이터를 활용하여 릴리스를 승격해야 하는지 여부를 결정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-214">This lets you leverage monitoring data in association with external systems (for example, incident management or additional bespoke systems) to determine whether a release should be promoted.</span></span>
* <span data-ttu-id="ebafa-215">릴리스 파이프라인에 수동으로 개입해야 하는 경우 [승인][vsts-approvals] 기능을 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-215">Where manual intervention in a release pipeline is required, consider using the [approvals][vsts-approvals] functionality.</span></span>
* <span data-ttu-id="ebafa-216">릴리스 파이프라인에서 최대한 빨리 [Application Insights][application-insights] 및 추가 모니터링 도구를 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-216">Consider using [Application Insights][application-insights] and additional monitoring tools as early as possible in your release pipeline.</span></span> <span data-ttu-id="ebafa-217">많은 조직이 프로덕션 환경에서만 모니터링을 시작하는데, 다른 환경을 모니터링하면 개발 프로세스 초기에 버그를 식별하고 프로덕션 환경의 문제를 방지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-217">Many organizations only begin monitoring in their production environment; by monitoring your other environments, you can identify bugs earlier in the development process and avoid issues in your production environment.</span></span>

## <a name="pricing"></a><span data-ttu-id="ebafa-218">가격</span><span class="sxs-lookup"><span data-stu-id="ebafa-218">Pricing</span></span>

<span data-ttu-id="ebafa-219">Azure DevOps 비용은 필요한 동시 빌드/릴리스 수, 테스트 사용자 수 등의 기타 요소와 조직 내에서 액세스 권한이 필요한 사용자 수에 따라 달라집니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-219">Azure DevOps costs depend on the number of users in your organization that require access, along with other factors like the number of concurrent build/releases required and number of test users.</span></span> <span data-ttu-id="ebafa-220">자세한 내용은 [Azure DevOps 가격 책정][vsts-pricing-page]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="ebafa-220">For more information, see [Azure DevOps pricing][vsts-pricing-page].</span></span>

* <span data-ttu-id="ebafa-221">[Azure DevOps][vsts-pricing-calculator]는 프로그램 개발 수명 주기를 관리할 수 있는 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-221">[Azure DevOps][vsts-pricing-calculator] is a service that enables you to manage your development life cycle.</span></span> <span data-ttu-id="ebafa-222">월별 사용자 수에 따라 요금이 청구됩니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-222">It is paid for on a per-user per-month basis.</span></span> <span data-ttu-id="ebafa-223">추가 테스트 사용자 또는 사용자 기본 라이선스 외에도 필요한 동시 파이프라인 수에 따라 추가 요금이 부과될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ebafa-223">There may be additional charges dependent upon concurrent pipelines needed, in addition to any additional test users or user basic licenses.</span></span>

## <a name="related-resources"></a><span data-ttu-id="ebafa-224">관련 리소스</span><span class="sxs-lookup"><span data-stu-id="ebafa-224">Related resources</span></span>

* <span data-ttu-id="ebafa-225">[DevOps란?][devops-whatis]</span><span class="sxs-lookup"><span data-stu-id="ebafa-225">[What is DevOps?][devops-whatis]</span></span>
* <span data-ttu-id="ebafa-226">[Microsoft의 DevOps - Azure DevOps 사용 방법][devops-microsoft]</span><span class="sxs-lookup"><span data-stu-id="ebafa-226">[DevOps at Microsoft - How we work with Azure DevOps][devops-microsoft]</span></span>
* <span data-ttu-id="ebafa-227">[단계별 자습서: Azure DevOps를 사용한 DevOps][devops-with-vsts]</span><span class="sxs-lookup"><span data-stu-id="ebafa-227">[Step-by-step Tutorials: DevOps with Azure DevOps][devops-with-vsts]</span></span>
* <span data-ttu-id="ebafa-228">[Devops 검사 목록][devops-checklist]</span><span class="sxs-lookup"><span data-stu-id="ebafa-228">[Devops Checklist][devops-checklist]</span></span>
* <span data-ttu-id="ebafa-229">[Azure DevOps 프로젝트를 사용하여 .NET용 CI/CD 파이프라인 만들기][devops-project-create]</span><span class="sxs-lookup"><span data-stu-id="ebafa-229">[Create a CI/CD pipeline for .NET with the Azure DevOps project][devops-project-create]</span></span>

<!-- links -->
[ansible]: /azure/ansible/
[application-insights]: /azure/application-insights/app-insights-overview
[app-service-reference-architecture]: ../../reference-architectures/app-service-web-app/basic-web-app.md
[azure-free-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F
[arm-templates]: /azure/azure-resource-manager/resource-group-overview#template-deployment
[architecture]: ./media/architecture-devops-dotnet-webapp.png
[availability]: /azure/architecture/checklist/availability
[chef]: /azure/chef/
[design-patterns-availability]: /azure/architecture/patterns/category/availability
[design-patterns-resiliency]: /azure/architecture/patterns/category/resiliency
[design-patterns-scalability]: /azure/architecture/patterns/category/performance-scalability
[design-patterns-security]: /azure/architecture/patterns/category/security
[desired-state-configuration]: /azure/automation/automation-dsc-overview
[devops-microsoft]: /azure/devops/devops-at-microsoft/
[devops-with-vsts]: https://almvm.azurewebsites.net/labs/vsts/
[devops-checklist]: /azure/architecture/checklist/dev-ops
[application-insights]: https://azure.microsoft.com/services/application-insights/
[cloud-based-load-testing]: https://visualstudio.microsoft.com/team-services/cloud-load-testing/
[cloud-based-load-testing-on-premises]: /vsts/test/load-test/clt-with-private-machines?view=vsts
[jenkins-on-azure]: /azure/jenkins/
[devops-whatis]: /azure/devops/what-is-devops
[download-keyvault-secrets]: /vsts/pipelines/tasks/deploy/azure-key-vault?view=vsts
[resource-groups]: /azure/azure-resource-manager/resource-group-overview
[resiliency-app-service]: /azure/architecture/checklist/resiliency-per-service#app-service
[resiliency]: /azure/architecture/checklist/resiliency
[scalability]: /azure/architecture/checklist/scalability
[vsts]: /vsts/?view=vsts#pivot=services
[continuous-integration]: /azure/devops/what-is-continuous-integration
[continuous-delivery]: /azure/devops/what-is-continuous-delivery
[web-apps]: /azure/app-service/app-service-web-overview
[terraform]: /azure/terraform/
[vsts-account-create]: /azure/devops/organizations/accounts/create-organization-msa-or-work-student?view=vsts
[vsts-approvals]: /vsts/pipelines/release/approvals/approvals?view=vsts
[devops-project]: https://portal.azure.com/?feature.customportal=false#create/Microsoft.AzureProject
[vsts-deployment-gates]: /vsts/pipelines/release/approvals/gates?view=vsts
[vsts-pricing-calculator]: https://azure.com/e/498aa024454445a8a352e75724f900b1
[vsts-pricing-page]: https://azure.microsoft.com/pricing/details/visual-studio-team-services/
[vsts-release-variables]: /vsts/pipelines/release/variables?view=vsts&tabs=batch
[vsts-tokenization]: https://marketplace.visualstudio.com/search?term=token&target=VSTS&category=All%20categories&sortBy=Relevance
[azure-key-vault]: /azure/key-vault/key-vault-overview
[infra-as-code]: https://blogs.msdn.microsoft.com/mvpawardprogram/2018/02/13/infrastructure-as-code/
[team-foundation-server]: https://visualstudio.microsoft.com/tfs/
[infra-as-code]: https://blogs.msdn.microsoft.com/mvpawardprogram/2018/02/13/infrastructure-as-code/
[service-fabric]: /azure/service-fabric/
[azure-functions]: /azure/azure-functions/
[azure-containers]: https://azure.microsoft.com/overview/containers/
[compare-vm-hosting]: /azure/app-service/choose-web-site-cloud-service-vm
[app-insights-cd-monitoring]: /azure/application-insights/app-insights-vsts-continuous-monitoring
[azure-region-pair-bcdr]: /azure/best-practices-availability-paired-regions
[devops-project-create]: /azure/devops-project/azure-devops-project-aspnet-core
[security]: /azure/security/