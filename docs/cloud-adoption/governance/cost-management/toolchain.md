---
title: 'CAF: Azure의 비용 관리 도구'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Azure의 비용 관리 도구
author: BrianBlanchard
ms.openlocfilehash: 58dfa604863f704fd9b9fbb8d0693447cecdaf84
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901448"
---
# <a name="cost-management-tools-in-azure"></a>Azure의 비용 관리 도구

[비용 관리](overview.md)는 [5가지 클라우드 거버넌스 분야](../governance-disciplines.md) 중 하나입니다. 이 분야에서는 클라우드 지출 계획을 설정하고, 클라우드 예산을 할당하고, 클라우드 예산을 모니터링/적용하고, 비용이 많이 발생하는 변칙 상황을 검색하고, 실제 지출이 예상과 다른 경우 클라우드 거버넌스 계획을 조정하는 방식을 중점적으로 결정합니다.

아래 표에는 이 거버넌스 분야를 지원하는 정책과 프로세스를 완료하는 데 활용할 수 있는 Azure 기본 도구 목록이 나와 있습니다.

|  | [Azure Portal](https://azure.microsoft.com/features/azure-portal/)  | [Azure Cost Management](/azure/cost-management/overview-cost-mgt)  | [Azure EA Content Pack](/power-bi/service-connect-to-azure-enterprise)  | [Azure Policy](/azure/governance/policy/overview) |
|---------|---------|---------|---------|---------|
|기업계약 필요 여부     | 아니요         | 예([Cloudyn](/azure/cost-management/overview)에서는 불필요)         | 예         | 아니요         |
|예산 제어     | 아니요         | 예         | no         | 예         |
|단일 리소스에 대한 지출 모니터링    | 예         | 예         | 예         | 아니요         |
|여러 리소스에 대한 지출 모니터링    | 아니요         | 예        | 예         | 아니요         |
|단일 리소스에 대한 지출 제어     | 예 - 수동 크기 조정         | 예         | no         | 예         |
|여러 리소스에서 지출 적용    | 아니요         | 예         | no         | 예         |
|추세 모니터링 및 검색     | 예 - 제한됨         | 예        | 예         | 아니요         |
|변칙적 지출 검색     | 아니요         | 예        | 예         | 아니요        |
|편차 조정     | 아니요        | 예        | 예        | 아니요        |
