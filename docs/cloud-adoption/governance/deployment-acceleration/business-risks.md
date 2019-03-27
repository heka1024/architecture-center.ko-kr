---
title: 'CAF: 배포를 가속화하는 동기 부여 및 비즈니스 위험'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 클라우드 거버넌스 전략의 일부인 배포 가속 분야에 대해 알아봅니다.
author: alexbuckgit
ms.openlocfilehash: 854b1fd420de605a665922e9b207e6aecbfab2f0
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58242144"
---
# <a name="deployment-acceleration-motivations-and-business-risks"></a>배포 가속 동기 부여 및 비즈니스 위험

이 문서에서는 고객이 일반적으로 클라우드 거버넌스 전략 내에 배포 가속화 분야를 도입하는 이유를 설명합니다. 정책 설명이 필요한 비즈니스 위험의 몇 가지 예제도 제공합니다.

<!-- markdownlint-disable MD026 -->

## <a name="is-deployment-acceleration-relevant"></a>배포 가속이 적절합니까?

온-프레미스 시스템은 종종 기준 이미지 또는 설치 스크립트를 사용하여 배포됩니다. 여러 단계를 수행해야 하거나 사람이 개입해야 하는 추가 구성은 일반적으로 불필요합니다. 이러한 수동 프로세스는 오류가 발행하기 쉽고 종종 "구성 드리프트"로 연결되며, 문제 해결 및 수정 작업에 오랜 시간이 걸립니다.

대부분의 Azure 리소스는 Azure Portal을 통해 수동으로 배포 및 구성할 수 있습니다. 관리할 리소스가 몇 개 없는 경우에는 이 방법으로 충분할 수 있습니다. 그러나 클라우드 자산이 증가함에 따라 조직에서는 Azure의 크기 조정, 장애 조치(failover) 및 재해 복구 기능을 활용할 수 있도록 클라우드 리소스 배포를 자동화해야 합니다. DevOps 또는 DevSecOps 접근 방식을 채택하는 것이 배포를 관리하는 가장 좋은 방법인 경우가 자주 있습니다.

견고한 배포 가속 계획이 있으면 클라우드 리소스가 올바르고 일관적으로 배포, 업데이트 및 구성되며 그 상태를 유지합니다. 배포 가속 전략의 완성도는 [Cost Management 전략](../cost-management/overview.md)에서도 중요한 요소로 작동할 수 있습니다. 클라우드 리소스의 프로비전 및 구성을 자동화하면 수요가 적거나 특정 시간에만 수요가 몰리는 경우 리소스 규모를 축소하거나 할당을 취소하여 필요할 때만 리소스 요금을 지불할 수 있습니다.

## <a name="business-risk"></a>비즈니스 위험

배포 가속 분야는 다음과 같은 비즈니스 위험을 해결하려고 시도합니다. 클라우드를 도입할 때 다음 항목을 모니터링하여 각각의 관련성을 확인합니다.

- **서비스 중단**. 예측 가능하고 반복 가능한 배포 프로세스가 없거나 시스템 구성 변경을 적절하게 관리하지 않으면 정상 작업이 중단되어 생산성 손실 또는 비즈니스 손실로 이어질 수 있습니다.
- **비용 초과**. 시스템 리소스 구성이 예기치 않게 변경되면 문제의 근본 원인을 식별하기가 어려워지고 개발, 운영 및 유지 관리 비용이 발생할 수 있습니다.
- **조직의 비효율성**. 개발, 운영 및 보안 팀 간의 장벽은 효율적인 클라우드 기술 도입과 통합 클라우드 거버넌스 모델 개발에 수많은 문제를 일으킬 수 있습니다.

## <a name="next-steps"></a>다음 단계

[클라우드 관리 템플릿](./template.md)을 사용하여 현재 클라우드 도입 계획으로 인해 발생할 가능성이 있는 비즈니스 위험을 문서화합니다.

현실적인 비즈니스 위험을 이해했다면, 그 다음 단계는 회사의 위험 허용 범위와 허용 범위를 모니터링할 지표 및 핵심 메트릭을 문서화하는 것입니다.

> [!div class="nextstepaction"]
> [메트릭, 지표 및 위험 허용 범위](./metrics-tolerance.md)
