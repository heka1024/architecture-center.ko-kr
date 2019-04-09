---
title: Azure Kubernetes Service에 대한 마이크로서비스 참조 구현
description: 이 참조 구현은 마이크로서비스 아키텍처의 모범 사례를 보여줍니다.
author: MikeWasson
ms.date: 02/26/2019
ms.topic: guide
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: microservices
ms.openlocfilehash: 17e275e5b5f45233f7467192402cb28fce35c57b
ms.sourcegitcommit: 0a8a60d782facc294f7f78ec0e9033e3ee16bf4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068908"
---
# <a name="designing-a-microservices-architecture"></a>마이크로서비스 아키텍처 디자인

마이크로 서비스는 복원력이 있고, 확장성이 뛰어나며, 독립적으로 배포할 수 있고, 신속하게 진화할 수 있는 클라우드 애플리케이션을 구축하는 데 널리 사용되는 아키텍처 스타일이 되었습니다. 하지만, 마이크로 서비스가 단순히 업계 유행어로만 남지 않으려면 애플리케이션을 설계하고 구축하는 데 있어 다른 접근 방식이 필요합니다.

이어지는 본 문서에서는 Azure에서 마이크로 서비스 아키텍처를 구축하고 실행하는 방법을 살펴봅니다. 다룰 주제는 다음과 같습니다.

- [서비스 간 통신](./interservice-communication.md)
- [API 디자인](./api-design.md)
- [API 게이트웨이](./gateway.md)
- [데이터 고려 사항](./data-considerations.md)
- [디자인 패턴](./patterns.md)

## <a name="prerequisites"></a>필수 조건

위의 문서를 읽기 전에, 다음 문서를 먼저 읽어봅니다.

- [마이크로서비스 아키텍처 소개](../introduction.md) 마이크로서비스의 장점 및 과제가 무엇이며 이 아키텍처 스타일을 언제 사용하는지 이해합니다.
- [도메인 분석을 사용하여 마이크로서비스 모델링](../model/domain-analysis.md) 마이크로서비스를 모델링하는 도메인 기반 접근 방식을 알아봅니다.

## <a name="reference-implementation"></a>참조 구현

마이크로서비스 아키텍처의 모범 사례를 보여드리기 위해 드론 배달 애플리케이션이라는 참조 구현을 만들었습니다. 이 구현은 AKS(Azure Kubernetes Service)를 사용하여 Kubernetes에서 실행됩니다. 참조 구현은 [GitHub][drone-ri]에서 찾을 수 있습니다.

![드론 배달 애플리케이션의 아키텍처](../images/drone-delivery.png)

## <a name="scenario"></a>시나리오

Fabrikam, Inc.라는 회사가 드론 배달 서비스를 시작합니다. 이 회사는 다수의 드론 항공기를 관리합니다. 기업은 서비스에 등록하고 사용자는 배달을 위해 드론이 상품을 픽업하도록 요청할 수 있습니다. 사용자가 픽업을 예약하면 백 엔드 시스템이 드론을 할당하고 사용자에게 예상 배달 시간을 알립니다. 배달이 진행되는 동안 지속적으로 업데이트되는 ETA를 통해 고객은 드론의 위치를 추적할 수 있습니다.

이 시나리오에는 상당히 복잡한 도메인이 포함됩니다. 비즈니스 우려 사항으로는 드론 예약, 패키지 추적, 사용자 계정 관리, 기록 데이터 저장 및 분석 등이 있습니다. 또한 Fabrikam은 빠르게 시장에 진입 및 반복하며 새로운 기능을 추가하고자 합니다. 애플리케이션은 SLO(서비스 수준 목표)가 높은 클라우드 규모에서 작동해야 합니다. 또한 시스템의 각 부분에서 데이터 저장소 및 쿼리에 대한 요구 사항이 완전히 달라야 합니다. 이러한 모든 사항을 고려한 끝에 Fabrikam은 드론 배달 애플리케이션에 마이크로 서비스 아키텍처를 사용하기로 선택합니다.

> [!NOTE]
> 마이크로 서비스 아키텍처와 다른 아키텍처 스타일 중에서 선택을 해야 하는 경우 도움이 필요하면 [Azure 애플리케이션 아키텍처 가이드](../../guide/index.md)를 참조하세요.

제공된 참조 구현에는 [AKS(Azure Kubernetes Service)](/azure/aks/)와 Kubernetes가 사용됩니다. 단, [Azure Service Fabric](/azure/service-fabric/)을 비롯한 모든 컨테이너 오케스트레이터에는 높은 수준의 아키텍처 결정과 도전 과제가 많이 적용됩니다.

<!-- links -->

[drone-ri]: https://github.com/mspnp/microservices-reference-implementation/tree/v0.1.0-orig

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [컴퓨팅 옵션 선택](./compute-options.md)