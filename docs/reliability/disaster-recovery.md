---
title: Azure 응용 프로그램에 대 한 오류 및 재해 복구
description: Azure에서 재해 복구의 개요에 도달
author: MikeWasson
ms.date: 04/10/2019
ms.topic: article
ms.service: architecture-center
ms.subservice: cloud-design-principles
ms.openlocfilehash: f00341b06b8092c798d6260317226190cbace66b
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59646655"
---
# <a name="failure-and-disaster-recovery-for-azure-applications"></a>Azure 응용 프로그램에 대 한 오류 및 재해 복구

*재해 복구* 치명적인 손실에 응용 프로그램 기능을 복원 하는 프로세스입니다.

재해가 발생 한 동안 제한 된 기능에 대 한 허용 범위는 다음 응용 프로그램에서 달라 지는 비즈니스 의사 결정 합니다. 완전히 사용할 수 없게 또는 제한 된 기능을 사용 하 여 부분적으로 사용 가능한 일부 응용 프로그램에 허용 될 수 또는 특정 기간에 대 한 처리를 지연 합니다. 다른 응용 프로그램에 대 한 모든 제한 된 기능 허용 되지 않습니다.

## <a name="disaster-recovery-plan"></a>재해 복구 계획

복구 계획을 만들어 시작 합니다. 계획을 완전히 테스트 된 후 완료 간주 됩니다. 인력, 프로세스 및 고객에 대해 정의한 서비스 수준 계약 (SLA) 내에서 기능을 복원 하는 데 필요한 응용 프로그램을 포함 합니다.

만들기 및 재해 복구 계획을 테스트 하는 경우 다음 사항을 고려 합니다.

- 계획, 지원에 문의 하 고 문제를 에스컬레이션에 대 한 프로세스를 포함 합니다. 이 정보는 복구 프로세스를 처음으로 작업 하는 동안 장시간된 가동 중지 시간을 방지 하는 데 도움이 됩니다.
- 애플리케이션 오류가 비즈니스에 미치는 영향을 평가합니다.
- 업무용 응용 프로그램에 대 한 지역 간 복구 아키텍처를 선택 합니다.
- 자동화 및 테스트를 비롯한 재해 복구 계획의 구체적인 소유자를 식별합니다.
- 프로세스, 특히 수동 단계를 문서화 합니다.
- 가능한 한 프로세스를 자동화 합니다.
- 모든 참조 및 트랜잭션 데이터 및 백업 복원을 테스트에 대 한 백업 전략을 정기적으로 설정 합니다.
- 응용 프로그램에서 사용 되는 Azure 서비스의 스택에 대 한 경고를 설정 합니다.
- 교육 운영 담당자가 계획을 실행 합니다.
- 유효성을 검사 하 고 계획을 개선 하려면 정기적인 재해 시뮬레이션을 수행 합니다.

사용 중인 경우 [Azure Site Recovery](/azure/site-recovery/) virtual machines (Vm)에 복제 하려면 전체 응용 프로그램을 장애 조치할 완전히 자동화 된 복구 계획을 만듭니다.

## <a name="manual-responses"></a>수동 응답

Automation을 이상적인 있지만 수동 응답 재해 복구를 위한 몇 가지 전략에 필요 합니다.

### <a name="alerts"></a>경고

애플리케이션을 모니터링하여 사전 개입이 필요할 수도 있는 경고 기호를 확인합니다. 예를 들어, Azure SQL Database 또는 Azure Cosmos DB는 일관 되 게 응용 프로그램을 제한, 데이터베이스 용량을 늘리거나 쿼리를 최적화 해야 할 수 있습니다. 응용 프로그램이 제한 오류를 투명 하 게 처리할 수 있습니다, 경우에 원격 분석 해야 계속 경고 확인할 수 있습니다.

서비스 제한 및 할당량 임계값에 대 한 Azure 리소스 메트릭 및 진단 로그에 경고를 구성 하는 것이 좋습니다. 가능한 경우 진단 로그 보다 더 낮은 대기 시간을 되는 메트릭에 대 한 경고를 설정 합니다.

통해 [Resource Health](/azure/service-health/resource-health-checks-resource-types), Azure는 Azure 서비스 제한 문제를 진단 하는 데 도움이 되는 상태 검사 몇 가지 기본 제공 상태를 제공 합니다.

### <a name="failover"></a>장애 조치(failover)

각 Azure 응용 프로그램 및 Azure 서비스에 대 한 재해 복구 전략을 구성 합니다. 허용 가능한 배포 전략을 재해 복구를 지원 하도록 각 응용 프로그램의 모든 구성 요소에 필요한 Sla에 따라 달라질 수 있습니다.  

Azure와 같은 수동 장애 조치를 허용 하도록 여러 Azure 서비스 내에서 다양 한 기능을 제공 [redis 캐시 지리적 복제본](/azure/azure-cache-for-redis/cache-how-to-geo-replication), 또는 대 한 자동화 된 장애 조치의 경우와 같은 [SQL 자동 장애 조치 그룹](/azure/sql-database/sql-database-auto-failover-group)합니다. 예를 들면 다음과 같습니다.

- 주로 virtual machines를 사용 하 여 응용 프로그램에 대 한 웹 및 논리 계층에 대 한 Azure Site Recovery를 사용할 수 있습니다. 자세한 내용은 [에서 Azure로 재해 복구 아키텍처](/azure/site-recovery/azure-to-azure-architecture)합니다. Vm의 SQL Server를 사용 하 여 [SQL Server Always On 가용성 그룹](/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-dr)합니다.
- App Service 및 Azure SQL Database를 사용 하는 응용 프로그램을 위해 보조 지역 장애 조치가 발생 하는 경우는 자동 크기 조정에에서 구성 된 작은 계층 App Service 계획을 사용할 수 있습니다. 데이터베이스 계층에 대 한 장애 조치 그룹을 사용 합니다.

두 경우 모두를 [Azure Traffic Manager](/azure/traffic-manager/traffic-manager-overview) 프로필 지역 간 자동 장애 조치를 제공 합니다. [부하 분산 장치](/azure/load-balancer/load-balancer-overview) 나 [application gateway](/azure/application-gateway/overview) 설정 해야 보조 지역에 장애 조치 시 빠른 가용성을 지원 하도록 합니다.

### <a name="operational-readiness-testing"></a>운영 준비 테스트

주 지역으로 장애 복구 및 보조 지역으로 장애 조치에 대 한 운영 준비 테스트를 수행 합니다. 많은 Azure 서비스에서 재해 복구 훈련을 위해 수동 장애 조치(failover)나 테스트 장애 조치를 지원합니다. 또는 종료 하거나 Azure 서비스를 제거 하 여 가동 중단을 시뮬레이션할 수 있습니다.

## <a name="application-failure"></a>애플리케이션 오류

응용 프로그램 오류는 복구할 수 또는 복구할 수 없는 합니다. 복구 가능한 오류를 줄일 수 있습니다 하지만 복구할 수 없는 오류는 응용 프로그램을 종료 합니다.

- 오류를 자동으로 처리 하 고 대체 작업을 수행 하 여 일부 오류를 투명 하 게 해결할 수 있습니다. 예를 들어, Traffic Manager는 호스트 가상 머신에서 기본 하드웨어 또는 운영 체제 소프트웨어로 인 한 오류를 자동으로 처리 합니다.
- 몇 가지 오류를 사용 하 여 응용 프로그램은 제한 된 기능을 사용 하 여 사용자 요청을 처리 계속 수 있습니다.
- 더 심각한 서비스 중단 사용할 수 없는 응용 프로그램을 렌더링할 수도 있습니다.

서비스 수준에서 책임을 구분 하는 잘 설계 된 시스템 &mdash; 디자인 타임 및 런타임 시. 이 분리 전체 응용 프로그램 중지 시 키 지 종속 서비스 중단을 방지 합니다. 예를 들어, 다음 모듈을 사용 하 여 웹 상거래 응용 프로그램을 고려 합니다.

![아트에 링크 추가](./_images/disaster-recovery.png)

호스팅 주문을 위한 데이터베이스가 다운 되 면 주문 처리 서비스가 판매 트랜잭션을 처리할 수 없습니다. 아키텍처에 따라 해당 수 없습니다 주문 제출 및 주문 처리 서비스를 계속할 수 있습니다. 있습니다 그러나 제품 데이터가 다른 위치에 저장 된 경우 제품 카탈로그는 여전히 사용할 수 있지만 응용 프로그램의 다른 부분을 사용할 수 없습니다.

이 사용자가 응용 프로그램 사용자에 게 모든 임시 문제 제공 하는 방법을 결정 하 고 빌드 시스템에 적절 한 알림입니다. 이전 예제에서는 제품 보기 및 장바구니에 추가 하는 응용 프로그램 수 있습니다. 그러나 고객이 구매할 때 응용 프로그램 알려야 순서 지정 기능을 일시적으로 사용할 수 없습니다. 고객에 게는 적합 하지는 않지만이 이렇게는 응용 프로그램 전체 서비스 중단을 방지 합니다.

## <a name="data-corruption-and-restoration"></a>데이터 손상 및 복원

데이터 저장소에 실패 하면 있을 데이터 불일치를 다시 사용할 수 있을 때 데이터를 복제 하는 경우에 특히. 복구 시간 목표 (RTO) 및 복구 지점 목표 (RPO) 복제 된 데이터 저장소의 예측할 수 있도록 데이터 손실의 양 이해 합니다.

지역 간 장애 조치 수동 또는 Microsoft에서 시작 되었는지을 이해 하려면 Azure 서비스 Sla를 검토 합니다. 지역 간 장애 조치 없는 Sla 사용 하 여 서비스에 대 한 Microsoft는 일반적으로 장애 조치할 시기를 결정 하 고 일반적으로 주 지역에서 데이터의 복구 우선 순위를 지정 합니다. 주 지역의 데이터는 복구할 수 없는 것으로 간주 됩니다, Microsoft를 통해 보조 지역으로 실패 합니다.

### <a name="restoring-data-from-backups"></a>백업에서 데이터를 복원합니다.

백업은은 실수로 인 한 삭제 또는 데이터 손상으로 인해 응용 프로그램의 구성 요소를 손실 하면 보호. 이러한 복원 하는 데 사용할 수 있는 이전 시점에서 구성 요소의 기능 버전을 유지 합니다.

재해 복구 전략의 백업에 대 한 대체 하지는 않지만 응용 프로그램 데이터를 정기적으로 백업 일부 재해 복구 시나리오를 지원 합니다. 백업 저장소 옵션에 재해 복구 전략을 기반으로 해야 합니다.

백업 프로세스를 실행 하는 빈도가 RPO를 결정 합니다. 예를 들어 매시간 백업을 수행 하 고 재해 발생 2 분 동안 백업 데이터의 58 분 끊어집니다. 재해 복구 계획은 손실 된 데이터를 어떻게 해결가 포함 되어야 합니다.

다른 저장소에 참조 데이터에 하나의 데이터 저장소에서 데이터에 대 한 일반적인 것입니다. 예를 들어 열을 사용 하 여 SQL Database를 Azure Storage에서 blob에에서 연결 되는 고려 합니다. 백업이 동시에 수행 하지 않습니다, 경우 데이터베이스의 실패 하기 전에 백업 되지 않은 blob에 대 한 포인터가 있을 수 있습니다. 응용 프로그램 또는 재해 복구 계획에 복구 후의 이러한 불일치를 처리 하는 프로세스를 구현 해야 합니다.

> [!NOTE]
> 사용 하 여 백업 하는 Vm의 것과 같은 일부 시나리오에서는 [Azure Backup](/azure/backup/backup-azure-vms-first-look-arm), 동일한 지역에 백업 에서만에서 복원할 수 있습니다. 와 같은 다른 Azure 서비스 [redis Azure Cache](/azure/azure-cache-for-redis/cache-how-to-geo-replication), 지역에서 서비스를 복원 하는 데 사용할 수 있는 지역에서 복제 된 백업을 제공 합니다.

### <a name="azure-storage-and-azure-sql-database"></a>Azure Storage 및 Azure SQL Database

Azure는 자동으로 동일한 지역에 세 번 여러 장애 도메인 내 Azure Storage 및 SQL Database 데이터를 저장합니다. 지역에서 복제를 사용하는 경우 데이터는 다른 지역에 세 번 중복해서 저장됩니다. 그러나 데이터가 손상 되거나 (예를 들어, 사용자 오류 때문에) 주 복사본에서 삭제, 변경 내용을 다른 복사본으로 복제 합니다.

잠재적 데이터 손상 또는 삭제를 관리 하기 위한 두 가지 옵션이 있습니다.

- **사용자 지정 백업 전략을 만듭니다.** Azure 또는 온-프레미스 비즈니스 요구 사항 및 거 버 넌 스 규정에 따라 백업을 저장할 수 있습니다.
- **특정 시점 복원 옵션을 사용 하 여** SQL 데이터베이스를 복구할 수 있습니다.

#### <a name="azure-storage-recovery"></a>Azure Storage 복구

Azure Storage에 대 한 사용자 지정 백업 프로세스를 개발 하거나 여러 타사 백업 도구 중 하나를 사용할 수 있습니다.

Azure Storage는 자동화 된 복제본을 통해 데이터 복원 력을 제공 하지만 응용 프로그램 코드 또는 사용자가 데이터 손상 방지 합니다. 데이터를 유지 관리 응용 프로그램 또는 사용자 오류 발생 후 정확도 감사 로그를 사용 하 여 보조 저장소 위치에 데이터를 복사 하는 등 고급 기술이 필요 합니다. 여러 옵션이 있습니다.

- [**블록 blob입니다.**](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs) 각 블록 Blob의 지정 시간 스냅숏을 만듭니다. 각 스냅숏에 대 한 이전 스냅숏 상태 이후 blob 내의 차이점을 저장 하는 데 필요한 저장소에 대해서만 요금이 청구 됩니다. 스냅숏은 원본 blob에 종속 되므로 다른 blob 또는 다른 저장소 계정에 복사 하는 것이 좋습니다. 이 방법을 사용 하면 실수로 삭제 되지 않도록 백업 데이터를 보호 하 합니다. 사용 하 여 [AzCopy](/azure/storage/common/storage-use-azcopy) 하거나 [Azure PowerShell](/azure/storage/common/storage-powershell-guide-full) 다른 저장소 계정으로 blob을 복사 합니다.

    자세한 내용은 [Blob의 스냅숏 만들기](/rest/api/storageservices/creating-a-snapshot-of-a-blob)를 참조하세요.

- [**Azure Files.**](/azure/storage/files/storage-files-introduction) 사용 하 여 [공유 스냅숏](/azure/storage/files/storage-snapshots-files), AzCopy 또는 PowerShell을 사용해 파일을 다른 저장소 계정으로 복사 합니다.
- [**Azure 테이블 저장소입니다.**](/azure/storage/tables/table-storage-overview) AzCopy를 사용하여 다른 지역에 있는 다른 저장소 계정으로 테이블 데이터를 내보냅니다.

#### <a name="sql-database-recovery"></a>SQL Database 복구

자동으로 SQL Database 데이터 손실 로부터 비즈니스를 보호 하기 위해 매주 전체 데이터베이스 백업과 함께 수행, 매시간 차등 데이터베이스 백업 및 트랜잭션 로그 백업을 마다 5-10 분입니다. 계층은 Basic, Standard 및 Premium SQL Database에 대 한 사용 하 여 지정 시간 복원 하려면 복원 이전 시점으로 데이터베이스입니다. 자세한 내용은 다음 문서를 참조 하세요.

- [자동화된 데이터베이스 백업을 사용하여 Azure SQL 데이터베이스 복구](/azure/sql-database/sql-database-recovery-using-backups)
- [Azure SQL Database의 비즈니스 연속성 개요](/azure/sql-database/sql-database-business-continuity)

동일 하거나 다른 Azure 지역에 보조 데이터베이스를 데이터베이스 변경 내용을 자동으로 복제 하는 SQL Database에 대 한 활성 지역 복제를 사용 하는 방법도 있습니다. 자세한 내용은 [만들기 및 활성 지역 복제를 사용 하 여](/azure/sql-database/sql-database-active-geo-replication)입니다.

백업에 대 한 수동 접근 방식을 사용 하 고 복원할 수 있습니다.

- 사용 된 **데이터베이스 복사본** 트랜잭션 일관성을 사용 하 여 데이터베이스의 백업 복사본을 만드는 명령입니다.
- Azure Blob storage에 저장 된 BACPAC 파일 (데이터베이스 스키마 및 연결 된 데이터가 포함 된 압축 된 파일)에 데이터베이스 내보내기를 지 원하는 Azure SQL Database Import/Export 서비스를 사용 합니다. 지역 전체의 서비스 중단을 방지 하려면 대체 지역에 BACPAC 파일을 복사 합니다.

### <a name="sql-server-on-vms"></a>VM의 SQL Server

Vm에서 실행 되는 SQL Server에 대해 두 가지 옵션이 있습니다: 전통적인 백업과 로그 전달 합니다.

- 전통적인 백업을 사용 하 여 시간 내에 특정 시점으로 복원할 수 있지만 복구 프로세스가 느립니다. 전통적인 백업을 복원 초기 전체 백업으로 시작 하 고 모든 증분 백업을 적용 해야 합니다.
- 로그 백업의 복원을 지연 하도록 로그 전달 세션을 구성할 수 있습니다. 주 복제본에 대 한 오류에서 복구에 대 한 창을 제공 합니다.

### <a name="azure-database-for-mysql-and-azure-database-for-postgresql"></a>Azure Database for MySQL 및 Azure Database for PostgreSQL

Azure Database for MySQL 및 PostgreSQL 용 Azure Database에서 데이터베이스를 백업을 5 분 마다 자동으로 수행 합니다. 서버 및 해당 데이터베이스를 이전 시점에서 시점으로 복원 하는 새 서버에 이러한 자동화 된 백업을 사용할 수 있습니다. 자세한 내용은 다음을 참조하세요.

- [Azure Portal을 사용하여 Azure Database for MySQL에서 서버를 백업 및 복원하는 방법](/azure/mysql/howto-restore-server-portal)
- [백업 및 Azure portal을 사용 하 여 PostgreSQL 용 Azure Database에서 서버를 복원 하는 방법](/azure/postgresql/howto-restore-server-portal)

### <a name="azure-cosmos-db"></a>Azure Cosmos DB

Cosmos DB는 정기적으로 백업을 자동으로 만듭니다. 백업은 다른 저장소 서비스에 개별적으로 저장 됩니다 하 고 지역 재해 로부터 보호 하기 위해 전역적으로 복제 됩니다. 데이터베이스 또는 컬렉션을 실수로 삭제한 경우 지원 티켓을 제출하거나 Azure 지원에 문의하여 마지막 자동 백업에서 데이터를 복원할 수 있습니다. 자세한 내용은 [온라인 백업 및 Azure Cosmos DB의 요청 시 복원](/azure/cosmos-db/online-backup-and-restore)합니다.

### <a name="azure-virtual-machines"></a>Azure Virtual Machines

Azure Virtual Machines를 응용 프로그램 오류 또는 우발적 삭제 로부터 보호를 사용 하 여 [Azure Backup](/azure/backup/)합니다. 여러 VM 디스크에서 만든된 백업을 일관성이.입니다. 또한 지역 손실 로부터 복구를 지원 하기 위해 지역에서 Azure Backup 자격 증명 모음을 복제할 수 있습니다.

## <a name="network-outage"></a>네트워크 중단

Azure 네트워크의 부분에 액세스할 수 없는 경우에 수 응용 프로그램 또는 데이터에 액세스할 수 없습니다. 이 경우 제한 된 기능을 사용 하 여 대부분의 응용 프로그램을 실행 하는 재해 복구 전략을 디자인 하는 것이 좋습니다.

옵션을 기능 제한 없는 경우 나머지 옵션은 응용 프로그램 가동 중지 시간 이거나 대체 지역에 장애 조치 합니다.

기능 제한 시나리오:

- 응용 프로그램에 액세스할 수 없으면 해당 데이터는 Azure 네트워크가 중단 되어 캐시 된 데이터를 사용 하 여 제한 된 응용 프로그램 기능을 사용 하 여 로컬로 실행할 수 있습니다.
- 연결 복원 될 때까지 대체 위치에 데이터를 저장할 수 있습니다.

## <a name="dependent-service-failure"></a>종속 서비스 오류

각 종속 서비스에 대 한 응용 프로그램은 응답 하는 방법과 서비스 중단의 영향을 이해 해야 합니다. 여러 서비스에는 각 서비스를 독립적으로 평가 하는 것은 재해 복구 계획을 개선할 수 있도록 복원 력 및 가용성을 지 원하는 기능이 포함 됩니다. 예를 들어, Azure Event Hubs 지원 [장애 조치](/azure/event-hubs/event-hubs-geo-dr#setup-and-failover-flow) 보조 네임 스페이스에 있습니다.

## <a name="region-wide-service-disruptions"></a>지역 전체의 서비스 중단

많은 오류는 동일한 Azure 지역 내에서 관리할 수 있습니다. 그러나 지역 전체의 서비스 중단의 예기치 않은 이벤트가, 데이터의 로컬 중복 복사본을 사용할 수 없습니다. 지역에서 복제를 사용 하도록 설정한 경우 blob 및 다른 지역에는 테이블의 세 개의 추가 복사본이 설정 됩니다. Microsoft 손실 된 지역을 선언 하는 경우 Azure 보조 지역에 DNS 항목을 모두 다시 매핑합니다.

> [!NOTE]
> 이 프로세스는 지역 전체의 서비스 중단에 대해서만 발생 하 고 컨트롤 내에 있지 않습니다. 더 나은 RPO 및 RTO를 달성하기 위해 [Azure Site Recovery](/azure/site-recovery/)를 사용하는 것이 좋습니다. 허용 가능한 가동 중단 이란 무엇 이며 복제 된 Vm에 장애 조치 하는 경우 결정 Site Recovery를 사용 합니다.

지역 전체의 서비스 중단에 대 한 응답에 배포 하 고 재해 복구 계획에 따라 달라 집니다.

- 복구 시간이 보장된 필요로 하지 않는 중요 하지 않은 응용 프로그램에 대 한 비용 제어 전략을으로 다른 지역으로 다시 배포 하는 것이 해야 합니다.
- 응용 프로그램의 배포 된 역할을 사용 하 여 다른 지역에서 호스트 되는 지역에 걸쳐 트래픽을 분산 하지 않습니다 (*활성/수동 배포*), 대체 지역에서 보조 호스 티 드 서비스를 전환 합니다.
- 대규모 보조 배포를 다른 지역에 있는 응용 프로그램 (*활성/활성 배포*), 해당 지역에 트래픽을 라우팅합니다.

지역 전체의 서비스 중단 으로부터 복구 하는 방법에 대 한 자세한 내용은 참조 하세요 [지역 전체의 서비스 중단 으로부터 복구](../resiliency/recovery-loss-azure-region.md)합니다.

### <a name="vm-recovery"></a>VM 복구

중요 한 앱에 대 한 지역 전체의 서비스 중단 발생 시 Vm을 복구 계획 합니다.

- 응용 프로그램 일치 하는 지역 간 백업을 만들려면 Azure Backup 또는 다른 백업 메서드를 사용 합니다. (백업 자격 증명 모음의 복제는 생성 시 구성 되어야 합니다.)
- 한 번의 클릭 응용 프로그램 장애 조치 및 장애 조치 테스트에 대 한 지역에서 복제를 Site Recovery를 사용 합니다.
- Traffic Manager를 사용 하 여 다른 지역에 사용자 트래픽을 장애 조치를 자동화할 수 있습니다.

자세한 내용은 참조 하세요 [Virtual machines는 지역 전체의 서비스 중단에서 복구](../resiliency/recovery-loss-azure-region.md#virtual-machines)합니다.

### <a name="storage-recovery"></a>저장소 복구

지역 전체의 서비스 중단 발생 시 저장소를 보호 합니다.

- 지역 중복 저장소를 사용 합니다.
- 저장소를 지리적으로 복제 하는 위치 인지 알고 있어야 합니다. 이 저장소를 사용 하 여 지역 선호도 필요로 하는 데이터의 다른 인스턴스를 배포 하는 위치에 영향을 줍니다.
- 장애 조치 후 일관성에 대 한 데이터를 확인 하 고 필요한 경우 복원 된 백업에서 합니다.

자세한 내용은 참조 하세요 [RA-GRS를 사용 하 여 항상 사용 가능한 응용 프로그램을 디자인](/azure/storage/common/storage-designing-ha-apps-with-ragrs)합니다.

### <a name="sql-database-and-sql-server"></a>SQL Database 및 SQL Server

Azure SQL Database는 두 가지 유형의 복구를 제공합니다.

- 지역 복원을 사용 하 여 다른 지역에 백업 복사본에서 데이터베이스를 복원 합니다. 자세한 내용은 [자동화 된 데이터베이스 백업을 사용 하 여 Azure SQL database 복구](/azure/sql-database/sql-database-recovery-using-backups)합니다.
- 활성 지역 복제를 사용 하 여 보조 데이터베이스로 장애 조치 합니다. 자세한 내용은 [만들기 및 활성 지역 복제를 사용 하 여](/azure/sql-database/sql-database-active-geo-replication)입니다.

Vm에서 실행 중인 SQL Server를 참조 하세요 [Azure Virtual Machines에서 SQL Server에 대 한 고가용성 및 재해 복구](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr/)합니다.

## <a name="service-specific-guidance"></a>서비스 관련 지침

다음 문서는 특정 Azure 서비스에 대 한 재해 복구를 설명 합니다.

| 서비스                       | 문서                                                                                                                                                                                       |
|-------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Azure Database for MySQL      | [Azure Database for MySQL의 비즈니스 연속성 개요](/azure/mysql/concepts-business-continuity)                                                  |
| Azure Database for PostgreSQL | [Azure Database for PostgreSQL의 비즈니스 연속성 개요](/azure/postgresql/concepts-business-continuity)                                        |
| Azure Cloud Services          | [Azure Cloud Services에 영향을 주는 Azure 서비스 중단 발생 시 수행할 작업](/azure/cloud-services/cloud-services-disaster-recovery-guidance) |
| Cosmos DB                     | [Azure Cosmos DB를 사용 하 여 고가용성](/azure/cosmos-db/high-availability)                                                                                |
| Azure Key Vault               | [Azure Key Vault 가용성 및 중복성](/azure/key-vault/key-vault-disaster-recovery-guidance)                                                        |
| Azure Storage                 | [Azure Storage에서 재해 복구 및 스토리지 계정 장애 조치(Failover)(미리 보기)](/azure/storage/common/storage-disaster-recovery-guidance)                       |
| SQL Database                  | [Azure SQL Database 또는 장애 조치를 보조 지역으로 복원](/azure/sql-database/sql-database-disaster-recovery)                                       |
| Virtual Machines              | [Azure 클라우드 시 Azure 서비스 중단에 영향](/azure/cloud-services/cloud-services-disaster-recovery-guidance)               |
| Azure Virtual Network         | [Virtual Network – 비즈니스 연속성](/azure/virtual-network/virtual-network-disaster-recovery-guidance)                                                  |

## <a name="next-steps"></a>다음 단계

- [데이터 손상 또는 우발적 삭제 로부터 복구](../resiliency/recovery-data-corruption.md)
- [지역 전체의 서비스 중단 으로부터 복구](../resiliency/recovery-loss-azure-region.md)