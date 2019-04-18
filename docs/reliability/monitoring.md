---
title: Azure의 안정성에 대 한 응용 프로그램 상태를 모니터링합니다.
description: Azure에서 응용 프로그램 안정성을 개선 하기 위해 모니터링을 사용 하는 방법
author: MikeWasson
ms.date: 04/10/2019
ms.topic: article
ms.service: architecture-center
ms.subservice: cloud-design-principles
ms.openlocfilehash: 46f9f3fb623a2facf4b9f9c6ec57376ae59e41dc
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59646672"
---
# <a name="monitoring-application-health-for-reliability-in-azure"></a>Azure의 안정성에 대 한 응용 프로그램 상태를 모니터링합니다.

모니터링 및 진단은 복원력에 매우 중요한 요소입니다. 오류가 발생 하는 경우 알아야 할 *하는* 실패 한 것 *때* 실패 &mdash; 및 *이유*합니다.

*모니터링* 동일 아닙니다 *실패 감지*합니다. 응용 프로그램 임시를 검색할 수 있습니다 예를 들어, 오류 및 가동 중지 시간을 방지 하는 다시 시도 합니다. 하지만 전반적인 응용 프로그램의 상태를 가져오려는 오류 비율을 모니터링할 수 있도록 다시 시도 작업을도 기록해 야 합니다.

별도 네 단계를 사용 하 여 파이프라인을 모니터링 및 진단 프로세스를 생각해 보십시오. 계측, 컬렉션 및 저장소, 분석 및 진단 및 시각화 및 경고

## <a name="instrumentation"></a>계측

계측 키 이므로는 응용 프로그램을 직접 모니터링 하는 데 적합 하지입니다. 대규모 분산된 시스템은 추가 되 고 시간이 지남에 따라 제거 하는 가상 머신 (Vm)에서 실행할 수 있습니다. 마찬가지로, 클라우드 응용 프로그램을 다양 한 데이터 저장소를 사용할 수 있습니다 하 고 단일 사용자 작업이 여러 하위 시스템에 걸쳐 있을 수 있습니다.

풍부한 계측을 제공 합니다.

- 오류에 대 한는 가능성이 있지만 아직 처리 하지 않은 상황을 완화 된 원인을 확인 하려면 충분 한 데이터를 제공 합니다. 발생 한 시스템을 계속 사용할 수 있는지 확인 합니다.
- 이미 발생 한 오류에 대 한 응용 프로그램 사용자에 게 해당 오류 메시지를 반환 해야 하지만 기능이 축소 된 상태 라도 실행을 계속 하려고 시도해 야 합니다.

모니터링 시스템 응용 프로그램을 효율적으로 복원할 수 있습니다 하 고 디자이너와 개발자가 필요한 경우 시스템 되풀이 상황을 방지 하기에 수정할 수 있도록 포괄적인 정보를 캡처해야 합니다.

모니터링에 대 한 원시 데이터는 다양 한를 비롯 한 원본에서에서 가져올 수 있습니다.

- 생성 된 것과 같은 응용 프로그램 로그를 [Azure Application Insights](/azure/azure-monitor/app/app-insights-overview) 서비스입니다.
- 운영 체제 성능 메트릭에 수집한 [Azure 모니터링 에이전트](/azure/azure-monitor/platform/agents-overview)합니다.
- [Azure 리소스](/azure/azure-monitor/platform/metrics-supported), Azure Monitor에서 수집한 메트릭을 포함 합니다.
- [Azure Service Health](/azure/service-health/service-health-overview), 활성 이벤트를 추적할 수 있도록 대시보드를 제공 합니다.
- [Azure AD 로그](/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics) Azure 플랫폼에 기본 제공 합니다.

대부분의 Azure 서비스에는 메트릭 및 진단 분석 하 고 문제의 원인을 결정 하도록 구성할 수는 있습니다. 자세한 내용은 참조 하세요 [Azure Monitor에서 수집한 모니터링 데이터](/azure/azure-monitor/platform/data-collection)입니다.

## <a name="collection-and-storage"></a>수집 및 저장

다양 한 위치 및를 비롯 한 형식에서 원시 계측 데이터를 보관할 수 있습니다.

- 응용 프로그램 추적 로그
- IIS 로그
- 성능 카운터

이러한 서로 다른 원본은 수집, 통합 되며 azure에서 Application Insights, Azure Monitor 메트릭, 서비스 상태, 저장소 계정 및 Azure Log Analytics와 같은 신뢰할 수 있는 데이터 저장소에 배치 합니다.

## <a name="analysis-and-diagnosis"></a>분석 및 진단

응용 프로그램의 상태를 전체적으로 볼을 얻고 문제를 해결 하려면 이러한 데이터 저장소에 통합 하는 데이터를 분석 합니다. 일반적으로 수 있습니다 [에 대 한 검색 하 고 분석](/azure/azure-monitor/log-query/log-query-overview) Application Insights 및 Log Analytics Kusto 쿼리 또는 뷰를 미리 구성 된 그래프를 사용 하 여 사용 하 여 데이터 [관리 솔루션](/azure/azure-monitor/insights/solutions-inventory)합니다. Azure Advisor에 중점을 둔 권장 사항 보기를 사용 하거나 [복원 력](/azure/advisor/advisor-high-availability-recommendations) 하 고 [성능](/azure/advisor/advisor-performance-recommendations)합니다.

## <a name="visualization-and-alerts"></a>시각화 및 경고

형식으로 손쉽게 대시보드 또는 전자 메일 경고와 같은 문제 또는 추세를 신속 하 게 확인 하는 연산자에 대 한 원격 분석 데이터를 제공 합니다.

사용 하 여 응용 프로그램 상태 전체 스택 살펴볼 [Azure 대시보드](/azure/azure-portal/azure-portal-dashboards) 그래프에서 Application Insights, Log Analytics, Azure Monitor 메트릭 및 서비스 상태 모니터링의 통합된 보기를 만듭니다. 사용 하 여 [Azure Monitor 경고](/azure/azure-monitor/platform/alerts-overview) Log Analytics 및 Application Insights에서 로그 알림을 Azure Monitor 메트릭, 서비스 상태 및 리소스 상태에 만듭니다.

모니터링 및 진단 하는 방법에 대 한 자세한 내용은 참조 하세요. [모니터링 및 진단](../best-practices/monitoring.md)합니다.

## <a name="health-probes-and-check-functions"></a>상태 프로브 및 검사 함수

응용 프로그램의 성능과 상태 시간이 지남에 따라 저하 될 수 있습니다 하 고 응용 프로그램이 실패할 때까지 저하 눈에 띄는 되지 않을 수 있습니다.

프로브를 구현 또는 함수를 확인 하 고 응용 프로그램 외부에서 정기적으로 실행 합니다. 이러한 검사는 전체적으로 응용 프로그램, 응용 프로그램의 개별 부분, 응용 프로그램에서는 특정 서비스에 대 한 또는 별도 구성 요소에 대 한 응답 시간 측정 같이 간단할 수 있습니다.

함수는 올바른 결과 생성, 대기 시간을 측정 하 고 가용성을 확인 하며 시스템에서 정보를 추출 하도록 프로세스를 실행할 수를 확인 합니다.

## <a name="long-running-workflow-failures"></a>장기 실행 워크플로 실패

장기 실행 워크플로 종종 각각은 독립적 이어야 하는 여러 단계를 포함 합니다.

롤백할 수 해야 전체 워크플로 여러 보충 트랜잭션을 실행 해야 합니다 또는 가능성을 최소화 하기 위해 장기 실행 프로세스의 진행률을 추적 합니다.

>[!TIP]
> 와 같은 패턴을 구현 하 여 장기 실행 워크플로의 진행 상태를 관리 및 모니터링 [Scheduler 에이전트 감독자](../patterns/scheduler-agent-supervisor.md)합니다.

## <a name="application-logs"></a>애플리케이션 로그

애플리케이션 로그는 진단 데이터의 중요한 소스입니다. 분석 하기 위해 필요할 때 대부분을 응용 프로그램 로깅에 대 한 모범 사례를 따릅니다.

### <a name="log-data-in-the-production-environment"></a>프로덕션 환경에서 로그 데이터

충분 한 정보는 프로덕션 상태에서 문제의 원인을 진단할 수 있다면 응용 프로그램이 프로덕션 환경에서 실행 되는 동안 강력한 원격 분석 데이터를 캡처하십시오.

### <a name="log-events-at-service-boundaries"></a>서비스 경계에서 이벤트 로그

서비스 경계 너머로 흐르는 상관 관계 ID를 포함합니다. 실패 트랜잭션이 여러 서비스 및 그 중 하나를 통해 흐르는 상관 관계 ID를 사용 하면 응용 프로그램 및 위험을 지적 트랜잭션 실패 이유에서 요청을 추적 합니다.

### <a name="use-semantic-structured-logging"></a>의미 체계 (구조적된) 로깅 사용

구조적된 로그를 사용 하 여 소비 및는 클라우드 규모에서 특히 중요 로그 데이터에 대 한 분석을 자동화 하려면 쉽습니다. 일반적으로 Log Analytics 작업 영역에서 아닌 저장소 계정에 Azure 리소스 메트릭 및 진단 데이터를 저장 하는 것이 좋습니다. 이러한 방식으로 신속 하 고 구조화 된 형식에서 원하는 데이터를 가져오기 위해 Kusto 쿼리를 사용할 수 있습니다. 또한 Azure Monitor Api 및 Azure Log Analytics Api를 사용할 수 있습니다.

### <a name="use-asynchronous-logging"></a>비동기 로깅을 사용합니다

경우에 따라 동기 로깅 작업 로그 기록 될 백업에 대 한 요청을 일으키는 응용 프로그램 코드를 차단 합니다. 응용 프로그램 로깅 하는 동안 가용성을 유지 하기 위해 비동기 로깅을 사용 합니다.

### <a name="separate-application-logging-from-auditing"></a>감사에서 별도 응용 프로그램 로깅

감사 레코드는 일반적으로 규정 준수 또는 규제 요구 사항에 대 한 유지 관리 및 완료 해야 합니다. 트랜잭션 삭제를 방지 하려면 별도로 진단 로그에서 감사 로그를 유지 합니다.

## <a name="remote-call-statistics"></a>원격 호출 통계

추적 및 보고 원격 실시간에서 통계를 호출 하 고 운영 팀은 응용 프로그램의 상태를 즉시 뷰를 포함 하므로이 정보를 검토 하는 쉬운 방법을 제공 합니다. 대기 시간, 처리량 및 99 및 95 백분위 수의 오류와 같은 원격 호출 메트릭을 요약 되어 있습니다.

## <a name="transient-exceptions-and-retries"></a>일시적 예외 및 다시 시도

문제 또는 응용 프로그램의 재시도 논리에서 오류를 파악 하는 시간이 지남에 따라 일시적 예외 및 재시도 횟수를 추적 합니다. 서비스에 문제가 이며 못할 시간이 지남에 따라 예외가 증가 추세를 나타낼 수 있습니다. 자세한 내용은 [서비스 관련 재시도 지침](../best-practices/retry-service-specific.md)을 참조하세요.

## <a name="early-warning-system"></a>조기 경고 시스템

사전 개입이 필요할 수 있는 경고 기호에 대 한 응용 프로그램을 모니터링 합니다. 응용 프로그램 및 해당 종속성의 전반적인 상태를 평가 하는 도구를 사용 하면 경우 시스템 또는 해당 구성 요소가 갑자기 사용할 수 없게 신속 하 게 인식에 도움이 됩니다. 조기 경고 시스템을 구현 하려면 사용 합니다.

1. 일시적 예외 및 원격 호출 대기 시간 등의 응용 프로그램의 상태, 핵심 성과 지표를 식별 합니다.
1. 중요성이 서 복구 응답이 필요 하기 전에 문제를 식별 하는 수준 임계값을 설정 합니다.
1. 임계값에 도달할 때 작업에 경고를 보냅니다.

것이 좋습니다 [Microsoft System Center 2016](https://www.microsoft.com/cloud-platform/system-center) 또는 타사 도구가 모니터링 기능을 제공 합니다. 대부분의 모니터링 솔루션은 주요 성능 카운터 및 서비스 가용성을 추적합니다. [Azure 리소스 상태](/azure/service-health/resource-health-checks-resource-types) 몇 가지 기본 제공 상태를 Azure 서비스에 부담을 진단 하는 데 도움이 되는 상태 검사를 제공 합니다.

## <a name="subscription-and-service-limitations"></a>구독 및 서비스 제한 사항

Azure 구독 코어, 저장소 계정 리소스 그룹의 번호와 같은 특정 리소스 종류에 제한이 있습니다. Azure 구독 제한에 대해 응용 프로그램을 실행 하는 하지 않습니다을 보장 하려면 해당 한도 및 할당량에 근접 하는 서비스에 대 한 폴링하는 경고를 만듭니다.

경고를 사용 하 여 다음과 같은 구독 제한이 해결 합니다.

### <a name="individual-services"></a>개별 서비스

개별 Azure 서비스에 다른 메트릭 및 연결, 초당 요청 수, 저장소 및 처리량에 사용량 제한이 있습니다. 응용 프로그램 서비스 제한 및 가능한 가동 중지 시간이 발생 이러한 제한 초과 하는 리소스를 사용 하려고 하면 실패 합니다.

특정 서비스 및 응용 프로그램 요구 사항에 따라 있습니다 수 종종 이러한 한도 넘지 (예를 들어 다른 가격 책정 계층 선택) 강화 또는 규모 확장 (새 인스턴스 추가).

### <a name="azure-storage-scalability-and-performance-targets"></a>Azure storage 확장성 및 성능 목표

Azure 구독 당 storage 계정의 최대 수를 허용합니다. 응용 프로그램에서 현재 구독에서 사용할 수 있는 것 보다 더 많은 저장소 계정에 필요한 경우 추가 저장소 계정을 사용 하 여 새 구독을 만듭니다. 자세한 내용은 [Azure 구독 및 서비스 제한, 할당량 및 제약 조건](/azure/azure-subscription-service-limits/#storage-limits)을 참조하세요.

Azure storage 확장성 및 성능 목표를 초과 하면 응용 프로그램 저장소 제한이 발생 합니다. 자세한 내용은 [Azure Storage 확장성 및 성능 목표](/azure/storage/storage-scalability-targets/)를 참조하세요.

### <a name="scalability-targets-for-virtual-machine-disks"></a>가상 머신 디스크에 대한 확장성 목표

Azure 인프라 서비스 제공 (IaaS) VM으로 다양 한 VM 크기 및 저장소 계정의 형식을 비롯 한 여러 요인에 따라 데이터 디스크 연결을 지원 합니다. 애플리케이션이 가상 머신 디스크에 대한 확장성 목표를 초과하는 경우 추가 저장소 계정을 프로비전하고 거기서 가상 머신 디스크를 만듭니다. 자세한 내용은 [Azure Storage 확장성 및 성능 목표](/azure/storage/storage-scalability-targets/#scalability-targets-for-virtual-machine-disks)를 참조하세요.

### <a name="vm-size"></a>VM 크기

실제 CPU, 메모리, 디스크 및 Vm의 I/O 적절 한 VM 크기의 제한 하는 경우 응용 프로그램 용량 문제가 발생할 수 있습니다. 문제를 해결 하려면 VM 크기를 늘립니다. 설명 하는 VM 크기 [Azure에서 가상 머신 크기](/azure/virtual-machines/virtual-machines-windows-sizes/?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)합니다.

워크 로드가 시간이 지남에 따라 변동 되는 경우 가상 머신 확장을 사용 하 여 자동으로 VM 인스턴스 수를 조정 하도록 설정 하는 것이 좋습니다. 그렇지 않은 경우 수동으로 늘리거나 Vm 수를 줄이려면 해야 합니다. 자세한 내용은 참조는 [virtual machine scale sets 개요](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview/)합니다.

### <a name="azure-sql-database"></a>Azure SQL Database

Azure SQL Database 계층을 응용 프로그램의 데이터베이스 트랜잭션 단위 (DTU) 요구 사항을 처리 하기에 적합 하지 않으면 데이터 사용이 제한 됩니다. 올바른 서비스 계획 선택에 대 한 자세한 내용은 참조 하세요. [모델을 구매 하는 Azure SQL Database](/azure/sql-database/sql-database-service-tiers/)합니다.

## <a name="third-party-services"></a>타사 서비스

응용 프로그램이 타사 서비스에 의존 하는 경우 이러한 서비스가 실패할 수 하는 방법 및 적용 오류는 응용 프로그램에 주지를 식별 합니다.

제 3 자 서비스 모니터링 및 진단 포함 되지 않을 수 있습니다. 이러한 서비스에 대 한 호출을 로그인 하 고 응용 프로그램의 상태 및 고유 식별자를 사용 하 여 진단 로깅을 사용 하 여 해당 상관 관계를 지정 합니다. 모니터링 및 진단에 대 한 검증 된 사례에 대 한 자세한 내용은 참조 하세요. [모니터링 및 진단 지침](../best-practices/monitoring.md)합니다.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [재해 복구에 대 한 계획](./disaster-recovery.md)
