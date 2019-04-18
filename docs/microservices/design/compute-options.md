# <a name="choosing-a-compute-option-for-microservices"></a>마이크로 서비스에 대 한 계산 옵션 선택

*계산*이라는 용어는 애플리케이션이 실행되는 계산 리소스의 호스팅 모델을 말합니다. 마이크로 서비스 아키텍처의 경우 두 가지 접근 방식이 특히 많이 사용됩니다.

- 전용 노드(VM)에서 실행되는 서비스를 관리하는 서비스 오케스트레이터
- FaaS(functions as a service)를 사용하고 서버를 사용하지 않는 아키텍처

이것이 유일한 옵션은 아니지만, 마이크로 서비스 구축을 위해 검증된 방법입니다. 애플리케이션에 두 가지 방법이 모두 포함될 수 있습니다.

## <a name="service-orchestrators"></a>서비스 오케스트레이터

오케스트레이터는 서비스 집합 배포 및 관리와 관련된 작업을 처리합니다. 이러한 작업에는 노드에 서비스 배치, 서비스 상태 모니터링, 비정상 서비스 다시 시작, 서비스 인스턴스 사이에서 네트워크 트래픽 부하 조정, 서비스 검색, 서비스 인스턴스 수의 규모 조정, 구성 업데이트 적용이 포함됩니다. 많이 사용되는 오케스트레이터에는 Kubernetes, Service Fabric, DC/OS, Docker Swarm 등이 있습니다.

Azure 플랫폼에서 다음 옵션을 고려합니다.

- [AKS(Azure Kubernetes Service)](/azure/aks/)는 관리되는 Kubernetes 서비스입니다. AKS는 Kubernetes를 프로비전하고 Kubernetes API 엔드포인트를 노출하지만 Kubernetes 제어 플레인을 호스트 및 관리하고 자동 업그레이드, 자동 패치 적용, 자동 크기 조정 및 기타 관리 작업을 수행합니다. AKS를 "서비스로 제공되는 Kubernetes API"라고 생각할 수 있습니다.

- [Service Fabric](/azure/service-fabric/)은 마이크로 서비스를 패키징, 배포 및 관리하기 위한 분산 시스템 플랫폼입니다. 마이크로 서비스는 Service Fabric에 컨테이너, 이진 실행 파일 또는 [Reliable Services](/azure/service-fabric/service-fabric-reliable-services-introduction)로 배포될 수 있습니다. Reliable Services 프로그래밍 모델을 사용하면, 서비스에서 Service Fabric 프로그래밍 API를 직접 사용하여 시스템을 쿼리하고, 상태를 보고하고, 구성 및 코드 변경에 대한 알림을 받고, 다른 서비스를 검색할 수 있습니다. Service Fabric을 통한 주요 차별화는 [Reliable Collections](/azure/service-fabric/service-fabric-reliable-services-reliable-collections)를 사용하여 상태 저장 서비스를 구축하는 데 강력하게 집중하는 것입니다.

- Docker Enterprise Edition 및 Mesosphere DC/OS 등의 다른 옵션은 Azure에서 IaaS 환경에서 실행할 수 있습니다. 배포 템플릿을 찾을 수 있습니다 [Azure Marketplace](https://azuremarketplace.microsoft.com)합니다.

## <a name="containers"></a>컨테이너

컨테이너와 마이크로 서비스가 같은 것처럼 말하는 경우도 있습니다. 그것은 사실이 아니며 &mdash;마이크로 서비스를 만드는 데는 컨테이너가 필요하지 않습니다.&mdash; 컨테이너에는 마이크로 서비스에 특히 적합한 다음과 같은 이점이 있습니다.

- **이식성**. 컨테이너 이미지는 라이브러리나 다른 종속성을 설치할 필요 없이 실행되는 독립 실행형 패키지입니다. 따라서 배포가 쉽습니다. 컨테이너는 신속하게 시작하고 중지할 수 있기 때문에 부하를 더 많이 처리하거나 노드 장애를 복구하도록 새로운 인스턴스를 스핀업할 수 있습니다.

- **밀도**. 컨테이너는 OS 리소스를 공유하기 때문에 가상 머신을 실행하는 것보다 경량입니다. 여러 컨테이너를 단일 노드로 묶는 것이 가능하기 때문에 다수의 소규모 서비스로 구성된 애플리케이션에 특히 유용합니다.

- **리소스 격리**. 컨테이너에서 사용할 수 있는 메모리 및 CPU 양을 제한할 수 있기 때문에 런어웨이 프로세스가 호스트 리소스를 소진하지 않도록 하는 데 도움이 됩니다. 자세한 내용은 [격벽 패턴](../../patterns/bulkhead.md)을 참조하세요.

## <a name="serverless-functions-as-a-service"></a>서버리스(Functions as a Service)

[서버리스](https://azure.microsoft.com/solutions/serverless/) 아키텍처를 사용하면 VM 또는 가상 네트워크 인프라를 관리할 필요가 없습니다. 대신, 사용자가 코드를 배포하면 호스팅 서비스가 해당 코드를 VM에 넣고 실행합니다. 이 방법은 이벤트 기반 트리거를 사용하여 조율하는 소규모의 세분화된 함수를 선호하는 경향이 있습니다. 예를 들어 큐에 배치된 메시지는 큐에서 읽고 메시지를 처리하는 함수를 트리거 할 수 있습니다.

[Azure Functions](/azure/azure-functions/) 를 HTTP 요청, Service Bus 큐 및 Event Hubs 이벤트를 비롯 한 다양 한 함수 트리거를 지 원하는 서버 리스 계산 서비스입니다. 전체 목록을 참조 하세요 [Azure Functions 트리거 및 바인딩 개념](/azure/azure-functions/functions-triggers-bindings)합니다. 수도 [Azure Event Grid](/azure/event-grid/), Azure에서 관리 되는 이벤트 라우팅 서비스인 합니다.

<!-- markdownlint-disable MD026 -->

## <a name="orchestrator-or-serverless"></a>오케스트레이터 또는 서버리스

<!-- markdownlint-enable MD026 -->

오케스트레이터 방식과 서버리스 방식 중에서 선택할 때 고려해야 할 몇 가지 요소가 있습니다.

**관리 효율성** 플랫폼이 모든 계산 리소스를 관리하기 때문에 서버리스 애플리케이션은 관리가 쉽습니다. 오케스트레이터는 클러스터 관리 및 구성의 일부 측면을 추상화하지만 기본 VM은 완전히 숨기지 않습니다. 오케스트레이터를 사용하면 부하 분산, CPU 및 메모리 사용량 및 네트워킹과 같은 문제에 대해 생각해야 합니다.

**유연성 및 제어**. 오케스트레이터를 사용하면 서비스와 클러스터를 구성하고 관리하는 데 있어 많은 제어가 가능합니다. 단점은 복잡성이 추가되는 것입니다. 서버리스 아키텍처를 사용하면 이러한 세부 사항이 추상화되기 때문에 제어를 어느 정도 포기해야 합니다.

**이식성**. 여기에 나열된 모든 오케스트레이터(Kubernetes, DC/OS, Docker Swarm 및 Service Fabric)는 온-프레미스 또는 다수의 공용 클라우드에서 실행할 수 있습니다.

**애플리케이션 통합**. 서버리스 아키텍처를 사용하면 복잡한 애플리케이션을 개발하기 어려울 수 있습니다. Azure의 한 가지 옵션은 [Azure Logic Apps](/azure/logic-apps/)를 사용하여 Azure Functions 집합을 조정하는 것입니다. 이러한 방법에 대한 예제는 [Azure Logic Apps와 통합하는 함수 만들기](/azure/azure-functions/functions-twitter-email)를 참조하세요.

**비용**. 오케스트레이터를 사용하면 클러스터에서 실행 중인 VM에 대한 비용을 지불합니다. 서버리스 애플리케이션을 사용하면 소비한 실제 계산 리소스에 대해서만 비용을 지불합니다. 두 경우 모두 저장소, 데이터베이스 및 메시징 서비스와 같은 추가 서비스에 대한 비용을 고려해야 합니다.

**확장성**. Azure Functions는 들어오는 이벤트 수를 기반으로 수요에 맞게 자동으로 규모가 조정됩니다. 오케스트레이터를 사용하면 클러스터에서 실행 중인 서비스 인스턴스의 수를 늘려서 규모를 확장할 수 있습니다. 클러스터에 VM을 더 추가하여 확장할 수도 있습니다.

참조 구현에는 주로 Kubernetes를 사용했지만 한 가지 서비스 즉, Delivery History(배달 기록) 서비스에 대해서는 Azure Functions를 사용했습니다. Azure Functions는 이벤트 기반 워크로드이기 때문에 이러한 특정 서비스에 적합합니다 Event Hubs 트리거를 사용하여 함수를 호출하면 서비스에 최소한의 코드만 필요합니다. Delivery History(배달 기록) 서비스는 기본 워크플로에 속하지 않으므로 Kubernetes 클러스터 외부에서 실행하면 사용자가 시작한 작업의 종단 간 대기 시간에 영향을 미치지 않습니다.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [서비스 간 통신](./interservice-communication.md)
