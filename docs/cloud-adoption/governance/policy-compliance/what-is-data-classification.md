---
title: 'CAF: 데이터 분류란'
description: 데이터 분류란?
author: BrianBlanchard
ms.date: 2/11/2019
ms.openlocfilehash: 07268e7242d92ac2581bf28b378a3c43d166620c
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901628"
---
<!-- markdownlint-disable MD026 -->

# <a name="what-is-data-classification"></a><span data-ttu-id="bfbd6-103">데이터 분류란 무엇인가요?</span><span class="sxs-lookup"><span data-stu-id="bfbd6-103">What is data classification?</span></span>

<span data-ttu-id="bfbd6-104">이것은 데이터 분류라는 일반적인 주제에 대한 소개 문서입니다.</span><span class="sxs-lookup"><span data-stu-id="bfbd6-104">This is an introductory article on the general topic of Data Classification.</span></span> <span data-ttu-id="bfbd6-105">데이터 분류는 모든 거버넌스에서 매우 일반적인 시작점입니다.</span><span class="sxs-lookup"><span data-stu-id="bfbd6-105">Data classification is a very common starting point for all governance.</span></span>

## <a name="business-risks-and-governance"></a><span data-ttu-id="bfbd6-106">비즈니스 위험과 거버넌스</span><span class="sxs-lookup"><span data-stu-id="bfbd6-106">Business risks and governance</span></span>

<span data-ttu-id="bfbd6-107">대부분의 조직에서 거버넌스에 투자하는 주된 이유는 다음 세 가지 비즈니스 위험을 줄이는 데 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bfbd6-107">In most organizations, the primary reasons for investing in governance can be reduced to three business risks:</span></span>

* <span data-ttu-id="bfbd6-108">데이터 침해와 관련된 책임</span><span class="sxs-lookup"><span data-stu-id="bfbd6-108">Liability associated with data breaches</span></span>
* <span data-ttu-id="bfbd6-109">가동 중단으로 인한 비즈니스 중단</span><span class="sxs-lookup"><span data-stu-id="bfbd6-109">Interruption to the business from outages</span></span>
* <span data-ttu-id="bfbd6-110">계획되지 않거나 예기치 않은 지출</span><span class="sxs-lookup"><span data-stu-id="bfbd6-110">Unplanned or unexpected spending</span></span>

<span data-ttu-id="bfbd6-111">이러한 세 가지 비즈니스 위험에 대한 여러 변형이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bfbd6-111">There are many variants of these three business risks.</span></span> <span data-ttu-id="bfbd6-112">그러나 위 세 가지가 가장 일반적입니다.</span><span class="sxs-lookup"><span data-stu-id="bfbd6-112">However, the tend to be the most common.</span></span>

## <a name="understand-then-mitigate"></a><span data-ttu-id="bfbd6-113">위험 파악 후 완화</span><span class="sxs-lookup"><span data-stu-id="bfbd6-113">Understand then mitigate</span></span>

<span data-ttu-id="bfbd6-114">위험을 완화하기 위해서는 먼저 위험을 파악해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="bfbd6-114">Before any risk can be mitigated, it must be understood.</span></span> <span data-ttu-id="bfbd6-115">데이터 침해 책임의 경우 데이터 분류를 이해하는 것으로 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="bfbd6-115">In the case of data breach liability, that understanding starts with data classification.</span></span> <span data-ttu-id="bfbd6-116">데이터 분류는 메타데이터 특성을 디지털 자산의 모든 자산과 연관시키는 프로세스로, 해당 자산과 관련된 데이터 형식을 식별합니다.</span><span class="sxs-lookup"><span data-stu-id="bfbd6-116">Data classification is the process of associating a meta data characteristic to every asset in a digital estate, which identifies the type of data associated with that asset.</span></span>

<span data-ttu-id="bfbd6-117">Microsoft는 클라우드로 마이그레이션 또는 배포하기 위한 잠재적인 대안으로 식별된 모든 자산을 데이터 분류, 업무상 중요도 및 청구 책임을 기록하기 위해 메타데이터로 문서화해야 한다고 제안합니다.</span><span class="sxs-lookup"><span data-stu-id="bfbd6-117">Microsoft suggests that any asset which has been identified as a potential candidate for migration or deployment to the cloud should have documented meta data to record the data classification, business criticality, and billing responsibility.</span></span> <span data-ttu-id="bfbd6-118">이 세 가지 분류 기준은 위험을 파악하고 완화하는 데 큰 도움이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bfbd6-118">These three points of classification can go a long way to understanding and mitigating risks.</span></span>

## <a name="microsofts-data-classification"></a><span data-ttu-id="bfbd6-119">Microsoft의 데이터 분류</span><span class="sxs-lookup"><span data-stu-id="bfbd6-119">Microsoft's data classification</span></span>

<span data-ttu-id="bfbd6-120">다음은 Microsoft에서 사용하는 분류 목록입니다.</span><span class="sxs-lookup"><span data-stu-id="bfbd6-120">The following is a list of classifications Microsoft uses.</span></span> <span data-ttu-id="bfbd6-121">업계 또는 기존 보안 요구 사항에 따라 데이터 분류 표준이 이미 조직 내에 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bfbd6-121">Depending on your industry or existing security requirements, data classifications standards may already exist within your organization.</span></span> <span data-ttu-id="bfbd6-122">표준이 없으면 이 샘플 분류를 사용하여 디지털 자산 및 위험 프로필을 보다 잘 이해할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bfbd6-122">If no standard exists, we welcome you to use this sample classification, to help you better understand your digital estate and risk profile.</span></span>  

* <span data-ttu-id="bfbd6-123">**업무 외:** Microsoft에 속하지 않는 개인 생활 데이터</span><span class="sxs-lookup"><span data-stu-id="bfbd6-123">**Non-Business:** Data from your personal life that does not belong to Microsoft</span></span>
* <span data-ttu-id="bfbd6-124">**공용:** 공개 사용을 위해 자유롭게 제공 및 승인된 비즈니스 데이터</span><span class="sxs-lookup"><span data-stu-id="bfbd6-124">**Public:** Business data that is freely available and approved for public consumption</span></span>
* <span data-ttu-id="bfbd6-125">**일반:** 대중을 대상으로 하지 않은 비즈니스 데이터</span><span class="sxs-lookup"><span data-stu-id="bfbd6-125">**General:** Business data that is not meant for a public audience</span></span>
* <span data-ttu-id="bfbd6-126">**기밀:** 지나치게 공유할 경우 Microsoft에 피해를 야기할 수 있는 비즈니스 데이터</span><span class="sxs-lookup"><span data-stu-id="bfbd6-126">**Confidential:** Business data that could cause harm to Microsoft if over-shared</span></span>
* <span data-ttu-id="bfbd6-127">**극비:** 지나치게 공유할 경우 Microsoft에 상당한 피해를 야기할 수 있는 비즈니스 데이터</span><span class="sxs-lookup"><span data-stu-id="bfbd6-127">**Highly Confidential:** Business data that would cause extensive harm to Microsoft if over-shared</span></span>

## <a name="tagging-data-classification-in-azure"></a><span data-ttu-id="bfbd6-128">Azure에서 데이터 분류 태그 지정</span><span class="sxs-lookup"><span data-stu-id="bfbd6-128">Tagging data classification in Azure</span></span>

<span data-ttu-id="bfbd6-129">모든 클라우드 공급자는 모든 자산에 대한 메타데이터를 기록하는 메커니즘을 제공해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="bfbd6-129">Every cloud provider should offer a mechanism for recording metadata about any asset.</span></span> <span data-ttu-id="bfbd6-130">메타데이터는 클라우드에서 자산을 관리하는 데 중요합니다.</span><span class="sxs-lookup"><span data-stu-id="bfbd6-130">Metadata is vital to managing assets in the cloud.</span></span> <span data-ttu-id="bfbd6-131">Azure의 경우 리소스 태그는 메타데이터 스토리지에 대해 권장되는 방식입니다.</span><span class="sxs-lookup"><span data-stu-id="bfbd6-131">In the case of Azure, resource tags are the suggested approach for metadata storage.</span></span> <span data-ttu-id="bfbd6-132">Azure에서 리소스 태그 지정에 대한 자세한 내용은 [태그를 사용하여 Azure 리소스 구성](/azure/azure-resource-manager/resource-group-using-tags) 문서를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="bfbd6-132">For additional information on resource tagging in Azure, see the article on [Using tags to organize your Azure resources](/azure/azure-resource-manager/resource-group-using-tags).</span></span>

## <a name="next-steps"></a><span data-ttu-id="bfbd6-133">다음 단계</span><span class="sxs-lookup"><span data-stu-id="bfbd6-133">Next steps</span></span>

<span data-ttu-id="bfbd6-134">실행 가능한 거버넌스 경험 중 하나에 데이터 분류를 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="bfbd6-134">Apply data classifications during one of the actionable governance journeys.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="bfbd6-135">실행 가능한 거버넌스 경험 시작</span><span class="sxs-lookup"><span data-stu-id="bfbd6-135">Begin an Actionable Governance Journeys</span></span>](../journeys/overview.md)
