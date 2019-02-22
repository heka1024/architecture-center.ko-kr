---
title: 'CAF: 소프트웨어 정의 네트워크 - 클라우드 네이티브'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 클라우드 네이티브 가상 네트워킹 서비스 설명
author: rotycenh
ms.openlocfilehash: c6200491bc9ba35a9f00e0003e51716b58628980
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901242"
---
# <a name="software-defined-networks-cloud-native"></a>소프트웨어 정의 네트워크: 네이티브 클라우드

클라우드 네이티브 가상 네트워크는 가상 머신 같은 IaaS 리소스를 클라우드 플랫폼에 배포할 때 필요합니다. 외부 소스에서 가상 네트워크에 액세스하려면 웹과 마찬가지로 명시적 프로비전이 필요합니다. 이러한 유형의 가상 네트워크는 서브넷, 라우팅 규칙 및 가상 방화벽/트래픽 관리 디바이스 생성을 지원합니다.

클라우드 네이티브 가상 네트워크는 클라우드 호스팅 워크로드를 지원하기 위해 조직의 온-프레미스 또는 비 클라우드 리소스에 종속되지 않습니다. 필요한 모든 리소스는 가상 네트워크 자체에 프로비저닝되거나 관리형 PaaS 제품을 사용하여 프로비저닝됩니다.

## <a name="cloud-native-assumptions"></a>클라우드 네이티브 가정

클라우드 네이티브 가상 네트워크를 배포할 때 다음과 같이 가정합니다.

- 가상 네트워크에 배포하는 워크로드는 온-프레미스 네트워크 내에서만 액세스할 수 있는 애플리케이션 또는 서비스에 종속되지 않습니다. 공용 인터넷을 통해 액세스할 수 있는 엔드포인트를 제공하지 않는 한, 온-프레미스에 내부적으로 호스팅되는 애플리케이션 및 서비스를 클라우드 플랫폼에 호스팅되는 리소스에 사용할 수 없습니다.
- 워크로드의 ID 관리 및 액세스 제어는 클라우드 환경에 호스팅되는 클라우드 플랫폼의 ID 서비스 또는 IaaS 서버에 따라 달라집니다. 온-프레미스 또는 다른 외부 위치에 호스팅되는 ID 서비스에 직접 연결할 필요는 없습니다.
- ID 서비스는 온-프레미스 디렉터리를 사용하여 SSO(Single Sign-On)를 지원할 필요가 없습니다.

클라우드 네이티브 가상 네트워크는 외부 종속성이 없습니다. 따라서 배포 및 구성이 간단하며, 덕분에 이 아키텍처는 실험이나 기타 소규모 자체 포함 또는 신속한 반복 배포에 가장 적합한 선택인 경우가 많습니다.

클라우드 도입 팀은 클라우드 네이티브 가상 네트워킹 아키텍처를 설명할 때 다음과 같은 추가 문제를 고려해야 합니다.

- 스토리지 또는 인증 서비스 같은 클라우드 기반 기능을 활용하려면 온-프레미스 데이터 센터에서 실행되도록 설계된 기존 워크로드를 대대적으로 수정해야 합니다.
- 클라우드 네이티브 네트워크는 클라우드 플랫폼 관리 도구를 통해서만 관리되고, 따라서 시간이 지나면 기존 IT 표준에서 관리 및 정책 확산이 발생할 수 있습니다.

## <a name="learn-more"></a>자세한 정보

Azure 플랫폼의 클라우드 네이티브 가상 네트워킹에 대한 자세한 내용은 다음을 참조하세요.

- [Azure Virtual Network: 방법 가이드](/azure/virtual-network/virtual-network-vnet-plan-design-arm). 새로 만들어진 Azure Virtual Networks는 기본적으로 클라우드 네이티브입니다. 가상 네트워크의 디자인 및 배포 계획을 수립할 때 다음 가이드를 사용하세요.
- [구독 제한: 네트워킹](/azure/azure-subscription-service-limits?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits)을 선택합니다. 모든 단일 가상 네트워크 및 연결된 리소스는 단일 구독 내에만 존재할 수 있으며, 구독 제한이 적용됩니다.
