---
title: Azure Monitor를 사용하여 Azure Databricks 모니터링
description: Azure Log Analytics에서 Azure Databricks의 모니터링을 사용하도록 설정하기 위한 Scala 라이브러리
author: petertaylor9999
ms.date: 03/26/2019
ms.openlocfilehash: 6d3d17b2e49919aea7bb08f59e19032c1c8bdafd
ms.sourcegitcommit: 9854bd27fb5cf92041bbfb743d43045cd3552a69
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58503274"
---
# <a name="monitoring-azure-databricks-with-azure-monitor"></a>Azure Monitor를 사용하여 Azure Databricks 모니터링

[Azure Databricks](/azure/azure-databricks/)는 빠르고, 강력한 [Apache Spark](https://spark.apache.org/) 기반 분석 서비스로, 빅 데이터 분석 및 AI(인공 지능) 솔루션을 빠르게 개발하고 배포하는 데 도움이 됩니다. 많은 사용자가 Azure Databricks 솔루션에서 Notebook의 단순성을 활용합니다. Azure Databricks는 더 강력한 컴퓨팅 옵션이 필요한 사용자를 위해 사용자 지정 애플리케이션 코드의 분산된 실행을 지원합니다.

모니터링은 모든 프로덕션 수준 솔루션에서 중요한 부분으로, Azure Databricks는 사용자 지정 애플리케이션 메트릭, 스트리밍 쿼리 이벤트 및 애플리케이션 로그 메시지를 모니터링하기 위한 강력한 기능을 제공합니다. Azure Databricks는 이 모니터링 데이터를 여러 로깅 서비스로 보낼 수 있습니다.

다음 문서에서는 Azure Databricks에서 Azure를 위한 모니터링 데이터 플랫폼인 [Azure Monitor](/azure/azure-monitor/overview)로 모니터링 데이터를 보내는 방법을 보여줍니다. 다음 문서를 순서대로 수행해야 합니다.

1. [Azure Databricks를 구성하여 Azure Monitor에 메트릭 보내기](./configure-cluster.md)
1. [Azure Databricks 애플리케이션 로그를 Azure Monitor에 보내기](./application-logs.md)
1. [대시보드를 사용하여 Azure Databricks 메트릭 시각화](./dashboards.md)

이러한 문서와 함께 제공되는 코드 라이브러리는 Azure Databricks의 핵심 모니터링 기능을 확장하여 Spark 메트릭, 이벤트 및 로깅 정보를 Azure Monitor로 보냅니다.

이러한 문서 및 함께 제공되는 코드 라이브러리는 Apache Spark 및 Azure Databricks 솔루션 개발자를 위한 것입니다. 코드는 JAR(Java 보관) 파일을 기반으로 빌드한 다음, Azure Databricks 클러스터에 배포해야 합니다. 코드는 [Scala](https://www.scala-lang.org/)와 Java의 조합이며, 출력 JAR 파일을 빌드하기 위한 [Maven](https://maven.apache.org) POM(프로젝트 개체 모델) 파일 세트가 있습니다. 먼저 Java, Scala 및 Maven을 이해하는 것이 좋습니다.

## <a name="next-steps"></a>다음 단계

코드 라이브러리를 빌드하고 Azure Databricks 클러스터에 배포하여 시작합니다.

> [!div class="nextstepaction"]
> [Azure Databricks를 구성하여 Azure Monitor에 메트릭 보내기](./configure-cluster.md)