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
# <a name="architectural-decision-guides"></a><span data-ttu-id="6579f-103">아키텍처 의사 결정 가이드</span><span class="sxs-lookup"><span data-stu-id="6579f-103">Architectural decision guides</span></span>

<span data-ttu-id="6579f-104">클라우드 채택 프레임워크의 아키텍처 의사 결정 가이드는 클라우드 거버넌스 설계 지침을 만들 때 도움이 되는 패턴과 모델에 대해 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="6579f-104">The architectural decision guides in the Cloud Adoption Framework describe patterns and models that help when creating cloud governance design guidance.</span></span> <span data-ttu-id="6579f-105">각 의사 결정 가이드에는 클라우드 배포의 핵심 인프라 구성 요소에 중점을 두고, 특정 클라우드 배포 시나리오를 지원하기 위한 잠재적 패턴 또는 모델이 나와 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6579f-105">Each decision guide focuses on one core infrastructure component of cloud deployments and lists potential patterns or models intended to support specific cloud deployment scenarios.</span></span>

<span data-ttu-id="6579f-106">조직에 대한 클라우드 거버넌스를 설정하려면 실행 가능한 거버넌스 경험에서 기준 로드맵을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="6579f-106">When you begin to establish cloud governance for your organization,  actionable governance journeys provide a baseline roadmap.</span></span> <span data-ttu-id="6579f-107">그러나 이러한 경험은 반영하지 못할 수 있는 조직의 요구 사항과 우선 순위에 대해 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="6579f-107">However, these journeys make assumptions about requirements and priorities that may not reflect those of your organization.</span></span>
<span data-ttu-id="6579f-108">이러한 의사 결정 가이드는 예제 설계 지침에서 선택한 아키텍처 설계 항목을 사용자 고유의 요구 사항에 맞추는 데 도움이 되는 대체 패턴과 모델을 제공하여 거버넌스 경험 샘플을 보완합니다.</span><span class="sxs-lookup"><span data-stu-id="6579f-108">These decision guides supplement the sample governance journeys by providing alternative patterns and models that help you align the architectural design choices made in the example design guidance with your own requirements.</span></span>

## <a name="design-guidance-categories"></a><span data-ttu-id="6579f-109">설계 지침 범주</span><span class="sxs-lookup"><span data-stu-id="6579f-109">Design guidance categories</span></span>

<span data-ttu-id="6579f-110">다음 각 범주는 모든 클라우드 배포의 기본 기술을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="6579f-110">Each of the following categories represents a foundational technology of all cloud deployments.</span></span> <span data-ttu-id="6579f-111">요구 사항과 일치하지 않는 설계 경험 예제의 각 선택 항목에 대해 관련된 아래 섹션의 옵션을 검토하여 요구 사항에 더 적합한 패턴 또는 모델을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="6579f-111">For each choice in the example design journeys that doesn’t match your requirements, examine the options in the relevant section below to choose a pattern or model better suited to your needs.</span></span>

<span data-ttu-id="6579f-112">[구독](./subscriptions/overview.md): 클라우드 배포의 구독 설계 및 계정 구조를 조직의 소유권, 청구 및 관리 기능에 맞게 계획합니다.</span><span class="sxs-lookup"><span data-stu-id="6579f-112">[Subscriptions](./subscriptions/overview.md): Plan your cloud deployment's subscription design and account structure to match your organization's ownership, billing, and management capabilities.</span></span>

<span data-ttu-id="6579f-113">[ID](./identity/overview.md): 클라우드 기반 ID 서비스를 기존 ID 리소스와 통합하여 IT 환경 내의 액세스 제어를 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="6579f-113">[Identity](./identity/overview.md): Integrate cloud-based identity services with your existing identity resources to support access control within your IT environment.</span></span>

<span data-ttu-id="6579f-114">[정책 적용](./policy-enforcement/overview.md): 클라우드에 배포하는 리소스와 워크로드에 대한 조직 정책 규칙을 정의하고 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="6579f-114">[Policy Enforcement](./policy-enforcement/overview.md): Define and enforce organizational policy rules for resources and workloads that you deploy to the cloud.</span></span>

<span data-ttu-id="6579f-115">[리소스 일관성](./resource-consistency/overview.md): 리소스 관리 및 정책 요구 사항을 적용하도록 클라우드 기반 리소스의 배포와 구조를 조정합니다.</span><span class="sxs-lookup"><span data-stu-id="6579f-115">[Resource Consistency](./resource-consistency/overview.md): Ensure that deployment and organization of your cloud-based resources align to enforce resource management and policy requirements.</span></span>

<span data-ttu-id="6579f-116">[리소스 태그 지정](./resource-tagging/overview.md): 청구 모델, 클라우드 계정 방식, 관리, 액세스 제어 및 운영 효율성을 지원하도록 클라우드 기반 리소스를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="6579f-116">[Resource Tagging](./resource-tagging/overview.md): Organize your cloud-based resources to support billing models, cloud accounting approaches, management, access control, and operational efficiency.</span></span> <span data-ttu-id="6579f-117">리소스 태그 지정에는 일관되고 잘 구성된 명명 및 메타데이터 체계가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="6579f-117">Resource tagging requires a consistent and well-organized naming and metadata scheme.</span></span>

<span data-ttu-id="6579f-118">[소프트웨어 정의 네트워크](./software-defined-network/overview.md): 가상화된 네트워킹 기능을 빠르게 배포하고 수정하여 보안 워크로드를 클라우드에 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="6579f-118">[Software Defined Networks](./software-defined-network/overview.md): Deploy secure workloads to the cloud using rapid deployment and modification of virtualized networking capabilities.</span></span> <span data-ttu-id="6579f-119">SDN(소프트웨어 정의 네트워크)은 민첩한 워크플로를 지원하고, 리소스를 격리하며, 클라우드 기반 시스템을 기존 IT 인프라와 통합할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6579f-119">Software-defined networks (SDNs) can support agile workflows, isolate resources, and integrate cloud-based systems with your existing IT infrastructure.</span></span>

<span data-ttu-id="6579f-120">[암호화](./encryption/overview.md): 클라우드 배포 내에서 보안의 중요한 측면인 암호화를 사용하여 중요한 데이터를 보호합니다.</span><span class="sxs-lookup"><span data-stu-id="6579f-120">[Encryption](./encryption/overview.md): Secure your sensitive data using encryption, an important aspect of security within a cloud deployment.</span></span>

<span data-ttu-id="6579f-121">[로그 및 보고](./log-and-report/overview.md): 클라우드 기반 리소스에서 생성된 로그 데이터를 모니터링합니다.</span><span class="sxs-lookup"><span data-stu-id="6579f-121">[Logs and Reporting](./log-and-report/overview.md): Monitor log data generated by cloud-based resources.</span></span> <span data-ttu-id="6579f-122">데이터 분석은 워크로드의 운영, 유지 관리 및 정책 적용 상태에 대한 상태 관련 인사이트를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="6579f-122">Analyzing data provides health-related insights into the operations, maintenance, and policy enforcement status of workloads.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6579f-123">다음 단계</span><span class="sxs-lookup"><span data-stu-id="6579f-123">Next steps</span></span>

<span data-ttu-id="6579f-124">구독과 계정에서 클라우드 배포의 기반 역할을 수행하는 방법에 대해 알아봅니다.</span><span class="sxs-lookup"><span data-stu-id="6579f-124">Learn how subscriptions and accounts serve as the base of a cloud deployment.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="6579f-125">구독 설계</span><span class="sxs-lookup"><span data-stu-id="6579f-125">Subscriptions design</span></span>](subscriptions/overview.md)
