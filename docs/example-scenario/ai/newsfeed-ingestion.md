---
title: Azure에서 뉴스 피드 대량 수집 및 분석
description: Azure Cosmos DB 및 Azure Cognitive Services를 포함한 Azure 서비스만을 사용하여 RSS 뉴스 피드에서 텍스트, 이미지, 감정 및 다른 데이터를 수집하고 분석하는 파이프라인을 만듭니다.
author: njray
ms.date: 2/1/2019
ms.custom: azcat-ai, AI
social_image_url: /azure/architecture/example-scenario/ai/media/mass-ingestion-newsfeeds-architecture.png
ms.topic: example-scenario
ms.service: architecture-center
ms.subservice: example-scenario
ms.openlocfilehash: e1bc636103753d474b545a6e3e9118b2ef302b34
ms.sourcegitcommit: ea97ac004c38c6b456794c1a8eef29f8d2b77d50
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58489267"
---
# <a name="mass-ingestion-and-analysis-of-news-feeds-on-azure"></a>Azure에서 뉴스 피드 대량 수집 및 분석

이 예제 시나리오는 거의 실시간으로 분석할 공용 RSS 뉴스 피드를 사용 하 여 문서 및 대량 수집에 대 한 파이프라인을 설명 합니다.  Azure Cognitive Services를 사용 하 여 텍스트 번역, 얼굴 인식 및 감정 검색을 포함 하 여 유용한 정보를 제공 합니다.

이 시나리오에 대 한 예제가 [영어][english]합니다 [러시아어][russian], 및 [독일어] [ german] 뉴스 피드, 있지만 쉽게 확장할 수 다른 RSS 피드입니다. 쉽게 배포 하기에 대 한 데이터 수집, 처리 및 분석 전적으로 Azure 서비스에 기반한 합니다.

## <a name="relevant-use-cases"></a>관련 사용 사례

이 시나리오를 처리 RSS 피드를 기반으로 하는 동안에 문서, 웹 사이트 또는 문서에는 필요한 모든 관련 된:

* 선택한 언어로 텍스트를 변환 합니다.
* 디지털 콘텐츠에서 키 구, 엔터티 및 사용자 데이터를 찾을.
* 디지털 문서와 연결 된 이미지에서 개체, 텍스트 및 랜드마크를 검색 합니다.
* 디지털 콘텐츠를 사용 하 여 연결 된 모든 이미지에 해당 성별 및 연령으로 사용자를 검색 합니다.

## <a name="architecture"></a>아키텍처

![][architecture]

솔루션을 통한 데이터 흐름은 다음과 같습니다.

1. RSS 뉴스 피드는 문서 또는 문서에서 데이터를 가져오는 생성자 처럼 작동 합니다. 예를 들어 아티클을 사용 하 여 데이터 일반적으로 제목, 뉴스 항목의 원래 본문의 요약이 포함 됩니다 및 경우에 따라 이미지입니다.

2. Azure Cosmos DB에 문서와 연결 된 모든 이미지를 삽입 하는 생성기 또는 수집 프로세스 [컬렉션][collection]합니다.

3. 알림을은 CosmosDB 및 Azure Blob Storage에서 문서 이미지 (있는 경우)의 문서 텍스트를 저장 하는 Azure Functions에서 수집 함수를 트리거합니다.  그런 다음 문서는 다음 큐로 전달 됩니다.

4. 변환 함수는 큐 이벤트에 의해 트리거됩니다. 사용 된 [텍스트 변환 API] [ translate-text] 언어를 검색 하도록 Azure Cognitive Services의 필요한 경우 변환 하 고 제목 및 본문에서 감정, 핵심 문구 및 엔터티를 수집 합니다. 그러면 다음 큐로 문서를 전달합니다.

5. 검색 함수는 큐에 대기 중인된 문서에서 트리거됩니다. 사용 된 [Computer Vision] [ vision] 개체, 랜드마크, 및 연결된 된 이미지에서 작성 된 단어 다음 문서를 전달 다음 큐를 검색 하는 서비스입니다.

6. 얼굴 함수가 트리거될 큐에 대기 중인된 문서에서 트리거됩니다. 사용 된 [Azure Face API] [ face] 검색할 서비스 성별 및 연령에 연결된 된 이미지에 대 한 얼굴을 다음 큐로 문서를 전달 합니다.

7. 모든 함수가 완료 되 면 알림 함수가 트리거될 수 있습니다. 아티클에 대 한 처리 된 레코드를 로드 하 고 모든 결과 검색 합니다. 발견 내용을 플래그가 지정 된 경우 사용자가 선택한 시스템에는 알림이 전송 됩니다.

함수는 각 처리 단계에서 Azure Cosmos DB에 결과 씁니다. 궁극적으로 데이터를 사용할 수 있습니다 원하는대로 합니다. 예를 들어, 비즈니스 프로세스를 개선, 새 고객을 찾거나, 고객 만족 문제를 식별 하려면 사용할 수 있습니다.

### <a name="components"></a>구성 요소

다음 Azure 구성 요소 목록은이 예제에서 사용 됩니다.

* [Azure Storage] [ storage] 원시 이미지 및 문서와 연결 된 비디오 파일을 저장 하는 데 사용 됩니다. 보조 저장소 계정을 Azure App Service를 사용 하 여 만들어지고 Azure Function 코드 및 로그를 호스트 하는 데 사용 됩니다.

* [Azure Cosmos DB] [ cosmos-db] 포함 문서 텍스트, 이미지 및 비디오 정보를 추적 합니다. Cognitive Services 단계의 결과가 여기에 저장 됩니다.

* [Azure Functions] [ functions] 큐 메시지에 응답 하 고 들어오는 콘텐츠를 변환 하는 데 사용 하는 함수 코드를 실행 합니다. [Azure App Service] [ aas] 함수 코드를 호스트 하 고 레코드를 순차적으로 처리 합니다. 이 시나리오는 다섯 가지 함수를 포함 합니다. 수집, 변환, 얼굴, 개체 감지 및 알림.

* [Azure Service Bus] [ service-bus] 함수가 사용 하는 Azure Service Bus 큐를 호스트 합니다.

* [Azure Cognitive Services] [ acs] 의 구현을 기반으로 하는 파이프라인에 대 한 인공 지능을 제공 합니다 [Computer Vision] [ vision] 서비스 [얼굴 API][face], 및 [번역할 텍스트] [ translate-text] 기계 번역 서비스입니다.

* [Azure Application Insights] [ aai] 문제를 진단할 수 있도록 하는 응용 프로그램의 기능을 이해 하는 분석을 제공 합니다.

### <a name="alternatives"></a>대안

* 알림 큐 및 Azure Functions에 따라 패턴을 사용 하는 대신이 데이터 흐름에 대 한 또 다른 패턴을 사용 합니다. 예를 들어 [Azure Service Bus 토픽] [ topics] 다양 한 부분으로 병렬로 문서 반대로이 예제에서 수행 되는 직렬 처리 하는 프로세스에 사용할 수 있습니다. 자세한 내용은 비교 [큐 및 토픽][queues-topics]합니다.

* 사용 하 여 [Azure Logic App] [ logic-app] 함수 코드를 구현 하 여 레코드 수준 같은 잠금을 구현 [Redlock] [ redlock] (병렬 필요 합니다. Azure Cosmos DB는 지원 될 때까지 처리 [부분 문서 업데이트][partial]). 자세한 내용은 [Functions 및 Logic Apps 비교][compare]합니다.

* 기존 Azure 서비스 하는 대신 사용자 지정된 AI 구성 요소를 사용 하 여이 아키텍처를 구현 합니다. 예를 들어, 성별, 일반 사용자 수를 달리 이미지에서 특정인을 검색 하는 사용자 지정된 모델을 사용 하 여 파이프라인을 확장 하 고이 예제에서 수집 된 데이터를 기간. 이 아키텍처에서 사용자 지정된 기계 학습 또는 인공 지능 모델 사용 하려면 Azure Functions에서 호출할 수 있도록에 RESTful 끝점으로 모델을 빌드하십시오.

* RSS 피드 대신 다른 입력된 메커니즘을 사용 합니다. Azure Cosmos DB 및 Azure Storage를 제공 하기 위해 여러 생성기 또는 수집 프로세스를 사용 합니다.

## <a name="considerations"></a>고려 사항

편의상이 예제 시나리오는 사용 가능한 Api 및 Azure Cognitive Services에서 서비스의 일부만을 사용합니다. 예를 들어, 이미지의 텍스트를 분석할 수 있습니다 사용 하 여 [Text Analytics API][text-analytics]합니다. 이 시나리오에서 대상 언어는 영어로 간주 됩니다 있지만 모든 입력을 변경할 수 있습니다 [지원 되는 언어] [ language] 세요.

### <a name="scalability"></a>확장성

Azure Functions 크기 조정에 따라 달라 집니다 합니다 [호스팅 계획] [ plan] 사용 합니다. 이 솔루션에서는 가정 된 [소비 계획][plan-c], 어떤 계산 power 자동으로 필요한 경우 함수에 할당 됩니다. 함수는 실행 하는 경우에 지불 합니다. 다른 옵션은 사용 하는 [Azure App Service] [ plan-aas] 계획 서로 다른 양의 리소스를 할당 하는 계층 간에 크기를 조정할 수 있습니다.

Azure Cosmos DB를 사용 하 여 키가 균등 하 게 워크 로드가 거의 충분히 많은 수의 간에 [파티션 키][keys]합니다. 컨테이너를 저장할 수 있는 데이터의 총 크기 또는 총 제한이 [처리량] [ throughput] 컨테이너를 지원할 수 있는 합니다.

### <a name="management-and-logging"></a>관리 및 로깅

이 솔루션에서는 [Application Insights] [ aai] 성능 및 로깅 정보를 수집 하도록 합니다. 이 배포에 필요한 다른 서비스와 동일한 리소스 그룹 배포를 사용 하 여 Application Insights 인스턴스에 만들어집니다.

솔루션에서 생성 한 로그를 보려면:

1. 로 이동 [Azure portal] [ portal] 배포에 대해 만든 리소스 그룹으로 이동 합니다.

2. 클릭 합니다 **Application Insights** 인스턴스.

3. **Application Insights** 섹션에서 이동할 **조사\\검색** 및 데이터를 검색 합니다.

### <a name="security"></a>보안

Azure Cosmos DB는 보안된 연결 및 공유 액세스 서명을 통해 C\# SDK Microsoft에서 제공 합니다. 다른 외부와 접한 노출 영역이 없습니다 있습니다. 보안에 자세히 알아보려면 [모범 사례] [ db-practices] Azure Cosmos DB에 대 한 합니다.

## <a name="pricing"></a>가격

예상된 일일 비용을 계속 사용할 수 있는이 배포는 대략 \$20 시스템을 통해 이동 하는 데이터가 없는 미국.

Azure Cosmos DB는 강력 하지만 가장 큰 요소를 발생 시킵니다 [비용] [ db-cost] 이 배포 합니다. 제공 하는 Azure Functions 코드를 리팩터링하여 다른 저장소 솔루션을 사용할 수 있습니다.

에 따라 달라 집니다 Azure Functions에 대 한 가격 책정 합니다 [계획] [ function-plan] 에서 실행 됩니다.

## <a name="deploy-the-scenario"></a>시나리오 배포

> [!NOTE]
> 기존 Azure 계정이 있어야 합니다. Azure 구독이 없는 경우 시작하기 전에 [체험 계정][free]을 만듭니다.

이 시나리오에 대 한 모든 코드는 합니다 [GitHub] [ github] 리포지토리. 이 리포지토리는이 데모에 대 한 파이프라인을 공급 하는 생성기 응용 프로그램을 빌드하는 데 사용 되는 소스 코드를 포함 합니다.

[architecture]: ./media/mass-ingestion-newsfeeds-architecture.png
[aai]: /azure/azure-monitor/app/app-insights-overview
[aas]: https://azure.microsoft.com/try/app-service/
[acs]: https://azure.microsoft.com/services/cognitive-services/directory/
[collection]: /rest/api/cosmos-db/collections
[compare]: /azure/azure-functions/functions-compare-logic-apps-ms-flow-webjobs#compare-azure-functions-and-azure-logic-apps
[cosmos-db]: /azure/cosmos-db/introduction
[db-cost]: https://azure.microsoft.com/pricing/details/cosmos-db/
[db-practices]: /azure/cosmos-db/database-security
[db-collection]: /azure/cosmos-db/databases-containers-items
[english]: https://www.nasa.gov/rss/dyn/breaking_news.rss
[face]: /azure/cognitive-services/face/overview
[free]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F
[functions]: /azure/azure-functions/functions-overview
[function-plan]: /azure/azure-functions/functions-scale
[german]: http://www.bamf.de/SiteGlobals/Functions/RSS/DE/Feed/RSSNewsfeed_Meldungen
[github]: https://github.com/Azure/cognitive-services
[keys]: /azure/cosmos-db/partition-data
[language]: /azure/cognitive-services/translator/reference/v3-0-languages
[logic-app]: /azure/logic-apps/logic-apps-overview
[queues-topics]: /azure/service-bus-messaging/service-bus-queues-topics-subscriptions
[partial]: https://feedback.azure.com/forums/263030-azure-cosmos-db/suggestions/6693091-be-able-to-do-partial-updates-on-document
[plan]: /azure/azure-functions/functions-scale
[plan-aas]: /azure/azure-functions/functions-scale#app-service-plan
[plan-c]: /azure/azure-functions/functions-scale#consumption-plan
[portal]: http://portal.azure.com
[redlock]: https://redis.io/topics/distlock
[russian]: http://government.ru/all/rss/
[service-bus]: /azure/service-bus-messaging/
[storage]: /azure/storage/common/storage-account-overview 
[throughput]: /azure/cosmos-db/scaling-throughput
[topics]: /azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions
[text-analytics]: /azure/cognitive-services/text-analytics/
[translate-text]: /azure/cognitive-services/translator/translator-info-overview
[vision]: /azure/cognitive-services/computer-vision/home
