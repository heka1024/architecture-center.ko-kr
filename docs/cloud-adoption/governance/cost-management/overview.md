---
title: 'CAF: Cost Management 분야'
description: 클라우드 거버넌스와 관련된 Cost Management에 대해 설명합니다.
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
author: BrianBlanchard
layout: LandingPage
ms.topic: landing-page
ms.openlocfilehash: a86b1a75da57cec9c9aaf88abb70049f70731561
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58243704"
---
# <a name="cost-management-discipline-overview"></a>Cost Management 분야 개요

Cost Management는 [CAF 거버넌스 모델](../overview.md)의 [5개 클라우드 거버넌스 분야](../governance-disciplines.md) 중 하나입니다. 클라우드 기술을 채택할 때 거버넌스 비용은 많은 고객의 주요 관심 사항입니다. 성능 요구 사항, 채택 속도 및 클라우드 서비스 비용 간에 균형을 맞추는 것은 어려울 수 있습니다. 이는 클라우드 기술을 구현하는 주요 비즈니스 전환 과정에서 특히 관련이 있습니다. 이 섹션에서는 클라우드 거버넌스 전략의 일환으로 Cost Management 분야를 개발하는 방법에 대해 간략하게 설명합니다.  

> [!NOTE]
> Cost Management 거버넌스는 조직의 IT 관련 비용에 대한 재무 관리와 관련된 기존 비즈니스 팀, 회계 방법 및 절차를 대체하지 않습니다. 이 분야의 주요 목적은 IT 지출과 관련된 잠재적인 클라우드 관련 위험을 식별하고, 클라우드 배포를 구현 및 관리하는 비즈니스 및 IT 팀에 위험 완화 지침을 제공하는 것입니다. 거버넌스 정책 및 프로세스를 개발할 때 관련 비즈니스 및 IT 직원이 계획 및 검토 프로세스에 참여해야 합니다.

이 지침의 주요 대상은 조직의 클라우드 설계자 및 클라우드 거버넌스 팀의 다른 구성원입니다. 그러나 이 분야에서 나오는 의사 결정, 정책 및 프로세스에는 비즈니스 및 IT 팀의 관련 구성원, 특히 클라우드 기반 워크로드의 소유, 관리 및 비용 지급을 담당하는 책임자의 참여 및 토론이 필요합니다.

## <a name="policy-statements"></a>정책 설명

실행 가능한 정책 설명과 이에 따른 아키텍처 요구 사항은 Cost Management 분야의 기초가 됩니다. 정책 설명에 대한 샘플을 보려면 [Cost Management 정책 설명](./policy-statements.md) 문서를 참조하세요. 이러한 샘플은 조직의 거버넌스 정책에 대한 시작 지점이 될 수 있습니다.

> [!CAUTION]
> 정책 샘플은 일반적인 고객 환경에서 제공됩니다. 이러한 정책을 특정 클라우드 거버넌스 요구 사항에 맞게 더 효율적으로 조정하려면 다음 단계를 실행하여 고유한 비즈니스 요구 사항이 충족되는 정책 설명을 만듭니다.

## <a name="developing-cost-management-governance-policy-statements"></a>Cost Management 거버넌스 정책 설명 개발

환경의 비용을 제어하기 위한 거버넌스 정책을 정의하는 데 도움이 되는 6단계는 다음과 같습니다.

<!-- markdownlint-disable MD033 -->

<ul  class="panelContent cardsE">
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
                        <h3>Cost Management 템플릿</h3>
                        <p class="x-hidden-focus">Cost Management 분야를 문서화하는 템플릿을 다운로드합니다.</p>
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
                        <p class="x-hidden-focus">일반적으로 Cost Management 분야와 관련된 동기와 위험을 파악합니다.</p>
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
                        <p class="x-hidden-focus">Cost Management 분야에 투자할 적절한 시기인지 여부를 파악할 수 있는 지표입니다.</p>
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
                        <p class="x-hidden-focus">Cost Management 분야의 정책 준수를 지원하기 위해 제안되는 프로세스입니다.</p>
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
                        <p class="x-hidden-focus">클라우드 관리 완성도를 클라우드 도입 단계에 맞게 조정합니다.</p>
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
                        <p class="x-hidden-focus">Cost Management 분야를 지원하기 위해 구현할 수 있는 Azure 서비스입니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

## <a name="next-steps"></a>다음 단계

특정 환경에서 비즈니스 위험을 평가하여 시작합니다.

> [!div class="nextstepaction"]
> [비즈니스 위험 이해](./business-risks.md)

<!-- markdownlint-enable MD033 -->
