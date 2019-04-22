<!-- TEMPLATE FILE - DO NOT ADD METADATA -->

### <a name="governance-of-resources"></a><span data-ttu-id="93a26-101">리소스 거버넌스</span><span class="sxs-lookup"><span data-stu-id="93a26-101">Governance of resources</span></span>

<span data-ttu-id="93a26-102">Azure Blueprints 및 청사진 내의 관련 자산을 통해 여러 구독에 거버넌스를 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-102">Enforcing governance across subscriptions will come from Azure Blueprints and the associated assets within the blueprint.</span></span>

1. <span data-ttu-id="93a26-103">"거버넌스 MVP"라는 Azure Blueprint를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-103">Create an Azure Blueprint named "Governance MVP".</span></span>
    1. <span data-ttu-id="93a26-104">표준 Azure 역할 사용을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-104">Enforce the use of standard Azure roles.</span></span>
    2. <span data-ttu-id="93a26-105">사용자가 기존 RBAC 구현에만 인증할 수 있도록 하는 규칙을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-105">Enforce that users can only authenticate against existing an RBAC implementation.</span></span>
    3. <span data-ttu-id="93a26-106">관리 그룹 내의 모든 구독에 이 청사진을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-106">Apply this blueprint to all subscriptions within the management group.</span></span>
2. <span data-ttu-id="93a26-107">다음을 적용하는 Azure Policy를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-107">Create an Azure Policy to apply or enforce the following:</span></span>
    1. <span data-ttu-id="93a26-108">리소스 태그를 지정할 때 비즈니스 기능, 데이터 분류, 중요도, SLA, 환경 및 애플리케이션의 값을 포함해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-108">Resource tagging should require values for Business Function, Data Classification, Criticality, SLA, Environment, and  Application.</span></span>
    2. <span data-ttu-id="93a26-109">애플리케이션 태그의 값은 리소스 그룹 이름과 일치해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-109">The value of the Application tag should match the name of the resource group.</span></span>
    3. <span data-ttu-id="93a26-110">각 리소스 그룹 및 리소스에 대한 역할 할당 유효성을 검사합니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-110">Validate role assignments for each resource group and resource.</span></span>
3. <span data-ttu-id="93a26-111">각 관리 그룹에 "거버넌스 MVP" Azure Blueprint를 게시 및 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-111">Publish and apply the "Governance MVP" Azure Blueprint to each management group.</span></span>

<span data-ttu-id="93a26-112">이러한 패턴을 사용하면 리소스를 검색/추적하고 기본적인 역할 관리를 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-112">These patterns enable resources to be discovered and tracked, and enforce basic role management.</span></span>

### <a name="demilitarized-zone-dmz"></a><span data-ttu-id="93a26-113">DMZ(완충 영역)</span><span class="sxs-lookup"><span data-stu-id="93a26-113">Demilitarized Zone (DMZ)</span></span>

<span data-ttu-id="93a26-114">특정 구독에 온-프레미스 리소스에 대한 일정 수준의 액세스 권한이 필요한 경우를 흔히 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-114">It’s common for specific subscriptions to require some level of access to on-premises resources.</span></span> <span data-ttu-id="93a26-115">일부 종속 리소스가 아직 온-프레미스 데이터 센터에 있는 마이그레이션 시나리오 또는 개발 시나리오가 이러한 경우에 해당될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-115">This may be the case for migration scenarios or development scenarios, when some dependent resources are still in the on-premises datacenter.</span></span> <span data-ttu-id="93a26-116">이 경우 거버넌스 MVP는 다음의 모범 사례를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-116">In this case, the governance MVP adds the following best practices:</span></span>

1. <span data-ttu-id="93a26-117">클라우드 DMZ를 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-117">Establish a Cloud DMZ.</span></span>
    1. <span data-ttu-id="93a26-118">[클라우드 DMZ 참조 아키텍처](/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid)가 Azure에서 VPN Gateway 생성을 위한 패턴 및 배포 모델을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-118">The [Cloud DMZ reference architecture](/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid) establishes a pattern and deployment model for creating a VPN Gateway in Azure.</span></span>
    2. <span data-ttu-id="93a26-119">온-프레미스 데이터 센터의 로컬 Edge 디바이스에 대해 적절한 DMZ 연결 및 보안 요구 사항이 마련되어 있는지 유효성을 검사합니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-119">Validate that proper DMZ connectivity and security requirements are in place for a local edge device in the on-premises datacenter.</span></span>
    3. <span data-ttu-id="93a26-120">로컬 Edge 디바이스가 Azure VPN Gateway 요구 사항과 호환되는지 유효성을 검사합니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-120">Validate that the local edge device is compatible with Azure VPN Gateway requirements.</span></span>
    4. <span data-ttu-id="93a26-121">온-프레미스 VPN에 대한 연결이 확인되면 해당 참조 아키텍처에서 생성된 Resource Manager 템플릿을 캡처합니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-121">Once connection to the on-premise VPN has been verified, capture the Resource Manager template created by that reference architecture.</span></span>
2. <span data-ttu-id="93a26-122">"DMZ"라는 두 번째 청사진을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-122">Create a second blueprint named "DMZ".</span></span>
    1. <span data-ttu-id="93a26-123">VPN Gateway용 Resource Manager 템플릿을 청사진에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-123">Add the Resource Manager template for the VPN Gateway to the blueprint.</span></span>
3. <span data-ttu-id="93a26-124">온-프레미스에 연결해야 하는 모든 구독에 DMZ 청사진을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-124">Apply the DMZ blueprint to any subscriptions requiring on-premises connectivity.</span></span> <span data-ttu-id="93a26-125">거버넌스 MVP 청사진과 함께 이 청사진을 추가로 적용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-125">This blueprint should be applied in addition to the governance MVP blueprint.</span></span>

<span data-ttu-id="93a26-126">IT 보안 및 기존 거버넌스 팀에서 제기하는 가장 큰 문제 중 하나는 초기 단계 클라우드 채택에서 기존 자산이 손상될 위험입니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-126">One of the biggest concerns raised by IT security and traditional governance teams, is the risk of early stage cloud adoption compromising existing assets.</span></span> <span data-ttu-id="93a26-127">클라우드 채택 팀은 위에서 설명한 방식을 통해 온-프레미스 자산에 대한 위험을 줄이면서 하이브리드 솔루션을 빌드하고 마이그레이션할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-127">The above approach allows cloud adoption teams to build and migrate hybrid solutions, with reduced risk to on-premises assets.</span></span> <span data-ttu-id="93a26-128">이 임시 솔루션은 이후 개선 단계에서 제거됩니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-128">In later evolution, this temporary solution would be removed.</span></span>

> [!NOTE]
> <span data-ttu-id="93a26-129">위의 내용은 기본 거버넌스 MVP를 빠르게 만들기 위해 시작할 수 있는 단계이며</span><span class="sxs-lookup"><span data-stu-id="93a26-129">The above is a starting point to quickly create a baseline governance MVP.</span></span> <span data-ttu-id="93a26-130">거버넌스 경험의 시작 부분일 뿐입니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-130">This is only the beginning of the governance journey.</span></span> <span data-ttu-id="93a26-131">회사가 클라우드 채택을 계속 진행하면서 다음 분야에서 위험이 추가로 발생하면 해당 단계를 추가로 개선해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-131">Further evolution will be needed as the company continues to adopt the cloud and takes on more risk in the following areas:</span></span>
>
> - <span data-ttu-id="93a26-132">중요 업무용 워크로드</span><span class="sxs-lookup"><span data-stu-id="93a26-132">Mission-critical workloads</span></span>
> - <span data-ttu-id="93a26-133">보호된 데이터</span><span class="sxs-lookup"><span data-stu-id="93a26-133">Protected data</span></span>
> - <span data-ttu-id="93a26-134">비용 관리</span><span class="sxs-lookup"><span data-stu-id="93a26-134">Cost management</span></span>
> - <span data-ttu-id="93a26-135">다중 클라우드 시나리오</span><span class="sxs-lookup"><span data-stu-id="93a26-135">Multi-cloud scenarios</span></span>
>
><span data-ttu-id="93a26-136">또한 이 MVP의 특정 세부 정보는 아래의 문서에서 설명하는 가상 기업의 예제 경험을 기준으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-136">Moreover, the specific details of this MVP are based on the example journey of a fictitious company, described in the articles that follow.</span></span> <span data-ttu-id="93a26-137">그러므로 이 모범 사례를 구현하기 전에 이 시리즈에 포함된 다른 문서의 내용을 확인하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="93a26-137">We highly recommend becoming familiar with the other articles in this series before implementing this best practice.</span></span>