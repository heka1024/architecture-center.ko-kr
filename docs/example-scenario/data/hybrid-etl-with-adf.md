---
title: 기존 온-프레미스 SSIS와 Azure Data Factory를 사용한 하이브리드 ETL
titleSuffix: Azure Example Scenarios
description: 기존 온-프레미스 SSIS(SQL Server Integration Services) 배포와 Azure Data Factory를 사용한 하이브리드 ETL
author: alhieng
ms.date: 9/20/2018
ms.custom: tsp-team
ms.openlocfilehash: 387b0aa1927a8d316aad76100f577da13833eae6
ms.sourcegitcommit: bb7fcffbb41e2c26a26f8781df32825eb60df70c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/20/2018
ms.locfileid: "53643515"
---
# <a name="hybrid-etl-with-existing-on-premises-ssis-and-azure-data-factory"></a>기존 온-프레미스 SSIS와 Azure Data Factory를 사용한 하이브리드 ETL

SQL Server 데이터베이스를 클라우드로 마이그레이션하는 조직은 엄청난 비용 절감, 성능 향상, 추가 유연성 및 확장성을 얻을 수 있습니다. 그러나 SSIS(SQL Server Integration Services)를 사용하여 빌드된 기존의 ETL(추출, 변환 및 로드) 프로세스를 다시 작업하는 것은 마이그레이션에 방해가 될 수 있습니다. 또는 데이터 로드 프로세스에는 아직 Azure Data Factory v2에서 지원되지 않는 복잡한 논리 및/또는 특정 데이터 도구 구성 요소가 필요합니다. 자주 사용되는 SSIS 기능으로는 유사 항목 조회 및 유사 항목 그룹화 변환, CDC(변경 데이터 캡처), SCD(느린 변경 차원) 및 DQS(Data Quality Services)가 있습니다.

기존 SQL 데이터베이스를 간편하게 리프트 앤 시프트 방식으로 마이그레이션하는 가장 적합한 옵션은 하이브리드 ETL입니다. 하이브리드 접근 방식은 Data Factory를 기본 오케스트레이션 엔진으로 사용하지만, 기존 SSIS 패키지를 계속 활용하여 데이터를 정리하고 온-프레미스 리소스를 사용합니다. 이 방법은 Data Factory SQL Server IR(통합 런타임)을 사용하여 기존 데이터베이스를 클라우드로 리프트 앤 시프트할 수 있도록 지원하는 한편, 기존 코드 및 SSIS 패키지를 사용합니다.

이 예제 시나리오는 데이터베이스를 클라우드로 이동하고, Data Factory를 기본 클라우드 기반 ETL 엔진으로 사용하고 기존 SSIS 패키지를 새 클라우드 데이터 워크플로에 통합하는 방안을 고려하는 조직과 관련이 있습니다. 여러 조직에서 특정 데이터 작업을 위한 SSIS ETL 패키지 개발에 많은 비용을 투자했습니다. 이러한 패키지를 다시 작성하기가 어려울 수 있습니다. 또한 많은 기존 코드 패키지가 로컬 리소스에 종속되어 있어서 클라우드로 마이그레이션할 수 없습니다.

Data Factory를 사용하면 고객은 기존 ETL 패키지를 활용하는 한편 온-프레미스 ETL 개발에 대한 추가 투자를 제한할 수 있습니다. 이 예제에서는 Azure Data Factory v2를 사용하는 새 클라우드 데이터 워크플로의 일부분으로 기존 SSIS 패키지를 활용하는 잠재적 사용 사례를 설명합니다.

## <a name="potential-use-cases"></a>잠재적인 사용 사례

일반적으로 SSIS는 여러 SQL Server 데이터 전문가들이 데이터 변환 및 로드를 위해 선택하는 ETL 도구입니다. 종종 개발 속도를 높이기 위해 특정 SSIS 기능 또는 타사 플러그 인 구성 요소를 사용하기도 합니다. 이러한 패키지를 바꾸거나 다시 개발하는 방법은 올바른 선택이 아닙니다. 고객이 데이터베이스를 클라우드로 마이그레이션할 수 없기 때문입니다. 고객은 기존 데이터베이스를 클라우드로 마이그레이션하고 기존 SSIS 패키지를 활용하는 데 영향을 많이 미치지 않는 방법을 원합니다.

다음은 몇 가지 가능한 온-프레미스 사용 사례입니다.

- 네트워크 라우터 로그를 데이터베이스에 로드하여 분석합니다.
- 분석 보고용 인적 자원 고용 데이터를 준비합니다.
- 제품 및 판매 데이터를 데이터 웨어하우스에 로드하여 판매량을 예측합니다.
- 재무 및 회계 관리용 운영 데이터 저장소 또는 데이터 웨어하우스 로드를 자동화합니다.

## <a name="architecture"></a>아키텍처

![Azure Data Factory를 사용하는 하이브리드 ETL 프로세스의 아키텍처 개요][architecture-diagram]

1. 데이터는 Blob 스토리지에서 Data Factory로 제공됩니다.
2. Data Factory 파이프라인은 통합 런타임을 통해 온-프레미스에 호스트된 SSIS 작업을 실행하는 저장 프로시저를 호출합니다.
3. 데이터 정리 작업이 실행되어 다운스트림에 사용할 데이터가 준비됩니다.
4. 데이터 정리 작업이 완료되면 정리 데이터를 Azure에 로드하는 복사 작업이 실행됩니다.
5. 그러면 정리 데이터가 SQL Data Warehouse의 테이블에 로드됩니다.

### <a name="components"></a>구성 요소

- [Blob 스토리지][docs-blob-storage]는 파일을 저장하는 데 사용되며 Data Factory가 데이터를 검색하는 원본으로도 사용됩니다.
- [SQL Server Integration Services][docs-ssis]는 작업별 워크로드를 실행하는 데 사용되는 온-프레미스 ETL 패키지를 포함하고 있습니다.
- [Azure Data Factory][docs-data-factory]는 여러 원본에서 데이터를 가져와서 결합하고, 오케스트레이션하고, 데이터 웨어하우스에 데이터를 로드하는 클라우드 오케스트레이션 엔진입니다.
- [SQL Data Warehouse][docs-sql-data-warehouse]는 표준 ANSI SQL 쿼리를 사용하여 쉽게 액세스할 수 있도록 데이터를 클라우드에 중앙 집중화합니다.

### <a name="alternatives"></a>대안

Data Factory는 Databricks Notebook, Python 스크립트, 가상 머신에서 실행되는 SSIS 인스턴스 등의 다른 기술을 사용하여 구현된 데이터 정리 프로시저를 호출할 수 있습니다. 하이브리드 방식의 대안으로 [Azure-SSIS 통합 런타임을 위한 유료 또는 라이선스 방식의 사용자 지정 구성 요소를 설치](/azure/data-factory/how-to-develop-azure-ssis-ir-licensed-components)할 수 있습니다.

## <a name="considerations"></a>고려 사항

IR(통합 런타임)은 자체 호스팅 IR 또는 Azure 호스팅 IR의 두 가지 모델을 지원합니다. 먼저 두 옵션 중 무엇을 사용할지 결정해야 합니다. 자체 호스팅은 경제적이지만 유지 관리 및 관리 오버헤드가 증가합니다. 자세한 내용은 [자체 호스팅 IR](/azure/data-factory/concepts-integration-runtime#self-hosted-integration-runtime)을 참조하세요. 어떤 IR을 사용할지 결정하는 데 도움이 필요한 경우 [사용할 IR 결정](/azure/data-factory/concepts-integration-runtime#determining-which-ir-to-use)을 참조하세요.

Azure 호스팅 방법의 경우 데이터 처리에 필요한 성능을 결정해야 합니다. Azure 호스팅 구성을 사용하면 구성 단계에서 VM 크기를 선택할 수 있습니다. VM 크기 선택에 대한 자세한 내용은 [VM 성능 고려 사항](/azure/cloud-services/cloud-services-sizes-specs#performance-considerations)을 참조하세요.

Azure에서 액세스할 수 없는 데이터 원본 또는 파일처럼 온-프레미스에 종속된 기존 SSIS 패키지가 이미 있는 경우 의사 결정이 훨씬 간단합니다. 이 시나리오에서 선택 가능한 유일한 옵션은 자체 호스팅 IR입니다. 이 방법은 기존 패키지를 다시 작성할 필요 없이 클라우드를 오케스트레이션 엔진으로 활용하는 최고의 유연성을 제공합니다.

궁극적인 의도는 처리된 데이터를 클라우드로 이동하여 추가로 구체화하거나 클라우드에 저장된 다른 데이터와 결합하는 것입니다. 디자인 프로세스의 일부로 Data Factory 파이프라인에서 사용되는 작업의 수를 추적합니다. 자세한 내용은 [Azure Data Factory의 파이프라인 및 작업](/azure/data-factory/concepts-pipelines-activities)을 참조하세요.

## <a name="pricing"></a>가격

Data Factory는 클라우드에서 데이터 이동을 오케스트레이션하는 비용 효율적인 방법입니다. 비용은 여러 가지 요소에 따라 달라집니다.

- 파이프라인 실행 수
- 파이프라인 내에서 사용되는 엔터티/작업의 수
- 모니터링 작업의 수
- 통합 실행 횟수(Azure 호스팅 IR 또는 자체 호스팅 IR)

Data Factory는 사용량을 기준으로 요금이 청구됩니다. 따라서 파이프라인을 실행하고 모니터링하는 동안에만 비용이 발생합니다. 기본 파이프라인 실행 시 발생하는 비용은 50센트, 모니터링 시 발생하는 비용은 25센트에 불과합니다. [Azure 비용 계산기](https://azure.microsoft.com/pricing/calculator/)를 사용하여 워크로드에 따른 정확한 비용을 계산할 수 있습니다.

하이브리드 ETL 워크로드를 실행하는 경우 SSIS 패키지를 호스트하는 데 사용되는 가상 머신의 비용을 고려해야 합니다. 이 비용은 D1v2(1코어, 3.5GB RAM, 50GB 디스크)부터 E64V3(64코어, 432GB RAM, 1600GB 디스크)에 이르는 VM 크기에 따라 결정됩니다. 적절한 VM 크기를 선택하는 방법에 대한 자세한 지침은 [VM 성능 고려 사항](/azure/cloud-services/cloud-services-sizes-specs#performance-considerations)을 참조하세요.

## <a name="next-steps"></a>다음 단계

- [Azure Data Factory](https://azure.microsoft.com/services/data-factory/)에 대해 자세히 알아봅니다.
- [단계별 자습서](/azure/data-factory/#step-by-step-tutorials)를 수행하여 Azure Data Factory를 시작합니다.
- [Azure Data Factory에서 Azure-SSIS 통합 런타임을 프로비전](/azure/data-factory/tutorial-deploy-ssis-packages-azure)합니다.

<!-- links -->
[architecture-diagram]: ./media/architecture-diagram-hybrid-etl-with-adf.png
[small-pricing]: https://azure.com/e/
[medium-pricing]: https://azure.com/e/
[large-pricing]: https://azure.com/e/
[availability]: /azure/architecture/checklist/availability
[resource-groups]: /azure/azure-resource-manager/resource-group-overview
[resiliency]: /azure/architecture/resiliency/
[security]: /azure/security/
[scalability]: /azure/architecture/checklist/scalability
[docs-blob-storage]: /azure/storage/blobs/
[docs-data-factory]: /azure/data-factory/introduction
[docs-resource-groups]: /azure/azure-resource-manager/resource-group-overview
[docs-ssis]: /sql/integration-services/sql-server-integration-services
[docs-sql-data-warehouse]: /azure/sql-data-warehouse/sql-data-warehouse-overview-what-is
