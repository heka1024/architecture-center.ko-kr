---
title: Azure의 은행 간에 분산 트러스트
description: 중앙 집중식 데이터베이스로 재분류하지 않고 통신 및 정보 공유에 대해 신뢰할 수 있는 환경을 설정합니다.
author: vitoc
ms.date: 09/09/2018
ms.openlocfilehash: fe27f885635ce5ae4ce368992affa1a85d7af416
ms.sourcegitcommit: 62945777e519d650159f0f963a2489b6bb6ce094
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2018
ms.locfileid: "48876763"
---
# <a name="decentralized-trust-between-banks-on-azure"></a>Azure의 은행 간에 분산 트러스트

이 예제 시나리오는 중앙 집중식 데이터베이스에 의존하지 않고 정보 공유를 위한 신뢰할 수 있는 환경을 구축하려는 은행 또는 기타 기관에 유용합니다. 이 예제의 목적을 달성하기 위해, 은행 간 크레딧 점수 정보 유지의 관점에서 시나리오를 살펴볼 것이지만, 어느 한 당사자가 사용하는 중앙 시스템에 의존하지 않고 조직 컨소시엄에서 유효성이 검증된 정보를 서로 공유하려는 모든 시나리오에 이 아키텍처를 적용할 수 있습니다.

전통적으로 금융 시스템 내의 은행은 개인의 신용 점수 및 기록에 대한 정보를 얻기 위해 신용 조사 기관 같은 중앙 집중식 소스에 의존합니다. 중앙 집중식 방법은 운영 위험이 집중되고 때로는 불필요한 제3자가 개입됩니다.

DLT(분산된 원장 기술)를 사용하면 은행 컨소시엄은 보다 효율적이고, 공격에 대한 내성이 강하고, 혁신적인 구조체를 구현하여 개인 정보 보호, 속도 및 비용과 관련된 전통적인 과제를 해결하는 새로운 플랫폼 역할을 할 수 있는 분산 시스템을 만들 수 있습니다.

이 예제에서는 가상 머신 확장 집합, Virtual Network, Key Vault, Storage, Load Balancer, Monitor 등의 Azure 서비스를 신속하게 프로비전하여 멤버 은행이 고유의 노드를 설정할 수 있는 효율적인 비공개 Ethereum PoA 블록체인을 배포하는 방법을 보여줍니다.

## <a name="relevant-use-cases"></a>관련 사용 사례

다음과 같은 다른 사용 사례의 디자인 패턴도 비슷합니다.

* 다국적 회사의 여러 사업부 간에 할당된 예산 이동
* 국가 간 결제
* 무역 금융 시나리오
* 여러 회사가 관련된 로열티 시스템
* 공급망 에코시스템 등

## <a name="architecture"></a>아키텍처

![분산된 은행 트러스트 아키텍처 다이어그램](./media/architecture-decentralized-trust.png)

이 시나리오에서는 둘 이상의 멤버로 구성된 컨소시엄 내부에 확장 가능하고 안전하고 모니터링되는 비공개 엔터프라이즈 블록체인 네트워크를 만드는 데 필요한 백 엔드 구성 요소를 다룹니다. 이러한 구성 요소를 프로비전하는 방법(즉, 서로 다른 구독 및 리소스 그룹에 배치) 및 연결 요구 사항(즉, VPN 또는 ExpressRoute)에 대한 구체적인 내용은 조직의 정책 요구 사항에 따라 여러분이 고민해야 합니다. 다음은 데이터 흐름입니다.

1. A 은행은 JSON-RPC를 통해 블록체인 네트워크로 트랜잭션을 전송하여 개인의 신용 레코드를 생성/업데이트합니다.
2. 데이터는 A 은행의 비공개 응용 프로그램 서버에서 Azure 부하 분산 장치를 거쳐 가상 머신 확장 집합의 유효성 검사 노드 VM으로 흐릅니다.
3. Ethereum PoA 네트워크는 미리 정해진 시간(이 시나리오에서는 2초)에 블록을 만듭니다.
4. 트랜잭션은 생성된 블록에 번들되고 블록체인 네트워크에서 유효성이 검사됩니다.
5. B 은행은 마찬가지로 JSON-RPC를 통해 자체 노드와 통신하여 A 은행에서 만든 신용 레코드를 읽을 수 있습니다.

### <a name="components"></a>구성 요소

* Virtual Machine Scale Sets의 Virtual Machines는 블록체인에 대한 유효성 검사기 프로세스를 호스트하는 주문형 계산 시설을 제공합니다.
* Key Vault는 각 유효성 검사기의 개인 키에 대한 보안 저장소 시설로 사용됩니다.
* Load Balancer는 RPC, 피어링 및 거버넌스 DApp 요청을 분산합니다.
* 지속적인 네트워크 정보를 호스팅하고 임대를 조정하는 Storage
* Operations Management Suite(몇 가지 Azure 서비스 번들)는 사용 가능한 노드, 분당 트랜잭션 수 및 컨소시엄 멤버에 대한 인사이트를 제공합니다.

### <a name="alternatives"></a>대안

이 예제에서는 신뢰할 수 있고 분산되고 이해하기 쉬운 방법으로 정보를 서로 교환 및 공유할 수 있는 환경을 만들려는 조직 컨소시엄을 위한 좋은 진입점인 Ethereum PoA 접근 방식을 선택했습니다. 제공되는 Azure 솔루션 템플릿도 컨소시엄 리더가 Ethereum PoA 블록체인을 시작하고 컨소시엄의 멤버 조직이 자체 리소스 그룹 및 구독 내에서 자체 Azure 리소스를 가동하여 기존 네트워크에 조인할 수 있는 빠르고 간편한 방법을 제공합니다.

기타 확장 시나리오 또는 다른 시나리오의 경우 트랜잭션 개인 정보 보호 같은 우려가 발생할 수 있습니다. 예를 들어 보안 전송 시나리오에서 컨소시엄의 멤버가 다른 멤버에게도 트랜잭션을 보여주려 하지 않을 수 있습니다. 고유한 방식으로 이러한 문제를 해결하는 Ethereum PoA의 대안이 있습니다.

* Corda
* Quorum
* Hyperledger

## <a name="considerations"></a>고려 사항

### <a name="availability"></a>가용성

[Azure Monitor][monitor]는 가용성을 보장하기 위해 문제에 대한 블록체인 네트워크를 지속적으로 모니터링하는 데 사용됩니다. 이 시나리오에 사용되는 블록체인 솔루션 템플릿이 성공적으로 배포되는 즉시 Azure Monitor 기반의 사용자 지정 모니터링 대시보드에 대한 링크가 전송됩니다. 대시보드는 지난 30분의 하트비트와 기타 유용한 통계를 보고하는 노드를 보여줍니다. 

다른 가용성 항목에 대해서는 Azure 아키텍처 센터의 [가용성 검사 목록][availability]을 참조하세요.

### <a name="scalability"></a>확장성

블록체인에 대한 일반적인 우려는 미리 정해진 시간 내에 블록체인이 포함할 수 있는 트랜잭션의 수입니다. 이 시나리오에서는 이러한 확장성을 작업 증명보다 잘 관리할 수 있는 인증 증명을 사용합니다. 인증 증명 기반 네트워크에서는 합의 참가자가 알려져 있고 관리되기 때문에 서로 알고 있는 조직 컨소시엄을 위한 비공개 블록체인에 더 적합합니다. 사용자 지정 대시보드를 통해 평균 블록 시간, 분당 트랜잭션, 계산 리소스 사용량 등의 매개 변수를 간편하게 모니터링할 수 있습니다. 규모 요구 사항에 따라 리소스를 조정할 수 있습니다.

확장 가능한 시나리오 설계에 대한 일반적인 지침은 Azure 아키텍처 센터의 [확장성 검사 목록][scalability]을 참조하세요.

### <a name="security"></a>보안

[Azure Key Vault][vault]는 유효성 검사기의 개인 키를 간편하게 저장하고 관리하는 데 사용됩니다. 이 예제의 기본 배포는 인터넷을 통해 액세스할 수 있는 블록체인 네트워크를 만듭니다. 비공개 네트워크를 원하는 프로덕션 시나리오의 경우 VNet-VNet VPN 게이트웨이 연결을 통해 멤버를 서로 연결할 수 있습니다. VPN을 구성하는 단계는 아래의 관련 리소스 섹션에 포함되어 있습니다.

보안 솔루션 설계에 대한 일반적인 지침은 [Azure 보안 설명서][security]를 참조하세요.

### <a name="resiliency"></a>복원력

유효성 검사기 노드를 다른 지역에 배포할 수 있으므로 Ethereum PoA 블록체인은 그 자체로 일정 수준의 복원력을 제공할 수 있습니다. Azure는 전 세계의 54개가 넘는 지역에 배포할 수 있는 옵션을 제공합니다. 이 시나리오와 비슷한 블록체인은 협력을 통해 복원력을 높이는 고유하고 신선한 가능성을 제공합니다. 네트워크 복원력은 중앙 집중식 단일 멤버가 아닌 컨소시엄의 모든 멤버를 통해 제공됩니다. 인증 증명 기반 블록체인을 사용하면 네트워크를 훨씬 계획적이고 의도적으로 복원할 수 있습니다.

복원력 있는 솔루션 설계에 대한 일반적인 지침은 [복원력 있는 Azure 응용 프로그램 디자인][resiliency]을 참조하세요.

## <a name="pricing"></a>가격

이 시나리오를 실행하는 데 들어가는 비용을 알아보기 위해 모든 서비스가 비용 계산기에서 미리 구성됩니다. 특정 사용 사례에 대한 가격 변동을 확인하려면 예상 성능 및 가용성 요구 사항에 맞게 변수를 적절하게 변경합니다.

응용 프로그램을 실행하는 확장 집합 VM 인스턴스의 수를 기준으로 세 가지 샘플 비용 프로필이 제공되었습니다(인스턴스가 다른 지역에 상주할 수 있음).

* [소형][small-pricing]: 이 가격 책정 예제는 매월 모니터링이 꺼진 VM 2대와 관련이 있습니다.
* [중형][medium-pricing]: 이 가격 책정 예제는 매월 모니터링이 켜진 VM 7대와 관련이 있습니다.
* [대형][large-pricing]: 이 가격 책정 예제는 매월 모니터링이 켜진 VM 15대와 관련이 있습니다.

위의 가격 책정은 한 컨소시엄 멤버가 블록체인 네트워크를 시작 또는 조인하기 위한 것입니다. 일반적으로 여러 회사 또는 조직이 관련되는 컨소시엄의 각 멤버는 고유의 Azure 구독을 받습니다.

## <a name="next-steps"></a>다음 단계

이 시나리오의 예제를 보려면 Azure에 [Ethereum PoA 블록체인 데모 응용 프로그램][deploy]을 배포한 다음, [시나리오 소스 코드의 README][source]를 진행합니다.

## <a name="related-resources"></a>관련 리소스

Azure에 Ethereum 인증 증명 솔루션 템플릿을 사용하는 방법에 대한 자세한 내용은 이 [사용 가이드][guide]를 참조하세요.

<!-- links -->
[small-pricing]: https://azure.com/e/4e429d721eb54adc9a1558fae3e67990
[medium-pricing]: https://azure.com/e/bb42cd77437744be8ed7064403bfe2ef
[large-pricing]: https://azure.com/e/e205b443de3e4adfadf4e09ffee30c56
[guide]: /azure/blockchain-workbench/ethereum-poa-deployment
[deploy]: https://portal.azure.com/?pub_source=email&pub_status=success#create/microsoft-azure-blockchain.azure-blockchain-ethereumethereum-poa-consortium
[source]: https://github.com/vitoc/creditscoreblockchain
[monitor]: /azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor
[availability]: /azure/architecture/checklist/availability
[scalability]: /azure/architecture/checklist/scalability
[resiliency]: ../../resiliency/index.md
[security]: /azure/security/
[vault]: https://azure.microsoft.com/services/key-vault/
