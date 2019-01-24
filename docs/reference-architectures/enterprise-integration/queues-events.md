---
title: 메시지 큐 및 이벤트를 사용한 엔터프라이즈 통합
titleSuffix: Azure Reference Architectures
description: Azure Logic Apps, Azure API Management, Azure Service Bus 및 Azure Event Grid를 사용하여 엔터프라이즈 통합 패턴을 구현하는 권장 아키텍처를 제공합니다.
author: mattfarm
ms.reviewer: jonfan, estfan, LADocs
ms.topic: reference-architecture
ms.date: 12/03/2018
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: integration-services
ms.openlocfilehash: 4c9d2e201bcfc077990d746a1decd55ede2f220a
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54480680"
---
# <a name="enterprise-integration-on-azure-using-message-queues-and-events"></a>Azure에서 메시지 큐 및 이벤트를 사용하여 엔터프라이즈 통합

이 참조 아키텍처는 서비스를 분리하는 메시지 큐 및 이벤트로 엔터프라이즈 백 엔드 시스템을 통합하여 더 높은 확장성과 안정성을 제공합니다. 백 엔드 시스템에는 SaaS(Software as a Service) 시스템, Azure 서비스 및 기업의 기존 웹 서비스가 포함될 수 있습니다.

![큐 및 이벤트를 사용한 엔터프라이즈 통합에 대한 참조 아키텍처](./_images/enterprise-integration-queues-events.png)

## <a name="architecture"></a>아키텍처

여기에 표시된 아키텍처는 [기본 엔터프라이즈 통합][basic-enterprise-integration]에 표시된 더 단순한 아키첵처를 기반으로 합니다. 이 아키텍처는 워크플로를 오케스트레이션하는 [Logic Apps][logic-apps] 및 API 카탈로그를 만드는 [API Management][apim]를 사용합니다.

이 버전의 아키텍처는 시스템의 안정성과 확장성을 높이는 데 도움이 되는 두 구성요소를 추가합니다.

- **[Azure Service Bus][service-bus]**. Service Bus는 안전하고 안정적인 메시지 브로커입니다.

- **[Azure Event Grid][event-grid]**. Event Grid는 이벤트 라우팅 서비스입니다. [게시/구독](../../patterns/publisher-subscriber.md)(pub/sub) 이벤트 모델을 사용합니다.

메시지 브로커를 사용하는 비동기 통신은 백 엔드 서비스에 대한 직접적인 비동기 호출보다 다양한 이점을 제공합니다.

- [큐 기반 부하 평준화 패턴](../../patterns/queue-based-load-leveling.md)을 사용하여 워크로드 급증을 처리하는 부하 평준화를 제공합니다.
- 여러 단계 또는 여러 애플리케이션과 관련된 장기 실행 워크플로의 진행률을 안정적으로 추적합니다.
- 애플리케이션을 분리하는 데 도움이 됩니다.
- 기존 메시지 기반 시스템과 통합됩니다.
- 백 엔드 시스템을 사용할 수 없을 때 작업을 큐에 추가할 수 있습니다.

Event Grid는 폴링 또는 예약된 작업에 의존하기보다는 시스템의 다양한 구성 요소가 발생하는 이벤트에 대응하도록 지원합니다. 메시지 큐와 마찬가지로 애플리케이션 및 서비스를 분리하는 데 도움이 됩니다. 애플리케이션 또는 서비스는 이벤트를 게시할 수 있으며, 관련 구독자에게 알림이 전송됩니다. 보낸 사람을 업데이트하지 않고 새 구독자를 추가할 수 있습니다.

여러 Azure 서비스는 Event Grid로 이벤트 보내기를 지원합니다. 예를 들어 논리 앱은 BLOB 저장소에 새 파일이 추가되면 이벤트를 수신 대기할 수 있습니다. 이 패턴은 파일 업데이트 또는 큐에 메시지 추가로 인해 일련의 프로세스가 시작될 때 이에 대응하는 워크플로를 구현합니다. 이 프로세스는 특정 순서대로 또는 병렬로 실행될 수 있습니다.

## <a name="recommendations"></a>권장 사항

[기본 엔터프라이즈 통합][basic-enterprise-integration]에 설명된 권장 사항이 이 아키텍처에 적용됩니다. 다음 권장 사항도 적용됩니다.

### <a name="service-bus"></a>Service Bus

Service Bus는 *끌어오기*와 *밀어넣기*라는 두 가지 제공 모드를 지원합니다. 끌어오기 모델에서는 수신기가 새 메시지를 지속적으로 폴링합니다. 폴링은 비효율적일 수 있습니다. 여러 큐가 각각 소량의 메시지를 수신하거나 메시지 수신 간격이 긴 경우에 특히 그렇습니다. 밀어넣기 모델에서는 새 메시지가 있을 때 Service Bus가 Event Grid를 통해 이벤트를 보냅니다. 수신기는 이벤트를 구독합니다. 이벤트가 트리거되면 수신기가 Service Bus에서 메시지의 다음 배치를 끌어옵니다.

Service Bus 메시지를 사용하는 논리 앱을 만들 때는 Event Grid 통합을 통해 밀어넣기 모델을 사용하는 것이 좋습니다. 논리 앱이 Service Bus를 폴링할 필요가 없으므로 보다 비용 효율적인 경우가 많습니다. 자세한 내용은 [Azure Service Bus-Event Grid 통합 개요](/azure/service-bus-messaging/service-bus-to-event-grid-integration-concept)를 참조하세요. 현재 Event Grid 알림을 위해서는 Service Bus [프리미엄 등급](https://azure.microsoft.com/pricing/details/service-bus/)이 필요합니다.

[PeekLock](/azure/service-bus-messaging/service-bus-messaging-overview#queues)를 사용하여 메시지 그룹에 액세스합니다. PeekLock을 사용하는 경우, 논리 앱은 메시지를 완료하거나 중단하기 전에 각 메시지의 유효성을 검사하는 단계를 수행할 수 있습니다. 이 방법은 실수로 인한 메시지 손실을 방지합니다.

### <a name="event-grid"></a>Event Grid

Event Grid 트리거가 실행되면 *하나 이상*의 이벤트가 발생했다는 의미입니다. 예를 들어 논리 앱이 Service Bus 메시지에 대한 Event Grid 트리거를 가져오면 여러 메시지를 처리할 수 있다고 가정해야 합니다.

Event Grid는 서버리스 모델을 사용합니다. 청구는 작업(이벤트 실행) 수를 기반으로 계산됩니다. 자세한 내용은 [Event Grid 가격](https://azure.microsoft.com/pricing/details/event-grid/)을 참조하세요. 현재, Event Grid에 대한 계층 고려 사항은 없습니다.

## <a name="scalability-considerations"></a>확장성 고려 사항

확장성을 높이기 위해 Service Bus 프리미엄 계층에서 메시징 단위 수를 확장할 수 있습니다. 프리미엄 계층 구성에는 한 개, 두 개 또는 네 개의 메시징 단위가 있을 수 있습니다. Service Bus 크기 조정에 대한 자세한 내용은 [Service Bus 메시징을 사용한 성능 향상의 모범 사례](/azure/service-bus-messaging/service-bus-performance-improvements)를 참조하세요.

## <a name="availability-considerations"></a>가용성 고려 사항

각 서비스의 SLA를 검토합니다.

- [API Management SLA][apim-sla]
- [Event Grid SLA][event-grid-sla]
- [Logic Apps SLA][logic-apps-sla]
- [Service Bus SLA][sb-sla]

심각한 중단이 발생하는 경우 장애 조치(failover)가 가능하도록 하려면 Service Bus 프리미엄 계층에서 지리적 재해 복구를 구현하는 것이 좋습니다. 자세한 내용은 [Azure Service Bus 지리적 재해 복구](/azure/service-bus-messaging/service-bus-geo-dr)를 참조하세요.

## <a name="security-considerations"></a>보안 고려 사항

Service Bus를 보호하려면 SAS(공유 액세스 서명)를 사용합니다. [SAS 인증](/azure/service-bus-messaging/service-bus-sas)을 사용하여 특정 권한으로 Service Bus 리소스에 대한 액세스를 사용자에게 부여할 수 있습니다. 자세한 내용은 [Service Bus 인증 및 권한 부여](/azure/service-bus-messaging/service-bus-authentication-and-authorization)를 참조하세요.

Service Bus 큐를 HTTP 엔드포인트(예: 새 메시지 게시용)로 노출해야 하는 경우, API Management를 사용하여 큐가 엔드포인트를 향하도록 배치하여 큐를 보호합니다. 그런 다음, 인증서 또는 OAuth 인증을 적절히 사용하여 엔드포인트를 보호할 수 있습니다. 엔드포인트를 보호하는 가장 쉬운 방법은 HTTP 요청/응답 트리거가 있는 논리 앱을 중개자로 사용하는 것입니다.

Event Grid 서비스는 유효성 검사 코드를 통해 이벤트 전달을 보호합니다. Logic Apps를 통해 이벤트를 사용하는 경우, 유효성 검사가 자동으로 수행됩니다. 자세한 내용은 [Event Grid 보안 및 인증](/azure/event-grid/security-authentication)을 참조하세요.

[apim]: /azure/api-management
[apim-sla]: https://azure.microsoft.com/support/legal/sla/api-management/
[event-grid]: /azure/event-grid/
[event-grid-sla]: https://azure.microsoft.com/support/legal/sla/event-grid
[logic-apps]: /azure/logic-apps/logic-apps-overview
[logic-apps-sla]: https://azure.microsoft.com/support/legal/sla/logic-apps
[sb-sla]: https://azure.microsoft.com/support/legal/sla/service-bus/
[service-bus]: /azure/service-bus-messaging/
[basic-enterprise-integration]: ./basic-enterprise-integration.md
