---
title: 'CAF: 대기업 - 거버넌스 전략의 기반이 되는 초기 기업 정책'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 2/11/2019
description: 대기업 - 거버넌스 전략의 기반이 되는 초기 기업 정책
author: BrianBlanchard
ms.openlocfilehash: d3bc31f95229a82cbc2f6ac6e684a87107783c07
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55902008"
---
# <a name="large-enterprise-initial-corporate-policy-behind-the-governance-strategy"></a><span data-ttu-id="ae35e-103">대기업: 거버넌스 전략의 기반이 되는 초기 기업 정책</span><span class="sxs-lookup"><span data-stu-id="ae35e-103">Large enterprise: Initial corporate policy behind the governance strategy</span></span>

<span data-ttu-id="ae35e-104">아래에서 설명하는 기업 정책은 거버넌스 경험이 시작되는 지점인 초기 거버넌스 위치를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="ae35e-104">The following corporate policy defines the initial governance position, which is the starting point for this journey.</span></span> <span data-ttu-id="ae35e-105">이 문서에서는 초기 단계의 위험, 초기 정책 설명 및 정책 설명 적용을 위한 초기 프로세스의 정의를 제시합니다.</span><span class="sxs-lookup"><span data-stu-id="ae35e-105">This article defines early-stage risks, initial policy statements, and early processes to enforce policy statements.</span></span>

> [!NOTE]
><span data-ttu-id="ae35e-106">기업 정책은 기술 문서는 아니지만 여러 기술 관련 사항을 결정하는 데 기준으로 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="ae35e-106">The corporate policy is not a technical document, but it drives many technical decisions.</span></span> <span data-ttu-id="ae35e-107">[개요](./overview.md)에 설명되어 있는 거버넌스 MVP는 이 정책에서 최종적으로 파생됩니다.</span><span class="sxs-lookup"><span data-stu-id="ae35e-107">The governance MVP described in the [overview](./overview.md) ultimately derives from this policy.</span></span> <span data-ttu-id="ae35e-108">따라서 조직은 거버넌스 MVP를 구현하기 전에 고유한 목표와 비즈니스 위험을 기준으로 하여 기업 정책을 개발해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ae35e-108">Before implementing a governance MVP, your organization should develop a corporate policy based on your own objectives and business risks.</span></span>

## <a name="cloud-governance-team"></a><span data-ttu-id="ae35e-109">클라우드 거버넌스 팀</span><span class="sxs-lookup"><span data-stu-id="ae35e-109">Cloud Governance team</span></span>

<span data-ttu-id="ae35e-110">CIO가 최근 PII 내역 및 중요 업무용 정책을 파악하고 해당 정책을 변경하는 경우의 영향을 검토하기 위해 IT 거버넌스 팀과 회의를 진행했습니다.</span><span class="sxs-lookup"><span data-stu-id="ae35e-110">The CIO recently held a meeting with the IT Governance team to understand the history of the PII and mission-critical policies and review the impact of changing those policies.</span></span> <span data-ttu-id="ae35e-111">이 회의에서 CIO는 IT 팀과 회사 전체에서 클라우드를 활용할 수 있는 가능성도 언급했습니다.</span><span class="sxs-lookup"><span data-stu-id="ae35e-111">She also discussed the overall potential of the cloud for IT and the company.</span></span>

<span data-ttu-id="ae35e-112">회의 후 IT 거버넌스 팀의 두 구성원이 클라우드 계획 작업 조사 및 지원 권한을 요청했습니다.</span><span class="sxs-lookup"><span data-stu-id="ae35e-112">After the meeting, two members of the IT Governance team requested permission to research and support the cloud planning efforts.</span></span> <span data-ttu-id="ae35e-113">거버넌스의 필요성을 알고 있었던 IT 거버넌스 팀 책임자는 클라우드가 섀도 IT를 제한할 수 있는 기회라고 판단하여 이 아이디어를 지원하기로 했습니다.</span><span class="sxs-lookup"><span data-stu-id="ae35e-113">Recognizing the need for governance and an opportunity to limit shadow IT, the Director of IT Governance supported this idea.</span></span> <span data-ttu-id="ae35e-114">그리하여 클라우드 거버넌스 팀이 조직되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ae35e-114">With that, the Cloud Governance team was born.</span></span> <span data-ttu-id="ae35e-115">클라우드 거버넌스 팀은 앞으로 몇 달간 거버넌스 측면에서 클라우드를 파악하는 중에 확인된 여러 가지 실수를 정리하는 작업을 인계받아 처리할 예정입니다.</span><span class="sxs-lookup"><span data-stu-id="ae35e-115">Over the next several months, they will inherit the cleanup of many mistakes made during exploration in the cloud from a governance perspective.</span></span> <span data-ttu-id="ae35e-116">이 과정이 완료되면 클라우드 거버넌스 팀은 클라우드 '보유자'로 지정됩니다.</span><span class="sxs-lookup"><span data-stu-id="ae35e-116">This will earn them the moniker of Cloud Custodians.</span></span> <span data-ttu-id="ae35e-117">이후 개선 과정에서는 이 경험에 시간별로 변경되는 클라우드 거버넌스 팀의 역할이 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="ae35e-117">In later evolutions, this journey will show how their roles change over time.</span></span>

[!INCLUDE [business-risk](../../../../../includes/cloud-adoption/governance/business-risks.md)]

## <a name="tolerance-indicators"></a><span data-ttu-id="ae35e-118">허용 범위 지표</span><span class="sxs-lookup"><span data-stu-id="ae35e-118">Tolerance indicators</span></span>

<span data-ttu-id="ae35e-119">현재 위험 허용 범위는 높고 클라우드 거버넌스 투자 의향은 낮은 상태입니다.</span><span class="sxs-lookup"><span data-stu-id="ae35e-119">The current risk tolerance is high and the appetite for investing in cloud governance is low.</span></span> <span data-ttu-id="ae35e-120">따라서 허용 범위 지표는 시간과 에너지를 더 투자하도록 하는 조기 경보 시스템 역할을 합니다.</span><span class="sxs-lookup"><span data-stu-id="ae35e-120">As such, the tolerance indicators act as an early warning system to trigger the investment of time and energy.</span></span> <span data-ttu-id="ae35e-121">지표에서 다음과 같은 수치가 관찰되면 거버넌스 전략을 개선하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="ae35e-121">If the following indicators are observed, it would be wise to evolve the governance strategy.</span></span>

- <span data-ttu-id="ae35e-122">비용 관리: 클라우드에 대한 배포 범위가 자산 1,000개를 초과하거나 월 지출 비용이 미화 10,000달러를 초과합니다.</span><span class="sxs-lookup"><span data-stu-id="ae35e-122">Cost Management: Scale of deployment exceeds 1,000 assets to the cloud, or monthly spending exceeds $10,000 USD per month.</span></span>
- <span data-ttu-id="ae35e-123">ID 기준: 레거시 또는 타사 MFA(Multi-Factor Authentication) 요구 사항이 적용되는 애플리케이션이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="ae35e-123">Identity Baseline: Inclusion of applications with legacy or third-party multifactor authentication (MFA) requirements.</span></span>
- <span data-ttu-id="ae35e-124">보안 기준: 정의된 클라우드 채택 계획에 보호된 데이터가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="ae35e-124">Security Baseline: Inclusion of protected data in defined cloud adoption plans.</span></span>
- <span data-ttu-id="ae35e-125">리소스 일관성: 정의된 클라우드 채택 계획에 중요 업무용 애플리케이션이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="ae35e-125">Resource Consistency: Inclusion of any mission-critical applications in defined cloud adoption plans.</span></span>

[!INCLUDE [policy-statements](../../../../../includes/cloud-adoption/governance/policy-statements.md)]

## <a name="next-steps"></a><span data-ttu-id="ae35e-126">다음 단계</span><span class="sxs-lookup"><span data-stu-id="ae35e-126">Next steps</span></span>

<span data-ttu-id="ae35e-127">클라우드 거버넌스 팀은 이 기업 정책을 통해 클라우드 채택의 기반이 되는 거버넌스 MVP 구현을 준비할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ae35e-127">This corporate policy prepares the Cloud Governance team to implement the governance MVP, which will be the foundation for adoption.</span></span> <span data-ttu-id="ae35e-128">다음 단계에서 이 MVP를 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="ae35e-128">The next step is to implement this MVP.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ae35e-129">모범 사례 설명</span><span class="sxs-lookup"><span data-stu-id="ae35e-129">Best practice explained</span></span>](./best-practice-explained.md)