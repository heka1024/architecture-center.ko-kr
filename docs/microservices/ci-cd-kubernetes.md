---
title: Kubernetes에서 마이크로 서비스에 대 한 CI/CD 파이프라인 빌드
description: Azure Kubernetes Service (AKS)를 마이크로 서비스를 배포 하는 것에 대 한 예제는 CI/CD 파이프라인을 설명 합니다.
author: MikeWasson
ms.date: 04/11/2019
ms.topic: guide
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: microservices
ms.openlocfilehash: e7da8a1b4111fb7856b919b40d033833a4e475e0
ms.sourcegitcommit: d58e6b2b891c9c99e951c59f15fce71addcb96b1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59533194"
---
<!-- markdownlint-disable MD040 -->

# <a name="building-a-cicd-pipeline-for-microservices-on-kubernetes"></a><span data-ttu-id="60baa-103">Kubernetes에서 마이크로 서비스에 대 한 CI/CD 파이프라인 빌드</span><span class="sxs-lookup"><span data-stu-id="60baa-103">Building a CI/CD pipeline for microservices on Kubernetes</span></span>

<span data-ttu-id="60baa-104">마이크로 서비스 아키텍처에 대 한 신뢰할 수 있는 CI/CD 프로세스를 만들지 어려울 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-104">It can be challenging to create a reliable CI/CD process for a microservices architecture.</span></span> <span data-ttu-id="60baa-105">개별 팀은 다른 팀을 방해 하거나 전체적으로 응용 프로그램을 불안정 않고도 빠르고 안정적으로 서비스를 해제할 수 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-105">Individual teams must be able to release services quickly and reliably, without disrupting other teams or destabilizing the application as a whole.</span></span>

<span data-ttu-id="60baa-106">이 문서에서는 Azure Kubernetes Service (AKS)를 마이크로 서비스를 배포 하는 것에 대 한 예제는 CI/CD 파이프라인을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-106">This article describes an example CI/CD pipeline for deploying microservices to Azure Kubernetes Service (AKS).</span></span> <span data-ttu-id="60baa-107">모든 팀과 프로젝트는 엄격한 규칙 집합으로이 문서를 사용 하지 있도록에 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-107">Every team and project is different, so don't take this article as a set of hard-and-fast rules.</span></span> <span data-ttu-id="60baa-108">대신 사용자 고유의 CI/CD 프로세스를 디자인 하기 위한 시작 지점에 것입니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-108">Instead, it's meant to be a starting point for designing your own CI/CD process.</span></span>

<span data-ttu-id="60baa-109">여기에 설명 된 예제에서는 파이프라인에서 찾을 수 있는 드 론 배달 응용 프로그램을 호출 하는 마이크로 서비스 참조 구현에 대해 만들어진 [GitHub][ri]합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-109">The example pipeline described here was created for a microservices reference implementation called the Drone Delivery application, which you can find on [GitHub][ri].</span></span> <span data-ttu-id="60baa-110">응용 프로그램 시나리오에 설명 되어 [여기](./design/index.md#reference-implementation)합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-110">The application scenario is described [here](./design/index.md#reference-implementation).</span></span>

<span data-ttu-id="60baa-111">파이프라인의 목표는 다음과 같이 요약할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-111">The goals of the pipeline can be summarized as follows:</span></span>

- <span data-ttu-id="60baa-112">팀 빌드 및 해당 서비스를 독립적으로 배포할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-112">Teams can build and deploy their services independently.</span></span>
- <span data-ttu-id="60baa-113">CI 프로세스를 통과 하는 코드 변경 내용이 프로덕션 환경과 유사한 환경에 자동으로 배포 됩니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-113">Code changes that pass the CI process are automatically deployed to a production-like environment.</span></span>
- <span data-ttu-id="60baa-114">품질 게이트는 파이프라인의 각 단계에 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-114">Quality gates are enforced at each stage of the pipeline.</span></span>
- <span data-ttu-id="60baa-115">서비스의 새 버전은 이전 버전과 함께 배포할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-115">A new version of a service can be deployed side by side with the previous version.</span></span>

<span data-ttu-id="60baa-116">자세한 배경 정보를 참조 하세요 [마이크로 서비스 아키텍처의 CI/CD](./ci-cd.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-116">For more background, see [CI/CD for microservices architectures](./ci-cd.md).</span></span>

## <a name="assumptions"></a><span data-ttu-id="60baa-117">가정</span><span class="sxs-lookup"><span data-stu-id="60baa-117">Assumptions</span></span>

<span data-ttu-id="60baa-118">이 예제에서는, 다음과 같습니다. 개발 팀과 코드에 대 한 몇 가지 가정을 기본</span><span class="sxs-lookup"><span data-stu-id="60baa-118">For purposes of this example, here are some assumptions about the development team and the code base:</span></span>

- <span data-ttu-id="60baa-119">코드 리포지토리는 마이크로 서비스 별로 폴더를 monorepo입니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-119">The code repository is a monorepo, with folders organized by microservice.</span></span>
- <span data-ttu-id="60baa-120">팀의 분기 전략이 [트렁크 기반 개발](https://trunkbaseddevelopment.com/)을 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-120">The team's branching strategy is based on [trunk-based development](https://trunkbaseddevelopment.com/).</span></span>
- <span data-ttu-id="60baa-121">팀에서는 [릴리스 분기를](/azure/devops/repos/git/git-branching-guidance?view=azure-devops#manage-releases) 릴리스를 관리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-121">The team uses [release branches](/azure/devops/repos/git/git-branching-guidance?view=azure-devops#manage-releases) to manage releases.</span></span> <span data-ttu-id="60baa-122">각 마이크로 서비스에 대 한 별도 릴리스 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-122">Separate releases are created for each microservice.</span></span>
- <span data-ttu-id="60baa-123">다음을 사용 하 여 CI/CD 프로세스 [Azure 파이프라인](/azure/devops/pipelines/?view=azure-devops) 을 빌드, 테스트 및 AKS에 마이크로 서비스를 배포 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-123">The CI/CD process uses [Azure Pipelines](/azure/devops/pipelines/?view=azure-devops) to build, test, and deploy the microservices to AKS.</span></span>
- <span data-ttu-id="60baa-124">각 마이크로 서비스에 대 한 컨테이너 이미지에 저장 됩니다 [Azure Container Registry](/azure/container-registry/)합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-124">The container images for each microservice are stored in [Azure Container Registry](/azure/container-registry/).</span></span>
- <span data-ttu-id="60baa-125">팀은 Helm 차트를 사용 하 여 각 마이크로 서비스를 패키지.</span><span class="sxs-lookup"><span data-stu-id="60baa-125">The team uses Helm charts to package each microservice.</span></span>

<span data-ttu-id="60baa-126">이러한 가정을 드라이브 다양 한 CI/CD 파이프라인의 특정 세부 정보입니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-126">These assumptions drive many of the specific details of the CI/CD pipeline.</span></span> <span data-ttu-id="60baa-127">그러나 여기에 설명 된 기본 방법은 다른 프로세스, 도구 및 Docker 허브 또는 Jenkins와 같은 서비스에 대 한 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-127">However, the basic approach described here be adapted for other processes, tools, and services, such as Jenkins or Docker Hub.</span></span>

## <a name="validation-builds"></a><span data-ttu-id="60baa-128">유효성 검사 빌드</span><span class="sxs-lookup"><span data-stu-id="60baa-128">Validation builds</span></span>

<span data-ttu-id="60baa-129">개발자는 배달 서비스를 호출 하는 마이크로 서비스에서 작동 하는지 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-129">Suppose that a developer is working on a microservice called the Delivery Service.</span></span> <span data-ttu-id="60baa-130">개발자는 새 기능을 개발할 때 기능 분기에 코드를 체크 인합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-130">While developing a new feature, the developer checks code into a feature branch.</span></span> <span data-ttu-id="60baa-131">관례상 기능 분기는 `feature/*`로 명명합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-131">By convention, feature branches are named `feature/*`.</span></span>

![CI/CD 워크플로](./images/aks-cicd-1.png)

<span data-ttu-id="60baa-133">분기 이름 및 원본 경로 기준으로 필터링 하는 트리거를 포함 하는 빌드 정의 파일:</span><span class="sxs-lookup"><span data-stu-id="60baa-133">The build definition file includes a trigger that filters by the branch name and the source path:</span></span>

```yaml
trigger:
  batch: true
  branches:
    include:
    - master
    - feature/*
    - topic/*

    exclude:
    - feature/experimental/*
    - topic/experimental/*

  paths:
     include:
     - /src/shipping/delivery/
```

<span data-ttu-id="60baa-134">&#11162;참조 된 [소스 파일](https://github.com/mspnp/microservices-reference-implementation/blob/master/src/shipping/delivery/azure-pipelines-validation.yml)합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-134">&#11162; See the [source file](https://github.com/mspnp/microservices-reference-implementation/blob/master/src/shipping/delivery/azure-pipelines-validation.yml).</span></span>

<span data-ttu-id="60baa-135">각 팀은 이러한 접근법을 사용하여 자체 빌드 파이프라인을 구축할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-135">Using this approach, each team can have its own build pipeline.</span></span> <span data-ttu-id="60baa-136">에 체크 인 된 코드만 `/src/shipping/delivery` 폴더 배달 서비스 빌드를 트리거합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-136">Only code that is checked into the `/src/shipping/delivery` folder triggers a build of the Delivery Service.</span></span> <span data-ttu-id="60baa-137">필터와 일치 하는 분기에 커밋을 푸시하는 CI 빌드를 트리거합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-137">Pushing commits to a branch that matches the filter triggers a CI build.</span></span> <span data-ttu-id="60baa-138">워크플로의 이 시점에서 CI 빌드는 최소한의 코드 확인을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-138">At this point in the workflow, the CI build runs some minimal code verification:</span></span>

1. <span data-ttu-id="60baa-139">코드를 작성 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-139">Build the code.</span></span>
1. <span data-ttu-id="60baa-140">단위 테스트를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-140">Run unit tests.</span></span>

<span data-ttu-id="60baa-141">목표는 개발자 빠른 피드백을 얻을 수 있도록 빌드 시간을 짧게 유지 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-141">The goal is to keep build times short, so the developer can get quick feedback.</span></span> <span data-ttu-id="60baa-142">기능 마스터 병합 준비 되 면 개발자가 열립니다를 확인 하세요.</span><span class="sxs-lookup"><span data-stu-id="60baa-142">Once the feature is ready to merge into master, the developer opens a PR.</span></span> <span data-ttu-id="60baa-143">그러면 일부 추가 검사를 수행하는 다른 CI 빌드가 트리거됩니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-143">This triggers another CI build that performs some additional checks:</span></span>

1. <span data-ttu-id="60baa-144">코드를 작성 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-144">Build the code.</span></span>
1. <span data-ttu-id="60baa-145">단위 테스트를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-145">Run unit tests.</span></span>
1. <span data-ttu-id="60baa-146">런타임 컨테이너 이미지를 빌드하십시오.</span><span class="sxs-lookup"><span data-stu-id="60baa-146">Build the runtime container image.</span></span>
1. <span data-ttu-id="60baa-147">이미지에서 취약성 검사를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-147">Run vulnerability scans on the image.</span></span>

![CI/CD 워크플로](./images/aks-cicd-2.png)

> [!NOTE]
> <span data-ttu-id="60baa-149">Azure DevOps 리포지토리에서 정의할 수 있습니다 [정책을](/azure/devops/repos/git/branch-policies) 분기를 보호 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-149">In Azure DevOps Repos, you can define [policies](/azure/devops/repos/git/branch-policies) to protect branches.</span></span> <span data-ttu-id="60baa-150">예를 들어 마스터에 병합하려면 정책에 승인자의 서명과 성공적인 CI 빌드가 필요할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-150">For example, the policy could require a successful CI build plus a sign-off from an approver in order to merge into master.</span></span>

## <a name="full-cicd-build"></a><span data-ttu-id="60baa-151">전체 CI/CD 빌드</span><span class="sxs-lookup"><span data-stu-id="60baa-151">Full CI/CD build</span></span>

<span data-ttu-id="60baa-152">어느 시점이 되면 팀은 새 버전의 Delivery Service를 배포할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-152">At some point, the team is ready to deploy a new version of the Delivery service.</span></span> <span data-ttu-id="60baa-153">릴리스 관리자는이 명명 패턴을 사용 하 여 마스터에서 분기를 만드는: `release/<microservice name>/<semver>`합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-153">The release manager creates a branch from master with this naming pattern: `release/<microservice name>/<semver>`.</span></span> <span data-ttu-id="60baa-154">예: `release/delivery/v1.0.2`</span><span class="sxs-lookup"><span data-stu-id="60baa-154">For example, `release/delivery/v1.0.2`.</span></span>

![CI/CD 워크플로](./images/aks-cicd-3.png)

<span data-ttu-id="60baa-156">이 분기의 생성 및 이전 단계 모두 실행 하는 전체 CI 빌드를 트리거:</span><span class="sxs-lookup"><span data-stu-id="60baa-156">Creation of this branch triggers a full CI build that runs all of the previous steps plus:</span></span>

1. <span data-ttu-id="60baa-157">Azure Container Registry에 컨테이너 이미지를 푸시하십시오.</span><span class="sxs-lookup"><span data-stu-id="60baa-157">Push the container image to Azure Container Registry.</span></span> <span data-ttu-id="60baa-158">분기 이름에서 가져온 버전 번호로 이미지에 태그가 지정됩니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-158">The image is tagged with the version number taken from the branch name.</span></span>
2. <span data-ttu-id="60baa-159">실행 `helm package` 서비스용 Helm 차트를 패키지 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-159">Run `helm package` to package the Helm chart for the service.</span></span> <span data-ttu-id="60baa-160">차트는 또한 버전 번호를 사용 하 여 태그가 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-160">The chart is also tagged with a version number.</span></span>
3. <span data-ttu-id="60baa-161">Helm 패키지를 컨테이너 레지스트리에 푸시하십시오.</span><span class="sxs-lookup"><span data-stu-id="60baa-161">Push the Helm package to Container Registry.</span></span>

<span data-ttu-id="60baa-162">Azure 파이프라인을 사용 하 여 배포 (CD) 프로세스를 트리거합니다를 가정 하 고이 빌드가 성공 [릴리스 파이프라인](/azure/devops/pipelines/release/what-is-release-management)합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-162">Assuming this build succeeds, it triggers a deployment (CD) process using an Azure Pipelines [release pipeline](/azure/devops/pipelines/release/what-is-release-management).</span></span> <span data-ttu-id="60baa-163">이 파이프라인에는 다음 단계에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-163">This pipeline has the following steps:</span></span>

1. <span data-ttu-id="60baa-164">Helm 차트를 QA 환경에 배포 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-164">Deploy the Helm chart to a QA environment.</span></span>
1. <span data-ttu-id="60baa-165">승인자가 서명한 후 패키지가 프로덕션 단계로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-165">An approver signs off before the package moves to production.</span></span> <span data-ttu-id="60baa-166">[승인을 통해 릴리스 배포 제어](/azure/devops/pipelines/release/approvals/approvals)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="60baa-166">See [Release deployment control using approvals](/azure/devops/pipelines/release/approvals/approvals).</span></span>
1. <span data-ttu-id="60baa-167">Azure Container Registry에서 프로덕션 네임 스페이스에 대 한 Docker 이미지 태그를 다시 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-167">Retag the Docker image for the production namespace in Azure Container Registry.</span></span> <span data-ttu-id="60baa-168">예를 들어 현재 태그가 `myrepo.azurecr.io/delivery:v1.0.2`라면 프로덕션 태그는 `myrepo.azurecr.io/prod/delivery:v1.0.2`입니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-168">For example, if the current tag is `myrepo.azurecr.io/delivery:v1.0.2`, the production tag is `myrepo.azurecr.io/prod/delivery:v1.0.2`.</span></span>
1. <span data-ttu-id="60baa-169">Helm 차트를 프로덕션 환경에 배포 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-169">Deploy the Helm chart to the production environment.</span></span>

<span data-ttu-id="60baa-170">팀이 높은 속도 사용 하 여 배포할 수 있도록 이러한 작업을 monorepo에도 개별 마이크로 서비스 범위 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-170">Even in a monorepo, these tasks can be scoped to individual microservices, so that teams can deploy with high velocity.</span></span> <span data-ttu-id="60baa-171">프로세스에 몇 가지 수동 단계가 있습니다. PR 승인, 릴리스 분기 만들기, 프로덕션 클러스터에 배포 승인.</span><span class="sxs-lookup"><span data-stu-id="60baa-171">The process has some manual steps: Approving PRs, creating release branches, and approving deployments into the production cluster.</span></span> <span data-ttu-id="60baa-172">이러한 단계는 정책에 따라 수동 &mdash; 조직에서 선호 하는 경우 자동화 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-172">These steps are manual by policy &mdash; they could be automated if the organization prefers.</span></span>

## <a name="isolation-of-environments"></a><span data-ttu-id="60baa-173">환경 격리</span><span class="sxs-lookup"><span data-stu-id="60baa-173">Isolation of environments</span></span>

<span data-ttu-id="60baa-174">고객은 개발, 스모크 테스트, 통합 테스트, 부하 테스트 및 프로덕션 환경을 비롯한 여러 환경에 서비스를 배포할 것입니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-174">You will have multiple environments where you deploy services, including environments for development, smoke testing, integration testing, load testing, and finally production.</span></span> <span data-ttu-id="60baa-175">이러한 환경에는 일정 수준의 격리가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-175">These environments need some level of isolation.</span></span> <span data-ttu-id="60baa-176">Kubernetes에서는 물리적 격리와 논리적 격리 중에서 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-176">In Kubernetes, you have a choice between physical isolation and logical isolation.</span></span> <span data-ttu-id="60baa-177">물리적 격리는 별도의 클러스터에 배포하는 것을 의미합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-177">Physical isolation means deploying to separate clusters.</span></span> <span data-ttu-id="60baa-178">논리적 격리는 앞에서 설명한 것처럼 네임스페이스와 정책을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-178">Logical isolation makes use of namespaces and policies, as described earlier.</span></span>

<span data-ttu-id="60baa-179">개발/테스트 환경에 사용할 별도의 클러스터와 함께 전용 프로덕션 클러스터를 만드는 방법을 권장합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-179">Our recommendation is to create a dedicated production cluster along with a separate cluster for your dev/test environments.</span></span> <span data-ttu-id="60baa-180">논리적 격리를 사용하여 개발/테스트 클러스터 내에서 별도의 환경을 격리하세요.</span><span class="sxs-lookup"><span data-stu-id="60baa-180">Use logical isolation to separate environments within the dev/test cluster.</span></span> <span data-ttu-id="60baa-181">개발/테스트 클러스터에 배포된 서비스는 비즈니스 데이터를 보관하는 데이터 저장소에 절대로 액세스하면 안 됩니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-181">Services deployed to the dev/test cluster should never have access to data stores that hold business data.</span></span>

## <a name="build-process"></a><span data-ttu-id="60baa-182">빌드 프로세스</span><span class="sxs-lookup"><span data-stu-id="60baa-182">Build process</span></span>

<span data-ttu-id="60baa-183">가능 하면 빌드 프로세스는 Docker 컨테이너로 패키지 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-183">When possible, package your build process into a Docker container.</span></span> <span data-ttu-id="60baa-184">옵션을 사용 하면 Docker를 사용 하 여 각 빌드 컴퓨터에서 빌드 환경을 구성할 필요 없이 코드 아티팩트를 빌드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-184">That allows you to build your code artifacts using Docker, without needing to configure the build environment on each build machine.</span></span> <span data-ttu-id="60baa-185">컨테이너 화 된 빌드 프로세스를 쉽게 규모 CI 파이프라인 새 빌드 에이전트를 추가 하 여 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-185">A containerized build process makes it easy to scale out the CI pipeline by adding new build agents.</span></span> <span data-ttu-id="60baa-186">또한 팀의 개발자 빌드 컨테이너를 실행 하면 코드를 빌드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-186">Also, any developer on the team can build the code simply by running the build container.</span></span>

<span data-ttu-id="60baa-187">다단계 빌드를 Docker에서 사용 하 여 단일 Dockerfile에서 빌드 환경 및 런타임 이미지를 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-187">By using multi-stage builds in Docker, you can define the build environment and the runtime image in a single Dockerfile.</span></span> <span data-ttu-id="60baa-188">예를 들어 다음과 같습니다. ASP.NET Core 응용 프로그램을 빌드하는 Dockerfile</span><span class="sxs-lookup"><span data-stu-id="60baa-188">For example, here's a Dockerfile that builds an ASP.NET Core application:</span></span>

```
FROM microsoft/dotnet:2.2-runtime AS base
WORKDIR /app

FROM microsoft/dotnet:2.2-sdk AS build
WORKDIR /src/Fabrikam.Workflow.Service

COPY Fabrikam.Workflow.Service/Fabrikam.Workflow.Service.csproj .
RUN dotnet restore Fabrikam.Workflow.Service.csproj

COPY Fabrikam.Workflow.Service/. .
RUN dotnet build Fabrikam.Workflow.Service.csproj -c Release -o /app

FROM build AS publish
RUN dotnet publish Fabrikam.Workflow.Service.csproj -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "Fabrikam.Workflow.Service.dll"]
```

<span data-ttu-id="60baa-189">&#11162;참조 된 [소스 파일](https://github.com/mspnp/microservices-reference-implementation/blob/master/src/shipping/workflow/Dockerfile)합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-189">&#11162; See the [source file](https://github.com/mspnp/microservices-reference-implementation/blob/master/src/shipping/workflow/Dockerfile).</span></span>

<span data-ttu-id="60baa-190">이 Dockerfile 여러 빌드 단계를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-190">This Dockerfile defines several build stages.</span></span> <span data-ttu-id="60baa-191">단계 라는 되었다는 `base` 에서 ASP.NET Core 런타임, 명명 된 스테이지 하는 동안 `build` 전체 ASP.NET Core SDK를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-191">Notice that the stage named `base` uses the ASP.NET Core runtime, while the stage named `build` uses the full ASP.NET Core SDK.</span></span> <span data-ttu-id="60baa-192">`build` ASP.NET Core 프로젝트를 빌드하려면 다음 단계에 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-192">The `build` stage is used to build the ASP.NET Core project.</span></span> <span data-ttu-id="60baa-193">최종 런타임 컨테이너에서 만들어집니다. 하지만 `base`, 런타임만 포함 하며 전체 SDK 이미지 보다 훨씬 작습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-193">But the final runtime container is built from `base`, which contains just the runtime and is significantly smaller than the full SDK image.</span></span>

### <a name="building-a-test-runner"></a><span data-ttu-id="60baa-194">Test runner를 작성합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-194">Building a test runner</span></span>

<span data-ttu-id="60baa-195">컨테이너에서 단위 테스트를 실행할 것도 좋은 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-195">Another good practice is to run unit tests in the container.</span></span> <span data-ttu-id="60baa-196">예를 들어 다음과 같습니다. test runner를 작성 하는 Docker 파일의 일부</span><span class="sxs-lookup"><span data-stu-id="60baa-196">For example, here is part of a Docker file that builds a test runner:</span></span>

```
FROM build AS testrunner
WORKDIR /src/tests

COPY Fabrikam.Workflow.Service.Tests/*.csproj .
RUN dotnet restore Fabrikam.Workflow.Service.Tests.csproj

COPY Fabrikam.Workflow.Service.Tests/. .
ENTRYPOINT ["dotnet", "test", "--logger:trx"]
```

<span data-ttu-id="60baa-197">개발자가 Docker 파일을 사용 하 여 테스트를 로컬로 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-197">A developer can use this Docker file to run the tests locally:</span></span>

```bash
docker build . -t delivery-test:1 --target=testrunner
docker run -p 8080:8080 delivery-test:1
```

<span data-ttu-id="60baa-198">또한 CI 파이프라인 빌드 확인 단계의 일부로 테스트를 실행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-198">The CI pipeline should also run the tests as part of the build verification step.</span></span>

<span data-ttu-id="60baa-199">이 파일은 Docker를 사용 하는 참고 `ENTRYPOINT` 테스트를 하지 Docker 실행 명령을 `RUN` 명령입니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-199">Note that this file uses the Docker `ENTRYPOINT` command to run the tests, not the Docker `RUN` command.</span></span>

- <span data-ttu-id="60baa-200">사용 하는 경우는 `RUN` 명령, 테스트가 실행 될 때마다 이미지를 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-200">If you use the `RUN` command, the tests run every time you build the image.</span></span> <span data-ttu-id="60baa-201">사용 하 여 `ENTRYPOINT`, 테스트 옵트인 됩니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-201">By using `ENTRYPOINT`, the tests are opt-in.</span></span> <span data-ttu-id="60baa-202">명시적으로 대상으로 할 때에 실행 합니다 `testrunner` 단계입니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-202">They run only when you explicitly target the `testrunner` stage.</span></span>
- <span data-ttu-id="60baa-203">실패 한 테스트 Docker 해도 `build` 명령이 실패 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-203">A failing test doesn't cause the Docker `build` command to fail.</span></span> <span data-ttu-id="60baa-204">이런 방식으로 컨테이너를 구분할 수 오류 테스트 실패를 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-204">That way you can distinguish container build failures from test failures.</span></span>
- <span data-ttu-id="60baa-205">탑재 된 볼륨 테스트 결과 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-205">Test results can be saved to a mounted volume.</span></span>

### <a name="container-best-practices"></a><span data-ttu-id="60baa-206">컨테이너에 대 한 유용한 정보</span><span class="sxs-lookup"><span data-stu-id="60baa-206">Container best practices</span></span>

<span data-ttu-id="60baa-207">컨테이너에 대해 고려해 야 할 몇 가지 다른 모범 사례는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-207">Here are some other best practices to consider for containers:</span></span>

- <span data-ttu-id="60baa-208">컨테이너 태그, 버전 관리에 대한 조직 전반의 규칙 및 클러스터(포드, 서비스 등)에 배포되는 리소스에 대한 명명 규칙을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-208">Define organization-wide conventions for container tags, versioning, and naming conventions for resources deployed to the cluster (pods, services, and so on).</span></span> <span data-ttu-id="60baa-209">배포 문제를 진단하기 쉽게 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-209">That can make it easier to diagnose deployment issues.</span></span>

- <span data-ttu-id="60baa-210">개발 및 테스트 주기 동안 CI/CD 프로세스는 많은 컨테이너 이미지를 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-210">During the development and test cycle, the CI/CD process will build many container images.</span></span> <span data-ttu-id="60baa-211">이러한 이미지 중 일부는 릴리스에 대 한 전용 차례로 일부 해당 릴리스 후보 프로덕션으로 승격 됩니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-211">Only some of those images are candidates for release, and then only some of those release candidates will get promoted to production.</span></span> <span data-ttu-id="60baa-212">어떤 이미지가 현재 프로덕션에 배포 되 고 필요한 경우 이전 버전으로 다시 롤백할 수를 알 수 있도록 명확한 버전 관리 전략을 사용 하는 경우</span><span class="sxs-lookup"><span data-stu-id="60baa-212">Have a clear versioning strategy, so that you know which images are currently deployed to production, and can roll back to a previous version if necessary.</span></span>

- <span data-ttu-id="60baa-213">항상 배포 특정 컨테이너 버전 태그 `latest`합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-213">Always deploy specific container version tags, not `latest`.</span></span>

- <span data-ttu-id="60baa-214">사용 하 여 [네임 스페이스](/azure/container-registry/container-registry-best-practices#repository-namespaces) 여전히 테스트 되는 이미지에서 프로덕션에 대 한 승인 된 이미지를 격리 하는 Azure Container Registry의 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-214">Use [namespaces](/azure/container-registry/container-registry-best-practices#repository-namespaces) in Azure Container Registry to isolate images that are approved for production from images that are still being tested.</span></span> <span data-ttu-id="60baa-215">프로덕션 환경에 배포할 준비가 될 때까지 이미지를 프로덕션 네임 스페이스로 이동 하지 마십시오.</span><span class="sxs-lookup"><span data-stu-id="60baa-215">Don't move an image into the production namespace until you're ready to deploy it into production.</span></span> <span data-ttu-id="60baa-216">이 연습을 컨테이너 이미지의 의미 체계 버전 관리에 결합하면 릴리스에 대해 승인되지 않은 버전을 실수로 배포하는 가능성을 줄일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-216">If you combine this practice with semantic versioning of container images, it can reduce the chance of accidentally deploying a version that wasn't approved for release.</span></span>

- <span data-ttu-id="60baa-217">권한 없는 사용자로 컨테이너를 실행 하 여 최소 권한의 원칙을 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-217">Follow the principle of least privilege by running containers as a nonprivileged user.</span></span> <span data-ttu-id="60baa-218">Kubernetes에서 컨테이너를 실행 하지 않도록 하는 pod 보안 정책을 만들 수 *루트*입니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-218">In Kubernetes, you can create a pod security policy that prevents containers from running as *root*.</span></span> <span data-ttu-id="60baa-219">참조 [Pod 루트 권한으로 실행 되지 않도록](https://docs.bitnami.com/kubernetes/how-to/secure-kubernetes-cluster-psp/)합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-219">See [Prevent Pods From Running With Root Privileges](https://docs.bitnami.com/kubernetes/how-to/secure-kubernetes-cluster-psp/).</span></span>

## <a name="helm-charts"></a><span data-ttu-id="60baa-220">Helm 차트</span><span class="sxs-lookup"><span data-stu-id="60baa-220">Helm charts</span></span>

<span data-ttu-id="60baa-221">Helm을 사용하여 서비스를 빌드하고 배포하는 방안을 고려해 보세요.</span><span class="sxs-lookup"><span data-stu-id="60baa-221">Consider using Helm to manage building and deploying services.</span></span> <span data-ttu-id="60baa-222">다음은 일부 CI/CD에 도움이 되는 Helm의 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-222">Here are some of the features of Helm that help with CI/CD:</span></span>

- <span data-ttu-id="60baa-223">종종 단일 마이크로 서비스는 여러 Kubernetes 개체에 의해 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-223">Often a single microservice is defined by multiple Kubernetes objects.</span></span> <span data-ttu-id="60baa-224">Helm에는 이러한 개체가 단일 Helm 차트에 패키지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-224">Helm allows these objects to be packaged into a single Helm chart.</span></span>
- <span data-ttu-id="60baa-225">차트는 일련의 kubectl 명령 보다는 단일 Helm 명령을 사용 하 여 배포할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-225">A chart can be deployed with a single Helm command, rather than a series of kubectl commands.</span></span>
- <span data-ttu-id="60baa-226">차트를 명시적으로 버전이 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-226">Charts are explicitly versioned.</span></span> <span data-ttu-id="60baa-227">Helm을 사용 하 여 버전, 릴리스, 보기 및 이전 버전으로 롤백합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-227">Use Helm to release a version, view releases, and roll back to a previous version.</span></span> <span data-ttu-id="60baa-228">이전 버전으로 롤백하는 기능과 함께 의미 체계 버전 관리를 사용하여 업데이트 및 수정 버전 추적.</span><span class="sxs-lookup"><span data-stu-id="60baa-228">Tracking updates and revisions, using semantic versioning, along with the ability to roll back to a previous version.</span></span>
- <span data-ttu-id="60baa-229">Helm 차트 레이블 및 선택기와 같은 정보를 여러 파일 중복을 피하기 위해 템플릿을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-229">Helm charts use templates to avoid duplicating information, such as labels and selectors, across many files.</span></span>
- <span data-ttu-id="60baa-230">Helm 차트 간의 종속성을 관리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-230">Helm can manage dependencies between charts.</span></span>
- <span data-ttu-id="60baa-231">차트, Azure Container Registry와 같은 Helm 리포지토리에 저장 하 고 빌드 파이프라인으로 통합 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-231">Charts can be stored in a Helm repository, such as Azure Container Registry, and integrated into the build pipeline.</span></span>

<span data-ttu-id="60baa-232">Container Registry를 Helm 리포지토리로 사용하는 방법에 대한 자세한 내용은 [애플리케이션 차트용 Helm 리포지토리로 Azure Container Registry 사용](/azure/container-registry/container-registry-helm-repos)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="60baa-232">For more information about using Container Registry as a Helm repository, see [Use Azure Container Registry as a Helm repository for your application charts](/azure/container-registry/container-registry-helm-repos).</span></span>

<span data-ttu-id="60baa-233">단일 마이크로 서비스는 여러 Kubernetes 구성 파일을 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-233">A single microservice may involve multiple Kubernetes configuration files.</span></span> <span data-ttu-id="60baa-234">서비스 업데이트 터치 선택기, 레이블 및 이미지 태그를 업데이트 하려면 이러한 파일의 모든 것을 의미할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-234">Updating a service can mean touching all of these files to update selectors, labels, and image tags.</span></span> <span data-ttu-id="60baa-235">Helm 차트 라고 하는 단일 패키지도 처리 하 고 쉽게 변수를 사용 하 여 YAML 파일을 업데이트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-235">Helm treats these as a single package called a chart and allows you to easily update the YAML files by using variables.</span></span> <span data-ttu-id="60baa-236">Helm (템플릿을 기반으로 이동) 하는 템플릿 언어를 사용 하 여 매개 변수가 있는 YAML 구성 파일을 작성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-236">Helm uses a template language (based on Go templates) to let you write parameterized YAML configuration files.</span></span>

<span data-ttu-id="60baa-237">예를 들어 다음과 같습니다. 배포를 정의 하는 YAML 파일의 일부</span><span class="sxs-lookup"><span data-stu-id="60baa-237">For example, here's part of a YAML file that defines a deployment:</span></span>

```yaml
apiVersion: apps/v1beta2
kind: Deployment
metadata:
  name: {{ include "package.fullname" . | replace "." "" }}
  labels:
    app.kubernetes.io/name: {{ include "package.name" . }}
    app.kubernetes.io/instance: {{ .Release.Name }}
  annotations:
    kubernetes.io/change-cause: {{ .Values.reason }}

...

  spec:
      containers:
      - name: &package-container_name fabrikam-package
        image: {{ .Values.dockerregistry }}/{{ .Values.image.repository }}:{{ .Values.image.tag }}
        imagePullPolicy: {{ .Values.image.pullPolicy }}
        env:
        - name: LOG_LEVEL
          value: {{ .Values.log.level }}
```

<span data-ttu-id="60baa-238">&#11162;참조 된 [소스 파일](https://github.com/mspnp/microservices-reference-implementation/blob/master/charts/package/templates/package-deploy.yaml)합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-238">&#11162; See the [source file](https://github.com/mspnp/microservices-reference-implementation/blob/master/charts/package/templates/package-deploy.yaml).</span></span>

<span data-ttu-id="60baa-239">배포 이름, 레이블 및 컨테이너에 사용 하 여 템플릿 매개 변수를 모두 배포 시에 제공 되는 사양 있는지 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-239">You can see that the deployment name, labels, and container spec all use template parameters, which are provided at deployment time.</span></span> <span data-ttu-id="60baa-240">예를 들어 다음 명령줄에서</span><span class="sxs-lookup"><span data-stu-id="60baa-240">For example, from the command line:</span></span>

```bash
helm install $HELM_CHARTS/package/ \
     --set image.tag=0.1.0 \
     --set image.repository=package \
     --set dockerregistry=$ACR_SERVER \
     --namespace backend \
     --name package-v0.1.0
```

<span data-ttu-id="60baa-241">CI/CD 파이프라인에는 Kubernetes에 직접 차트를 설치할 수 없습니다, 있지만 차트 보관 파일 (.tgz 파일)을 만들고 차트를 Helm 리포지토리에 Azure Container Registry와 같은 푸시하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-241">Although your CI/CD pipeline could install a chart directly to Kubernetes, we recommend creating a chart archive (.tgz file) and pushing the chart to a Helm repository such as Azure Container Registry.</span></span> <span data-ttu-id="60baa-242">자세한 내용은 [Helm 차트 Azure 파이프라인에서의 패키지 Docker 기반 응용 프로그램](/azure/devops/pipelines/languages/helm?view=azure-devops)합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-242">For more information, see [Package Docker-based apps in Helm charts in Azure Pipelines](/azure/devops/pipelines/languages/helm?view=azure-devops).</span></span>

<span data-ttu-id="60baa-243">Helm 자체 네임 스페이스를 배포 및 역할 기반 액세스 제어 (RBAC)를 사용 하 여 배포할 수 있습니다 하는 네임 스페이스를 제한 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-243">Consider deploying Helm to its own namespace and using role-based access control (RBAC) to restrict which namespaces it can deploy to.</span></span> <span data-ttu-id="60baa-244">자세한 내용은 [역할 기반 Access Control](https://helm.sh/docs/using_helm/#helm-and-role-based-access-control) Helm 설명서에서.</span><span class="sxs-lookup"><span data-stu-id="60baa-244">For more information, see [Role-based Access Control](https://helm.sh/docs/using_helm/#helm-and-role-based-access-control) in the Helm documentation.</span></span>

### <a name="revisions"></a><span data-ttu-id="60baa-245">수정</span><span class="sxs-lookup"><span data-stu-id="60baa-245">Revisions</span></span>

<span data-ttu-id="60baa-246">Helm 차트에는 사용 해야 하는 버전 번호를 항상 [유의 적 버전](https://semver.org/)합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-246">Helm charts always have a version number, which must use [semantic versioning](https://semver.org/).</span></span> <span data-ttu-id="60baa-247">차트 수도 있습니다는 `appVersion`합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-247">A chart can also have an `appVersion`.</span></span> <span data-ttu-id="60baa-248">이 필드는 선택 사항이 며 차트 버전 관련 될 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-248">This field is optional, and doesn't have to be related to the chart version.</span></span> <span data-ttu-id="60baa-249">일부 팀 할 응용 프로그램 버전 개별적으로 차트에 대 한 업데이트에서.</span><span class="sxs-lookup"><span data-stu-id="60baa-249">Some teams might want to application versions separately from updates to the charts.</span></span> <span data-ttu-id="60baa-250">하지만 차트 버전 및 응용 프로그램 버전 간에 1:1 관계 이므로 한 버전 번호를 사용 하는 간단 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-250">But a simpler approach is to use one version number, so there's a 1:1 relation between chart version and application version.</span></span> <span data-ttu-id="60baa-251">이런 방식으로 릴리스 당 하나의 차트를 저장 한 원하는 릴리스를 쉽게 배포할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-251">That way, you can store one chart per release and easily deploy the desired release:</span></span>

```bash
helm install <package-chart-name> --version <desiredVersion>
```

<span data-ttu-id="60baa-252">배포 템플릿에 변경 원인의 주석을 제공 하려면 것도 좋은 방법:</span><span class="sxs-lookup"><span data-stu-id="60baa-252">Another good practice is to provide a change-cause annotation in the deployment template:</span></span>

```yaml
apiVersion: apps/v1beta2
kind: Deployment
metadata:
  name: {{ include "delivery.fullname" . | replace "." "" }}
  labels:
     ...
  annotations:
    kubernetes.io/change-cause: {{ .Values.reason }}
```

<span data-ttu-id="60baa-253">각 수정 버전의 변경 원인을 필드를 볼 수 있습니다 사용 하는 `kubectl rollout history` 명령입니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-253">This lets you view the change-cause field for each revision, using the `kubectl rollout history` command.</span></span> <span data-ttu-id="60baa-254">이전 예에서의 변경 원인은 Helm 차트 매개 변수로 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-254">In the previous example, the change-cause is provided as a Helm chart parameter.</span></span>

```bash
$ kubectl rollout history deployments/delivery-v010 -n backend
deployment.extensions/delivery-v010
REVISION  CHANGE-CAUSE
1         Initial deployment
```

<span data-ttu-id="60baa-255">사용할 수도 있습니다는 `helm list` 수정 기록을 보려면 명령을:</span><span class="sxs-lookup"><span data-stu-id="60baa-255">You can also use the `helm list` command to view the revision history:</span></span>

```bash
~$ helm list
NAME            REVISION    UPDATED                     STATUS        CHART            APP VERSION     NAMESPACE
delivery-v0.1.0 1           Sun Apr  7 00:25:30 2019    DEPLOYED      delivery-v0.1.0  v0.1.0          backend
```

> [!TIP]
> <span data-ttu-id="60baa-256">사용 된 `--history-max` Helm을 초기화할 때 플래그입니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-256">Use the `--history-max` flag when initializing Helm.</span></span> <span data-ttu-id="60baa-257">이 설정은 수정 Tiller 기록에 저장 하는 횟수를 제한 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-257">This setting limits the number of revisions that Tiller saves in its history.</span></span> <span data-ttu-id="60baa-258">Tiller configmaps 수정 기록을 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-258">Tiller stores revision history in configmaps.</span></span> <span data-ttu-id="60baa-259">자주 업데이트를 해제 하는 경우 기록 크기를 제한 하지 않는 이상는 configmaps 매우 큰 증가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-259">If you're releasing updates frequently, the configmaps can grow very large unless you limit the history size.</span></span>

## <a name="azure-devops-pipeline"></a><span data-ttu-id="60baa-260">Azure DevOps 파이프라인</span><span class="sxs-lookup"><span data-stu-id="60baa-260">Azure DevOps Pipeline</span></span>

<span data-ttu-id="60baa-261">Azure 파이프라인에서 파이프라인으로 나뉩니다 *파이프라인을 빌드합니다* 하 고 *릴리스 파이프라인*합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-261">In Azure Pipelines, pipelines are divided into *build pipelines* and *release pipelines*.</span></span> <span data-ttu-id="60baa-262">빌드 파이프라인 CI 프로세스를 실행 하 고 빌드 아티팩트를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-262">The build pipeline runs the CI process and creates build artifacts.</span></span> <span data-ttu-id="60baa-263">Kubernetes에서 마이크로 서비스 아키텍처에 대 한 이러한 아티팩트는 컨테이너 이미지와 각 마이크로 서비스를 정의 하는 Helm 차트입니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-263">For a microservices architecture on Kubernetes, these artifacts are the container images and Helm charts that define each microservice.</span></span> <span data-ttu-id="60baa-264">릴리스 파이프라인을 클러스터에 마이크로 서비스를 배포 하는 CD 프로세스를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-264">The release pipeline runs that CD process that deploys a microservice into a cluster.</span></span>

<span data-ttu-id="60baa-265">이 문서의 앞부분에서 설명 하는 CI 흐름에 따라 빌드 파이프라인의 다음 태스크를 구성 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-265">Based on the CI flow described earlier in this article, a build pipeline might consist of the following tasks:</span></span>

1. <span data-ttu-id="60baa-266">테스트 러너 컨테이너를 빌드하십시오.</span><span class="sxs-lookup"><span data-stu-id="60baa-266">Build the test runner container.</span></span>

    ```yaml
    - task: Docker@1
      inputs:
        azureSubscriptionEndpoint: $(AzureSubscription)
        azureContainerRegistry: $(AzureContainerRegistry)
        arguments: '--pull --target testrunner'
        dockerFile: $(System.DefaultWorkingDirectory)/$(dockerFileName)
        imageName: '$(imageName)-test'
    ```

1. <span data-ttu-id="60baa-267">테스트 러너 컨테이너에 대해 실행 되는 docker를 호출 하 여 테스트를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-267">Run the tests, by invoking docker run against the test runner container.</span></span>

    ```yaml
    - task: Docker@1
      inputs:
        azureSubscriptionEndpoint: $(AzureSubscription)
        azureContainerRegistry: $(AzureContainerRegistry)
        command: 'run'
        containerName: testrunner
        volumes: '$(System.DefaultWorkingDirectory)/TestResults:/app/tests/TestResults'
        imageName: '$(imageName)-test'
        runInBackground: false
    ```

1. <span data-ttu-id="60baa-268">테스트 결과 게시 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-268">Publish the test results.</span></span> <span data-ttu-id="60baa-269">참조 [빌드를 통합 하 고 작업을 테스트](/azure/devops/pipelines/languages/docker?view=azure-devops&tabs=yaml#integrate-build-and-test-tasks)합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-269">See [Integrate build and test tasks](/azure/devops/pipelines/languages/docker?view=azure-devops&tabs=yaml#integrate-build-and-test-tasks).</span></span>

    ```yaml
    - task: PublishTestResults@2
      inputs:
        testResultsFormat: 'VSTest'
        testResultsFiles: 'TestResults/*.trx'
        searchFolder: '$(System.DefaultWorkingDirectory)'
        publishRunAttachments: true
    ```

1. <span data-ttu-id="60baa-270">런타임 컨테이너를 빌드하십시오.</span><span class="sxs-lookup"><span data-stu-id="60baa-270">Build the runtime container.</span></span>

    ```yaml
    - task: Docker@1
      inputs:
        azureSubscriptionEndpoint: $(AzureSubscription)
        azureContainerRegistry: $(AzureContainerRegistry)
        dockerFile: $(System.DefaultWorkingDirectory)/$(dockerFileName)
        includeLatestTag: false
        imageName: '$(imageName)'
    ```

1. <span data-ttu-id="60baa-271">Azure Container Registry (또는 다른 컨테이너 레지스트리)에 컨테이너를 푸시하십시오.</span><span class="sxs-lookup"><span data-stu-id="60baa-271">Push to the container to Azure Container Registry (or other container registry).</span></span>

    ```yaml
    - task: Docker@1
      inputs:
        azureSubscriptionEndpoint: $(AzureSubscription)
        azureContainerRegistry: $(AzureContainerRegistry)
        command: 'Push an image'
        imageName: '$(imageName)'
        includeSourceTags: false
    ```

1. <span data-ttu-id="60baa-272">Helm 차트를 패키지 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-272">Package the Helm chart.</span></span>

    ```yaml
    - task: HelmDeploy@0
      inputs:
        command: package
        chartPath: $(chartPath)
        chartVersion: $(Build.SourceBranchName)
        arguments: '--app-version $(Build.SourceBranchName)'
    ```

1. <span data-ttu-id="60baa-273">Azure Container Registry (또는 기타 Helm 리포지토리에) Helm 패키지를 푸시하십시오.</span><span class="sxs-lookup"><span data-stu-id="60baa-273">Push the Helm package to Azure Container Registry (or other Helm repository).</span></span>

    ```yaml
    task: AzureCLI@1
      inputs:
        azureSubscription: $(AzureSubscription)
        scriptLocation: inlineScript
        inlineScript: |
        az acr helm push $(System.ArtifactsDirectory)/$(repositoryName)-$(Build.SourceBranchName).tgz --name $(AzureContainerRegistry);
    ```

<span data-ttu-id="60baa-274">&#11162;참조 된 [소스 파일](https://github.com/mspnp/microservices-reference-implementation/blob/master/src/shipping/delivery/azure-pipelines-ci.yml)합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-274">&#11162; See the [source file](https://github.com/mspnp/microservices-reference-implementation/blob/master/src/shipping/delivery/azure-pipelines-ci.yml).</span></span>

<span data-ttu-id="60baa-275">CI 파이프라인에서 출력에는 프로덕션이 준비 된 컨테이너 이미지 및 마이크로 서비스에 대 한 업데이트 된 Helm 차트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-275">The output from the CI pipeline is a production-ready container image and an updated Helm chart for the microservice.</span></span> <span data-ttu-id="60baa-276">이 시점에서 릴리스 파이프라인을 통해 걸릴 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-276">At this point, the release pipeline can take over.</span></span> <span data-ttu-id="60baa-277">다음 단계를 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-277">It performs the following steps:</span></span>

- <span data-ttu-id="60baa-278">개발/QA/스테이징 환경에 배포 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-278">Deploy to dev/QA/staging environments.</span></span>
- <span data-ttu-id="60baa-279">승인자가 승인 하거나 거부할 배포 될 때까지 기다립니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-279">Wait for an approver to approve or reject the deployment.</span></span>
- <span data-ttu-id="60baa-280">릴리스에 대 한 컨테이너 이미지 태그를 다시 지정</span><span class="sxs-lookup"><span data-stu-id="60baa-280">Retag the container image for release</span></span>
- <span data-ttu-id="60baa-281">릴리스 태그를 컨테이너 레지스트리에 푸시하십시오.</span><span class="sxs-lookup"><span data-stu-id="60baa-281">Push the release tag to the container registry.</span></span>
- <span data-ttu-id="60baa-282">프로덕션 클러스터에 Helm 차트를 업그레이드 합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-282">Upgrade the Helm chart in the production cluster.</span></span>

<span data-ttu-id="60baa-283">릴리스 파이프라인을 만드는 방법에 대 한 자세한 내용은 참조 하세요. [릴리스 파이프라인 초안 릴리스 및 릴리스 옵션](/azure/devops/pipelines/release/?view=azure-devops)합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-283">For more information about creating a release pipeline, see [Release pipelines, draft releases, and release options](/azure/devops/pipelines/release/?view=azure-devops).</span></span>

<span data-ttu-id="60baa-284">다음 다이어그램은이 문서에 설명 된 종단 간 CI/CD 프로세스를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-284">The following diagram shows the end-to-end CI/CD process described in this article:</span></span>

![CD/CD 파이프라인](./images/aks-cicd-flow.png)

## <a name="next-steps"></a><span data-ttu-id="60baa-286">다음 단계</span><span class="sxs-lookup"><span data-stu-id="60baa-286">Next steps</span></span>

<span data-ttu-id="60baa-287">이 문서에서 찾을 수 있는 참조 구현을 살펴보았습니다 [GitHub][ri]합니다.</span><span class="sxs-lookup"><span data-stu-id="60baa-287">This article was based on a reference implementation that you can find on [GitHub][ri].</span></span>

[ri]: https://github.com/mspnp/microservices-reference-implementation