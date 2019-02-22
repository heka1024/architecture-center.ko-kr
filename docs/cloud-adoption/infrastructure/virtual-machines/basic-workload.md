---
title: 'CAF: 기본 워크로드 배포'
description: Azure에 기본 워크로드를 배포하는 방법 설명
author: petertaylor9999
ms.date: 12/31/2018
ms.openlocfilehash: bd3d006100e68f2fa1d71deeb0c72180ad4c19ea
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55902063"
---
# <a name="deploy-a-basic-workload-in-azure"></a>Azure에서 기본 워크로드 배포

*워크로드*라는 용어는 일반적으로 애플리케이션이나 서비스와 같은 기능의 임의 단위로 정의됩니다. 그러므로 서버에 배포되는 코드 아티팩트와 애플리케이션 관련 기타 서비스 측면에서 워크로드의 개념을 쉽게 이해할 수 있습니다. 워크로드의 정의는 온-프레미스 애플리케이션이나 서비스에는 유용한 수 있지만 클라우드 애플리케이션의 경우에는 정의의 범위를 확장해야 합니다.

클라우드에서 워크로드는 모든 아티팩트뿐만 아니라 클라우드 리소스도 포함합니다. 이 정의에 클라우드 리소스가 포함되는 이유는 "infrastructure as code"라는 개념 때문입니다. [Azure 작동 방식](../../getting-started/what-is-azure.md)에서 알아본 것처럼 Azure의 리소스는 오케스트레이터 서비스에서 배포됩니다. 이 오케스트레이터 서비스는 웹 API를 통해 기능을 표시하며, 웹 API는 PowerShell/Azure CLI(명령줄 인터페이스)/Azure Portal 등의 여러 도구를 사용하여 호출할 수 있습니다. 즉, 애플리케이션과 연결된 코드 아티팩트와 함께 저장될 수 있는 컴퓨터가 읽을 수 있는 파일에서 Azure 리소스를 지정할 수 있습니다.

이렇게 하면 코드 아티팩트 및 필요한 클라우드 리소스 측면에서 워크로드를 정의할 수 있으므로 워크로드를 추가로 격리할 수 있습니다. 리소스가 구성되는 방식, 네트워크 토폴로지 또는 기타 특성을 기준으로 워크로드를 격리할 수 있습니다. 워크로드 격리의 목적은 팀이 해당 리소스의 모든 측면을 독립적으로 관리할 수 있도록 워크로드의 특정 리소스를 팀과 연결하는 데 있습니다. 이렇게 하면 여러 팀이 Azure에서 리소스 관리 서비스를 공유하는 반면 각자의 리소스가 의도하지 않게 삭제되거나 수정되는 것을 방지할 수 있습니다.

이 격리를 통해 DevOps라는 또 다른 개념을 파악할 수 있습니다. DevOps는 위에서 설명한 소프트웨어 개발 및 IT 작업이 모두 포함되는 소프트웨어 개발 사례를 포함하며, 자동화 사용 방법을 최대한 많이 추가합니다. DevOps의 원리 중 하나는 지속적인 통합 및 지속적인 업데이트(CI/CD)로 알려져 있습니다. 연속 통합이란 개발자가 코드 변경을 커밋할 때마다 실행되는 자동화된 빌드 프로세스를 지칭합니다. 그리고 지속적인 업데이트는 테스트용 개발 환경이나 최종 배포용 프로덕션 환경과 같은 여러 환경에 이 코드를 배포하는 자동화된 프로세스를 지칭합니다.

## <a name="basic-workload"></a>기본 워크로드

*기본 워크로드*는 일반적으로 단일 웹 애플리케이션 또는 VM(가상 머신)이 포함된 VNet(가상 네트워크)으로 정의됩니다.

> [!NOTE]
> 이 가이드는 애플리케이션 개발을 다루지 않습니다. Azure에서 애플리케이션을 개발하는 방법에 대한 자세한 내용은 [Azure 애플리케이션 아키텍처 가이드](/azure/architecture/guide/)를 참조하세요.

워크로드가 웹 애플리케이션인지 아니면 VM인지에 관계 없이 이러한 각 배포에는 *리소스 그룹*이 필요합니다. 아래 단계를 진행하기 전에 리소스 그룹 생성 권한이 있는 사용자가 이 리소스 그룹을 만들어야 합니다.

## <a name="basic-web-application-paas"></a>기본 웹 애플리케이션(PaaS)

기본 웹 애플리케이션의 경우 [웹앱 설명서](/azure/app-service?toc=/azure/architecture/cloud-adoption-guide/toc.json)에서 5분 빠른 시작 중 하나를 선택하여 단계를 수행합니다.

> [!NOTE]
> 일부 빠른 시작 가이드에는 리소스 그룹 배포 작업이 기본적으로 포함되어 있습니다. 이 경우에 리소스 그룹을 명시적으로 만들 필요는 없습니다. 그렇지 않으면 위에서 만든 리소스 그룹에 웹 애플리케이션을 배포합니다.

간단한 워크로드를 배포하면 [기본 웹 애플리케이션](/azure/architecture/reference-architectures/app-service-web-app/basic-web-app?toc=/azure/architecture/cloud-adoption-guide/toc.json)을 Azure에 배포하는 검증된 방법에 대해 자세히 알아볼 수 있습니다.

## <a name="single-windows-or-linux-vm-iaas"></a>단일 Windows 또는 Linux VM(IaaS)

VM에서 실행되는 간단한 워크로드의 경우 첫 단계는 가상 네트워크를 배포하는 것입니다. 가상 머신, 부하 분산 장치 및 게이트웨이와 같은 Azure의 모든 IaaS(Infrastructure as a Service) 리소스에는 가상 네트워크가 필요합니다. [Azure 가상 네트워크](/azure/virtual-network/virtual-networks-overview?toc=/azure/architecture/cloud-adoption-guide/toc.json)에 대해 알아본 다음, [포털을 사용하여 Azure에 Virtual Network를 배포](/azure/virtual-network/quick-create-portal?toc=/azure/architecture/cloud-adoption-guide/toc.json)하는 단계를 수행합니다. Azure Portal에서 가상 네트워크에 대한 설정을 지정할 때 위에서 만든 리소스 그룹의 이름을 지정해야 합니다.

다음 단계에서는 단일 Windows 또는 Linux VM을 배포할지를 결정합니다. Windows VM의 경우 [포털에서 Azure에 Windows VM을 배포](/azure/virtual-machines/windows/quick-create-portal?toc=/azure/architecture/cloud-adoption-guide/toc.json)하는 단계를 수행합니다. 다시 Azure Portal에서 가상 머신에 대한 설정을 지정할 때 위에서 만든 리소스 그룹의 이름을 지정합니다.

단계를 수행하고 VM을 배포하면 [Azure에서 Windows VM을 실행하는 입증된 방법](/azure/architecture/reference-architectures/virtual-machines-windows/single-vm?toc=/azure/architecture/cloud-adoption-guide/toc.json)에 대해 알아볼 수 있습니다. Linux VM의 경우 [포털에서 Azure에 Linux VM을 배포](/azure/virtual-machines/linux/quick-create-portal?toc=/azure/architecture/cloud-adoption-guide/toc.json)하는 단계를 수행합니다. [Azure에서 Linux VM을 실행하기 위한 입증된 방법](/azure/architecture/reference-architectures/virtual-machines-linux/single-vm?toc=/azure/architecture/cloud-adoption-guide/toc.json)에 대해 자세히 알아볼 수도 있습니다.

## <a name="next-steps"></a>다음 단계

Azure Cloud에서 핵심 인프라 구성 요소를 사용하는 방법은 [아키텍처 관련 결정 가이드](../../decision-guides/overview.md)를 참조하세요.
