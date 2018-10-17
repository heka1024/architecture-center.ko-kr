---
title: Citrix를 사용한 Linux 가상 데스크톱
description: Azure에서 Citrix를 사용하여 Linux 데스크톱에 대해 VDI 환경을 빌드합니다.
author: miguelangelopereira
ms.date: 09/12/2018
ms.openlocfilehash: 374d59f7a528bd89870baa601a49a30ea00a08f1
ms.sourcegitcommit: b2a4eb132857afa70201e28d662f18458865a48e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48819145"
---
# <a name="linux-virtual-desktops-with-citrix"></a>Citrix를 사용한 Linux 가상 데스크톱

이 예제 시나리오는 Linux 데스크톱용 VDI(가상 데스크톱 인프라)가 필요한 모든 산업에 적용 됩니다. VDI는 데이터 센터의 서버에 상주하는 가상 머신 내부에서 사용자 데스크톱을 실행하는 프로세스를 의미합니다. 이 시나리오의 고객은 VDI 요구 사항을 해결하기 위해 Citrix 기반 솔루션을 사용하기로 선택했습니다.

조직에서는 직원들이 여러 장치와 운영 체제를 사용하는 이기종 환경을 운영하는 경우가 자주 있습니다. 안전한 환경을 유지하면서도 일관적인 응용 프로그램 액세스를 제공하기가 어려울 수 있습니다. Linux 데스크톱용 VDI 솔루션을 사용하면 조직에서는 최종 사용자가 사용하는 장치 또는 OS에 관계없이 액세스를 제공할 수 있습니다.

이 샘플 솔루션은 다음과 같은 이점이 있습니다.
* 공유 Linux 가상 데스크톱을 사용하여 동일한 인프라에 더 많은 사용자 액세스를 부여하여 투자 수익률을 높일 수 있습니다. 리소스를 중앙 집중식 VDI 환경에 통합하면 최종 사용자 장치의 성능이 높지 않아도 됩니다.
* 최종 사용자 장치에 관계없이 일관적인 성능이 제공됩니다.
* 사용자가 아무 장치에서(비 Linux 장치 포함) Linux 응용 프로그램에 액세스할 수 있습니다.
* 분산된 직원에 대한 중요한 데이터를 Azure 데이터 센터에 안전하게 보관할 수 있습니다.

## <a name="relevant-use-cases"></a>관련 사용 사례

이 시나리오에 적합한 사용 사례는 다음과 같습니다.

* Linux 또는 비 Linux 장치에서 업무에 중요한 특수 Linux VDI 데스크톱에 액세스

## <a name="architecture"></a>아키텍처

[![](./media/azure-citrix-sample-diagram.png "아키텍처 다이어그램")](./media/azure-citrix-sample-diagram.png#lightbox)

이 예제 시나리오는 회사 네트워크에서 Linux 가상 데스크톱 액세스를 허용하는 방법을 보여줍니다.

* 클라우드에 빠르고 안정적으로 연결할 수 있도록 온-프레미스 환경과 Azure 간에 ExpressRoute를 설정합니다.
* VDI용으로 Citrix XenDeskop 솔루션을 배포합니다.
* Ubuntu(또는 지원되는 다른 배포판)에서 CitrixVDA를 실행합니다.
* Azure 네트워크 보안 그룹은 올바른 네트워크 ACL을 적용합니다.
* Citrix ADC(NetScaler)는 모든 Citrix 서비스를 게시하고 부하를 분산합니다.
* Active Directory Domain Services는 Citrix 서버를 도메인 가입하는 데 사용됩니다. VDA 서버는 도메인 가입되지 않습니다.
* Azure 하이브리드 파일 동기화는 솔루션에서 공유 저장소를 지원합니다. 예를 들어 원격/홈 솔루션에 사용할 수 있습니다.

이 시나리오에서는 다음 SKU를 사용합니다.

- Citrix ADC(NetScaler): [NetScaler 12.0 VPX Standard Edition 200 MBPS PAYG 이미지](https://azuremarketplace.microsoft.com/pt-br/marketplace/apps/citrix.netscalervpx-120?tab=PlansAndPrice)가 포함된 D4sv3 2개
- Citrix 라이선스 서버: D2s v3 1개
- Citrix VDA: D8s v3 4개
- Citrix Storefront: D2s v3 2개
- Citrix 전송 컨트롤러: D2s v3 2개
- 도메인 컨트롤러: D2sv3 2개
- Azure 파일 서버: D2sv3 2개

> [!NOTE]
> NetScaler 이외의 모든 라이선스는 BYOL

### <a name="components"></a>구성 요소

- [Azure Virtual Network](/azure/virtual-network/virtual-networks-overview)는 VM과 같은 리소스에서 상호 간 통신, 인터넷 통신 및 온-프레미스 네트워크 통신을 안전하게 수행할 수 있게 합니다. 가상 네트워크는 격리 및 세분화를 제공하고, 트래픽을 필터링 및 라우팅하며, 위치 간 연결을 허용합니다. 이 시나리오에서는 한 가상 네트워크를 모든 리소스에 사용합니다.
- [Azure 네트워크 보안 그룹](/azure/virtual-network/security-overview)에는 원본 또는 대상 IP 주소, 포트 및 프로토콜에 따라 인바운드 또는 아웃바운드 네트워크 트래픽을 허용하거나 거부하는 보안 규칙 목록이 포함되어 있습니다. 이 시나리오의 가상 네트워크는 응용 프로그램 구성 요소 간의 트래픽 흐름을 제한하는 네트워크 보안 그룹 규칙으로 보호됩니다.
- [Azure 부하 분산 장치](/azure/application-gateway/overview)는 규칙 및 상태 프로브에 따라 인바운드 트래픽을 분산합니다. 부하 분산 장치는 짧은 대기 시간과 높은 처리량을 제공하고, 모든 TCP 및 UDP 응용 프로그램에 대해 최대 수백만 개의 흐름으로 확장합니다. 이 시나리오에서는 내부 부하 분산 장치를 사용하여 Citrix NetScaler의 트래픽을 분산합니다.
- 모든 공유 저장소에 [Azure 하이브리드 파일 동기화](https://github.com/MicrosoftDocs/azure-docs/edit/master/articles/storage/files/storage-sync-files-planning.md)가 사용됩니다. 저장소는 하이브리드 파일 동기화를 사용하여 두 파일 서버로 복제됩니다.
- [Azure SQL Database](/azure/sql-database/sql-database-technical-overview)는 최신의 안정적인 Microsoft SQL Server 데이터베이스 엔진 버전을 기반으로 하는 관계형 DBaaS(Database-as-a-Service)입니다. Citrix 데이터베이스 호스팅에 사용됩니다.
- [ExpressRoute](/azure/expressroute/expressroute-introduction)를 사용하면 연결 공급자가 지원하는 개인 연결을 통해 온-프레미스 네트워크를 Microsoft 클라우드로 확장할 수 있습니다. 
- [Active Directory Domain Services]는 디렉터리 서비스 및 사용자 인증에 사용됩니다.
- [Azure 가용성 집합](/azure/virtual-machines/windows/tutorial-availability-sets)을 사용하면 Azure에 배포한 VM이 클러스터의 격리된 여러 하드웨어 노드에 분산됩니다. 이렇게 하면 Azure 내의 하드웨어 또는 소프트웨어 오류가 발생할 때 VM의 하위 집합에만 영향을 주며 전체 솔루션을 사용 가능한 운영 상태로 유지할 수 있습니다. 
- [Citrix ADC(NetScaler)](https://www.citrix.com/products/citrix-adc)는 응용 프로그램별로 트래픽을 분석하여 웹 응용 프로그램의 레이어 4-레이어 7(L4–L7) 네트워크 트래픽을 지능적으로 분산, 최적화, 보호하는 응용 프로그램 전송 컨트롤러입니다. 
- [Citrix Storefront](https://www.citrix.com/products/citrix-virtual-apps-and-desktops/citrix-storefront.html)는 보안을 개선하고 배포를 간소화하는 엔터프라이즈 앱 저장소로, 모든 플랫폼의 Citrix Receiver에서 거의 네이티브에 가까운 강력한 최신 사용자 경험을 제공합니다. StoreFront를 사용하면 다중 사이트 및 다중 버전 Citrix 가상 앱 및 데스크톱 환경을 간편하게 관리할 수 있습니다. 
- [Citrix 라이선스 서버](https://www.citrix.com/buy/licensing/overview.html)는 Citrix 제품의 라이선스를 관리합니다.
- [Citrix XenDesktops VDA](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service)는 응용 프로그램 및 데스크톱 연결을 지원합니다. VDA는 사용자를 위한 응용 프로그램 또는 가상 데스크톱을 실행하는 머신에 설치됩니다. 머신을 전송 컨트롤러에 등록하고 사용자 장치에 대한 HDX(High Definition eXperience) 연결을 관리할 수 있습니다.
- [Citrix 전송 컨트롤러](https://docs.citrix.com/en-us/xenapp-and-xendesktop/7-15-ltsr/manage-deployment/delivery-controllers)는 사용자 액세스를 관리하고 연결을 중개 및 최적화하는 역할을 담당하는 서버 쪽 구성 요소입니다. 이 컨트롤러는 데스크톱 및 서버 이미지를 만드는 Machine Creation Services도 제공합니다.

### <a name="alternatives"></a>대안

- VMware, Workspot 등과 같이 Azure에서 지원되는 VDI 솔루션을 사용하는 여러 파트너가 있습니다. 이 특정 샘플 아키텍처는 Citrix를 사용하는 배포된 프로젝트를 기반으로 합니다.
- Citrix는 이 아키텍처의 일부를 추상화하는 클라우드 서비스를 제공합니다. 이 솔루션을 대체할 수 있습니다. 자세한 내용은 [Citrix 클라우드](https://www.citrix.com/products/citrix-cloud)를 참조하세요.

## <a name="considerations"></a>고려 사항

- [Citrix Linux 요구 사항](https://docs.citrix.com/en-us/linux-virtual-delivery-agent/current-release/system-requirements)을 확인하세요.
- 대기 시간이 전체 솔루션에 영향을 미칠 수 있습니다. 프로덕션 환경의 경우 대기 시간에 맞게 적절하게 테스트하세요.
- 시나리오에 따라, VDA용 GPU가 포함된 VM이 솔루션에 필요할 수 있습니다. 이 솔루션에서는 GPU가 필수가 아닌 것으로 가정합니다.

### <a name="availability-scalability-and-security"></a>가용성, 확장성 및 보안

- 이 샘플 솔루션은 라이선싱 서버 이외의 모든 역할에 고가용성을 제공할 목적으로 설계되었습니다. 라이선스 서버가 오프라인으로 전환되더라도 환경이 30일의 유예 기간 동안 정상적으로 작동하므로 해당 서버에 추가 중복이 필요 없습니다.
- 비슷한 역할을 제공하는 모든 서버를 [가용성 집합](/azure/virtual-machines/windows/manage-availability#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy)에 배포해야 합니다.
- 이 샘플 솔루션에는 재해 복구 기능이 없습니다. [Azure Site Recovery](/azure/site-recovery/site-recovery-overview)를 이 디자인에 추가하면 유용하게 사용할 수 있습니다.
- 프로덕션 배포의 경우 [백업](/azure/backup/backup-introduction-to-azure-backup), [모니터링](/azure/monitoring-and-diagnostics/monitoring-overview), [업데이트 관리](/azure/automation/automation-update-management) 같은 관리 솔루션을 구현해야 합니다.
- 이 샘플 솔루션은 사용량 혼합 시 동시 사용자 약 250명(VDA 서버당 약 50-60명)을 감당할 수 있습니다. 하지만 사용하는 응용 프로그램 종류에 따라 숫자가 크게 달라질 수 있습니다. 프로덕션 사용 시 엄격한 부하 테스트를 수행해야 합니다.

## <a name="deploy-this-scenario"></a>시나리오 배포

배포 정보는 공식 [Citrix 설명서](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/install-configure.html)를 참조하세요.

## <a name="pricing"></a>가격

- Citrix XenDestop 라이선스는 Azure 서비스 요금에 포함되지 않습니다.
- Citrix NetScaler 라이선스는 종량제 모델에 포함됩니다.
- 예약 인스턴스를 사용하면 솔루션의 계산 비용이 크게 감소합니다.
- ExpressRoute 비용은 포함되지 않습니다.

## <a name="next-steps"></a>다음 단계

- [여기](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/install-configure)서 계획 및 배포에 대한 Citrix 설명서를 살펴보세요.
- Azure에 Citrix ADC(NetScaler)를 배포하려면 [여기](https://github.com/citrix/netscaler-azure-templates)서 Citrix에서 제공하는 Resource Manager 템플릿을 검토하세요.
