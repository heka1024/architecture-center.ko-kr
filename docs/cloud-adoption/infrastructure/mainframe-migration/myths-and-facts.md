---
title: '메인프레임 마이그레이션: 허구 및 팩트'
description: 애플리케이션을 메인프레임 환경에서 Azure로 마이그레이션합니다. Azure는 현재 메인프레임에서 실행되는 시스템에 대한 검증되고 가용성 및 확장성이 뛰어난 인프라입니다.
author: njray
ms.date: 12/27/2018
ms.openlocfilehash: bcad01ec044d2d802b055e328a9496aae7b33311
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901475"
---
# <a name="mainframe-myths-and-facts"></a>메인프레임 허구 및 팩트

메인프레임은 컴퓨팅 역사에서 대단히 중요하며 특정 워크로드에 대해 실행 가능한 상태를 유지합니다. 메인프레임은 신뢰할 수 있는 강력한 환경을 만드는 오랜 운영 절차를 통해 검증된 플랫폼이라는 데 대부분 동의합니다. 소프트웨어는 MIPS(초당 백만 지침) 단위로 측정된 사용량에 따라 실행되며, 광범위한 사용량 보고서는 요금 상환에 사용할 수 있습니다.

메인프레임의 안정성, 가용성 및 처리 능력은 거의 허구적인 비율로 수행됐습니다. Azure에 가장 적합한 메인프레임 워크로드를 평가하려면 먼저 허구와 실제를 구분해야 합니다.

## <a name="myth-mainframes-never-go-down-and-have-a-minimum-of-five-9s-of-availability"></a>허구: 메인프레임은 아래로 이동하지 않고 최소 5개의 9초 가용성 있음

메인프레임 하드웨어 및 운영 체제는 안정적이고 안정적으로 표시됩니다. 그러나 실제로 유지 관리 및 재부팅(초기 프로그램 로드 또는 IPL이라고도 함)을 위해 가동 중지 시간을 예약해야 합니다. 이러한 작업을 고려하는 경우 메인프레임 솔루션은 종종 두세 개의 9초 가용성에 가까우며, 고성능 Intel 기반 서버의 솔루션에 해당합니다.

메인프레임은 또한 다른 모든 서버처럼 재해에 취약한 상태로 유지되며, 이러한 유형의 오류를 처리하려면 UPS(무정전 전원 공급 장치) 시스템이 필요합니다.

## <a name="myth-mainframes-have-limitless-scalability"></a>허구: 메인프레임의 무제한 확장성

메인프레임의 확장성은 CICS(고객 정보 제어 시스템) 같은 시스템 소프트웨어 용량 및 메인프레임 엔진 및 스토리지의 새 인스턴스 용량에 따라 달라집니다. 메인프레임을 사용하는 일부 대기업은 성능에 대해 해당 CICS를 사용자 지정했으며, 그렇지 않으면 사용 가능한 가장 큰 메인프레임의 역량을 초과할 수 있습니다.

## <a name="myth-intel-based-servers-are-not-as-powerful-as-mainframes"></a>허구: Intel 기반 서버는 메인프레임만큼 강력하지 않음

새 코어 집약의 Intel 기반 시스템에는 메인프레임만큼 많은 컴퓨팅 용량이 있습니다.

## <a name="myth-the-cloud-cannot-accommodate-mission-critical-applications-for-large-companies-such-as-financial-institutions"></a>허구: 클라우드는 금융 기관과 같은 대규모 회사의 중요 업무용 애플리케이션을 수용할 수 없음

클라우드 솔루션이 부족한 일부 격리된 인스턴스가 있을 수 있지만, 이는 일반적으로 애플리케이션 알고리즘을 배포할 수 없기 때문입니다. 이러한 몇 가지 예제는 규칙이 아닌 예외입니다.

## <a name="summary"></a>요약

그에 비해 Azure는 해당 메인프레임 기능을 훨씬 저렴한 비용으로 제공할 수 있는 다른 플랫폼을 제공합니다. 또한 클라우드의 구독 기반 및 사용량 기반 비용 모델의 TCO(총 소유 비용)는 메인프레임 컴퓨터보다 훨씬 저렴합니다.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [메인프레임에서 Azure로 전환](migration-strategies.md)
