---
title: Azure DevOps를 사용하여 CI/CD 파이프라인 설계
titleSuffix: Azure Example Scenarios
description: Azure DevOps를 사용하여 Azure Web Apps에 .NET 앱을 빌드하고 릴리스합니다.
author: christianreddington
ms.date: 12/06/2018
ms.topic: example-scenario
ms.service: architecture-center
ms.subservice: example-scenario
ms.custom: fasttrack, seodec18
social_image_url: /azure/architecture/example-scenario/apps/media/architecture-devops-dotnet-webapp.svg
ms.openlocfilehash: 0d6ac13fb357657a2ec5e6284abadb46d6926907
ms.sourcegitcommit: 3b15d65e7c35a19506e562c444343f8467b6a073
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/25/2019
ms.locfileid: "54908497"
---
# <a name="design-a-cicd-pipeline-using-azure-devops"></a>Azure DevOps를 사용하여 CI/CD 파이프라인 설계

이 시나리오는 CI(지속적인 통합) 및 CD(지속적인 배포) 파이프라인을 구축하기 위한 아키텍처와 디자인 지침을 제공합니다. 이 예제의 CI/CD 파이프라인은 Azure App Service에 2계층 .NET 웹 애플리케이션을 배포합니다.

최신 CI/CD 프로세스로 마이그레이션하면 애플리케이션 빌드, 배포, 테스트 및 모니터링에서 여러 가지 혜택을 얻을 수 있습니다. Azure DevOps를 App Service 같은 다른 서비스와 함께 사용하면 조직에서는 지원 인프라 관리 대신 앱 개발에 집중할 수 있습니다.

## <a name="relevant-use-cases"></a>관련 사용 사례

Azure DevOps 및 CI/CD 프로세스를 고려해야 하는 시나리오:

- 애플리케이션 개발 및 개발 수명 주기 가속화
- 자동화된 빌드 및 릴리스 프로세스에 대한 품질 및 일관성 구축
- 애플리케이션 안정성 및 가동 시간 향상

## <a name="architecture"></a>아키텍처

![Azure DevOps 및 Azure App Service를 사용하는 DevOps 시나리오와 관련된 Azure 구성 요소의 아키텍처 다이어그램][architecture]

시나리오를 통한 데이터 흐름은 다음과 같습니다.

1. 개발자가 애플리케이션 소스 코드를 변경합니다.
2. web.config 파일을 포함한 애플리케이션 코드가 Azure Repos의 소스 코드 리포지토리로 커밋됩니다.
3. 지속적인 통합에서 Azure Test Plans를 사용하여 애플리케이션 빌드 및 단위 테스트를 트리거합니다.
4. Azure Pipelines 내부의 지속적인 배포에서 *환경별 구성 값*을 사용하여 애플리케이션 아티팩트의 자동 배포를 트리거합니다.
5. Azure App Service에 아티팩트가 배포됩니다.
6. Azure Application Insights에서 상태, 성능 및 사용량 데이터를 수집합니다.
7. 개발자가 상태, 성능 및 사용량 정보를 모니터링하고 관리합니다.
8. 백로그 정보는 Azure Boards를 사용하여 새 기능과 버그 수정의 우선 순위를 지정하는 데 사용됩니다.

### <a name="components"></a>구성 요소

- [Azure DevOps][vsts]는 계획 및 프로젝트 관리부터 코드 관리, 지속적인 빌드 및 릴리스까지 모든 개발 수명 주기를 관리할 수 있는 서비스입니다.

- [Azure Web Apps][web-apps]는 웹 애플리케이션, REST API 및 모바일 백 엔드를 호스팅하는 PaaS 서비스입니다. 이 문서에서는 .NET에 집중하고 있지만 몇 가지 추가 개발 플랫폼 옵션이 지원됩니다.

- [Application Insights][application-insights]는 웹 개발자를 위해 여러 플랫폼에서 확장 가능한 자체 APM(Application Performance Management) 서비스입니다.

### <a name="alternatives"></a>대안

이 문서에서는 Azure DevOps를 중점적으로 다루지만, [Azure DevOps Server][azure-devops-server](이전 명칭은 Team Foundation Server)를 온-프레미스 대체 도구로 사용할 수 있습니다. 또는 [Jenkins][jenkins-on-azure]를 사용하는 오픈 소스 개발 파이프라인에 대한 기술 세트를 사용할 수도 있습니다.

코드로써의 인프라(Infrastructure as Code) 관점에서, [Resource Manager 템플릿][arm-templates]은 Azure DevOps 프로젝트의 일부로 사용되었지만, [Terraform][terraform] 또는 [Chef][chef] 같은 다른 관리 기술을 생각해볼 수도 있습니다. IaaS(Infrastructure as a Service) 기반 배포를 선호하고 구성 관리가 필요한 경우 [Azure Automation State Configuration][desired-state-configuration], [Ansible][ansible] 또는 [Chef][chef] 중 하나를 고려할 수 있습니다.

Azure Web Apps에 호스팅하는 대안은 다음과 같습니다.

- [Azure Virtual Machines][compare-vm-hosting]는 높은 수준의 제어가 필요하거나 Web Apps에서 가능하지 않은 OS 구성 요소 및 서비스를 사용하는 워크로드(예: Windows GAC 또는 COM)를 처리합니다.

- [Service Fabric][service-fabric]은 워크로드 아키텍처가 높은 수준의 제어를 통해 클러스터 전체에 배포되고 실행되는 이점이 있는 분산된 구성 요소에 집중되는 경우에 좋은 옵션입니다. Service Fabric은 컨테이너를 호스팅하는 데도 사용할 수 있습니다.

- [Azure Functions][azure-functions]는 워크로드 아키텍처가 세분화된 분산된 구성 요소에 집중되고, 최소한의 종속성이 필요하며, 개별 구성 요소가 요청 시에만 실행되고(불연속) 구성 요소의 오케스트레이션이 필요하지 않은 경우에 효과적인 서버리스 접근 방식을 제공합니다.

이 [Azure 컴퓨팅 서비스에 대한 의사 결정 트리](/azure/architecture/guide/technology-choices/compute-decision-tree)는 올바른 마이그레이션 경로를 선택할 때 도움이 될 수 있습니다.

## <a name="management-and-security-considerations"></a>관리 및 보안 고려 사항

- VSTS 마켓플레이스에 제공되는 [토큰화 작업][vsts-tokenization] 중 하나를 활용하는 방안을 고려해 봅니다.

- [Azure Key Vault][download-keyvault-secrets] 작업은 Azure Key Vault에서 릴리스로 비밀을 다운로드할 수 있습니다. 그런 다음, 릴리스 정의에서 이러한 비밀을 변수로 사용하면 원본 제어에 저장되지 않습니다.

- 릴리스 정의에 [릴리스 변수][vsts-release-variables]를 사용하여 환경 구성을 변경합니다. 릴리스 변수의 범위는 전체 릴리스 또는 지정된 환경으로 지정할 수 있습니다. 변수를 비밀 정보에 사용하는 경우 자물쇠 아이콘을 선택해야 합니다.

- 릴리스 파이프라인에 [배포 게이트][vsts-deployment-gates]를 사용해야 합니다. 이렇게 하면 외부 시스템(예: 인시던트 관리 또는 추가 맞춤형 시스템)과 공동으로 모니터링 데이터를 활용하여 릴리스를 승격해야 하는지 여부를 결정할 수 있습니다.

- 릴리스 파이프라인에서 수동 개입이 필요한 경우 [승인][vsts-approvals] 기능을 사용합니다.

- 릴리스 파이프라인에서 최대한 빨리 [Application Insights][application-insights] 및 추가 모니터링 도구를 사용하는 것이 좋습니다. 많은 조직이 프로덕션 환경 모니터링만 시작합니다. 다른 환경을 모니터링하면 개발 프로세스 초기에 버그를 식별하고 프로덕션 환경의 문제를 방지할 수 있습니다.

## <a name="deploy-the-scenario"></a>시나리오 배포

### <a name="prerequisites"></a>필수 조건

- 기존 Azure 계정이 있어야 합니다. Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)을 만듭니다.

- Azure DevOps 조직에 가입해야 합니다. 자세한 내용은 [빠른 시작: 조직 만들기][vsts-account-create]를 참조하세요.

### <a name="walk-through"></a>연습

[Azure DevOps 프로젝트](/azure/devops-project/azure-devops-project-github)는 고객을 대신하여 App Service 계획, App Service 및 App Insights 리소스를 배포하고, 고객을 대신하여 Azure DevOps 프로젝트를 구성합니다.

Azure DevOps 프로젝트를 개발하고 빌드가 완료되면 관련 코드 변경 사항, 작업 항목 및 테스트 결과를 검토합니다. 코드에 실행할 테스트가 포함되어 있지 않으므로 테스트 결과가 표시되지 않습니다.

이 프로젝트는 릴리스 파이프라인 및 지속적인 배포 트리거를 만들고, 개발 환경에 애플리케이션을 배포합니다. 지속적인 배포 프로세스의 일부로 릴리스가 여러 환경에 걸쳐 있음을 알 수 있습니다. 릴리스가 두 인프라를 모두 아우르고(코드로써의 인프라 같은 기술을 사용하여), 구성 후 작업과 함께 필요한 애플리케이션 패키지를 배포할 수 있습니다.

## <a name="pricing"></a>가격

Azure DevOps 비용은 필요한 동시 빌드/릴리스 수, 테스트 사용자 수 등의 기타 요소와 조직 내에서 액세스 권한이 필요한 사용자 수에 따라 달라집니다. 자세한 내용은 [Azure DevOps 가격 책정][vsts-pricing-page]을 참조하세요.

이 [가격 책정 계산기][vsts-pricing-calculator]는 사용자 수 20명인 Azure DevOps를 실행하기 위한 예상 수치를 제공합니다.

Azure DevOps는 사용자당 월별 요금이 청구됩니다. 추가 테스트 사용자 또는 사용자 기본 라이선스 외에도 필요한 동시 파이프라인 수에 따라 추가 요금이 부과될 수 있습니다.

## <a name="related-resources"></a>관련 리소스

다음 리소스를 검토하여 CI/CD 및 Azure DevOps에 대해 자세히 알아보세요.

- [DevOps란?][devops-whatis]
- [Microsoft의 DevOps - Azure DevOps 사용 방법][devops-microsoft]
- [단계별 자습서: Azure DevOps를 사용한 DevOps][devops-with-vsts]
- [Devops 검사 목록][devops-checklist]
- [Azure DevOps 프로젝트를 사용하여 .NET용 CI/CD 파이프라인 만들기][devops-project-create]

<!-- links -->

[ansible]: /azure/ansible/
[application-insights]: /azure/application-insights/app-insights-overview
[app-service-reference-architecture]: ../../reference-architectures/app-service-web-app/basic-web-app.md
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
