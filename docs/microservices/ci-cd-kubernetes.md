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

# <a name="building-a-cicd-pipeline-for-microservices-on-kubernetes"></a>Kubernetes에서 마이크로 서비스에 대 한 CI/CD 파이프라인 빌드

마이크로 서비스 아키텍처에 대 한 신뢰할 수 있는 CI/CD 프로세스를 만들지 어려울 수 있습니다. 개별 팀은 다른 팀을 방해 하거나 전체적으로 응용 프로그램을 불안정 않고도 빠르고 안정적으로 서비스를 해제할 수 있어야 합니다.

이 문서에서는 Azure Kubernetes Service (AKS)를 마이크로 서비스를 배포 하는 것에 대 한 예제는 CI/CD 파이프라인을 설명 합니다. 모든 팀과 프로젝트는 엄격한 규칙 집합으로이 문서를 사용 하지 있도록에 다릅니다. 대신 사용자 고유의 CI/CD 프로세스를 디자인 하기 위한 시작 지점에 것입니다.

여기에 설명 된 예제에서는 파이프라인에서 찾을 수 있는 드 론 배달 응용 프로그램을 호출 하는 마이크로 서비스 참조 구현에 대해 만들어진 [GitHub][ri]합니다. 응용 프로그램 시나리오에 설명 되어 [여기](./design/index.md#reference-implementation)합니다.

파이프라인의 목표는 다음과 같이 요약할 수 있습니다.

- 팀 빌드 및 해당 서비스를 독립적으로 배포할 수 있습니다.
- CI 프로세스를 통과 하는 코드 변경 내용이 프로덕션 환경과 유사한 환경에 자동으로 배포 됩니다.
- 품질 게이트는 파이프라인의 각 단계에 적용 됩니다.
- 서비스의 새 버전은 이전 버전과 함께 배포할 수 있습니다.

자세한 배경 정보를 참조 하세요 [마이크로 서비스 아키텍처의 CI/CD](./ci-cd.md)합니다.

## <a name="assumptions"></a>가정

이 예제에서는, 다음과 같습니다. 개발 팀과 코드에 대 한 몇 가지 가정을 기본

- 코드 리포지토리는 마이크로 서비스 별로 폴더를 monorepo입니다.
- 팀의 분기 전략이 [트렁크 기반 개발](https://trunkbaseddevelopment.com/)을 기반으로 합니다.
- 팀에서는 [릴리스 분기를](/azure/devops/repos/git/git-branching-guidance?view=azure-devops#manage-releases) 릴리스를 관리할 수 있습니다. 각 마이크로 서비스에 대 한 별도 릴리스 생성 됩니다.
- 다음을 사용 하 여 CI/CD 프로세스 [Azure 파이프라인](/azure/devops/pipelines/?view=azure-devops) 을 빌드, 테스트 및 AKS에 마이크로 서비스를 배포 합니다.
- 각 마이크로 서비스에 대 한 컨테이너 이미지에 저장 됩니다 [Azure Container Registry](/azure/container-registry/)합니다.
- 팀은 Helm 차트를 사용 하 여 각 마이크로 서비스를 패키지.

이러한 가정을 드라이브 다양 한 CI/CD 파이프라인의 특정 세부 정보입니다. 그러나 여기에 설명 된 기본 방법은 다른 프로세스, 도구 및 Docker 허브 또는 Jenkins와 같은 서비스에 대 한 적용할 수 있습니다.

## <a name="validation-builds"></a>유효성 검사 빌드

개발자는 배달 서비스를 호출 하는 마이크로 서비스에서 작동 하는지 가정 합니다. 개발자는 새 기능을 개발할 때 기능 분기에 코드를 체크 인합니다. 관례상 기능 분기는 `feature/*`로 명명합니다.

![CI/CD 워크플로](./images/aks-cicd-1.png)

분기 이름 및 원본 경로 기준으로 필터링 하는 트리거를 포함 하는 빌드 정의 파일:

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

&#11162;참조 된 [소스 파일](https://github.com/mspnp/microservices-reference-implementation/blob/master/src/shipping/delivery/azure-pipelines-validation.yml)합니다.

각 팀은 이러한 접근법을 사용하여 자체 빌드 파이프라인을 구축할 수 있습니다. 에 체크 인 된 코드만 `/src/shipping/delivery` 폴더 배달 서비스 빌드를 트리거합니다. 필터와 일치 하는 분기에 커밋을 푸시하는 CI 빌드를 트리거합니다. 워크플로의 이 시점에서 CI 빌드는 최소한의 코드 확인을 실행합니다.

1. 코드를 작성 합니다.
1. 단위 테스트를 실행 합니다.

목표는 개발자 빠른 피드백을 얻을 수 있도록 빌드 시간을 짧게 유지 하는 것입니다. 기능 마스터 병합 준비 되 면 개발자가 열립니다를 확인 하세요. 그러면 일부 추가 검사를 수행하는 다른 CI 빌드가 트리거됩니다.

1. 코드를 작성 합니다.
1. 단위 테스트를 실행 합니다.
1. 런타임 컨테이너 이미지를 빌드하십시오.
1. 이미지에서 취약성 검사를 실행 합니다.

![CI/CD 워크플로](./images/aks-cicd-2.png)

> [!NOTE]
> Azure DevOps 리포지토리에서 정의할 수 있습니다 [정책을](/azure/devops/repos/git/branch-policies) 분기를 보호 합니다. 예를 들어 마스터에 병합하려면 정책에 승인자의 서명과 성공적인 CI 빌드가 필요할 수 있습니다.

## <a name="full-cicd-build"></a>전체 CI/CD 빌드

어느 시점이 되면 팀은 새 버전의 Delivery Service를 배포할 수 있습니다. 릴리스 관리자는이 명명 패턴을 사용 하 여 마스터에서 분기를 만드는: `release/<microservice name>/<semver>`합니다. 예: `release/delivery/v1.0.2`

![CI/CD 워크플로](./images/aks-cicd-3.png)

이 분기의 생성 및 이전 단계 모두 실행 하는 전체 CI 빌드를 트리거:

1. Azure Container Registry에 컨테이너 이미지를 푸시하십시오. 분기 이름에서 가져온 버전 번호로 이미지에 태그가 지정됩니다.
2. 실행 `helm package` 서비스용 Helm 차트를 패키지 합니다. 차트는 또한 버전 번호를 사용 하 여 태그가 지정 됩니다.
3. Helm 패키지를 컨테이너 레지스트리에 푸시하십시오.

Azure 파이프라인을 사용 하 여 배포 (CD) 프로세스를 트리거합니다를 가정 하 고이 빌드가 성공 [릴리스 파이프라인](/azure/devops/pipelines/release/what-is-release-management)합니다. 이 파이프라인에는 다음 단계에 있습니다.

1. Helm 차트를 QA 환경에 배포 합니다.
1. 승인자가 서명한 후 패키지가 프로덕션 단계로 이동합니다. [승인을 통해 릴리스 배포 제어](/azure/devops/pipelines/release/approvals/approvals)를 참조하세요.
1. Azure Container Registry에서 프로덕션 네임 스페이스에 대 한 Docker 이미지 태그를 다시 지정 합니다. 예를 들어 현재 태그가 `myrepo.azurecr.io/delivery:v1.0.2`라면 프로덕션 태그는 `myrepo.azurecr.io/prod/delivery:v1.0.2`입니다.
1. Helm 차트를 프로덕션 환경에 배포 합니다.

팀이 높은 속도 사용 하 여 배포할 수 있도록 이러한 작업을 monorepo에도 개별 마이크로 서비스 범위 수 있습니다. 프로세스에 몇 가지 수동 단계가 있습니다. PR 승인, 릴리스 분기 만들기, 프로덕션 클러스터에 배포 승인. 이러한 단계는 정책에 따라 수동 &mdash; 조직에서 선호 하는 경우 자동화 될 수 있습니다.

## <a name="isolation-of-environments"></a>환경 격리

고객은 개발, 스모크 테스트, 통합 테스트, 부하 테스트 및 프로덕션 환경을 비롯한 여러 환경에 서비스를 배포할 것입니다. 이러한 환경에는 일정 수준의 격리가 필요합니다. Kubernetes에서는 물리적 격리와 논리적 격리 중에서 선택할 수 있습니다. 물리적 격리는 별도의 클러스터에 배포하는 것을 의미합니다. 논리적 격리는 앞에서 설명한 것처럼 네임스페이스와 정책을 사용합니다.

개발/테스트 환경에 사용할 별도의 클러스터와 함께 전용 프로덕션 클러스터를 만드는 방법을 권장합니다. 논리적 격리를 사용하여 개발/테스트 클러스터 내에서 별도의 환경을 격리하세요. 개발/테스트 클러스터에 배포된 서비스는 비즈니스 데이터를 보관하는 데이터 저장소에 절대로 액세스하면 안 됩니다.

## <a name="build-process"></a>빌드 프로세스

가능 하면 빌드 프로세스는 Docker 컨테이너로 패키지 합니다. 옵션을 사용 하면 Docker를 사용 하 여 각 빌드 컴퓨터에서 빌드 환경을 구성할 필요 없이 코드 아티팩트를 빌드할 수 있습니다. 컨테이너 화 된 빌드 프로세스를 쉽게 규모 CI 파이프라인 새 빌드 에이전트를 추가 하 여 합니다. 또한 팀의 개발자 빌드 컨테이너를 실행 하면 코드를 빌드할 수 있습니다.

다단계 빌드를 Docker에서 사용 하 여 단일 Dockerfile에서 빌드 환경 및 런타임 이미지를 정의할 수 있습니다. 예를 들어 다음과 같습니다. ASP.NET Core 응용 프로그램을 빌드하는 Dockerfile

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

&#11162;참조 된 [소스 파일](https://github.com/mspnp/microservices-reference-implementation/blob/master/src/shipping/workflow/Dockerfile)합니다.

이 Dockerfile 여러 빌드 단계를 정의합니다. 단계 라는 되었다는 `base` 에서 ASP.NET Core 런타임, 명명 된 스테이지 하는 동안 `build` 전체 ASP.NET Core SDK를 사용 합니다. `build` ASP.NET Core 프로젝트를 빌드하려면 다음 단계에 사용 됩니다. 최종 런타임 컨테이너에서 만들어집니다. 하지만 `base`, 런타임만 포함 하며 전체 SDK 이미지 보다 훨씬 작습니다.

### <a name="building-a-test-runner"></a>Test runner를 작성합니다.

컨테이너에서 단위 테스트를 실행할 것도 좋은 방법입니다. 예를 들어 다음과 같습니다. test runner를 작성 하는 Docker 파일의 일부

```
FROM build AS testrunner
WORKDIR /src/tests

COPY Fabrikam.Workflow.Service.Tests/*.csproj .
RUN dotnet restore Fabrikam.Workflow.Service.Tests.csproj

COPY Fabrikam.Workflow.Service.Tests/. .
ENTRYPOINT ["dotnet", "test", "--logger:trx"]
```

개발자가 Docker 파일을 사용 하 여 테스트를 로컬로 실행할 수 있습니다.

```bash
docker build . -t delivery-test:1 --target=testrunner
docker run -p 8080:8080 delivery-test:1
```

또한 CI 파이프라인 빌드 확인 단계의 일부로 테스트를 실행 해야 합니다.

이 파일은 Docker를 사용 하는 참고 `ENTRYPOINT` 테스트를 하지 Docker 실행 명령을 `RUN` 명령입니다.

- 사용 하는 경우는 `RUN` 명령, 테스트가 실행 될 때마다 이미지를 빌드합니다. 사용 하 여 `ENTRYPOINT`, 테스트 옵트인 됩니다. 명시적으로 대상으로 할 때에 실행 합니다 `testrunner` 단계입니다.
- 실패 한 테스트 Docker 해도 `build` 명령이 실패 합니다. 이런 방식으로 컨테이너를 구분할 수 오류 테스트 실패를 빌드합니다.
- 탑재 된 볼륨 테스트 결과 저장할 수 있습니다.

### <a name="container-best-practices"></a>컨테이너에 대 한 유용한 정보

컨테이너에 대해 고려해 야 할 몇 가지 다른 모범 사례는 다음과 같습니다.

- 컨테이너 태그, 버전 관리에 대한 조직 전반의 규칙 및 클러스터(포드, 서비스 등)에 배포되는 리소스에 대한 명명 규칙을 정의합니다. 배포 문제를 진단하기 쉽게 만들 수 있습니다.

- 개발 및 테스트 주기 동안 CI/CD 프로세스는 많은 컨테이너 이미지를 빌드합니다. 이러한 이미지 중 일부는 릴리스에 대 한 전용 차례로 일부 해당 릴리스 후보 프로덕션으로 승격 됩니다. 어떤 이미지가 현재 프로덕션에 배포 되 고 필요한 경우 이전 버전으로 다시 롤백할 수를 알 수 있도록 명확한 버전 관리 전략을 사용 하는 경우

- 항상 배포 특정 컨테이너 버전 태그 `latest`합니다.

- 사용 하 여 [네임 스페이스](/azure/container-registry/container-registry-best-practices#repository-namespaces) 여전히 테스트 되는 이미지에서 프로덕션에 대 한 승인 된 이미지를 격리 하는 Azure Container Registry의 합니다. 프로덕션 환경에 배포할 준비가 될 때까지 이미지를 프로덕션 네임 스페이스로 이동 하지 마십시오. 이 연습을 컨테이너 이미지의 의미 체계 버전 관리에 결합하면 릴리스에 대해 승인되지 않은 버전을 실수로 배포하는 가능성을 줄일 수 있습니다.

- 권한 없는 사용자로 컨테이너를 실행 하 여 최소 권한의 원칙을 따릅니다. Kubernetes에서 컨테이너를 실행 하지 않도록 하는 pod 보안 정책을 만들 수 *루트*입니다. 참조 [Pod 루트 권한으로 실행 되지 않도록](https://docs.bitnami.com/kubernetes/how-to/secure-kubernetes-cluster-psp/)합니다.

## <a name="helm-charts"></a>Helm 차트

Helm을 사용하여 서비스를 빌드하고 배포하는 방안을 고려해 보세요. 다음은 일부 CI/CD에 도움이 되는 Helm의 기능입니다.

- 종종 단일 마이크로 서비스는 여러 Kubernetes 개체에 의해 정의 됩니다. Helm에는 이러한 개체가 단일 Helm 차트에 패키지할 수 있습니다.
- 차트는 일련의 kubectl 명령 보다는 단일 Helm 명령을 사용 하 여 배포할 수 있습니다.
- 차트를 명시적으로 버전이 지정 됩니다. Helm을 사용 하 여 버전, 릴리스, 보기 및 이전 버전으로 롤백합니다. 이전 버전으로 롤백하는 기능과 함께 의미 체계 버전 관리를 사용하여 업데이트 및 수정 버전 추적.
- Helm 차트 레이블 및 선택기와 같은 정보를 여러 파일 중복을 피하기 위해 템플릿을 사용 합니다.
- Helm 차트 간의 종속성을 관리할 수 있습니다.
- 차트, Azure Container Registry와 같은 Helm 리포지토리에 저장 하 고 빌드 파이프라인으로 통합 될 수 있습니다.

Container Registry를 Helm 리포지토리로 사용하는 방법에 대한 자세한 내용은 [애플리케이션 차트용 Helm 리포지토리로 Azure Container Registry 사용](/azure/container-registry/container-registry-helm-repos)을 참조하세요.

단일 마이크로 서비스는 여러 Kubernetes 구성 파일을 포함할 수 있습니다. 서비스 업데이트 터치 선택기, 레이블 및 이미지 태그를 업데이트 하려면 이러한 파일의 모든 것을 의미할 수 있습니다. Helm 차트 라고 하는 단일 패키지도 처리 하 고 쉽게 변수를 사용 하 여 YAML 파일을 업데이트할 수 있습니다. Helm (템플릿을 기반으로 이동) 하는 템플릿 언어를 사용 하 여 매개 변수가 있는 YAML 구성 파일을 작성할 수 있습니다.

예를 들어 다음과 같습니다. 배포를 정의 하는 YAML 파일의 일부

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

&#11162;참조 된 [소스 파일](https://github.com/mspnp/microservices-reference-implementation/blob/master/charts/package/templates/package-deploy.yaml)합니다.

배포 이름, 레이블 및 컨테이너에 사용 하 여 템플릿 매개 변수를 모두 배포 시에 제공 되는 사양 있는지 확인할 수 있습니다. 예를 들어 다음 명령줄에서

```bash
helm install $HELM_CHARTS/package/ \
     --set image.tag=0.1.0 \
     --set image.repository=package \
     --set dockerregistry=$ACR_SERVER \
     --namespace backend \
     --name package-v0.1.0
```

CI/CD 파이프라인에는 Kubernetes에 직접 차트를 설치할 수 없습니다, 있지만 차트 보관 파일 (.tgz 파일)을 만들고 차트를 Helm 리포지토리에 Azure Container Registry와 같은 푸시하는 것이 좋습니다. 자세한 내용은 [Helm 차트 Azure 파이프라인에서의 패키지 Docker 기반 응용 프로그램](/azure/devops/pipelines/languages/helm?view=azure-devops)합니다.

Helm 자체 네임 스페이스를 배포 및 역할 기반 액세스 제어 (RBAC)를 사용 하 여 배포할 수 있습니다 하는 네임 스페이스를 제한 하는 것이 좋습니다. 자세한 내용은 [역할 기반 Access Control](https://helm.sh/docs/using_helm/#helm-and-role-based-access-control) Helm 설명서에서.

### <a name="revisions"></a>수정

Helm 차트에는 사용 해야 하는 버전 번호를 항상 [유의 적 버전](https://semver.org/)합니다. 차트 수도 있습니다는 `appVersion`합니다. 이 필드는 선택 사항이 며 차트 버전 관련 될 필요가 없습니다. 일부 팀 할 응용 프로그램 버전 개별적으로 차트에 대 한 업데이트에서. 하지만 차트 버전 및 응용 프로그램 버전 간에 1:1 관계 이므로 한 버전 번호를 사용 하는 간단 합니다. 이런 방식으로 릴리스 당 하나의 차트를 저장 한 원하는 릴리스를 쉽게 배포할 수 있습니다.

```bash
helm install <package-chart-name> --version <desiredVersion>
```

배포 템플릿에 변경 원인의 주석을 제공 하려면 것도 좋은 방법:

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

각 수정 버전의 변경 원인을 필드를 볼 수 있습니다 사용 하는 `kubectl rollout history` 명령입니다. 이전 예에서의 변경 원인은 Helm 차트 매개 변수로 제공 됩니다.

```bash
$ kubectl rollout history deployments/delivery-v010 -n backend
deployment.extensions/delivery-v010
REVISION  CHANGE-CAUSE
1         Initial deployment
```

사용할 수도 있습니다는 `helm list` 수정 기록을 보려면 명령을:

```bash
~$ helm list
NAME            REVISION    UPDATED                     STATUS        CHART            APP VERSION     NAMESPACE
delivery-v0.1.0 1           Sun Apr  7 00:25:30 2019    DEPLOYED      delivery-v0.1.0  v0.1.0          backend
```

> [!TIP]
> 사용 된 `--history-max` Helm을 초기화할 때 플래그입니다. 이 설정은 수정 Tiller 기록에 저장 하는 횟수를 제한 합니다. Tiller configmaps 수정 기록을 저장합니다. 자주 업데이트를 해제 하는 경우 기록 크기를 제한 하지 않는 이상는 configmaps 매우 큰 증가할 수 있습니다.

## <a name="azure-devops-pipeline"></a>Azure DevOps 파이프라인

Azure 파이프라인에서 파이프라인으로 나뉩니다 *파이프라인을 빌드합니다* 하 고 *릴리스 파이프라인*합니다. 빌드 파이프라인 CI 프로세스를 실행 하 고 빌드 아티팩트를 만듭니다. Kubernetes에서 마이크로 서비스 아키텍처에 대 한 이러한 아티팩트는 컨테이너 이미지와 각 마이크로 서비스를 정의 하는 Helm 차트입니다. 릴리스 파이프라인을 클러스터에 마이크로 서비스를 배포 하는 CD 프로세스를 실행 합니다.

이 문서의 앞부분에서 설명 하는 CI 흐름에 따라 빌드 파이프라인의 다음 태스크를 구성 될 수 있습니다.

1. 테스트 러너 컨테이너를 빌드하십시오.

    ```yaml
    - task: Docker@1
      inputs:
        azureSubscriptionEndpoint: $(AzureSubscription)
        azureContainerRegistry: $(AzureContainerRegistry)
        arguments: '--pull --target testrunner'
        dockerFile: $(System.DefaultWorkingDirectory)/$(dockerFileName)
        imageName: '$(imageName)-test'
    ```

1. 테스트 러너 컨테이너에 대해 실행 되는 docker를 호출 하 여 테스트를 실행 합니다.

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

1. 테스트 결과 게시 합니다. 참조 [빌드를 통합 하 고 작업을 테스트](/azure/devops/pipelines/languages/docker?view=azure-devops&tabs=yaml#integrate-build-and-test-tasks)합니다.

    ```yaml
    - task: PublishTestResults@2
      inputs:
        testResultsFormat: 'VSTest'
        testResultsFiles: 'TestResults/*.trx'
        searchFolder: '$(System.DefaultWorkingDirectory)'
        publishRunAttachments: true
    ```

1. 런타임 컨테이너를 빌드하십시오.

    ```yaml
    - task: Docker@1
      inputs:
        azureSubscriptionEndpoint: $(AzureSubscription)
        azureContainerRegistry: $(AzureContainerRegistry)
        dockerFile: $(System.DefaultWorkingDirectory)/$(dockerFileName)
        includeLatestTag: false
        imageName: '$(imageName)'
    ```

1. Azure Container Registry (또는 다른 컨테이너 레지스트리)에 컨테이너를 푸시하십시오.

    ```yaml
    - task: Docker@1
      inputs:
        azureSubscriptionEndpoint: $(AzureSubscription)
        azureContainerRegistry: $(AzureContainerRegistry)
        command: 'Push an image'
        imageName: '$(imageName)'
        includeSourceTags: false
    ```

1. Helm 차트를 패키지 합니다.

    ```yaml
    - task: HelmDeploy@0
      inputs:
        command: package
        chartPath: $(chartPath)
        chartVersion: $(Build.SourceBranchName)
        arguments: '--app-version $(Build.SourceBranchName)'
    ```

1. Azure Container Registry (또는 기타 Helm 리포지토리에) Helm 패키지를 푸시하십시오.

    ```yaml
    task: AzureCLI@1
      inputs:
        azureSubscription: $(AzureSubscription)
        scriptLocation: inlineScript
        inlineScript: |
        az acr helm push $(System.ArtifactsDirectory)/$(repositoryName)-$(Build.SourceBranchName).tgz --name $(AzureContainerRegistry);
    ```

&#11162;참조 된 [소스 파일](https://github.com/mspnp/microservices-reference-implementation/blob/master/src/shipping/delivery/azure-pipelines-ci.yml)합니다.

CI 파이프라인에서 출력에는 프로덕션이 준비 된 컨테이너 이미지 및 마이크로 서비스에 대 한 업데이트 된 Helm 차트 됩니다. 이 시점에서 릴리스 파이프라인을 통해 걸릴 수 있습니다. 다음 단계를 수행합니다.

- 개발/QA/스테이징 환경에 배포 합니다.
- 승인자가 승인 하거나 거부할 배포 될 때까지 기다립니다.
- 릴리스에 대 한 컨테이너 이미지 태그를 다시 지정
- 릴리스 태그를 컨테이너 레지스트리에 푸시하십시오.
- 프로덕션 클러스터에 Helm 차트를 업그레이드 합니다.

릴리스 파이프라인을 만드는 방법에 대 한 자세한 내용은 참조 하세요. [릴리스 파이프라인 초안 릴리스 및 릴리스 옵션](/azure/devops/pipelines/release/?view=azure-devops)합니다.

다음 다이어그램은이 문서에 설명 된 종단 간 CI/CD 프로세스를 보여줍니다.

![CD/CD 파이프라인](./images/aks-cicd-flow.png)

## <a name="next-steps"></a>다음 단계

이 문서에서 찾을 수 있는 참조 구현을 살펴보았습니다 [GitHub][ri]합니다.

[ri]: https://github.com/mspnp/microservices-reference-implementation