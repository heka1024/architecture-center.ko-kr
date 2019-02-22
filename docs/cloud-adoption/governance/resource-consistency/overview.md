---
title: 'CAF: 리소스 일관성 분야 개요'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 리소스 일관성 분야 개요
author: BrianBlanchard
layout: LandingPage
ms.topic: landing-page
ms.openlocfilehash: ce29efab1502d59513528ab4b640173346aee516
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901605"
---
# <a name="caf-resource-consistency-discipline-overview"></a>CAF: 리소스 일관성 분야 개요

리소스 일관성은 [CAF 거버넌스 모델](../overview.md)의 [5가지 클라우드 거버넌스 분야](../governance-disciplines.md) 중 하나입니다. 이 분야에서는 환경, 애플리케이션 또는 워크로드의 작업 관리와 관련된 정책을 설정하는 방법에 중점을 둡니다. IT 운영 팀은 보통 애플리케이션, 워크로드 및 자산 성능에 대한 모니터링을 제공합니다. 또한 일반적으로 크기 조정 요구 사항을 충족시키고, 성능 SLA(서비스 수준 계약) 위반을 수정하며, 자동화된 업데이트 관리를 통해 성능 SLA 위반을 사전에 방지하는 데 필요한 작업을 실행합니다. 클라우드 거버넌스의 5가지 분야 내에서 리소스 일관성은 리소스가 IT 운영에서 검색 가능하고 복구 솔루션에 포함되며 반복 가능한 운영 프로세스에 온보딩할 수 있는 방식으로 일관되게 구성되도록 해주는 분야입니다.

> [!NOTE]
> 리소스 일관성 거버넌스는 조직에서 클라우드 기반 리소스를 효과적으로 관리할 수 있도록 하는 기존 IT 팀, 프로세스 및 절차를 대체하지 않습니다. 이 분야의 주된 목적은 잠재적인 비즈니스 위험을 파악하고 클라우드에서 리소스 관리를 담당하는 IT 직원에게 위험 완화 지침을 제공하는 것입니다. 거버넌스 정책 및 프로세스를 개발할 때 관련 IT 팀이 계획 및 검토 프로세스에 참여해야 합니다.

CAF의 이 섹션에서는 클라우드 거버넌스 전략의 일환으로 리소스 일관성 분야를 개발하는 방법에 대해 간략하게 설명합니다. 이 지침의 주요 대상은 조직의 클라우드 설계자 및 클라우드 거버넌스 팀의 다른 구성원입니다. 그러나 이 분야에서 관련 사항을 결정하고 정책과 프로세스를 작성할 때는 조직의 리소스 일관성 솔루션 구현과 관리를 담당하는 IT 팀의 관련 구성원과의 협의 및 논의를 거쳐야 합니다.

조직에 리소스 일관성 전략 관련 전문 지식을 보유한 직원이 없다면 외부 컨설턴트를 이 분야의 일원으로 참여시키는 것이 좋습니다. 또한 [Microsoft Consulting Services](https://www.microsoft.com/enterprise/services), [Microsoft FastTrack](https://azure.microsoft.com/programs/azure-fasttrack) 클라우드 채택 서비스 또는 기타 외부 클라우드 채택 전문가와 협력하여 클라우드 기반 자산을 구성, 추적 및 최적화하는 최선의 방법을 논의하는 것도 좋습니다.

## <a name="policy-statements"></a>정책 설명

실행 가능한 정책 설명과 이에 따른 아키텍처 요구 사항은 리소스 일관성 분야의 기초가 됩니다. 정책 설명 샘플을 보려면 [리소스 일관성 정책 설명](./policy-statements.md) 문서를 참조하세요. 이러한 샘플을 토대로 조직 거버넌스 정책을 작성할 수 있습니다.

> [!CAUTION]
> 정책 샘플은 일반적인 고객 환경에서 제공됩니다. 이러한 정책을 특정 클라우드 거버넌스 요구 사항에 맞게 더 효율적으로 조정하려면 다음 단계를 실행하여 고유한 비즈니스 요구 사항이 충족되는 정책 설명을 만듭니다.

## <a name="developing-resource-consistency-governance-policy-statements"></a>리소스 일관성 거버넌스 정책 설명 개발

다음 6단계에서는 리소스 일관성 거버넌스를 개발할 때 고려해야 할 수 있는 옵션과 이에 대한 예를 제공합니다. 이러한 각 단계를 참조해 클라우드 거버넌스 팀 내에서, 그리고 영향을 받는 조직 반의 실무자 및 IT 팀과 논의를 진행하여 리소스 일관성 위험을 완화하는 데 필요한 정책과 프로세스를 수립합니다.

<!-- markdownlint-disable MD033 -->

<ul class="panelContent cardsE">
<li style="display: flex; flex-direction: column;">
    <a href="./template.md">
        <div class="cardSize">
            <div class="cardPadding" >
                <div class="card" >
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../../_images/governance/process-template.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText" style="padding-left:0px;">
                        <h3>리소스 일관성 템플릿</h3>
                        <p class="x-hidden-focus">리소스 일관성 분야를 문서화하는 템플릿을 다운로드합니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li><li style="display: flex; flex-direction: column;">
    <a href="./business-risks.md">
        <div class="cardSize">
            <div class="cardPadding" >
                <div class="card" >
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../../_images/governance/process-risks.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText" style="padding-left:0px;">
                        <h3>비즈니스 위험</h3>
                        <p class="x-hidden-focus">일반적으로 리소스 일관성 분야와 관련된 동기와 위험을 파악합니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./metrics-tolerance.md">
        <div class="cardSize">
            <div class="cardPadding" >
                <div class="card" >
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../../_images/governance/process-metrics.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText" style="padding-left:0px;">
                        <h3>지표 및 메트릭</h3>
                        <p class="x-hidden-focus">리소스 일관성 분야에 투자할 적절한 시기인지 여부를 파악할 수 있는 지표입니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./compliance-processes.md">
        <div class="cardSize">
            <div class="cardPadding" >
                <div class="card" >
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../../_images/governance/process-enforce.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText" style="padding-left:0px;">
                        <h3>정책 준수 프로세스</h3>
                        <p class="x-hidden-focus">리소스 일관성 분야의 정책 준수를 지원하기 위해 제안되는 프로세스입니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./discipline-improvement.md">
        <div class="cardSize">
            <div class="cardPadding" >
                <div class="card" >
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../../_images/governance/process-maturity.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText" style="padding-left:0px;">
                        <h3>완성도</h3>
                        <p class="x-hidden-focus">클라우드 관리 완성도를 클라우드 채택 단계에 맞게 조정합니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./toolchain.md">
        <div class="cardSize">
            <div class="cardPadding" >
                <div class="card" >
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../../_images/governance/process-toolchain.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText" style="padding-left:0px;">
                        <h3>도구 체인</h3>
                        <p class="x-hidden-focus">리소스 일관성 분야를 지원하기 위해 구현할 수 있는 Azure 서비스입니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

## <a name="next-steps"></a>다음 단계

먼저 특정 환경의 [비즈니스 위험](./business-risks.md)을 평가합니다.

> [!div class="nextstepaction"]
> [비즈니스 위험 이해](./business-risks.md)
