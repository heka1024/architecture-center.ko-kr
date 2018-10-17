---
title: 건축 산업에서 IoT 및 데이터 분석
description: IoT 장치 및 데이터 분석을 사용하여 건축 프로젝트의 포괄적인 관리 및 작업을 제공합니다.
author: alexbuckgit
ms.date: 08/29/2018
ms.openlocfilehash: 7ab0de50b0eba1ab420e450f3408fe5dc45f04ac
ms.sourcegitcommit: b2a4eb132857afa70201e28d662f18458865a48e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48818499"
---
# <a name="iot-and-data-analytics-in-the-construction-industry"></a>건축 산업에서 IoT 및 데이터 분석

이 예제 시나리오는 여러 IoT 장치의 데이터를 포괄적인 데이터 분석 아키텍처에 통합하여 의사 결정을 개선하고 자동화하는 솔루션을 빌드하는 조직과 관련이 있습니다. 잠재적인 응용 프로그램에는 여러 IoT 기반 데이터 입력에서 대량의 데이터를 수집하는 건설, 채굴, 제조 또는 기타 산업 솔루션이 포함됩니다.

이 시나리오에서는 한 건설 장비 제조업체가 IoT 및 GPS 기술을 사용하여 원격 분석 데이터를 전송하는 차량, 계량기 및 드론을 빌드합니다. 이 회사는 운영 상태 및 장비 상태를 보다 정확하게 모니터링할 수 있도록 데이터 아키텍처를 현대화하려고 합니다. 온-프레미스 인프라를 사용하여 이 회사의 레거시 솔루션을 대체하려면 시간과 노력이 많이 필요하며, 예상되는 데이터 볼륨을 처리하기에 충분한 규모로 확장할 수 없습니다.

이 회사는 클라우드 기반 "스마트 건설" 솔루션을 빌드하려고 합니다. 건설 현장에 대한 포괄적인 데이터를 수집하고 다양한 현장 요소의 운영 및 유지 관리를 자동화해야 합니다. 이 회사의 목표는 다음과 같습니다.

* 모든 건설 현장 장비와 데이터를 통합하고 분석하여 장비 가동 중지 시간을 최소화하고 도난 사고를 줄입니다.
* 건설 장비를 원격 및 자동으로 제어하여 노동력 부족을 완화하고, 궁극적으로는 더 적은 수의 미숙련 작업자로 공사를 완료합니다.
* 지원 인프라의 운영 비용과 노동력 요구 사항을 최소화하는 한편, 생산성과 안전을 강화합니다.
* 원격 분석 데이터의 증가를 지원하도록 인프라를 쉽게 확장합니다.
* 시스템 가용성을 손상하지 않고 국내에서 리소스를 프로비전하여 모든 관련 법률 요구 사항을 준수합니다.
* 오픈 소스 소프트웨어를 사용하여 작업자의 현재 기술에 투자한 비용의 효과를 극대화합니다.

IoT Hub 및 HDInsight 같은 관리되는 Azure 서비스를 사용하여 고객이 더 적은 운영 비용으로 포괄적인 솔루션을 신속하게 빌드 및 배포합니다. 추가 데이터 분석 요구 사항이 있는 경우 제공되는 [Azure의 완전히 관리되는 데이터 분석 서비스][product-category] 목록을 검토해야 합니다.

## <a name="relevant-use-cases"></a>관련 사용 사례

이 솔루션을 사용하는 데 적합한 사용 사례는 다음과 같습니다.

* 건설, 채굴 또는 장비 제조 시나리오
* 저장 및 분석을 위한 대규모 장치 데이터 컬렉션
* 대규모 데이터 집합의 수집 및 분석

## <a name="architecture"></a>아키텍처

![건설 산업의 IoT 및 데이터 분석을 위한 아키텍처][architecture]

솔루션을 통한 데이터 흐름은 다음과 같습니다.

1. 건설 장비는 주기적으로 센서 데이터를 수집한 후 Azure 가상 머신 클러스터에 호스트되는 부하 분산 웹 서비스로 건설 결과 데이터를 전송합니다.
2. 사용자 지정 웹 서비스는 건설 결과 데이터를 수집하여 Azure 가상 머신에서 실행되는 Apache Cassandra 클러스터에 저장합니다.
3. IoT 센서는 다양한 건설 장비에 대한 또 다른 데이터 집합을 수집하여 IoT Hub로 전송합니다.
4. 수집된 원시 데이터는 IoT Hub에서 Azure Blob 저장소로 직접 전송되어 그 즉시 검토 및 분석에 사용할 수 있습니다.
5. IoT Hub를 통해 수집된 데이터는 Azure Stream Analytics 작업에서 거의 실시간으로 처리된 후 Azure SQL 데이터베이스에 저장됩니다.
6. 분석가 및 최종 사용자는 스마트 건설 클라우드 웹 응용 프로그램을 사용하여 센서 데이터 및 이미지를 살펴보고 분석할 수 있습니다. 
7. 일괄 처리 작업은 웹 응용 프로그램 사용자의 요청이 있을 시 시작됩니다. 일괄 처리 작업은 HDInsight 기반의 Apache Spark에서 실행되어 Cassandra 클러스터에 저장된 새 데이터를 분석합니다. 

### <a name="components"></a>구성 요소

* [IoT Hub](/azure/iot-hub/about-iot-hub)는 장치별 ID를 사용하여 클라우드 플랫폼과 건설 장비 및 기타 현장 요소 간에 안전한 양방향 통신이 가능하도록 중앙 메시지 허브 역할을 합니다. IoT Hub는 각 장치에 대한 데이터를 신속하게 데이터 분석 파이프라인으로 수집할 수 있습니다. 
* [Azure Stream Analytics](/azure/stream-analytics/stream-analytics-introduction)는 장치 및 다른 데이터 원본에서 스트림하는 대량의 데이터를 분석할 수 있는 이벤트 처리 엔진입니다. 또한 데이터 스트림에서 정보를 추출하여 패턴과 관계를 식별할 수 있습니다. 이 시나리오에서는 Stream Analytics가 IoT 장치에서 데이터를 수집 및 분석하고 Azure SQL Database에 결과를 저장합니다. 
* [Azure SQL Database](/azure/sql-database/sql-database-technical-overview)는 IoT 장치 및 계량기 데이터를 분석한 결과를 포함하며, 분석가 및 사용자는 Azure 기반 웹 응용 프로그램을 통해 결과를 볼 수 있습니다. 
* [Blob 저장소](/azure/storage/blobs/storage-blobs-introduction)는 IoT 허브 장치에서 수집된 이미지 데이터를 저장합니다. 이미지 데이터는 웹 응용 프로그램을 통해 볼 수 있습니다.
* [Traffic Manager](/azure/traffic-manager/traffic-manager-overview)는 다른 Azure 지역의 서비스 엔드포인트에 대한 사용자 트래픽 분산을 제어합니다.
* [부하 분산 장치](/azure/load-balancer/load-balancer-overview)는 건설 장비 장치에서 제출된 데이터를 VM 기반 웹 서비스에 분산하여 고가용성을 제공합니다.
* [Azure Virtual Machines](/azure/virtual-machines)는 건설 결과 데이터를 수신하여 Apache Cassandra 데이터베이스로 수집하는 웹 서비스를 호스트합니다.
* [Apache Cassandra](https://cassandra.apache.org)는 나중에 Apache Spark로 처리할 수 있도록 건설 데이터를 저장하는 데 사용되는 분산 NoSQL 데이터베이스입니다.
* [웹앱](/azure/app-service/app-service-web-overview)은 최종 사용자 웹 응용 프로그램을 호스트하며, 원본 데이터와 이미지를 쿼리하고 살펴보는 데 사용할 수 있습니다. 사용자는 응용 프로그램을 통해 Apache Spark에서 일괄 처리 작업을 시작할 수도 있습니다.
* [HDInsight 기반의 Apache Spark](/azure/hdinsight/spark/apache-spark-overview)는 빅 데이터 분석 응용 프로그램의 성능을 향상하는 메모리 내 처리를 지원합니다. 이 시나리오에서 Spark는 Apache Cassandra에 저장된 데이터에 대해 복잡한 알고리즘을 실행하는 데 사용됩니다.


### <a name="alternatives"></a>대안

* [Cosmos DB](/azure/cosmos-db/introduction)는 NoSQL 데이터베이스를 대체하는 기술입니다. Cosmos DB는 다양한 고객 요구 사항을 충족하도록 [여러 일관성 수준이 잘 정의된](/azure/cosmos-db/consistency-levels) [글로벌 규모의 다중 마스터 지원](/azure/cosmos-db/multi-region-writers)을 제공합니다. [Cassandra API](/azure/cosmos-db/cassandra-introduction)도 지원합니다. 
* [Azure Databricks](/azure/azure-databricks/what-is-azure-databricks)는 Azure에 최적화된 Apache Spark 기반 분석 플랫폼입니다. Azure와 통합되어 원클릭 설정, 능률적인 워크플로, 대화형 공동 작업 영역을 제공합니다.
* [Data Lake Storage](/azure/storage/data-lake-storage)는 Blob 저장소를 대체합니다. 이 시나리오에서는 대상 지역에서 Data Lake Storage를 사용할 수 없습니다.
* [웹앱](/azure/app-service)을 사용하여 건설 결과 데이터를 수집하는 웹 서비스를 호스트할 수도 있습니다.
* 다양한 기술 옵션을 통해 실시간 메시지 수집, 데이터 저장, 스트림 처리, 분석 데이터 저장, 분석 및 보고를 수행할 수 있습니다. 이러한 옵션, 기능 및 주요 선택 조건에 대한 개요는 [Azure 데이터 아키텍처 가이드](/azure/architecture/data-guide)의 [빅 데이터 아키텍처: 실시간 처리](/azure/architecture/data-guide/technology-choices/real-time-ingestion)를 참조하세요.

## <a name="considerations"></a>고려 사항

Azure 지역의 광범위한 가용성은 이 시나리오의 중요한 요소입니다. 한 국가에 두 개 이상의 지역이 있으면 재해 복구를 제공하는 동시에 계약 의무 및 법률 집행 요구 사항을 준수할 수 있습니다. Azure의 지역 간 고속 통신도 이 시나리오에서 중요한 요소입니다.

Azure의 오픈 소스 기술 지원 덕분에 고객이 기존 직원 기술을 활용할 수 있었습니다. 또한 고객은 온-프레미스 솔루션에 비해 저렴한 비용과 운영 워크로드를 투입하여 신기술 도입 속도를 높일 수 있습니다. 

## <a name="pricing"></a>가격

다음 고려 사항은 이 솔루션의 비용 중 상당 부분을 차지합니다.

* Azure 가상 머신 비용은 추가 인스턴스가 프로비전될 때마다 선형으로 증가합니다. 할당 취소된 가상 머신은 저장소 비용만 발생시키고 계산 비용은 발생시키지 않습니다. 이처럼 할당 취소된 머신은 수요가 증가할 때 다시 할당할 수 있습니다.
* [IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub) 비용은 프로비전된 IoT 단위 수와 선택한 서비스 계층에 따라 달라지며, 이에 따라 단위당 허용되는 일별 메시지 수가 결정됩니다. 
* [Stream Analytics](https://azure.microsoft.com/pricing/details/stream-analytics)는 데이터를 서비스로 처리하는 데 필요한 스트리밍 단위의 개수로 가격이 결정됩니다.

## <a name="related-resources"></a>관련 리소스

비슷한 아키텍처 구현을 보려면 [Komatsu 고객 사례][customer-story]를 읽어보세요.

빅 데이터 아키텍처에 대한 지침은 [Azure 데이터 아키텍처 가이드](/azure/architecture/data-guide)에서 확인할 수 있습니다.

<!-- links -->
[product-category]: https://azure.microsoft.com/product-categories/analytics/
[customer-site]: https://home.komatsu/en/
[customer-story]: https://customers.microsoft.com/story/komatsu-manufacturing-azure-iot-hub-japan
[architecture]: ./media/architecture-big-data-with-iot.png
