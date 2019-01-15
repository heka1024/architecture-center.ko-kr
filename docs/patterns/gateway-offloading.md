---
title: 게이트웨이 오프로딩 패턴
titleSuffix: Cloud Design Patterns
description: 공유 또는 특수 서비스 기능을 게이트웨이 프록시에 오프로드합니다.
keywords: 디자인 패턴
author: dragon119
ms.date: 06/23/2017
ms.custom: seodec18
ms.openlocfilehash: 50af3d8593279986ed6efee55667187424c18e56
ms.sourcegitcommit: 680c9cef945dff6fee5e66b38e24f07804510fa9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54010217"
---
# <a name="gateway-offloading-pattern"></a>게이트웨이 오프로딩 패턴

공유 또는 특수 서비스 기능을 게이트웨이 프록시에 오프로드합니다. 이 패턴은 SSL 인증서 사용과 같은 공유 서비스 기능을 애플리케이션의 다른 부분에서 게이트웨이로 이동하여 애플리케이션 개발을 간소화할 수 있습니다.

## <a name="context-and-problem"></a>컨텍스트 및 문제점

일부 기능은 여러 서비스에서 공통적으로 사용되며, 이러한 기능은 구성, 관리 및 유지 관리가 필요합니다. 모든 애플리케이션 배포에서 분산되는 공유 또는 특수 서비스는 관리 오버헤드를 높이며, 배포 오류의 발생 가능성을 높입니다. 공유 기능에 대한 업데이트는 해당 기능을 공유하는 모든 서비스에 배포해야 합니다.

보안 문제(토큰 유효성 검사, 암호화, SSL 인증서 관리) 및 기타 복잡한 작업을 제대로 처리하려면 팀원들에게 매우 전문적인 기술이 필요할 수 있습니다. 예를 들어 애플리케이션에 필요한 인증서를 모든 애플리케이션 인스턴스에서 구성 및 배포해야 합니다. 각각의 새로운 배포에서 인증서가 만료되지 않도록 관리해야 합니다. 모든 애플리케이션 배포에서 만료 예정인 모든 공용 인증서를 업데이트하고 테스트하고 확인해야 합니다.

인증, 권한 부여, 로깅, 모니터링 또는 [제한](./throttling.md)과 같은 기타 공통 서비스를 많은 수의 배포에서 구현하고 관리하는 것은 어려울 수 있습니다. 오버헤드와 오류 가능성을 줄이기 위해서는 이러한 유형의 기능을 통합하는 것이 더 나을 수 있습니다.

## <a name="solution"></a>해결 방법

일부 기능, 특히 인증서 관리, 인증, SSL 종료, 모니터링, 프로토콜 변환 또는 제한과 같이 교차 적용되는 문제를 API 게이트웨이에 오프로드합니다.

다음 다이어그램에서는 인바운드 SSL 연결을 종료하는 API 게이트웨이를 보여 줍니다. 여기서는 원래 요청자 대신, API 게이트웨이의 업스트림 HTTP 서버에서 데이터를 요청합니다.

 ![게이트웨이 오프로딩 패턴의 다이어그램](./_images/gateway-offload.png)

이 패턴의 이점은 다음과 같습니다.

- 웹 서버 인증서 및 보안 웹 사이트의 구성 등과 같은 지원 리소스를 배포하고 유지 관리할 필요를 없앰으로써 서비스의 개발을 단순화합니다. 구성이 좀 더 단순해지면 관리 및 확장이 용이해지며, 서비스 업그레이드가 더 간단해집니다.

- 보안처럼 전문 지식이 필요한 기능은 전담 팀에서 구현하도록 합니다. 이렇게 하면 이러한 전문적이면서도 교차 적용되는 문제를 관련 전문가에게 맡기고, 핵심 팀은 애플리케이션 기능에 주력할 수 있습니다.

- 요청 및 응답 로깅/모니터링에 대해 일관성을 유지합니다. 서비스가 올바르게 계측되지 않더라도, 최소 수준의 모니터링 및 로깅을 보장하도록 게이트웨이를 구성할 수 있습니다.

## <a name="issues-and-considerations"></a>문제 및 고려 사항

- API 게이트웨이가 높은 가용성을 유지하고 오류에 대해 복원력을 가지는지 확인합니다. API 게이트웨이의 여러 인스턴스를 실행하여 단일 실패 지점을 방지합니다.
- 게이트웨이가 애플리케이션 및 엔드포인트의 용량 및 크기 조정 요구 사항에 맞게 디자인되었는지 확인합니다. 게이트웨이가 애플리케이션에서 병목 현상을 유발하는 요인이 되지 않으면서 충분히 확장 가능하도록 합니다.
- 보안 또는 데이터 전송 등의 전체 애플리케이션에서 사용되는 기능만 오프로드합니다.
- 비즈니스 논리는 API 게이트웨이로 절대 오프로드하지 않아야 합니다.
- 트랜잭션을 추적해야 할 경우 로깅을 위한 상관 관계 ID를 생성하는 것이 좋습니다.

## <a name="when-to-use-this-pattern"></a>이 패턴을 사용해야 하는 경우

다음의 경우에 이 패턴을 사용합니다.

- 애플리케이션 배포에 SSL 인증서 또는 암호화와 같은 공유 문제가 있습니다.
- 메모리 리소스, 저장소 용량 또는 네트워크 연결 등의 다른 리소스 요구 사항이 있을 수 있는 여러 애플리케이션 배포에서 공통되는 기능이 필요합니다.
- 네트워크 보안, 제한 또는 기타 네트워크 경계 관련 문제 등에 대한 책임을 좀 더 전문적인 팀으로 전환하려고 합니다.

이러한 패턴은 서비스 간에 결합을 발생시킬 경우 적합하지 않을 수 있습니다.

## <a name="example"></a>예

Nginx를 SSL 오프로드 어플라이언스로 사용할 경우 다음 구성은 인바운드 SSL 연결을 종료하고 업스트림 HTTP 서버 3개 중 하나로 연결을 분산합니다.

```console
upstream iis {
        server  10.3.0.10    max_fails=3    fail_timeout=15s;
        server  10.3.0.20    max_fails=3    fail_timeout=15s;
        server  10.3.0.30    max_fails=3    fail_timeout=15s;
}

server {
        listen 443;
        ssl on;
        ssl_certificate /etc/nginx/ssl/domain.cer;
        ssl_certificate_key /etc/nginx/ssl/domain.key;

        location / {
                set $targ iis;
                proxy_pass http://$targ;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto https;
proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header Host $host;
        }
}
```

## <a name="related-guidance"></a>관련 지침

- [프런트 엔드에 대한 백 엔드 패턴](./backends-for-frontends.md)
- [게이트웨이 집계 패턴](./gateway-aggregation.md)
- [게이트웨이 라우팅 패턴](./gateway-routing.md)
