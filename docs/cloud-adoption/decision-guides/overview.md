---
title: 'CAF: 아키텍처 의사 결정 가이드'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 클라우드 채택 프레임워크의 아키텍처 의사 결정 가이드에 대해 알아봅니다.
author: rotycenh
ms.openlocfilehash: b43047b5fc5b636bc84b9f28ec24846730e63672
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901959"
---
# <a name="architectural-decision-guides"></a>아키텍처 의사 결정 가이드

클라우드 채택 프레임워크의 아키텍처 의사 결정 가이드는 클라우드 거버넌스 설계 지침을 만들 때 도움이 되는 패턴과 모델에 대해 설명합니다. 각 의사 결정 가이드에는 클라우드 배포의 핵심 인프라 구성 요소에 중점을 두고, 특정 클라우드 배포 시나리오를 지원하기 위한 잠재적 패턴 또는 모델이 나와 있습니다.

조직에 대한 클라우드 거버넌스를 설정하려면 실행 가능한 거버넌스 경험에서 기준 로드맵을 제공합니다. 그러나 이러한 경험은 반영하지 못할 수 있는 조직의 요구 사항과 우선 순위에 대해 가정합니다.
이러한 의사 결정 가이드는 예제 설계 지침에서 선택한 아키텍처 설계 항목을 사용자 고유의 요구 사항에 맞추는 데 도움이 되는 대체 패턴과 모델을 제공하여 거버넌스 경험 샘플을 보완합니다.

## <a name="design-guidance-categories"></a>설계 지침 범주

다음 각 범주는 모든 클라우드 배포의 기본 기술을 나타냅니다. 요구 사항과 일치하지 않는 설계 경험 예제의 각 선택 항목에 대해 관련된 아래 섹션의 옵션을 검토하여 요구 사항에 더 적합한 패턴 또는 모델을 선택합니다.

[구독](./subscriptions/overview.md): 클라우드 배포의 구독 설계 및 계정 구조를 조직의 소유권, 청구 및 관리 기능에 맞게 계획합니다.

[ID](./identity/overview.md): 클라우드 기반 ID 서비스를 기존 ID 리소스와 통합하여 IT 환경 내의 액세스 제어를 지원합니다.

[정책 적용](./policy-enforcement/overview.md): 클라우드에 배포하는 리소스와 워크로드에 대한 조직 정책 규칙을 정의하고 적용합니다.

[리소스 일관성](./resource-consistency/overview.md): 리소스 관리 및 정책 요구 사항을 적용하도록 클라우드 기반 리소스의 배포와 구조를 조정합니다.

[리소스 태그 지정](./resource-tagging/overview.md): 청구 모델, 클라우드 계정 방식, 관리, 액세스 제어 및 운영 효율성을 지원하도록 클라우드 기반 리소스를 구성합니다. 리소스 태그 지정에는 일관되고 잘 구성된 명명 및 메타데이터 체계가 필요합니다.

[소프트웨어 정의 네트워크](./software-defined-network/overview.md): 가상화된 네트워킹 기능을 빠르게 배포하고 수정하여 보안 워크로드를 클라우드에 배포합니다. SDN(소프트웨어 정의 네트워크)은 민첩한 워크플로를 지원하고, 리소스를 격리하며, 클라우드 기반 시스템을 기존 IT 인프라와 통합할 수 있습니다.

[암호화](./encryption/overview.md): 클라우드 배포 내에서 보안의 중요한 측면인 암호화를 사용하여 중요한 데이터를 보호합니다.

[로그 및 보고](./log-and-report/overview.md): 클라우드 기반 리소스에서 생성된 로그 데이터를 모니터링합니다. 데이터 분석은 워크로드의 운영, 유지 관리 및 정책 적용 상태에 대한 상태 관련 인사이트를 제공합니다.

## <a name="next-steps"></a>다음 단계

구독과 계정에서 클라우드 배포의 기반 역할을 수행하는 방법에 대해 알아봅니다.

> [!div class="nextstepaction"]
> [구독 설계](subscriptions/overview.md)
