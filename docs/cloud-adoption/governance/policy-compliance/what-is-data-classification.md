---
title: 'CAF: 데이터 분류란'
description: 데이터 분류란?
author: BrianBlanchard
ms.date: 2/11/2019
ms.openlocfilehash: 07268e7242d92ac2581bf28b378a3c43d166620c
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901628"
---
<!-- markdownlint-disable MD026 -->

# <a name="what-is-data-classification"></a>데이터 분류란 무엇인가요?

이것은 데이터 분류라는 일반적인 주제에 대한 소개 문서입니다. 데이터 분류는 모든 거버넌스에서 매우 일반적인 시작점입니다.

## <a name="business-risks-and-governance"></a>비즈니스 위험과 거버넌스

대부분의 조직에서 거버넌스에 투자하는 주된 이유는 다음 세 가지 비즈니스 위험을 줄이는 데 있습니다.

* 데이터 침해와 관련된 책임
* 가동 중단으로 인한 비즈니스 중단
* 계획되지 않거나 예기치 않은 지출

이러한 세 가지 비즈니스 위험에 대한 여러 변형이 있습니다. 그러나 위 세 가지가 가장 일반적입니다.

## <a name="understand-then-mitigate"></a>위험 파악 후 완화

위험을 완화하기 위해서는 먼저 위험을 파악해야 합니다. 데이터 침해 책임의 경우 데이터 분류를 이해하는 것으로 시작합니다. 데이터 분류는 메타데이터 특성을 디지털 자산의 모든 자산과 연관시키는 프로세스로, 해당 자산과 관련된 데이터 형식을 식별합니다.

Microsoft는 클라우드로 마이그레이션 또는 배포하기 위한 잠재적인 대안으로 식별된 모든 자산을 데이터 분류, 업무상 중요도 및 청구 책임을 기록하기 위해 메타데이터로 문서화해야 한다고 제안합니다. 이 세 가지 분류 기준은 위험을 파악하고 완화하는 데 큰 도움이 될 수 있습니다.

## <a name="microsofts-data-classification"></a>Microsoft의 데이터 분류

다음은 Microsoft에서 사용하는 분류 목록입니다. 업계 또는 기존 보안 요구 사항에 따라 데이터 분류 표준이 이미 조직 내에 있을 수 있습니다. 표준이 없으면 이 샘플 분류를 사용하여 디지털 자산 및 위험 프로필을 보다 잘 이해할 수 있습니다.  

* **업무 외:** Microsoft에 속하지 않는 개인 생활 데이터
* **공용:** 공개 사용을 위해 자유롭게 제공 및 승인된 비즈니스 데이터
* **일반:** 대중을 대상으로 하지 않은 비즈니스 데이터
* **기밀:** 지나치게 공유할 경우 Microsoft에 피해를 야기할 수 있는 비즈니스 데이터
* **극비:** 지나치게 공유할 경우 Microsoft에 상당한 피해를 야기할 수 있는 비즈니스 데이터

## <a name="tagging-data-classification-in-azure"></a>Azure에서 데이터 분류 태그 지정

모든 클라우드 공급자는 모든 자산에 대한 메타데이터를 기록하는 메커니즘을 제공해야 합니다. 메타데이터는 클라우드에서 자산을 관리하는 데 중요합니다. Azure의 경우 리소스 태그는 메타데이터 스토리지에 대해 권장되는 방식입니다. Azure에서 리소스 태그 지정에 대한 자세한 내용은 [태그를 사용하여 Azure 리소스 구성](/azure/azure-resource-manager/resource-group-using-tags) 문서를 참조하세요.

## <a name="next-steps"></a>다음 단계

실행 가능한 거버넌스 경험 중 하나에 데이터 분류를 적용합니다.

> [!div class="nextstepaction"]
> [실행 가능한 거버넌스 경험 시작](../journeys/overview.md)
