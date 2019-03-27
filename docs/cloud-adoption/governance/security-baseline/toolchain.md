---
title: 'CAF: Azure에서 보안 기준 도구'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Azure에서 보안 기준 강화를 용이하게 해주는 도구 설명
author: BrianBlanchard
ms.openlocfilehash: b316626c8ad717514f7f592abefa0f33a92afdca
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58243174"
---
# <a name="security-baseline-tools-in-azure"></a>Azure에서 보안 기준 도구

[보안 기준](overview.md)은 [5가지 클라우드 거버넌스 분야](../governance-disciplines.md) 중 하나입니다. 이 분야는 네트워크, 자산 및 클라우드 공급자 솔루션에 상주할 데이터를 보호(가장 중요)하는 정책을 수립하는 방법에 중점을 둡니다. 클라우드 거버넌스의 5가지 분야 내에 있는 보안 기준에는 디지털 자산 및 데이터의 분류가 포함됩니다. 또한 데이터, 자산 및 네트워크의 보안과 관련된 위험, 비즈니스 허용 범위 및 완화 전략에 대한 설명서도 포함됩니다. 기술적 측면에서는, [암호화](../../decision-guides/encryption/overview.md), [네트워크 요구 사항](../../decision-guides/software-defined-network/overview.md), [하이브리드 ID 전략](../../decision-guides/identity/overview.md), [리소스 그룹](../../decision-guides/resource-consistency/overview.md) 간 보안 정책의 [자동 적용](../../decision-guides/policy-enforcement/overview.md)을 위한 도구와 관련된 의사 결정 참여도 포함됩니다.

다음은 보안 기준을 지원하는 정책과 프로세스를 발전시키는 데 도움이 되는 Azure 도구 목록입니다.

|                                                            | [Azure Portal](https://azure.microsoft.com/features/azure-portal/) / [Resource Manager](/azure/azure-resource-manager/resource-group-overview)  | [Azure Key Vault](/azure/key-vault)  | [Azure AD](/azure/active-directory/fundamentals/active-directory-whatis) | [Azure Policy](/azure/governance/policy/overview) | [Azure Security Center](/azure/security-center/security-center-intro) | [Azure Monitor](/azure/azure-monitor/overview) |
|------------------------------------------------------------|---------------------------------|-----------------|----------|--------------|-----------------------|---------------|
| 리소스 및 리소스 생성에 액세스 제어 적용   | 예                             | no              | 예      | 아니요           | 아니요                    | 아니요            |
| 가상 네트워크 보호                                    | 예                             | 아니요              | 아니요       | 예          | 아니요                    | 아니요            |
| 가상 디스크 암호화                                     | 아니요                              | 예             | 아니요       | 아니요           | 아니요                    | 아니요            |
| PaaS 스토리지 및 데이터베이스 암호화                         | 아니요                              | 예             | 아니요       | 아니요           | 아니요                    | 아니요            |
| 하이브리드 ID 서비스 관리                            | 아니요                              | 아니요              | 예      | 아니요           | 아니요                    | 아니요            |
| 허용되는 리소스 유형 제한                         | 아니요                              | 아니요              | 아니요       | 예          | 아니요                    | 아니요            |
| 지리적 지역 제한 적용                          | 아니요                              | 아니요              | 아니요       | 예          | 아니요                    | 아니요            |
| 네트워크 및 리소스의 보안 상태 모니터링          | 아니요                              | 아니요              | 아니요       | 아니요           | 예                   | 예           |
| 악의적 활동 감지                                  | 아니요                              | 아니요              | 아니요       | 아니요           | 예                   | 예           |
| 사전에 취약점 감지                        | 아니요                              | 아니요              | 아니요       | 아니요           | 예                   | 아니요            |
| 백업 및 재해 복구 구성                     | 예                             | 아니요              | 아니요       | 아니요           | 아니요                    | 아니요            |

Azure 보안 도구 및 서비스의 전체 목록은 [Azure에서 사용 가능한 보안 서비스 및 기술](/azure/security/azure-security-services-technologies)을 참조하세요.

또한 고객이 보안 기준 활동을 수월하게 하기 위해 타사 도구를 사용하는 것도 매우 일반적입니다. 자세한 내용은 [Azure Security Center에서 보안 솔루션 통합](/azure/security-center/security-center-partner-integration) 문서를 참조하세요.

보안 도구 외에도 [Microsoft Trust Center](https://www.microsoft.com/trustcenter/guidance/risk-assessment)에는 마이그레이션 계획 프로세스의 일환으로 위험 평가를 수행하는 데 도움이 되는 광범위한 지침, 보고서 및 관련 설명서가 포함되어 있습니다.
