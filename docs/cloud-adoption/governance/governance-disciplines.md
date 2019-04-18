---
title: 'CAF: 클라우드 거버넌스의 5분야'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: CAF 내 거버넌스 콘텐츠에 대한 개요입니다.
author: BrianBlanchard
layout: LandingPage
ms.topic: landing-page
ms.openlocfilehash: ddd7f5e4b7e79ebe2ddf87be7421b6c4691aced1
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59639872"
---
# <a name="the-five-disciplines-of-cloud-governance"></a>클라우드 거버넌스의 5분야

<!-- markdownlint-disable MD033 -->

<ul class="panelContent cardsI">
<li style="display: flex; flex-direction: column;">
    <div class="cardSize">
        <div class="cardPadding" style="padding-bottom:10px;">
            <div class="card" style="padding-bottom:10px;">
                <div class="cardText" style="padding-left:0px;">
비즈니스 프로세스 또는 기술 플랫폼을 변경하면 위험이 발생합니다. 클라우드 거버넌스 팀은 클라우드 보유자라고도 하며, 채택 또는 혁신 노력의 중단을 최소화하면서 이러한 위험을 완화해야 합니다.<br/><br/>회사 정책 개발에 집중 하 여 (선택한 클라우드 플랫폼)에 관계 없이 이러한 결정을 안내 하는 카페 거 버 넌 스 모델 및 <a href="#disciplines-of-cloud-governance">분야의 클라우드 거 버 넌 스</a>합니다. <a href="./journeys/overview.md">실행 가능한 경험</a> Azure 서비스를 사용 하 여이 모델을 보여 줍니다. 이 문서는 CAF 거버넌스 모델의 5개 분야에 대한 방문 페이지 역할을 합니다.
                </div>
            </div>
        </div>
    </div>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="../_images/operational-transformation-govern-highres.png" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize">
            <div class="cardPadding" style="padding-bottom:10px;">
                <div class="card" style="padding-bottom:10px;">
                    <div class="cardText" style="padding-left:0px;">
<img src="../_images/operational-transformation-govern-highres.png" alt="Diagram of the CAF governance model: Corporate policy and governance disciplines">
<br>
<i>그림 1. 회사 정책 및는 5 개 분야의 클라우드 거 버 넌 스 다이어그램</i>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

<!-- markdownlint-enable MD033 -->

## <a name="disciplines-of-cloud-governance"></a>클라우드 거버넌스 분야

각 클라우드 공급자에서 정책 하 고 도구 체인을 정렬 하는 데 가이드로 사용 될 수 있는 일반적인 거 버 넌 스 분야 있습니다. 이러한 분야는 적절한 수준의 자동화 및 클라우드 공급자 전반에 걸친 회사 정책 적용에 관한 결정을 안내합니다.

<!-- markdownlint-disable MD033 -->

<ul class="panelContent cardsA">
<li style="display: flex; flex-direction: column;">
    <a href="./cost-management/overview.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../_images/governance/cost-management.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Cost Management</h3>
                        <p>비용은 클라우드 사용자의 주요 관심 사항입니다. 모든 클라우드 플랫폼에 대한 비용 관리 정책을 개발합니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./security-baseline/overview.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../_images/governance/security-baseline.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>보안 기준</h3>
                        <p>보안은 복잡하고 개인적인 주제이며 각 회사마다 고유합니다. 보안 요구 사항이 확정되면 클라우드 거버넌스 정책 및 적용에서 네트워크, 데이터 및 자산 구성에 걸쳐 이러한 요구 사항을 적용합니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./identity-baseline/overview.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../_images/governance/identity-baseline.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>ID 기준</h3>
                        <p>ID 요구 사항의 적용이 일관되지 않으면 위반 위험이 높아질 수 있습니다. ID 기준 분야는 클라우드 채택 활동에서 ID가 일관되게 적용되도록 하는 방법에 중점을 둡니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./resource-consistency/overview.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../_images/governance/resource-consistency.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>리소스 일관성</h3>
                        <p>클라우드 작업은 리소스 구성의 일관성에 따라 달라집니다. 거버넌스 도구를 통해 리소스를 일관되게 구성하여 온보딩, 드리프트, 검색 기능 및 복구와 관련된 위험을 완화할 수 있습니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./deployment-acceleration/overview.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../_images/governance/deployment-acceleration.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>배포 가속</h3>
                        <p>배포 및 구성 방법의 중앙 집중화, 표준화 및 일관성을 통해 거버넌스 사례가 향상됩니다. 클라우드 기반 거버넌스 도구를 통해 사용할 수 있게 되면 배포 작업을 가속화할 수 있는 클라우드 요소를 만듭니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

<!-- markdownlint-enable MD033 -->
