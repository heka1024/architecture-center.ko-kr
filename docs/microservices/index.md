---
title: Azure에서 마이크로 서비스 구축
description: 'Azure에서 마이크로서비스 아키텍처 디자인, 빌드 및 운영'
ms.date: 03/07/2019
layout: LandingPage
ms.topic: landing-page
---

# <a name="building-microservices-on-azure"></a>Azure에서 마이크로 서비스 구축

<!-- markdownlint-disable MD033 -->

<img src="../_images/microservices.svg" style="float:left; margin-top:8px; margin-right:8px; max-width: 80px; max-height: 80px;"/>

마이크로서비스는 복원력 있고 확장성이 뛰어나고 독립적으로 배포할 수 있고 신속하게 진화할 수 있는 애플리케이션을 빌드하는 데 널리 사용되는 아키텍처 스타일입니다. 하지만 성공적인 마이크로서비스 아키텍처를 만들려면 애플리케이션을 설계하고 구축하는 다른 방법이 필요합니다.

<ul  class="panelContent cardsZ">
<li style="display: flex; flex-direction: column;">
    <a href="./introduction.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>마이크로서비스란?</h3>
                        <p>마이크로서비스는 다른 아키텍처와 어떤 차이가 있으며 언제 사용하나요?</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="../guide/architecture-styles/microservices.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>마이크로 서비스 아키텍처 스타일</h3>
                        <p>마이크로서비스 아키텍처 스타일에 대한 개략적인 개요</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

## <a name="examples-of-microservices-architectures"></a>마이크로서비스 아키텍처 예제

<ul  class="panelContent cardsZ">
<li style="display: flex; flex-direction: column;">
    <a href="../example-scenario/infrastructure/service-fabric-microservices.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>Service Fabric을 사용하여 모놀리식 애플리케이션 분해</h3>
                        <p>ASP.NET 웹 사이트를 마이크로서비스로 분해하는 반복 접근 방식입니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="../example-scenario/data/ecommerce-order-processing.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>Azure의 확장성 있는 주문 처리</h3>
                        <p>마이크로서비스를 통해 구현된 함수형 프로그래밍 모델을 사용하여 주문을 처리합니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

## <a name="build-a-microservices-application"></a>마이크로서비스 애플리케이션 빌드

<ul  class="panelContent cardsZ">
<li style="display: flex; flex-direction: column;">
    <a href="./model/domain-analysis.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>도메인 분석을 사용하여 마이크로서비스 모델링</h3>
                        <p>도메인 분석을 사용하여 마이크로서비스 경계를 정의하면 마이크로서비스를 디자인할 때 자주 하는 실수를 피할 수 있습니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="../reference-architectures/microservices/aks.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>AKS(Azure Kubernetes Service)의 참조 아키텍처</h3>
                        <p>이 참조 아키텍처는 대부분의 배포에서 시작점으로 사용할 수 있는 기본 AKS 구성을 보여줍니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="../reference-architectures/microservices/service-fabric.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>Azure Service Fabric의 참조 아키텍처</h3>
                        <p>이 참조 아키텍처는 대부분의 배포에서 시작점으로 사용할 수 있는 권장 구성을 보여줍니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./design/index.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>마이크로서비스 아키텍처 디자인</h3>
                        <p>다음 문서에서는 AKS(Azure Kubernetes Service)를 사용하는 참조 구현을 기반으로 마이크로서비스 애플리케이션을 빌드하는 방법을 자세히 살펴봅니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./design/patterns.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>디자인 패턴</h3>
                        <p>유용한 마이크로서비스 디자인 패턴입니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

## <a name="operate-microservices-in-production"></a>프로덕션 환경에서 마이크로서비스 작동

<ul  class="panelContent cardsZ">
<li style="display: flex; flex-direction: column;">
    <a href="./logging-monitoring.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>로깅 및 모니터링</h3>
                        <p>마이크로서비스 아키텍처의 분산 특성 때문에 로깅 및 모니터링이 특히 중요합니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./ci-cd.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>지속적인 통합 및 배포</h3>
                        <p>CI/CD(지속적인 통합 및 지속적인 업데이트)는 성공적인 마이크로서비스 구현을 위한 핵심 요소입니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>
