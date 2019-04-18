---
title: 'CAF: Azure의 작동 방식'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
description: Azure의 내부 작동 방식 설명
author: petertaylor9999
ms.date: 02/11/2019
ms.openlocfilehash: 215e4e4954a2f3e722e3ac865fea19508f6edadd
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59068823"
---
<!-- markdownlint-disable MD026 -->

# <a name="how-does-azure-work"></a>Azure의 작동 방식

Azure는 Microsoft의 공용 클라우드 플랫폼입니다. Azure는 대규모 platform-as-a-service (PaaS), 인프라 (IaaS) 서비스 (DBaaS) 기능으로 데이터베이스 서비스를 비롯 하 여 서비스의 컬렉션을 제공 합니다. 하지만 Azure란 정확히 무엇이며 어떤 방식으로 작동할까요?

<!-- markdownlint-disable MD034 -->

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2ixGo]

<!-- markdownlint-enable MD034 -->

다른 클라우드 플랫폼과 마찬가지로, Azure는 **가상화**라는 기술에 의존합니다. 대부분의 컴퓨터 하드웨어는 간단히 말해서 실리콘에 영구적으로 또는 반영구적으로 인코딩되는 명령 집합이므로, 소프트웨어에 에뮬레이트할 수 있습니다. 소프트웨어 명령을 하드웨어의 명령에 매핑하는 에뮬레이션 계층을 사용하여, 가상화된 하드웨어는 마치 실제 하드웨어인 것처럼 소프트웨어에서 실행될 수 있습니다.

기본적으로, 클라우드는 고객을 대신해서 가상화된 하드웨어를 실행하는 하나 이상의 데이터 센터의 물리적 서버 집합입니다. 그렇다면 클라우드는 어떻게 수백만 명의 고객을 위해 수백만 개의 가상화된 하드웨어 인스턴스를 동시에 만들고 시작하고 중지하고 삭제할 수 있을까요?

이를 이해하기 위해 데이터 센터의 하드웨어 아키텍처를 살펴보겠습니다.  각 데이터 센터 내에서 컬렉션인 서버 랙에 앉아 서버. 각 서버 랙에는 네트워크 연결 및 전력을 공급하는 PDU(전원 분배 장치)를 제공하는 네트워크 스위치와 함께 많은 서버 **블레이드**가 포함되어 있습니다. 경우에 따라 랙은 **클러스터**라는 좀 더 큰 단위로 함께 그룹화됩니다.

각 랙 또는 클러스터 내에서, 대부분의 서버는 사용자 대신 이러한 가상화된 하드웨어 인스턴스를 실행하도록 지정됩니다. 그러나 패브릭 컨트롤러 라고 하는 클라우드 관리 소프트웨어를 실행할 서버 중 일부입니다. **패브릭 컨트롤러**는 많은 책임을 담당하는 분산된 애플리케이션입니다. 서비스를 할당하고, 서버와 해당 서버에서 실행되는 서비스의 상태를 모니터링하고, 실패할 경우 서버를 복구합니다.

패브릭 컨트롤러의 각 인스턴스는 클라우드 오케스트레이션 소프트웨어를 실행하는 **프런트 엔드**라고도 하는 또 다른 서버 집합에 연결됩니다. 프런트 엔드는 웹 서비스, RESTful API 및 클라우드가 수행하는 모든 기능에 사용되는 내부 Azure Database를 호스트합니다.

프런트 엔드와 같은 Azure 리소스를 할당할 고객 요청을 처리 하는 서비스를 호스트 하는 예를 들어 [가상 네트워크](/azure/virtual-network/virtual-networks-overview)를 [가상 머신](/azure/virtual-machines), 및와 같은 서비스 [CosmosDB](/azure/cosmos-db/introduction). 첫째, 프런트 엔드는 사용자의 유효성을 검사하고 사용자가 요청된 리소스를 할당할 수 있도록 허가되는지 확인합니다. 그렇다면 프런트 엔드는 충분 한 용량이 있는 서버 랙을 찾은 데이터베이스를 검사 하 고 패브릭 컨트롤러는 리소스를 할당 하는 랙에 지시 합니다.

따라서 기본적으로 Azure는 서버 및 해당 서버에서 가상화 된 하드웨어 및 소프트웨어의 작업 및 복잡 한 집합이 구성을 오케스트레이션 하는 분산된 응용 프로그램을 실행 하는 네트워킹 하드웨어의 방대한 컬렉션입니다. 또한 이러한 오케스트레이션을 통해 Azure가 강력한 &mdash; 사용자는 더 이상 유지 관리 하 고 Azure가 백그라운드에서 이러한 모든 않기 때문에 하드웨어를 업그레이드 하는 일을 담당 합니다.

## <a name="next-steps"></a>다음 단계

Azure 내부 기능을 이해 했으므로 클라우드 리소스 거 버 넌 스에 알아봅니다.

> [!div class="nextstepaction"]
> [리소스 거버넌스 알아보기](what-is-governance.md)

<!-- Links -->

[docs-add-users-to-aad]: /azure/active-directory/add-users-azure-active-directory?toc=/azure/architecture/cloud-adoption-guide/toc.json
