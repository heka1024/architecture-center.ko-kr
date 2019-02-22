---
title: 'CAF: 보안 기준 분야 개요'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 보안 기준 분야 개요
author: BrianBlanchard
layout: LandingPage
ms.topic: landing-page
ms.openlocfilehash: 81398ff943a9a582ae3cc29d9ff7519a0d1f8e54
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901595"
---
# <a name="caf-security-baseline-discipline-overview"></a>CAF: 보안 기준 분야 개요

보안 기준은 [CAF 거버넌스 모델](../overview.md)의 [5가지 클라우드 거버넌스 분야](../governance-disciplines.md) 중 하나입니다. 보안은 모든 IT 배포의 한 구성 요소이며 클라우드는 고유한 보안 문제를 야기합니다. 대부분의 비즈니스는 클라우드 변환을 고려할 때 중요한 데이터를 조직에서 최우선으로 보호해야 하는 규정 요구 사항의 적용을 받습니다. 클라우드 환경에 대한 잠재적인 보안 위협을 식별하고 이러한 위협을 해결하기 위한 프로세스와 절차를 수립하는 것이 IT 보안 또는 사이버 보안 팀의 최우선 과제입니다. 보안 기준 분야는 요구 사항이 더욱 까다로워지더라도 기술 요구 사항과 보안 제약 조건이 클라우드 환경에 지속적으로 적용되도록 보장합니다.

> [!NOTE]
> 보안 기준 거버넌스는 조직에서 클라우드 배포 리소스를 보호하는 데 사용하는 기존 IT 팀, 프로세스 및 절차를 대체하지 않습니다. 이 분야의 주된 목적은 보안 관련 비즈니스 위험을 파악하고 보안 인프라를 담당하는 IT 직원에게 위험 완화 지침을 제공하는 것입니다. 거버넌스 정책 및 프로세스를 개발할 때 관련 IT 팀이 계획 및 검토 프로세스에 참여해야 합니다.

이 문서에서는 클라우드 거버넌스 전략의 일환으로 보안 기준 분야를 개발하는 방법에 대해 간략하게 설명합니다. 이 지침의 주요 대상은 조직의 클라우드 설계자 및 클라우드 거버넌스 팀의 다른 구성원입니다. 그러나 이 분야에서 나오는 의사 결정, 정책 및 프로세스에는 IT 및 보안 팀의 관련 구성원, 특히 네트워킹, 암호화 및 ID 서비스 구현을 담당하는 기술 책임자의 참여 및 토론이 필요합니다.

클라우드 배포 및 광범위한 비즈니스 성공에 있어 적절한 보안 결정을 내리는 것이 중요합니다. 조직에서 사이버 보안에 대한 전문 지식을 보유한 직원이 없다면, 외부 보안 컨설턴트를 이 분야의 일원으로 참여시키는 것이 좋습니다. 또한 [Microsoft Consulting Services](https://www.microsoft.com/enterprise/services), [Microsoft FastTrack](https://azure.microsoft.com/programs/azure-fasttrack/) 클라우드 채택 서비스 또는 기타 외부 클라우드 채택 전문가와 협력하여 이 분야에 대한 문제를 논의하는 것도 좋습니다.

## <a name="policy-statements"></a>정책 설명

실행 가능한 정책 설명과 이에 따른 아키텍처 요구 사항은 보안 기준 분야의 기초가 됩니다. 정책 설명에 대한 샘플을 보려면 [보안 기준 정책 설명](./policy-statements.md) 문서를 참조하세요. 이러한 샘플을 토대로 조직 거버넌스 정책을 작성할 수 있습니다.

> [!CAUTION]
> 정책 샘플은 일반적인 고객 환경에서 제공됩니다. 이러한 정책을 특정 클라우드 거버넌스 요구 사항에 맞게 더 효율적으로 조정하려면 다음 단계를 실행하여 고유한 비즈니스 요구 사항이 충족되는 정책 설명을 만듭니다.

## <a name="developing-security-baseline-governance-policy-statements"></a>보안 기준 거버넌스 정책 설명 개발

다음 6단계에서는 보안 기준 거버넌스를 개발할 때 고려해야 할 잠재적 옵션과 이에 대한 예를 제공합니다. 각 단계를 클라우드 거버넌스 팀 내의 시작 논점으로 사용하여 영향을 받는 조직 전체 비즈니스, IT 및 보안 팀과 함께 보안 관련 위험을 완화하는 데 필요한 정책과 프로세스를 수립합니다.

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
                        <h3>보안 기준 템플릿</h3>
                        <p class="x-hidden-focus">보안 기준 분야를 문서화하는 템플릿을 다운로드합니다.</p>
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
                        <p class="x-hidden-focus">일반적으로 보안 기준 분야와 관련된 동기와 위험을 파악합니다.</p>
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
                        <p class="x-hidden-focus">보안 기준 분야에 투자할 적절한 시기인지 여부를 파악할 수 있는 지표입니다.</p>
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
                        <p class="x-hidden-focus">보안 기준 분야의 정책 준수를 지원하기 위해 제안되는 프로세스입니다.</p>
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
                        <p class="x-hidden-focus">보안 기준 분야를 지원하기 위해 구현할 수 있는 Azure 서비스입니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

<!-- markdownlint-enable MD033 -->

## <a name="next-steps"></a>다음 단계

특정 환경에서 비즈니스 위험을 평가하여 시작합니다.

> [!div class="nextstepaction"]
> [비즈니스 위험 이해](./business-risks.md)
