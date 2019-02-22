---
title: 'CAF: ID 기준 정책 설명 샘플'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: ID 기준 정책 설명 샘플
author: BrianBlanchard
ms.openlocfilehash: 5fad9265b9c048ee502c7e084ddd03faa0ad3e23
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55902032"
---
# <a name="identity-baseline-sample-policy-statements"></a><span data-ttu-id="34ca2-103">ID 기준 정책 설명 샘플</span><span class="sxs-lookup"><span data-stu-id="34ca2-103">Identity Baseline sample policy statements</span></span>

<span data-ttu-id="34ca2-104">개별 클라우드 정책 설명은 위험 평가 프로세스 중에 식별된 특정 위험을 처리하기 위한 지침입니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-104">Individual cloud policy statements are guidelines for addressing specific risks identified during your risk assessment process.</span></span> <span data-ttu-id="34ca2-105">이러한 설명에서는 위험과 이를 처리하기 위한 계획에 대한 간략한 요약을 제공해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-105">These statements should provide a concise summary of risks and plans to deal with them.</span></span> <span data-ttu-id="34ca2-106">각 설명 정의에 포함되는 정보는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-106">Each statement definition should include these pieces of information:</span></span>

- <span data-ttu-id="34ca2-107">기술 위험 - 이 정책에서 처리하는 위험에 대한 요약입니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-107">Technical risk - A summary of the risk this policy will address.</span></span>
- <span data-ttu-id="34ca2-108">정책 설명 - 정책 요구 사항에 대한 명확한 요약 설명입니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-108">Policy statement - A clear summary explanation of the policy requirements.</span></span>
- <span data-ttu-id="34ca2-109">설계 옵션 - IT 팀과 개발자가 정책을 구현할 때 사용할 수 있는 실행 가능한 추천 사항, 사양 또는 기타 지침입니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-109">Design options - Actionable recommendations, specifications, or other guidance that IT teams and developers can use when implementing the policy.</span></span>

<span data-ttu-id="34ca2-110">다음 정책 설명 샘플은 일반적인 몇 가지 ID 관련 비즈니스 위험을 처리하고 사용자 조직의 요구 사항을 처리하기 위해 정책 설명 초안을 작성할 때 참조할 수 있는 예로 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-110">The following sample policy statements address a number of common identity-related business risks, and are provided as examples for you to reference when drafting policy statements to address your own organization's needs.</span></span> <span data-ttu-id="34ca2-111">이러한 예는 사용 금지되는 것이 아니며, 특정 위험을 처리하기 위한 몇 가지 정책 옵션이 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-111">These examples are not meant to be proscriptive, and there are potentially several policy options for dealing with any particular risk.</span></span> <span data-ttu-id="34ca2-112">비즈니스 및 IT 팀과 긴밀히 협력하여 고유한 위험 세트에 가장 적합한 정책 솔루션을 식별합니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-112">Work closely with business and IT teams to identify the best policy solutions for your unique set of risks.</span></span>

## <a name="lack-of-access-controls"></a><span data-ttu-id="34ca2-113">액세스 제어 부족</span><span class="sxs-lookup"><span data-stu-id="34ca2-113">Lack of access controls</span></span>

<span data-ttu-id="34ca2-114">**기술 위험**: 충분하지 않거나 일시적인 액세스 제어 설정으로 인해 중요 리소스 또는 중요 업무용 리소스에 대한 무단 액세스 위험이 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-114">**Technical risk**: Insufficient or ad-hoc access control settings can introduce risk of unauthorized access to sensitive or mission-critical resources.</span></span>

<span data-ttu-id="34ca2-115">**정책 설명**: 클라우드에 배포된 모든 자산은 현재 거버넌스 정책을 통해 승인된 ID와 역할을 사용하여 제어해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-115">**Policy statement**: All assets deployed to the cloud should be controlled using identities and roles approved by current governance policies.</span></span>

<span data-ttu-id="34ca2-116">**잠재적 설계 옵션**: [Azure Active Directory 조건부 액세스](/azure/active-directory/conditional-access/overview)는 Azure의 기본 액세스 제어 메커니즘입니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-116">**Potential design options**: [Azure Active Directory conditional access](/azure/active-directory/conditional-access/overview) is the default access control mechanism in Azure.</span></span>

## <a name="overprovisioned-access"></a><span data-ttu-id="34ca2-117">오버프로비저닝된 액세스</span><span class="sxs-lookup"><span data-stu-id="34ca2-117">Overprovisioned access</span></span>

<span data-ttu-id="34ca2-118">**기술 위험**: 해당 책임 영역을 벗어난 리소스를 제어할 수 있는 사용자 및 그룹의 무단 수정으로 인해 가동 중단 또는 보안 취약성이 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-118">**Technical risk**: Users and groups with control over resources beyond their area of responsibility can result in unauthorized modifications leading to outages or security vulnerabilities.</span></span>

<span data-ttu-id="34ca2-119">**정책 설명**: 구현되는 정책은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-119">**Policy statement**: The following policies will be implemented:</span></span>

- <span data-ttu-id="34ca2-120">최소 권한 액세스 모델은 중요 업무용 애플리케이션 또는 보호된 데이터와 관련된 모든 리소스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-120">A least privilege access model will be applied to any resources involved in mission-critical applications or protected data.</span></span>
- <span data-ttu-id="34ca2-121">승격된 권한은 예외여야 하며, 클라우드 거버넌스 팀에서 이러한 예외를 기록해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-121">Elevated permissions should be an exception, and any such exceptions must be recorded with the Cloud Governance team.</span></span> <span data-ttu-id="34ca2-122">예외는 정기적으로 감사됩니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-122">Exceptions will be audited regularly.</span></span>

<span data-ttu-id="34ca2-123">**잠재적 설계 옵션**: [알아야 할 사항](https://wikipedia.org/wiki/Need_to_know) 및 [최소 권한 보안](https://wikipedia.org/wiki/Principle_of_least_privilege) 원칙에 따라 액세스를 제한하는 RBAC(역할 기반 액세스 제어) 전략을 구현하려면 [Azure Identity Management 모범 사례](/azure/security/azure-security-identity-management-best-practices)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="34ca2-123">**Potential design options**: Consult the [Azure Identity Management best practices](/azure/security/azure-security-identity-management-best-practices) to implement a role-based access control (RBAC) strategy that restricts access based on the [need to know](https://wikipedia.org/wiki/Need_to_know) and [least privilege security](https://wikipedia.org/wiki/Principle_of_least_privilege) principles.</span></span>

## <a name="lack-of-shared-management-accounts-between-on-premises-and-the-cloud"></a><span data-ttu-id="34ca2-124">온-프레미스와 클라우드 간 공유 관리 계정 부족</span><span class="sxs-lookup"><span data-stu-id="34ca2-124">Lack of shared management accounts between on-premises and the cloud</span></span>

<span data-ttu-id="34ca2-125">**기술 위험**: 클라우드 리소스에 대한 액세스 권한이 충분하지 않을 수 있는 온-프레미스 Active Directory의 계정이 있는 IT 관리 또는 운영 직원은 운영 또는 보안 문제를 효율적으로 해결하지 못할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-125">**Technical risk**: IT management or administrative staff with accounts on your on-premises Active Directory may not have sufficient access to cloud resources may not be able to efficiently resolve operational or security issues.</span></span>

<span data-ttu-id="34ca2-126">**정책 설명**: 승격된 권한이 있는 온-프레미스 Active Directory 인프라의 모든 그룹은 승인된 RBAC 역할에 매핑되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-126">**Policy statement**: All groups in the on-premises Active Directory infrastructure that have elevated privileges should be mapped to an approved RBAC role.</span></span>

<span data-ttu-id="34ca2-127">**잠재적 설계 옵션**: 클라우드 기반 Azure Active Directory와 온-프레미스 Active Directory 간에 하이브리드 ID 솔루션을 구현하고, 필요한 온-프레미스 그룹을 작업 수행에 필요한 RBAC 역할에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-127">**Potential design options**: Implement a hybrid identity solution between your cloud-based Azure Active Directory and your on-premise Active Directory, and add the required on-premises groups to the RBAC roles necessary to do their work.</span></span>

## <a name="weak-authentication-mechanisms"></a><span data-ttu-id="34ca2-128">약한 인증 메커니즘</span><span class="sxs-lookup"><span data-stu-id="34ca2-128">Weak authentication mechanisms</span></span>

<span data-ttu-id="34ca2-129">**기술 위험**: 기본 사용자/암호 조합과 같이 보안이 충분하지 않은 사용자 인증 방법을 사용하는 ID 관리 시스템에서는 암호가 손상되거나 해킹될 수 있으므로 보안 클라우드 시스템에 대한 무단 액세스 위험이 큽니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-129">**Technical risk**: Identity management systems with insufficiently secure user authentication methods, such as basic user/password combinations, can lead to compromised or hacked passwords, providing a major risk of unauthorized access to secure cloud systems.</span></span>

<span data-ttu-id="34ca2-130">**정책 설명**: 모든 계정은 MFA(다단계 인증) 방법을 사용하여 보안 리소스에 로그인해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-130">**Policy statement**: All accounts are required to login to secured resources using a multi-factor authentication (MFA) method.</span></span>

<span data-ttu-id="34ca2-131">**잠재적 설계 옵션**: Azure Active Directory의 경우 사용자 인증 프로세스의 일환으로 [Azure Multi-Factor Authentication](/azure/active-directory/authentication/concept-mfa-howitworks)을 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-131">**Potential design options**: For Azure Active Directory, implement [Azure Multi-Factor Authentication](/azure/active-directory/authentication/concept-mfa-howitworks) as part of your user authorization process.</span></span>

## <a name="isolated-identity-providers"></a><span data-ttu-id="34ca2-132">격리된 ID 공급자</span><span class="sxs-lookup"><span data-stu-id="34ca2-132">Isolated identity providers</span></span>

<span data-ttu-id="34ca2-133">**기술 위험**: 호환되지 않는 ID 공급자는 고객 또는 다른 비즈니스 파트너와 리소스 또는 서비스를 공유하지 못할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-133">**Technical risk**: Incompatible identity providers can result in the inability to share resources or services with customers or other business partners.</span></span>

<span data-ttu-id="34ca2-134">**정책 설명**: 고객 인증이 필요한 애플리케이션을 배포하려면 내부 사용자에 대한 기본 ID 공급자와 호환되는 승인된 ID 공급자를 사용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-134">**Policy statement**: Deployment of any applications that require customer authentication must use an approved identity provider that is compatible with the primary identity provider for internal users.</span></span>

<span data-ttu-id="34ca2-135">**잠재적 설계 옵션**: 내부 및 고객 ID 공급자 간에 [Azure Active Directory와의 페더레이션](/azure/active-directory/hybrid/whatis-fed)을 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-135">**Potential design options**: Implement [Federation with Azure Active Directory](/azure/active-directory/hybrid/whatis-fed) between your internal and customer identity providers.</span></span>

## <a name="identity-reviews"></a><span data-ttu-id="34ca2-136">ID 검토</span><span class="sxs-lookup"><span data-stu-id="34ca2-136">Identity reviews</span></span>

<span data-ttu-id="34ca2-137">**기술 위험**: 시간이 지남에 따라 새 클라우드 배포 또는 기타 보안 문제가 추가되면 보안 리소스에 대한 무단 액세스 위험이 커질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-137">**Technical risk**: Over time business change, the addition of new cloud deployments or other security concerns can increase the risks of unauthorized access to secure resources.</span></span>

<span data-ttu-id="34ca2-138">**정책 설명**: 클라우드 자산 구성으로 방지해야 하는 악의적 행위자 또는 사용 패턴을 식별하기 위해 ID 관리 팀과의 분기별 검토가 클라우드 거버넌스 프로세스에 포함되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-138">**Policy statement**: Cloud Governance processes must include quarterly review with identity management teams to identify malicious actors or usage patterns that should be prevented by cloud asset configuration.</span></span>

<span data-ttu-id="34ca2-139">**잠재적 설계 옵션**: 거버넌스 팀 구성원과 ID 서비스 관리를 담당하는 IT 직원이 모두 참여하는 분기별 보안 검토 회의를 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-139">**Potential design options**: Establish a quarterly security review meeting that includes both governance team members and IT staff responsible for managing identity services.</span></span> <span data-ttu-id="34ca2-140">기존 보안 데이터와 메트릭을 검토하여 현재 ID 관리 정책 및 도구에 격차를 설정하고 새 위험을 완화하도록 정책을 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-140">Review existing security data and metrics to establish gaps in current identity management policy and tooling, and update policy to mitigate any new risks.</span></span>

## <a name="next-steps"></a><span data-ttu-id="34ca2-141">다음 단계</span><span class="sxs-lookup"><span data-stu-id="34ca2-141">Next steps</span></span>

<span data-ttu-id="34ca2-142">이 문서에서 언급한 샘플을 시작 지점으로 사용하여 클라우드 채택 계획에 부합하는 특정 비즈니스 위험을 처리하는 정책을 개발합니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-142">Use the samples mentioned in this article as a starting point to develop policies that address specific business risks that align with your cloud adoption plans.</span></span>

<span data-ttu-id="34ca2-143">보안 기준과 관련된 사용자 고유의 사용자 지정 정책 설명 개발하려면 [보안 기준 템플릿](template.md)을 다운로드합니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-143">To begin developing your own custom policy statements related to Identity Baseline, download the [Identity Baseline template](template.md).</span></span>

<span data-ttu-id="34ca2-144">이 분야의 채택을 가속화하려면 사용자 환경에 가장 가깝게 일치하는 [실행 가능한 거버넌스 경험](../journeys/overview.md)을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-144">To accelerate adoption of this discipline, choose the [Actionable Governance Journey](../journeys/overview.md) that most closely aligns with your environment.</span></span> <span data-ttu-id="34ca2-145">그런 다음, 특정 회사 정책 결정을 통합하도록 설계를 수정합니다.</span><span class="sxs-lookup"><span data-stu-id="34ca2-145">Then modify the design to incorporate your specific corporate policy decisions.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="34ca2-146">실행 가능한 거버넌스 경험</span><span class="sxs-lookup"><span data-stu-id="34ca2-146">Actionable Governance Journeys</span></span>](../journeys/overview.md)