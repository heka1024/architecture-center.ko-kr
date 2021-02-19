---
title: API 디자인 지침
titleSuffix: Best practices for cloud applications
description: 잘 디자인된 Web API를 만드는 방법에 관한 지침입니다.
author: dragon119
ms.date: 01/12/2018
ms.topic: best-practice
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.custom: seodec18
ms.openlocfilehash: 06090b0862a7c737d9ee93512f851d3fcf2e2d9f
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59640875"
---
# <a name="api-design"></a>API 디자인

대부분의 최신 웹 애플리케이션은 클라이언트가 애플리케이션과 상호 작용하는 데 사용할 수 있는 API를 표시합니다. 잘 디자인된 웹 API는 아래와 같은 특성을 지원해야 합니다.

- **플랫폼 독립성**. 모든 클라이언트는 내부에서 API가 구현되는 방법에 관계없이 API를 호출할 수 있어야 합니다. 그러려면 표준 프로토콜을 사용해야 하고, 클라이언트 및 웹 서비스가 교환할 데이터 형식에 동의할 수 있는 메커니즘이 있어야 합니다.

- **서비스 진화**. Web API는 클라이언트 애플리케이션과 독립적으로 기능을 진화시키고 추가할 수 있어야 합니다. API가 진화해도 기존 클라이언트 애플리케이션은 수정 없이 계속 작동할 수 있어야 합니다. 모든 기능은 클라이언트 애플리케이션이 해당 기능을 완전히 이용할 수 있도록 검색이 가능해야 합니다.

이 지침에서는 Web API를 디자인할 때 고려해야 하는 사항을 설명합니다.

## <a name="introduction-to-rest"></a>REST 소개

2000년에 Roy Fielding은 웹 서비스를 디자인하는 아키텍처 접근 방식으로 REST(Representational State Transfer)를 제안했습니다. REST는 하이퍼미디어 기반 분산 시스템을 구축하기 위한 아키텍처 스타일입니다. REST는 어떤 기본 프로토콜과도 독립적이며 HTTP에 연결될 필요가 없습니다. 그러나 대부분의 일반적인 REST 구현에서 애플리케이션 프로토콜로 HTTP를 사용하고, 이 지침에서는 HTTP를 위한 REST API 디자인에 중점을 둡니다.

REST가 HTTP보다 우수한 주요 장점은 개방형 표준을 사용하므로 API 또는 클라이언트 애플리케이션의 구현이 특정 구현에 바인딩되지 않는다는 것입니다. 예를 들어 REST 웹 서비스는 ASP.NET으로 작성할 수 있으며, 클라이언트 애플리케이션은 HTTP 요청을 생성하고 HTTP 응답을 구문 분석할 수 있는 모든 언어 또는 도구 집합을 사용할 수 있습니다.

다음은 HTTP를 사용하는 RESTful API의 몇 가지 기본 디자인 원칙입니다.

- REST API는 *리소스*를 중심으로 디자인되며, 클라이언트에서 액세스할 수 있는 모든 종류의 개체, 데이터 또는 서비스가 리소스에 포함됩니다.

- 리소스마다 해당 리소스를 고유하게 식별하는 URI인 *식별자*가 있습니다. 예를 들어 특정 고객 주문의 URI는 다음과 같습니다.

    ```HTTP
    https://adventure-works.com/orders/1
    ```

- 클라이언트가 리소스의 *표현*을 교환하여 서비스와 상호 작용합니다. 많은 Web API가 교환 형식으로 JSON을 사용합니다. 예를 들어 위에 나열된 URI에 대한 GET 요청은 이 응답 본문을 반환할 수 있습니다.

    ```json
    {"orderId":1,"orderValue":99.90,"productId":1,"quantity":1}
    ```

- REST API는 균일한 인터페이스를 사용하므로 클라이언트와 서비스 구현을 분리하는 데 도움이 됩니다. HTTP를 기반으로 하는 REST API의 경우 리소스에 표준 HTTP 동사 수행 작업을 사용하는 것이 균일한 인터페이스에 포함됩니다. 가장 일반적인 작업은 GET, POST, PUT, PATCH 및 DELETE입니다.

- REST API는 상태 비저장 요청 모델을 사용합니다. HTTP 요청은 독립적이어야 하고 임의 순서로 발생할 수 있으므로, 요청 사이에 일시적인 상태 정보를 유지할 수 없습니다. 정보는 리소스 자체에만 저장되며 각 요청은 자동 작업이어야 합니다. 이러한 제약 조건이 있기 때문에 웹 서비스의 확장성이 우수합니다. 클라이언트와 특정 서버 사이에 선호도를 유지할 필요가 없기 때문입니다. 모든 서버는 모든 클라이언트의 모든 요청을 처리할 수 있습니다. 그렇긴 하지만, 다른 요소가 확장성을 제한할 수 있습니다. 예를 들어 많은 웹 서비스가 백 엔드 데이터 저장소에 데이터를 쓰며, 이 경우 규모 확장이 어려울 수 있습니다. ([데이터 분할](./data-partitioning.md) 문서를 보시면 데이터 저장소의 규모 확장 전략에 대해 설명되어 있습니다.)

- REST API는 표현에 포함된 하이퍼미디어 링크에 따라 구동됩니다. 예를 들어 다음은 주문의 JSON 표현을 보여줍니다. 주문과 관련된 고객을 가져오거나 업데이트하는 링크를 포함하고 있습니다.

    ```json
    {
        "orderID":3,
        "productID":2,
        "quantity":4,
        "orderValue":16.60,
        "links": [
            {"rel":"product","href":"https://adventure-works.com/customers/3", "action":"GET" },
            {"rel":"product","href":"https://adventure-works.com/customers/3", "action":"PUT" }
        ]
    }
    ```

2008년에 Leonard Richardson은 Web API에 대한 다음과 같은 [성숙도 모델](https://martinfowler.com/articles/richardsonMaturityModel.html)을 제안했습니다.

- 수준 0: 한 URI를 정의합니다. 모든 작업은 이 URI에 대한 POST 요청입니다.
- 수준 1: 개별 리소스에 대한 별도의 URI를 만듭니다.
- 수준 2: HTTP 메서드를 사용하여 리소스에 대한 작업을 정의합니다.
- 수준 3: 하이퍼미디어(HATEOAS, 아래에 설명)를 사용합니다.

수준 3은 Fielding의 정의에 따르면 진정한 RESTful API에 해당합니다. 실제로 게시된 여러 Web API가 수준 2의 어딘가에 해당합니다.

## <a name="organize-the-api-around-resources"></a>리소스를 중심으로 API 구성

즉, 웹 API가 표시하는 비즈니스 엔터티에 집중해야 합니다. 예를 들어 전자 상거래 시스템에서는 기본 엔터티가 고객과 주문입니다. 주문 정보가 포함된 HTTP POST 요청을 전송하여 주문 만들기를 구현할 수 있습니다. HTTP 응답은 주문이 성공적으로 수행되었는지 여부를 나타냅니다. 가능하다면 리소스 URI는 동사(리소스에 대한 작업)가 아닌 명사(리소스)를 기반으로 해야 합니다.

```HTTP
https://adventure-works.com/orders // Good

https://adventure-works.com/create-order // Avoid
```

리소스는 단일 물리적 데이터 항목을 기반으로 할 필요가 없습니다. 예를 들어 주문 리소스는 내부적으로 관계형 데이터베이스의 여러 테이블로 구현할 수 있지만 클라이언트에 대해서는 단일 엔터티로 표시됩니다. 단순히 데이터베이스의 내부 구조를 반영하는 API를 만들지 마세요. REST의 목적은 엔터티 및 해당 엔터티에서 애플리케이션이 수행할 수 있는 작업을 모델링하는 것입니다. 클라이언트는 내부 구현에 노출되면 안 됩니다.

엔터티는 종종 컬렉션(주문, 고객)으로 그룹화됩니다. 컬렉션은 컬렉션 내 항목과는 별도의 리소스이며 고유한 URI가 있어야 합니다. 예를 들어 다음 URI는 주문 컬렉션을 나타낼 수 있습니다.

```HTTP
https://adventure-works.com/orders
```

컬렉션 URI에 HTTP GET 요청을 보내면 컬렉션에 있는 항목 목록을 검색합니다. 또한 컬렉션의 항목마다 고유의 URI가 있습니다. 항목의 URI에 대한 HTTP GET 요청은 해당 항목의 세부 정보를 반환합니다.

URI에 일관적인 명명 규칙을 적용합니다. 일반적으로 이렇게 하면 컬렉션을 참조하는 URI에 대해 복수 명사를 사용할 수 있습니다. 컬렉션 및 항목에 대한 URI를 계층 구조로 구성하는 것이 좋습니다. 예를 들어 `/customers`는 고객 컬렉션의 경로이고, `/customers/5`는 ID가 5인 고객의 경로입니다. 이 접근 방식을 사용하면 웹 API를 직관적으로 유지할 수 있습니다. 또한 많은 Web API 프레임워크는 매개 변수가 있는 URI 경로를 기반으로 요청을 라우팅할 수 있으므로 개발자는 경로 `/customers/{id}`에 대한 경로를 정의할 수 있습니다.

서로 다른 리소스 형식과 이러한 연결을 표시하는 방법 사이의 관계도 고려해야 합니다. 예를 들어 `/customers/5/orders`는 고객 5에 대한 모든 주문을 나타낼 수 있습니다. 반대 방향으로 이동하여 `/orders/99/customer` 같은 URI를 사용하여 주문에서 고객으로의 연결을 표시할 수도 있습니다. 그러나 이 모델을 너무 많이 확장하면 구현이 어려울 수 있습니다. HTTP 응답 메시지의 본문에 연결된 리소스에 대한 탐색 가능한 링크를 제공하는 방법이 더 좋습니다. 이 메커니즘은 섹션에서 자세히 설명 되어 [관련 된 리소스에 대 한 탐색을 사용 하도록 설정 하려면 사용 하 여 HATEOAS](#use-hateoas-to-enable-navigation-to-related-resources)합니다.

좀 더 복잡한 시스템에서는 `/customers/1/orders/99/products`처럼 클라이언트가 여러 관계 수준을 탐색할 수 있는 URI를 제공하고 싶을 수 있습니다. 그러나 이 수준의 복잡성은 유지하기 어려울 수 있으며 나중에 리소스 사이의 관계가 변하면 유연성이 떨어집니다. 그 대신 URI를 비교적 간단하게 유지해 보세요. 애플리케이션이 리소스 참조를 지정한 후에는 이 참조를 사용하여 해당 리소스와 관련된 항목을 찾을 수 있어야 합니다. 이전 쿼리를 `/customers/1/orders` URI로 바꿔서 고객 1의 모든 주문을 찾은 후 `/orders/99/products`로 바꿔서 이 주문의 제품을 찾을 수 있습니다.

> [!TIP]
> 리소스 URI를 *컬렉션/항목/컬렉션*보다 더 복잡하게 요구하지 않는 것이 좋습니다.

또 다른 요소는 모든 웹 요청이 웹 서버의 부하를 높인다는 점입니다. 요청이 많을수록 부하가 커집니다. 따라서 다수의 작은 리소스를 표시하는 "번잡한" Web API를 피하도록 노력해야 합니다. 이러한 API를 사용하려면 클라이언트 애플리케이션이 요구하는 모든 데이터를 찾을 수 있도록 여러 요청을 보내야 할 수 있습니다. 그 대신, 데이터를 비정규화하고 단일 요청을 통해 관련 정보를 검색할 수 있는 더 큰 리소스로 결합하는 것이 좋습니다. 단, 이 접근 방식과 클라이언트에 필요 없는 데이터를 가져오는 오버헤드의 균형을 조정해야 합니다. 큰 개체를 검색하면 요청의 대기 시간이 증가하고 추가 대역폭 비용이 발생할 수 있습니다. 이러한 성능 안티패턴에 대한 자세한 내용은 [번잡한 I/O](../antipatterns/chatty-io/index.md) 및 [불필요한 가져오기](../antipatterns/extraneous-fetching/index.md)를 참조하세요.

Web API와 기본 데이터 원본 사이에 종속성이 발생하지 않도록 해야 합니다. 예를 들어 데이터가 관계형 데이터베이스에 저장되는 경우 Web API는 각 테이블을 리소스 컬렉션으로 표시할 필요가 없습니다. 사실 이것은 서투른 디자인입니다. 그 대신 Web API를 데이터베이스의 추상화라고 생각해 보세요. 필요하다면 데이터베이스와 Web API 사이에 매핑 계층을 도입합니다. 이 방법을 사용하면 클라이언트 애플리케이션이 기본 데이터베이스 스키마의 변경 내용으로부터 격리됩니다.

마지막으로, 웹 API에 의해 구현된 일부 작업을 특정 리소스에 매핑하지 못할 수 있습니다. HTTP GET 요청을 통해 기능을 호출하고 결과를 HTTP 응답 메시지로 반환하는 *리소스가 아닌* 시나리오를 처리할 수 있습니다. 예를 들어 더하기 및 빼기 같은 단순한 계산기 작업을 구현하는 Web API는 이러한 작업을 의사 리소스로 표시하고 쿼리 문자열을 사용하여 필요한 매개 변수를 지정하는 URI를 제공할 수 있습니다. 예를 들어 URI */add?operand1=99&operand2=1*에 대한 GET 요청은 본문에 값 100이 포함된 응답 메시지를 반환합니다. 그러나 이러한 형식의 URI는 제한적으로만 사용해야 합니다.

## <a name="define-operations-in-terms-of-http-methods"></a>HTTP 메서드를 기준으로 작업 정의

HTTP 프로토콜은 요청에 의미 체계의미를 할당하는 다양한 메서드를 정의합니다. 대부분의 RESTful 웹 API에서 사용하는 일반적인 HTTP 메서드는 다음과 같습니다.

- **GET**은 지정된 URI에서 리소스의 표현을 검색합니다. 응답 메시지의 본문은 요청된 리소스의 세부 정보를 포함하고 있습니다.
- **POST**는 지정된 URI에 새 리소스를 만듭니다. 요청 메시지의 본문은 새 리소스의 세부 정보를 제공합니다. 참고로 POST를 사용하여 실제로 리소스를 만들지 않는 작업을 트리거할 수도 있습니다.
- **PUT**은 지정된 URI에 리소스를 만들거나 대체합니다. 요청 메시지의 본문은 만들 또는 업데이트할 리소스를 지정합니다.
- **PATCH**는 리소스의 부분 업데이트를 수행합니다. 요청 본문은 리소스에 적용할 변경 내용을 지정합니다.
- **DELETE**는 지정된 URI의 리소스를 제거합니다.

특정 요청의 효과는 리소스가 컬렉션인지 아니면 개별 항목인지에 따라 달라집니다. 다음 표는 전자 상거래 예제를 사용 하여 대부분의 RESTful 구현에서 적용하는 일반적인 규칙을 요약합니다. 참고로 이 요청 중 일부는 구현되지 않을 수 있으며, 구현 여부는 특정 시나리오에 따라 다릅니다.

| **리소스** | **POST** | **GET** | **PUT** | **DELETE** |
| --- | --- | --- | --- | --- |
| /customers |새 고객 만들기 |모든 고객 검색 |고객 대량 업데이트 |모든 고객 제거 |
| /customers/1 |오류 |고객 1에 대한 세부 정보 검색 |고객 1이 있는 경우 고객 1의 세부 정보 업데이트 |고객 1 제거 |
| /customers/1/orders |고객 1에 대한 새 주문 만들기 |고객 1에 대한 모든 주문 검색 |고객 1의 주문 대량 업데이트 |고객 1의 모든 주문 제거 |

POST, PUT 및 PATCH의 차이점을 구분하기 어려울 수 있습니다.

- POST 요청은 리소스를 만듭니다. 서버는 새 리소스에 대한 URI를 할당하고 클라이언트에 해당 URI를 반환합니다. REST 모델에서는 컬렉션에 POST 요청을 자주 적용합니다. 새 리소스가 컬렉션에 추가됩니다. POST 요청은 새 리소스를 만들지 않고 기존 리소스에 처리할 데이터를 보내는데 사용할 수도 있습니다.

- PUT 요청은 리소스를 만들거나 *또는* 기존 리소스를 업데이트합니다. 클라이언트는 리소스의 URI를 지정합니다. 요청 본문에는 리소스의 완전한 표현이 포함됩니다. 이 URI를 사용하는 리소스가 이미 있으면 리소스가 대체됩니다. 아직 없고 서버에서 리소스 만들기를 지원하는 경우 새 리소스가 생성됩니다. PUT 요청은 컬렉션보다는 특정 고객 같은 개별 항목인 리소스에 가장 자주 적용됩니다. 서버에서 PUT을 통한 업데이트를 지원하지만 만들기는 지원하지 않을 수 있습니다. PUT을 통한 만들기 지원 여부는 리소스가 존재하기 전에 클라이언트가 의미 있는 방법으로 리소스에 URI를 할당할 수 있는지 여부에 따라 결정됩니다. 할당할 수 없는 경우 POST를 사용하여 리소스를 만들고 PUT 또는 PATCH를 사용하여 업데이트합니다.

- PATCH 요청은 기존 리소스에 *부분 업데이트*를 수행합니다. 클라이언트는 리소스의 URI를 지정합니다. 요청 본문은 리소스에 적용할 *변경 내용*을 지정합니다. 클라이언트가 리소스의 전체 표현이 아닌 변경 내용만 보내기 때문에 PUT을 사용하는 것보다 이 방법이 더 효율적일 수 있습니다. 또한 서버에서 리소스 만들기를 지원하는 경우 기술적으로 PATCH는 새 리소스를 만들 수 있습니다("null" 리소스에 대한 업데이트를 지정하여).

PUT 요청은 idempotent여야 합니다. 클라이언트가 동일한 PUT 요청을 여러 번 제출하는 경우 그 결과가 항상 같아야 합니다(같은 값을 사용하여 같은 리소스가 수정되므로). POST 및 PATCH 요청이 반드시 idempotent가 된다는 보장은 없습니다.

## <a name="conform-to-http-semantics"></a>HTTP 의미 체계 준수

이 섹션에서는 HTTP 사양을 준수하는 API 디자인에 대한 몇 가지 일반적인 고려 사항을 설명합니다. 그러나 가능한 모든 세부 정보 또는 시나리오를 다루지는 않습니다. 궁금한 점은 HTTP 사양을 참조하세요.

### <a name="media-types"></a>미디어 유형

앞서 언급했듯이, 클라이언트와 서버는 리소스 표현을 교환합니다. 예를 들어 POST 요청에서는 요청 본문에 만들 리소스의 표현이 포함됩니다. GET 요청에서는 응답 본문에 가져온 리소스의 표현이 포함됩니다.

HTTP 프로토콜에서 형식은 MIME 유형이라고도 하는 *미디어 유형*을 사용하여 지정됩니다. 이진이 아닌 데이터의 경우 대부분의 Web API는 JSON(미디어 유형 = 애플리케이션/json) 및 XML(미디어 유형 = 애플리케이션/xml)을 지원합니다.

요청 또는 응답의 Content-Type 헤더는 표현 형식을 지정합니다. 다음은 JSON 데이터를 포함하는 POST 요청의 예입니다.

```HTTP
POST https://adventure-works.com/orders HTTP/1.1
Content-Type: application/json; charset=utf-8
Content-Length: 57

{"Id":1,"Name":"Gizmo","Category":"Widgets","Price":1.99}
```

서버에서 미디어 유형을 지원하지 않으면 HTTP 상태 코드 415(지원되지 않는 미디어 유형)를 반환해야 합니다.

클라이언트 요청에는 클라이언트가 응답 메시지에서 서버로부터 받는 미디어 유형 목록을 포함하는 Accept 헤더가 포함될 수 있습니다. 예: 

```HTTP
GET https://adventure-works.com/orders/2 HTTP/1.1
Accept: application/json
```

서버가 나열된 미디어 유형 중 어떤 것도 일치시킬 수 없는 경우 HTTP 상태 코드 406(허용되지 않음)을 반환해야 합니다.

### <a name="get-methods"></a>GET 메서드

성공적인 GET 메서드는 일반적으로 HTTP 상태 코드 200(정상)를 반환합니다. 리소스를 찾을 수 없는 경우 메서드가 404(찾을 수 없음)를 반환해야 합니다.

### <a name="post-methods"></a>POST 메서드

POST 메서드는 새 리소스를 만드는 경우 HTTP 상태 코드 201(만들어짐)을 반환합니다. 새 리소스의 URI는 응답의 Location 헤더에 포함됩니다. 응답 본문은 리소스의 표현을 포함합니다.

이 메서드가 일부 처리를 수행하지만 새 리소스를 만들지 않는 경우 메서드는 HTTP 상태 코드 200을 반환하고 작업의 결과를 응답 본문에 포함할 수 있습니다. 또는 반환할 결과가 없으면 메서드가 응답 본문 없이 HTTP 상태 코드 204(내용 없음)를 반환할 수 있습니다.

클라이언트가 잘못된 데이터를 요청에 배치하면 서버에서 HTTP 상태 코드 400(잘못된 요청)을 반환해야 합니다. 응답 본문에는 오류에 대한 추가 정보 또는 자세한 정보를 제공하는 URI 링크가 포함될 수 있습니다.

### <a name="put-methods"></a>PUT 메서드

PUT 메서드는 POST 메서드와 마찬가지로 새 리소스를 만드는 경우 HTTP 상태 코드 201(만들어짐)을 반환합니다. 이 메서드는 기존 리소스를 업데이트할 경우 200(정상) 또는 204(내용 없음)를 반환합니다. 상황에 따라 기존 리소스를 업데이트할 수 없는 경우도 있습니다. 이 경우 HTTP 상태 코드 409(충돌)를 반환하는 방안을 고려해야 합니다.

컬렉션의 복수 리소스에 대한 업데이트를 일괄 처리할 수 있는 일괄 HTTP PUT 작업의 구현을 생각해 보겠습니다. PUT 요청은 컬렉션의 URI를 지정해야 하며, 요청 본문에 수정할 리소스의 세부 정보를 지정해야 합니다. 이 접근 방식은 데이터 전송량을 줄이고 성능을 향상시킬 수 있습니다.

### <a name="patch-methods"></a>PATCH 메서드

클라이언트는 PATCH 요청을 사용하여 업데이트를 *패치 문서*의 형태로 기존 리소스에 보냅니다. 서버는 패치 문서를 처리하여 업데이트를 수행합니다. 패치 문서는 리소스 전체가 아니라 적용할 변경 내용만 설명합니다. PATCH 메서드에 대한 사양([RFC 5789](https://tools.ietf.org/html/rfc5789))은 패치 문서에 대한 특정 형식을 정의하지 않습니다. 형식은 요청의 미디어 형식에서 유추해야 합니다.

Web API에 대한 가장 일반적인 데이터 형식은 JSON일 것입니다. 두 가지 주요 JSON 기반 패치 형식으로 *JSON 패치* 및 *JSON 병합 패치*가 있습니다.

JSON 병합 패치는 비교적 간단합니다. 패치 문서는 원래 JSON 리소스와 동일한 구조를 갖지만 변경 또는 추가할 필드의 하위 집합만 포함하고 있습니다. 또한 패치 문서에서 필드 값에 대해 `null`을 지정하여 필드를 삭제할 수 있습니다. (즉, 원래 리소스가 명시적 null 값을 가질 수 있으면 병합 패치가 적합하지 않습니다.)

예를 들어 원래 리소스의 JSON 표현은 다음과 같습니다.

```json
{
    "name":"gizmo",
    "category":"widgets",
    "color":"blue",
    "price":10
}
```

이 리소스에 가능한 JSON 병합 패치는 다음과 같습니다.

```json
{
    "price":12,
    "color":null,
    "size":"small"
}
```

이를 통해 서버에게 `name`과 `category`는 그대로 둔 채, `price`를 업데이트하고, `color`를 삭제하고, `size`를 추가하라고 할 수 있습니다. JSON 병합 패치의 정확한 세부 정보는 [RFC 7396](https://tools.ietf.org/html/rfc7396)을 참조하세요. JSON 병합 패치의 미디어 유형은 `application/merge-patch+json`입니다.

원래 리소스가 명시적 null 값을 포함할 수 있으면 패치 문서에서 `null`이 갖는 특별한 의미 때문에 병합 패치가 적합하지 않습니다. 또한 패치 문서는 서버에서 업데이트를 적용할 순서를 지정하지 않습니다. 데이터 및 도메인에 따라 이것이 중요할 수도 있고 중요하지 않을 수도 있습니다. [RFC 6902](https://tools.ietf.org/html/rfc6902)에 정의된 JSON 패치는 좀 더 유연합니다. 작업의 결과로 적용할 변경 내용을 지정합니다. 작업에는 추가, 제거, 바꾸기, 복사 및 테스트(값의 유효성 검사)가 포함됩니다. JSON 패치의 미디어 유형은 `application/json-patch+json`입니다.

다음은 적절한 HTTP 상태 코드와 함께 PATCH 요청을 처리할 때 발생할 수 있는 몇 가지 일반적인 오류 조건입니다.

| 오류 조건 | HTTP 상태 코드 |
|-----------|------------|
| 지원되지 않는 패치 문서 형식입니다. | 415(지원되지 않는 미디어 형식) |
| 패치 문서의 형식이 잘못되었습니다. | 400(잘못된 요청) |
| 패치 문서가 유효하지만 현재 상태에서는 변경 내용을 리소스에 적용할 수 없습니다. | 409(충돌)

### <a name="delete-methods"></a>DELETE 메서드

삭제 작업이 성공하면 웹 서버는 프로세스가 성공적으로 처리되었지만 응답 본문에 추가 정보가 포함되지 않았음을 나타내는 HTTP 204 상태 코드로 응답해야 합니다. 리소스가 없는 경우 웹 서버는 HTTP 404(찾을 수 없음)를 반환할 수 있습니다.

### <a name="asynchronous-operations"></a>비동기 작업

경우에 따라 POST, PUT, PATCH 또는 DELETE 작업을 수행하려면 완료하는 데 약간 시간이 걸리는 처리 작업이 필요할 수 있습니다. 처리 작업이 완료될 때까지 기다렸다가 클라이언트에 응답을 보내는 경우 허용되지 않는 수준의 대기 시간이 발생할 수 있습니다. 이 경우 비동기 작업을 수행하는 방안을 고려해 보아야 합니다. 요청 처리가 수락되었지만 아직 완료되지 않았음을 나타내는 HTTP 상태 코드 202(수락됨)를 반환합니다.

클라이언트가 상태 엔드포인트를 폴링하여 상태를 모니터링할 수 있도록 비동기 요청의 상태를 반환하는 엔드포인트를 표시해야 합니다. 202 응답의 Location 헤더에 상태 엔드포인트의 URI를 포함합니다. 예: 

```HTTP
HTTP/1.1 202 Accepted
Location: /api/status/12345
```

클라이언트가 이 엔드포인트에 GET 요청을 보내는 경우 응답에 요청의 현재 상태가 포함되어야 합니다. 필요에 따라 예상 완료 시간 또는 작업 취소 링크를 포함할 수 있습니다.

```HTTP
HTTP/1.1 200 OK
Content-Type: application/json

{
    "status":"In progress",
    "link": { "rel":"cancel", "method":"delete", "href":"/api/status/12345" }
}
```

비동기 작업에서 새 리소스를 만드는 경우 작업 완료 후 상태 엔드포인트에서 상태 코드 303(다른 항목 보기)을 반환해야 합니다. 303 응답에 새 리소스의 URI를 제공하는 Location 헤더를 포함합니다.

```HTTP
HTTP/1.1 303 See Other
Location: /api/orders/12345
```

자세한 내용은 [REST의 비동기 작업](https://www.adayinthelifeof.nl/2011/06/02/asynchronous-operations-in-rest/)을 참조하세요.

## <a name="filter-and-paginate-data"></a>데이터 필터링 및 페이지 매기기

단일 URI를 통해 리소스 컬렉션을 표시하면 정보의 하위 집합만 필요할 때에도 애플리케이션이 대량의 데이터를 가져올 수 있습니다. 예를 들어 클라이언트 애플리케이션에서 비용이 특정 값을 초과하는 모든 주문을 찾아야 한다고 가정해 봅시다. 클라이언트 응용 프로그램은 */orders* URI에서 모든 주문을 검색한 후 클라이언트 쪽에서 이러한 주문을 필터링할 것입니다. 이 프로세스는 매우 비효율적입니다. Web API를 호스팅하는 서버의 네트워크 대역폭 및 처리 성능이 낭비됩니다.

이 방법 대신, */orders?minCost=n*처럼 API가 URI의 쿼리 문자열에서 필터 전달을 허용할 수 있습니다. 그러면 Web API가 쿼리 문자열의 `minCost` 매개 변수를 구문 분석 및 처리하고 서버 쪽에서 필터링된 결과를 반환합니다.

컬렉션 리소스에 대한 GET 요청은 다수의 항목을 반환할 가능성이 있습니다. 단일 요청에서 반환하는 데이터의 양이 제한되도록 Web API를 디자인해야 합니다. 검색할 최대 항목 수와 컬렉션의 시작 오프셋을 지정하는 쿼리 문자열을 지원하는 방안을 고려해 봅니다. 예: 

```HTTP
/orders?limit=25&offset=50
```

또한 서비스 거부 공격을 방지하기 위해 반환되는 항목 수를 제한하는 방안도 고려해 봅니다. 클라이언트 애플리케이션을 돕기 위해, 페이지가 매겨진 데이터를 반환하는 GET 요청은 컬렉션의 사용할 수 있는 총 리소스 수를 나타내는 모종의 메타데이터 형식을 포함해야 합니다.

필드 이름을 */orders?sort=ProductID* 같은 값으로 가져오는 정렬 매개 변수를 제공하여 데이터를 가져올 때 데이터를 정렬하는 비슷한 전략을 사용할 수 있습니다. 그러나 쿼리 문자열 매개 변수는 여러 캐시 구현에서 캐시된 데이터의 키로 사용되는 리소스 식별자의 일부를 구성하기 때문에 이 접근 방식은 캐싱에 나쁜 영향을 미칠 수 있습니다.

각 항목에 대량의 데이터가 포함된 경우 각 항목에 대해 반환되는 필드를 제한하도록 이 접근 방식을 확장할 수 있습니다. 예를 들어 쉼표로 구분된 필드 목록을 수락하는 */orders?fields=ProductID,Quantity* 같은 쿼리 문자열 매개 변수를 사용할 수 있습니다.

쿼리 문자열의 모든 선택적 매개 변수에 의미 있는 기본값을 제공합니다. 예를 들어 페이지 매김을 구현하는 경우 `limit` 매개 변수를 10으로, `offset` 매개 변수를 0으로 설정하고, 주문을 구현하는 경우 정렬 매개 변수를 리소스의 키로 설정하고, 프로젝션을 지원하는 경우 `fields` 매개 변수를 리소스의 모든 필드로 설정합니다.

## <a name="support-partial-responses-for-large-binary-resources"></a>대용량 이진 리소스에 대한 부분 응답 지원

리소스에 파일 또는 이미지 같은 대용량 이진 필드가 포함될 수 있습니다. 신뢰할 수 없는 간헐적 연결에 의해 야기되는 문제를 해결하고 응답 시간을 개선하려면 이러한 리소스를 청크로 검색할 수 있게 하는 방안을 고려해 보세요. 이렇게 하려면 Web API가 큰 리소스의 GET 요청에 대해 Accept-Ranges 헤더를 지원해야 합니다. 이 헤더는 GET 작업이 부분 요청을 지원한다는 것을 나타냅니다. 클라이언트 애플리케이션은 바이트 범위로 지정된 리소스 하위 집합을 반환하는 GET 요청을 제출할 수 있습니다.

또한 이러한 리소스에 대해 HTTP HEAD 요청을 구현하는 방안을 고려해 봅니다. HEAD 요청은 리소스에 대해 설명하는 HTTP 헤더만 반환하고 메시지 본문이 비어 있다는 점을 제외하면 GET 요청과 비슷합니다. 클라이언트 애플리케이션은 부분적인 GET 요청을 사용하여 리소스를 가져올지 여부를 결정하는 HEAD 요청을 사용할 수 있습니다. 예: 

```HTTP
HEAD https://adventure-works.com/products/10?fields=productImage HTTP/1.1
```

다음은 응답 메시지 예제입니다.

```HTTP
HTTP/1.1 200 OK

Accept-Ranges: bytes
Content-Type: image/jpeg
Content-Length: 4580
```

Content-Length 헤더는 총 리소스 크기를 제공하고, Accept-Ranges 헤더는 해당 GET 작업이 일부 결과를 지원함을 나타냅니다. 클라이언트 애플리케이션은 이 정보를 사용하여 더 작은 청크에서 이미지를 검색할 수 있습니다. 첫 번째 요청은 범위 헤더를 사용하여 처음 2500 바이트를 가져옵니다.

```HTTP
GET https://adventure-works.com/products/10?fields=productImage HTTP/1.1
Range: bytes=0-2499
```

응답 메시지는 HTTP 상태 코드 206을 반환하여 이 응답이 부분 응답임을 나타냅니다. Content-Length 헤더는 메시지 본문에 반환된 실제 바이트 수(리소스의 크기가 아닌)를 지정하며, Content-Range 헤더는 해당 바이트가 리소스의 어느 부분인지(4580 바이트 중 바이트 0-2499)를 나타냅니다.

```HTTP
HTTP/1.1 206 Partial Content

Accept-Ranges: bytes
Content-Type: image/jpeg
Content-Length: 2500
Content-Range: bytes 0-2499/4580

[...]
```

클라이언트 애플리케이션의 후속 요청은 리소스의 나머지 부분을 검색할 수 있습니다.

## <a name="use-hateoas-to-enable-navigation-to-related-resources"></a>HATEOAS를 사용하여 관련 리소스 탐색

REST를 실행하는 기본적인 동기 중 하나는 URI 체계에 대해 미리 알고 있지 않아도 전체 리소스 집합을 탐색할 수 있어야 하기 때문입니다. 각 HTTP GET 요청은 응답에 포함된 하이퍼링크를 통해 요청된 개체와 직접 관련된 리소스를 찾는 데 필요한 정보를 반환해야 하며, 이러한 각 리소스에 대해 사용할 수 있는 작업을 설명하는 정보도 제공되어야 합니다. 이 원칙을 HATEOAS(Hypertext as the Engine of Application State)라 합니다. 시스템은 실질적으로 유한 상태 시스템으로서, 각 요청에 대한 응답은 한 상태에서 다른 상태로 바꾸는 데 필요한 정보를 포함하고 있으며, 다른 정보는 필요하지 않습니다.

> [!NOTE]
> 현재 HATEOAS 원칙을 모델링하는 방법을 정의하는 표준 또는 사양은 없습니다. 이 섹션에 나오는 예제에서는 한 가지 가능한 해결 방법을 보여 줍니다.

예를 들어 주문과 고객 간의 관계를 처리하기 위해 주문 고객에게 사용 가능한 작업을 식별하는 링크를 주문 표현에 포함할 수 있습니다. 다음은 가능한 표현입니다.

```json
{
  "orderID":3,
  "productID":2,
  "quantity":4,
  "orderValue":16.60,
  "links":[
    {
      "rel":"customer",
      "href":"https://adventure-works.com/customers/3",
      "action":"GET",
      "types":["text/xml","application/json"]
    },
    {
      "rel":"customer",
      "href":"https://adventure-works.com/customers/3",
      "action":"PUT",
      "types":["application/x-www-form-urlencoded"]
    },
    {
      "rel":"customer",
      "href":"https://adventure-works.com/customers/3",
      "action":"DELETE",
      "types":[]
    },
    {
      "rel":"self",
      "href":"https://adventure-works.com/orders/3",
      "action":"GET",
      "types":["text/xml","application/json"]
    },
    {
      "rel":"self",
      "href":"https://adventure-works.com/orders/3",
      "action":"PUT",
      "types":["application/x-www-form-urlencoded"]
    },
    {
      "rel":"self",
      "href":"https://adventure-works.com/orders/3",
      "action":"DELETE",
      "types":[]
    }]
}
```

이 예에서 `links` 배열에는 링크 집합이 있습니다. 각 링크는 관련 엔터티에 대한 작업을 나타냅니다. 각 링크의 데이터에는 관계("고객"), URI(`https://adventure-works.com/customers/3`), HTTP 메서드 및 지원되는 MIME 형식이 포함됩니다. 이 모든 정보가 있어야 클라이언트 애플리케이션이 작업을 호출할 수 있습니다.

또한 `links` 배열은 검색된 리소스 자체에 대한 자체 참조 정보를 포함합니다. 이러한 관계가 *자체* 관계입니다.

리소스의 상태에 따라 반환되는 링크 집합이 달라질 수 있습니다. 이것이 바로 "애플리케이션 상태 엔진"이라는 하이퍼텍스트가 의미하는 바입니다.

## <a name="versioning-a-restful-web-api"></a>RESTful 웹 API 버전 관리

Web API가 정적 상태를 유지할 가능성은 매우 낮습니다. 비즈니스 요구 사항이 변경됨에 따라 자원의 새 컬렉션이 추가될 수 있으므로, 리소스 간의 관계가 변할 수 있으며 리소스 데이터의 구조가 수정될 수 있습니다. 웹 API를 새로운 또는 서로 다른 요구 사항을 처리하도록 업데이트하는 동안 해당 변경이 웹 API를 사용하는 클라이언트 애플리케이션에 미치는 영향을 고려해야 합니다. 문제는 개발자가 해당 API를 완전히 제어할 수 있는 웹 API를 디자인 및 구현하더라도, 해당 개발자는 원격으로 작업하는 제3자 조직이 구축할 수 있는 클라이언트 애플리케이션을 같은 정도로 제어하지 못한다는 것입니다. 따라서 새 클라이언트 애플리케이션이 새 기능과 리소스의 장점을 이용할 수 있도록 하면서도 기존 클라이언트 애플리케이션이 변경되지 않고 계속 작동할 수 있도록 해야 합니다.

버전 관리를 사용하면 웹 API는 자신이 표시하는 기능과 리소스를 나타낼 수 있으며, 클라이언트 애플리케이션은 기능 또는 리소스의 특정 버전으로 지정된 요청을 제출할 수 있습니다. 다음 섹션에서는 각각 자체의 이점과 절충점을 가지고 있는 다양한 접근 방식을 설명합니다.

### <a name="no-versioning"></a>버전 관리 없음

가장 간단한 방법이며 일부 내부 API에 대해 허용될 수 있습니다. 큰 변화는 새 리소스 또는 새 연결로 나타낼 수 있습니다. 기존 리소스에 콘텐츠를 추가해도 이 콘텐츠가 표시될 것으로 예상하지 않은 클라이언트 애플리케이션은 해당 콘텐츠를 무시할 것이므로 주요 변경 내용이 표시되지 않을 수 있습니다.

예를 들어 URI `https://adventure-works.com/customers/3`에 대한 요청은 클라이언트 애플리케이션이 예상하는 `id`, `name` 및 `address` 필드가 포함된 단일 고객의 세부 정보를 반환해야 합니다.

```HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{"id":3,"name":"Contoso LLC","address":"1 Microsoft Way Redmond WA 98053"}
```

> [!NOTE]
> 간단한 설명을 위해 이 섹션에 표시된 예제 응답은 HATEOAS 링크를 포함하고 있지 않습니다.

`DateCreated` 필드가 고객 리소스의 체계에 추가되면 응답은 다음과 같습니다.

```HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{"id":3,"name":"Contoso LLC","dateCreated":"2014-09-04T12:11:38.0376089Z","address":"1 Microsoft Way Redmond WA 98053"}
```

기존 클라이언트 애플리케이션은 인식되지 않은 필드를 무시할 수 있으면 계속 올바르게 작동할 수 있으며, 한편 새 클라이언트 애플리케이션을 새 필드를 처리하도록 디자인할 수 있습니다. 그러나 리소스가 더 크게 변경되거나(필드 제거 또는 이름 변경 등) 리소스 간의 관계가 변경된 경우에는 이러한 변화가 주요 변경 내용으로 인식되어 기존 클라이언트 애플리케이션이 올바르게 작동하지 못할 수 있습니다. 이러한 상황에서는 다음 방법 중 하나를 고려해야 합니다.

### <a name="uri-versioning"></a>URI 버전 관리

웹 API를 수정하거나 리소스의 체계를 변경할 때마다 각 리소스의 URI에 버전 번호를 추가합니다. 앞에서는 기존 URI가 전과 같이 계속 작동하여 원래 체계를 준수하는 리소스를 반환해야 합니다.

앞의 예제를 확장하여 `address` 필드가 주소의 각 구성 부분을 포함하고 있는 하위 필드(예: `streetAddress`, `city`, `state` 및 `zipCode`)로 재구성된다면, `https://adventure-works.com/v2/customers/3` 같은 버전 번호가 들어 있는 URI를 통해 리소스의 이 버전을 표시할 수 있습니다.

```HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{"id":3,"name":"Contoso LLC","dateCreated":"2014-09-04T12:11:38.0376089Z","address":{"streetAddress":"1 Microsoft Way","city":"Redmond","state":"WA","zipCode":98053}}
```

이 버전 관리 메커니즘은 매우 간단하지만 요청을 적절한 엔드포인트로 라우팅하는 서버에 따라 달라집니다. 그러나 여러 번 반복을 통해 웹 API가 성숙해짐에 따라 이 메커니즘을 다룰 수 없게 될 수 있으며 서버가 다양한 버전을 지원해야 합니다. 또한 엄격히 말해서, 클라이언트 애플리케이션이 같은 데이터(고객 3)를 가져오므로, URI가 버전에 따라 달라져서는 안 됩니다. 또한 이 체계는 모든 링크가 자신의 URI에 버전 번호를 포함해야 하므로 HATEOAS 구현을 복잡하게 만듭니다.

### <a name="query-string-versioning"></a>쿼리 문자열 버전 관리

여러 URI를 제공하는 대신, HTTP 요청에 추가된 쿼리 문자열 내에 `https://adventure-works.com/customers/3?version=2` 같은 매개 변수를 사용하여 리소스의 버전을 지정할 수 있습니다. 버전 매개 변수는 이전 클라이언트 애플리케이션에서 생략했다면 기본적으로 1과 같은 의미 있는 값입니다.

이 접근 방식은 같은 리소스가 언제나 같은 URI에서 검색된다는 의미 체계 장점이 있지만, 쿼리 문자열을 구문 분석하고 해당 HTTP 응답을 다시 보내기 위해 요청을 처리하는 코드에 따라 달라집니다. 또한 이 접근 방식은 HATEOAS를 URI 버전 관리 메커니즘으로 구현할 때와 같이 복잡합니다.

> [!NOTE]
> 일부 구형 웹 브라우저와 웹 프록시는 URI에 쿼리 문자열을 포함하는 요청에 대한 응답을 캐싱하지 않습니다. Web API를 사용 하 고 해당 웹 브라우저 내에서 실행 되는 웹 응용 프로그램의 성능을 저하 시킬 수 있습니다.

### <a name="header-versioning"></a>헤더 버전 관리

버전 번호를 쿼리 문자열 매개 변수로 추가하지 않고 리소스의 버전을 나타내는 사용자 지정 헤더를 구현할 수 있습니다. 이 접근 방식을 사용하려면 클라이언트 애플리케이션이 적절한 헤더를 요청에 추가해야 하지만, version 헤더가 생략된 경우 클라이언트 요청을 처리하는 코드가 기본값(버전 1)을 사용할 수 있습니다. 다음 예제에서는 *Custom-Header*라는 사용자 지정 헤더를 사용합니다. 이 헤더의 값은 웹 API의 버전을 나타냅니다.

버전 1:

```HTTP
GET https://adventure-works.com/customers/3 HTTP/1.1
Custom-Header: api-version=1
```

```HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{"id":3,"name":"Contoso LLC","address":"1 Microsoft Way Redmond WA 98053"}
```

버전 2:

```HTTP
GET https://adventure-works.com/customers/3 HTTP/1.1
Custom-Header: api-version=2
```

```HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{"id":3,"name":"Contoso LLC","dateCreated":"2014-09-04T12:11:38.0376089Z","address":{"streetAddress":"1 Microsoft Way","city":"Redmond","state":"WA","zipCode":98053}}
```

참고로 이전의 두 방법 방식과 마찬가지로 HATEOAS를 구현하려면 모든 링크에 적절한 사용자 지정 헤더를 포함해야 합니다.

### <a name="media-type-versioning"></a>미디어 형식 버전 관리

이 지침의 앞부분에서 설명한 대로, 클라이언트 애플리케이션은 웹 서버에 HTTP GET 요청을 보낼 때 Accept 헤더를 사용하여 처리할 수 있는 콘텐츠의 형식을 지정해야 합니다. 흔히 *Accept* 헤더의 목적은 클라이언트 애플리케이션에서 응답 본문이 XML, JSON 또는 클라이언트가 구문 분석할 수 있는 몇몇 다른 일반적인 형식 중 어느 형식인지 지정할 수 있도록 하는 것입니다. 그러나 클라이언트 애플리케이션이 예상하는 리소스의 버전을 나타낼 수 있도록 하는 정보를 포함한 사용자 지정 미디어 형식을 정의할 수 있습니다. 다음 예제는 *Accept* 헤더를 값 *application/vnd.adventure-works.v1+json*과 함께 지정하는 요청을 나타냅니다. *vnd.adventure-works.v1* 요소는 웹 서버에 대해 리소스의 버전 1을 반환해야 한다는 것을 나타내며, 한편 *json* 요소는 응답 본문의 형식이 JSON이어야 함을 지정합니다.

```HTTP
GET https://adventure-works.com/customers/3 HTTP/1.1
Accept: application/vnd.adventure-works.v1+json
```

요청을 처리하는 코드는 *Accept* 헤더를 처리하고 가능하면 해당 헤더를 적용해야 합니다. 클라이언트 애플리케이션은 *Accept* 헤더에 복수의 형식을 지정할 수 있으며, 이 경우 웹 서버는 응답 본문에 가장 적절한 형식을 선택할 수 있습니다. 웹 서버는 Content-Type 헤더를 사용하여 응답 본문에 있는 데이터의 형식을 확인합니다.

```HTTP
HTTP/1.1 200 OK
Content-Type: application/vnd.adventure-works.v1+json; charset=utf-8

{"id":3,"name":"Contoso LLC","address":"1 Microsoft Way Redmond WA 98053"}
```

Accept 헤더가 모든 알려진 미디어 형식을 지정하지 않은 경우, 웹 서버는 HTTP 406(승인 금지) 응답 메시지를 생성하거나 기본 미디어 형식이 포함된 메시지를 반환할 수 있습니다.

이 접근 방식은 엄격히 말해서 버전 관리 메커니즘인지 여부에 대한 논란의 여지가 있으며 당연히 리소스 링크에 관련 데이터의 MIME 형식을 포함할 수 있는 HATEOAS에 적합합니다.

> [!NOTE]
> 버전 관리 전략을 선택할 때에는 성능에 미치는 영향, 특히 웹 서버의 캐싱을 고려해야 합니다. URI 버전 관리 및 쿼리 문자열 버전 관리 체계는 같은 URI/쿼리 문자열 조합이 매번 같은 데이터를 참조하므로 캐싱하기에 적합합니다.
>
> 일반적으로 헤더 버전 관리 및 미디어 형식 버전 관리 메커니즘에는 사용자 지정 헤더 또는 Accept 헤더의 값을 검사하기 위해 추가 논리가 필요합니다. 대규모 환경의 경우, 서로 다른 버전의 웹 API를 사용하는 많은 클라이언트가 서버 쪽 캐시에 상당한 양의 중복된 데이터를 발생시킬 수 있습니다. 클라이언트 애플리케이션이 캐싱을 구현하는 프록시를 통해 웹 서버와 통신하는 경우 이 문제가 심각할 수 있으며, 현재 요청된 데이터의 복사본을 자체의 캐시에 저장하지 않은 경우에만 요청을 웹 서버에 전달해야 합니다.

## <a name="open-api-initiative"></a>Open API Initiative

[Open API Initiative](https://www.openapis.org/)는 공급업체에서 REST API 설명을 표준화하기 위해 업계 컨소시엄에서 만들었습니다. 이 이니셔티브의 일부로, Swagger 2.0 사양의 명칭이 OAS(Open API Specification)로 바뀐 후 Open API Initiative에 추가되었습니다.

사용하는 웹 API에 OpenAPI를 채택할 수도 있습니다. 몇 가지 고려할 점은 다음과 같습니다.

- OpenAPI 사양에는 REST API 디자인 방식에 대한 독자적인 지침이 포함되어 있습니다. 이 사양은 상호 운용성 측면에서는 이점이 있지만, 사양에 맞게 API를 디자인할 때는 좀 더 주의해야 합니다.

- OpenAPI는 구현 우선 방식이 아닌, 계약 우선 방식을 권장합니다. 계약 우선 방식에서는 API 계약(인터페이스)을 먼저 디자인한 후 계약을 구현하는 코드를 작성합니다.

- Swagger와 같은 도구는 API 계약에서 클라이언트 라이브러리 또는 문서를 생성할 수 있습니다. 예를 들어, [Swagger를 사용하는 ASP.NET 웹 API 도움말 페이지](/aspnet/core/tutorials/web-api-help-pages-using-swagger)를 참조하세요.

## <a name="more-information"></a>자세한 정보

- [Microsoft REST API 지침](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md). 공용 REST API 디자인에 대한 구체적인 권장 사항입니다.

- [Web API 검사 목록](https://mathieu.fenniak.net/the-api-checklist/). Web API를 디자인 및 구현할 때 고려해야 하는 항목 목록입니다.

- [Open API Initiative](https://www.openapis.org/). Open API에 대한 설명서 및 구현 세부 정보입니다.
