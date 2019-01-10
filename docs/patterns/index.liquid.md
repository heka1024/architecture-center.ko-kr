---
title: 클라우드 디자인 패턴
titleSuffix: Azure Architecture Center
description: Microsoft Azure에 대한 클라우드 디자인 패턴
keywords: Azure
author: dragon119
ms.date: 12/10/2018
ms.custom: seodec18
ms.openlocfilehash: 873d4cf02690a2cc3ffe4f35b044dedf70700fb5
ms.sourcegitcommit: 680c9cef945dff6fee5e66b38e24f07804510fa9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54011026"
---
# <a name="cloud-design-patterns"></a>클라우드 디자인 패턴

[!INCLUDE [header](../../_includes/header.md)]

이러한 디자인 패턴은 클라우드에서 안정적이고 확장성 있는 안전한 애플리케이션을 빌드하는 데 유용합니다.

각 패턴은 패턴이 해결하는 문제, 패턴을 적용하기 위한 고려 사항 및 Microsoft Azure 기반의 예제에 대해 설명합니다. 대부분의 패턴은 Azure에서 패턴을 구현하는 방법을 보여주는 코드 샘플 또는 코드 조각을 포함하고 있습니다. 그러나 대부분의 패턴은 Azure에 호스팅되든 다른 클라우드 플랫폼에 호스팅되든, 분산 시스템과 관련되어 있습니다.

## <a name="problem-areas-in-the-cloud"></a>클라우드의 문제 영역

<!-- markdownlint-disable MD033 -->

<ul id="categories" class="panel">
{%- for category in categories %}
    <li>
    {% include 'pattern-category-card' %}
    </li>
{%- endfor %}
</ul>

<!-- markdownlint-enable MD033 -->

## <a name="catalog-of-patterns"></a>패턴 카탈로그

| 패턴 | 요약 |
|---------|---------|
|         |         |

{%- for pattern in patterns %} | [{{ pattern.title }}](./{{ pattern.file }}) | {{ pattern.description }} | {%- endfor %}
