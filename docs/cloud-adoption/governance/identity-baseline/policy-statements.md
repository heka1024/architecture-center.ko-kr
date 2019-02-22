---
title: 'CAF: ID 기준 정책 설명 샘플'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: ID 기준 정책 설명 샘플
author: BrianBlanchard
ms.openlocfilehash: 5fad9265b9c048ee502c7e084ddd03faa0ad3e23
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55902032"
---
# <a name="identity-baseline-sample-policy-statements"></a>ID 기준 정책 설명 샘플

개별 클라우드 정책 설명은 위험 평가 프로세스 중에 식별된 특정 위험을 처리하기 위한 지침입니다. 이러한 설명에서는 위험과 이를 처리하기 위한 계획에 대한 간략한 요약을 제공해야 합니다. 각 설명 정의에 포함되는 정보는 다음과 같습니다.

- 기술 위험 - 이 정책에서 처리하는 위험에 대한 요약입니다.
- 정책 설명 - 정책 요구 사항에 대한 명확한 요약 설명입니다.
- 설계 옵션 - IT 팀과 개발자가 정책을 구현할 때 사용할 수 있는 실행 가능한 추천 사항, 사양 또는 기타 지침입니다.

다음 정책 설명 샘플은 일반적인 몇 가지 ID 관련 비즈니스 위험을 처리하고 사용자 조직의 요구 사항을 처리하기 위해 정책 설명 초안을 작성할 때 참조할 수 있는 예로 제공됩니다. 이러한 예는 사용 금지되는 것이 아니며, 특정 위험을 처리하기 위한 몇 가지 정책 옵션이 있을 수 있습니다. 비즈니스 및 IT 팀과 긴밀히 협력하여 고유한 위험 세트에 가장 적합한 정책 솔루션을 식별합니다.

## <a name="lack-of-access-controls"></a>액세스 제어 부족

**기술 위험**: 충분하지 않거나 일시적인 액세스 제어 설정으로 인해 중요 리소스 또는 중요 업무용 리소스에 대한 무단 액세스 위험이 발생할 수 있습니다.

**정책 설명**: 클라우드에 배포된 모든 자산은 현재 거버넌스 정책을 통해 승인된 ID와 역할을 사용하여 제어해야 합니다.

**잠재적 설계 옵션**: [Azure Active Directory 조건부 액세스](/azure/active-directory/conditional-access/overview)는 Azure의 기본 액세스 제어 메커니즘입니다.

## <a name="overprovisioned-access"></a>오버프로비저닝된 액세스

**기술 위험**: 해당 책임 영역을 벗어난 리소스를 제어할 수 있는 사용자 및 그룹의 무단 수정으로 인해 가동 중단 또는 보안 취약성이 발생할 수 있습니다.

**정책 설명**: 구현되는 정책은 다음과 같습니다.

- 최소 권한 액세스 모델은 중요 업무용 애플리케이션 또는 보호된 데이터와 관련된 모든 리소스에 적용됩니다.
- 승격된 권한은 예외여야 하며, 클라우드 거버넌스 팀에서 이러한 예외를 기록해야 합니다. 예외는 정기적으로 감사됩니다.

**잠재적 설계 옵션**: [알아야 할 사항](https://wikipedia.org/wiki/Need_to_know) 및 [최소 권한 보안](https://wikipedia.org/wiki/Principle_of_least_privilege) 원칙에 따라 액세스를 제한하는 RBAC(역할 기반 액세스 제어) 전략을 구현하려면 [Azure Identity Management 모범 사례](/azure/security/azure-security-identity-management-best-practices)를 참조하세요.

## <a name="lack-of-shared-management-accounts-between-on-premises-and-the-cloud"></a>온-프레미스와 클라우드 간 공유 관리 계정 부족

**기술 위험**: 클라우드 리소스에 대한 액세스 권한이 충분하지 않을 수 있는 온-프레미스 Active Directory의 계정이 있는 IT 관리 또는 운영 직원은 운영 또는 보안 문제를 효율적으로 해결하지 못할 수 있습니다.

**정책 설명**: 승격된 권한이 있는 온-프레미스 Active Directory 인프라의 모든 그룹은 승인된 RBAC 역할에 매핑되어야 합니다.

**잠재적 설계 옵션**: 클라우드 기반 Azure Active Directory와 온-프레미스 Active Directory 간에 하이브리드 ID 솔루션을 구현하고, 필요한 온-프레미스 그룹을 작업 수행에 필요한 RBAC 역할에 추가합니다.

## <a name="weak-authentication-mechanisms"></a>약한 인증 메커니즘

**기술 위험**: 기본 사용자/암호 조합과 같이 보안이 충분하지 않은 사용자 인증 방법을 사용하는 ID 관리 시스템에서는 암호가 손상되거나 해킹될 수 있으므로 보안 클라우드 시스템에 대한 무단 액세스 위험이 큽니다.

**정책 설명**: 모든 계정은 MFA(다단계 인증) 방법을 사용하여 보안 리소스에 로그인해야 합니다.

**잠재적 설계 옵션**: Azure Active Directory의 경우 사용자 인증 프로세스의 일환으로 [Azure Multi-Factor Authentication](/azure/active-directory/authentication/concept-mfa-howitworks)을 구현합니다.

## <a name="isolated-identity-providers"></a>격리된 ID 공급자

**기술 위험**: 호환되지 않는 ID 공급자는 고객 또는 다른 비즈니스 파트너와 리소스 또는 서비스를 공유하지 못할 수 있습니다.

**정책 설명**: 고객 인증이 필요한 애플리케이션을 배포하려면 내부 사용자에 대한 기본 ID 공급자와 호환되는 승인된 ID 공급자를 사용해야 합니다.

**잠재적 설계 옵션**: 내부 및 고객 ID 공급자 간에 [Azure Active Directory와의 페더레이션](/azure/active-directory/hybrid/whatis-fed)을 구현합니다.

## <a name="identity-reviews"></a>ID 검토

**기술 위험**: 시간이 지남에 따라 새 클라우드 배포 또는 기타 보안 문제가 추가되면 보안 리소스에 대한 무단 액세스 위험이 커질 수 있습니다.

**정책 설명**: 클라우드 자산 구성으로 방지해야 하는 악의적 행위자 또는 사용 패턴을 식별하기 위해 ID 관리 팀과의 분기별 검토가 클라우드 거버넌스 프로세스에 포함되어야 합니다.

**잠재적 설계 옵션**: 거버넌스 팀 구성원과 ID 서비스 관리를 담당하는 IT 직원이 모두 참여하는 분기별 보안 검토 회의를 설정합니다. 기존 보안 데이터와 메트릭을 검토하여 현재 ID 관리 정책 및 도구에 격차를 설정하고 새 위험을 완화하도록 정책을 업데이트합니다.

## <a name="next-steps"></a>다음 단계

이 문서에서 언급한 샘플을 시작 지점으로 사용하여 클라우드 채택 계획에 부합하는 특정 비즈니스 위험을 처리하는 정책을 개발합니다.

보안 기준과 관련된 사용자 고유의 사용자 지정 정책 설명 개발하려면 [보안 기준 템플릿](template.md)을 다운로드합니다.

이 분야의 채택을 가속화하려면 사용자 환경에 가장 가깝게 일치하는 [실행 가능한 거버넌스 경험](../journeys/overview.md)을 선택합니다. 그런 다음, 특정 회사 정책 결정을 통합하도록 설계를 수정합니다.

> [!div class="nextstepaction"]
> [실행 가능한 거버넌스 경험](../journeys/overview.md)