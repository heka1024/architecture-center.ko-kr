---
title: 대시보드를 사용 하 여 Azure Databricks 메트릭을 시각화합니다
description: Azure Databricks에서 성능을 모니터링 하려면 Grafana 대시보드를 배포 하는 방법
author: petertaylor9999
ms.date: 03/26/2019
ms.openlocfilehash: 36fcd93f6ca757e8e750d0fcbbdf0311c08560b0
ms.sourcegitcommit: 1a3cc91530d56731029ea091db1f15d41ac056af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58887831"
---
# <a name="use-dashboards-to-visualize-azure-databricks-metrics"></a><span data-ttu-id="c81e9-103">대시보드를 사용 하 여 Azure Databricks 메트릭을 시각화합니다</span><span class="sxs-lookup"><span data-stu-id="c81e9-103">Use dashboards to visualize Azure Databricks metrics</span></span>

<span data-ttu-id="c81e9-104">이 문서에서는 성능 문제에 대 한 Azure Databricks 작업을 모니터링 하려면 Grafana 대시보드를 설정 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-104">This article shows how to set up a Grafana dashboard to monitor Azure Databricks jobs for performance issues.</span></span>

<span data-ttu-id="c81e9-105">[Azure Databricks](/azure/azure-databricks/) 는 빠르고 강력 하 고 공동 작업 [Apache Spark](https://spark.apache.org/)– 쉽게 빠르게 개발 및 빅 데이터 분석 및 AI (인공 지능) 솔루션을 배포 하는 분석 서비스를 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-105">[Azure Databricks](/azure/azure-databricks/) is a fast, powerful, and collaborative [Apache Spark](https://spark.apache.org/)–based analytics service that makes it easy to rapidly develop and deploy big data analytics and artificial intelligence (AI) solutions.</span></span> <span data-ttu-id="c81e9-106">모니터링은 프로덕션 환경에서 Azure Databricks 워크 로드 운영의 중요 한 구성 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-106">Monitoring is a critical component of operating Azure Databricks workloads in production.</span></span> <span data-ttu-id="c81e9-107">분석을 위해 작업 영역으로 메트릭을 수집 하려면 첫 번째 단계가입니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-107">The first step is to gather metrics into a workspace for analysis.</span></span> <span data-ttu-id="c81e9-108">Azure에서 로그 데이터를 관리 하기 위한 최상의 솔루션은 [Azure Monitor](/azure/azure-monitor/)합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-108">In Azure, the best solution for managing log data is [Azure Monitor](/azure/azure-monitor/).</span></span> <span data-ttu-id="c81e9-109">Azure Databricks에서 Azure monitor에 보내는 로그 데이터를 고유 하 게 지원 하지 않습니다 하지만 [이 기능에 대 한 라이브러리](https://github.com/mspnp/spark-monitoring) 에서 사용할 수 있습니다 [Github](https://github.com)합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-109">Azure Databricks does not natively support sending log data to Azure monitor, but a [library for this functionality](https://github.com/mspnp/spark-monitoring) is available in [Github](https://github.com).</span></span>

<span data-ttu-id="c81e9-110">이 라이브러리는 스트리밍 이벤트 메트릭을 쿼리 하는 Apache Spark 구조 뿐만 아니라 Azure Databricks 서비스 메트릭 로깅을 사용 하도록 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-110">This library enables logging of Azure Databricks service metrics as well as Apache Spark structure streaming query event metrics.</span></span> <span data-ttu-id="c81e9-111">Azure Databricks 클러스터에이 라이브러리를 성공적으로 배포한 후 집합이 더 이상 배포할 수 있습니다 [Grafana](https://granfana.com) 프로덕션 환경의 일부로 배포할 수 있는 대시보드.</span><span class="sxs-lookup"><span data-stu-id="c81e9-111">Once you've successfully deployed this library to an Azure Databricks cluster, you can further deploy a set of [Grafana](https://granfana.com) dashboards that you can deploy as part of your production environment.</span></span>

![대시보드의 스크린샷](./_images/dashboard-screenshot.png)

## <a name="prequisites"></a><span data-ttu-id="c81e9-113">필수 조건</span><span class="sxs-lookup"><span data-stu-id="c81e9-113">Prequisites</span></span>

<span data-ttu-id="c81e9-114">복제는 [Github 리포지토리](https://github.com/mspnp/spark-monitoring) 및 [배포 지침에 따라](./configure-cluster.md) 을 빌드하고 Azure Log Analytics 작업 영역에 로그를 보내도록 Azure Databricks 라이브러리에 대 한 Azure Monitor 로깅을 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-114">Clone the [Github repository](https://github.com/mspnp/spark-monitoring) and [follow the deployment instructions](./configure-cluster.md) to build and configure the Azure Monitor logging for Azure Databricks library to send logs to your Azure Log Analytics workspace.</span></span>

## <a name="deploy-the-azure-log-analytics-workspace"></a><span data-ttu-id="c81e9-115">Azure Log Analytics 작업 영역 배포</span><span class="sxs-lookup"><span data-stu-id="c81e9-115">Deploy the Azure Log Analytics workspace</span></span>

<span data-ttu-id="c81e9-116">Azure Log Analytics 작업 영역을 배포 하려면 다음이 단계를 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-116">To deploy the Azure Log Analytics workspace, follow these steps:</span></span>

1. <span data-ttu-id="c81e9-117">이동 된 `/perftools/deployment/loganalytics` 디렉터리입니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-117">Navigate to the `/perftools/deployment/loganalytics` directory.</span></span>
1. <span data-ttu-id="c81e9-118">배포 된 **logAnalyticsDeploy.json** Azure Resource Manager 템플릿.</span><span class="sxs-lookup"><span data-stu-id="c81e9-118">Deploy the **logAnalyticsDeploy.json** Azure Resource Manager template.</span></span> <span data-ttu-id="c81e9-119">Resource Manager 템플릿을 배포 하는 방법에 대 한 자세한 내용은 참조 하세요. [Resource Manager 템플릿과 Azure CLI로 리소스 배포][rm-cli]합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-119">For more information about deploying Resource Manager templates, see [Deploy resources with Resource Manager templates and Azure CLI][rm-cli].</span></span> <span data-ttu-id="c81e9-120">서식 파일에 다음 매개 변수:</span><span class="sxs-lookup"><span data-stu-id="c81e9-120">The template has the following parameters:</span></span>

    * <span data-ttu-id="c81e9-121">**location**: Log Analytics 작업 영역 및 대시보드는 배포 된 지역입니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-121">**location**: The region where the Log Analytics workspace and dashboards are deployed.</span></span>
    * <span data-ttu-id="c81e9-122">**serviceTier**: 지정한 작업 영역 가격 책정 계층입니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-122">**serviceTier**: Rhe workspace pricing tier.</span></span> <span data-ttu-id="c81e9-123">참조 [여기] [ sku] 유효한 값의 목록은 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-123">See [here][sku] for a list of valid values.</span></span>
    * <span data-ttu-id="c81e9-124">**dataRetention** (선택 사항): 데이터를 기록 하는 일 수는 Log Analytics 작업 영역에서 유지 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-124">**dataRetention** (optional): The number of days log data is retained in the Log Analytics workspace.</span></span> <span data-ttu-id="c81e9-125">기본값은 30일입니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-125">The default value is 30 days.</span></span> <span data-ttu-id="c81e9-126">가격 책정 계층 이면 `Free`, 데이터 보존 7 일 이내 여야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-126">If the pricing tier is `Free`, the data retention must be 7 days.</span></span>
    * <span data-ttu-id="c81e9-127">**WorkspaceName** (선택 사항): 작업 영역에 대 한 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-127">**workspaceName** (optional): A name for the workspace.</span></span> <span data-ttu-id="c81e9-128">지정 하지 않으면 템플릿의 이름을 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-128">If not specified, the template generates a name.</span></span>

    ```bash
    az group deployment create --resource-group <resource-group-name> --template-file logAnalyticsDeploy.json --parameters location='East US' serviceTier='Standalone'
    ```

<span data-ttu-id="c81e9-129">이 작업 영역을 만드는 템플릿과 대시보드에서 사용 되는 미리 정의 된 쿼리 집합을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-129">This template creates the workspace and also creates a set of predefined queries that are used by by dashboard.</span></span>

## <a name="deploy-grafana-in-a-virtual-machine"></a><span data-ttu-id="c81e9-130">가상 컴퓨터에서 Grafana를 배포 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-130">Deploy Grafana in a virtual machine</span></span>

<span data-ttu-id="c81e9-131">Grafana는 오픈 소스 프로젝트 Grafana 플러그 인을 사용 하 여 Azure Monitor에 대 한 Azure Log Analytics 작업 영역에 저장 된 시간 시계열 메트릭 시각화를 배포할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-131">Grafana is an open source project you can deploy to visualize the time series metrics stored in your Azure Log Analytics workspace using the Grafana plugin for Azure Monitor.</span></span> <span data-ttu-id="c81e9-132">Grafana 가상 머신 (VM)에서 실행 하 고 저장소 계정, 가상 네트워크 및 기타 리소스를 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-132">Grafana executes on a virtual machine (VM) and requires a storage account, virtual network, and other resources.</span></span> <span data-ttu-id="c81e9-133">Bitnami 사용 하 여 가상 컴퓨터를 배포 하려면 Grafana 이미지 및 관련된 리소스를 인증이 단계를 수행 하십시오.</span><span class="sxs-lookup"><span data-stu-id="c81e9-133">To deploy a virtual machine with the bitnami certified Grafana image and associated resources, follow these steps:</span></span>

1. <span data-ttu-id="c81e9-134">Azure CLI를 사용 하 여 Grafana에 대 한 Azure Marketplace 이미지 사용 조건에 동의 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-134">Use the Azure CLI to accept the Azure Marketplace image terms for Grafana.</span></span>

    ```bash
    az vm image accept-terms --publisher bitnami --offer grafana --plan default
    ```

1. <span data-ttu-id="c81e9-135">이동 된 `/spark-monitoring/perftools/deployment/grafana` GitHub 리포지토리의 로컬 복사본에 있는 디렉터리입니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-135">Navigate to the `/spark-monitoring/perftools/deployment/grafana` directory in your local copy of the GitHub repo.</span></span>
1. <span data-ttu-id="c81e9-136">배포 된 **grafanaDeploy.json** 다음과 같이 Resource Manager 템플릿:</span><span class="sxs-lookup"><span data-stu-id="c81e9-136">Deploy the **grafanaDeploy.json** Resource Manager template as follows:</span></span>

    ```bash
    export DATA_SOURCE="https://raw.githubusercontent.com/mspnp/spark-monitoring/master/perftools/deployment/grafana/AzureDataSource.sh"
    az group deployment create \
        --resource-group <resource-group-name> \
        --template-file grafanaDeploy.json \
        --parameters adminPass='<vm password>' dataSource=$DATA_SOURCE
    ```

<span data-ttu-id="c81e9-137">배포가 완료 되 면 Grafana의 bitnami 이미지는 가상 컴퓨터에 설치 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-137">Once the deployment is complete, the bitnami image of Grafana is installed on the virtual machine.</span></span>

## <a name="update-the-grafana-password"></a><span data-ttu-id="c81e9-138">Grafana 암호 업데이트</span><span class="sxs-lookup"><span data-stu-id="c81e9-138">Update the Grafana password</span></span>

<span data-ttu-id="c81e9-139">설치 프로세스의 일부로, Grafana 설치 스크립트를 출력에 대 한 임시 암호를 **관리자** 사용자입니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-139">As part of the setup process, the Grafana installation script outputs a temporary password for the **admin** user.</span></span> <span data-ttu-id="c81e9-140">이 임시 암호에 로그인 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-140">You need this temporary password to sign in.</span></span> <span data-ttu-id="c81e9-141">임시 암호를 가져오려면 다음이 단계를 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-141">To obtain the temporary password, follow these steps:</span></span>  

1. <span data-ttu-id="c81e9-142">Azure 포털에 로그인합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-142">Log in to the Azure portal.</span></span>  
1. <span data-ttu-id="c81e9-143">리소스를 배포한 리소스 그룹을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-143">Select the resource group where the resources were deployed.</span></span>
1. <span data-ttu-id="c81e9-144">Grafana가 설치 되는 VM을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-144">Select the VM where Grafana was installed.</span></span> <span data-ttu-id="c81e9-145">기본 매개 변수 이름을 배포 템플릿에서 사용 하는 경우 VM 이름을 앞 **sparkmonitoring-vm-grafana**합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-145">If you used the default parameter name in the deployment template, the VM name is prefaced with **sparkmonitoring-vm-grafana**.</span></span>
1. <span data-ttu-id="c81e9-146">에 **지원 + 문제 해결** 섹션에서 클릭 **부트 진단** 부팅 진단 페이지를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-146">In the **Support + troubleshooting** section, click **Boot diagnostics** to open the boot diagnostics page.</span></span>
1. <span data-ttu-id="c81e9-147">클릭 **직렬 로그** 부팅 진단 페이지입니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-147">Click **Serial log** on the boot diagnostics page.</span></span>
1. <span data-ttu-id="c81e9-148">다음 문자열을 검색 합니다. "Bitnami 응용 프로그램 암호 설정".</span><span class="sxs-lookup"><span data-stu-id="c81e9-148">Search for the following string: "Setting Bitnami application password to".</span></span>
1. <span data-ttu-id="c81e9-149">암호를 안전한 위치에 복사 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-149">Copy the password to a safe location.</span></span>

<span data-ttu-id="c81e9-150">다음으로, 다음이 단계를 수행 하 여 Grafana 관리자 암호를 변경:</span><span class="sxs-lookup"><span data-stu-id="c81e9-150">Next, change the Grafana administrator password by following these steps:</span></span>

1. <span data-ttu-id="c81e9-151">Azure portal에서 VM을 선택 하 고 클릭 **개요**합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-151">In the Azure portal, select the VM and click **Overview**.</span></span>
1. <span data-ttu-id="c81e9-152">공용 IP 주소를 복사합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-152">Copy the public IP address.</span></span>
1. <span data-ttu-id="c81e9-153">웹 브라우저를 열고 다음 URL로 이동: `http://<IP addresss>:3000`합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-153">Open a web browser and navigate to the following URL: `http://<IP addresss>:3000`.</span></span>
1. <span data-ttu-id="c81e9-154">Grafana 로그인 화면에서 입력 **관리자** 사용자 이름 및 이전 단계에서 Grafana 암호 사용에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-154">At the Grafana log in screen, enter **admin** for the user name, and use the Grafana password from the previous steps.</span></span>
1. <span data-ttu-id="c81e9-155">로그인 선택 **구성** (기어 아이콘).</span><span class="sxs-lookup"><span data-stu-id="c81e9-155">Once logged in, select **Configuration** (the gear icon).</span></span>
1. <span data-ttu-id="c81e9-156">선택 **서버 관리자**합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-156">Select **Server Admin**.</span></span>
1. <span data-ttu-id="c81e9-157">에 **사용자** 탭을 선택 합니다 **관리자** 로그인 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-157">On the **Users** tab, select the **admin** login.</span></span>
1. <span data-ttu-id="c81e9-158">암호를 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-158">Update the password.</span></span>

## <a name="create-an-azure-monitor-data-source"></a><span data-ttu-id="c81e9-159">Azure Monitor 데이터 원본 만들기</span><span class="sxs-lookup"><span data-stu-id="c81e9-159">Create an Azure Monitor data source</span></span>

1. <span data-ttu-id="c81e9-160">Grafana Log Analytics 작업 영역에 대 한 액세스를 관리할 수 있도록 주 서비스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-160">Create a service principal that allows Grafana to manage access to your Log Analytics workspace.</span></span> <span data-ttu-id="c81e9-161">자세한 내용은 참조 하세요. [Azure CLI를 사용 하 여 Azure 서비스 주체 만들기](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest)</span><span class="sxs-lookup"><span data-stu-id="c81e9-161">For more information, see [Create an Azure service principal with Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest)</span></span>

    ```bash
    az ad sp create-for-rbac --name http://<service principal name> --role "Log Analytics Reader"
    ```

1. <span data-ttu-id="c81e9-162">AppId, 암호 및이 명령의 출력에서 테 넌 트에 대 한 값을 note:</span><span class="sxs-lookup"><span data-stu-id="c81e9-162">Note the values for appId, password, and tenant in the output from this command:</span></span>

    ```json
    {
        "appId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "displayName": "azure-cli-2019-03-27-00-33-39",
        "name": "http://<service principal name>",
        "password": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "tenant": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    }
    ```

1. <span data-ttu-id="c81e9-163">로그인 Grafana 설명 된 대로 이전 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-163">Log into Grafana as described earlier.</span></span> <span data-ttu-id="c81e9-164">선택 **Configuration** (기어 아이콘) 차례로 **데이터 원본**합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-164">Select **Configuration** (the gear icon) and then **Data Sources**.</span></span>
1. <span data-ttu-id="c81e9-165">에 **데이터 원본** 탭을 클릭 **데이터 원본 추가**합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-165">In the **Data Sources** tab, click **Add data source**.</span></span>
1. <span data-ttu-id="c81e9-166">선택 **Azure Monitor** 데이터 원본 유형으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-166">Select **Azure Monitor** as the data source type.</span></span>
1. <span data-ttu-id="c81e9-167">에 **설정** 섹션에 데이터 소스에 대 한 이름을 입력 합니다 **이름** 텍스트 상자에 붙여넣습니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-167">In the **Settings** section, enter a name for the data source in the **Name** textbox.</span></span>
1. <span data-ttu-id="c81e9-168">에 **Azure 모니터링에 대 한 API 세부 정보** 섹션에서 다음 정보를 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-168">In the **Azure Monitor API Details** section, enter the following information:</span></span>

    * <span data-ttu-id="c81e9-169">등록 ID: Azure 구독 ID입니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-169">Subscription Id: Your Azure subscription ID.</span></span>
    * <span data-ttu-id="c81e9-170">테 넌 트 Id: 앞에서 테 넌 트 ID입니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-170">Tenant Id: The tenant ID from earlier.</span></span>
    * <span data-ttu-id="c81e9-171">클라이언트 Id: 앞에서 "appId"의 값입니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-171">Client Id: The value of "appId" from earlier.</span></span>
    * <span data-ttu-id="c81e9-172">클라이언트 암호: 앞에서 "password"의 값입니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-172">Client Secret: The value of "password" from earlier.</span></span>

1. <span data-ttu-id="c81e9-173">에 **Azure Log Analytics API 세부 정보** 섹션을 **Azure Monitor API에 따라 동일한 세부 정보** 확인란을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-173">In the **Azure Log Analytics API Details** section, check the **Same Details as Azure Monitor API** checkbox.</span></span>
1. <span data-ttu-id="c81e9-174">클릭 **저장 및 테스트**합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-174">Click **Save & Test**.</span></span> <span data-ttu-id="c81e9-175">Log Analytics 데이터 원본을 올바르게 구성 하는 경우에 성공 메시지가 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-175">If the Log Analytics data source is correctly configured, a success message is displayed.</span></span>

## <a name="create-the-dashboard"></a><span data-ttu-id="c81e9-176">대시보드 만들기</span><span class="sxs-lookup"><span data-stu-id="c81e9-176">Create the dashboard</span></span>

<span data-ttu-id="c81e9-177">다음 단계를 수행 하 여 Grafana에서 대시보드를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-177">Create the dashboards in Grafana by following these steps:</span></span>

1. <span data-ttu-id="c81e9-178">이동 된 `/perftools/dashboards/grafana` GitHub 리포지토리의 로컬 복사본에 있는 디렉터리입니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-178">Navigate to the `/perftools/dashboards/grafana` directory in your local copy of the GitHub repo.</span></span>
1. <span data-ttu-id="c81e9-179">다음 스크립트를 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-179">Run the following script:</span></span>

    ```bash
    export WORKSPACE=<your Azure Log Analytics workspace ID>
    export LOGTYPE=SparkListenerEvent_CL

    sh DashGen.sh
    ```

    <span data-ttu-id="c81e9-180">스크립트의 출력은 이라는 파일로 **SparkMonitoringDash.json**합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-180">The output from the script is a file named **SparkMonitoringDash.json**.</span></span>

1. <span data-ttu-id="c81e9-181">Grafana 대시보드로 돌아가 **만들기** (더하기 아이콘).</span><span class="sxs-lookup"><span data-stu-id="c81e9-181">Return to the Grafana dashboard and select **Create** (the plus icon).</span></span>
1. <span data-ttu-id="c81e9-182">**가져오기**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-182">Select **Import**.</span></span>
1. <span data-ttu-id="c81e9-183">클릭 **.json 파일을 업로드**합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-183">Click **Upload .json File**.</span></span>
1. <span data-ttu-id="c81e9-184">선택 된 **SparkMonitoringDash.json** 2 단계에서 만든 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-184">Select the **SparkMonitoringDash.json** file created in step 2.</span></span>
1. <span data-ttu-id="c81e9-185">에 **옵션** 섹션의 **ALA**, 이전에 만든 Azure Monitor 데이터 원본을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-185">In the **Options** section, under **ALA**, select the Azure Monitor data source created earlier.</span></span>
1. <span data-ttu-id="c81e9-186">**가져오기**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-186">Click **Import**.</span></span>

## <a name="visualizations-in-the-dashboards"></a><span data-ttu-id="c81e9-187">대시보드에서 시각화</span><span class="sxs-lookup"><span data-stu-id="c81e9-187">Visualizations in the dashboards</span></span>

<span data-ttu-id="c81e9-188">Azure Log Analytics 및 Grafana 대시보드에 시계열 시각화 집합이 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-188">Both the Azure Log Analytics and Grafana dashboards include a set of time-series visualizations.</span></span> <span data-ttu-id="c81e9-189">각 그래프는 Apache Spark와 관련 된 메트릭 데이터의 시계열 도표 [작업](https://spark.apache.org/docs/latest/job-scheduling.html), 작업 및 각 단계를 구성 하는 작업의 단계입니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-189">Each graph is time-series plot of metric data related to an Apache Spark [job](https://spark.apache.org/docs/latest/job-scheduling.html), stages of the job, and tasks that make up each stage.</span></span>

<span data-ttu-id="c81e9-190">시각화는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-190">The visualizations are as follows:</span></span>

### <a name="job-latency"></a><span data-ttu-id="c81e9-191">작업 대기 시간</span><span class="sxs-lookup"><span data-stu-id="c81e9-191">Job latency</span></span>

<span data-ttu-id="c81e9-192">이 시각화는 작업의 전반적인 성능에 대 한 대략적인 뷰는 작업에 대 한 실행 대기 시간을 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-192">This visualization shows execution latency for a job, which is a coarse view on the overall peformance of a job.</span></span> <span data-ttu-id="c81e9-193">시작부터 완료 될 때까지 작업 실행 기간을 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-193">Displays the job execution duration from start to completion.</span></span> <span data-ttu-id="c81e9-194">작업 시작 시간을 참고 작업 제출 시간이 빠른 응모 같지는 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-194">Note that the job start time is not the same as the job submission time.</span></span> <span data-ttu-id="c81e9-195">대기 시간 백분위 수 (10%, 30%, 50%, 90%)로 표시 됩니다. 클러스터 ID 및 응용 프로그램 ID로 인덱싱된 작업 실행</span><span class="sxs-lookup"><span data-stu-id="c81e9-195">Latency is represented as percentiles (10%, 30%, 50%, 90%) of job execution indexed by cluster ID and application ID.</span></span>

### <a name="stage-latency"></a><span data-ttu-id="c81e9-196">스테이지 대기 시간</span><span class="sxs-lookup"><span data-stu-id="c81e9-196">Stage latency</span></span>

<span data-ttu-id="c81e9-197">시각화 클러스터당, 응용 프로그램 및 개별 단계 당 각 단계의 대기 시간을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-197">The visualization shows the latency of each stage per cluster, per application, and per individual stage.</span></span> <span data-ttu-id="c81e9-198">이 시각화는 느리게 실행 되는 특정 단계를 식별 하는 데 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-198">This visualization is useful for identifying a particular stage that is running slowly.</span></span>

### <a name="task-latency"></a><span data-ttu-id="c81e9-199">작업 대기 시간</span><span class="sxs-lookup"><span data-stu-id="c81e9-199">Task latency</span></span>

<span data-ttu-id="c81e9-200">이 시각화 작업 실행 대기 시간을 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-200">This visualization shows task execution latency.</span></span> <span data-ttu-id="c81e9-201">대기 시간 백분위 클러스터, 스테이지 이름 및 응용 프로그램 당 작업 실행으로 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-201">Latency is represented as a percentile of task execution per cluster, stage name, and application.</span></span>

### <a name="sum-task-execution-per-host"></a><span data-ttu-id="c81e9-202">호스트당 합계 작업 실행</span><span class="sxs-lookup"><span data-stu-id="c81e9-202">Sum Task Execution per host</span></span>

<span data-ttu-id="c81e9-203">이 시각화는 호스트 클러스터에서 실행 당 작업 실행 대기의 합계를 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-203">This visualization shows the sum of task execution latency per host running on a cluster.</span></span> <span data-ttu-id="c81e9-204">호스트당 작업 실행 대기 시간 보기 훨씬 더 높은 전체 작업 보다 대기 시간이 다른 호스트가 있는 호스트를 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-204">Viewing task execution latency per host identifies hosts that have much higher overall task latency than other hosts.</span></span> <span data-ttu-id="c81e9-205">이 작업이 비효율적으로 또는 불균등에 게 배포 된 호스트를 의미할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-205">This may mean that tasks have been inefficiently or unevenly distributed to hosts.</span></span>

### <a name="task-metrics"></a><span data-ttu-id="c81e9-206">작업 메트릭</span><span class="sxs-lookup"><span data-stu-id="c81e9-206">Task metrics</span></span>

<span data-ttu-id="c81e9-207">이 시각화에는 지정된 된 태스크의 실행에 대 한 실행 메트릭 집합을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-207">This visualization shows a set of the execution metrics for a given task's execution.</span></span> <span data-ttu-id="c81e9-208">이러한 메트릭에 크기 및 데이터 셔플 기간, serialization 및 deserialization 작업 기간 및 다른 사용자가 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-208">These metrics include the size and duration of a data shuffle, duration of serialization and deserialization operations, and others.</span></span> <span data-ttu-id="c81e9-209">전체 메트릭 집합에 대 한 패널에 대 한 Log Analytics 쿼리를 봅니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-209">For the full set of metrics, view the Log Analytics query for the panel.</span></span> <span data-ttu-id="c81e9-210">이 시각화는 작업과 각 작업의 식별 리소스 소비를 구성 하는 작업을 이해 하는 데 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-210">This visualization is useful for understanding the operations that make up a task and identifying resource consumption of each operation.</span></span> <span data-ttu-id="c81e9-211">그래프에 스파이크가 조사 해야 하는 비용이 많이 드는 작업을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-211">Spikes in the graph represent costly operations that should be investigated.</span></span>

### <a name="cluster-throughput"></a><span data-ttu-id="c81e9-212">클러스터 처리량</span><span class="sxs-lookup"><span data-stu-id="c81e9-212">Cluster throughput</span></span>

<span data-ttu-id="c81e9-213">이 시각화 클러스터 및 응용 프로그램 작업 한 클러스터 및 응용 프로그램의 크기를 표시 하 여 인덱싱된 작업 항목의 높은 수준의 뷰입니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-213">This visualization is a high level view of work items indexed by cluster and application to represent the amount of work done per cluster and application.</span></span> <span data-ttu-id="c81e9-214">작업, 작업 및 클러스터, 응용 프로그램 및 1 분 단위로 단계를 완료 하는 단계 수를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-214">It shows the number of jobs, tasks, and stages completed per cluster, application, and stage in one minute increments.</span></span> 

### <a name="streaming-throughputlatency"></a><span data-ttu-id="c81e9-215">스트리밍 처리량/대기 시간</span><span class="sxs-lookup"><span data-stu-id="c81e9-215">Streaming Throughput/Latency</span></span>

<span data-ttu-id="c81e9-216">이 visualzation 구조화 된 스트리밍 쿼리와 관련 된 메트릭을 관련이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-216">This visualzation is related to the metrics associated with a structured streaming query.</span></span> <span data-ttu-id="c81e9-217">그래프는 초당 입력된 행의 수 및 처리 된 초당 행 수를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-217">The graphs shows the number of input rows per second and the number of rows processed per second.</span></span> <span data-ttu-id="c81e9-218">스트리밍 메트릭은 응용 프로그램당도 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-218">The streaming metrics are also represented per application.</span></span> <span data-ttu-id="c81e9-219">구조적된 스트리밍 쿼리를 처리할와 시각화 나타냅니다 OnQueryProgress 이벤트가 생성 될 때 이러한 메트릭을 전송 됩니다 (밀리초) 쿼리 일괄 처리를 실행 하는 데 걸린 기간 대기 시간이 스트리밍.</span><span class="sxs-lookup"><span data-stu-id="c81e9-219">These metrics are sent when the OnQueryProgress event is generated as the structured streaming query is processed and the visualization represents streaming latency as the amount of time, in milliseconds, taken to execute a query batch.</span></span>

### <a name="resource-consumption-per-executor"></a><span data-ttu-id="c81e9-220">실행 기 당 리소스 사용량</span><span class="sxs-lookup"><span data-stu-id="c81e9-220">Resource consumption per executor</span></span>

<span data-ttu-id="c81e9-221">다음은 대시보드 표시는 특정 종류의 리소스 및 각 클러스터에서 실행 기 당 사용 되는 방법에 대 한 시각화의 집합입니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-221">Next is a set of visualizations for the dashboard show the particular type of resource and how it is consumed per executor on each cluster.</span></span> <span data-ttu-id="c81e9-222">이러한 시각화는 이상 값 실행 기 당 리소스 소비를 식별할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-222">These visualizations help identify outliers in resource consumption per executor.</span></span> <span data-ttu-id="c81e9-223">예를 들어, 특정 실행자에 대 한 작업 할당 왜곡 되 리소스 소비를 클러스터에서 실행 중인 다른 실행자를 기준으로 상승 되는 합니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-223">For example, if the work allocation for a particular executor is skewed, resource consumption will be elevated in relation to other executors running on the cluster.</span></span> <span data-ttu-id="c81e9-224">이 실행 기에 대 한 리소스 사용량 급증 하 여 식별할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-224">This can be identified by spikes in the resource consumption for an executor.</span></span>

### <a name="executor-compute-time-metrics"></a><span data-ttu-id="c81e9-225">실행 기 계산 시간 메트릭</span><span class="sxs-lookup"><span data-stu-id="c81e9-225">Executor compute time metrics</span></span>

<span data-ttu-id="c81e9-226">다음은 실행 기의 비율이 직렬화 시간 표시 시간, CPU 시간 및 Java 가상 머신 시간 전체 실행자 계산 시간으로 역직렬화 하는 대시보드에 대 한 시각화의 집합.</span><span class="sxs-lookup"><span data-stu-id="c81e9-226">Next is a set of visualizations for the dashboard that show the ratio of executor serialize time, deserialize time, CPU time, and Java virtual machine time to overall executor compute time.</span></span> <span data-ttu-id="c81e9-227">이 처리 하는 전체 실행 기에 영향을 주는 시각적으로 얼마나 많은 각 이러한 4 메트릭에 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-227">This demonstrates visually how much each of these four metrics are contributing to overall executor processing.</span></span>

### <a name="shuffle-metrics"></a><span data-ttu-id="c81e9-228">순서 섞기 메트릭</span><span class="sxs-lookup"><span data-stu-id="c81e9-228">Shuffle metrics</span></span>

<span data-ttu-id="c81e9-229">최종 집합 데이터 shuffle 구조적된 스트리밍 쿼리를 사용 하 여 모든 실행 기에서 관련 된 메트릭을 시각화 표시입니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-229">The final set of visualizations show the data shuffle metrics associated with a structured streaming query across all executors.</span></span> <span data-ttu-id="c81e9-230">여기에 읽기, 쓴 바이트 셔플, 파일 시스템 사용 되는 쿼리에서 메모리 및 디스크 사용량의 순서 섞기 바이트 셔플 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c81e9-230">These include shuffle bytes read, shuffle bytes written, shuffle memory, and disk usage in queries where the file system is used.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c81e9-231">다음 단계</span><span class="sxs-lookup"><span data-stu-id="c81e9-231">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c81e9-232">성능 병목 현상 문제 해결</span><span class="sxs-lookup"><span data-stu-id="c81e9-232">Troubleshoot performance bottlenecks</span></span>](./performance-troubleshooting.md)

<!-- links -->

[rm-cli]: /azure/azure-resource-manager/resource-group-template-deploy-cli
[sku]: /azure/templates/Microsoft.OperationalInsights/2015-11-01-preview/workspaces#sku-object