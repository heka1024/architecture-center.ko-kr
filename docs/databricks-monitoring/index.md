---
title: Azure Monitor를 사용하여 Azure Databricks 모니터링
description: Azure Log Analytics에서 Azure Databricks의 모니터링을 사용하도록 설정하기 위한 Scala 라이브러리
author: petertaylor9999
ms.date: 03/26/2019
ms.openlocfilehash: 93798ccf74735a880eab2999008b1495e6a63e10
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59640536"
---
# <a name="monitoring-azure-databricks"></a><span data-ttu-id="bb59c-103">Azure Databricks 모니터링</span><span class="sxs-lookup"><span data-stu-id="bb59c-103">Monitoring Azure Databricks</span></span>

<span data-ttu-id="bb59c-104">[Azure Databricks](/azure/azure-databricks/)는 빠르고, 강력한 [Apache Spark](https://spark.apache.org/) 기반 분석 서비스로, 빅 데이터 분석 및 AI(인공 지능) 솔루션을 빠르게 개발하고 배포하는 데 도움이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="bb59c-104">[Azure Databricks](/azure/azure-databricks/) is a fast, powerful [Apache Spark](https://spark.apache.org/)–based analytics service that makes it easy to rapidly develop and deploy big data analytics and artificial intelligence (AI) solutions.</span></span> <span data-ttu-id="bb59c-105">많은 사용자가 Azure Databricks 솔루션에서 Notebook의 단순성을 활용합니다.</span><span class="sxs-lookup"><span data-stu-id="bb59c-105">Many users take advantage of the simplicity of notebooks in their Azure Databricks solutions.</span></span> <span data-ttu-id="bb59c-106">Azure Databricks는 더 강력한 컴퓨팅 옵션이 필요한 사용자를 위해 사용자 지정 애플리케이션 코드의 분산된 실행을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="bb59c-106">For users that require more robust computing options, Azure Databricks supports the distributed execution of custom application code.</span></span>

<span data-ttu-id="bb59c-107">모니터링은 모든 프로덕션 수준 솔루션에서 중요한 부분으로, Azure Databricks는 사용자 지정 애플리케이션 메트릭, 스트리밍 쿼리 이벤트 및 애플리케이션 로그 메시지를 모니터링하기 위한 강력한 기능을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="bb59c-107">Monitoring is a critical part of any production-level solution, and Azure Databricks offers robust functionality for monitoring custom application metrics, streaming query events, and application log messages.</span></span> <span data-ttu-id="bb59c-108">Azure Databricks는 이 모니터링 데이터를 여러 로깅 서비스로 보낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bb59c-108">Azure Databricks can send this monitoring data to different logging services.</span></span>

<span data-ttu-id="bb59c-109">다음 문서에서는 Azure Databricks에서 Azure를 위한 모니터링 데이터 플랫폼인 [Azure Monitor](/azure/azure-monitor/overview)로 모니터링 데이터를 보내는 방법을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="bb59c-109">The following articles show how to send monitoring data from Azure Databricks to [Azure Monitor](/azure/azure-monitor/overview), the monitoring data platform for Azure.</span></span> <span data-ttu-id="bb59c-110">다음 문서를 순서대로 수행해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb59c-110">You should follow these articles in order.</span></span>

1. [<span data-ttu-id="bb59c-111">Azure Databricks를 구성하여 Azure Monitor에 메트릭 보내기</span><span class="sxs-lookup"><span data-stu-id="bb59c-111">Configure Azure Databricks to send metrics to Azure Monitor</span></span>](./configure-cluster.md)
1. [<span data-ttu-id="bb59c-112">Azure Databricks 애플리케이션 로그를 Azure Monitor에 보내기</span><span class="sxs-lookup"><span data-stu-id="bb59c-112">Send Azure Databricks application logs to Azure Monitor</span></span>](./application-logs.md)
1. [<span data-ttu-id="bb59c-113">대시보드를 사용하여 Azure Databricks 메트릭 시각화</span><span class="sxs-lookup"><span data-stu-id="bb59c-113">Use dashboards to visualize Azure Databricks metrics</span></span>](./dashboards.md)

<span data-ttu-id="bb59c-114">이러한 문서와 함께 제공되는 코드 라이브러리는 Azure Databricks의 핵심 모니터링 기능을 확장하여 Spark 메트릭, 이벤트 및 로깅 정보를 Azure Monitor로 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="bb59c-114">The code library that accompanies these articles extends the core monitoring functionality of Azure Databricks to send Spark metrics, events, and logging information to Azure Monitor.</span></span>

<span data-ttu-id="bb59c-115">이러한 문서 및 함께 제공되는 코드 라이브러리는 Apache Spark 및 Azure Databricks 솔루션 개발자를 위한 것입니다.</span><span class="sxs-lookup"><span data-stu-id="bb59c-115">The audience for these articles and the accompanying code library are Apache Spark and Azure Databricks solution developers.</span></span> <span data-ttu-id="bb59c-116">코드는 JAR(Java 보관) 파일을 기반으로 빌드한 다음, Azure Databricks 클러스터에 배포해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb59c-116">The code must be built into Java Archive (JAR) files and then deployed to an Azure Databricks cluster.</span></span> <span data-ttu-id="bb59c-117">코드는 [Scala](https://www.scala-lang.org/)와 Java의 조합이며, 출력 JAR 파일을 빌드하기 위한 [Maven](https://maven.apache.org) POM(프로젝트 개체 모델) 파일 세트가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bb59c-117">The code is a combination of [Scala](https://www.scala-lang.org/) and Java, with a corresponding set of [Maven](https://maven.apache.org) project object model (POM) files to build the output JAR files.</span></span> <span data-ttu-id="bb59c-118">먼저 Java, Scala 및 Maven을 이해하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="bb59c-118">Understanding of Java, Scala, and Maven are recommended as prerequisites.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bb59c-119">다음 단계</span><span class="sxs-lookup"><span data-stu-id="bb59c-119">Next steps</span></span>

<span data-ttu-id="bb59c-120">코드 라이브러리를 빌드하고 Azure Databricks 클러스터에 배포하여 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="bb59c-120">Start by building the code library and deploying it to your Azure Databricks cluster.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="bb59c-121">Azure Databricks를 구성하여 Azure Monitor에 메트릭 보내기</span><span class="sxs-lookup"><span data-stu-id="bb59c-121">Configure Azure Databricks to send metrics to Azure Monitor</span></span>](./configure-cluster.md)