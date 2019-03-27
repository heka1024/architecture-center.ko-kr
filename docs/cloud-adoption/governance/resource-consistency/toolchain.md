---
title: 'CAF: Azure의 리소스 일관성 도구'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Azure의 리소스 일관성 도구
author: BrianBlanchard
ms.openlocfilehash: 68503289f60fbb3682264ff39546ca7b7700cef5
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58241584"
---
# <a name="resource-consistency-tools-in-azure"></a>Azure의 리소스 일관성 도구

[리소스 일관성](overview.md)은 [5개 클라우드 거버넌스 분야](../governance-disciplines.md) 중 하나입니다. 이 분야는 환경, 애플리케이션 또는 워크로드의 운영 관리와 관련된 정책을 설정하는 방법에 중점을 둡니다. 5개 클라우드 거버넌스 분야의 리소스 일관성에는 애플리케이션, 워크로드 및 자산 성능 모니터링이 포함됩니다. 또한 크기 조정 요구 사항을 충족시키고, 성능 SLA 위반을 수정하며, 자동화된 업데이트 관리를 통해 성능 SLA 위반을 사전에 방지하는 데 필요한 작업도 포함됩니다.

이 거버넌스 분야를 지원하는 정책과 프로세스를 완성하는 데 도움이 되는 Azure 도구 목록은 다음과 같습니다.

|    | [Azure Portal](https://azure.microsoft.com/features/azure-portal/)  | [Azure 리소스 관리자](/azure/azure-resource-manager/resource-group-overview)  | [Azure Blueprints](/azure/governance/blueprints/overview) | [Azure Automation](/azure/automation/automation-intro) | [Azure AD](/azure/active-directory/fundamentals/active-directory-whatis) |
|---------|---------|---------|---------|---------|---------|
| 리소스 배포                             | 예 | 예 | 예 | 예 | 아니요  |
| 리소스 관리                             | 예 | 예 | 예 | 예 | 아니요  |
| 템플릿을 사용하여 리소스 배포             | 아니요  | 예 | no  | 예 | 아니요  |
| 오케스트레이션된 환경 배포          | 아니요  | 아니요  | 예 | 아니요  | 아니요  |
| 리소스 그룹 정의                       | 예 | 예 | 예 | 아니요  | 아니요  |
| 워크로드 및 계정 소유자 관리           | 예 | 예 | 예 | 아니요  | 아니요  |
| 리소스에 대한 조건부 액세스 관리       | 예 | 예 | 예 | 아니요  | 아니요  |
| RBAC 사용자 구성                         | 예 | 아니요  | 아니요  | 아니요  | 예 |
| 리소스에 역할 및 권한 할당 | 예 | 예 | 예 | no  | 예 |
| 리소스 간 종속성 정의        | 아니요  | 예 | 예 | 아니요  | 아니요  |
| 액세스 제어 적용                         | 예 | 예 | 예 | no  | 예 |
| 가용성 및 확장성 평가          | 아니요  | 아니요  | 아니요  | 예 | 아니요  |
| 리소스에 태그 적용                      | 예 | 예 | 예 | 아니요  | 아니요  |
| Azure Policy 규칙 할당                    | 예 | 예 | 예 | 아니요  | 아니요  |
| 재해 복구 리소스 계획         | 예 | 예 | 예 | 아니요  | 아니요  |
| 자동화된 업데이트 관리 적용                  | 아니요  | 아니요  | 아니요  | 예 | 아니요  |
| 청구 관리                               | 예 | 아니요  | 아니요  | 아니요  | 아니요  |

이러한 리소스 일관성 도구 및 기능과 함께 성능 및 상태 문제에 대해 배포된 리소스를 모니터링해야 합니다. [Azure Monitor](/azure/azure-monitor/overview)는 Azure의 기본 모니터링 및 보고 솔루션입니다. Azure Monitor는 클라우드 리소스를 모니터링하는 데 사용할 수 있는 별도의 여러 기능을 제공하며, 일반적인 모니터링 요구 사항을 해결할 수 있는 기능을 보여 주는 목록은 다음과 같습니다.

|                                                    | [Azure Portal](https://azure.microsoft.com/features/azure-portal/) | [Application Insights](/azure/application-insights/app-insights-overview) | [Log Analytics](/azure/azure-monitor/log-query/log-query-overview) | [Azure Monitor Rest API](/rest/api/monitor/) |
|----------------------------------------------------|--------------|----------------------|---------------|------------------------|
| 가상 머신 원격 분석 데이터 로깅                 | 아니요           | 아니요                   | 예           | 아니요                     |
| 가상 네트워킹 원격 분석 데이터 로깅              | 아니요           | 아니요                   | 예           | 아니요                     |
| PaaS 서비스 원격 분석 데이터 로깅                   | 아니요           | 아니요                   | 예           | 아니요                     |
| 애플리케이션 원격 분석 데이터 로깅                     | 아니요           | 예                  | 아니요            | 아니요                     |
| 보고서 및 경고 구성                       | 예          | 아니요                   | 아니요            | 예                    |
| 일반 보고서 또는 사용자 지정 분석 예약        | 아니요           | 아니요                   | 아니요            | 아니요                     |
| 로그 및 성능 데이터 시각화 및 분석     | 예          | 아니요                   | 아니요            | 아니요                     |
| 온-프레미스 또는 타사 모니터링 솔루션 통합     | 아니요           | 아니요                   | 아니요            | 예                    |

배포를 계획하는 경우 로깅 데이터가 저장되는 위치와 클라우드 기반 [보고 및 모니터링 서비스](../../decision-guides/log-and-report/overview.md)를 기존 프로세스 및 도구와 통합하는 방법을 고려해야 합니다.

> [!NOTE]
> 조직에서는 타사 DevOps 도구도 사용하여 워크로드와 리소스를 모니터링합니다. 자세한 내용은 [DevOps Tool Integrations](https://azure.microsoft.com/products/devops-tool-integrations/)를 참조하세요.

# <a name="next-steps"></a>다음 단계

Azure에서 [정책 정의](/azure/governance/policy/)를 만들고, 할당하고, 관리하는 방법에 대해 알아봅니다.
