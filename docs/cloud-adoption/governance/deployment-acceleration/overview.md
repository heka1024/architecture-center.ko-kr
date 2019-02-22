---
title: 'CAF: 배포 가속화 분야 개요'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 2/11/2019
description: 클라우드 거버넌스 관련 배포 가속화에 대한 설명입니다.
author: BrianBlanchard
layout: LandingPage
ms.topic: landing-page
ms.openlocfilehash: ba48bb32292f0bb27ed83550483ceecd725071ff
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901848"
---
# <a name="caf-deployment-acceleration-discipline-overview"></a>CAF: 배포 가속화 분야 개요

배포 가속화는 [CAF 거버넌스 모델](../overview.md)의 [5개 클라우드 거버넌스 분야](../governance-disciplines.md) 중 하나입니다. 이 분야는 자산 구성 또는 배포를 제어하는 정책을 설정하는 방법에 중점을 둡니다. 클라우드 거버넌스 의 5개 분야에서, 배포 가속화에는 배포, 구성 맞춤 및 스크립트 재사용이 포함됩니다. 수동 작업 또는 완전히 자동화된 DevOps 작업을 통해 수행될 수 있습니다. 두 경우 모두 정책은 대체로 동일하게 유지됩니다. 이 분야가 성숙해지면 클라우드 거버넌스 팀은 재사용 가능한 자산을 적용하여 배포를 가속화하고 클라우드 도입의 장애물을 제거함으로써 DevOps 및 배포 전략에서 파트너 역할을 수행할 수 있습니다.

이 문서에서는 클라우드 솔루션 구현을 계획, 구축, 도입 및 운영하는 단계에서 회사가 겪는 배포 가속화 프로세스를 설명합니다. 한 문서로 모든 비즈니스 요구 사항을 설명하는 것은 불가능합니다. 따라서, 이 문서의 각 섹션에서는 제안된 최소 활동과 잠재적인 활동이 간략하게 설명되어 있습니다. 이러한 활동의 목표는 [정책 MVP](../policy-compliance/overview.md#minimum-viable-product-mvp-for-policy)를 구축하되, [점진적 정책](../policy-compliance/overview.md#incremental-policy-growth) 전개를 위한 프레임워크를 설정하는 것입니다. 클라우드 거버넌스 팀은 배포 가속화 위치를 개선하기 위해 이러한 활동에 얼마나 투자할 지를 결정해야 합니다.

> [!NOTE]
> 배포 가속화 분야는 조직에서 클라우드 기반 리소스를 효과적으로 배포하고 구성할 수 있는 기존 IT 팀, 프로세스 및 절차를 대체하지 않습니다. 이 분야의 주된 목적은 잠재적인 비즈니스 위험을 파악하고 클라우드에서 리소스 관리를 담당하는 IT 직원에게 위험 완화 지침을 제공하는 것입니다. 거버넌스 정책 및 프로세스를 개발할 때 관련 IT 팀이 계획 및 검토 프로세스에 참여해야 합니다.

이 지침의 주요 대상은 조직의 클라우드 설계자 및 클라우드 거버넌스 팀의 다른 멤버입니다. 그러나 이 분야에서 나오는 의사 결정, 정책 및 프로세스에는 비즈니스 및 IT 팀의 관련 멤버, 특히 클라우드 기반 워크로드의 배포 및 구성을 담당하는 책임자의 참여 및 토론이 필요합니다.

## <a name="policy-statements"></a>정책 설명

실행 가능한 정책 설명과 이에 따른 아키텍처 요구 사항은 배포 가속화 분야의 기초가 됩니다. 정책 설명 샘플을 보려면 [배포 가속화 정책 설명](./policy-statements.md) 문서를 참조하세요. 이러한 샘플을 토대로 조직 거버넌스 정책을 작성할 수 있습니다.

> [!CAUTION]
> 정책 샘플은 일반적인 고객 환경에서 제공됩니다. 이러한 정책을 특정 클라우드 거버넌스 요구 사항에 맞게 더 효율적으로 조정하려면 다음 단계를 실행하여 고유한 비즈니스 요구 사항이 충족되는 정책 설명을 작성하십시오.

## <a name="developing-deployment-acceleration-governance-policy-statements"></a>배포 가속화 거버넌스 정책 설명 개발

다음 6단계를 통해 클라우드 환경에서 리소스 배포 및 구성을 제어하는 거버넌스 정책을 정의할 수 있습니다.

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
                        <h3>배포 가속화 템플릿</h3>
                        <p class="x-hidden-focus">배포 가속화 분야를 문서화할 수 있는 템플릿을 다운로드합니다.</p>
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
                        <p class="x-hidden-focus">일반적으로 배포 가속화 분야와 관련된 동기와 위험을 파악합니다.</p>
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
                        <p class="x-hidden-focus">배포 가속화 분야에 투자하기에 적절한 시기인지 파악할 수 있는 지표입니다.</p>
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
                        <p class="x-hidden-focus">배포 가속화 분야의 정책 준수를 지원하기 위해 제안되는 프로세스입니다.</p>
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
                        <p class="x-hidden-focus">배포 가속화 분야를 지원하도록 구현할 수 있는 Azure 서비스입니다.</p>
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

<!-- markdownlint-enable MD033 -->