---
title: 'CAF: 규정 준수 소개'
description: 규정 준수란?
author: BrianBlanchard
ms.date: 2/8/2019
ms.openlocfilehash: 5ff7b591d7cab647a99cee223e6271928e185a2f
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55902047"
---
# <a name="introduction-to-regulatory-compliance"></a>규정 준수 소개

이 문서는 규정 준수에 대한 소개 문서이므로 규정 준수 전략을 구현하기 위한 것이 아닙니다. 단지 일반적으로 이해하기 위한 것입니다. [Azure 규정 준수 제안](https://aka.ms/allcompliance)에 대한 자세한 내용은 [Microsoft 보안 센터]에서 사용할 수 있습니다. 또한 다운로드 가능한 모든 설명서는 [Microsoft Service Trust Portal](https://servicetrust.microsoft.com/)의 비공개 계약에 따라 Azure 고객에게 제공됩니다.

규정 준수는 회사에서 해당 지리적 위치 또는 업계의 관리 기관이 제정한 법률을 준수하도록 하는 규율과 프로세스를 가리킵니다. IT 규정 준수에서는 법률과 규제로 설정된 정책 및 절차의 위반을 탐지하고 방지하기 위해 사용자 및/또는 프로세스에서 회사 시스템을 모니터링합니다. 결과적으로 이는 매우 광범위한 모니터링 및 적용 프로세스에 다시 적용됩니다. 업계 및 지리적 위치에 따라 이러한 프로세스는 상당히 길고 복잡해질 수 있습니다.

다국적 조직(특히 의료 및 금융 서비스와 같이 규제가 심한 업계에 속한 조직)의 경우 규정 준수가 매우 어려울 수 있습니다. 표준과 규정이 풍부하지만 자주 변경되므로 기업에서 진화하는 국제 전자 데이터 처리 법률에 뒤처지지 않도록 하는 것이 어렵습니다.

보안 제어와 마찬가지로 조직에서는 클라우드의 규정 준수와 관련된 책임 분담을 이해해야 합니다. 클라우드 공급자는 자체의 플랫폼과 서비스에서 규정을 준수하는지 확인하기 위해 노력하고 있습니다. 그러나 조직은 자체의 애플리케이션과 타사에서 제공하는 애플리케이션이 규정을 준수하는지도 확인해야 합니다. 마찬가지로 클라우드 서비스를 사용하는 규제 산업의 애플리케이션에도 클라우드 공급자의 인증이 필요할 수 있습니다.

다양한 산업 및 지리적 위치의 준수 규정에 대한 설명은 다음과 같습니다.

## <a name="hipaa"></a>HIPAA

PHI(보호된 상태 정보)를 처리하는 의료 애플리케이션은 HIPAA(Health Information Portability and Accountability Act)에 포함된 개인 정보 규칙과 보안 규칙을 모두 따릅니다. 최소한 HIPAA는 의료 기업에서 PHI를 받거나 만든 모든 PHI를 보호한다는 서면 보증을 클라우드 공급자로부터 받도록 요구할 수 있습니다.

## <a name="pci"></a>PCI

PCI DSS(지불 카드 업계 데이터 보안 표준)는 Visa, Master Card, American Express, Discover 및 JCB를 포함한 주요 카드 체계에 속한 유명 상표의 신용 카드를 처리하는 조직을 위한 독점적인 정보 보안 표준입니다. PCI 표준은 카드 상표를 통해 위임되며, 지불 카드 업계 보안 표준 위원회(Payment Card Industry Security Standards Council)에서 관리합니다. 이 표준은 신용 카드 사기를 줄이기 위해 카드 소유자 데이터에 대한 제어를 강화하기 위해 만들어졌습니다. 규정 준수에 대한 유효성 검사는 매년 외부의 QSA(Qualified Security Assessor), 대량의 트랜잭션을 처리하는 조직에 대한 ROC(Report on Compliance)를 작성하는 회사 관련 ISA(Internal Security Assessor) 또는 기업에 대한 SAQ(Self-Assessment Questionnaire)를 통해 수행됩니다.

## <a name="pii"></a>PII

PII(개인 식별 정보)는 소비자, 직원, 파트너 또는 기타 생명이 있는 실체 또는 법적 실체를 식별하는 데 사용할 수 있는 모든 데이터 요소입니다. 많은 신규 법률, 특히 개인 정보 및 개별 PII를 다루는 법률에서는 기업 자체에서 규정 준수 및 발생할 수 있는 모든 위반을 보고하도록 요구하고 있습니다.

## <a name="gdpr"></a>GDPR

이 영역에서 가장 중요한 개발 중 하나는 유럽 연합 내의 개인에 대한 데이터 보호를 강화하기 위해 설계된 유럽 GDPR(일반 데이터 보호 규정)에 대한 유럽 위원회(European Commission)의 최근 제정입니다. GDPR에서는 "이름, 집 주소, 사진, 이메일 주소, 은행 세부 정보, 소셜 네트워킹 웹 사이트의 게시물, 의료 정보 또는 컴퓨터의 IP 주소"와 같은 개인 관련 데이터가 EU 내의 서버에서 유지 관리되고 외부로 전송되지 않도록 요구합니다. 또한 회사는 개인에게 모든 데이터 침해를 알리고 데이터 보호 관리자가 회사에 있어야 한다고 규정하고 있습니다. 다른 국가에서도 비슷한 유형의 규정을 갖추고 있거나 개발하고 있습니다.

## <a name="compliant-foundation-in-azure"></a>Azure의 규정 준수 기반

고객이 전 세계의 규제 산업 및 시장에서 자체 규정 준수 의무를 이행할 수 있도록 Azure는 너비(총 제안 수) 및 깊이(평가 범위에 있는 고객 관련 서비스 수) 측면에서 업계의 가장 큰 규정 준수 포트폴리오를 유지 관리하고 있습니다. Azure 규정 준수 제안은 4개 세그먼트, 즉 글로벌 적용 가능, US Government, 산업별 및 지역/국가별로 그룹화됩니다.

Azure 규정 준수 제안은 Microsoft에서 생성한 계약 수정, 자체 평가, 고객 지침 문서뿐만 아니라 독립적인 타사 감사 업체에서 생성한 공식 인증, 증명, 유효성 검사, 권한 부여 및 평가도 포함한 다양한 유형의 보증을 기반으로 합니다. 이 문서의 각 제안 설명에는 Azure 고객 관련 서비스가 평가 범위에 있음을 나타내는 최신 범위 설명과 함께 고객이 자체 규정 준수 의무를 이행하는 데 도움이 되는 다운로드 가능한 리소스에 대한 링크가 제공됩니다.

Azure 규정 준수 제안에 대한 자세한 내용은 [Microsoft 보안 센터](/trustcenter/compliance/complianceofferings)에서 사용할 수 있습니다. 또한 다운로드 가능한 모든 설명서는 다음 섹션에 있는 [Service Trust Portal](https://servicetrust.microsoft.com)의 비공개 계약에 따라 Azure 고객에게 제공됩니다.

* 감사 보고서: FedRAMP, GRC 평가, ISO, PCI DSS 및 SOC 보고서 섹션 포함
* 데이터 보호 리소스: 규정 준수 가이드, FAQ 및 백서, 침투 테스트 및 보안 평가 섹션 포함
