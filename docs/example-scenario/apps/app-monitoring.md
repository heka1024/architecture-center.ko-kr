---
title: Azure에서 웹 응용 프로그램 모니터링
description: Azure App Service에 호스트되는 웹 응용 프로그램을 모니터링합니다.
author: adamboeglin
ms.date: 09/12/2018
ms.openlocfilehash: b1beb5cf5e29ab1ceb760bf95eab85d819b69342
ms.sourcegitcommit: 62945777e519d650159f0f963a2489b6bb6ce094
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2018
ms.locfileid: "48876854"
---
# <a name="web-application-monitoring-on-azure"></a>Azure에서 웹 응용 프로그램 모니터링

Azure의 PaaS(Platform as a Service) 제품은 고객 대신 계산 리소스를 관리하고, 어떤 면에서는 배포 모니터링 방법을 변경합니다. Azure는 여러 모니터링 서비스를 포함하고 있으며, 각 서비스는 특정 역할을 수행합니다. 이러한 서비스가 한 데 모여 응용 프로그램 및 응용 프로그램에서 사용하는 Azure 리소스로부터 원격 분석 데이터를 수집, 분석하고 그에 따라 조치를 취하는 포괄적인 솔루션을 제공합니다.

이 시나리오는 사용 가능한 모니터링 서비스를 해결하며 여러 데이터 원본에 사용될 데이터 흐름 모델에 대해 설명합니다. 모니터링과 관련하여, 여러 도구와 서비스가 Azure 배포에 사용됩니다. 이 시나리오에서는 즉시 사용 가능한 서비스를 선택하겠습니다. 다른 이유는 없고, 쉽게 사용할 수 있기 때문입니다. 다른 모니터링 옵션은 이 문서의 뒷부분에서 설명합니다.

## <a name="relevant-use-cases"></a>관련 사용 사례

이 시나리오에 적합한 사용 사례는 다음과 같습니다.

- 원격 분석 데이터를 모니터링하는 웹 응용 프로그램 계측.
- Azure에 배포된 응용 프로그램에 대한 프런트 엔드 및 백 엔드 원격 분석 수집.
- Azure의 서비스와 연결된 메트릭 및 할당량 모니터링.

## <a name="architecture"></a>아키텍처

![앱 모니터 아키텍처 다이어그램][architecture]

이 시나리오에서는 관리 Azure 환경을 사용하여 응용 프로그램 및 데이터 계층을 호스트합니다. 시나리오를 통한 데이터 흐름은 다음과 같습니다.

1. 사용자가 응용 프로그램과 상호 작용합니다.
2. 브라우저 및 앱 서비스가 원격 분석 데이터를 내보냅니다.
3. Application Insights에서 응용 프로그램 상태, 성능 및 사용량 데이터를 수집하고 분석 합니다.
4. 개발자와 관리자가 상태, 성능 및 사용량 정보를 검토할 수 있습니다.
5. Azure SQL Database가 원격 분석 데이터를 내보냅니다.
6. Azure Monitor가 인프라 메트릭 및 할당량을 수집하여 분석합니다.
7. Log Analytics가 로그 및 메트릭을 수집하고 분석합니다.
8. 개발자와 관리자가 상태, 성능 및 사용량 정보를 검토할 수 있습니다.

### <a name="components"></a>구성 요소

- [Azure App Service](/azure/app-service/)는 관리 가상 머신에 앱을 빌드하고 호스트하는 PaaS 서비스입니다. 앱이 실행되는 기본 계산 인프라가 자동으로 관리됩니다. App Service는 리소스 사용 할당량 및 앱 메트릭 모니터링, 진단 정보 로깅, 메트릭 기반의 경고를 제공합니다. 뿐만 아니라 Application Insights를 사용하여 다른 지역에서 응용 프로그램을 테스트하는 [가용성 테스트][availability-tests]를 만들 수 있습니다.
- [Application Insights][application-insights]는 개발자를 위한 확장 가능한 APM(응용 프로그램 성능 관리) 서비스이며 여러 플랫폼을 지원합니다. 응용 프로그램을 모니터링하고, 성능 저하 및 오류 같은 응용 프로그램 이상을 감지하고, Azure Portal로 원격 분석 데이터를 보냅니다. Application Insights를 로깅, 분산된 추적 및 사용자 지정 응용 프로그램 메트릭에도 사용할 수 있습니다.
- [Azure Monitor][azure-monitor]는 대부분의 Azure 서비스에 대한 기본 수준의 인프라 [메트릭 및 로그][metrics]를 제공합니다. Azure Portal에서 차트 작성, REST API를 통한 액세스, PowerShell 또는 CLI를 사용한 쿼리 등 여러 가지 방법으로 메트릭과 상호 작용할 수 있습니다. Azure Monitor는 [Log Analytics 및 기타 서비스]에 데이터를 직접 제공할 수도 있으며, 서비스에서 데이터를 쿼리하고 온-프레미스 또는 클라우드의 다른 소스에서 수집된 데이터와 결합할 수 있습니다.
- [Log Analytics][log-analytics]는 Application Insights에서 수집한 사용량 및 성능 데이터와 앱을 지원하는 Azure 리소스의 구성 및 성능 데이터 간에 상관 관계를 지정할 수 있습니다. 이 시나리오에서는 [Azure Log Analytics 에이전트][Azure Log Analytics agent]를 사용하여 SQL Server 감사 로그를 Log Analytics로 푸시합니다. Azure Portal의 Log Analytics 블레이드에서 데이터를 쿼리하고 볼 수 있습니다.

## <a name="considerations"></a>고려 사항

가장 좋은 방법은 개발 시 [Application Insights SDK][Application Insights SDKs]를 사용하여 코드에 Application Insights를 추가하고 응용 프로그램별로 사용자 지정하는 것입니다. 이러한 오픈 소스 SDK는 대부분의 응용 프로그램 프레임워크에서 사용할 수 있습니다. 수집한 데이터를 강화하고 제어하려면 테스트 및 프로덕션 배포에 사용할 SDK를 개발 프로세스에 통합합니다. 주 요구 사항은 앱이 인터넷 주소를 사용하여 호스트된 Application Insights 수집 엔드포인트를 직접적으로 또는 간접적으로 살펴볼 수 있게 만들어주는 것입니다. 그 후 원격 분석 데이터를 추가하거나 기존 원격 분석 컬렉션을 보강할 수 있습니다.

쉽게 시작하는 또 다른 방법은 런타임 모니터링입니다. 수집된 원격 분석 데이터는 구성 파일을 통해 제어해야 합니다. 예를 들어 [Application Insights 상태 모니터][Application Insights Status Monitor] 같은 도구가 올바른 폴더에 SDK를 배포하고 모니터링을 시작하는 올바른 구성을 추가할 수 있게 해주는 런타임 메서드를 포함할 수 있습니다.

Application Insights와 마찬가지로, Log Analytics는 [여러 원본의 데이터를 분석][analyzing data across sources]하고, 복합 쿼리를 만들고, 지정된 조건에 대한 [사전 경고를 보내는][sending proactive alerts] 도구를 제공합니다. [Azure Portal][the Azure portal]에서도 원격 분석 데이터를 볼 수 있습니다. Log Analytics는 [Azure Monitor][Azure Monitor] 같은 기존 모니터링 서비스에 가치를 추가하며 온-프레미스 환경도 모니터링할 수 있습니다.

Application Insights와 Log Analytics [Azure Log Analytics 쿼리 언어][Azure Log Analytics Query Language]를 사용합니다. [리소스 간 쿼리](https://azure.microsoft.com/blog/query-across-resources)를 사용하여 Application Insights 및 Log Analytics에서 수집한 원격 분석 데이터를 단일 쿼리로 분석할 수도 있습니다.

Azure Monitor, Application Insights, Log Analytics는 모두 [경고](/azure/monitoring-and-diagnostics/monitoring-overview-alerts)를 보냅니다. 예를 들어 Azure Monitor는 CPU 사용률 같은 플랫폼 수준 메트릭에 대한 경고를 보내고, Application Insights는 서버 응답 시간 같은 응용 프로그램 수준 메트릭에 대해 경고합니다. Azure Monitor는 Azure 활동 로그의 새 이벤트에 대해 경고하고, Log Analytics는 사용하도록 구성된 서비스의 메트릭 또는 이벤트 데이터에 대한 경고를 보냅니다. [Azure Monitor의 통합 경고](/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts)는 다른 분류를 사용하는 Azure의 새로운 통합 경고 환경입니다.

### <a name="alternatives"></a>대안

이 문서에서는 편리하게 사용할 수 있는 모니터링 옵션과 인기 있는 기능에 대해 설명하지만, 자체 로깅 메커니즘을 만드는 옵션을 포함하여 다양한 선택지가 있습니다. 모범 사례는 솔루션에서 계층을 빌드할 때 모니터링 서비스를 추가하는 것입니다. 다음은 몇 가지 가능한 확장 및 대안입니다.

- [Azure Monitor Data Source For Grafana][Azure Monitor Data Source For Grafana]를 사용하여 Azure Monitor 및 Application Insights 메트릭을 Grafana에 통합합니다.
- [Data Dog][data-dog]은 Azure Monitor용 커넥터를 제공합니다.
- [Azure Automation][Azure Automation]을 사용하여 모니터링 기능을 자동화합니다.
- [ITSM 솔루션][ITSM solutions]과의 통신을 추가합니다.
- [관리 솔루션][management solution]을 사용하여 Log Analytics를 확장합니다.

### <a name="scalability-and-availability"></a>확장성 및 가용성

이 시나리오는 큰 부분을 모니터링하기 위한 PaaS 솔루션에 중점을 둡니다. 왜냐하면 이러한 솔루션은 간편하게 고객 대신 가용성 및 확장성을 처리하고 SLA(서비스 수준 계약)를 통해 지원되기 때문입니다. 예를 들어 App Services는 가용성에 대한 [SLA][SLA]를 보장합니다.

Application Insights는 초당 처리할 수 있는 요청 수의 [한도][app-insights-limits]가 있습니다. 요청 한도를 초과하면 메시지 제한이 발생할 수 있습니다. 이를 방지하려면 데이터 속도를 줄이는 [필터링][message-filtering] 또는 [샘플링][message-sampling]을 구현하세요.

그러나 실행하는 앱의 고가용성에 대한 고려 사항은 개발자의 책임입니다. 예를 들어 확장에 대한 정보는 기본 웹 응용 프로그램 참조 아키텍처의 [확장성 고려 사항](#scalability-considerations) 섹션을 참조하세요. 앱을 배포한 후에는 Application Insights를 사용하여 [앱의 가용성을 모니터링][monitor its availability]하는 테스트를 설정할 수 있습니다.

### <a name="security"></a>보안

중요한 정보 및 규정 준수 요구 사항은 데이터 수집, 보존 및 저장소에 영향을 줍니다. [Application Insights][application-insights] 및 [Log Analytics][log-analytics]의 원격 분석 데이터 처리 방법을 알아보세요.

다음 보안 고려 사항을 적용할 수 있습니다.

- 개발자가 자체 데이터를 수집하거나 기존 원격 분석 데이터를 보완하는 것이 허용되는 경우 개인 정보 처리에 대한 계획을 세웁니다.
- 데이터 보존을 고려합니다. 예를 들어 Application Insights는 원격 분석 데이터를 90일 동안 보존합니다. Microsoft Power BI, 연속 내보내기 또는 REST API를 사용하여 액세스하려는 데이터를 더 오래 보관합니다. 저장소 요율이 적용됩니다.
- Azure 리소스에 대한 액세스를 제한하여 데이터 액세스 권한과 특정 응용 프로그램에서 원격 분석 데이터를 볼 수 있는 사람을 제어합니다. 모니터링 원격 분석 데이터에 대한 액세스를 잠그려면 [Application Insights에서 리소스, 역할 및 액세스 제어][Resources, roles, and access control in Application Insights]를 참조하세요.
- 사용자가 버전을 추가하지 못하도록 응용 프로그램 코드에서 읽기/쓰기 액세스를 제어하거나 응용 프로그램에서 데이터 수집을 제한하는 태그 마커를 고려합니다. Application Insights를 사용하면 리소스로 전송된 개별 데이터 항목을 제어하지 않기 때문에, 어떤 데이터에 액세스할 수 있는 사용자가 개별 리소스의 모든 데이터에 액세스할 수 있습니다.
- 필요한 경우 Azure 리소스에 대한 정책 또는 비용 컨트롤을 강제 적용하는 [거버넌스](/azure/security/governance-in-azure) 메커니즘을 추가합니다. 예를 들어 정책 및 역할 기반 액세스 같은 보안 관련 모니터링을 위한 Log Analytics를 사용하거나 정책 정의를 만들고, 할당하고, 관리하는 [Azure Policy](/azure/azure-policy/azure-policy-introduction)를 사용합니다.
- Azure 리소스의 잠재적인 보안 문제를 모니터링하고 중앙에서 보안 상태를 살펴보려면 [Azure Security Center](/azure/security-center/security-center-intro) 사용을 고려합니다.

## <a name="pricing"></a>가격

모니터링 요금이 가파르게 증가할 수 있으므로 선불 가격 책정을 고려하고, 모니터링 대상을 이해하고, 각 서비스와 연결된 요금을 확인합니다. Azure Monitor는 비용 없이 [기본 메트릭][basic metrics]을 제공하며, [Application Insights][application-insights-pricing] 및 [Log Analytics][log-analytics]의 모니터링 비용은 수집되는 데이터의 양과 실행하는 테스트 수에 따라 결정됩니다.

[가격 책정 계산기][pricing]를 사용하여 비용을 예측해 보면 시작하는 데 도움이 될 수 있습니다. 특정 사용 사례의 가격 책정이 어떻게 변하는지 확인하려면 예상 배포에 따라 다양한 옵션을 변경하세요.

Application Insights의 원격 분석 데이터는 디버깅 동안 그리고 개발자가 앱을 게시한 Azure Portal로 전송됩니다. 비용 없이 테스트 용도로 사용할 수 있도록 제한된 양의 원격 분석 데이터를 계측합니다. 표시기를 더 추가하려면 원격 분석 제한을 높이면 됩니다. 세밀하게 제어하는 방법은 [Application Insights의 샘플링][Sampling in Application Insights]을 참조하세요.

배포 후에는 성능 지표의 [라이브 메트릭 스트림][Live Metrics Stream]을 볼 수 있습니다. 이 데이터는 저장되지 않습니다. 즉, 실시간 메트릭을 보는 것입니다. 하지만 원격 분석 데이터를 수집해 두었다가 나중에 분석할 수 있습니다. 라이브 스트림 데이터 요금은 없습니다.

Log Analytics는 서비스에 수집된 데이터에 대해 GB(기가바이트)당 비용이 청구됩니다. 매달 Azure Log Analytics 서비스에 수집되는 데이터 5GB까지는 무료이고, 처음 31일 동안은 데이터가 Log Analytics 작업 영역에 무료로 보관됩니다.

## <a name="next-steps"></a>다음 단계

고유의 모니터링 솔루션을 시작할 수 있도록 설계된 다음 리소스를 확인해 보세요.

[기본 웹 응용 프로그램 참조 아키텍처][Basic web application reference architecture]

[ASP.NET 웹 응용 프로그램 모니터링 시작][Start monitoring your ASP.NET Web Application]

[Azure Virtual Machines에 대한 데이터 수집][Collect data about Azure Virtual Machines]

## <a name="related-resources"></a>관련 리소스

[Azure 응용 프로그램 및 리소스 모니터링][Monitoring Azure applications and resources]

[Azure Application Insights를 사용하여 런타임 예외 찾기 및 진단][Find and diagnose run-time exceptions with Azure Application Insights]

<!-- links -->
[architecture]: ./media/architecture-app-monitoring.png
[availability-tests]: /azure/application-insights/app-insights-monitor-web-app-availability
[application-insights]: /azure/application-insights/app-insights-overview
[azure-monitor]: /azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor
[metrics]: /azure/monitoring-and-diagnostics/monitoring-supported-metrics
[Log Analytics 및 기타 서비스]: /azure/log-analytics/log-analytics-azure-storage
[log-analytics]: /azure/log-analytics/log-analytics-overview
[Azure Log Analytics agent]: https://blogs.msdn.microsoft.com/sqlsecurity/2017/12/28/azure-log-analytics-oms-agent-now-collects-sql-server-audit-logs/
[application-insights-pricing]: https://azure.microsoft.com/pricing/details/application-insights/
[Application Insights SDKs]: /azure/application-insights/app-insights-asp-net
[Application Insights Status Monitor]: https://azure.microsoft.com/updates/application-insights-status-monitor-and-sdk-updated/
[analyzing data across sources]: /azure/log-analytics/log-analytics-dashboards
[sending proactive alerts]: /azure/log-analytics/log-analytics-alerts
[the Azure portal]: /azure/log-analytics/log-analytics-tutorial-dashboards
[Azure Log Analytics Query Language]: https://docs.loganalytics.io/docs/Learn
[cross-resource queries]: https://azure.microsoft.com/blog/query-across-resources/
[alerts]: /azure/monitoring-and-diagnostics/monitoring-overview-alerts
[Alerts (Preview)]: /azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts
[Azure Monitor Data Source For Grafana]: https://grafana.com/plugins/grafana-azure-monitor-datasource
[Azure Automation]: /azure/automation/automation-intro
[ITSM solutions]: https://azure.microsoft.com/blog/itsm-connector-for-azure-is-now-generally-available/
[management solution]: /azure/monitoring/monitoring-solutions
[SLA]: https://azure.microsoft.com/support/legal/sla/app-service/v1_4/
[monitor its availability]: /azure/application-insights/app-insights-monitor-web-app-availability
[Resources, roles, and access control in Application Insights]: /azure/application-insights/app-insights-resources-roles-access-control
[basic metrics]: /azure/monitoring-and-diagnostics/monitoring-supported-metrics
[pricing]: https://azure.microsoft.com/pricing/calculator/#log-analyticsc126d8c1-ec9c-4e5b-9b51-4db95d06a9b1
[Sampling in Application Insights]: /azure/application-insights/app-insights-sampling
[Live Metrics Stream]: /azure/application-insights/app-insights-live-stream
[Basic web application reference architecture]: /azure/architecture/reference-architectures/app-service-web-app/basic-web-app#scalability-considerations
[Start monitoring your ASP.NET Web Application]: /azure/application-insights/quick-monitor-portal
[Collect data about Azure Virtual Machines]: /azure/log-analytics/log-analytics-quick-collect-azurevm
[Monitoring Azure applications and resources]: /azure/monitoring-and-diagnostics/monitoring-overview
[Find and diagnose run-time exceptions with Azure Application Insights]: /azure/application-insights/app-insights-tutorial-runtime-exceptions
[data-dog]: https://www.datadoghq.com/blog/azure-monitoring-enhancements/
[app-insights-limits]: /azure/azure-subscription-service-limits#application-insights-limits
[message-filtering]: /azure/application-insights/app-insights-api-filtering-sampling
[message-sampling]: /azure/application-insights/app-insights-sampling
