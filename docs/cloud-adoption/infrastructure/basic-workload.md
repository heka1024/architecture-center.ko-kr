---
title: '엔터프라이즈 클라우드 채택: 기본 워크로드 배포'
description: Azure에 기본 워크로드를 배포하는 방법 설명
author: petertaylor9999
ms.date: 09/10/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: Windows, Linux
ms.openlocfilehash: 031a8f2e1dc0b137fc830733d025997a2657ef3c
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54481408"
---
# <a name="enterprise-cloud-adoption-deploy-a-basic-workload"></a>엔터프라이즈 클라우드 채택: 기본 워크로드 배포

**워크로드**라는 용어는 일반적으로 애플리케이션이나 서비스 같은 임의 단위의 기능을 정의하는 것으로 이해됩니다. 워크로드를 서버에 배포되는 코드 아티팩트의 관점뿐만 아니라 필요한 기타 서비스의 관점으로도 고려해야 합니다. 이는 온-프레미스 애플리케이션 또는 서비스에 대한 유용한 정의이지만 클라우드에서는 이를 확장해야 합니다.

클라우드에서 워크로드는 모든 아티팩트뿐만 아니라 클라우드 리소스도 포함합니다. infrastructure-as-code로 알려진 개념 때문에 클라우드 리소스를 정의의 일부로 포함합니다. [Azure 작동 방법](../getting-started/what-is-azure.md)에서 알아본 것처럼 오케스트레이터 서비스에서 Azure의 리소스를 배포합니다. 오케스트레이터 서비스는 웹 API를 통해 이 기능을 공개하고 Powershell, Azure CLI(명령줄 인터페이스) 및 Azure Portal 같은 여러 도구를 사용하여 이 웹 API를 호출할 수 있습니다. 즉, 애플리케이션과 연결된 코드 아티팩트와 함께 저장될 수 있는 컴퓨터가 읽을 수 있는 파일에서 리소스를 지정할 수 있습니다.

이렇게 하면 코드 아티팩트 및 필요한 클라우드 리소스의 관점으로 워크로드를 정의할 수 있고, 따라서 워크로드를 격리할 수 있습니다. 워크로드는 리소스가 조직되는 방법 및 네트워크 토폴로지 또는 다른 특성에 의해 격리될 수 있습니다. 워크로드 격리의 목적은 팀이 해당 리소스의 모든 측면을 독립적으로 관리할 수 있도록 워크로드의 특정 리소스를 팀과 연결하는 데 있습니다. 이렇게 하면 여러 팀이 Azure에서 리소스 관리 서비스를 공유하는 반면 각자의 리소스가 의도하지 않게 삭제되거나 수정되는 것을 방지할 수 있습니다.

이 격리를 통해 DevOps라는 또 다른 개념을 사용할 수도 있습니다. DevOps는 소프트웨어 개발 및 IT 작업 모두를 포함하는 소프트웨어 개발 구성 요소를 포함하지만 가능한 한 많은 자동화 사용을 추가합니다. DevOps의 원리 중 하나는 지속적인 통합 및 지속적인 업데이트(CI/CD)로 알려져 있습니다. 지속적인 통합은 개발자가 코드 변경에 커밋할 때마다 실행되는 자동화된 빌드 프로세스를 의미하고, 지속적인 업데이트는 테스트를 위한 개발 환경 또는 최종 배포를 위한 프로덕션 환경과 같이 다양한 환경에 이 코드를 배포하는 자동화된 프로세스를 의미합니다.

## <a name="basic-workload"></a>기본 워크로드

**기본 워크로드**는 일반적으로 단일 웹 애플리케이션 또는 VM(가상 머신)을 사용하는 VNet(가상 네트워크)로 정의됩니다. 

> [!NOTE]
> 이 가이드는 애플리케이션 개발을 다루지 않습니다. Azure에서 애플리케이션을 개발하는 방법에 대한 자세한 내용은 [Azure 애플리케이션 아키텍처 가이드](/azure/architecture/guide/)를 참조하세요.

워크로드가 웹 애플리케이션인지 아니면 VM인지에 관계 없이 이러한 각 배포에는 **리소스 그룹**이 필요합니다. 리소스 그룹을 만드는 사용 권한 가진 사용자는 다음 단계를 수행하기 전에 이 작업을 수행해야 합니다.

## <a name="basic-web-application-paas"></a>기본 웹 애플리케이션(PaaS)

기본 웹 애플리케이션의 경우 [웹앱 설명서](/azure/app-service?toc=/azure/architecture/cloud-adoption-guide/toc.json)에서 5분 빠른 시작 중 하나를 선택하여 단계를 수행합니다. 

> [!NOTE]
> 기본적으로 일부 빠른 시작은 리소스 그룹을 배포합니다. 이 경우에 리소스 그룹을 명시적으로 만들 필요는 없습니다. 그렇지 않으면 위에서 만든 리소스 그룹에 웹 애플리케이션을 배포합니다.

간단한 워크로드를 배포하면 [기본 웹 애플리케이션](/azure/architecture/reference-architectures/app-service-web-app/basic-web-app?toc=/azure/architecture/cloud-adoption-guide/toc.json)을 Azure에 배포하는 입증된 방법에 대해 자세히 알아볼 수 있습니다.

## <a name="single-windows-or-linux-vm-iaas"></a>단일 Windows 또는 Linux VM(IaaS)

가상 머신에서 실행되는 간단한 워크로드의 경우 첫 번째 단계는 가상 네트워크를 배포하는 것입니다. 가상 머신, 부하 분산 장치 및 게이트웨이와 같은 Azure의 모든 IaaS 리소스에는 가상 네트워크가 필요합니다. [Azure 가상 네트워크](/azure/virtual-network/virtual-networks-overview?toc=/azure/architecture/cloud-adoption-guide/toc.json)에 대해 알아본 다음, [포털을 사용하여 Azure에 Virtual Network를 배포](/azure/virtual-network/quick-create-portal?toc=/azure/architecture/cloud-adoption-guide/toc.json)하는 단계를 수행합니다. Azure Portal에서 가상 네트워크에 대한 설정을 지정할 때 위에서 만든 리소스 그룹의 이름을 지정합니다.

다음 단계에서는 단일 Windows 또는 Linux VM을 배포할지를 결정합니다. Windows VM의 경우 [포털에서 Azure에 Windows VM을 배포](/azure/virtual-machines/windows/quick-create-portal?toc=/azure/architecture/cloud-adoption-guide/toc.json)하는 단계를 수행합니다. 다시 Azure Portal에서 가상 머신에 대한 설정을 지정할 때 위에서 만든 리소스 그룹의 이름을 지정합니다.

단계를 수행하고 VM을 배포하면 [Azure에서 Windows VM을 실행하는 입증된 방법](/azure/architecture/reference-architectures/virtual-machines-windows/single-vm?toc=/azure/architecture/cloud-adoption-guide/toc.json)에 대해 알아볼 수 있습니다. Linux VM의 경우 [포털에서 Azure에 Linux VM을 배포](/azure/virtual-machines/linux/quick-create-portal?toc=/azure/architecture/cloud-adoption-guide/toc.json)하는 단계를 수행합니다. [Azure에서 Linux VM을 실행하기 위한 입증된 방법](/azure/architecture/reference-architectures/virtual-machines-linux/single-vm?toc=/azure/architecture/cloud-adoption-guide/toc.json)에 대해 자세히 알아볼 수도 있습니다.
