---
title: 'CAF: 보안 기준 분야 향상'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 보안 기준 분야 향상
author: BrianBlanchard
ms.openlocfilehash: 28a971f56c9f8ada1d184bdc1cb3dbb9a17c3507
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58246454"
---
# <a name="security-baseline-discipline-improvement"></a>보안 기준 분야 향상

보안 기준 분야는 네트워크, 자산 및 클라우드 공급자 솔루션에 상주할 데이터(가장 중요)를 보호하는 정책을 수립하는 방법에 중점을 둡니다. 5개 클라우드 거버넌스 분야의 보안 기준에는 디지털 자산 및 데이터에 대한 분류가 포함됩니다. 또한 데이터, 자산 및 네트워크의 보안과 관련된 위험, 비즈니스 허용 범위 및 완화 전략에 대한 설명서도 포함됩니다. 기술적 관점에서 볼 때 [암호화](../../decision-guides/encryption/overview.md), [네트워크 요구 사항](../../decision-guides/software-defined-network/overview.md), [하이브리드 ID 전략](../../decision-guides/identity/overview.md) 및 클라우드 보안 기준 정책을 개발하는 데 사용된 [프로세스](compliance-processes.md)와 관련된 의사 결정에의 참여도 포함됩니다.

이 문서에서는 보안 기준 분야를 더 효율적으로 개발하고 완성도를 높이기 위해 회사에서 참여할 수 있는 몇 가지 잠재적 작업에 대해 간략하게 설명합니다. 이러한 작업은 클라우드 솔루션 구현의 계획, 구축, 채택 및 운영 단계로 나눌 수 있으며, [클라우드 거버넌스에 대한 점진적 접근 방식](../journeys/overview.md#an-incremental-approach-to-cloud-governance)을 개발하는 과정에서 반복됩니다.

![도입의 4단계](../../_images/adoption-phases.png)

*그림 1. 클라우드 거버넌스의 증분 방식 도입 단계.*

한 문서에서 모든 비즈니스의 요구 사항을 설명할 수는 없습니다. 따라서 이 문서에서는 거버넌스 완성 프로세스의 각 단계에서 권장되는 최소 활동과 수행할 가능성이 있는 활동의 예를 간략하게 설명합니다. 이러한 활동의 초기 목표는 [정책 MVP](../journeys/overview.md#an-incremental-approach-to-cloud-governance)를 구축하고 점진적 정책 전개를 위한 프레임워크를 설정하는 것입니다. 클라우드 거버넌스 팀에서 보안 기준 거버넌스 기능을 향상시키기 위해 이러한 활동에 투자할 비용을 결정해야 합니다.

> [!CAUTION]
> 이 문서에서 설명하는 최소 활동과 잠재적 활동은 모두 특정 회사 정책 또는 타사 규정 준수 요구 사항에 부합하지 않습니다. 이 지침은 클라우드 거버넌스 모델을 사용하여 두 요구 사항을 모두 충족하기 위한 논의를 원활하게 진행할 수 있도록 설계되었습니다.

## <a name="planning-and-readiness"></a>계획 및 준비

이 거버넌스 완성 과정 단계에서는 서로 분리되어 있는 사업 결과와 실행 가능한 전략을 연결합니다. 이 프로세스에서는 리더십 팀이 특정 메트릭을 정의하여 디지털 자산에 매핑한 다음, 전반적인 마이그레이션 작업 계획을 시작합니다.

**최소 제안 작업:**

- [보안 기준 도구 체인](toolchain.md) 옵션을 평가합니다.
- 아키텍처 지침 문서 초안을 개발하여 주요 이해 관계자에게 배포합니다.
- 아키텍처 지침 개발의 영향을 받는 사용자와 팀을 교육하고 참여시킵니다.
- 우선 순위가 지정된 보안 작업을 마이그레이션 백로그에 추가합니다.

**잠재적 활동:**

- 데이터 분류 스키마를 정의합니다.
- 디지털 자산 계획 프로세스를 수행하여 비즈니스 프로세스와 지원 작업을 강화하는 현재 IT 자산의 인벤토리를 작성합니다.
- [정책 검토](../../governance/policy-compliance/what-is-a-cloud-policy-review.md)를 수행하여 기존의 회사 IT 보안 정책을 현대화하는 프로세스를 시작하고, 알려진 위험을 처리하는 MVP 정책을 정의합니다.
- 클라우드 플랫폼의 보안 지침을 검토합니다. Azure의 경우 이러한 지침은 [Microsoft Service Trust 플랫폼](https://www.microsoft.com/trustcenter/stp/default.aspx)에서 찾을 수 있습니다.
- 보안 기준 정책에 [보안 개발 수명 주기](https://www.microsoft.com/securityengineering/sdl/)가 포함되어 있는지 확인합니다.
- 다음 1~3개의 릴리스에 따라 네트워크, 데이터 및 자산 관련 비즈니스 위험을 평가하고, 이러한 위험에 대한 조직의 허용 범위를 측정합니다.
- Microsoft의 [top trends in cybersecurity](https://www.microsoft.com/security/operations/security-intelligence-report)(주요 사이버 보안 추세) 보고서를 검토하여 현재 보안 상황에 대한 개요를 확인합니다.
- 조직에서 [Security DevOps](https://www.microsoft.com/en-us/securityengineering/devsecops)(보안 DevOps) 역할을 개발하는 것이 좋습니다.

<!-- "en-us" location is required for the URL above. -->

## <a name="build-and-pre-deployment"></a>빌드 및 배포 전

환경을 성공적으로 마이그레이션하려면 여러 가지 기술 및 비기술적 필수 구성 요소가 필요합니다. 이 프로세스는 의사 결정, 준비 및 마이그레이션을 진행하는 핵심 인프라에 중점을 둡니다.

**최소 제안 작업:**

- 배포 전 단계에서 롤아웃하여 [보안 기준 도구 체인](toolchain.md)을 구현합니다.
- 아키텍처 지침 문서를 업데이트하고 주요 이해 관계자에게 배포합니다.
- 보안 작업을 우선 순위가 지정된 마이그레이션 백로그에 구현합니다.
- 교육 자료와 설명서, 인식 커뮤니케이션, 인센티브 및 다른 프로그램을 개발하여 사용자의 채택을 촉진합니다.

**잠재적 활동:**

- 클라우드 호스팅 데이터에 대한 조직의 [암호화](../../decision-guides/encryption/overview.md) 전략을 결정합니다.
- 클라우드 배포의 [ID](../../decision-guides/identity/overview.md) 전략을 평가합니다. 클라우드 기반 ID 솔루션이 온-프레미스 ID 공급자와 공존하거나 통합되는 방법을 결정합니다.
- 가상화된 네트워킹 기능을 안전하게 유지할 수 있도록 [SDN(소프트웨어 정의 네트워킹)](../../decision-guides/software-defined-network/overview.md) 설계에 대한 네트워크 경계 정책을 결정합니다.
- 조직의 [최소 권한 액세스](/azure/active-directory/users-groups-roles/roles-delegate-by-task) 정책을 평가하고, 작업 기반 역할을 사용하여 특정 리소스에 대한 액세스를 제공합니다.
- 보안 및 모니터링 메커니즘을 모든 클라우드 서비스 및 가상 머신에 적용합니다.
- 가능한 경우 [보안 정책](../../decision-guides/policy-enforcement/overview.md)을 자동화합니다.
- 보안 기준 정책을 검토하고, [보안 개발 수명 주기](https://www.microsoft.com/securityengineering/sdl/)에 요약된 것과 같은 모범 사례 지침에 따라 계획을 수정해야 하는지 여부를 결정합니다.

## <a name="adopt-and-migrate"></a>채택 및 마이그레이션

마이그레이션은 기존 디지털 자산의 애플리케이션이나 워크로드 이동, 테스트 및 도입에 중점을 두는 증분 방식 프로세스입니다.

**최소 제안 작업:**

- [보안 기준 도구 체인](toolchain.md)을 배포 전 환경에서 프로덕션 환경으로 마이그레이션합니다.
- 아키텍처 지침 문서를 업데이트하고 주요 이해 관계자에게 배포합니다.
- 교육 자료와 설명서, 인식 커뮤니케이션, 인센티브 및 다른 프로그램을 개발하여 사용자의 채택을 촉진합니다.

**잠재적 활동:**

- 최신 보안 기준과 위협 정보를 검토하여 새 비즈니스 위험을 식별합니다.
- 발생할 수 있는 새 보안 위험을 처리할 수 있는 조직의 허용 범위를 측정합니다.
- 정책의 편차를 식별하고 수정을 적용합니다.
- 보안 및 액세스 제어 자동화를 조정하여 정책 준수를 최대한 보장합니다.  
- 빌드/배포 전 단계에서 정의된 모범 사례가 제대로 실행되는지 확인합니다.
- 최소 권한 액세스 정책을 검토하고, 액세스 제어를 조정하여 보안을 최대화합니다.
- 워크로드에 대해 보안 기준 도구 체인을 테스트하여 취약성을 식별하고 해결합니다.

## <a name="operate-and-post-implementation"></a>운영 및 구현 후

변환이 완료되면 애플리케이션이나 워크로드의 원래 수명 주기 동안 거버넌스와 관련 작업이 실행되어야 합니다. 이 거버넌스 완성 과정 단계에서는 일반적으로 솔루션이 구현되고 변환 주기가 안정화되기 시작한 후에 진행되는 활동을 중점적으로 수행합니다.

**최소 제안 작업:**

- [보안 기준 도구 체인](toolchain.md)을 유효성 검사 및/또는 구체화합니다.
- 알림 및 보고서를 사용자 지정하여 잠재적인 보안 문제를 경고합니다.
- 향후 채택 프로세스를 안내하기 위한 아키텍처 지침을 구체화합니다.
- 영향을 받는 팀에 주기적으로 전달하고 교육하여 아키텍처 지침을 지속적으로 준수하도록 합니다.

**잠재적 활동:**

- 워크로드에 대한 패턴과 동작을 검색하고, 모니터링 및 보고 도구를 구성하여 비정상적인 활동, 액세스 또는 리소스 사용을 식별하고 알려줍니다.
- 모니터링 및 보고 정책을 지속적으로 업데이트하여 최신 취약성, 악용 및 공격을 탐지합니다.
- 무단 액세스를 빠르게 중지하고 공격자에 의해 손상되었을 수 있는 리소스를 사용하지 않도록 설정하는 절차를 마련합니다.
- 가능한 경우 최신 보안 모범 사례를 정기적으로 검토하고, 보안 정책, 자동화 및 모니터링 기능에 추천 사항을 적용합니다.

## <a name="next-steps"></a>다음 단계

이제 클라우드 보안 거버넌스의 개념을 이해했으므로 [Microsoft에서 제공하는 Azure에 대한 보안 및 모범 사례 지침](azure-security-guidance.md)에 대해 자세히 알아봅니다.

> [!div class="nextstepaction"]
> [Azure 보안 지침에 자세한 정보](azure-security-guidance.md)
> [Azure 보안 소개](/azure/security/azure-security)
> [로깅, 보고 및 모니터링에 자세한 정보](../../decision-guides/log-and-report/overview.md)
