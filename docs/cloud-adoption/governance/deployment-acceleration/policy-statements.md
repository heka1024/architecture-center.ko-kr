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
# <a name="deployment-acceleration-sample-policy-statements"></a><span data-ttu-id="5a79a-103">배포 가속화 샘플 정책 설명</span><span class="sxs-lookup"><span data-stu-id="5a79a-103">Deployment Acceleration sample policy statements</span></span>

<span data-ttu-id="5a79a-104">개별 클라우드 정책 설명은 위험 평가 프로세스 중에 식별된 특정 위험을 처리하기 위한 지침입니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-104">Individual cloud policy statements are guidelines for addressing specific risks identified during your risk assessment process.</span></span> <span data-ttu-id="5a79a-105">이러한 설명에서는 위험과 이를 처리하기 위한 계획에 대한 간략한 요약을 제공해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-105">These statements should provide a concise summary of risks and plans to deal with them.</span></span> <span data-ttu-id="5a79a-106">각 설명 정의에 포함되는 정보는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-106">Each statement definition should include these pieces of information:</span></span>

- <span data-ttu-id="5a79a-107">**기술 위험**.</span><span class="sxs-lookup"><span data-stu-id="5a79a-107">**Technical risk.**</span></span> <span data-ttu-id="5a79a-108">이 정책이 처리할 위험의 요약입니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-108">A summary of the risk this policy will address.</span></span>
- <span data-ttu-id="5a79a-109">**정책 설명**.</span><span class="sxs-lookup"><span data-stu-id="5a79a-109">**Policy statement.**</span></span> <span data-ttu-id="5a79a-110">정책 요구 사항에 대한 명확한 요약 설명입니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-110">A clear summary explanation of the policy requirements.</span></span>
- <span data-ttu-id="5a79a-111">**설계 옵션**.</span><span class="sxs-lookup"><span data-stu-id="5a79a-111">**Design options.**</span></span> <span data-ttu-id="5a79a-112">IT 팀과 개발자가 정책을 구현할 때 사용할 수 있는 실행 가능한 추천 사항, 사양 또는 기타 지침입니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-112">Actionable recommendations, specifications, or other guidance that IT teams and developers can use when implementing the policy.</span></span>

<span data-ttu-id="5a79a-113">다음 정책 설명 샘플은 일반적인 몇 가지 구성 관련 비즈니스 위험을 처리하고 사용자 조직의 요구 사항을 처리하기 위해 정책 설명 초안을 작성할 때 참조할 수 있는 예로 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-113">The following sample policy statements address a number of common configuration-related business risks, and are provided as examples for you to reference when drafting policy statements to address your own organization's needs.</span></span> <span data-ttu-id="5a79a-114">이러한 예제를 사용 금지해야 하는 것은 아니며, 모든 특정 위험을 해결하기 위한 여러 정책 옵션이 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-114">Note that these examples are not meant to be proscriptive, and there are potentially several policy options for dealing with any particular risk.</span></span> <span data-ttu-id="5a79a-115">비즈니스 및 IT 팀과 긴밀히 협력하여 고유한 위험 세트에 가장 적합한 정책 솔루션을 식별합니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-115">Work closely with business and IT teams to identify the best policy solutions for your unique set of risks.</span></span>

## <a name="reliance-on-manual-deployment-or-configuration-of-systems"></a><span data-ttu-id="5a79a-116">수동 배포 또는 시스템 구성 사용</span><span class="sxs-lookup"><span data-stu-id="5a79a-116">Reliance on manual deployment or configuration of systems</span></span>

<span data-ttu-id="5a79a-117">**기술 위험**: 배포나 구성 중 사용자 작업에 의존하면 사람의 실수 가능성이 높아지고 시스템 배포 및 구성의 반복 및 예측 가능성은 낮아집니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-117">**Technical risk**: Relying on human intervention during deployment or configuration increases the likelihood of human error and reduces the repeatability and predictability of system deployments and configuration.</span></span> <span data-ttu-id="5a79a-118">일반적으로 시스템 리소스의 배포 속도도 느려집니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-118">It also typically leads to slower deployment of system resources.</span></span>

<span data-ttu-id="5a79a-119">**정책 설명**: 클라우드에 배포된 모든 자산은 가능한 모든 경우에서 템플릿이나 자동화 스크립트로 배포되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-119">**Policy statement**: All assets deployed to the cloud should be deployed using templates or automation scripts whenever possible.</span></span>

<span data-ttu-id="5a79a-120">**잠재적 설계 옵션**: [Azure Resource Manager 템플릿](/azure/azure-resource-manager/resource-group-overview#template-deployment)은 Azure에 리소스를 배포하기 위한 코드 형태의 인프라 방식을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-120">**Potential design options**: [Azure Resource Manager templates](/azure/azure-resource-manager/resource-group-overview#template-deployment) provides an infrastructure-as-code approach to deploying your resources to Azure.</span></span> <span data-ttu-id="5a79a-121">[Azure Building Blocks](https://github.com/mspnp/template-building-blocks/wiki)는 Azure 리소스 배포를 간소화하도록 설계된 Resource Manager 템플릿 집합과 명령줄 도구를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-121">The [Azure Building Blocks](https://github.com/mspnp/template-building-blocks/wiki) provide a command-line tool and set of Resource Manager templates designed to simplify deployment of Azure resources.</span></span>

## <a name="lack-of-visibility-into-system-issues"></a><span data-ttu-id="5a79a-122">시스템 문제에 대한 가시성 부족</span><span class="sxs-lookup"><span data-stu-id="5a79a-122">Lack of visibility into system issues</span></span>

<span data-ttu-id="5a79a-123">**기술 위험**: 비즈니스 시스템에 대한 모니터링과 진단이 부족하여 운영 인력이 시스템 중단 발생 전에 문제를 식별하여 조치하지 못하며, 중단을 제대로 해결하는 데 필요한 시간이 크게 늘어날 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-123">**Technical risk**: Insufficient monitoring and diagnostics for business systems prevent operations personnel from identifying and remediating issues before a system outage occurs, and can significantly increase the time needed to properly resolve an outage.</span></span>

<span data-ttu-id="5a79a-124">**정책 설명**: 구현되는 정책은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-124">**Policy statement**: The following policies will be implemented:</span></span>

- <span data-ttu-id="5a79a-125">모든 프로덕션 시스템과 구성 요소에 대해 주요 메트릭 및 진단 측정값을 식별하며 모니터링 및 진단 도구를 해당 시스템에 적용하고 운영 입력이 정기적으로 모니터링합니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-125">Key metrics and diagnostics measures will be identified for all production systems and components, and monitoring and diagnostic tools will be applied to these systems and monitored regularly by operations personnel.</span></span>
- <span data-ttu-id="5a79a-126">운영에서는 프로덕션 환경에서 사용하기 전에 먼저 준비 및 QA 같은 비 프로덕션 환경에서 모니터링 및 진단 도구의 사용을 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-126">Operations will consider using monitoring and diagnostic tools in non-production environments such as Staging and QA to identify system issues before they occur in the production environment.</span></span>

<span data-ttu-id="5a79a-127">**잠재적 설계 옵션**: Log Analytics 및 Application Insights를 포함한 [Azure Monitor](/azure/azure-monitor/)는 애플리케이션의 업무 수행을 이해하고 영향을 미치는 문제와 관련 리소스를 미리 식별할 수 있게 원격 분석 데이터를 수집하여 분석하는 도구를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-127">**Potential design options**: [Azure Monitor](/azure/azure-monitor/), which also includes Log Analytics and Application Insights, provides tools for collecting and analyzing telemetry to help you understand how your applications are performing and proactively identify issues affecting them and the resources they depend on.</span></span>

## <a name="configuration-security-reviews"></a><span data-ttu-id="5a79a-128">구성 보안 검토</span><span class="sxs-lookup"><span data-stu-id="5a79a-128">Configuration security reviews</span></span>

<span data-ttu-id="5a79a-129">**기술 위험**: 시간이 지날수록 새 보안 위협 또는 우려로 인해 보안 리소스에 대한 무단 액세스 위험이 증대합니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-129">**Technical risk**: Over time, new security threats or concerns can increase the risks of unauthorized access to secure resources.</span></span>

<span data-ttu-id="5a79a-130">**정책 설명**: 클라우드 자산 구성으로 방지해야 하는 악의적 행위자 또는 사용 패턴을 식별하기 위해 구성 관리 팀과의 분기별 검토가 클라우드 거버넌스 프로세스에 포함되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-130">**Policy statement**: Cloud Governance processes must include quarterly review with configuration management teams to identify malicious actors or usage patterns that should be prevented by cloud asset configuration.</span></span>

<span data-ttu-id="5a79a-131">**잠재적 설계 옵션**: 거버넌스 팀 구성원과, 구성 클라우드 애플리케이션 및 리소스를 담당하는 IT 직원이 모두 참여하는 분기별 보안 검토 회의를 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-131">**Potential design options**: Establish a quarterly security review meeting that includes both governance team members and IT staff responsible for configuration cloud applications and resources.</span></span> <span data-ttu-id="5a79a-132">기존 보안 데이터와 메트릭을 검토하여 현재 배포 관리 정책 및 도구에 격차를 설정하고 새 위험을 완화하도록 정책을 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-132">Review existing security data and metrics to establish gaps in current Deployment Acceleration policy and tooling, and update policy to mitigate any new risks.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5a79a-133">다음 단계</span><span class="sxs-lookup"><span data-stu-id="5a79a-133">Next steps</span></span>

<span data-ttu-id="5a79a-134">이 문서에서 언급한 샘플을 시작 지점으로 사용하여 클라우드 채택 계획에 부합하는 특정 비즈니스 위험을 처리하는 정책을 개발합니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-134">Use the samples mentioned in this article as a starting point to develop policies that address specific business risks that align with your cloud adoption plans.</span></span>

<span data-ttu-id="5a79a-135">ID 관리와 관련한 사용자 고유의 사용자 지정 정책 설명 개발하려면 [ID 기준 템플릿](template.md)을 다운로드합니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-135">To begin developing your own custom policy statements related to identity management, download the [Identity Baseline template](template.md).</span></span>

<span data-ttu-id="5a79a-136">이 분야의 도입을 가속화하려면 사용자 환경에 가장 가깝게 일치하는 [실행 가능한 거버넌스 경험](../journeys/overview.md)을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-136">To accelerate adoption of this discipline, choose the [Actionable Governance Journey](../journeys/overview.md) that most closely aligns with your environment.</span></span> <span data-ttu-id="5a79a-137">그런 다음, 특정 회사 정책 결정을 통합하도록 설계를 수정합니다.</span><span class="sxs-lookup"><span data-stu-id="5a79a-137">Then modify the design to incorporate your specific corporate policy decisions.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="5a79a-138">실행 가능한 거버넌스 경험</span><span class="sxs-lookup"><span data-stu-id="5a79a-138">Actionable Governance Journeys</span></span>](../journeys/overview.md)