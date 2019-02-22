---
title: 'CAF: 배포 가속화 샘플 정책 설명'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 배포 가속화 샘플 정책 설명
author: alexbuckgit
ms.openlocfilehash: 4f7d59e6653c29db03f966d1c7105524b72586fc
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901824"
---
# <a name="deployment-acceleration-sample-policy-statements"></a>배포 가속화 샘플 정책 설명

개별 클라우드 정책 설명은 위험 평가 프로세스 중에 식별된 특정 위험을 처리하기 위한 지침입니다. 이러한 설명에서는 위험과 이를 처리하기 위한 계획에 대한 간략한 요약을 제공해야 합니다. 각 설명 정의에 포함되는 정보는 다음과 같습니다.

- **기술 위험**. 이 정책이 처리할 위험의 요약입니다.
- **정책 설명**. 정책 요구 사항에 대한 명확한 요약 설명입니다.
- **설계 옵션**. IT 팀과 개발자가 정책을 구현할 때 사용할 수 있는 실행 가능한 추천 사항, 사양 또는 기타 지침입니다.

다음 정책 설명 샘플은 일반적인 몇 가지 구성 관련 비즈니스 위험을 처리하고 사용자 조직의 요구 사항을 처리하기 위해 정책 설명 초안을 작성할 때 참조할 수 있는 예로 제공됩니다. 이러한 예제를 사용 금지해야 하는 것은 아니며, 모든 특정 위험을 해결하기 위한 여러 정책 옵션이 있을 수 있습니다. 비즈니스 및 IT 팀과 긴밀히 협력하여 고유한 위험 세트에 가장 적합한 정책 솔루션을 식별합니다.

## <a name="reliance-on-manual-deployment-or-configuration-of-systems"></a>수동 배포 또는 시스템 구성 사용

**기술 위험**: 배포나 구성 중 사용자 작업에 의존하면 사람의 실수 가능성이 높아지고 시스템 배포 및 구성의 반복 및 예측 가능성은 낮아집니다. 일반적으로 시스템 리소스의 배포 속도도 느려집니다.

**정책 설명**: 클라우드에 배포된 모든 자산은 가능한 모든 경우에서 템플릿이나 자동화 스크립트로 배포되어야 합니다.

**잠재적 설계 옵션**: [Azure Resource Manager 템플릿](/azure/azure-resource-manager/resource-group-overview#template-deployment)은 Azure에 리소스를 배포하기 위한 코드 형태의 인프라 방식을 제공합니다. [Azure Building Blocks](https://github.com/mspnp/template-building-blocks/wiki)는 Azure 리소스 배포를 간소화하도록 설계된 Resource Manager 템플릿 집합과 명령줄 도구를 제공합니다.

## <a name="lack-of-visibility-into-system-issues"></a>시스템 문제에 대한 가시성 부족

**기술 위험**: 비즈니스 시스템에 대한 모니터링과 진단이 부족하여 운영 인력이 시스템 중단 발생 전에 문제를 식별하여 조치하지 못하며, 중단을 제대로 해결하는 데 필요한 시간이 크게 늘어날 수 있습니다.

**정책 설명**: 구현되는 정책은 다음과 같습니다.

- 모든 프로덕션 시스템과 구성 요소에 대해 주요 메트릭 및 진단 측정값을 식별하며 모니터링 및 진단 도구를 해당 시스템에 적용하고 운영 입력이 정기적으로 모니터링합니다.
- 운영에서는 프로덕션 환경에서 사용하기 전에 먼저 준비 및 QA 같은 비 프로덕션 환경에서 모니터링 및 진단 도구의 사용을 고려합니다.

**잠재적 설계 옵션**: Log Analytics 및 Application Insights를 포함한 [Azure Monitor](/azure/azure-monitor/)는 애플리케이션의 업무 수행을 이해하고 영향을 미치는 문제와 관련 리소스를 미리 식별할 수 있게 원격 분석 데이터를 수집하여 분석하는 도구를 제공합니다.

## <a name="configuration-security-reviews"></a>구성 보안 검토

**기술 위험**: 시간이 지날수록 새 보안 위협 또는 우려로 인해 보안 리소스에 대한 무단 액세스 위험이 증대합니다.

**정책 설명**: 클라우드 자산 구성으로 방지해야 하는 악의적 행위자 또는 사용 패턴을 식별하기 위해 구성 관리 팀과의 분기별 검토가 클라우드 거버넌스 프로세스에 포함되어야 합니다.

**잠재적 설계 옵션**: 거버넌스 팀 구성원과, 구성 클라우드 애플리케이션 및 리소스를 담당하는 IT 직원이 모두 참여하는 분기별 보안 검토 회의를 설정합니다. 기존 보안 데이터와 메트릭을 검토하여 현재 배포 관리 정책 및 도구에 격차를 설정하고 새 위험을 완화하도록 정책을 업데이트합니다.

## <a name="next-steps"></a>다음 단계

이 문서에서 언급한 샘플을 시작 지점으로 사용하여 클라우드 채택 계획에 부합하는 특정 비즈니스 위험을 처리하는 정책을 개발합니다.

ID 관리와 관련한 사용자 고유의 사용자 지정 정책 설명 개발하려면 [ID 기준 템플릿](template.md)을 다운로드합니다.

이 분야의 도입을 가속화하려면 사용자 환경에 가장 가깝게 일치하는 [실행 가능한 거버넌스 경험](../journeys/overview.md)을 선택합니다. 그런 다음, 특정 회사 정책 결정을 통합하도록 설계를 수정합니다.

> [!div class="nextstepaction"]
> [실행 가능한 거버넌스 경험](../journeys/overview.md)