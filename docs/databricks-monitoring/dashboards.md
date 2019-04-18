---
title: 대시보드를 사용하여 Azure Databricks 메트릭 시각화
description: Azure Databricks에서 성능을 모니터링 하려면 Grafana 대시보드를 배포 하는 방법
author: petertaylor9999
ms.date: 03/26/2019
ms.openlocfilehash: a84203a9188848e6363a80ac455332e8f6a73cda
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59640314"
---
# <a name="use-dashboards-to-visualize-azure-databricks-metrics"></a>대시보드를 사용하여 Azure Databricks 메트릭 시각화

이 문서에서는 성능 문제에 대 한 Azure Databricks 작업을 모니터링 하려면 Grafana 대시보드를 설정 하는 방법을 보여 줍니다.

[Azure Databricks](/azure/azure-databricks/) 는 빠르고 강력 하 고 공동 작업 [Apache Spark](https://spark.apache.org/)– 쉽게 빠르게 개발 및 빅 데이터 분석 및 AI (인공 지능) 솔루션을 배포 하는 분석 서비스를 기반으로 합니다. 모니터링은 프로덕션 환경에서 Azure Databricks 워크 로드 운영의 중요 한 구성 요소입니다. 분석을 위해 작업 영역으로 메트릭을 수집 하려면 첫 번째 단계가입니다. Azure에서 로그 데이터를 관리 하기 위한 최상의 솔루션은 [Azure Monitor](/azure/azure-monitor/)합니다. Azure Databricks에서 Azure monitor에 보내는 로그 데이터를 고유 하 게 지원 하지 않습니다 하지만 [이 기능에 대 한 라이브러리](https://github.com/mspnp/spark-monitoring) 에서 사용할 수 있습니다 [Github](https://github.com)합니다.

이 라이브러리는 스트리밍 이벤트 메트릭을 쿼리 하는 Apache Spark 구조 뿐만 아니라 Azure Databricks 서비스 메트릭 로깅을 사용 하도록 설정 합니다. Azure Databricks 클러스터에이 라이브러리를 성공적으로 배포한 후 집합이 더 이상 배포할 수 있습니다 [Grafana](https://granfana.com) 프로덕션 환경의 일부로 배포할 수 있는 대시보드.

![대시보드의 스크린샷](./_images/dashboard-screenshot.png)

## <a name="prerequisites"></a>필수 조건

복제는 [Github 리포지토리](https://github.com/mspnp/spark-monitoring) 및 [배포 지침에 따라](./configure-cluster.md) 을 빌드하고 Azure Log Analytics 작업 영역에 로그를 보내도록 Azure Databricks 라이브러리에 대 한 Azure Monitor 로깅을 구성 합니다.

## <a name="deploy-the-azure-log-analytics-workspace"></a>Azure Log Analytics 작업 영역 배포

Azure Log Analytics 작업 영역을 배포 하려면 다음이 단계를 수행 합니다.

1. 이동 된 `/perftools/deployment/loganalytics` 디렉터리입니다.
1. 배포 된 **logAnalyticsDeploy.json** Azure Resource Manager 템플릿. Resource Manager 템플릿을 배포 하는 방법에 대 한 자세한 내용은 참조 하세요. [Resource Manager 템플릿과 Azure CLI로 리소스 배포][rm-cli]합니다. 서식 파일에 다음 매개 변수:

    * **location**: Log Analytics 작업 영역 및 대시보드는 배포 된 지역입니다.
    * **serviceTier**: 지정한 작업 영역 가격 책정 계층입니다. 참조 [여기] [ sku] 유효한 값의 목록은 합니다.
    * **dataRetention** (선택 사항): 데이터를 기록 하는 일 수는 Log Analytics 작업 영역에서 유지 됩니다. 기본값은 30일입니다. 가격 책정 계층 이면 `Free`, 데이터 보존 7 일 이내 여야 합니다.
    * **WorkspaceName** (선택 사항): 작업 영역에 대 한 이름입니다. 지정 하지 않으면 템플릿의 이름을 생성 합니다.

    ```bash
    az group deployment create --resource-group <resource-group-name> --template-file logAnalyticsDeploy.json --parameters location='East US' serviceTier='Standalone'
    ```

이 작업 영역을 만드는 템플릿과 대시보드에서 사용 되는 미리 정의 된 쿼리 집합을 만듭니다.

## <a name="deploy-grafana-in-a-virtual-machine"></a>가상 컴퓨터에서 Grafana를 배포 합니다.

Grafana는 오픈 소스 프로젝트 Grafana 플러그 인을 사용 하 여 Azure Monitor에 대 한 Azure Log Analytics 작업 영역에 저장 된 시간 시계열 메트릭 시각화를 배포할 수 있습니다. Grafana 가상 머신 (VM)에서 실행 하 고 저장소 계정, 가상 네트워크 및 기타 리소스를 필요 합니다. Bitnami 사용 하 여 가상 컴퓨터를 배포 하려면 Grafana 이미지 및 관련된 리소스를 인증이 단계를 수행 하십시오.

1. Azure CLI를 사용 하 여 Grafana에 대 한 Azure Marketplace 이미지 사용 조건에 동의 합니다.

    ```bash
    az vm image accept-terms --publisher bitnami --offer grafana --plan default
    ```

1. 이동 된 `/spark-monitoring/perftools/deployment/grafana` GitHub 리포지토리의 로컬 복사본에 있는 디렉터리입니다.
1. 배포 된 **grafanaDeploy.json** 다음과 같이 Resource Manager 템플릿:

    ```bash
    export DATA_SOURCE="https://raw.githubusercontent.com/mspnp/spark-monitoring/master/perftools/deployment/grafana/AzureDataSource.sh"
    az group deployment create \
        --resource-group <resource-group-name> \
        --template-file grafanaDeploy.json \
        --parameters adminPass='<vm password>' dataSource=$DATA_SOURCE
    ```

배포가 완료 되 면 Grafana의 bitnami 이미지는 가상 컴퓨터에 설치 됩니다.

## <a name="update-the-grafana-password"></a>Grafana 암호 업데이트

설치 프로세스의 일부로, Grafana 설치 스크립트를 출력에 대 한 임시 암호를 **관리자** 사용자입니다. 이 임시 암호에 로그인 해야 합니다. 임시 암호를 가져오려면 다음이 단계를 수행 합니다.  

1. Azure 포털에 로그인합니다.  
1. 리소스를 배포한 리소스 그룹을 선택 합니다.
1. Grafana가 설치 되는 VM을 선택 합니다. 기본 매개 변수 이름을 배포 템플릿에서 사용 하는 경우 VM 이름을 앞 **sparkmonitoring-vm-grafana**합니다.
1. 에 **지원 + 문제 해결** 섹션에서 클릭 **부트 진단** 부팅 진단 페이지를 엽니다.
1. 클릭 **직렬 로그** 부팅 진단 페이지입니다.
1. 다음 문자열을 검색 합니다. "Bitnami 응용 프로그램 암호 설정".
1. 암호를 안전한 위치에 복사 합니다.

다음으로, 다음이 단계를 수행 하 여 Grafana 관리자 암호를 변경:

1. Azure portal에서 VM을 선택 하 고 클릭 **개요**합니다.
1. 공용 IP 주소를 복사합니다.
1. 웹 브라우저를 열고 다음 URL로 이동: `http://<IP address>:3000`합니다.
1. Grafana 로그인 화면에서 입력 **관리자** 사용자 이름 및 이전 단계에서 Grafana 암호 사용에 대 한 합니다.
1. 로그인 선택 **구성** (기어 아이콘).
1. 선택 **서버 관리자**합니다.
1. 에 **사용자** 탭을 선택 합니다 **관리자** 로그인 합니다.
1. 암호를 업데이트 합니다.

## <a name="create-an-azure-monitor-data-source"></a>Azure Monitor 데이터 원본 만들기

1. Grafana Log Analytics 작업 영역에 대 한 액세스를 관리할 수 있도록 주 서비스를 만듭니다. 자세한 내용은 참조 하세요. [Azure CLI를 사용 하 여 Azure 서비스 주체 만들기](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest)

    ```bash
    az ad sp create-for-rbac --name http://<service principal name> --role "Log Analytics Reader"
    ```

1. AppId, 암호 및이 명령의 출력에서 테 넌 트에 대 한 값을 note:

    ```json
    {
        "appId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "displayName": "azure-cli-2019-03-27-00-33-39",
        "name": "http://<service principal name>",
        "password": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "tenant": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    }
    ```

1. 로그인 Grafana 설명 된 대로 이전 합니다. 선택 **Configuration** (기어 아이콘) 차례로 **데이터 원본**합니다.
1. 에 **데이터 원본** 탭을 클릭 **데이터 원본 추가**합니다.
1. 선택 **Azure Monitor** 데이터 원본 유형으로 합니다.
1. 에 **설정** 섹션에 데이터 소스에 대 한 이름을 입력 합니다 **이름** 텍스트 상자에 붙여넣습니다.
1. 에 **Azure 모니터링에 대 한 API 세부 정보** 섹션에서 다음 정보를 입력 합니다.

    * 등록 ID: Azure 구독 ID입니다.
    * 테 넌 트 Id: 앞에서 테 넌 트 ID입니다.
    * 클라이언트 Id: 앞에서 "appId"의 값입니다.
    * 클라이언트 암호: 앞에서 "password"의 값입니다.

1. 에 **Azure Log Analytics API 세부 정보** 섹션을 **Azure Monitor API에 따라 동일한 세부 정보** 확인란을 선택 합니다.
1. 클릭 **저장 및 테스트**합니다. Log Analytics 데이터 원본을 올바르게 구성 하는 경우에 성공 메시지가 표시 됩니다.

## <a name="create-the-dashboard"></a>대시보드 만들기

다음 단계를 수행 하 여 Grafana에서 대시보드를 만듭니다.

1. 이동 된 `/perftools/dashboards/grafana` GitHub 리포지토리의 로컬 복사본에 있는 디렉터리입니다.
1. 다음 스크립트를 실행합니다.

    ```bash
    export WORKSPACE=<your Azure Log Analytics workspace ID>
    export LOGTYPE=SparkListenerEvent_CL

    sh DashGen.sh
    ```

    스크립트의 출력은 이라는 파일로 **SparkMonitoringDash.json**합니다.

1. Grafana 대시보드로 돌아가 **만들기** (더하기 아이콘).
1. **가져오기**를 선택합니다.
1. 클릭 **.json 파일을 업로드**합니다.
1. 선택 된 **SparkMonitoringDash.json** 2 단계에서 만든 파일입니다.
1. 에 **옵션** 섹션의 **ALA**, 이전에 만든 Azure Monitor 데이터 원본을 선택 합니다.
1. **가져오기**를 클릭합니다.

## <a name="visualizations-in-the-dashboards"></a>대시보드에서 시각화

Azure Log Analytics 및 Grafana 대시보드에 시계열 시각화 집합이 포함 됩니다. 각 그래프는 Apache Spark와 관련 된 메트릭 데이터의 시계열 도표 [작업](https://spark.apache.org/docs/latest/job-scheduling.html), 작업 및 각 단계를 구성 하는 작업의 단계입니다.

시각화는 다음과 같습니다.

### <a name="job-latency"></a>작업 대기 시간

이 시각화는 작업의 전체 성능에 대략적인 보기 작업에 대 한 실행 대기 시간을 표시 합니다. 시작부터 완료 될 때까지 작업 실행 기간을 표시합니다. 작업 시작 시간을 참고 작업 제출 시간이 빠른 응모 같지는 않습니다. 대기 시간 백분위 수 (10%, 30%, 50%, 90%)로 표시 됩니다. 클러스터 ID 및 응용 프로그램 ID로 인덱싱된 작업 실행

### <a name="stage-latency"></a>스테이지 대기 시간

시각화 클러스터당, 응용 프로그램 및 개별 단계 당 각 단계의 대기 시간을 보여 줍니다. 이 시각화는 느리게 실행 되는 특정 단계를 식별 하는 데 유용 합니다.

### <a name="task-latency"></a>작업 대기 시간

이 시각화 작업 실행 대기 시간을 표시 합니다. 대기 시간 백분위 클러스터, 스테이지 이름 및 응용 프로그램 당 작업 실행으로 표시 됩니다.

### <a name="sum-task-execution-per-host"></a>호스트당 합계 작업 실행

이 시각화는 호스트 클러스터에서 실행 당 작업 실행 대기의 합계를 표시 합니다. 호스트당 작업 실행 대기 시간 보기 훨씬 더 높은 전체 작업 보다 대기 시간이 다른 호스트가 있는 호스트를 식별 합니다. 이 작업이 비효율적으로 또는 불균등에 게 배포 된 호스트를 의미할 수 있습니다.

### <a name="task-metrics"></a>작업 메트릭

이 시각화에는 지정된 된 태스크의 실행에 대 한 실행 메트릭 집합을 보여 줍니다. 이러한 메트릭에 크기 및 데이터 셔플 기간, serialization 및 deserialization 작업 기간 및 다른 사용자가 포함 됩니다. 전체 메트릭 집합에 대 한 패널에 대 한 Log Analytics 쿼리를 봅니다. 이 시각화는 작업과 각 작업의 식별 리소스 소비를 구성 하는 작업을 이해 하는 데 유용 합니다. 그래프에 스파이크가 조사 해야 하는 비용이 많이 드는 작업을 나타냅니다.

### <a name="cluster-throughput"></a>클러스터 처리량

이 시각화 클러스터 및 응용 프로그램 작업 한 클러스터 및 응용 프로그램의 크기를 표시 하 여 인덱싱된 작업 항목의 높은 수준의 뷰입니다. 작업, 작업 및 클러스터, 응용 프로그램 및 1 분 단위로 단계를 완료 하는 단계 수를 보여 줍니다. 

### <a name="streaming-throughputlatency"></a>스트리밍 처리량/대기 시간

이 시각화는 구조적된 스트리밍 쿼리를 사용 하 여 관련 메트릭을 관련이 있습니다. 그래프는 초당 입력된 행의 수 및 처리 된 초당 행 수를 보여 줍니다. 스트리밍 메트릭은 응용 프로그램당도 표시 됩니다. 구조적된 스트리밍 쿼리를 처리할와 시각화 나타냅니다 OnQueryProgress 이벤트가 생성 될 때 이러한 메트릭을 전송 됩니다 (밀리초) 쿼리 일괄 처리를 실행 하는 데 걸린 기간 대기 시간이 스트리밍.

### <a name="resource-consumption-per-executor"></a>실행 기 당 리소스 사용량

다음은 대시보드 표시는 특정 종류의 리소스 및 각 클러스터에서 실행 기 당 사용 되는 방법에 대 한 시각화의 집합입니다. 이러한 시각화는 이상 값 실행 기 당 리소스 소비를 식별할 수 있습니다. 예를 들어, 특정 실행자에 대 한 작업 할당 왜곡 되 리소스 소비를 클러스터에서 실행 중인 다른 실행자를 기준으로 상승 되는 합니다. 이 실행 기에 대 한 리소스 사용량 급증 하 여 식별할 수 있습니다.

### <a name="executor-compute-time-metrics"></a>실행 기 계산 시간 메트릭

다음은 실행 기의 비율이 직렬화 시간 표시 시간, CPU 시간 및 Java 가상 머신 시간 전체 실행자 계산 시간으로 역직렬화 하는 대시보드에 대 한 시각화의 집합. 이 처리 하는 전체 실행 기에 영향을 주는 시각적으로 얼마나 많은 각 이러한 4 메트릭에 보여 줍니다.

### <a name="shuffle-metrics"></a>순서 섞기 메트릭

최종 집합 데이터 shuffle 구조적된 스트리밍 쿼리를 사용 하 여 모든 실행 기에서 관련 된 메트릭을 시각화 표시입니다. 여기에 읽기, 쓴 바이트 셔플, 파일 시스템 사용 되는 쿼리에서 메모리 및 디스크 사용량의 순서 섞기 바이트 셔플 포함 됩니다.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [성능 병목 현상 문제 해결](./performance-troubleshooting.md)

<!-- links -->

[rm-cli]: /azure/azure-resource-manager/resource-group-template-deploy-cli
[sku]: /azure/templates/Microsoft.OperationalInsights/2015-11-01-preview/workspaces#sku-object