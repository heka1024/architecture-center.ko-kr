---
title: 'CAF: ID 기준 분야 개요'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 클라우드 거버넌스 관련 ID 기준에 대해 설명합니다.
author: BrianBlanchard
layout: LandingPage
ms.topic: landing-page
ms.openlocfilehash: 14569b8db68434ee30fa7bff7fba2da5fa537049
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58242034"
---
# <a name="caf-identity-baseline-discipline-overview"></a>CAF: ID 기준 분야 개요

ID 기준은 [CAF 거버넌스 모델](../overview.md)의 [5개 클라우드 거버넌스 분야](../governance-disciplines.md) 중 하나입니다. 네트워크 보안이 중요시되었던 기존의 방식과는 달리, ID가 클라우드의 기본 보안 경계로 간주되는 영역이 갈수록 증가하고 있습니다. ID 서비스는 IT 환경 내의 액세스 제어와 조직을 지원하는 핵심 메커니즘을 제공합니다. ID 기준 분야는 클라우드 채택 작업 전반에서 인증 및 권한 부여 요구 사항을 일관되게 적용하여 [보안 기준 분야](../security-baseline/overview.md)를 보완합니다.

> [!NOTE]
> ID 기준 거버넌스는 조직에서 ID 서비스를 관리하고 보호할 수 있도록 하는 기존 IT 팀, 프로세스 및 절차를 대체하지 않습니다. 이 분야의 주된 목적은 ID 관련 비즈니스 위험 가능성을 파악하고 ID 관리 인프라 구현, 유지 관리 및 운영을 담당하는 IT 담당자에게 위험 완화 지침을 제공하는 것입니다. 거버넌스 정책 및 프로세스를 개발할 때 관련 IT 팀이 계획 및 검토 프로세스에 참여해야 합니다.

CAF 지침의 이 섹션에서는 클라우드 거버넌스 전략의 일환으로 ID 기준 분야를 개발하는 방식에 대해 간략하게 설명합니다. 이 지침의 주요 대상은 조직의 클라우드 설계자 및 클라우드 거버넌스 팀의 다른 구성원입니다. 그러나 이 분야에서 관련 사항을 결정하고 정책과 프로세스를 작성할 때는 조직 ID 관리 솔루션 구현과 관리를 담당하는 IT 팀의 관련 구성원과의 협의 및 논의를 거쳐야 합니다.

조직에 ID 기준 및 보안 관련 전문 지식을 보유한 직원이 없다면 외부 컨설턴트를 이 분야의 일원으로 참여시키는 것이 좋습니다. 또한 [Microsoft Consulting Services](https://www.microsoft.com/enterprise/services), [Microsoft FastTrack](https://azure.microsoft.com/programs/azure-fasttrack) 클라우드 채택 서비스 또는 기타 외부 클라우드 채택 전문가와 협력하여 이 분야에 대한 문제를 논의하는 것도 좋습니다.

## <a name="policy-statements"></a>정책 설명

실행 가능한 정책 설명과 이에 따른 아키텍처 요구 사항은 ID 기준 분야의 기초가 됩니다. 정책 설명 샘플을 보려면 [ID 기준 정책 설명](./policy-statements.md) 문서를 참조하세요. 이러한 샘플을 토대로 조직 거버넌스 정책을 작성할 수 있습니다.

> [!CAUTION]
> 정책 샘플은 일반적인 고객 환경에서 제공됩니다. 이러한 정책을 특정 클라우드 거버넌스 요구 사항에 맞게 더 효율적으로 조정하려면 다음 단계를 실행하여 고유한 비즈니스 요구 사항이 충족되는 정책 설명을 만듭니다.

## <a name="developing-identity-baseline-governance-policy-statements"></a>ID 기준 거버넌스 정책 설명 개발

다음 6단계에서는 ID 기준 거버넌스를 개발할 때 고려해야 할 수 있는 옵션과 이에 대한 예를 제공합니다. 이러한 각 단계를 참조해 클라우드 거버넌스 팀 내에서, 그리고 영향을 받는 조직 반의 실무자 및 IT 팀과 논의를 진행하여 ID 관련 위험을 완화하는 데 필요한 정책과 프로세스를 수립합니다.

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
                        <h3>ID 기준 템플릿</h3>
                        <p class="x-hidden-focus">ID 기준 분야를 문서화하는 템플릿을 다운로드합니다.</p>
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
                        <p class="x-hidden-focus">일반적으로 ID 기준 분야와 관련된 동기와 위험을 파악합니다.</p>
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
                        <p class="x-hidden-focus">ID 기준 분야에 투자할 적절한 시기인지 여부를 파악할 수 있는 지표입니다.</p>
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
                        <p class="x-hidden-focus">ID 기준 분야의 정책 준수를 지원하기 위해 제안되는 프로세스입니다.</p>
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
                        <p class="x-hidden-focus">ID 기준 분야를 지원하기 위해 구현할 수 있는 Azure 서비스입니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

<!-- markdownlint-enable MD033 -->

## <a name="next-steps"></a>다음 단계

먼저 특정 환경의 [비즈니스 위험](./business-risks.md)을 평가합니다.

> [!div class="nextstepaction"]
> [비즈니스 위험 이해](./business-risks.md)
