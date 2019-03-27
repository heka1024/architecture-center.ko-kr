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
# <a name="deployment-acceleration-motivations-and-business-risks"></a><span data-ttu-id="e8ea9-103">배포 가속 동기 부여 및 비즈니스 위험</span><span class="sxs-lookup"><span data-stu-id="e8ea9-103">Deployment Acceleration motivations and business risks</span></span>

<span data-ttu-id="e8ea9-104">이 문서에서는 고객이 일반적으로 클라우드 거버넌스 전략 내에 배포 가속화 분야를 도입하는 이유를 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="e8ea9-104">This article discusses the reasons that customers typically adopt a Deployment Acceleration discipline within a cloud governance strategy.</span></span> <span data-ttu-id="e8ea9-105">정책 설명이 필요한 비즈니스 위험의 몇 가지 예제도 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="e8ea9-105">It also provides a few examples of business risks that drive policy statements.</span></span>

<!-- markdownlint-disable MD026 -->

## <a name="is-deployment-acceleration-relevant"></a><span data-ttu-id="e8ea9-106">배포 가속이 적절합니까?</span><span class="sxs-lookup"><span data-stu-id="e8ea9-106">Is Deployment Acceleration relevant?</span></span>

<span data-ttu-id="e8ea9-107">온-프레미스 시스템은 종종 기준 이미지 또는 설치 스크립트를 사용하여 배포됩니다.</span><span class="sxs-lookup"><span data-stu-id="e8ea9-107">On-premises systems are often deployed using baseline images or installation scripts.</span></span> <span data-ttu-id="e8ea9-108">여러 단계를 수행해야 하거나 사람이 개입해야 하는 추가 구성은 일반적으로 불필요합니다.</span><span class="sxs-lookup"><span data-stu-id="e8ea9-108">Additional configuration is usually necessary, which may involve multiple steps or human intervention.</span></span> <span data-ttu-id="e8ea9-109">이러한 수동 프로세스는 오류가 발행하기 쉽고 종종 "구성 드리프트"로 연결되며, 문제 해결 및 수정 작업에 오랜 시간이 걸립니다.</span><span class="sxs-lookup"><span data-stu-id="e8ea9-109">These manual processes are error-prone and often result in "configuration drift", requiring time-consuming troubleshooting and remediation tasks.</span></span>

<span data-ttu-id="e8ea9-110">대부분의 Azure 리소스는 Azure Portal을 통해 수동으로 배포 및 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e8ea9-110">Most Azure resources can be deployed and configured manually via the Azure portal.</span></span> <span data-ttu-id="e8ea9-111">관리할 리소스가 몇 개 없는 경우에는 이 방법으로 충분할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e8ea9-111">This approach may be sufficient for your needs when only have a few resources to manage.</span></span> <span data-ttu-id="e8ea9-112">그러나 클라우드 자산이 증가함에 따라 조직에서는 Azure의 크기 조정, 장애 조치(failover) 및 재해 복구 기능을 활용할 수 있도록 클라우드 리소스 배포를 자동화해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e8ea9-112">However, as your cloud estate grows, your organization should automate the deployment of your cloud resources to take advantage of the scaling, failover, and disaster recovery capabilities that Azure provides.</span></span> <span data-ttu-id="e8ea9-113">DevOps 또는 DevSecOps 접근 방식을 채택하는 것이 배포를 관리하는 가장 좋은 방법인 경우가 자주 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e8ea9-113">Adopting a DevOps or DevSecOps approach is often the best way to manage your deployments.</span></span>

<span data-ttu-id="e8ea9-114">견고한 배포 가속 계획이 있으면 클라우드 리소스가 올바르고 일관적으로 배포, 업데이트 및 구성되며 그 상태를 유지합니다.</span><span class="sxs-lookup"><span data-stu-id="e8ea9-114">A robust Deployment Acceleration plan ensures that your cloud resources are deployed, updated, and configured correctly and consistently, and remain that way.</span></span> <span data-ttu-id="e8ea9-115">배포 가속 전략의 완성도는 [Cost Management 전략](../cost-management/overview.md)에서도 중요한 요소로 작동할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e8ea9-115">The maturity of your Deployment Acceleration strategy can also be a significant factor in your [Cost Management strategy](../cost-management/overview.md).</span></span> <span data-ttu-id="e8ea9-116">클라우드 리소스의 프로비전 및 구성을 자동화하면 수요가 적거나 특정 시간에만 수요가 몰리는 경우 리소스 규모를 축소하거나 할당을 취소하여 필요할 때만 리소스 요금을 지불할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e8ea9-116">Automated provisioning and configuration of your cloud resources allows you to scale down or deallocate resources when demand is low or time-bound, so you only pay for resources as you need them.</span></span>

## <a name="business-risk"></a><span data-ttu-id="e8ea9-117">비즈니스 위험</span><span class="sxs-lookup"><span data-stu-id="e8ea9-117">Business risk</span></span>

<span data-ttu-id="e8ea9-118">배포 가속 분야는 다음과 같은 비즈니스 위험을 해결하려고 시도합니다.</span><span class="sxs-lookup"><span data-stu-id="e8ea9-118">The Deployment Acceleration discipline attempts to address the following business risks.</span></span> <span data-ttu-id="e8ea9-119">클라우드를 도입할 때 다음 항목을 모니터링하여 각각의 관련성을 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="e8ea9-119">During cloud adoption, monitor each of the following for relevance:</span></span>

- <span data-ttu-id="e8ea9-120">**서비스 중단**.</span><span class="sxs-lookup"><span data-stu-id="e8ea9-120">**Service disruption**.</span></span> <span data-ttu-id="e8ea9-121">예측 가능하고 반복 가능한 배포 프로세스가 없거나 시스템 구성 변경을 적절하게 관리하지 않으면 정상 작업이 중단되어 생산성 손실 또는 비즈니스 손실로 이어질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e8ea9-121">Lack of predictable repeatable deployment processes or unmanaged changes to system configurations can disrupt normal operations and can result in lost productivity or lost business.</span></span>
- <span data-ttu-id="e8ea9-122">**비용 초과**.</span><span class="sxs-lookup"><span data-stu-id="e8ea9-122">**Cost overruns**.</span></span> <span data-ttu-id="e8ea9-123">시스템 리소스 구성이 예기치 않게 변경되면 문제의 근본 원인을 식별하기가 어려워지고 개발, 운영 및 유지 관리 비용이 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e8ea9-123">Unexpected changes in configuration of system resources can make identifying root cause of issues more difficult, raising the costs of development, operations, and maintenance.</span></span>
- <span data-ttu-id="e8ea9-124">**조직의 비효율성**.</span><span class="sxs-lookup"><span data-stu-id="e8ea9-124">**Organizational inefficiencies**.</span></span> <span data-ttu-id="e8ea9-125">개발, 운영 및 보안 팀 간의 장벽은 효율적인 클라우드 기술 도입과 통합 클라우드 거버넌스 모델 개발에 수많은 문제를 일으킬 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e8ea9-125">Barriers between development, operations, and security teams can cause numerous challenges to effective adoption of cloud technologies and the development of a unified cloud governance model.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e8ea9-126">다음 단계</span><span class="sxs-lookup"><span data-stu-id="e8ea9-126">Next steps</span></span>

<span data-ttu-id="e8ea9-127">[클라우드 관리 템플릿](./template.md)을 사용하여 현재 클라우드 도입 계획으로 인해 발생할 가능성이 있는 비즈니스 위험을 문서화합니다.</span><span class="sxs-lookup"><span data-stu-id="e8ea9-127">Using the [Cloud Management Template](./template.md), document business risks that are likely to be introduced by the current cloud adoption plan.</span></span>

<span data-ttu-id="e8ea9-128">현실적인 비즈니스 위험을 이해했다면, 그 다음 단계는 회사의 위험 허용 범위와 허용 범위를 모니터링할 지표 및 핵심 메트릭을 문서화하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="e8ea9-128">Once an understanding of realistic business risks is established, the next step is to document the business's tolerance for risk and the indicators and key metrics to monitor that tolerance.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e8ea9-129">메트릭, 지표 및 위험 허용 범위</span><span class="sxs-lookup"><span data-stu-id="e8ea9-129">Metrics, indicators, and risk tolerance</span></span>](./metrics-tolerance.md)
