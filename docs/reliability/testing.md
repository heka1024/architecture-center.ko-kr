---
title: 복원 력 및 가용성을 위해 Azure 응용 프로그램 테스트
description: Azure에서 응용 프로그램의 안정성을 테스트 하는 방법
author: MikeWasson
ms.date: 04/10/2019
ms.topic: article
ms.service: architecture-center
ms.subservice: cloud-design-principles
ms.openlocfilehash: cf33a859540810279b1cb85be5a6fb2ebe4500dc
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59646641"
---
# <a name="testing-azure-applications-for-resiliency-and-availability"></a>복원 력 및 가용성을 위해 Azure 응용 프로그램 테스트

복원 력 테스트를 엔드-투-엔드 워크 로드를 일시적인 오류 조건에서 수행 하는 방법을 확인 해야 합니다.

가상 및 실제 사용자 데이터 모두를 사용 하 여 프로덕션에서 테스트를 실행 합니다. 테스트 및 프로덕션은 거의 동일 하므로 사용 하 여 프로덕션에서 응용 프로그램의 유효성을 검사 해야는 [청록색](https://martinfowler.com/bliki/BlueGreenDeployment.html) 하거나 [카나리아 배포](https://martinfowler.com/bliki/CanaryRelease.html)합니다. 이러한 방식으로 테스트 하는 실제 상황에서는 응용 프로그램이 완벽 하 게 배포 하는 경우 예상 대로 작동 하는지 확인할 수 있도록 합니다.

테스트 계획의 일환으로, 다음을 포함 합니다.

- 자동화 된 배포 전 테스트
- 오류 주입 테스트
- 최대 부하 테스트
- 재해 복구 테스트
- 타사 서비스 테스트

테스트는 반복적인 프로세스입니다. 애플리케이션을 테스트하고, 결과를 측정하고, 결과를 분석 및 해결하고, 다시 프로세스를 반복합니다.

## <a name="perform-fault-injection-testing"></a>오류 주입 테스트 수행

오류 주입 테스트 또는 실제 오류를 트리거하거나 시뮬레이션 하 여 시스템의 복원 력을 실패 하는 동안 확인 합니다. 다음은 오류를 유도 하기 위한 몇 가지 전략입니다.

- 가상 머신 (VM) 인스턴스를 종료 합니다.
- 프로세스가 충돌합니다.
- 인증서가 만료됩니다.
- 액세스 키가 변경됩니다.
- 도메인 컨트롤러에서 DNS 서비스를 중단합니다.
- RAM 또는 스레드 수 같은 가용 시스템 리소스를 제한합니다.
- 디스크를 탑재 해제합니다.
- VM을 다시 배포합니다.

테스트 계획 일반적인 오류 시나리오 외에도 디자인 단계에서 식별 가능한 오류 지점을 통합 해야 합니다.

- 프로덕션 최대한 가까운 환경에서 응용 프로그램을 테스트 합니다.
- 조합 하 여 오류를 테스트 합니다.
- 복구 시간을 측정 하 고 비즈니스 요구 사항을 충족 해야 합니다.
- 확인 오류 계단식으로 배열 되지 한 격리 된 방식으로 처리 됩니다.

오류 시나리오에 대 한 자세한 내용은 참조 하세요. [Azure 응용 프로그램에 대 한 오류 및 재해 복구](./disaster-recovery.md)합니다.

## <a name="test-under-peak-loads"></a>최대 부하 테스트

부하 테스트 하는 것은 중요 서비스 제한 또는 부하가 과부하가 백 엔드 데이터베이스와 같은 때에 발생 하는 오류를 식별 합니다. 최대 부하 및 최대 부하를 프로덕션 데이터 또는 프로덕션 데이터와 최대한 가까운 가상 데이터를 사용 하 여 예상 된 증가 대 한 테스트 합니다. 목표는 응용 프로그램이 실제 조건에서 어떻게 동작 하는지 확인 하는 것입니다.

## <a name="conduct-disaster-recovery-drills"></a>재해 복구 훈련 수행

좋은 재해 복구 계획을 시작 하 고 작동 하는지 정기적으로 테스트 합니다.

Azure Virtual Machines에 대해 사용할 수 [Azure Site Recovery](/azure/site-recovery/azure-to-azure-quickstart/) 복제를 수행할 [재해 복구 훈련](/azure/site-recovery/azure-to-azure-tutorial-dr-drill/) 프로덕션 응용 프로그램 또는 진행 중인 복제에 영향을 주지 않고 합니다.

### <a name="failover-and-failback-testing"></a>장애 조치 및 장애 복구 테스트

장애 조치 및 장애 복구는 응용 프로그램의 종속 서비스 돌아와야 동기화 된 방식으로 재해 복구 하는 동안 확인을 테스트 합니다. 시스템 및 작업 변경에는 장애 조치 및 장애 복구 기능을 영향을 줄 수 있지만 주 시스템이 실패 하거나 과부하 될 때까지 영향이 검색 될 수 있습니다. 장애 조치 기능을 테스트 *하기 전에* 라이브 문제에 대 한 보정 해야 합니다. 또한 종속 서비스가 장애 조치 하 고 올바른 순서로 실패 해야 합니다.

사용 중인 경우 [Azure Site Recovery](/azure/site-recovery/) Vm을 복제, 재해 복구 훈련을 실행할 정기적으로 테스트 장애 조치 복제 전략의 유효성을 검사를 수행 하 여 합니다. 진행 중인 VM 복제 또는 프로덕션 환경에는 테스트 장애 조치를 적용 되지 않습니다. 자세한 내용은 [Azure로 재해 복구 훈련 실행](/azure/site-recovery/site-recovery-test-failover-to-azure)합니다.

### <a name="simulation-testing"></a>시뮬레이션 테스트

시뮬레이션 테스트에서는 소규모의 실제 상황을 만듭니다. 시뮬레이션에는 적절 하 게 처리 되지 않은 모든 문제를 강조 표시 및 복구 계획에 솔루션의 효과 보여 줍니다.

시뮬레이션 테스트를 수행 하면 모범 사례를 따릅니다.

- 실제 비즈니스를 중단 하지 않습니다 하지만 실제 상황을 찾기란 하는 방식으로 시뮬레이션을 수행 합니다.
- 시뮬레이션 된 시나리오는 완전히 제어할 수 있는지 확인 합니다. 복구 계획 실패 한 것 처럼 보이지만, 하는 경우 손상 없이 상황을 정상 상태로 복원할 수 있습니다.
- 시뮬레이션 연습을 수행할 시기와 방법에 대 한 관리를 알립니다. 계획에는 시간 프레임 및 시뮬레이션 중 영향을 받는 리소스를 자세히 설명 해야 합니다.

## <a name="test-health-probes-for-load-balancers-and-traffic-managers"></a>부하 분산 장치 및 traffic manager에 대 한 상태 프로브를 테스트 합니다.

구성 하 고 부하 분산 장치 및 traffic manager 상태 프로브를 테스트 합니다. 상태 끝점 시스템의 중요 한 부분을 확인 하 고 적절 하 게 응답을 확인 합니다.

- 에 대 한 [Azure Traffic Manager](/azure/traffic-manager/traffic-manager-overview/), 상태 프로브를 다른 지역으로 장애 조치할 지 여부를 결정 합니다. 상태 끝점에는 동일한 지역 내에서 배포 되는 모든 중요 한 종속성을 확인 해야 합니다.
- 에 대 한 [Azure Load Balancer](/azure/load-balancer/load-balancer-overview/), 상태 프로브 VM을 윤 번에서 제거할지 여부를 결정 합니다. 상태 끝점은 VM의 상태를 보고 해야 합니다. 다른 계층 또는 외부 서비스를 포함하지 마세요. 그렇지 않으면 VM 외부에서 발생하는 실패 때문에 부하 분산 장치가 VM을 윤번에서 제거하게 됩니다.

애플리케이션의 상태 모니터링 구현에 대한 지침은 [상태 엔드포인트 모니터링 패턴](../patterns/health-endpoint-monitoring.md)을 참조하세요.

## <a name="test-monitoring-systems"></a>모니터링 시스템 테스트

테스트 계획에서 시스템 모니터링이 포함 됩니다. 장애 조치 및 장애 복구에 대 한 자동화 된 시스템 모니터링 및 계측의 올바른 작동에 따라 달라 집니다. 시스템 상태 및 연산자 경고를 시각화 하는 대시보드도 정확 하 게 모니터링 및 계측에 따라 달라 집니다. 이러한 요소에 오류가 발생하고 중요한 정보를 누락되거나 부정확한 데이터가 보고된 경우 운영자는 시스템이 비정상 또는 실패임을 모를 수 있습니다.

## <a name="include-third-party-services-in-test-scenarios"></a>테스트 시나리오에서 타사 서비스를 포함 합니다.

응용 프로그램이 타사 서비스에 의존 하는 경우 위치를 식별 하 고 이러한 서비스가 실패할 수 있습니다 이러한 오류를 모르겠으면 방법과 응용 프로그램에 포함 됩니다. 항상 염두에서에 서비스 수준 계약 (SLA) 제 3 자 서비스 및 효과 대 한 재해 복구 계획을 가질 수 있습니다.

하지는 타사 서비스는 그 중에 호출을 기록 하 고 응용 프로그램의 상태를 사용 하 여 상관 관계 및 고유 식별자를 사용 하 여 로깅을 진단 되므로 모니터링 및 진단 기능을 제공할 수 있습니다. 모니터링 및 진단에 대 한 검증 된 사례에 대 한 자세한 내용은 참조 하세요. [모니터링 및 진단 지침](../best-practices/monitoring.md)합니다.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [복원 력 및 가용성에 대 한 배포](./deploy.md)
