---
title: 일괄 처리 기술 선택
description: ''
author: zoinerTejada
ms.date: 11/03/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.openlocfilehash: 53f8b233b0e0c1ff83a72a04b2707caa528d6f6b
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58248518"
---
# <a name="choosing-a-batch-processing-technology-in-azure"></a>Azure에서 일괄 처리 기술 선택

빅 데이터 솔루션은 종종 장기 실행 일괄 처리 작업을 사용하여 데이터를 필터링 및 집계하고, 분석을 위해 준비합니다. 일반적으로 이러한 작업이 수행되는 동안 확장 가능한 스토리지(예: HDFS, Azure Data Lake Store 및 Azure Storage)에서 원본 데이터를 읽고, 처리하고, 확장 가능한 스토리지의 새 파일에 출력을 씁니다.

대량의 데이터를 처리하기 위해 이러한 일괄 처리 엔진은 기본적으로 계산 능력을 스케일 아웃할 수 있어야 합니다. 그러나 실시간 처리와 달리, 일괄 처리는 분 단위에서 시간 단위로 측정되는 대기 시간(데이터 수집 시간과 결과 계산 사이의 간격)을 발생하게 됩니다.

## <a name="technology-choices-for-batch-processing"></a>일괄 처리에 사용할 기술 선택

### <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse

[SQL Data Warehouse](/azure/sql-data-warehouse/)는 대규모 데이터 분석을 수행하도록 설계되고 배포된 시스템입니다. 고성능 분석을 실행하는 데 적합하도록 하는 MPP(대규모 병렬 처리)를 지원합니다. 많은 양의 데이터(1TB 이상)가 있고 병렬 처리를 활용하는 분석 워크 로드를 실행하는 경우 SQL Data Warehouse를 고려합니다.

### <a name="azure-data-lake-analytics"></a>Azure 데이터 레이크 분석

[Data Lake Analytics](/azure/data-lake-analytics/data-lake-analytics-overview)는 주문형 분석 작업 서비스입니다. Azure Data Lake Store에 저장된 거대한 데이터 세트의 분산 처리에 최적화되었습니다.

- 언어: [U-SQL](/azure/data-lake-analytics/data-lake-analytics-u-sql-get-started)(Python, R 및 C# 확장 포함).
- Azure Data Lake Store, Azure Storage Blob, Azure SQL Database 및 SQL Data Warehouse와 통합됩니다.
- 가격 책정 모델은 작업에 따라 다릅니다.

### <a name="hdinsight"></a>HDInsight

HDInsight는 관리 Hadoop 서비스입니다. Hadoop 클러스터를 Azure에 배포하고 관리하는 데 사용됩니다. 일괄 처리에는 [Spark](/azure/hdinsight/spark/apache-spark-overview), [Hive](/azure/hdinsight/hadoop/hdinsight-use-hive), [Hive LLAP](/azure/hdinsight/interactive-query/apache-interactive-query-get-started), [MapReduce](/azure/hdinsight/hadoop/hdinsight-use-mapreduce)를 사용할 수 있습니다.

- 언어: R, Python, Java, Scala, SQL
- Active Directory, Apache Ranger 기반 액세스 제어를 사용한 Kerberos 인증
- Hadoop 클러스터 전체에 대한 컨트롤 제공

### <a name="azure-databricks"></a>Azure Databricks

[Azure Databricks](/azure/azure-databricks/)는 Apache Spark 기반 분석 플랫폼입니다. "서비스로 제공되는 Spark"라고 생각하시면 됩니다. Azure 플랫폼에서 Spark를 사용하는 가장 쉬운 방법입니다.

- 언어: R, Python, Java, Scala, Spark SQL
- 빠른 클러스터 시작 시간, 자동 종료, 자동 크기 조정을 제공합니다.
- 사용자 대신 Spark 클러스터를 관리합니다.
- Azure Blob Storage, ADLS(Azure Data Lake Storage), SQL DW(Azure SQL Data Warehouse) 및 기타 서비스와 기본적으로 통합됩니다. [데이터 원본](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html)을 참조하세요.
- Azure Active Directory를 사용하여 사용자를 인증합니다.
- 공동 작업 및 데이터 탐색용 웹 기반 [Notebook](https://docs.azuredatabricks.net/user-guide/notebooks/index.html)을 제공합니다.
- [GPU 사용 클러스터](https://docs.azuredatabricks.net/user-guide/clusters/gpu.html) 지원

### <a name="azure-distributed-data-engineering-toolkit"></a>Azure 분산 데이터 엔지니어링 도구 키트

[AZTK(분산 데이터 엔지니어링 도구 키트](https://github.com/azure/aztk))는 Azure의 Docker 클러스터에 주문형 Spark를 프로비전하는 도구입니다.

AZTK는 Azure 서비스가 아닙니다. CLI 및 Python SDK 인터페이스를 사용하는 클라이언트 쪽 도구이며, Azure Batch를 기반으로 합니다. 이 옵션은 Spark 클러스터를 배포할 때 대부분의 인프라를 제어할 수 있습니다.

- 사용자 고유의 Docker 이미지를 사용합니다.
- 우선 순위가 낮은 VM을 사용하면 80% 할인 혜택을 받을 수 있습니다.
- 우선 순위가 낮은 VM과 높은 VM을 모두 사용하는 혼합 모드 클러스터입니다.
- Azure Blob Storage 및 Azure Data Lake 연결을 기본적으로 지원합니다.

## <a name="key-selection-criteria"></a>주요 선택 조건

선택 옵션의 범위를 좁히려면 먼저 다음 질문에 답변합니다.

- 사용자 고유의 서버를 관리하지 않고 관리되는 서비스를 원하시나요?

- 선언적 또는 명령적 방식 중에서 어떤 방식으로 일괄 처리 논리를 작성하려고 하나요?

- 일괄 처리를 많이 수행할 예정인가요? 그렇다면 클러스터를 자동 종료할 수 있는 옵션 또는 가격 책정 모델이 일괄 작업 기준인 옵션을 고려해야 합니다.

- 예를 들어 참조 데이터를 조회하기 위해 일괄 처리 방식으로 관계형 데이터 저장소를 쿼리해야 하나요? 그렇다면 외부 관계형 저장소의 쿼리를 허용하는 옵션을 고려합니다.

## <a name="capability-matrix"></a>기능 매트릭스

다음 표에서는 주요 기능 차이점을 요약해서 보여 줍니다.

### <a name="general-capabilities"></a>일반 기능

<!-- markdownlint-disable MD033 -->

| | Azure 데이터 레이크 분석 | Azure SQL Data Warehouse | HDInsight | Azure Databricks |
| --- | --- | --- | --- | --- | --- |
| 관리되는 서비스인지 여부 | 예 | 예 | 예 <sup>1</sup> | 예 |
| 관계형 데이터 저장소 | 예 | 예 | 아니요 | 아니요 |
| 가격 책정 모델 | 일괄 처리 작업 기준 | 클러스터 시간 기준 | 클러스터 시간 기준 | Databricks 단위<sup>2</sup> + 클러스터 시간 |

[1] 수동 구성 및 크기 조정 사용

[2] DBU(Databricks 단위)는 시간당 처리 능력의 단위입니다.

### <a name="capabilities"></a>기능

| | Azure 데이터 레이크 분석 | SQL Data Warehouse | HDInsight(Spark 포함) | HDInsight(Spark 포함) | HDInsight(Hive LLAP 포함) | Azure Databricks |
| --- | --- | --- | --- | --- | --- | --- |
| 자동 확장 | 아니요 | 아니요 | 아니요 | 아니요 | 아니요 | 예 |
| 스케일 아웃 단위  | 작업 기준 | 클러스터 기준 | 클러스터 기준 | 클러스터 기준 | 클러스터 기준 | 클러스터 기준 |
| 데이터의 메모리 내 캐싱 | 아니요 | 예 | 예 | no | 예 | 예 |
| 외부 관계형 저장소에서 쿼리 | 예 | no | 예 | 아니요 | 아니요 | 예 |
| Authentication  | Azure AD | SQL / Azure AD | 아니요 | Azure AD<sup>1</sup> | Azure AD<sup>1</sup> | Azure AD |
| 감사  | 예 | 예 | 아니요 | 예 <sup>1</sup> | 예 <sup>1</sup> | 예 |
| 행 수준 보안 | 아니요 | 아니요 | 아니요 | 예 <sup>1</sup> | 예 <sup>1</sup> | 아니요 |
| 방화벽 지원 여부 | 예 | 예 | 예 | 예 <sup>2</sup> | 예 <sup>2</sup> | 아니요 |
| 동적 데이터 마스킹 | 아니요 | 아니요 | 아니요 | 예 <sup>1</sup> | 예 <sup>1</sup> | 아니요 |

<!-- markdownlint-enable MD033 -->

[1] [도메인 가입 HDInsight 클러스터](/azure/hdinsight/domain-joined/apache-domain-joined-introduction)를 사용해야 합니다.

[2] [Azure Virtual Network 내에서 사용](/azure/hdinsight/hdinsight-extend-hadoop-virtual-network)할 때 지원됩니다.
