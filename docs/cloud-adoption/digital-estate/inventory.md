---
title: 'CAF: 디지털 자산에 대한 인벤토리 데이터 수집'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 디지털 자산에 대한 인벤토리를 수집하는 방법입니다.
author: BrianBlanchard
ms.date: 12/10/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.openlocfilehash: 0c68ff1e5ff51395698d37fb9b59c7a76c9479b7
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55897255"
---
# <a name="gather-inventory-data-for-a-digital-estate"></a>디지털 자산에 대한 인벤토리 데이터 수집

인벤토리 개발은 [디지털 자산 계획](overview.md)의 첫 번째 단계입니다. 이 프로세스에서는 나중에 분석하고 합리적으로 할당하기 위해 특정 비즈니스 기능을 지원하는 IT 자산 목록이 수집됩니다. 이 문서에서는 상향식 분석 방법이 요구 사항 계획에 가장 적절하다고 가정합니다. 자세한 내용은 [디지털 자산 계획 방법](./approach.md)을 참조하세요.

## <a name="take-inventory-of-a-digital-estate"></a>디지털 자산의 인벤토리 가져오기

디지털 자산을 지원하는 인벤토리는 원하는 디지털 전환 및 해당 전환 과정에 따라 달라집니다.

- **운영 전환**. 운영 전환 중에는 모든 VM 및 서버의 중앙 집중식 목록을 만들 수 있는 검사 도구에서 인벤토리를 수집하는 것이 좋은 경우가 많습니다. 일부 도구는 워크로드 맞춤을 정의하는 데 도움이 되는 네트워크 매핑 및 종속성도 만들 수 있습니다.

- **증분 전환**. 중분 전환을 위한 인벤토리는 고객과 함께 시작됩니다. 처음부터 끝까지 고객 환경을 매핑하는 작업부터 시작하는 것이 좋습니다. 애플리케이션, API, 데이터 및 기타 자산에 대한 맵을 맞추면 분석에 필요한 자세한 인벤토리가 생성됩니다.

- **파괴적 전환**. 파괴적 전환은 제품이나 서비스에 중점을 둡니다. 여기서는 인벤토리에 시장 개혁의 기회와 필요한 역량의 매핑이 포함됩니다.

## <a name="accuracy-and-completeness-of-an-inventory"></a>인벤토리의 정확도와 완성도

인벤토리는 첫 번째 반복에서 거의 완료되지 않습니다. 클라우드 전략 팀의 다양한 구성원이 이해관계자와 고급 사용자를 조율하여 인벤토리 유효성 검사를 수행하면 매우 좋습니다. 가능하면 네트워크 및 종속성 분석 같은 추가 도구를 사용하여 트래픽이 전송되지만 인벤토리에 없는 자산을 파악할 수 있습니다.

## <a name="next-steps"></a>다음 단계

인벤토리가 컴파일되고 유효성이 검사되면 합리화를 수행할 수 있습니다. 디지털 자산 계획의 다음 단계는 인벤토리 합리화입니다.

> [!div class="nextstepaction"]
> [디지털 자산 합리화](rationalize.md)