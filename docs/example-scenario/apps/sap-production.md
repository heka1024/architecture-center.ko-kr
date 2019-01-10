---
title: Oracle 데이터베이스를 사용하여 SAP 프로덕션 워크로드 실행
titleSuffix: Azure Example Scenarios
description: Oracle 데이터베이스를 사용하여 Azure에서 SAP 프로덕션 배포를 실행합니다.
author: DharmeshBhagat
ms.date: 09/12/2018
ms.custom: fasttrack
ms.openlocfilehash: 02a6eb43d3e11604857b8bd1f461c22a48f655c7
ms.sourcegitcommit: 1f4cdb08fe73b1956e164ad692f792f9f635b409
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54110930"
---
# <a name="running-sap-production-workloads-using-an-oracle-database-on-azure"></a>Azure에서 Oracle 데이터베이스를 사용하여 SAP 프로덕션 워크로드 실행

SAP 시스템은 중요 업무용 비즈니스 애플리케이션을 실행하는 데 사용됩니다. 가동 중단이 발생하면 키 프로세스가 중단되어 비용이 증가하거나 수익이 감소할 수 있습니다. 이러한 결과를 방지하려면 장애 발생 시에도 높은 가용성과 복원력을 제공하는 SAP 인프라가 필요합니다.

고가용성 SAP 환경을 구축하려면 시스템 아키텍처와 프로세스에서 단일 실패 지점을 제거해야 합니다. 단일 실패 지점은 사이트 장애, 시스템 구성 요소의 오류 또는 사람의 실수로 인해 발생할 수 있습니다.

이 예제 시나리오에서는 HA(고가용성) Oracle 데이터베이스와 함께 Azure 기반의 Windows 또는 Linux VM(가상 머신)에 SAP를 배포하는 방법을 보여줍니다. SAP 배포 시 요구 사항에 따라 다양한 크기의 VM을 사용할 수 있습니다.

## <a name="relevant-use-cases"></a>관련 사용 사례

관련된 다른 사용 사례는 다음과 같습니다.

- SAP에서 실행되는 중요 업무용 워크로드.
- 중요하지 않은 SAP 워크로드.
- 고가용성 환경을 시뮬레이션 하는 SAP 테스트 환경.

## <a name="architecture"></a>아키텍처

![Azure의 프로덕션 SAP 환경 아키텍처 개요][architecture]

이 예제에는 Oracle 데이터베이스, SAP 중앙 서비스 및 다른 가상 머신에서 실행되는 여러 SAP 애플리케이션 서버에 대한 고가용성 구성이 포함되어 있습니다. Azure 네트워크는 보안을 위해 [허브-스포크 토폴로지](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)를 사용합니다. 솔루션을 통한 데이터 흐름은 다음과 같습니다.

1. 사용자가 SAP 사용자 인터페이스, 웹 브라우저 또는 Microsoft Excel 같은 기타 클라이언트 도구를 통해 SAP 시스템에 액세스합니다. ExpressRoute 연결이 조직의 온-프레미스 네트워크에서 Azure에서 실행되는 리소스에 대한 액세스를 제공합니다.
2. ExpressRoute가 ExpressRoute VNet(가상 네트워크) 게이트웨이에서 Azure를 종료합니다. 허브 VNet에서 만든 ExpressRoute 게이트웨이를 통해 게이트웨이 서브넷으로 네트워크 트래픽이 라우팅됩니다.
3. 허브 VNet이 스포크 VNet에 피어링됩니다. 애플리케이션 계층 서브넷이 SAP를 실행 중인 가상 머신을 가용성 집합에 호스트합니다.
4. ID 관리 서버가 솔루션에 인증 서비스를 제공합니다.
5. 시스템 관리자가 점프 상자를 사용하여 Azure에 배포된 리소스를 안전하게 관리합니다.

### <a name="components"></a>구성 요소

- [Virtual Network](/azure/virtual-network/virtual-networks-overview)는 이 시나리오에서 Azure에 가상 허브-스포크 토폴로지를 만드는 데 사용됩니다.

- [Virtual Machines](/azure/virtual-machines/windows/overview)는 솔루션의 각 계층에 대한 계산 리소스를 제공합니다. 가상 머신의 각 클러스터는 [가용성 집합](/azure/virtual-machines/windows/regions-and-availability#availability-sets)으로 구성됩니다.

- [ExpressRoute](/azure/expressroute/expressroute-introduction)는 연결 공급자가 설정한 비공개 연결을 통해 온-프레미스 네트워크를 Microsoft 클라우드로 확장합니다.

- [NSG(네트워크 보안 그룹)](/azure/virtual-network/security-overview)는 네트워크 액세스를 가상 네트워크의 리소스로 제한합니다. NSG에는 원본 또는 대상 IP 주소, 포트 및 프로토콜에 따라 네트워크 트래픽을 허용하거나 거부하는 보안 규칙 목록이 포함되어 있습니다.

- [리소스 그룹](/azure/azure-resource-manager/resource-group-overview#resource-groups)은 Azure 리소스에 대한 논리 컨테이너 역할을 합니다.

### <a name="alternatives"></a>대안

SAP는 Azure 환경에서 다양한 운영 체제, 데이터베이스 관리 시스템 및 VM 유형을 조합할 수 있는 유연한 옵션을 제공합니다. 자세한 내용은 [SAP Note 1928533](https://launchpad.support.sap.com/#/notes/1928533) "Azure의 SAP 애플리케이션: 지원되는 제품 및 Azure VM 유형"을 참조하세요.

## <a name="considerations"></a>고려 사항

- Azure에 고가용성 SAP 환경을 구축하기 위한 모범 사례가 정의되어 있습니다. 자세한 내용은 [SAP NetWeaver에 대한 고가용성 아키텍처 및 시나리오](/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios)를 참조하세요. [Azure VM의 SAP 애플리케이션 고가용성](/azure/virtual-machines/workloads/sap/high-availability-guide)도 참조하세요.

- Oracle 데이터베이스도 Azure에 대한 모범 사례를 제공합니다. 자세한 내용은 [Azure에서 Oracle 데이터베이스 설계 및 구현](/azure/virtual-machines/workloads/oracle/oracle-design)을 참조하세요.

- Oracle Data Guard는 중요 업무용 Oracle 데이터베이스의 단일 실패 지점을 제거하는 데 사용됩니다. 자세한 내용은 [Azure에서 Linux 가상 머신에 Oracle Data Guard 구현](/azure/virtual-machines/workloads/oracle/configure-oracle-dataguard)을 참조하세요.

- Microsoft Azure는 Oracle 데이터베이스를 사용하여 SAP 제품을 배포하는 데 사용할 수 있는 인프라 서비스를 제공합니다. 자세한 내용은 [SAP 워크로드용 Azure에 Oracle DBMS 배포](/azure/virtual-machines/workloads/sap/dbms_guide_oracle)를 참조하세요.

## <a name="pricing"></a>가격

이 시나리오를 실행하는 데 들어가는 비용을 살펴볼 수 있도록, 모든 서비스가 비용 계산기에 미리 구성되어 있습니다. 특정 사용 사례에 대한 가격이 변경되는 정도를 확인하려면 필요한 트래픽에 맞게 적절한 변수를 변경합니다.

받을 것으로 예상되는 트래픽 양에 따라 다음과 같은 네 가지 샘플 비용 프로필이 제공되었습니다.

|크기|SAP|DB VM 유형|DB 저장소|(A)SCS VM|(A)SCS 저장소|앱 VM 유형|앱 저장소|Azure 요금 계산기|
|----|----|-------|-------|-----|---|---|--------|---------------|
|작음|30000|DS13_v2|4xP20, 1xP20|DS11_v2|1x P10|DS13_v2|1x P10|[소형](https://azure.com/e/45880ba0bfdf47d497851a7cf2650c7c)|
|중간|70000|DS14_v2|6xP20, 1xP20|DS11_v2|1x P10|4x DS13_v2|1x P10|[중형](https://azure.com/e/9a523f79591347ca9a48c3aaa1406f8a)|
큰|180000|E32s_v3|5xP30, 1xP20|DS11_v2|1x P10|6x DS14_v2|1x P10|[대형](https://azure.com/e/f70fccf571e948c4b37d4fecc07cbf42)|
초대형|250000|M64s|6xP30, 1xP30|DS11_v2|1x P10|10x DS14_v2|1x P10|[초대형](https://azure.com/e/58c636922cf94faf9650f583ff35e97b)|

> [!NOTE]
> 이 가격 책정은 VM 및 저장소 비용만 나타내는 가이드입니다. 네트워킹, 백업 저장소 및 데이터 수신/송신 요금은 제외됩니다.

- [소형](https://azure.com/e/45880ba0bfdf47d497851a7cf2650c7c): 소형 시스템은 vCPU 8개, 56GB RAM, 112GB 임시 스토리지가 있는 DS13_v2 VM 유형으로 구성되며, 512GB 프리미엄 스토리지 디스크 5개를 추가할 수 있습니다. vCPU 2개, 14GB RAM, 28GB 임시 저장소가 있는 DS11_v2 VM 유형을 사용하는 SAP Central Instance 서버. vCPU 8개, 56GB RAM, 400GB 임시 저장소가 있는 SAP 애플리케이션 서버용 단일 VM 유형 DS13_v2이며, 128GB 프리미엄 저장소 디스크 1개를 추가할 수 있습니다.

- [중형](https://azure.com/e/9a523f79591347ca9a48c3aaa1406f8a): 중형 시스템은 vCPU 16개, 112GB RAM, 800GB 임시 스토리지가 있는 DS14_v2 VM 유형으로 구성되며, 512GB 프리미엄 스토리지 디스크 7개를 추가할 수 있습니다. vCPU 2개, 14GB RAM, 28GB 임시 저장소가 있는 DS11_v2 VM 유형을 사용하는 SAP Central Instance 서버. vCPU 8개, 56GB RAM, 400GB 임시 저장소가 있는 SAP 애플리케이션 서버용 4VM DS13_v2 유형이며, 128GB 프리미엄 저장소 디스크 1개를 추가할 수 있습니다.

- [대형](https://azure.com/e/f70fccf571e948c4b37d4fecc07cbf42): 대형 시스템은 vCPU 32개, 256GB RAM, 800GB 임시 스토리지가 있는 E32s_v3 VM 유형으로 구성되며, 512GB 프리미엄 스토리지 디스크 3개와 128GB 프리미엄 스토리지 디스크 1개를 추가할 수 있습니다. vCPU 2개, 14GB RAM, 28GB 임시 저장소가 있는 DS11_v2 VM 유형을 사용하는 SAP Central Instance 서버. vCPU 16개, 112GB RAM, 224GB 임시 저장소가 있는 SAP 애플리케이션 서버용 6VM DS14_v2 유형이며, 128GB 프리미엄 저장소 디스크 6개를 추가할 수 있습니다.

- [초대형](https://azure.com/e/58c636922cf94faf9650f583ff35e97b): 초대형 시스템은 vCPU 64개, 1024GB RAM, 2000GB 임시 스토리지가 있는 M64s VM 유형으로 구성되며, 1024GB 프리미엄 스토리지 디스크 7개를 추가할 수 있습니다. vCPU 2개, 14GB RAM, 28GB 임시 저장소가 있는 DS11_v2 VM 유형을 사용하는 SAP Central Instance 서버. vCPU 16개, 112GB RAM, 224GB 임시 저장소가 있는 SAP 애플리케이션 서버용 10VM DS14_v2 유형이며, 128GB 프리미엄 저장소 디스크 10개를 추가할 수 있습니다.

## <a name="deployment"></a>배포

다음 링크를 사용하여 이 시나리오를 위한 기본 인프라를 배포하세요.

<!-- markdownlint-disable MD033 -->

<a
href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Fsolution-architectures%2Fmaster%2Fapps%2Fsap-3tier-distributed-ora%2Fazuredeploy.json" target="_blank">
    <img src="https://azuredeploy.net/deploybutton.png"/>
</a>

<!-- markdownlint-enable MD033 -->

> [!NOTE]
> SAP 및 Oracle은 이 배포 중에 설치되지 않습니다. 이러한 구성 요소를 별도로 배포해야 합니다.

## <a name="related-resources"></a>관련 리소스

Azure에서 SAP 프로덕션 워크로드를 실행하는 방법에 대한 다른 내용은 다음 참조 아키텍처를 검토하세요.

- [AnyDB용 SAP NetWeaver](/azure/architecture/reference-architectures/sap/sap-netweaver)
- [SAP S/4HANA](/azure/architecture/reference-architectures/sap/sap-s4hana)
- [SAP HANA 큰 인스턴스](/azure/architecture/reference-architectures/sap/hana-large-instances)

<!-- links -->
[architecture]: media/architecture-sap-production.png
