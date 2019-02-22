---
title: 'CAF: 중소기업 - 거버넌스 전략의 기반이 되는 초기 기업 정책'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 2/11/2019
description: 중소기업 - 거버넌스 전략의 기반이 되는 초기 기업 정책
author: BrianBlanchard
ms.openlocfilehash: 4f49d0aae2c6ab5a5c8feba669cbbb904af2773b
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901362"
---
# <a name="small-to-medium-enterprise-initial-corporate-policy-behind-the-governance-strategy"></a><span data-ttu-id="0d768-103">중소기업: 거버넌스 전략의 기반이 되는 초기 기업 정책</span><span class="sxs-lookup"><span data-stu-id="0d768-103">Small-to-medium enterprise: Initial corporate policy behind the governance strategy</span></span>

<span data-ttu-id="0d768-104">아래에서 설명하는 기업 정책은 거버넌스 경험이 시작되는 지점인 초기 거버넌스 위치를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="0d768-104">The following corporate policy defines an initial governance position, which is the starting point for this journey.</span></span> <span data-ttu-id="0d768-105">이 문서에서는 초기 단계의 위험, 초기 정책 설명 및 정책 설명 적용을 위한 초기 프로세스의 정의를 제시합니다.</span><span class="sxs-lookup"><span data-stu-id="0d768-105">This article defines early-stage risks, initial policy statements, and early processes to enforce policy statements.</span></span>

> [!NOTE]
><span data-ttu-id="0d768-106">기업 정책은 기술 문서는 아니지만 여러 기술 관련 사항을 결정하는 데 기준으로 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="0d768-106">The corporate policy is not a technical document, but it drives many technical decisions.</span></span> <span data-ttu-id="0d768-107">[개요](./overview.md)에 설명되어 있는 거버넌스 MVP는 이 정책에서 최종적으로 파생됩니다.</span><span class="sxs-lookup"><span data-stu-id="0d768-107">The governance MVP described in the [overview](./overview.md) ultimately derives from this policy.</span></span> <span data-ttu-id="0d768-108">따라서 조직은 거버넌스 MVP를 구현하기 전에 고유한 목표와 비즈니스 위험을 기준으로 하여 기업 정책을 개발해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0d768-108">Before implementing a governance MVP, your organization should develop a corporate policy based on your own objectives and business risks.</span></span>

## <a name="cloud-governance-team"></a><span data-ttu-id="0d768-109">클라우드 거버넌스 팀</span><span class="sxs-lookup"><span data-stu-id="0d768-109">Cloud Governance team</span></span>

<span data-ttu-id="0d768-110">이 문서의 설명에서 클라우드 거버넌스 팀은 거버넌스의 필요성을 인지한 시스템 관리자 두 명으로 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="0d768-110">In this narrative, the Cloud Governance team is comprised of two systems administrators who have recognized the need for governance.</span></span> <span data-ttu-id="0d768-111">이 두 관리자는 '클라우드 보유자' 직책을 맡아 앞으로 몇 달 동안 회사의 클라우드 현재 상태 거버넌스를 정리하는 작업을 인계받아 진행할 예정입니다.</span><span class="sxs-lookup"><span data-stu-id="0d768-111">Over the next several months, they will inherit the job of cleaning up the governance of the company’s cloud presence, earning them the title of Cloud Custodians.</span></span> <span data-ttu-id="0d768-112">이후 거버넌스 경험이 개선됨에 따라 이 직함은 변경될 가능성이 높습니다.</span><span class="sxs-lookup"><span data-stu-id="0d768-112">In subsequent evolutions, this title will likely change.</span></span>

[!INCLUDE [business-risk](../../../../../includes/cloud-adoption/governance/business-risks.md)]

## <a name="tolerance-indicators"></a><span data-ttu-id="0d768-113">허용 범위 지표</span><span class="sxs-lookup"><span data-stu-id="0d768-113">Tolerance indicators</span></span>

<span data-ttu-id="0d768-114">현재 위험 허용 범위는 높고 클라우드 거버넌스 투자 의향은 낮은 상태입니다.</span><span class="sxs-lookup"><span data-stu-id="0d768-114">The current tolerance for risk is high and the appetite for investing in cloud governance is low.</span></span> <span data-ttu-id="0d768-115">따라서 허용 범위 지표는 시간과 에너지를 더 투자하도록 하는 조기 경보 시스템 역할을 합니다.</span><span class="sxs-lookup"><span data-stu-id="0d768-115">As such, the tolerance indicators act as an early warning system to trigger more investment of time and energy.</span></span> <span data-ttu-id="0d768-116">지표에서 다음과 같은 수치가 관찰되면 거버넌스 전략을 개선해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0d768-116">If and when the following indicators are observed, you should evolve the governance strategy.</span></span>

- <span data-ttu-id="0d768-117">비용 관리: 클라우드에 대한 배포 범위가 자산 100개를 초과하거나 월 지출 비용이 미화 1,000달러를 초과합니다.</span><span class="sxs-lookup"><span data-stu-id="0d768-117">Cost Management: The scale of deployment exceeds 100 assets to the cloud, or monthly spending exceeds $1,000 USD per month.</span></span>
- <span data-ttu-id="0d768-118">보안 기준: 정의된 클라우드 채택 계획에 보호된 데이터가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="0d768-118">Security Baseline: Inclusion of protected data in defined cloud adoption plans.</span></span>
- <span data-ttu-id="0d768-119">리소스 일관성: 정의된 클라우드 채택 계획에 중요 업무용 애플리케이션이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="0d768-119">Resource Consistency: Inclusion of any mission-critical applications in defined cloud adoption plans.</span></span>

[!INCLUDE [policy-statements](../../../../../includes/cloud-adoption/governance/policy-statements.md)]

## <a name="next-steps"></a><span data-ttu-id="0d768-120">다음 단계</span><span class="sxs-lookup"><span data-stu-id="0d768-120">Next steps</span></span>

<span data-ttu-id="0d768-121">클라우드 거버넌스 팀은 이 기업 정책을 통해 클라우드 채택의 기반이 되는 거버넌스 MVP 구현을 준비할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0d768-121">This corporate policy prepares the Cloud Governance team to implement the governance MVP, which will be the foundation for adoption.</span></span> <span data-ttu-id="0d768-122">다음 단계에서 이 MVP를 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="0d768-122">The next step is to implement this MVP.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="0d768-123">모범 사례 설명</span><span class="sxs-lookup"><span data-stu-id="0d768-123">Best practice explained</span></span>](./best-practice-explained.md)