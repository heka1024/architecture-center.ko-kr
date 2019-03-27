---
title: Azure에서 Linux VM 실행
titleSuffix: Azure Reference Architectures
description: Azure에서 Linux 가상 머신을 실행하는 방법에 대한 모범 사례입니다.
author: telmosampaio
ms.date: 12/13/2018
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: seodec18
ms.openlocfilehash: ec71e35bec0fa9fad604456130f8596fcf127ebb
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58241179"
---
# <a name="run-a-linux-virtual-machine-on-azure"></a>Azure에서 Linux 가상 머신 실행

Azure에서 VM(가상 머신)을 프로비전하려면 VM 외에도 네트워킹 및 스토리지 리소스와 같은 추가 구성 요소가 필요합니다. 이 문서에서는 Azure에서 Linux VM을 실행하는 모범 사례를 보여줍니다.

![Azure의 Linux VM](./images/single-vm-diagram.png)

## <a name="resource-group"></a>리소스 그룹

[리소스 그룹][resource-manager-overview]은 관련 Azure 리소스를 보유하는 논리 컨테이너입니다. 일반적으로 리소스의 수명 및 관리하는 주체에 따라 리소스를 그룹화합니다.

동일한 수명 주기를 공유하는 긴밀하게 연결된 리소스를 동일한 [리소스 그룹][resource-manager-overview]에 배치합니다. 리소스 그룹을 사용하여 리소스를 그룹 단위로 배포 및 모니터링하고, 리소스 그룹별로 청구 비용을 추적할 수 있습니다. 리소스를 하나의 집합으로 삭제할 수도 있습니다. 이러한 기능은 테스트 배포에서 유용합니다. 의미 있는 리소스 이름을 할당하면 간단하게 특정 리소스를 찾고 해당 역할을 이해할 수 있습니다. 자세한 내용은 [Azure 리소스에 대한 권장되는 명명 규칙][naming-conventions]을 참조하세요.

## <a name="virtual-machine"></a>가상 머신

게시된 이미지 목록 또는 Azure Blob Storage에 업로드된 사용자 지정 관리되는 이미지나 VHD(가상 하드 디스크) 파일에서 VM을 프로비전할 수 있습니다.  Azure에서는 CentOS, Debian, Red Hat Enterprise, Ubuntu 및 FreeBSD를 포함하여 인기 있는 다양한 Linux 배포판을 실행할 수 있습니다. 자세한 내용은 [Azure 및 Linux][azure-linux]를 참조하세요.

Azure는 다양한 가상 머신 크기를 제공합니다. 자세한 내용은 [Azure의 가상 머신 크기][virtual-machine-sizes]를 참조하세요. 기존 워크로드를 Azure로 이동하는 경우 온-프레미스 서버와 가장 근접하게 일치하는 VM 크기부터 사용하기 시작합니다. 그런 다음, CPU, 메모리, 디스크 IOPS(초당 입력/출력 작업 수)에 따라 실제 워크로드의 성능을 측정하고 필요에 따라 크기를 조정합니다. 

일반적으로 내부 사용자 또는 고객에게 가장 가까운 Azure 지역을 선택합니다. 일부 지역에서는 일부 VM 크기를 사용하지 못할 수 있습니다. 자세한 내용은 [지역별 서비스][services-by-region]를 참조하세요. 지정된 지역에서 사용할 수 있는 VM 크기 목록을 보려면 다음 Azure CLI(명령줄 인터페이스) 명령을 실행합니다.

```azurecli
az vm list-sizes --location <location>
```

게시된 VM 이미지를 선택하는 방법에 대한 자세한 내용은 [Linux VM 이미지 찾기][select-vm-image]를 참조하세요.

## <a name="disks"></a>디스크

최상의 디스크 I/O 성능을 위해서는 SSD(반도체 드라이브)에 데이터를 저장하는 [Premium Storage][premium-storage]가 권장됩니다. 비용은 프로비전된 디스크 용량을 기준으로 산정됩니다. IOPS 및 처리량(즉, 데이터 전송 속도)도 디스크 크기에 따라 달라지므로 디스크를 프로비전할 때 세 가지 요소(용량, IOPS, 처리량)를 모두 고려합니다.

또한 [관리되는 디스크][managed-disks]를 사용하는 것이 좋습니다. Managed Disks는 스토리지를 처리하여 디스크 관리를 단순화합니다. 관리 디스크에는 저장소 계정이 필요하지 않습니다. 디스크의 크기와 유형을 지정하기만 하면 고가용성 리소스로 배포됩니다.

OS 디스크는 [Azure Storage][azure-storage]에 저장된 VHD이므로 호스트 컴퓨터가 중단되어도 계속 유지됩니다.  Linux VM의 경우 OS 디스크는 `/dev/sda1`입니다. 또한 애플리케이션 데이터에 사용되는 영구 VHD인 [데이터 디스크][data-disk]를 하나 이상 만드는 것이 좋습니다.

VHD를 만들 때 형식은 지정되지 않습니다. 디스크를 포맷하려면 VM에 로그인합니다. Linux 셸에서 데이터 디스크는 `/dev/sdc`, `/dev/sdd` 등으로 표시됩니다. `lsblk`를 실행하여 디스크를 포함하는 블록 디바이스를 나열할 수 있습니다. 데이터 디스크를 사용하려면 파티션 및 파일 시스템을 만들고 디스크를 탑재합니다. 예: 

```bash
# Create a partition.
sudo fdisk /dev/sdc     # Enter 'n' to partition, 'w' to write the change.

# Create a file system.
sudo mkfs -t ext3 /dev/sdc1

# Mount the drive.
sudo mkdir /data1
sudo mount /dev/sdc1 /data1
```

데이터 디스크를 추가하면 디스크에 LUN(논리 단위 번호) ID가 할당됩니다. 예를 들어, 디스크를 교체하고 동일한 LUN ID를 유지하거나 특정 LUN ID를 검색하는 애플리케이션이 있는 경우 필요에 따라 LUN ID &mdash;을(를) 지정할 수 있습니다. 그렇지만 LUN ID는 디스크마다 고유해야 합니다.

Premium Storage 계정에서 VM의 디스크가 SSD이기 때문에 I/O 스케줄러를 변경하여 SSD의 성능을 최적화하려고 할 수 있습니다. SSD에 NOOP 스케줄러를 사용하는 것이 일반적으로 권장되지만 [iostat] 와 같은 도구를 사용하여 워크로드에 대한 디스크 I/O 성능을 모니터링할 수 있습니다.

VM은 임시 디스크를 사용하여 만들어집니다. 이 디스크는 호스트 컴퓨터의 실제 드라이브에 저장됩니다. Azure Storage에는 저장되지 *않으며* 다시 부팅되는 동안에 그리고 다른 VM의 수명 주기 이벤트 동안에 삭제될 수 있습니다. 페이지 또는 스왑 파일과 같은 임시 데이터에 대해서만 이 디스크를 사용합니다. Linux VM의 경우 임시 디스크는 `/dev/sdb1`이며 `/mnt/resource` 또는 `/mnt`에 탑재됩니다.

## <a name="network"></a>네트워크

네트워킹 구성 요소에는 다음 리소스가 포함됩니다.

- **가상 네트워크**. 모든 VM은 가상 네트워크에 배포되어 여러 서브넷으로 분할될 수 있습니다.

- **NIC(네트워크 인터페이스)**. NIC를 통해 VM을 가상 네트워크와 통신하도록 할 수 있습니다. VM에 여러 개의 NIC가 필요한 경우 각 [VM 크기][vm-size-tables]에 대한 최대 NIC 수가 정의되어 있습니다.

- **공용 IP 주소**. 공용 IP 주소는 RDP(원격 데스크톱) 등을 통해 VM과 통신하는 데 필요합니다. 이 공용 IP 주소는 동적 또는 정적일 수 있습니다. 기본값은 동적입니다.

- 변경되지 않는 고정 IP 주소가 필요한 경우(예: DNS에 'A' 레코드를 만들어야 하는 경우 또는 안전한 목록에 IP 주소를 추가해야 하는 경우) [고정 IP 주소][static-ip]를 예약합니다.
- IP 주소의 FQDN(정규화된 도메인 이름)을 만들 수도 있습니다. 그런 후 FQDN을 가리키는 DNS에 [CNAME 레코드][cname-record]를 등록할 수 있습니다. 자세한 내용은 [Azure Portal에서 정규화된 도메인 이름 만들기][fqdn]를 참조하세요.

- **NSG(네트워크 보안 그룹)**. [네트워크 보안 그룹][nsg]은 VM에 대한 네트워크 트래픽을 허용하거나 거부하는 데 사용됩니다. NSG는 서브넷 또는 개별 VM 인스턴스로 연결할 수 있습니다.

모든 NSG에는 모든 인바운드 인터넷 트래픽을 차단하는 규칙을 포함하여 [기본 규칙][nsg-default-rules] 집합이 있습니다. 기본 규칙은 삭제할 수 없으나 다른 규칙으로 재정의할 수 있습니다. 인터넷 트래픽을 사용하도록 설정하려면 특정 포트(예: HTTP용 포트 80)로의 인바운드 트래픽을 허용하는 규칙을 만듭니다. SSH를 사용하도록 설정하려면 TCP 포트 22에 인바운드 트래픽을 허용하는 NSG 규칙을 추가합니다.

## <a name="operations"></a>작업

**SSH**. Linux VM을 만들기 전에 2048비트 RSA 공개-개인 키 쌍을 생성합니다. VM을 만들 때 공개 키 파일을 사용합니다. 자세한 내용은 [Azure에서 Linux 및 Mac과 함께 SSH를 사용하는 방법][ssh-linux]을 참조하세요.

**진단**. 기본 상태 메트릭, 진단 인프라 로그 및 [부팅 진단][boot-diagnostics]을 모니터링 및 진단을 사용하도록 설정할 수 있습니다. 부팅 진단은 VM이 부팅할 수 없는 상태로 전환되는 경우 부팅 오류를 진단하는 데 도움이 될 수 있습니다. 로그를 저장할 Azure Storage 계정을 만듭니다. 표준 LRS(로컬 중복 저장소) 계정은 진단 로그에 충분합니다. 자세한 내용은 [모니터링 및 진단 사용][enable-monitoring]을 참조하세요.

**가용성**. 사용자의 VM은 [계획된 유지 관리][planned-maintenance] 또는 [계획되지 않은 가동 중지 시간][manage-vm-availability]의 영향을 받을 수 있습니다. [VM 다시 부팅 로그][reboot-logs]를 사용하여 VM 재부팅이 계획된 유지 관리로 발생했는지 여부를 결정할 수 있습니다. 더 높은 가용성을 위해 [가용성 집합](/azure/virtual-machines/linux/manage-availability#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy)에 여러 VM을 배포합니다. 이 구성은 높은 수준의 [SLA(서비스 수준 계약)][vm-sla]를 제공합니다.

**백업** 실수로 인한 데이터 손실을 방지하려면 [Azure Backup](/azure/backup/) 서비스를 사용하여 VM을 지역 중복 스토리지에 백업하세요. Azure Backup은 애플리케이션 일치 백업을 제공합니다.

**VM 중지**. Azure에서는 "중지됨"과 "할당 취소됨" 상태를 구분합니다. VM 상태가 중지되면 요금이 청구되지만 VM 할당이 취소되면 청구되지 않습니다. Azure Portal에서 **중지** 버튼은 VM 할당을 취소합니다. 로그인한 상태에서 OS를 통해 종료하면 VM은 중지되지만 할당 취소되지 **않으므로** 비용이 계속 청구됩니다.

**VM 삭제**. VM을 삭제하는 경우 VHD는 삭제되지 않습니다. 즉, 데이터 손실 없이 안전하게 VM을 삭제할 수 있습니다. 그러나 저장소에 대한 비용은 계속 청구됩니다. VHD를 삭제하려면 [Blob Storage][blob-storage]에서 파일을 삭제합니다. 실수로 삭제하지 않도록 하려면 [리소스 잠금][resource-lock]을 사용하여 전체 리소스 그룹을 잠그거나 VM과 같은 개별 리소스를 잠급니다.

## <a name="security-considerations"></a>보안 고려 사항

[Azure Security Center][security-center]를 사용하여 Azure 리소스의 보안 상태를 중앙에서 살펴볼 수 있습니다. Security Center는 잠재적인 보안 문제를 모니터링하고 배포의 보안 상태에 대한 종합적인 그림을 제공합니다. 보안 센터는 각 Azure 구독을 기준으로 구성됩니다. [Security Center 표준에 Azure 구독 온보딩][security-center-get-started]에서 설명된 대로 보안 데이터 컬렉션을 활성화합니다. 데이터 수집이 사용되도록 설정되면 보안 센터는 해당 구독에서 만든 모든 VM을 자동으로 검색합니다.

**패치 관리**. 이 기능이 설정된 경우 Security Center는 보안 및 중요 업데이트 누락 여부를 확인합니다. VM의 [그룹 정책 설정][group-policy]을 사용하여 자동 시스템 업데이트를 사용하도록 설정합니다.

**맬웨어 방지**. 이 기능이 설정되면 보안 센터는 맬웨어 방지 소프트웨어 설치 여부를 확인합니다. 또한 보안 센터를 사용하여 Azure 포털 내에서 맬웨어 방지 소프트웨어를 설치할 수도 있습니다.

**액세스 제어**. [RBAC(역할 기반 액세스 제어)][rbac]를 사용하여 Azure 리소스에 대한 액세스를 제어합니다. RBAC를 통해 DevOps 팀의 구성원에게 권한 역할을 할당할 수 있습니다. 예를 들어 읽기 권한자 역할은 Azure 리소스를 볼 수 있지만 만들거나 관리하거나 삭제할 수는 없습니다. 일부 권한은 Azure 리소스 형식에 적용됩니다. 예를 들어 Virtual Machine Contributor 역할은 VM을 다시 시작하거나 할당을 취소하고, 관리자 암호를 재설정하고, 새 VM을 만드는 등의 작업을 수행할 수 있습니다. 이 아키텍처에 유용할 수 있는 기타 [기본 제공 RBAC 역할][rbac-roles]에는 [DevTest Labs 사용자][rbac-devtest] 및 [네트워크 참가자][rbac-network]가 포함됩니다.

> [!NOTE]
> RBAC는 VM에 로그온한 사용자가 수행할 수 있는 작업을 제한하지 않습니다. 이러한 사용 권한은 게스트 OS의 계정 유형에 따라 결정됩니다.

**감사 로그**. [감사 로그][audit-logs]를 사용하여 프로비전 동작 및 기타 VM 이벤트를 확인합니다.

**데이터 암호화**. OS 및 데이터 디스크를 암호화해야 하는 경우 [Azure Disk Encryption][disk-encryption]을 사용하세요.

## <a name="next-steps"></a>다음 단계

- Linux VM을 프로비전하려면 [Azure CLI로 Linux VM 만들기 및 관리](/azure/virtual-machines/linux/tutorial-manage-vm)를 참조하세요.
- Linux VM의 완전한 N 계층 아키텍처는 [Apache Cassandra를 사용하는 Azure의 Linux N 계층 애플리케이션](./n-tier-cassandra.md)을 참조하세요.

<!-- links -->
[audit-logs]: https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/
[azure-linux]: /azure/virtual-machines/virtual-machines-linux-azure-overview
[azure-storage]: /azure/storage/storage-introduction
[blob-storage]: /azure/storage/storage-introduction
[boot-diagnostics]: https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/
[cname-record]: https://en.wikipedia.org/wiki/CNAME_record
[data-disk]: /azure/virtual-machines/virtual-machines-linux-about-disks-vhds
[disk-encryption]: /azure/security/azure-security-disk-encryption
[enable-monitoring]: /azure/monitoring-and-diagnostics/insights-how-to-use-diagnostics
[fqdn]: /azure/virtual-machines/virtual-machines-linux-portal-create-fqdn
[group-policy]: /windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates
[iostat]: https://en.wikipedia.org/wiki/Iostat
[manage-vm-availability]: /azure/virtual-machines/virtual-machines-linux-manage-availability
[managed-disks]: /azure/storage/storage-managed-disks-overview
[naming-conventions]: ../../best-practices/naming-conventions.md
[nsg]: /azure/virtual-network/virtual-networks-nsg
[nsg-default-rules]: /azure/virtual-network/virtual-networks-nsg#default-rules
[planned-maintenance]: /azure/virtual-machines/virtual-machines-linux-planned-maintenance
[premium-storage]: /azure/virtual-machines/linux/premium-storage
[rbac]: /azure/active-directory/role-based-access-control-what-is
[rbac-roles]: /azure/active-directory/role-based-access-built-in-roles
[rbac-devtest]: /azure/active-directory/role-based-access-built-in-roles#devtest-labs-user
[rbac-network]: /azure/active-directory/role-based-access-built-in-roles#network-contributor
[reboot-logs]: https://azure.microsoft.com/blog/viewing-vm-reboot-logs/
[resource-lock]: /azure/resource-group-lock-resources
[resource-manager-overview]: /azure/azure-resource-manager/resource-group-overview
[security-center]: /azure/security-center/security-center-intro
[security-center-get-started]: /azure/security-center/security-center-get-started
[select-vm-image]: /azure/virtual-machines/virtual-machines-linux-cli-ps-findimage
[services-by-region]: https://azure.microsoft.com/regions/#services
[ssh-linux]: /azure/virtual-machines/virtual-machines-linux-mac-create-ssh-keys
[static-ip]: /azure/virtual-network/virtual-networks-reserved-public-ip
[virtual-machine-sizes]: /azure/virtual-machines/virtual-machines-linux-sizes
[visio-download]: https://archcenter.blob.core.windows.net/cdn/vm-reference-architectures.vsdx
[vm-size-tables]: /azure/virtual-machines/virtual-machines-linux-sizes
[vm-sla]: https://azure.microsoft.com/support/legal/sla/virtual-machines
