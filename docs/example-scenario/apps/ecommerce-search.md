---
title: 전자상거래에 대한 지능형 제품 검색 엔진
description: 전자상거래 응용 프로그램에서 세계적 수준의 검색 환경을 제공합니다.
author: jelledruyts
ms.date: 09/14/2018
ms.custom: fasttrack
ms.openlocfilehash: 5eabdb94b9345e73b21526681e0dbd6ae859d7be
ms.sourcegitcommit: a0e8d11543751d681953717f6e78173e597ae207
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/06/2018
ms.locfileid: "53004901"
---
# <a name="intelligent-product-search-engine-for-e-commerce"></a>전자상거래에 대한 지능형 제품 검색 엔진

이 예제 시나리오에서는 전용 검색 서비스를 사용하여 전자상거래 고객의 검색 결과 관련성을 대폭 향상하는 방법을 보여줍니다.

검색은 고객이 제품을 검색하고 궁극적으로 구매하는 주요 메커니즘이며, 실시간에 가까운 결과, 언어 분석, 지리적 위치, 필터링, 패싯, 자동 완성, 적중 항목 강조 표시 등을 제공하여 검색 쿼리의 _의도_와 관련된 검색 결과 및 통합형 검색 환경을 검색 자이언트의 환경과 일치시키는 것이 중요합니다.

SQL Server 또는 Azure SQL Database처럼 제품 데이터가 관계형 데이터베이스에 저장되는 일반적인 전자상거래 웹 응용 프로그램을 떠올려 보세요. `LIKE` 쿼리 또는 [전체 텍스트 검색][docs-sql-fts] 기능을 사용하여 데이터베이스 내에서 검색 쿼리를 처리하는 경우가 자주 있습니다. [Azure Search][docs-search]를 대신 사용하면 운영 데이터베이스가 쿼리 처리에서 해방되고, 고객에게 가능한 최상의 검색 환경을 제공하는 구현하기 까다로운 기능을 쉽게 활용할 수 있습니다. 또한 Azure Search가 PaaS(Platform as a Service) 구성 요소이므로 인프라 관리에 대해 걱정하거나 검색 전문가가 될 필요가 없습니다.

## <a name="relevant-use-cases"></a>관련 사용 사례

관련된 다른 사용 사례는 다음과 같습니다.

* 사용자의 물리적 위치와 가까운 부동산 목록 또는 매장을 찾습니다.
* _최신_ 정보에 더 높은 우선 순위를 부여하여 뉴스 사이트에서 문서를 검색하거나 스포츠 결과를 찾습니다.
* 대형 리포지토리를 검색하여 정책 입안자 및 공증인 같은 _문서 중심_ 조직을 찾습니다.

결국 어떤 형태로든 검색 기능이 있는 _모든_ 애플리케이션은 전용 검색 서비스의 이점을 얻을 수 있습니다.

## <a name="architecture"></a>아키텍처

![전자상거래에 대한 지능형 제품 검색 엔진과 관련된 Azure 구성 요소의 아키텍처 개요][architecture]

이 시나리오에서는 고객이 제품 카탈로그를 검색할 수 있는 전자상거래 솔루션을 다룹니다.
1. 고객은 각종 장치로 **전자상거래 웹 응용 프로그램**을 탐색합니다.
2. 제품 카탈로그는 트랜잭션 처리를 위해 **Azure SQL Database**에 유지됩니다.
3. Azure Search는 **검색 인덱서**를 사용하여 통합 변경 내용 추적을 통해 자동으로 검색 인덱스를 최신 상태로 유지합니다.
4. 고객의 검색 쿼리는 쿼리를 처리하여 가장 관련성이 높은 결과를 반환하는 **Azure Search** 서비스로 오프로드됩니다.
5. 웹 기반 검색 환경의 대안으로, 고객은 소셜 미디어에서 또는 디지털 도우미에서 바로 **대화형 봇**을 사용하여 제품을 검색하고 검색 쿼리 및 결과를 조금씩 구체화할 수 있습니다.
6. 필요에 따라, **Cognitive Search** 기능을 사용하여 인공 지능을 적용하면 보다 스마트한 처리가 가능합니다.

### <a name="components"></a>구성 요소

* [App Services - Web Apps][docs-webapps]는 인프라를 관리할 필요 없이 자동 크기 조정 및 고가용성을 허용하는 웹 애플리케이션을 호스팅합니다.
* [SQL Database][docs-sql-database]는 관계형 데이터, JSON, 공간 및 XML과 같은 구조를 지원하는 Microsoft Azure의 범용 관계형 데이터베이스 관리 서비스입니다.
* [Azure Search][docs-search]는 웹, 모바일 및 엔터프라이즈 응용 프로그램에서 이질적인 비공개 콘텐츠에 대한 풍부한 검색 환경을 제공하는 검색 기반 클라우드 솔루션입니다.
* [Bot Service][docs-botservice]는 지능형 봇을 빌드, 테스트, 배포 및 관리할 수 있는 도구를 제공합니다.
* [Cognitive Services][docs-cognitive]를 사용하면 지능형 알고리즘을 사용하여 자연스러운 의사 소통 방법을 통해 사용자의 요구 사항을 보고, 듣고, 말하고, 이해하고 해석할 수 있습니다.

### <a name="alternatives"></a>대안

* 예를 들어 SQL Server 전체 텍스트 검색을 통해 **데이터베이스 내 검색** 기능을 사용할 수 있지만, 트랜잭션 저장소에서도 쿼리를 처리하므로(처리 성능 요구 사항 증가) 데이터베이스 내에서 검색 기능이 좀 더 제한됩니다.
* Azure Virtual Machines에 (Azure Search가 빌드되는) 오픈 소스 [Apache Lucene][apache-lucene]를 호스트할 수 있지만, 다시 IaaS(Infrastructure-as-a-Service)를 관리해야 하므로 Azure Search가 Lucene을 기반으로 제공하는 여러 기능의 이점을 누릴 수 없습니다.
* Azure Marketplace에서 타사 공급업체의 검색 제품인 [Elastic Search][elastic-marketplace]를 대안으로 배포하는 방안도 고려해 볼 수 있지만, 이 예에서는 IaaS 워크로드를 실행합니다.

데이터 계층에 대한 다른 옵션은 다음과 같습니다.

* [Cosmos DB](/azure/cosmos-db/introduction) - 글로벌하게 분산된 Microsoft의 다중 모델 데이터베이스입니다. Costmos DB는 Mongo DB, Cassandra, Graph 데이터 또는 간단한 테이블 저장소와 같은 다른 데이터 모델을 실행하는 플랫폼을 제공합니다. Azure Search는 Cosmos DB에서 데이터를 직접 인덱싱하는 기능도 지원합니다.

## <a name="considerations"></a>고려 사항

### <a name="scalability"></a>확장성

Azure Search의 서비스의 [가격 책정 계층][search-tier]은 사용 가능한 기능을 결정하지 않지만, 사용자가 얻는 최대 저장소 및 프로비전 가능한 파티션 및 복제본 수를 정의하므로 [용량 계획][search-capacity]에 주로 사용됩니다. **파티션**을 사용하면 더 많은 문서를 인덱싱하고 더 높은 쓰기 처리량을 얻을 수 있는 반면, **복제본**은 더 많은 QPS(Queries-Per-Second) 및 고가용성을 제공합니다.

파티션 및 복제본 수를 동적으로 변경할 수 있지만 가격 책정 계층을 변경하는 것은 불가능하기 때문에 대상 워크로드에 적합한 계층을 신중하게 고민해야 합니다. 어떻게든 계층을 변경해야 하는 경우 새 서비스를 나란히 프로비전하고 거기에 인덱스를 다시 로드하면 그 시점부터 새 서비스에서 응용 프로그램을 가리킬 수 있습니다.

### <a name="availability"></a>가용성

Azure Search는 복제본이 2개 이상 있으면 _읽기_(즉, 쿼리)에 대해, 복제본이 3개 이상 있으면 _업데이트_에 대해 [99.9% 가용성 SLA][search-sla]를 제공합니다. 그러므로 고객이 안정적으로 _검색_할 수 있게 하려면 복제본을 2개 이상 프로비전해야 하고, 실제 _인덱스 변경 내용_을 고가용성 작업으로 간주해야 하는 경우에는 3개 이상 프로비전해야 합니다.

가동 중지 시간 없이 인덱스를 크게 변경해야 하는 경우(예: 데이터 형식 변경, 필드 삭제 또는 이름 바꾸기) 인덱스를 다시 빌드해야 합니다. 서비스 계층 변경과 마찬가지로, 새 인덱스를 만들고 데이터를 다시 채운 후 새 인덱스를 가리키도록 응용 프로그램을 업데이트해야 합니다.

### <a name="security"></a>보안

Azure Search는 여러 [보안 및 데이터 보호 표준][search-security]을 준수하므로 대부분의 산업에 사용할 수 있습니다.

서비스 액세스를 보호하기 위해 Azure Search는 두 가지 유형의 키를 사용합니다. **관리자 키**는 서비스에 대해 _모든_ 작업을 수행할 수 있고, **쿼리 키**는 쿼리 같은 읽기 전용 작업에만 사용할 수 있습니다. 일반적으로 검색을 수행하는 응용 프로그램은 인덱스를 업데이트 하지 않으므로 관리 키가 아닌 쿼리 키를 사용하여 구성해야 하며, 특히 웹 브라우저에서 실행되는 스크립트처럼 검색이 최종 사용자 장치에서 수행되는 경우에 더욱 그렇습니다.

### <a name="search-relevance"></a>검색 관련성

전자상거래 응용 프로그램의 완성도는 검색 결과와 고객의 관련성에 달렸습니다. 사용자 연구 결과에 따라 최적의 결과를 제공하도록 신중하게 검색 서비스를 튜닝하거나, [검색 트래픽 분석][search-analysis] 같은 기본 제공 기능을 사용하여 고객의 검색 패턴을 이해하면 데이터를 기반으로 의사를 결정할 수 있습니다.

검색 서비스를 튜닝하는 일반적인 방법은 다음과 같습니다.

* 검색 결과의 관련성에 영향을 주는 [점수 매기기 프로필][search-scoring]을 사용합니다. 예: 쿼리와 일치하는 필드, 데이터의 현재 상태, 사용자와의 물리적 거리 등.
* 고급 NLP(고급 자연어 처리) 스택을 사용하여 쿼리를 보다 정확하게 해석하는 [Microsoft 언어 분석기][search-languages]를 사용합니다.
* 특히 제품의 제조업체 및 모델 같은 비언어 기반 정보를 기반으로 검색하려는 경우 올바른 제품이 검색되도록 [사용자 지정 분석기][search-analyzers]를 사용합니다.

## <a name="deploy-this-scenario"></a>시나리오 배포

이 시나리오의 보다 완전한 전자상거래 버전을 배포하려면 간단한 티켓 구매 응용 프로그램을 실행하는 .NET 샘플 응용 프로그램을 제공하는 이 [단계별 자습서][end-to-end-walkthrough]를 따르면 됩니다. 이 자습서는 Azure Search도 포함하고 있으며 앞에서 설명한 여러 기능을 사용합니다. 또한 Azure 리소스 대부분의 배포를 자동화하는 Resource Manager 템플릿도 있습니다.

## <a name="pricing"></a>가격

이 시나리오를 실행하는 데 들어가는 비용을 알아볼 수 있도록, 위에서 언급한 모든 서비스가 비용 계산기에 미리 구성되어 있습니다. 특정 사용 사례의 가격 변동을 확인하려면 예상 사용량에 맞게 변수를 적절하게 변경합니다.

가져오는 데 필요한 트래픽 양을 기준으로 다음 세 가지 샘플 비용 프로필을 제공했습니다.

* [소형][small-pricing]: 이 프로필에서는 웹 사이트를 호스트할 단일 `Standard S1` 웹앱, Azure Bot 서비스의 체험 계층, 단일 `Basic` Azure Search 서비스 및 `Standard S2` SQL Database를 사용합니다.
* [중형][medium-pricing]: 이 프로필에서는 웹앱을 `Standard S3` 계층의 인스턴스 2개로 확장하고, 검색 서비스를 `Standard S1` 계층으로 업그레이드하고, `Standard S6` SQL Database를 사용합니다.
* [대형][large-pricing]: 가장 큰 프로필에서는 `Premium P2V2` 웹앱의 인스턴스 4개를 사용하고, Azure Bot 서비스를 `Standard S1` 계층으로 업그레이드하고(프리미엄 채널에서 1.000.000개 메시지), `Standard S3` Azure Search 서비스 단위 2개 및 `Premium P6` SQL Database를 사용합니다.

## <a name="related-resources"></a>관련 리소스

Azure Search에 대한 자세한 내용은 [설명서 센터][docs-search]를 방문하여 [샘플][search-samples]을 살펴보거나 실제로 작동하는 완전히 구현된 [데모 사이트][search-demo]를 참조하세요.

<!-- links -->
[architecture]: ./media/architecture-ecommerce-search.png
[docs-sql-fts]: /sql/relational-databases/search/query-with-full-text-search
[docs-search]: /azure/search/search-what-is-azure-search
[docs-sql-database]: /azure/sql-database/sql-database-technical-overview
[docs-webapps]: /azure/app-service/app-service-web-overview
[docs-botservice]: /azure/bot-service/
[docs-cognitive]: /azure/cognitive-services/
[apache-lucene]: https://lucene.apache.org/
[elastic-marketplace]: https://azuremarketplace.microsoft.com/marketplace/apps/elastic.elasticsearch
[end-to-end-walkthrough]: https://github.com/Azure/fta-customerfacingapps/tree/master/ecommerce/articles
[search-sla]: https://go.microsoft.com/fwlink/?LinkId=716855
[search-tier]: /azure/search/search-sku-tier
[search-capacity]: /azure/search/search-capacity-planning
[search-security]: /azure/search/search-security-overview
[search-analysis]: /azure/search/search-traffic-analytics
[search-languages]: /rest/api/searchservice/language-support
[search-analyzers]: /rest/api/searchservice/custom-analyzers-in-azure-search
[search-scoring]: /rest/api/searchservice/add-scoring-profiles-to-a-search-index
[search-samples]: https://azure.microsoft.com/resources/samples/?service=search&sort=0
[search-demo]: https://azjobsdemo.azurewebsites.net/
[small-pricing]: https://azure.com/e/db2672a55b6b4d768ef0060a8d9759bd
[medium-pricing]: https://azure.com/e/a5ad0706c9e74add811e83ef83766a1c
[large-pricing]: https://azure.com/e/57f95a898daa487795bd305599973ee6
