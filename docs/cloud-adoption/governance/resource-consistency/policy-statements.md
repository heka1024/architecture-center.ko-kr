---
title: 'CAF: 리소스 일관성 샘플 정책 문'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 리소스 일관성 샘플 정책 문
author: BrianBlanchard
ms.openlocfilehash: 9217ae73b0edf5b40bedac1cdc961b7a34da87d1
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901468"
---
# <a name="resource-consistency-sample-policy-statements"></a><span data-ttu-id="1be0c-103">리소스 일관성 샘플 정책 문</span><span class="sxs-lookup"><span data-stu-id="1be0c-103">Resource Consistency sample policy statements</span></span>

<span data-ttu-id="1be0c-104">개별 클라우드 정책 문은 위험 평가 프로세스 중에 파악된 특정 위험을 해결하기 위한 지침입니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-104">Individual cloud policy statements are guidelines for addressing specific risks identified during your risk assessment process.</span></span> <span data-ttu-id="1be0c-105">이러한 명령문은 위험 및 위험을 해결하기 위한 계획의 간략한 요약을 제공해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-105">These statements should provide a concise summary of risks and plans to deal with them.</span></span> <span data-ttu-id="1be0c-106">각 명령문 정의에는 다음 정보가 포함되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-106">Each statement definition should include these pieces of information:</span></span>

- <span data-ttu-id="1be0c-107">기술적 위험 - 이 정책이 처리할 위험의 요약입니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-107">Technical risk - A summary of the risk this policy will address.</span></span>
- <span data-ttu-id="1be0c-108">정책 문 - 정책 요구 사항의 명확한 요약 설명입니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-108">Policy statement - A clear summary explanation of the policy requirements.</span></span>
- <span data-ttu-id="1be0c-109">디자인 옵션 - IT 팀과 개발자가 정책 구현 시에 사용할 수 있는 실행 가능한 권장 사항, 사양 또는 기타 지침입니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-109">Design options - Actionable recommendations, specifications, or other guidance that IT teams and developers can use when implementing the policy.</span></span>

<span data-ttu-id="1be0c-110">다음 샘플 정책 문은 여러 리소스 일관성 관련 일반 비즈니스 위험을 해결하고, 사용자 조직의 요구를 해결하는 정책 문의 초안을 작성할 때 참조할 수 있도록 예제로 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-110">The following sample policy statements address a number of common Resource Consistency-related business risks, and are provided as examples for you to reference when drafting policy statements to address your own organization's needs.</span></span> <span data-ttu-id="1be0c-111">이러한 예제를 사용 금지해야 하는 것은 아니며, 모든 특정 위험을 해결하기 위한 여러 정책 옵션이 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-111">Note that these examples are not meant to be proscriptive, and there are potentially several policy options for dealing with any particular risk.</span></span> <span data-ttu-id="1be0c-112">실무 팀 및 IT 팀과 긴밀하게 협력하여 고유한 위험 세트에 가장 적합한 정책 솔루션을 식별합니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-112">Work closely with business and IT teams to identify the best policy solutions for your unique set of risks.</span></span>

## <a name="tagging"></a><span data-ttu-id="1be0c-113">태그 지정</span><span class="sxs-lookup"><span data-stu-id="1be0c-113">Tagging</span></span>

<span data-ttu-id="1be0c-114">**기술적 위험**: 배포된 리소스와 관련된 적절한 메타데이터를 태그 지정하지 않으면 IT 운영 팀은 필수 SLA, 비즈니스 작업이나 운영 비용의 중요도에 따라 리소스의 지원 또는 최적화의 우선 순위를 지정할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-114">**Technical risk**: Without proper metadata tagging associated with deployed resources, IT Operations cannot prioritize support or optimization of resources based on required SLA, importance to business operations, or operational cost.</span></span> <span data-ttu-id="1be0c-115">이렇게 하면 IT 리소스 및 인시던트 해결의 잠재적 지연이 잘못 할당될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-115">This can result in mis-allocation of IT resources and potential delays in incident resolution.</span></span>

<span data-ttu-id="1be0c-116">**정책 문**: 다음 정책이 구현됩니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-116">**Policy statement**: The following policies will be implemented:</span></span>

- <span data-ttu-id="1be0c-117">비용, 중요성, SLA 및 환경의 값을 사용하여 배포된 자산의 태그를 지정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-117">Deployed assets should be tagged with the following values: cost, criticality, SLA, and environment.</span></span>
- <span data-ttu-id="1be0c-118">거버넌스 도구는 비용, 중요성, SLA, 애플리케이션 및 환경과 관련된 태그 지정의 유효성을 확인해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-118">Governance tooling must validate tagging related to cost, criticality, SLA, application, and environment.</span></span> <span data-ttu-id="1be0c-119">모든 값은 거버넌스 팀에서 관리되는 미리 정의된 값에 맞춰 정렬되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-119">All values must align to predefined values managed by the governance team.</span></span>

<span data-ttu-id="1be0c-120">**잠재적인 설계 옵션**: Azure에서 [표준 이름-값 메타데이터 태그](/azure/azure-resource-manager/resource-group-using-tags)는 대부분의 리소스 유형에서 지원됩니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-120">**Potential design options**: In Azure, [standard name-value metadata tags](/azure/azure-resource-manager/resource-group-using-tags) are supported on most resource types.</span></span> <span data-ttu-id="1be0c-121">[Azure Policy](/azure/governance/policy/overview)는 리소스 만들기의 일부로 특정 태그를 적용하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-121">[Azure Policy](/azure/governance/policy/overview) is used to enforce specific tags as part of resource creation.</span></span>

## <a name="ungoverned-subscriptions"></a><span data-ttu-id="1be0c-122">비관리 구독</span><span class="sxs-lookup"><span data-stu-id="1be0c-122">Ungoverned subscriptions</span></span>

<span data-ttu-id="1be0c-123">**기술적 위험**: 구독 및 관리 그룹의 임의 생성으로 인해 제대로 거버넌스 정책이 적용되지 않는 클라우드 자산의 격리된 부분이 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-123">**Technical risk**: Arbitrary creation of subscriptions and management groups can lead to isolated sections of your cloud estate that are not properly subject to your governance policies.</span></span>

<span data-ttu-id="1be0c-124">**정책 문**: 모든 중요 업무용 애플리케이션 또는 보호되는 데이터의 새 구독 또는 관리 그룹의 생성에는 클라우드 거버넌스 팀의 검토가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-124">**Policy statement**: Creation of new subscriptions or management groups for any mission-critical applications or protected data will require a review from the Cloud Governance team.</span></span> <span data-ttu-id="1be0c-125">승인된 변경 내용은 적절한 Blueprint 할당으로 통합됩니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-125">Approved changes will be integrated into a proper blueprint assignment.</span></span>

<span data-ttu-id="1be0c-126">**잠재적인 설계 옵션**: 구독 생성 및 액세스 제어 프로세스를 제어하는 승인된 거버넌스 팀 구성원에게만 조직[Azure 관리 그룹](/azure/governance/management-groups/)에 대한 관리 액세스를 잠급니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-126">**Potential design options**: Lock down administrative access to your organizations [Azure management groups](/azure/governance/management-groups/) to only approved governance team members who will control the subscription creation and access control process.</span></span>

## <a name="manage-updates-to-virtual-machines"></a><span data-ttu-id="1be0c-127">가상 머신에 대한 업데이트 관리</span><span class="sxs-lookup"><span data-stu-id="1be0c-127">Manage updates to virtual machines</span></span>

<span data-ttu-id="1be0c-128">**기술적 위험**: 최신 업데이트 및 소프트웨어 패치로 최신 상태로 유지되지 않는 VM(가상 머신)은 보안 및 성능 문제에 취약하게 돼 결국 서비스가 중단될 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-128">**Technical risk**: Virtual machines (VMs) that are not up-to-date with the latest updates and software patches are vulnerable to security or performance issues, which can result in service disruptions.</span></span>

<span data-ttu-id="1be0c-129">**정책 문**: 거버넌스 도구는 모든 배포된 VM에 자동 업데이트가 사용되도록 적용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-129">**Policy statement**: Governance tooling must enforce that automatic updates are enabled on all deployed VMs.</span></span> <span data-ttu-id="1be0c-130">위반은 운영 관리 팀에서 검토되고 작업 정책에 따라 수정돼야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-130">Violations must be reviewed with operational management teams and remediated in accordance with operations policies.</span></span> <span data-ttu-id="1be0c-131">자동으로 업데이트되지 않는 자산은 IT 운영 팀에서 소유한 프로세스에 포함돼야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-131">Assets that are not automatically updated must be included in processes owned by IT Operations.</span></span>

<span data-ttu-id="1be0c-132">**잠재적인 설계 옵션**: Azure에서 호스팅한 VM의 경우 [Azure Automation의 업데이트 관리 솔루션](/azure/automation/automation-update-management)을 사용하여 일관된 업데이트 관리를 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-132">**Potential design options**: For Azure hosted VMs, you can provide consistent update management using the [Update Management solution in Azure Automation](/azure/automation/automation-update-management).</span></span>

## <a name="deployment-compliance"></a><span data-ttu-id="1be0c-133">배포 규정 준수</span><span class="sxs-lookup"><span data-stu-id="1be0c-133">Deployment compliance</span></span>

<span data-ttu-id="1be0c-134">**기술적 위험**: 배포 스크립트 및 클라우드 거버넌스 팀에서 완벽하게 검사하지 않는 자동화 도구는 결국 정책을 위반하는 리소스 배포가 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-134">**Technical risk**: Deployment scripts and automation tooling that is not fully vetted by the Cloud Governance team can result in resource deployments that violate policy.</span></span>

<span data-ttu-id="1be0c-135">**정책 문**: 다음 정책이 구현됩니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-135">**Policy statement**: The following policies will be implemented:</span></span>

- <span data-ttu-id="1be0c-136">배포된 자산에 거버넌스를 지속적으로 적용하려면 클라우드 거버넌스 팀에서 배포 도구를 승인해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-136">Deployment tooling must be approved by the Cloud Governance team to ensure ongoing governance of deployed assets.</span></span>
- <span data-ttu-id="1be0c-137">배포 스크립트는 주기적 검토 및 감사를 위해 클라우드 거버넌스 팀에서 액세스할 수 있는 중앙 리포지토리에 유지되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-137">Deployment scripts must be maintained in central repository accessible by the Cloud Governance team for periodic review and auditing.</span></span>

<span data-ttu-id="1be0c-138">**잠재적인 설계 옵션**: 자동 배포를 관리하는 [Azure Blueprints](/azure/governance/blueprints/)를 일관되게 사용하면 조직의 거버넌스 표준 및 정책을 준수하는 Azure 리소스를 일관되게 배포할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-138">**Potential design options**: Consistent use of [Azure Blueprints](/azure/governance/blueprints/) to manage automated deployments allows consistent deployments of Azure resources that adhere to your organization's governance standards and policies.</span></span>

## <a name="monitoring"></a><span data-ttu-id="1be0c-139">모니터링</span><span class="sxs-lookup"><span data-stu-id="1be0c-139">Monitoring</span></span>

<span data-ttu-id="1be0c-140">**기술적 위험**: 부적절하게 구현되거나 일관되지 않게 계측된 모니터링을 통해 워크로드 상태 문제 또는 기타 정책 준수 위반을 감지할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-140">**Technical risk**: Improperly implemented or inconsistently instrumented monitoring can prevent the detection of workload health issues or other policy compliance violations.</span></span>

<span data-ttu-id="1be0c-141">**정책 문**: 다음 정책이 구현됩니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-141">**Policy statement**: The following policies will be implemented:</span></span>

- <span data-ttu-id="1be0c-142">거버넌스 도구는 중요 업무용 애플리케이션 또는 보호되는 데이터와 관련된 모든 자산이 리소스 고갈 및 최적화에 대한 모니터링에 포함되는지 확인해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-142">Governance tooling must validate that all assets related to mission-critical applications or protected data are included in monitoring for resource depletion and optimization.</span></span>
- <span data-ttu-id="1be0c-143">거버넌스 도구는 적절한 수준의 로깅 데이터가 모든 중요 업무용 애플리케이션 또는 보호되는 데이터에 대해 수집되고 있는지 확인해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-143">Governance tooling must validate that the appropriate level of logging data is being collected for all mission-critical applications or protected data.</span></span>

<span data-ttu-id="1be0c-144">**잠재적인 설계 옵션**: [Azure Monitor](/azure/azure-monitor/overview)는 Azure 플랫폼의 기본 모니터링 서비스이며, 일관된 모니터링은 리소스를 배포할 경우 [Azure Blueprints](/azure/governance/blueprints/)의 사용을 통해 적용될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-144">**Potential design options**: [Azure Monitor](/azure/azure-monitor/overview) is the default monitoring service in the Azure platform, and consistent monitoring can be enforced through the use of [Azure Blueprints](/azure/governance/blueprints/) when deploying resources.</span></span>

## <a name="disaster-recovery"></a><span data-ttu-id="1be0c-145">재해 복구</span><span class="sxs-lookup"><span data-stu-id="1be0c-145">Disaster recovery</span></span>

<span data-ttu-id="1be0c-146">**기술적 위험**: 리소스 오류, 삭제 또는 손상은 결국 중요 업무용 애플리케이션 또는 서비스 중단 및 중요한 데이터의 손실로 이어질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-146">**Technical risk**: Resource failure, deletions, or corruption can result in disruption of mission-critical applications or services and the loss of sensitive data.</span></span>

<span data-ttu-id="1be0c-147">**정책 문**: 모든 중요 업무용 애플리케이션 및 보호되는 데이터에는 가동 중단 또는 시스템 오류로 인한 비즈니스 영향을 최소화하기 위한 백업 및 복구 솔루션이 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-147">**Policy statement**: All mission-critical applications and protected data must have backup and recovery solutions implemented to minimize business impact of outages or system failures.</span></span>

<span data-ttu-id="1be0c-148">**잠재적인 설계 옵션**: [Azure Site Recovery] 서비스는 BCDR(비즈니스 연속성 및 재해 복구) 시나리오에서 가동 중단 기간을 최소화하기 위한 백업, 복구 및 복제 기능을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-148">**Potential design options**: The [Azure Site Recovery] service provides backup, recovery, and replication capabilities intended to minimize outage duration in business continuity and disaster recovery (BCDR) scenarios.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1be0c-149">다음 단계</span><span class="sxs-lookup"><span data-stu-id="1be0c-149">Next steps</span></span>

<span data-ttu-id="1be0c-150">이 문서에서 설명한 샘플을 토대로 하여 특정 비즈니스 위험을 해결하고 클라우드 채택 계획에 적합한 정책을 개발합니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-150">Use the samples mentioned in this article as a starting point to develop policies that address specific business risks that align with your cloud adoption plans.</span></span>

<span data-ttu-id="1be0c-151">리소스 일관성과 관련된 고유한 사용자 지정 정책 문의 개발을 시작하려면 [리소스 일관성 템플릿](template.md)을 다운로드합니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-151">To begin developing your own custom policy statements related to Resource Consistency, download the [Resource Consistency template](template.md).</span></span>

<span data-ttu-id="1be0c-152">이러한 원칙을 신속하게 채택하려면 사용자 환경에 가장 적합한 [실행 가능한 거버넌스 경험](../journeys/overview.md)을 선택한 다음,</span><span class="sxs-lookup"><span data-stu-id="1be0c-152">To accelerate adoption of this discipline, choose the [Actionable Governance Journey](../journeys/overview.md) that most closely aligns with your environment.</span></span> <span data-ttu-id="1be0c-153">디자인을 수정하여 회사의 구체적인 정책 결정 사항을 통합합니다.</span><span class="sxs-lookup"><span data-stu-id="1be0c-153">Then modify the design to incorporate your specific corporate policy decisions.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1be0c-154">실행 가능한 거버넌스 경험</span><span class="sxs-lookup"><span data-stu-id="1be0c-154">Actionable Governance Journeys</span></span>](../journeys/overview.md)
