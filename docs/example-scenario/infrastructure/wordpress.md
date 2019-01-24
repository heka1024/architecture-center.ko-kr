---
title: 확장성이 높고 안전한 WordPress 웹 사이트
titleSuffix: Azure Example Scenarios
description: 미디어 이벤트에 대해 확장성이 높고 안전한 WordPress 웹 사이트를 빌드합니다.
author: david-stanford
ms.date: 09/18/2018
ms.topic: example-scenario
ms.service: architecture-center
ms.subservice: example-scenario
ms.openlocfilehash: 22297c5f908bd52a064048fcfebb07ebab1f4d23
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54488548"
---
# <a name="highly-scalable-and-secure-wordpress-website"></a>확장성이 높고 안전한 WordPress 웹 사이트

이 예제 시나리오는 확장성이 높고 안전한 WordPress를 설치해야 하는 회사에 적용됩니다. 이 시나리오는 큰 규칙에 사용되었으며 세션에서 사이트로 전달한 트래픽 급증을 충족하도록 크기를 조정할 수 있었던 배포를 기반으로 합니다.

## <a name="relevant-use-cases"></a>관련 사용 사례

관련된 다른 사용 사례는 다음과 같습니다.

- 트래픽 급증을 일으키는 미디어 이벤트.
- 콘텐츠 관리 시스템으로 WordPress를 사용하는 블로그.
- WordPress를 사용하는 비즈니스 또는 전자상거래 웹 사이트.
- 다른 콘텐츠 관리 시스템을 사용하여 빌드된 웹 사이트입.

## <a name="architecture"></a>아키텍처

[![확장 가능하고 안전한 WordPress 배포와 관련된 Azure 구성 요소의 아키텍처 개요](media/secure-scalable-wordpress.png)](media/secure-scalable-wordpress.png#lightbox)

이 시나리오에서는 Ubuntu 웹 서버 및 MariaDB를 사용하는 확장 가능하고 안전한 WordPress 설치에 대해 다룹니다. 이 시나리오에는 별도의 데이터 흐름 두 개가 있는데, 첫 번째는 웹 사이트에 대한 사용자 액세스입니다.

1. 사용자는 CDN을 통해 프런트 엔드 웹 사이트에 액세스합니다.
2. CDN은 Azure 부하 분산 장치를 원본으로 사용하며, 여기서 캐시되지 않은 데이터를 끌어옵니다.
3. Azure 부하 분산 장치는 웹 서버의 가상 머신 확장 집합에 요청을 분산합니다.
4. WordPress 애플리케이션은 Maria DB 클러스터에서 모든 동적 정보를 끌어오고, 모든 정적 콘텐츠는 Azure Files에 호스트됩니다.
5. SSL 키는 Azure Key Vault에 저장됩니다.

두 번째 워크플로는 작성자가 새 콘텐츠를 제공하는 방법입니다.

1. 작성자가 공용 VPN 게이트웨이에 안전하게 연결합니다.
2. VPN 인증 정보가 Azure Active Directory에 저장됩니다.
3. 그 후 관리 점프 상자에 대한 연결이 설정됩니다.
4. 그러면 작성자는 관리자 점프 상자에서 Azure 부하 분산 장치에 연결하여 클러스터를 작성할 수 있습니다.
5. Azure 부하 분산 장치는 Maria DB 클러스터에 대한 쓰기 권한이 있는 웹 서버의 가상 머신 확장 집합에 트래픽을 분산합니다.
6. 새 정적 콘텐츠는 Azure 파일에 업로드되고 동적 콘텐츠는 Maria DB 클러스터에 기록됩니다.
7. 이러한 변경 내용이 rsync 또는 마스터/슬레이브 복제를 통해 대체 지역에 복제됩니다.

### <a name="components"></a>구성 요소

- [Azure CDN(콘텐츠 전송 네트워크)](/azure/cdn/cdn-overview)은 사용자에게 웹 콘텐츠를 효율적으로 제공하는 서버의 분산 네트워크입니다. CDN은 최종 사용자와 가까운 POP(point-of-presence) 위치의 에지 서버에 캐시된 콘텐츠를 저장하여 대기 시간을 최소화합니다.
- [가상 네트워크](/azure/virtual-network/virtual-networks-overview)는 VM 같은 리소스가 상호 간 통신, 인터넷 통신 및 온-프레미스 네트워크 통신을 안전하게 수행할 수 있게 해줍니다. 가상 네트워크는 격리 및 세분화를 제공하고, 트래픽을 필터링 및 라우팅하며, 위치 간 연결을 허용합니다. 두 네트워크는 Vnet 피어링을 통해 연결됩니다.
- [네트워크 보안 그룹](/azure/virtual-network/security-overview)에는 원본 또는 대상 IP 주소, 포트 및 프로토콜에 따라 인바운드 또는 아웃바운드 네트워크 트래픽을 허용하거나 거부하는 보안 규칙 목록이 포함되어 있습니다. 이 시나리오의 가상 네트워크는 애플리케이션 구성 요소 간의 트래픽 흐름을 제한하는 네트워크 보안 그룹 규칙으로 보호됩니다.
- [부하 분산 장치](/azure/load-balancer/load-balancer-overview)는 규칙 및 상태 프로브에 따라 인바운드 트래픽을 분산합니다. 부하 분산 장치는 짧은 대기 시간과 높은 처리량을 제공하고, 모든 TCP 및 UDP 애플리케이션에 대해 최대 수백만 개의 흐름으로 확장합니다. 부하 분산 장치는 이 시나리오에 사용되어 콘텐츠 전송 네트워크의 트래픽을 프런트 엔드 웹 서버로 분산합니다.
- [가상 머신 확장 집합][docs-vmss]을 사용하면 부하 분산된 동일한 VM 그룹을 만들고 관리할 수 있습니다. VM 인스턴스의 수는 요구 또는 정의된 일정에 따라 자동으로 늘리거나 줄일 수 있습니다. 이 시나리오에는 별도의 가상 머신 확장 집합 두 개가 사용되는데, 하나는 콘텐츠를 서비스하는 프런트 엔드 웹 서버용이고, 다른 하나는 새 콘텐츠를 작성하는 데 사용되는 프런트 엔드 웹 서버용입니다.
- 모든 VM이 데이터에 액세스할 수 있도록, [Azure Files](/azure/storage/files/storage-files-introduction)는 이 시나리오의 모든 WordPress 콘텐츠를 호스트하는 완전 관리 파일 공유를 클라우드에 제공합니다.
- [Azure Key Vault](/azure/key-vault/key-vault-overview)는 암호, 인증서 및 키를 저장하고 액세스를 철저하게 제어하는 데 사용됩니다.
- [Azure AD(Azure Active Directory)](/azure/active-directory/fundamentals/active-directory-whatis)는 다중 테넌트 클라우드 기반 디렉터리 및 ID 관리 서비스입니다. 이 시나리오에서 Azure AD는 웹 사이트 및 VPN 터널에 대한 인증 서비스를 제공합니다.

### <a name="alternatives"></a>대안

- [Linux용 SQL Server](/azure/virtual-machines/linux/sql/sql-server-linux-virtual-machines-overview)는 MariaDB 데이터 저장소를 대체할 수 있습니다.
- 완전 관리 솔루션을 선호하는 경우 [MySQL용 Azure 데이터베이스](/azure/mysql/overview)로 MariaDB 데이터 저장소를 대체할 수 있습니다.

## <a name="considerations"></a>고려 사항

### <a name="availability"></a>가용성

이 시나리오의 VM 인스턴스는 여러 지역에 배포되며, WordPress 콘텐츠용 RSYNC와 MariaDB 클러스터용 마스터 슬레이브 복제를 통해 두 지역 간에 데이터가 복제됩니다.

다른 가용성 항목에 대해서는 Azure 아키텍처 센터의 [가용성 검사 목록][availability]을 참조하세요.

### <a name="scalability"></a>확장성

이 시나리오에서는 각 지역에 있는 두 프런트 엔드 웹 서버 클러스터에 가상 머신 확장 집합을 사용합니다. 확장 집합을 사용하면 프론트 엔드 애플리케이션 계층을 실행하는 VM 인스턴스의 수를 고객 요구 또는 정의된 일정에 따라 자동으로 조정할 수 있습니다. 자세한 내용은 [가상 머신 확장 집합을 사용한 자동 크기 조정 개요][docs-vmss-autoscale]를 참조하세요.

백 엔드는 가용성 집합의 MariaDB 클러스터입니다. 자세한 내용은 [MariaDB 클러스터 자습서][mariadb-tutorial]를 참조하세요.

다른 확장성 항목에 대해서는 Azure 아키텍처 센터의 [확장성 검사 목록][scalability]을 참조하세요.

### <a name="security"></a>보안

프론트 엔드 애플리케이션 계층으로 전송되는 모든 가상 네트워크 트래픽은 네트워크 보안 그룹을 통해 보호됩니다. 규칙은 프런트 엔드 애플리케이션 계층 VM 인스턴스만 백 엔드 데이터베이스 계층에 액세스할 수 있도록 트래픽 흐름을 제한합니다. 데이터베이스 계층에는 아웃바운드 인터넷 트래픽이 허용되지 않습니다. 공격 공간을 줄이기 위해 직접 원격 관리 포트가 열려 있지 않습니다. 자세한 내용은 [Azure 네트워크 보안 그룹][docs-nsg]을 참조하세요.

보안 시나리오 설계에 대한 일반적인 지침은 [Azure 보안 설명서][security]를 참조하세요.

### <a name="resiliency"></a>복원력

여러 지역, 데이터 복제 및 가상 머신 확장 집합이 결합된 이 시나리오에서는 Azure 부하 분산 장치를 사용합니다. 이러한 네트워킹 구성 요소는 연결된 VM 인스턴스에 트래픽을 분산하고, 트래픽이 정상 VM에만 분산되도록 보장하는 상태 프로브를 포함합니다. 이 모든 네트워킹 구성 요소는 CDN을 통해 프런트에 배치됩니다. 이렇게 하면 트래픽을 중단하고 최종 사용자 액세스에 영향을 미칠 수 있는 문제로부터 네트워킹 리소스와 애플리케이션을 탄력적으로 복원할 수 있습니다.

복원력 있는 시나리오 설계에 대한 일반적인 지침은 [Azure용 복원 애플리케이션 디자인][resiliency]을 참조하세요.

## <a name="pricing"></a>가격

이 시나리오를 실행하는 데 들어가는 비용을 알아보기 위해 모든 서비스가 비용 계산기에서 미리 구성됩니다. 특정 사용 사례에 대한 가격이 변경되는 정도를 확인하려면 필요한 트래픽에 맞게 적절한 변수를 변경합니다.

위에 제공된 아키텍처 다이어그램에 따라 미리 구성된 [비용 프로필][pricing]을 제공해 드렸습니다. 사용 사례에 맞는 가격 책정 계산기를 구성하려면 몇 가지 사항을 고려해야 합니다.

- 매달 예상하는 트래픽이 GB 단위로 얼마나 되나요? 트래픽 양은 가상 머신 확장 집합의 데이터를 표시하는 데 필요한 VM 수에 영향을 주기 때문에 비용에 가장 큰 영향을 미치게 됩니다. 또한 CDN을 통해 표시되는 데이터의 양과도 직접적인 상관 관계가 있습니다.
- 웹 사이트에 새 데이터를 얼마나 작성합니까? 웹 사이트에 기록되는 새 데이터의 양은 지역 간에 미러링되는 데이터의 양과 상관 관계가 있습니다.
- 동적 콘텐츠의 양이 얼마나 되나요? 정적 콘텐츠의 양이 얼마나 되나요? 동적 콘텐츠와 정적 콘텐츠의 양 차이는 데이터베이스 계층에서 검색해야 하는 데이터 양과 CDN에서 캐시해야 하는 데이터 양에 영향을 줍니다.

<!-- links -->
[architecture]: ./media/architecture-secure-scalable-wordpress.png
[mariadb-tutorial]: /azure/virtual-machines/linux/classic/mariadb-mysql-cluster
[docs-vmss]: /azure/virtual-machine-scale-sets/overview
[docs-vmss-autoscale]: /azure/virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview
[docs-nsg]: /azure/virtual-network/security-overview
[security]: /azure/security/
[availability]: ../../checklist/availability.md
[resiliency]: /azure/architecture/resiliency/
[scalability]: /azure/architecture/checklist/scalability
[pricing]: https://azure.com/e/a8c4809dab444c1ca4870c489fbb196b
