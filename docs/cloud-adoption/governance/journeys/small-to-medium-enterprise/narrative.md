---
title: 'CAF: 중소기업 - 거버넌스 전략의 기반이 되는 초기 설명'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 중소기업 거버넌스 경험에 해당하는 사용 사례를 제시하는 설명을 소개합니다.
author: BrianBlanchard
ms.openlocfilehash: 6173b01f310169017761110d6641acfa51d45df8
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55902208"
---
# <a name="small-to-medium-enterprise-the-narrative-behind-the-governance-strategy"></a><span data-ttu-id="15fdd-103">중소기업: 거버넌스 전략의 기반이 되는 설명</span><span class="sxs-lookup"><span data-stu-id="15fdd-103">Small-to-medium enterprise: The narrative behind the governance strategy</span></span>

<span data-ttu-id="15fdd-104">아래의 설명은 [중소기업 거버넌스 경험](./overview.md)에 해당하는 사용 사례를 제시합니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-104">The following narrative describes the use case for the [small-to-medium enterprise governance journey](./overview.md).</span></span> <span data-ttu-id="15fdd-105">해당 경험을 구현하기 전에 이 설명에 반영된 가정과 추론 방식을 파악해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-105">Before implementing the journey, it’s important to understand the assumptions and reasoning that are reflected in this narrative.</span></span> <span data-ttu-id="15fdd-106">그리고 나면 거버넌스 전략을 더 효율적으로 조직의 경험에 맞게 조정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-106">Then you can better align the governance strategy to your own organization’s journey.</span></span>

## <a name="back-story"></a><span data-ttu-id="15fdd-107">배경 정보</span><span class="sxs-lookup"><span data-stu-id="15fdd-107">Back story</span></span>

<span data-ttu-id="15fdd-108">이사회에서 연초에 다양한 방식을 통해 사업을 적극적으로 추진하기 위한 계획을 세웠습니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-108">The board of directors started the year with plans to energize the business in several ways.</span></span> <span data-ttu-id="15fdd-109">이러한 계획에 따라 관리자들에게 사용자 환경을 개선하여 시장 점유율을 높일 것을 요구하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-109">They are pushing leadership to improve customer experiences to gain market share.</span></span> <span data-ttu-id="15fdd-110">또한 업계를 선도하는 업체로 자리매김하기 위해 신제품과 서비스도 적극 홍보하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-110">They are also pushing for new products and services that will position the company as a thought leader in the industry.</span></span> <span data-ttu-id="15fdd-111">그와 동시에 폐기물을 줄이고 불필요한 비용을 절감하기 위한 노력도 시작했습니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-111">They also initiated a parallel effort to reduce waste and cut unnecessary costs.</span></span> <span data-ttu-id="15fdd-112">이사회와 관리자의 이러한 조치는 다소 부담스럽기는 하지만 향후의 회사 성장에 자본이 최대한 집중 투자되고 있음을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-112">Though intimidating, the actions of the board and leadership show that this effort is focusing as much capital as possible on future growth.</span></span>

<span data-ttu-id="15fdd-113">이전에는 이러한 전략 관련 논의에서 회사 CIO가 배제되었습니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-113">In the past, the company’s CIO has been excluded from these strategic conversations.</span></span> <span data-ttu-id="15fdd-114">그러나 향후의 회사 비전은 기술적 성장과 본질적으로 연결되므로, 이제는 IT 관리자도 전사적 계획을 지원하기 위해 해당 논의에 참여하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-114">However, because the future vision is intrinsically linked to technical growth, IT has a seat at the table to help guide these big plans.</span></span> <span data-ttu-id="15fdd-115">즉, 이제는 IT 서비스를 새로운 방식으로 제공해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-115">IT is now expected to deliver in new ways.</span></span> <span data-ttu-id="15fdd-116">IT 팀은 이러한 변화를 수용할 준비가 되어 있지 않으므로 새로운 방식을 학습하는 과정에서 어려움을 겪을 가능성이 높습니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-116">The team isn’t really prepared for these changes and is likely to struggle with the learning curve.</span></span>

## <a name="business-characteristics"></a><span data-ttu-id="15fdd-117">기업 특성</span><span class="sxs-lookup"><span data-stu-id="15fdd-117">Business characteristics</span></span>

<span data-ttu-id="15fdd-118">이 회사의 비즈니스 프로필은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-118">The company has the following business profile:</span></span>

- <span data-ttu-id="15fdd-119">모든 영업 및 운영 부서가 한 국가에 있으며 기타 국가의 고객 비율은 낮은 편입니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-119">All sales and operations reside in a single country, with a low percentage of global customers.</span></span>
- <span data-ttu-id="15fdd-120">사업은 단일 사업부로 운영되며 영업/마케팅/운영/IT 등의 기능별로 예산이 책정됩니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-120">The business operates as a single business unit, with budget aligned to functions, including Sales, Marketing, Operations, and IT.</span></span>
- <span data-ttu-id="15fdd-121">대부분의 IT 업무는 자본 낭비나 비용 센터로 간주됩니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-121">The business views most of IT as a capital drain or a cost center.</span></span>

## <a name="current-state"></a><span data-ttu-id="15fdd-122">현재 상태</span><span class="sxs-lookup"><span data-stu-id="15fdd-122">Current state</span></span>

<span data-ttu-id="15fdd-123">이 회사의 현재 IT 및 클라우드 운영 상태는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-123">Here is the current state of the company’s IT and cloud operations:</span></span>

- <span data-ttu-id="15fdd-124">IT는 호스트된 인프라 환경 2개에서 운영됩니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-124">IT operates two hosted infrastructure environments.</span></span> <span data-ttu-id="15fdd-125">환경 하나에는 프로덕션 자산이 포함되어 있고</span><span class="sxs-lookup"><span data-stu-id="15fdd-125">One environment contains production assets.</span></span> <span data-ttu-id="15fdd-126">두 번째 환경에는 재해 복구 및 일부 개발/테스트 자산이 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-126">The second environment contains disaster recovery and some dev/test assets.</span></span> <span data-ttu-id="15fdd-127">이러한 환경은 서로 다른 두 공급자를 통해 호스트됩니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-127">These environments are hosted by two different providers.</span></span> <span data-ttu-id="15fdd-128">IT 팀은 이 두 데이터 센터를 각각 Prod/DR로 지칭합니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-128">IT refers to these two datacenters as Prod and DR respectively.</span></span>
- <span data-ttu-id="15fdd-129">IT 팀은 모든 최종 사용자 이메일 계정을 Office 365로 마이그레이션하는 방식으로 클라우드를 처음 채택했습니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-129">IT entered the cloud by migrating all end-user email accounts to Office 365.</span></span> <span data-ttu-id="15fdd-130">이 마이그레이션은 6개월 전에 완료되었습니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-130">This migration was completed six months ago.</span></span> <span data-ttu-id="15fdd-131">그 외의 IT 자산 몇 가지도 클라우드에 배포되었습니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-131">Few other IT assets have been deployed to the cloud.</span></span>
- <span data-ttu-id="15fdd-132">애플리케이션 개발 팀은 개발/테스트 환경에서 클라우드의 기본 기능을 익히고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-132">The application development teams are working in a dev/test capacity to learn about cloud native capabilities.</span></span>
- <span data-ttu-id="15fdd-133">BI(비즈니스 인텔리전스) 팀은 클라우드에서 빅 데이터를 사용하여, 그리고 새 플랫폼에서 데이터 큐레이션을 실험해 보고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-133">The business intelligence (BI) team is experimenting with big data in the cloud and curation of data on new platforms.</span></span>
- <span data-ttu-id="15fdd-134">이 회사에는 고객 PII(개인 식별이 가능한 정보) 및 금융 데이터는 클라우드에서 호스트할 수 없음을 규정하는 대략적으로 정의된 정책이 있습니다. 이 정책은 현재 배포에서 중요 업무용 애플리케이션을 제한합니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-134">The company has a loosely defined policy stating that customer personally identifiable information (PII) and financial data cannot be hosted in the cloud, which limits mission-critical applications in the current deployments.</span></span>
- <span data-ttu-id="15fdd-135">IT 투자는 대개 CapEx(기업비)를 통해 제어됩니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-135">IT investments are controlled largely by capital expense (CapEx).</span></span> <span data-ttu-id="15fdd-136">이러한 투자는 연 단위로 계획됩니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-136">Those investments are planned yearly.</span></span> <span data-ttu-id="15fdd-137">지난 몇 년 동안은 기본 유지 관리 요구 사항보다 약간 더 많은 비용이 투자되었습니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-137">In the past several years, investments have included little more than basic maintenance requirements.</span></span>

## <a name="future-state"></a><span data-ttu-id="15fdd-138">향후 상태</span><span class="sxs-lookup"><span data-stu-id="15fdd-138">Future state</span></span>

<span data-ttu-id="15fdd-139">이후 몇 년 동안 예상되는 변화는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-139">The following changes are anticipated over the next several years:</span></span>

- <span data-ttu-id="15fdd-140">CIO가 향후 상태 목표 달성을 위해 PII 및 금융 데이터 관련 정책을 검토하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-140">The CIO is reviewing the policy on PII and financial data to allow for the future state goals.</span></span>
- <span data-ttu-id="15fdd-141">애플리케이션 개발 및 BI 팀은 신제품 및 고객 참여 비전을 토대로 하여 향후 24개월 동안 프로덕션 환경에 클라우드 기반 솔루션을 릴리스하고자 합니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-141">The application development and BI teams want to release cloud-based solutions to production over the next 24 months based on the vision for customer engagement and new products.</span></span>
- <span data-ttu-id="15fdd-142">올해 IT 팀은 VM 2,000대를 클라우드로 마이그레이션하여 DR 데이터 센터의 재해 복구 워크로드 사용 중지를 완료할 예정입니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-142">This year, the IT team will finish retiring the disaster recovery workloads of the DR datacenter by migrating 2,000 VMs to the cloud.</span></span> <span data-ttu-id="15fdd-143">그러면 향후 5년 동안 미화 2,500만 달러의 비용을 절약할 수 있을 것으로 예상됩니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-143">This is expected to produce an estimated $25M USD cost savings over the next five years.</span></span>
    <span data-ttu-id="15fdd-144">![향후 5년 동안 미화 2,500만 달러의 비용 절감 효과를 보여 주는 온-프레미스 비용과 Azure 비용 비교 그래프](../../../_images/governance/calculator-small-to-medium-enterprise.png)</span><span class="sxs-lookup"><span data-stu-id="15fdd-144">![On-premise costs versus Azure costs demonstrating a return of $25M USD over the next five years](../../../_images/governance/calculator-small-to-medium-enterprise.png)</span></span>
- <span data-ttu-id="15fdd-145">이 회사는 집행된 기업비를 IT 내의 OpEx(운영 비용)로 전환함으로써 IT 투자 방식을 변경할 계획입니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-145">The company plans to change how it makes IT investments by repositioning the committed CapEx as an operational expense (OpEx) within IT.</span></span> <span data-ttu-id="15fdd-146">이 변경으로 인해 비용을 더욱 세부적으로 제어할 수 있으며, IT 팀은 계획된 기타 작업을 신속하게 진행할 수 있게 될 것입니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-146">This change will provide greater cost control and enable IT to accelerate other planned efforts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="15fdd-147">다음 단계</span><span class="sxs-lookup"><span data-stu-id="15fdd-147">Next steps</span></span>

<span data-ttu-id="15fdd-148">이 회사는 거버넌스 구현을 작성하기 위한 기업 정책을 개발했습니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-148">The company has developed a corporate policy to shape the governance implementation.</span></span> <span data-ttu-id="15fdd-149">이 기업 정책에 따라 대부분의 기술적 사항을 결정합니다.</span><span class="sxs-lookup"><span data-stu-id="15fdd-149">The corporate policy drives many of the technical decisions.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="15fdd-150">초기 기업 정책 검토</span><span class="sxs-lookup"><span data-stu-id="15fdd-150">Review the initial corporate policy</span></span>](./initial-corporate-policy.md)
