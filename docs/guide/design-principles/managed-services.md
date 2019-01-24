---
title: 관리되는 서비스 사용
titleSuffix: Azure Application Architecture Guide
description: 가능하면 IaaS(Infrastructure as a Service)보다 PaaS(Platform as a Service)를 사용합니다.
author: MikeWasson
ms.date: 08/30/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: seojan19
ms.openlocfilehash: f08914e1eacc4f02fdb16093fe590f46249e3da3
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54485811"
---
# <a name="use-managed-services"></a>관리되는 서비스 사용

## <a name="when-possible-use-platform-as-a-service-paas-rather-than-infrastructure-as-a-service-iaas"></a>가능하면 IaaS(Infrastructure as a Service)보다 PaaS(Platform as a Service)를 사용합니다.

IaaS는 부품 상자를 사용하는 것과 같습니다. 어떤 것도 작성할 수 있지만 직접 조립해야 합니다. 관리되는 서비스를 보다 쉽게 구성 및 관리할 수 있습니다. VM을 프로비전하거나, Vnet을 설정하거나, 패치 및 업데이트와 VM에서 소프트웨어를 실행하는 것과 관련된 기타 모든 오버헤드를 관리할 필요가 없습니다.

예를 들어, 애플리케이션에 메시지 큐가 필요하다고 가정해보겠습니다. RabbitMQ 등을 사용하여 VM에서 자체 메시징 서비스를 설정할 수 있습니다. 그렇지만 Azure Service Bus는 신뢰할 수 있는 메시징을 이미 서비스로 제공하고 있으므로 더 간단하게 설정할 수 있습니다. Service Bus 네임 스페이스를 만든 다음(배포 스크립트의 일부로 수행할 수 있음) 클라이언트 SDK를 사용하여 Service Bus를 호출합니다.

물론, 애플리케이션에 IaaS 방식이 더 적합할 수 밖에 없는 특정 요구 사항이 있을 수 있습니다. 그러나 애플리케이션이 IaaS를 기준으로 하더라도 관리 서비스를 통합하는 것이 적절한 경우를 고려해야 합니다. 여기에는 캐시, 큐 및 데이터 저장소가 포함됩니다.

| 기존 실행 서비스... | 새 서비스... |
|-----------------------|-------------|
| Active Directory | Azure Active Directory Domain Services |
| Elasticsearch | Azure Search |
| Hadoop은 | HDInsight |
| IIS | App Service |
| MongoDB | Cosmos DB |
| Redis | Azure Redis 캐시(영문) |
| SQL Server | Azure SQL Database |
