---
title: Azure DevOps를 사용하여 CI/CD 파이프라인 설계
description: Azure DevOps를 사용하여 Azure Web Apps에 .NET 앱을 빌드하고 릴리스합니다.
author: christianreddington
ms.date: 12/06/2018
ms.custom:
- fasttrack
- seodec18
ms.openlocfilehash: 23945493115522d099b6b26922f567653da0367e
ms.sourcegitcommit: 4ba3304eebaa8c493c3e5307bdd9d723cd90b655
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53307285"
---
# <a name="design-a-cicd-pipeline-using-azure-devops"></a><span data-ttu-id="ca85b-103">Azure DevOps를 사용하여 CI/CD 파이프라인 설계</span><span class="sxs-lookup"><span data-stu-id="ca85b-103">Design a CI/CD pipeline using Azure DevOps</span></span>

<span data-ttu-id="ca85b-104">이 시나리오는 CI(지속적인 통합) 및 CD(지속적인 배포) 파이프라인을 구축하기 위한 아키텍처와 디자인 지침을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-104">This scenario provides architecture and design guidance for building a continuous integration (CI) and continuous deployment (CD) pipeline.</span></span>  <span data-ttu-id="ca85b-105">이 예제의 CI/CD 파이프라인은 Azure App Service에 2계층 .NET 웹 애플리케이션을 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-105">In this example, the CI/CD pipeline deploys a two-tier .NET web application to the Azure App Service.</span></span>

<span data-ttu-id="ca85b-106">최신 CI/CD 프로세스로 마이그레이션하면 애플리케이션 빌드, 배포, 테스트 및 모니터링에서 여러 가지 혜택을 얻을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-106">Migrating to modern CI/CD processes provides many benefits for application builds, deployments, testing, and monitoring.</span></span> <span data-ttu-id="ca85b-107">Azure DevOps를 App Service 같은 다른 서비스와 함께 사용하면 조직에서는 지원 인프라 관리 대신 앱 개발에 집중할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-107">By utilizing Azure DevOps along with other services such as App Service, organizations can focus on the development of their apps rather than the management of the supporting infrastructure.</span></span>

## <a name="relevant-use-cases"></a><span data-ttu-id="ca85b-108">관련 사용 사례</span><span class="sxs-lookup"><span data-stu-id="ca85b-108">Relevant use cases</span></span>

<span data-ttu-id="ca85b-109">Azure DevOps 및 CI/CD 프로세스를 고려해야 하는 시나리오:</span><span class="sxs-lookup"><span data-stu-id="ca85b-109">Consider Azure DevOps and CI/CD processes for:</span></span>

- <span data-ttu-id="ca85b-110">응용 프로그램 개발 및 개발 수명 주기 가속화</span><span class="sxs-lookup"><span data-stu-id="ca85b-110">Accelerating application development and development life cycles</span></span>
- <span data-ttu-id="ca85b-111">자동화된 빌드 및 릴리스 프로세스에 대한 품질 및 일관성 구축</span><span class="sxs-lookup"><span data-stu-id="ca85b-111">Building quality and consistency into an automated build and release process</span></span>
- <span data-ttu-id="ca85b-112">애플리케이션 안정성 및 가동 시간 향상</span><span class="sxs-lookup"><span data-stu-id="ca85b-112">Increasing application stability and uptime</span></span>

## <a name="architecture"></a><span data-ttu-id="ca85b-113">아키텍처</span><span class="sxs-lookup"><span data-stu-id="ca85b-113">Architecture</span></span>

![Azure DevOps 및 Azure App Service를 사용하는 DevOps 시나리오와 관련된 Azure 구성 요소의 아키텍처 다이어그램][architecture]

<span data-ttu-id="ca85b-115">시나리오를 통한 데이터 흐름은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-115">The data flows through the scenario as follows:</span></span>

1. <span data-ttu-id="ca85b-116">개발자가 애플리케이션 소스 코드를 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-116">A developer changes application source code.</span></span>
2. <span data-ttu-id="ca85b-117">web.config 파일을 포함한 애플리케이션 코드가 Azure Repos의 소스 코드 리포지토리로 커밋됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-117">Application code including the web.config file is committed to the source code repository in Azure Repos.</span></span>
3. <span data-ttu-id="ca85b-118">지속적인 통합에서 Azure Test Plans를 사용하여 애플리케이션 빌드 및 단위 테스트를 트리거합니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-118">Continuous integration triggers application build and unit tests using Azure Test Plans.</span></span>
4. <span data-ttu-id="ca85b-119">Azure Pipelines 내부의 지속적인 배포에서 *환경별 구성 값*을 사용하여 애플리케이션 아티팩트의 자동 배포를 트리거합니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-119">Continuous deployment within Azure Pipelines triggers an automated deployment of application artifacts *with environment-specific configuration values*.</span></span>
5. <span data-ttu-id="ca85b-120">Azure App Service에 아티팩트가 배포됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-120">The artifacts are deployed to Azure App Service.</span></span>
6. <span data-ttu-id="ca85b-121">Azure Application Insights에서 상태, 성능 및 사용량 데이터를 수집합니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-121">Azure Application Insights collects and analyzes health, performance, and usage data.</span></span>
7. <span data-ttu-id="ca85b-122">개발자가 상태, 성능 및 사용량 정보를 모니터링하고 관리합니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-122">Developers monitor and mange health, performance, and usage information.</span></span>
8. <span data-ttu-id="ca85b-123">백로그 정보는 Azure Boards를 사용하여 새 기능과 버그 수정의 우선 순위를 지정하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-123">Backlog information is used to prioritize new features and bug fixes using Azure Boards.</span></span>

### <a name="components"></a><span data-ttu-id="ca85b-124">구성 요소</span><span class="sxs-lookup"><span data-stu-id="ca85b-124">Components</span></span>

- <span data-ttu-id="ca85b-125">[Azure DevOps][vsts]는 계획 및 프로젝트 관리부터 코드 관리, 지속적인 빌드 및 릴리스까지 모든 개발 수명 주기를 관리할 수 있는 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-125">[Azure DevOps][vsts] is a service for managing your development life cycle end-to-end &mdash; from planning and project management, to code management, and continuing to build and release.</span></span>

- <span data-ttu-id="ca85b-126">[Azure Web Apps][web-apps]는 웹 응용 프로그램, REST API 및 모바일 백 엔드를 호스팅하는 PaaS 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-126">[Azure Web Apps][web-apps] is a PaaS service for hosting web applications, REST APIs, and mobile back ends.</span></span> <span data-ttu-id="ca85b-127">이 문서에서는 .NET에 집중하고 있지만 몇 가지 추가 개발 플랫폼 옵션이 지원됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-127">While this article focuses on .NET, there are several additional development platform options supported.</span></span>

- <span data-ttu-id="ca85b-128">[Application Insights][application-insights]는 웹 개발자를 위해 여러 플랫폼에서 확장 가능한 자체 APM(Application Performance Management) 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-128">[Application Insights][application-insights] is a first-party, extensible Application Performance Management (APM) service for web developers on multiple platforms.</span></span>

### <a name="alternatives"></a><span data-ttu-id="ca85b-129">대안</span><span class="sxs-lookup"><span data-stu-id="ca85b-129">Alternatives</span></span>

<span data-ttu-id="ca85b-130">이 문서에서는 Azure DevOps를 중점적으로 다루지만, [Azure DevOps Server][azure-devops-server](이전 명칭은 Team Foundation Server)를 온-프레미스 대체 도구로 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-130">While this article focuses on Azure DevOps, [Azure DevOps Server][azure-devops-server] (previously known as Team Foundation Server) could be used as an on-premises substitute.</span></span> <span data-ttu-id="ca85b-131">또는 [Jenkins][jenkins-on-azure]를 사용하는 오픈 소스 개발 파이프라인에 대한 기술 세트를 사용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-131">Alternatively, you could also use a set of technologies for an open-source development pipeline using [Jenkins][jenkins-on-azure].</span></span>

<span data-ttu-id="ca85b-132">코드로써의 인프라(Infrastructure as Code) 관점에서, [Resource Manager 템플릿][arm-templates]은 Azure DevOps 프로젝트의 일부로 사용되었지만, [Terraform][terraform] 또는 [Chef][chef] 같은 다른 관리 기술을 생각해볼 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-132">From an infrastructure-as-code perspective, [Resource Manager templates][arm-templates] were used as part of the Azure DevOps project, but you could consider other management technologies such as [Terraform][terraform] or [Chef][chef].</span></span> <span data-ttu-id="ca85b-133">IaaS(Infrastructure as a Service) 기반 배포를 선호하고 구성 관리가 필요한 경우 [Azure Automation State Configuration][desired-state-configuration], [Ansible][ansible] 또는 [Chef][chef] 중 하나를 고려할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-133">If you prefer an infrastructure-as-a-service (IaaS)-based deployment and require configuration management, you could consider either [Azure Automation State Configuration][desired-state-configuration], [Ansible][ansible], or [Chef][chef].</span></span>

<span data-ttu-id="ca85b-134">Azure Web Apps에 호스팅하는 대안은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-134">You could consider these alternatives to hosting in Azure Web Apps:</span></span>

- <span data-ttu-id="ca85b-135">[Azure Virtual Machines][compare-vm-hosting]는 높은 수준의 제어가 필요하거나 Web Apps에서 가능하지 않은 OS 구성 요소 및 서비스를 사용하는 워크로드(예: Windows GAC 또는 COM)를 처리합니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-135">[Azure Virtual Machines][compare-vm-hosting] handles workloads that require a high degree of control, or depend on OS components and services that are not possible with Web Apps (for example, the Windows GAC, or COM).</span></span>

- <span data-ttu-id="ca85b-136">[Service Fabric][service-fabric]은 워크로드 아키텍처가 높은 수준의 제어를 통해 클러스터 전체에 배포되고 실행되는 이점이 있는 분산된 구성 요소에 집중되는 경우에 좋은 옵션입니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-136">[Service Fabric][service-fabric] is a good option if the workload architecture is focused around distributed components that benefit from being deployed and run across a cluster with a high degree of control.</span></span> <span data-ttu-id="ca85b-137">Service Fabric은 컨테이너를 호스팅하는 데도 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-137">Service Fabric can also be used to host containers.</span></span>

- <span data-ttu-id="ca85b-138">[Azure Functions][azure-functions]는 워크로드 아키텍처가 세분화된 분산된 구성 요소에 집중되고, 최소한의 종속성이 필요하며, 개별 구성 요소가 요청 시에만 실행되고(불연속) 구성 요소의 오케스트레이션이 필요하지 않은 경우에 효과적인 서버리스 접근 방식을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-138">[Azure Functions][azure-functions] provides an effective serverless approach if the workload architecture is centered around fine grained distributed components, requiring minimal dependencies, where individual components are only required to run on demand (not continuously) and orchestration of components is not required.</span></span>

<span data-ttu-id="ca85b-139">이 [Azure 컴퓨팅 서비스에 대한 의사 결정 트리](/azure/architecture/guide/technology-choices/compute-decision-tree)는 올바른 마이그레이션 경로를 선택할 때 도움이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-139">This [decision tree for Azure compute services](/azure/architecture/guide/technology-choices/compute-decision-tree) may help when choosing the right path to take for a migration.</span></span>

## <a name="management-and-security-considerations"></a><span data-ttu-id="ca85b-140">관리 및 보안 고려 사항</span><span class="sxs-lookup"><span data-stu-id="ca85b-140">Management and Security Considerations</span></span>

- <span data-ttu-id="ca85b-141">VSTS 마켓플레이스에 제공되는 [토큰화 작업][vsts-tokenization] 중 하나를 활용하는 방안을 고려해 봅니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-141">Consider leveraging one of the [tokenization tasks][vsts-tokenization] available in the VSTS marketplace.</span></span>

- <span data-ttu-id="ca85b-142">[Azure Key Vault][download-keyvault-secrets] 작업은 Azure Key Vault에서 릴리스로 비밀을 다운로드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-142">[Azure Key Vault][download-keyvault-secrets] tasks can download secrets from an Azure Key Vault into your release.</span></span> <span data-ttu-id="ca85b-143">그런 다음, 릴리스 정의에서 이러한 비밀을 변수로 사용하면 원본 제어에 저장되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-143">You can then use those secrets as variables in your release definition, which avoids storing them in source control.</span></span>

- <span data-ttu-id="ca85b-144">릴리스 정의에 [릴리스 변수][vsts-release-variables]를 사용하여 환경 구성을 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-144">Use [release variables][vsts-release-variables] in your release definitions to drive configuration changes of your environments.</span></span> <span data-ttu-id="ca85b-145">릴리스 변수의 범위는 전체 릴리스 또는 지정된 환경으로 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-145">Release variables can be scoped to an entire release or a given environment.</span></span> <span data-ttu-id="ca85b-146">변수를 비밀 정보에 사용하는 경우 자물쇠 아이콘을 선택해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-146">When using variables for secret information, ensure that you select the padlock icon.</span></span>

- <span data-ttu-id="ca85b-147">릴리스 파이프라인에 [배포 게이트][vsts-deployment-gates]를 사용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-147">[Deployment gates][vsts-deployment-gates] should be used in your release pipeline.</span></span> <span data-ttu-id="ca85b-148">이렇게 하면 외부 시스템(예: 인시던트 관리 또는 추가 맞춤형 시스템)과 공동으로 모니터링 데이터를 활용하여 릴리스를 승격해야 하는지 여부를 결정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-148">This lets you leverage monitoring data in association with external systems (for example, incident management or additional bespoke systems) to determine whether a release should be promoted.</span></span>

- <span data-ttu-id="ca85b-149">릴리스 파이프라인에서 수동 개입이 필요한 경우 [승인][vsts-approvals] 기능을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-149">Where manual intervention in a release pipeline is required, use the [approvals][vsts-approvals] functionality.</span></span>

- <span data-ttu-id="ca85b-150">릴리스 파이프라인에서 최대한 빨리 [Application Insights][application-insights] 및 추가 모니터링 도구를 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-150">Consider using [Application Insights][application-insights] and additional monitoring tools as early as possible in your release pipeline.</span></span> <span data-ttu-id="ca85b-151">많은 조직이 프로덕션 환경 모니터링만 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-151">Many organizations only begin monitoring in their production environment.</span></span> <span data-ttu-id="ca85b-152">다른 환경을 모니터링하면 개발 프로세스 초기에 버그를 식별하고 프로덕션 환경의 문제를 방지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-152">By monitoring your other environments, you can identify bugs earlier in the development process and avoid issues in your production environment.</span></span>

## <a name="deploy-the-scenario"></a><span data-ttu-id="ca85b-153">시나리오 배포</span><span class="sxs-lookup"><span data-stu-id="ca85b-153">Deploy the scenario</span></span>

### <a name="prerequisites"></a><span data-ttu-id="ca85b-154">필수 조건</span><span class="sxs-lookup"><span data-stu-id="ca85b-154">Prerequisites</span></span>

- <span data-ttu-id="ca85b-155">기존 Azure 계정이 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-155">You must have an existing Azure account.</span></span> <span data-ttu-id="ca85b-156">Azure 구독이 없는 경우 시작하기 전에 [체험 계정][azure-free-account]을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-156">If you don't have an Azure subscription, create a [free account][azure-free-account] before you begin.</span></span>

- <span data-ttu-id="ca85b-157">Azure DevOps 조직에 가입해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-157">You must sign up for an Azure DevOps organization.</span></span> <span data-ttu-id="ca85b-158">자세한 내용은 [빠른 시작: 조직 만들기][vsts-account-create]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="ca85b-158">For more information, see [Quickstart: Create your organization][vsts-account-create].</span></span>

### <a name="walk-through"></a><span data-ttu-id="ca85b-159">연습</span><span class="sxs-lookup"><span data-stu-id="ca85b-159">Walk-through</span></span>

<span data-ttu-id="ca85b-160">[Azure DevOps 프로젝트](/azure/devops-project/azure-devops-project-github)는 고객을 대신하여 App Service 계획, App Service 및 App Insights 리소스를 배포하고, 고객을 대신하여 Azure DevOps 프로젝트를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-160">The [Azure DevOps project](/azure/devops-project/azure-devops-project-github) will deploy an App Service Plan, App Service, and an App Insights resource for you, as well as configure the Azure DevOps project for you.</span></span>

<span data-ttu-id="ca85b-161">Azure DevOps 프로젝트를 개발하고 빌드가 완료되면 관련 코드 변경 사항, 작업 항목 및 테스트 결과를 검토합니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-161">Once you've deployed the Azure DevOps project and the build is completed, review the associated code changes, work items, and test results.</span></span> <span data-ttu-id="ca85b-162">코드에 실행할 테스트가 포함되어 있지 않으므로 테스트 결과가 표시되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-162">You will notice that no test results are displayed, because the code does not contain any tests to run.</span></span>

<span data-ttu-id="ca85b-163">이 프로젝트는 릴리스 파이프라인 및 지속적인 배포 트리거를 만들고, 개발 환경에 애플리케이션을 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-163">The project creates a release pipeline and continuous deployment trigger, deploying our application into the Dev environment.</span></span> <span data-ttu-id="ca85b-164">지속적인 배포 프로세스의 일부로 릴리스가 여러 환경에 걸쳐 있음을 알 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-164">As part of a continuous deployment process, you may see releases that span multiple environments.</span></span> <span data-ttu-id="ca85b-165">릴리스가 두 인프라를 모두 아우르고(코드로써의 인프라 같은 기술을 사용하여), 구성 후 작업과 함께 필요한 응용 프로그램 패키지를 배포할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-165">A release can span both infrastructure (using techniques such as infrastructure-as-code), and can also deploy the application packages required along with any post-configuration tasks.</span></span>

## <a name="pricing"></a><span data-ttu-id="ca85b-166">가격</span><span class="sxs-lookup"><span data-stu-id="ca85b-166">Pricing</span></span>

<span data-ttu-id="ca85b-167">Azure DevOps 비용은 필요한 동시 빌드/릴리스 수, 테스트 사용자 수 등의 기타 요소와 조직 내에서 액세스 권한이 필요한 사용자 수에 따라 달라집니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-167">Azure DevOps costs depend on the number of users in your organization that require access, along with other factors like the number of concurrent build/releases required and number of test users.</span></span> <span data-ttu-id="ca85b-168">자세한 내용은 [Azure DevOps 가격 책정][vsts-pricing-page]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="ca85b-168">For more information, see [Azure DevOps pricing][vsts-pricing-page].</span></span>

<span data-ttu-id="ca85b-169">이 [가격 책정 계산기][vsts-pricing-calculator]는 사용자 수 20명인 Azure DevOps를 실행하기 위한 예상 수치를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-169">This [pricing calculator][vsts-pricing-calculator] provides an estimate for running Azure DevOps with 20 users.</span></span>

<span data-ttu-id="ca85b-170">Azure DevOps는 사용자당 월별 요금이 청구됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-170">Azure DevOps is billed on a per-user per-month basis.</span></span> <span data-ttu-id="ca85b-171">추가 테스트 사용자 또는 사용자 기본 라이선스 외에도 필요한 동시 파이프라인 수에 따라 추가 요금이 부과될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca85b-171">There may be additional charges dependent upon concurrent pipelines needed, in addition to any additional test users or user basic licenses.</span></span>

## <a name="related-resources"></a><span data-ttu-id="ca85b-172">관련 리소스</span><span class="sxs-lookup"><span data-stu-id="ca85b-172">Related resources</span></span>

<span data-ttu-id="ca85b-173">다음 리소스를 검토하여 CI/CD 및 Azure DevOps에 대해 자세히 알아보세요.</span><span class="sxs-lookup"><span data-stu-id="ca85b-173">Review the following resources to learn more about CI/CD and Azure DevOps:</span></span>

- <span data-ttu-id="ca85b-174">[DevOps란?][devops-whatis]</span><span class="sxs-lookup"><span data-stu-id="ca85b-174">[What is DevOps?][devops-whatis]</span></span>
- <span data-ttu-id="ca85b-175">[Microsoft의 DevOps - Azure DevOps 사용 방법][devops-microsoft]</span><span class="sxs-lookup"><span data-stu-id="ca85b-175">[DevOps at Microsoft - How we work with Azure DevOps][devops-microsoft]</span></span>
- <span data-ttu-id="ca85b-176">[단계별 자습서: Azure DevOps를 사용한 DevOps][devops-with-vsts]</span><span class="sxs-lookup"><span data-stu-id="ca85b-176">[Step-by-step Tutorials: DevOps with Azure DevOps][devops-with-vsts]</span></span>
- <span data-ttu-id="ca85b-177">[Devops 검사 목록][devops-checklist]</span><span class="sxs-lookup"><span data-stu-id="ca85b-177">[Devops Checklist][devops-checklist]</span></span>
- <span data-ttu-id="ca85b-178">[Azure DevOps 프로젝트를 사용하여 .NET용 CI/CD 파이프라인 만들기][devops-project-create]</span><span class="sxs-lookup"><span data-stu-id="ca85b-178">[Create a CI/CD pipeline for .NET with the Azure DevOps project][devops-project-create]</span></span>

<!-- links -->

[ansible]: /azure/ansible/
[application-insights]: /azure/application-insights/app-insights-overview
[app-service-reference-architecture]: ../../reference-architectures/app-service-web-app/basic-web-app.md
[azure-free-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F
[arm-templates]: /azure/azure-resource-manager/resource-group-overview#template-deployment
[architecture]: ./media/architecture-devops-dotnet-webapp.svg
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
[vsts]: /vsts/?view=vsts#pivot=services
[continuous-integration]: /azure/devops/what-is-continuous-integration
[continuous-delivery]: /azure/devops/what-is-continuous-delivery
[web-apps]: /azure/app-service/app-service-web-overview
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
[azure-devops-server]: https://visualstudio.microsoft.com/tfs/
[infra-as-code]: https://blogs.msdn.microsoft.com/mvpawardprogram/2018/02/13/infrastructure-as-code/
[service-fabric]: /azure/service-fabric/
[azure-functions]: /azure/azure-functions/
[azure-containers]: https://azure.microsoft.com/overview/containers/
[compare-vm-hosting]: /azure/app-service/choose-web-site-cloud-service-vm
[app-insights-cd-monitoring]: /azure/application-insights/app-insights-vsts-continuous-monitoring
[azure-region-pair-bcdr]: /azure/best-practices-availability-paired-regions
[devops-project-create]: /azure/devops-project/azure-devops-project-aspnet-core
[terraform]: /azure/terraform/
