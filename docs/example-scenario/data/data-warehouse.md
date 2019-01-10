---
title: 영업 및 마케팅에 대한 데이터 웨어하우징 및 분석
titleSuffix: Azure Example Scenarios
description: 여러 원본의 데이터를 통합하고 데이터 분석을 최적화합니다.
author: alexbuckgit
ms.date: 09/15/2018
ms.openlocfilehash: 2ac06fcd0805b66371fcc004794b123c46a6ce0e
ms.sourcegitcommit: 1f4cdb08fe73b1956e164ad692f792f9f635b409
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54112382"
---
# <a name="data-warehousing-and-analytics-for-sales-and-marketing"></a>영업 및 마케팅에 대한 데이터 웨어하우징 및 분석

이 예제 시나리오에서는 여러 원본의 대규모 데이터를 Azure의 통합 분석 플랫폼에 통합하는 데이터 파이프라인을 보여줍니다. 이 시나리오는 영업 및 마케팅 솔루션을 기반으로 하지만, 디자인 패턴은 전자상거래, 소매, 의료 등과 같이 대형 데이터 세트를 분석하는 고급 기능이 필요한 여러 산업과 관련이 있습니다.

이 예제에서는 인센티브 프로그램을 만드는 영업 및 마케팅 회사를 보여줍니다. 이러한 프로그램은 고객, 공급업체, 영업 사원 및 직원에게 보상을 제공합니다. 이러한 프로그램에서는 데이터가 매우 중요하며, 회사는 Azure를 사용하여 데이터 분석을 통해 얻은 인사이트를 개선하는 방법을 원합니다.

회사에서는 적시에 적절한 데이터를 사용하여 의사를 결정할 수 있도록 최신 데이터 분석 기법을 원합니다. 이 회사의 목표는 다음과 같습니다.

- 여러 종류의 데이터 소스를 클라우드 규모 플랫폼 하나로 결합합니다.
- 데이터의 일관성을 유지하고 쉽게 비교할 수 있도록 원본 데이터를 일반적인 분류 및 구조체로 변환합니다.
- 많은 비용을 들여 온-프레미스 인프라를 배포하고 유지 관리할 필요 없이, 수천 개의 인센티브 프로그램을 지원할 수 있는 고도로 병렬화된 방법을 사용하여 데이터를 로드합니다.
- 데이터 분석에 집중할 수 있도록 데이터를 수집하고 변환하는 데 필요한 시간을 대폭 줄입니다.

## <a name="relevant-use-cases"></a>관련 사용 사례

다음의 경우 이 방법을 사용할 수도 있습니다.

- 데이터 웨어하우스를 믿을 수 있는 단일 데이터 원본으로 설정합니다.
- 관계형 데이터 원본을 구조화되지 않은 다른 데이터 세트와 통합합니다.
- 의미 체계 모델링 및 강력한 시각화 도구를 사용하여 간단하게 데이터를 분석합니다.

## <a name="architecture"></a>아키텍처

![Azure의 데이터 웨어하우징 및 분석 시나리오를 위한 아키텍처][architecture]

솔루션을 통한 데이터 흐름은 다음과 같습니다.

1. 각 데이터 원본에서 주기적으로 Azure Blob 저장소의 스테이징 영역으로 업데이트가 내보내집니다.
2. Data Factory가 Blob 저장소의 데이터를 SQL Data Warehouse의 스테이징 테이블에 증분 방식으로 로드합니다. 이 프로세스에서 데이터가 정리 및 변환됩니다. Polybase는 큰 데이터 세트에 대한 프로세스를 병렬로 처리할 수 있습니다.
3. 새로운 데이터 일괄 처리를 웨어하우스에 로드하면 이전에 만든 Analysis Services 테이블 형식 모델이 새로 고침됩니다. 이 의미 체계 모델은 비즈니스 데이터 및 관계 분석을 간소화합니다.
4. 비즈니스 분석가는 Microsoft Power BI를 사용하여 Analysis Services 의미 체계 모델을 통해 웨어하우징된 데이터를 분석합니다.

### <a name="components"></a>구성 요소

회사는 여러 플랫폼에 걸쳐 데이터 원본을 갖고 있습니다.

- SQL Server 온-프레미스
- Oracle 온-프레미스
- Azure SQL Database
- Azure Table Storage
- Cosmos DB

여러 Azure 구성 요소를 사용하여 여러 데이터 원본에서 데이터가 로드됩니다.

- [Blob 저장소](/azure/storage/blobs/storage-blobs-introduction)는 SQL Data Warehouse로 로드될 원본 데이터를 스테이징하는 데 사용됩니다.
- [Data Factory](/azure/data-factory)는 스테이징된 데이터를 SQL Data Warehouse의 일반적인 구조체로 변환할 때 오케스트레이션을 수행합니다. Data Factory는 [SQL Data Warehouse에 데이터를 로드할 때 Polybase를 사용](/azure/data-factory/connector-azure-sql-data-warehouse#use-polybase-to-load-data-into-azure-sql-data-warehouse)하여 처리량을 극대화합니다.
- [SQL Data Warehouse](/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is)는 대규모 데이터 세트를 저장하고 분석하는 분산 시스템입니다. MPP(대규모 병렬 처리)를 사용하기 때문에 고성능 분석을 실행하는 데 적합합니다. SQL Data Warehouse [PolyBase](/sql/relational-databases/polybase/polybase-guide)를 사용하여 Blob 저장소에서 신속하게 데이터를 로드할 수 있습니다.
- [Analysis Services](/azure/analysis-services)는 데이터에 대한 의미 체계 모델을 제공합니다. 또한 데이터를 분석할 때 시스템 성능을 향상합니다.
- [Power BI](/power-bi) 는 데이터를 분석하고 통찰력을 공유하는 비즈니스 분석 도구 제품군입니다. Power BI는 Analysis Services에 저장된 의미 체계 모델을 쿼리할 수도 있고, SQL Data Warehouse를 직접 쿼리할 수도 있습니다.
- [Azure Active Directory](/azure/active-directory)(Azure AD)는 Power BI를 통해 Analysis Services 서버에 연결하는 사용자를 인증합니다. 또한 Data Factory는 Azure AD를 사용하여 서비스 주체 또는 [Azure 리소스용 관리 ID](/azure/active-directory/managed-identities-azure-resources/overview)를 통해 SQL Data Warehouse를 인증할 수 있습니다.

### <a name="alternatives"></a>대안

- 이 예제 파이프라인에는 여러 종류의 데이터 원본이 포함되어 있습니다. 이 아키텍처는 다양한 관계형 및 비관계형 데이터 원본을 처리할 수 있습니다.
- Data Factory는 데이터 파이프라인에 대한 워크플로를 오케스트레이션합니다. 데이터를 한 번만 또는 주문이 있을 때만 로드하려면 SQL Server 대량 복사(bcp) 및 AzCopy 같은 도구를 사용하여 Blob 저장소로 데이터를 복사하면 됩니다. 그런 다음, Polybase를 사용하여 SQL Data Warehouse로 데이터를 직접 로드하면 됩니다.
- 매우 큰 데이터 집합이 있는 경우 분석 데이터용 무제한 스토리지를 제공하는 [Data Lake Storage](/azure/storage/data-lake-storage/introduction)를 고려해 볼 수 있습니다.
- 온-프레미스 [SQL Server 병렬 데이터 웨어하우스](/sql/analytics-platform-system) 어플라이언스를 빅 데이터 처리에도 사용할 수 있습니다. 그러나 SQL Data Warehouse 같은 관리되는 클라우드 기반 솔루션을 사용하면 운영 비용이 대폭 절감되는 것을 자주 볼 수 있습니다.
- SQL Data Warehouse는 250GB 미만 OLTP 워크로드 또는 데이터 집합에 적합하지 않습니다. 이러한 시나리오에는 Azure SQL Database 또는 SQL Server를 사용해야 합니다.
- 다른 대안을 비교하면 다음 항목을 참조하세요.

  - [Azure의 데이터 파이프라인 오케스트레이션 기술 선택](/azure/architecture/data-guide/technology-choices/pipeline-orchestration-data-movement)
  - [Azure에서 일괄 처리 기술 선택](/azure/architecture/data-guide/technology-choices/batch-processing)
  - [Azure에서 분석 데이터 저장소 선택](/azure/architecture/data-guide/technology-choices/analytical-data-stores)
  - [Azure에서 데이터 분석 기술 선택](/azure/architecture/data-guide/technology-choices/analysis-visualizations-reporting)

## <a name="considerations"></a>고려 사항

이 아키텍처의 기술을 선택한 이유는 회사의 확장성 및 가용성 요구 사항을 충족하면서도 비용을 조절할 수 있기 때문입니다.

- SQL Data Warehouse의 [방대한 병렬 처리 아키텍처](/azure/sql-data-warehouse/massively-parallel-processing-mpp-architecture)는 우수한 확장성 및 성능을 제공합니다.
- SQL Data Warehouse는 [SLA를 보장](https://azure.microsoft.com/support/legal/sla/sql-data-warehouse)하며, [고가용성을 달성하기 위한 권장 사례를 제공](/azure/sql-data-warehouse/sql-data-warehouse-best-practices)합니다.
- 분석 작업이 적은 경우 회사에서 [수요에 따라 SQL Data Warehouse의 규모를 조정](/azure/sql-data-warehouse/sql-data-warehouse-manage-compute-overview)하여 계산을 줄이거나 일시 중지하여 비용을 줄일 수 있습니다.
- 쿼리 워크로드가 높은 시간에는 Azure Analysis Services를 [규모 확장](/azure/analysis-services/analysis-services-scale-out)하여 응답 시간을 줄일 수 있습니다. 처리 작업 때문에 클라이언트 쿼리가 느려지지 않도록 쿼리 풀에서 처리를 분리할 수도 있습니다.
- 또한 Azure Analysis Services는 [SLA를 보장](https://azure.microsoft.com/support/legal/sla/analysis-services)하며, [고가용성을 달성하기 위한 권장 사례를 제공](/azure/analysis-services/analysis-services-bcdr)합니다.
- [SQL Data Warehouse 보안 모델](/azure/sql-data-warehouse/sql-data-warehouse-overview-manage-security)은 Azure AD 또는 SQL Server 인증과 암호화를 통해 연결 보안, [인증 및 권한 부여](/azure/sql-data-warehouse/sql-data-warehouse-authentication)를 제공합니다. [Azure Analysis Services](/azure/analysis-services/analysis-services-manage-users)는 ID 관리 및 사용자 인증에 Azure AD를 사용합니다.

## <a name="pricing"></a>가격

Azure 가격 계산기를 통해 [데이터 웨어하우징 시나리오를 위한 가격 책정 샘플][calculator]을 검토하세요. 값을 조정하여 요구 사항이 비용에 미치는 영향을 확인할 수 있습니다.

- [SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/gen2)는 계산 및 저장 수준을 독립적으로 조정할 수 있습니다. 계산 리소스는 시간당 요금이 청구되며, 수요에 따라 이러한 리소스를 조정하거나 일시 중지할 수 있습니다. 저장소 리소스는 테라바이트 단위로 요금이 청구되므로 수집하는 데이터의 양이 많을수록 비용이 증가합니다.
- [Data Factory](https://azure.microsoft.com/pricing/details/data-factory) 비용은 워크로드에서 수행된 읽기/쓰기 작업, 모니터링 작업 및 오케스트레이션 작업의 수에 따라 결정됩니다. 데이터 스트림이 추가되고 각 데이터 스트림에서 처리하는 데이터 양이 증가할 때마다 Data Factory 비용이 증가합니다.
- [Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services)는 개발자, 기본 및 표준 계층에서 사용할 수 있습니다. 인스턴스는 QPU(쿼리 처리 단위) 및 가용 메모리를 기준으로 가격이 결정됩니다. 비용을 낮게 유지하려면 실행하는 쿼리 수, 쿼리에서 처리하는 데이터의 양, 쿼리가 실행되는 빈도를 최소화하세요.
- [Power BI](https://powerbi.microsoft.com/pricing)는 다양한 요구 사항에 대한 다양한 제품 옵션을 갖고 있습니다. [Power BI Embedded](https://azure.microsoft.com/pricing/details/power-bi-embedded)는 애플리케이션 내부에 Power BI 기능을 포함할 수 있는 Azure 기반 옵션을 제공합니다. Power BI Embedded 인스턴스는 위의 가격 책정 샘플에 포함되어 있습니다.

## <a name="next-steps"></a>다음 단계

- 이 아키텍처 인스턴스를 Azure에 배포하는 방법에 대한 지침이 포함된 [자동화 엔터프라이즈 BI를 위한 Azure 참조 아키텍처](/azure/architecture/reference-architectures/data/enterprise-bi-adf)를 검토합니다.
- [Maritz Motivation Solutions 고객 사례][source-document]를 검토합니다. 이 스토리는 고객 데이터를 관리하는 비슷한 방법을 설명합니다.
- [Azure 데이터 아키텍처 가이드](/azure/architecture/data-guide)의 데이터 파이프라인, 데이터 웨어하우징, OLAP(온라인 분석 처리) 및 빅 데이터에 대한 포괄적인 아키텍처 지침을 찾아봅니다.

<!-- links -->

[source-document]: https://customers.microsoft.com/story/maritz
[calculator]: https://azure.com/e/b798fb70c53e4dd19fdeacea4db78276
[architecture]: ./media/architecture-data-warehouse.png
