---
title: Enterprise 비즈니스 인텔리전스
titleSuffix: Azure Reference Architectures
description: Azure를 사용하여 온-프레미스에 저장된 관계형 데이터에서 비즈니스 정보를 얻으세요.
author: MikeWasson
ms.date: 11/06/2018
ms.custom: seodec18
ms.openlocfilehash: 3808cc5d09e2e0a5aaee1a6cfcb050b98a0ef2ee
ms.sourcegitcommit: bb7fcffbb41e2c26a26f8781df32825eb60df70c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/20/2018
ms.locfileid: "53644226"
---
# <a name="enterprise-bi-in-azure-with-sql-data-warehouse"></a>SQL Data Warehouse를 사용하는 Azure의 Enterprise BI

이 참조 아키텍처는 온-프레미스 SQL Server 데이터베이스에서 SQL Data Warehouse로 데이터를 이동하고 분석을 위해 데이터를 변경하는 [ELT(추출 부하 변형)](../../data-guide/relational-data/etl.md#extract-load-and-transform-elt) 파이프라인을 구현합니다.

이 아키텍처에 대한 참조 구현은 [GitHub][github-folder]에서 사용할 수 있습니다.

![SQL Data Warehouse를 사용하는 Azure의 Enteprise BI를 위한 아키텍처 다이어그램](./images/enterprise-bi-sqldw.png)

**시나리오**: 조직에는 SQL Server 데이터베이스 온-프레미스에 저장된 대규모 OLTP 데이터 세트가 있습니다. 조직은 Power BI를 사용하여 분석을 수행하기 위해 SQL Data Warehouse를 사용하려고 합니다.

이 참조 아키텍처는 일회성 또는 주문형 작업을 위해 설계되었습니다. 지속적으로(매시간 또는 매일) 데이터를 이동해야 하는 경우 Azure Data Factory를 사용하여 자동화된 워크플로를 정의하는 것이 좋습니다. Data Factory를 사용하는 참조 아키텍처는 [SQL Data Warehouse 및 Azure Data Factory를 사용하는 자동화된 엔터프라이즈 BI][adf-ra]를 참조하세요.

## <a name="architecture"></a>아키텍처

이 아키텍처는 다음 구성 요소로 구성됩니다.

### <a name="data-source"></a>데이터 원본

**SQL Server**. 원본 데이터는 SQL Server 데이터베이스 온-프레미스에 위치합니다. 온-프레미스 환경을 시뮬레이션하기 위해 이 아키텍처에 대한 배포 스크립트는 설치된 SQL Server를 사용하여 Azure에서 VM을 프로비전합니다. [Wide World Importers OLTP 예제 데이터베이스][wwi]는 원본 데이터로 사용됩니다.

### <a name="ingestion-and-data-storage"></a>수집 및 데이터 저장소

**Blob Storage** Blob 저장소는 SQL Data Warehouse로 로딩하기 전에 데이터를 복사하는 준비 영역으로 사용됩니다.

**Azure SQL Data Warehouse** [SQL Data Warehouse](/azure/sql-data-warehouse/)는 대규모 데이터 분석을 수행하도록 설계되고 배포된 시스템입니다. 고성능 분석을 실행하는 데 적합하도록 하는 MPP(대규모 병렬 처리)를 지원합니다.

### <a name="analysis-and-reporting"></a>분석 및 보고

**Azure Analysis Services**. [Analysis Services](/azure/analysis-services/)는 데이터 모델링 기능을 제공하는 완전히 관리되는 서비스입니다. Analysis Services를 사용하여 사용자가 쿼리할 수 있는 의미 체계 모델을 만듭니다. Analysis Services는 BI 대시보드 시나리오에서 특히 유용합니다. 이 아키텍처에서 Analysis Services는 의미 체계 모델을 처리하도록 데이터 웨어하우스에서 데이터를 읽고, 대시보드 쿼리를 효율적으로 처리합니다. 또한 신속한 쿼리 처리에 대한 복제본을 확장하여 탄력적 동시성을 지원합니다.

현재 Azure Analysis Services는 테이블 형식 모델을 지원하지만 다차원 모델을 지원하지는 않습니다. 테이블 형식 모델은 관계형 모델링 구문(테이블 및 열)을 사용하는 반면 다차원 모델은 OLAP 모델링 구문(큐브, 차원 및 측정값)을 사용합니다. 다차원 모델이 필요한 경우 SSAS(SQL Server Analysis Services)를 사용합니다. 자세한 내용은 [테이블 형식 및 다차원 솔루션 비교](/sql/analysis-services/comparing-tabular-and-multidimensional-solutions-ssas)를 참조하세요.

**Power BI**. Power BI는 비즈니스 정보에 대한 데이터를 분석하는 비즈니스 분석 도구 제품군입니다. 이 아키텍처에서 Analysis Services에 저장된 의미 체계 모델을 쿼리합니다.

### <a name="authentication"></a>인증

**Azure Active Directory**(Azure AD)는 Power BI를 통해 Analysis Services 서버에 연결하는 사용자를 인증합니다.

## <a name="data-pipeline"></a>데이터 파이프라인

이 참조 아키텍처는 데이터 원본으로 [WorldWideImporters](/sql/sample/world-wide-importers/wide-world-importers-oltp-database) 샘플 데이터베이스를 사용합니다. 데이터 파이프라인에는 다음 단계가 있습니다.

1. SQL Server에서 플랫 파일로 데이터를 내보냅니다(bcp 유틸리티).
2. Azure Blob Storage로 플랫 파일을 복사합니다(AzCopy).
3. SQL Data Warehouse로 데이터를 로드합니다(PolyBase).
4. 데이터를 별모양 스키마로 변환합니다(T-SQL).
5. 의미 체계 모델을 Analysis Services로 로드합니다(SQL Server Data Tools).

![엔터프라이즈 BI 파이프라인의 다이어그램](./images/enterprise-bi-sqldw-pipeline.png)

> [!NOTE]
> 1&ndash;3단계의 경우 Redgate Data Platform Studio를 사용하는 것이 좋습니다. Data Platform Studio는 가장 적합한 호환성 수정 및 최적화를 적용하므로 SQL Data Warehouse를 시작하는 가장 빠른 방법입니다. 자세한 내용은 [Redgate Data Platform Studio를 사용하여 데이터 로드](/azure/sql-data-warehouse/sql-data-warehouse-load-with-redgate)를 참조하세요.
>

다음 섹션에서는 이러한 단계를 자세히 설명합니다.

### <a name="export-data-from-sql-server"></a>SQL Server에서 데이터 내보내기

[bcp](/sql/tools/bcp-utility)(대량 복사 프로그램) 유틸리티는 SQL 테이블에서 플랫 텍스트 파일을 만드는 신속한 방법입니다. 이 단계에서는 내보내려는 열을 선택하지만 데이터를 변환하지 않습니다. 모든 데이터 변환은 SQL Data Warehouse에서 수행되어야 합니다.

**권장 사항**

가능하면 프로덕션 환경에서 리소스 경합을 최소화하기 위해 사용량이 적은 시간에 데이터 추출을 예약합니다.

데이터베이스 서버에서 bcp를 실행하지 않습니다. 대신 다른 컴퓨터에서 실행합니다. 로컬 드라이브에 파일을 씁니다. 동시 쓰기를 처리하기에 충분한 I/O 리소스가 있는지 확인합니다. 최상의 성능을 위해서 전용 고속 저장소 드라이브에 파일을 내보냅니다.

Gzip 압축된 형식으로 내보낸 데이터를 저장하여 네트워크 전송 속도를 높일 수 있습니다. 그러나 압축된 파일을 웨어하우스로 로드하는 것은 압축되지 않은 파일을 로드하는 것보다 느리므로 더 빠른 네트워크 전송과 더 빠른 로딩 간에 균형 유지가 있습니다. Gzip 압축을 사용하려는 경우 단일 Gzip 파일을 만들지 마십시오. 대신 여러 개의 압축된 파일로 데이터를 분할합니다.

### <a name="copy-flat-files-into-blob-storage"></a>플랫 파일을 Blob 저장소에 복사

[AzCopy](/azure/storage/common/storage-use-azcopy) 유틸리티는 Azure Blob 저장소로 데이터의 고성능 복사를 위해 설계되었습니다.

**권장 사항**

원본 데이터의 위치 근처 지역에서 저장소 계정을 만듭니다. 동일한 지역에 저장소 계정 및 SQL Data Warehouse 인스턴스를 배포합니다.

CPU 및 I/O 사용은 프로덕션 작업에 영향을 줄 수 있으므로 프로덕션 작업을 실행하는 동일한 컴퓨터에서 AzCopy를 실행하지 마십시오.

업로드 속도를 확인하려면 먼저 업로드를 테스트합니다. AzCopy에서 /NC 옵션을 사용하여 동시 복사 작업의 수를 지정할 수 있습니다. 기본 값으로 시작한 다음, 이 설정으로 실험하여 성능을 조정합니다. 저대역폭 환경에서는 많은 수의 동시 작업으로 네트워크 연결에 과부하가 걸려 작업이 성공적으로 실행되지 못할 수 있습니다.

AzCopy는 공용 인터넷을 통해 저장소로 데이터를 이동합니다. 충분히 빠르지 않은 경우 [ExpressRoute](/azure/expressroute/) 회로 설정을 고려합니다. ExpressRoute는 Azure에 대한 전용 비공개 연결을 통해 데이터를 라우팅하는 서비스입니다. 네트워크 연결 속도가 너무 느린 경우 다른 옵션은 디스크의 데이터를 Azure 데이터 센터로 물리적으로 배달하는 것입니다. 자세한 내용은 [Azure 간 데이터 전송](/azure/architecture/data-guide/scenarios/data-transfer)을 참조하세요.

복사 작업 중 AzCopy는 임시 저널 파일을 만듭니다. 이를 통해 중단되는 경우(예: 네트워크 오류로 인해) AzCopy에서 작업을 다시 시작할 수 있습니다. 저널 파일을 저장할 디스크 공간이 충분한지 확인합니다. /Z 옵션을 사용하여 저널 파일이 기록되는 위치를 지정할 수 있습니다.

### <a name="load-data-into-sql-data-warehouse"></a>SQL Data Warehouse로 데이터 로드

[PolyBase](/sql/relational-databases/polybase/polybase-guide)를 사용하여 Blob 저장소에서 데이터 웨어하우스로 파일을 로드합니다. PolyBase는 SQL Data Warehouse의 MPP(대규모 병렬 처리) 아키텍처를 활용하도록 디자인되었으며 가장 빠르게 SQL Data Warehouse로 데이터를 로드할 수 있게 합니다.

데이터 로드는 두 단계 프로세스로 이루어집니다.

1. 데이터에 대한 외부 테이블 집합을 만듭니다. 외부 테이블은 웨어하우스의 외부에 저장된 데이터를 가리키는 테이블 정의이며 &mdash; 이 경우 Blob 저장소의 플랫 파일입니다. 이 단계는 데이터를 웨어하우스로 이동하지 않습니다.
2. 준비 테이블을 만들고, 준비 테이블로 데이터를 로드합니다. 이 단계는 데이터를 웨어하우스로 복사합니다.

**권장 사항**

많은 양의 데이터(1TB 이상)가 있고 병렬 처리를 활용하는 분석 워크 로드를 실행하는 경우 SQL Data Warehouse를 고려합니다. SQL Data Warehouse는 OLTP 워크로드 또는 소량의 데이터 집합(< 250GB)에 잘 맞지 않습니다. 250GB보다 작은 데이터 집합의 경우 Azure SQL Database 또는 SQL Server를 고려합니다. 자세한 내용은 [데이터 웨어하우징](../../data-guide/relational-data/data-warehousing.md)을 참조하세요.

힙 테이블로 인덱싱되지 않은 준비 테이블을 만듭니다. 프로덕션 테이블을 만드는 쿼리는 전체 테이블 검색이 발생하므로 준비 테이블을 인덱싱할 이유가 없습니다.

PolyBase는 웨어하우스에서 자동으로 병렬 처리를 활용합니다. 로드 성능은 DWU를 늘리면 확장합니다. 최상의 성능을 위해 단일 로드 작업을 사용합니다. 입력 데이터를 청크로 분리하고 여러 동시 로드를 실행하는 성능 이점이 없습니다.

PolyBase는 Gzip 압축된 파일을 읽을 수 있습니다. 그러나 파일 압축 풀기는 단일 스레드 작업이므로 압축된 파일당 단일 판독기만 사용됩니다. 따라서 대량의 단일 압축된 파일을 로드하지 않도록 합니다. 대신 병렬 처리를 활용하기 위해 여러 개의 압축된 파일로 데이터를 분할합니다.

다음과 같은 제한 사항을 고려해야 합니다.

- PolyBase는 `varchar(8000)`, `nvarchar(4000)` 또는 `varbinary(8000)`의 최대 열 크기를 지원합니다. 이러한 한도를 초과하는 데이터가 있는 경우 한 가지 옵션은 내보낼 때 데이터를 청크로 분리한 다음, 가져온 후 청크를 다시 어셈블하는 것입니다.

- PolyBase는 \n 또는 새 줄의 고정된 행 종결자를 사용합니다. 새 줄 문자가 원본 데이터에 표시되는 경우 문제가 발생할 수 있습니다.

- 원본 데이터 스키마는 SQL Data Warehouse에서 지원되지 않는 데이터 형식을 포함할 수 있습니다.

이러한 제한 사항을 해결하기 위해 필요한 전환을 수행하는 저장 프로시저를 만들 수 있습니다. bcp를 실행할 때 이 저장 프로시저를 참조합니다. 또는 [Redgate Data Platform Studio](/azure/sql-data-warehouse/sql-data-warehouse-load-with-redgate)에서 SQL Data Warehouse에서 지원되지 않는 데이터 형식을 자동으로 변환합니다.

자세한 내용은 다음 문서를 참조하세요.

- [Azure SQL Data Warehouse에 데이터를 로드하는 모범 사례](/azure/sql-data-warehouse/guidance-for-loading-data)
- [SQL Data Warehouse로 스키마 마이그레이션](/azure/sql-data-warehouse/sql-data-warehouse-migrate-schema)
- [SQL Data Warehouse의 테이블에 대한 데이터 형식을 정의하기 위한 지침](/azure/sql-data-warehouse/sql-data-warehouse-tables-data-types)

### <a name="transform-the-data"></a>데이터 변환

데이터를 변환하고 프로덕션 테이블로 이동합니다. 이 단계에서 데이터는 의미 체계 모델링에 적합한 차원 테이블 및 팩트 테이블을 사용하여 별모양 스키마로 변환됩니다.

전반적으로 최적의 쿼리 성능을 제공하는 클러스터형 columnstore 인덱스가 포함된 프로덕션 테이블을 만듭니다. Columnstore 인덱스는 많은 레코드를 검색하는 쿼리에 최적화됩니다. Columnstore 인덱스는 싱글톤 조회(즉, 단일 행 조회)도 수행하지 않습니다. 자주 싱글톤 조회를 수행해야 하는 경우 테이블에 비클러스터형 인덱스를 추가할 수 있습니다. 싱글톤 조회는 비클러스터형 인덱스를 사용하여 훨씬 더 빠르게 실행할 수 있습니다. 그러나 싱글톤 조회는 일반적으로 OLTP 작업보다 데이터 웨어하우스 시나리오에서 덜 일반적입니다. 자세한 내용은 [SQL Data Warehouse의 테이블 인덱싱](/azure/sql-data-warehouse/sql-data-warehouse-tables-index)을 참조하세요.

> [!NOTE]
> 클러스터형 columnstore 테이블은 `varchar(max)`, `nvarchar(max)` 또는 `varbinary(max)` 데이터 형식을 지원하지 않습니다. 이 경우 힙 또는 클러스터형 인덱스를 고려합니다. 별도 테이블에 해당 열을 넣을 수 있습니다.

샘플 데이터베이스는 매우 크지 않으므로 파티션이 없는 복제된 테이블을 만들었습니다. 프로덕션 작업의 경우 분산 테이블을 사용하면 쿼리 성능을 개선할 수 있습니다. [Azure SQL Data Warehouse의 분산 테이블 디자인 지침](/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute)을 참조하세요. 예제 스크립트는 정적 [리소스 클래스](/azure/sql-data-warehouse/resource-classes-for-workload-management)를 사용하여 쿼리를 실행합니다.

### <a name="load-the-semantic-model"></a>의미 체계 모델 로드

Azure Analysis Services에서 테이블 형식 모델로 데이터를 로드합니다. 이 단계에서는 SSDT(SQL Server Data Tools)를 사용하여 의미 체계 데이터 모델을 만듭니다. 또한 Power BI Desktop 파일에서 가져와서 모델을 만들 수도 있습니다. SQL Data Warehouse는 외래 키를 지원하지 않으므로 테이블에서 조인할 수 있도록 관계를 의미 체계 모델에 추가해야 합니다.

### <a name="use-power-bi-to-visualize-the-data"></a>Power BI를 사용하여 데이터 시각화

Power BI는 Azure Analysis Services에 연결하기 위한 두 가지 옵션을 지원합니다.

- 가져오기 데이터를 Power BI 모델로 가져옵니다.
- 라이브 연결 데이터를 Analysis Services에서 직접 가져옵니다.

Power BI 모델로 데이터를 복사할 필요가 없기 때문에 라이브 연결을 권장합니다. 또한 DirectQuery를 사용하면 결과는 항상 최신 원본 데이터와 일치하게 됩니다. 자세한 내용은 [Power BI로 연결](/azure/analysis-services/analysis-services-connect-pbi)을 참조하세요.

**권장 사항**

데이터 웨어하우스에 대해 직접 BI 대시보드 쿼리를 실행하지 마십시오. BI 대시보드는 웨어하우스에 대한 직접 쿼리가 충족할 수 없는 매우 낮은 응답 시간이 필요합니다. 또한 대시보드를 새로 고치면 성능에 영향을 줄 수 있는 동시 쿼리 수에 불리하게 간주됩니다.

Azure Analysis Services는 BI 대시보드의 쿼리 요구 사항을 처리하도록 설계되었으므로 Power BI에서 Analysis Services를 쿼리하는 것이 좋습니다.

## <a name="scalability-considerations"></a>확장성 고려 사항

### <a name="sql-data-warehouse"></a>SQL Data Warehouse

SQL Data Warehouse를 사용하여 주문형 계산 리소스를 확장할 수 있습니다. 쿼리 엔진은 계산 노드 수에 따라 병렬 처리에 대한 쿼리를 최적화하고, 필요에 따라 노드 간에 데이터를 이동합니다. 자세한 내용은 [Azure SQL Data Warehouse에서 계산 관리](/azure/sql-data-warehouse/sql-data-warehouse-manage-compute-overview)를 참조하세요.

### <a name="analysis-services"></a>Analysis Services

프로덕션 작업의 경우 Azure Analysis Services에 대한 표준 계층은 분할 및 DirectQuery를 지원하므로 이를 권장합니다. 계층 내에서 인스턴스 크기는 메모리 및 처리 용량을 결정합니다. 처리 능력은 QPU(쿼리 처리 단위)로 측정됩니다. QPU 사용량을 모니터링하여 적절한 크기를 선택합니다. 자세한 내용은 [서버 메트릭 모니터링](/azure/analysis-services/analysis-services-monitor)을 참조하세요.

높은 부하 상태에서 쿼리 성능은 쿼리 동시성으로 인해 저하될 수 있습니다. 더 많은 쿼리를 동시에 수행할 수 있도록 쿼리를 처리하는 복제본의 풀을 만들어 Analysis Services를 확장할 수 있습니다. 데이터 모델 처리 작업은 항상 주 서버에서 발생합니다. 기본적으로 주 서버도 쿼리를 처리합니다. 필요에 따라 쿼리 풀이 모든 쿼리를 처리하도록 단독으로 처리를 실행하도록 주 서버를 지정할 수 있습니다. 높은 처리 요구 사항이 있는 경우 쿼리 풀에서 처리를 구분해야 합니다. 높은 쿼리 부하와 상대적으로 약한 처리가 있는 경우 쿼리 풀에 주 서버를 포함할 수 있습니다. 자세한 내용은 [Azure Analysis Services 확장](/azure/analysis-services/analysis-services-scale-out)을 참조하세요.

불필요한 처리 시간을 줄이기 위해 테이블 형식 모델을 논리적 부분으로 분할하는 데 파티션을 사용하는 것이 좋습니다. 각 파티션은 개별적으로 처리될 수 있습니다. 자세한 내용은 [파티션](/sql/analysis-services/tabular-models/partitions-ssas-tabular)을 참조하세요.

## <a name="security-considerations"></a>보안 고려 사항

### <a name="ip-whitelisting-of-analysis-services-clients"></a>Analysis Services 클라이언트의 IP 허용 목록

클라이언트 IP 주소를 허용 목록에 추가하는 데 Analysis Services 방화벽 기능을 사용하는 것이 좋습니다. 활성화된 경우 방화벽은 방화벽 규칙에 지정된 것 이외의 모든 클라이언트 연결을 차단합니다. 기본 규칙은 Power BI 서비스를 허용 목록에 추가하지만 필요한 경우 이 규칙을 비활성화할 수 있습니다. 자세한 내용은 [새 방화벽 기능을 사용하여 Azure Analysis Services 보안 강화](https://azure.microsoft.com/blog/hardening-azure-analysis-services-with-the-new-firewall-capability/)를 참조하세요.

### <a name="authorization"></a>권한 부여

Azure Analysis Services는 Azure AD(Azure Active Directory)를 사용하여 Analysis Services 서버에 연결하는 사용자를 인증합니다. 역할을 만든 다음, 해당 역할에 Azure AD 사용자 또는 그룹을 할당하여 특정 사용자가 볼 수 있는 데이터를 제한할 수 있습니다. 각 역할의 경우 다음을 수행할 수 있습니다.

- 테이블 또는 개별 열을 보호합니다.
- 필터 식에 따라 개별 행을 보호합니다.

자세한 내용은 [데이터베이스 역할 및 사용자 관리](/azure/analysis-services/analysis-services-database-users)를 참조하세요.

## <a name="deploy-the-solution"></a>솔루션 배포

참조 구현을 배포하고 실행하려면 [GitHub readme][github-folder]의 단계를 따릅니다. 다음을 배포합니다.

- 온-프레미스 데이터베이스 서버를 시뮬레이션하는 Windows VM Power BI Desktop과 함께 SQL Server 2017 및 관련된 도구를 포함합니다.
- SQL Server 데이터베이스에서 가져온 데이터를 저장할 Blob 저장소를 제공하는 Azure 저장소 계정
- Azure SQL Data Warehouse 인스턴스
- Azure Analysis Services 인스턴스

## <a name="next-steps"></a>다음 단계

- Azure Data Factory를 사용하여 ELT 파이프라인을 자동화합니다. [SQL Data Warehouse 및 Azure Data Factory를 사용하는 자동화된 Enterprise BI][adf-ra]를 참조하세요.

## <a name="related-resources"></a>관련 리소스

동일한 기술 중 일부를 사용하여 특정 솔루션을 보여주는 다음 [Azure 예제 시나리오](/azure/architecture/example-scenario)를 검토해 보세요.

- [영업 및 마케팅에 대한 데이터 웨어하우징 및 분석](/azure/architecture/example-scenario/data/data-warehouse)
- [기존 온-프레미스 SSIS와 Azure Data Factory를 사용한 하이브리드 ETL](/azure/architecture/example-scenario/data/hybrid-etl-with-adf)

<!-- links -->

[adf-ra]: ./enterprise-bi-adf.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/data/enterprise_bi_sqldw
[wwi]: /sql/sample/world-wide-importers/wide-world-importers-oltp-database
