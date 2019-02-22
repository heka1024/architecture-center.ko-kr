---
title: 'CAF: 보안 기준 샘플 정책 설명'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 보안 기준 샘플 정책 설명
author: BrianBlanchard
ms.openlocfilehash: ac40e022f8dff0c3021c04cd9d6ae42d28afaf1f
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901381"
---
# <a name="security-baseline-sample-policy-statements"></a><span data-ttu-id="fd0bb-103">보안 기준 샘플 정책 설명</span><span class="sxs-lookup"><span data-stu-id="fd0bb-103">Security Baseline sample policy statements</span></span>

<span data-ttu-id="fd0bb-104">개별 클라우드 정책 설명은 위험 평가 프로세스 중에 파악된 특정 위험을 해결하기 위한 지침입니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-104">Individual cloud policy statements are guidelines for addressing specific risks identified during your risk assessment process.</span></span> <span data-ttu-id="fd0bb-105">이러한 설명은 위험의 간략한 요약과 위험을 해결하기 위한 계획을 제공해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-105">These statements should provide a concise summary of risks and plans to deal with them.</span></span> <span data-ttu-id="fd0bb-106">각 설명 정의에는 다음 정보가 포함되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-106">Each statement definition should include these pieces of information:</span></span>

- <span data-ttu-id="fd0bb-107">기술적 위험 - 이 정책이 처리할 위험의 요약입니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-107">Technical risk - A summary of the risk this policy will address.</span></span>
- <span data-ttu-id="fd0bb-108">정책 설명 - 정책 요구 사항의 명확한 요약 설명입니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-108">Policy statement - A clear summary explanation of the policy requirements.</span></span>
- <span data-ttu-id="fd0bb-109">기술 옵션 - IT 팀과 개발자가 정책 구현 시에 사용할 수 있는 실행 가능한 권장 사항, 사양 또는 기타 지침입니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-109">Technical options - Actionable recommendations, specifications, or other guidance that IT teams and developers can use when implementing the policy.</span></span>

<span data-ttu-id="fd0bb-110">여러 보안 관련 일반 비즈니스 위험을 해결하는 다음 샘플 정책 설명은 사용자 조직의 요구를 해결하는 실제 정책 설명의 초안을 작성할 때 참조할 수 있도록 예제로 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-110">The following sample policy statements address a number of common security-related business risks, and are provided as examples for you to reference when drafting actual policy statements addressing your own organization's needs.</span></span> <span data-ttu-id="fd0bb-111">특정 위험을 해결하기 위해 반드시 이러한 예제를 사용해야 하는 것은 아니며, 확인된 한 가지 위험을 해결할 수 있도록 여러 정책 옵션이 제공될 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-111">These examples are not meant to be proscriptive, and there are potentially several policy options for dealing with any single identified risk.</span></span> <span data-ttu-id="fd0bb-112">실무 팀, 보안 팀 및 IT 팀과 긴밀하게 협력하여 특정 보안 위험에 가장 적합한 정책 솔루션을 선택하세요.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-112">Work closely with business, security, and IT teams to identify the best policy solutions for your particular security risks.</span></span>  

## <a name="asset-classification"></a><span data-ttu-id="fd0bb-113">자산 분류</span><span class="sxs-lookup"><span data-stu-id="fd0bb-113">Asset classification</span></span>

<span data-ttu-id="fd0bb-114">**기술적 위험**: 중요한 데이터를 포함하는 자산 또는 중요 업무용 자산으로 정확하게 확인되지 않은 자산은 충분히 보호되지 않을 수 있으므로 데이터가 누수되거나 업무가 중단될 가능성이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-114">**Technical risk**: Assets that are not correctly identified as mission-critical or involving sensitive data may not receive sufficient protections, leading to potential data leaks or business disruptions.</span></span>

<span data-ttu-id="fd0bb-115">**정책 설명**: 배포된 모든 자산은 중요도 및 데이터 분류를 기준으로 범주를 지정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-115">**Policy statement**: All deployed assets must be categorized by criticality and data classification.</span></span> <span data-ttu-id="fd0bb-116">자산을 클라우드로 배포하기 전에 클라우드 거버넌스 팀과 애플리케이션 소유자가 분류를 검토해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-116">Classifications must be reviewed by the Cloud Governance team and the application owner before deployment to the cloud.</span></span>

<span data-ttu-id="fd0bb-117">**사용 가능한 디자인 옵션**: [리소스 태그 지정 표준](../../decision-guides/resource-tagging/overview.md)을 정하고 IT 담당자가 [Azure 리소스 태그](/azure/azure-resource-manager/resource-group-using-tags)를 사용해 배포된 모든 리소스에 해당 표준을 일관되게 적용하도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-117">**Potential design option**: Establish [resource tagging standards](../../decision-guides/resource-tagging/overview.md) and ensure IT staff apply them consistently to any deployed resources using [Azure resource tags](/azure/azure-resource-manager/resource-group-using-tags).</span></span>

## <a name="data-encryption"></a><span data-ttu-id="fd0bb-118">데이터 암호화.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-118">Data encryption</span></span>

<span data-ttu-id="fd0bb-119">**기술적 위험**: 보호된 데이터가 저장 중에 노출될 위험이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-119">**Technical risk**: There is a risk of protected data being exposed during storage.</span></span>

<span data-ttu-id="fd0bb-120">**정책 설명**: 보호된 데이터는 미사용 시 모두 암호화해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-120">**Policy statement**: All protected data must be encrypted when at rest.</span></span>

<span data-ttu-id="fd0bb-121">**사용 가능한 디자인 옵션**: [Azure 암호화 개요](/azure/security/security-azure-encryption-overview) 문서에 나와 있는 Azure 플랫폼에서 미사용 데이터 암호화를 수행하는 방법의 설명을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-121">**Potential design option**: See the [Azure encryption overview](/azure/security/security-azure-encryption-overview) article for a discussion of how data at rest encryption is performed on the Azure platform.</span></span>  

## <a name="network-isolation"></a><span data-ttu-id="fd0bb-122">네트워크 격리</span><span class="sxs-lookup"><span data-stu-id="fd0bb-122">Network isolation</span></span>

<span data-ttu-id="fd0bb-123">**기술적 위험**: 네트워크와 네트워크 내 서브넷 간의 연결로 인해 데이터 누수 또는 중요 업무용 서비스 중단을 야기할 수 있는 취약성이 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-123">**Technical risk**: Connectivity between networks and subnets within networks introduces potential vulnerabilities that can result in data leaks or disruption of mission-critical services.</span></span>

<span data-ttu-id="fd0bb-124">**정책 설명**: 보호된 데이터를 포함하는 네트워크 서브넷은 다른 서브넷에서 격리해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-124">**Policy statement**: Network subnets containing protected data must be isolated from any other subnets.</span></span> <span data-ttu-id="fd0bb-125">보호된 데이터 서브넷 간의 네트워크 트래픽은 정기적으로 감사해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-125">Network traffic between protected data subnets is to be audited regularly.</span></span>

<span data-ttu-id="fd0bb-126">**사용 가능한 디자인 옵션**: Azure에서 [Azure Virtual Network](/azure/virtual-network/virtual-networks-overview)를 통해 네트워크 및 서브넷 격리를 관리합니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-126">**Potential design option**: In Azure, network and subnet isolation is managed through [Azure Virtual Networks](/azure/virtual-network/virtual-networks-overview).</span></span>

## <a name="secure-external-access"></a><span data-ttu-id="fd0bb-127">외부 액세스 보호</span><span class="sxs-lookup"><span data-stu-id="fd0bb-127">Secure external access</span></span>

<span data-ttu-id="fd0bb-128">**기술적 위험**: 공용 인터넷에서 워크로드 액세스를 허용하면 침입 위험이 발생하여 데이터가 무단 노출되거나 업무가 중단될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-128">**Technical risk**: Allowing access to workloads from the public internet introduces a risk of intrusion resulting in unauthorized data exposure or business disruption.</span></span>

<span data-ttu-id="fd0bb-129">**정책 설명**: 공용 인터넷이나 데이터 센터를 통해서는 보호된 데이터를 포함하는 서브넷에 직접 액세스할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-129">**Policy statement**: No subnet containing protected data can be directly accessed over public internet or across datacenters.</span></span> <span data-ttu-id="fd0bb-130">이러한 서브넷에 액세스하려면 중간 서브넷을 거쳐야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-130">Access to those subnets must be routed through intermediate subnet works.</span></span> <span data-ttu-id="fd0bb-131">즉, 해당 서브넷에 액세스할 때는 항상 패킷 검사 및 차단 기능을 실행할 수 있는 방화벽 솔루션을 거쳐야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-131">All access into those subnets must come through a firewall solution capable of performing packet scanning and blocking functions.</span></span>

<span data-ttu-id="fd0bb-132">**사용 가능한 디자인 옵션**: Azure에서 [공용 인터넷과 클라우드 기반 네트워크 간에 DMZ](/azure/architecture/reference-architectures/dmz/secure-vnet-dmz)를 배포하여 공용 엔드포인트를 보호합니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-132">**Potential design option**: In Azure, secure public endpoints by deploying a [DMZ between the public internet and your cloud-based network](/azure/architecture/reference-architectures/dmz/secure-vnet-dmz).</span></span>

## <a name="ddos-protection"></a><span data-ttu-id="fd0bb-133">DDoS 보호</span><span class="sxs-lookup"><span data-stu-id="fd0bb-133">DDoS protection</span></span>

<span data-ttu-id="fd0bb-134">**기술적 위험**: DDoS(배포된 서비스 거부) 공격으로 인해 업무가 중단될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-134">**Technical risk**: Distributed denial of service (DDoS) attacks can result in a business interruption.</span></span>

<span data-ttu-id="fd0bb-135">**정책 설명**: 공개적으로 액세스할 수 있는 모든 네트워크 엔드포인트에 자동화된 DDoS 완화 메커니즘을 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-135">**Policy statement**: Deploy automated DDoS mitigation mechanisms to all publicly accessible network endpoints.</span></span>

<span data-ttu-id="fd0bb-136">**사용 가능한 디자인 옵션**: [Azure DDoS Protection](/azure/virtual-network/ddos-protection-overview)을 사용하여 DDoS 공격으로 인한 업무 중단을 최소화합니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-136">**Potential design option**: Use [Azure DDoS Protection](/azure/virtual-network/ddos-protection-overview) to minimize disruptions caused by DDoS attacks.</span></span>

## <a name="secure-on-premises-connectivity"></a><span data-ttu-id="fd0bb-137">온-프레미스 연결 보안 유지</span><span class="sxs-lookup"><span data-stu-id="fd0bb-137">Secure on-premises connectivity</span></span>

<span data-ttu-id="fd0bb-138">**기술적 위험**: 공용 인터넷을 통해 클라우드 네트워크와 온-프레미스 간에 전송되는 암호화되지 않은 트래픽은 가로채기에 취약하므로 데이터 노출 위험이 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-138">**Technical risk**: Unencrypted traffic between your cloud network and on-premises over the public internet is vulnerable to interception, introducing the risk of data exposure.</span></span>

<span data-ttu-id="fd0bb-139">**정책 설명**: 온-프레미스와 클라우드 네트워크 간의 모든 연결은 암호화된 보안 VPN 연결이나 전용 개인 WAN 링크를 통해 설정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-139">**Policy statement**: All connections between the on-premises and cloud networks must take place either through a secure encrypted VPN connection or a dedicated private WAN link.</span></span>

<span data-ttu-id="fd0bb-140">**사용 가능한 디자인 옵션**: Azure에서 ExpressRoute 또는 Azure VPN을 사용하여 온-프레미스와 클라우드 네트워크 간의 개인 연결을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-140">**Potential design option**: In Azure, use ExpressRoute or Azure VPN to establish private connections between your on-premises and cloud networks.</span></span>

## <a name="network-monitoring-and-enforcement"></a><span data-ttu-id="fd0bb-141">네트워크 모니터링 및 적용</span><span class="sxs-lookup"><span data-stu-id="fd0bb-141">Network monitoring and enforcement</span></span>

<span data-ttu-id="fd0bb-142">**기술적 위험**: 네트워크 구성을 변경하면 새로운 취약성과 데이터 노출 위험이 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-142">**Technical risk**: Changes to network configuration can lead to new vulnerabilities and data exposure risks.</span></span>

<span data-ttu-id="fd0bb-143">**정책 설명**: 거버넌스 도구는 보안 기준 팀이 정의한 네트워크 구성 요구 사항을 감사하고 적용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-143">**Policy statement**: Governance tooling must audit and enforce network configuration requirements defined by the Security Baseline team.</span></span>

<span data-ttu-id="fd0bb-144">**사용 가능한 디자인 옵션**: Azure에서는 [Azure Network Watcher](/azure/network-watcher/network-watcher-monitoring-overview)를 사용하여 네트워크 활동을 모니터링할 수 있으며 [Azure Security Center](/azure/security-center/security-center-network-recommendations)를 사용하여 보안 취약성을 파악할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-144">**Potential design option**: In Azure, network activity can be monitored using [Azure Network Watcher](/azure/network-watcher/network-watcher-monitoring-overview), and [Azure Security Center](/azure/security-center/security-center-network-recommendations) can help identify security vulnerabilities.</span></span> <span data-ttu-id="fd0bb-145">Azure Policy에서는 보안 팀이 정의한 제한에 따라 네트워크 리소스 및 리소스 구성 정책을 제한할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-145">Azure Policy allows you to restrict network resources and resource configuration policy according to limits defined by the security team.</span></span>

## <a name="security-review"></a><span data-ttu-id="fd0bb-146">보안 검토</span><span class="sxs-lookup"><span data-stu-id="fd0bb-146">Security review</span></span>

<span data-ttu-id="fd0bb-147">**기술적 위험**: 새로운 보안 위협과 공격 유형이 계속 나타나므로 클라우드 리소스 노출 또는 제공 중단 위험이 증가합니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-147">**Technical risk**: Over time, new security threats and attack types emerge, increasing the risk of exposure or disruption of your cloud resources.</span></span>

<span data-ttu-id="fd0bb-148">**정책 설명**: 보안 팀은 클라우드 배포에 영향을 줄 수 있는 추세 및 익스플로잇 가능성을 정기적으로 검토하여 클라우드에서 사용되는 보안 기준 도구에 업데이트를 제공해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-148">**Policy statement**: Trends and potential exploits that could affect cloud deployments should be reviewed regularly by the security team to provide updates to Security Baseline tooling used in the cloud.</span></span>

<span data-ttu-id="fd0bb-149">**사용 가능한 디자인 옵션**: 관련 IT 및 거버넌스 팀 구성원들이 참여하는 정기 보안 검토 회의를 진행합니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-149">**Potential design option**: Establish a regular security review meeting that includes relevant IT and governance team members.</span></span> <span data-ttu-id="fd0bb-150">기존 보안 데이터와 메트릭을 검토해 현재 정책 및 보안 기준 도구에서 부족한 부분을 확인한 다음 새로운 위험을 완화할 수 있도록 정책을 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-150">Review existing security data and metrics to establish gaps in current policy and Security Baseline tooling, and update policy to mitigate any new risks.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fd0bb-151">다음 단계</span><span class="sxs-lookup"><span data-stu-id="fd0bb-151">Next steps</span></span>

<span data-ttu-id="fd0bb-152">이 문서에서 설명한 샘플을 토대로 하여 특정 보안 위험을 해결하며 클라우드 채택 계획에 적합한 정책을 개발합니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-152">Use the samples mentioned in this article as a starting point to develop policies that address specific security risks that align with your cloud adoption plans.</span></span>

<span data-ttu-id="fd0bb-153">보안 기준 관련 사용자 지정 정책 설명 개발을 시작하려면 [보안 기준 템플릿](template.md)을 다운로드하세요.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-153">To begin developing your own custom policy statements related to Security Baseline, download the [Security Baseline template](template.md).</span></span>

<span data-ttu-id="fd0bb-154">이러한 원칙을 신속하게 채택하려면 사용자 환경에 가장 가깝게 일치하는 [실행 가능한 거버넌스 경험](../journeys/overview.md)을 선택한 다음</span><span class="sxs-lookup"><span data-stu-id="fd0bb-154">To accelerate adoption of this discipline, choose the [Actionable Governance Journey](../journeys/overview.md) that most closely aligns with your environment.</span></span> <span data-ttu-id="fd0bb-155">디자인을 수정하여 회사의 구체적인 정책 결정 사항을 통합합니다.</span><span class="sxs-lookup"><span data-stu-id="fd0bb-155">Then modify the design to incorporate your specific corporate policy decisions.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="fd0bb-156">실행 가능한 거버넌스 경험</span><span class="sxs-lookup"><span data-stu-id="fd0bb-156">Actionable Governance Journeys</span></span>](../journeys/overview.md)
