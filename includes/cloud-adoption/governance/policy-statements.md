<!-- TEMPLATE FILE - DO NOT ADD METADATA -->

## <a name="policy-statements"></a><span data-ttu-id="f27c5-101">정책 설명</span><span class="sxs-lookup"><span data-stu-id="f27c5-101">Policy statements</span></span>

<span data-ttu-id="f27c5-102">다음 정책 설명은 정의된 위험을 완화하는 데 필요한 요구 사항을 제시합니다.</span><span class="sxs-lookup"><span data-stu-id="f27c5-102">The following policy statements establish the requirements needed to mitigate the defined risks.</span></span> <span data-ttu-id="f27c5-103">이러한 정책은 거버넌스 MVP의 기능 요구 사항을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="f27c5-103">These policies define the functional requirements for the governance MVP.</span></span> <span data-ttu-id="f27c5-104">각 요구 사항은 거버넌스 MVP 구현에서 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="f27c5-104">Each will be represented in the implementation of the governance MVP.</span></span>

<span data-ttu-id="f27c5-105">신속한 배포:</span><span class="sxs-lookup"><span data-stu-id="f27c5-105">Deployment Acceleration:</span></span>

- <span data-ttu-id="f27c5-106">정의된 그룹화 및 태그 지정 전략에 따라 모든 자산을 그룹화하고 태그를 지정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f27c5-106">All assets must be grouped and tagged according to defined grouping and tagging strategies.</span></span>
- <span data-ttu-id="f27c5-107">모든 자산은 승인된 배포 모델을 사용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f27c5-107">All assets must use an approved deployment model.</span></span>
- <span data-ttu-id="f27c5-108">클라우드 공급자에 대한 거버넌스 기준이 설정되고 나면 모든 배포 도구는 거버넌스 팀이 정의한 도구와 호환되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f27c5-108">Once a governance foundation has been established for a cloud provider, any deployment tooling must be compatible with the tools defined by the governance team.</span></span>

<span data-ttu-id="f27c5-109">ID 기준:</span><span class="sxs-lookup"><span data-stu-id="f27c5-109">Identity Baseline:</span></span>

- <span data-ttu-id="f27c5-110">현재 거버넌스 정책을 통해 승인된 ID와 역할을 사용하여 클라우드에 배포된 모든 자산을 제어해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f27c5-110">All assets deployed to the cloud should be controlled using identities and roles approved by current governance policies.</span></span>
- <span data-ttu-id="f27c5-111">온-프레미스 Active Directory 인프라에서 권한이 상승된 모든 그룹은 승인된 RBAC 역할에 매핑되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f27c5-111">All groups in the on-premises Active Directory infrastructure that have elevated privileges should be mapped to an approved RBAC role.</span></span>

<span data-ttu-id="f27c5-112">보안 기준:</span><span class="sxs-lookup"><span data-stu-id="f27c5-112">Security Baseline:</span></span>

- <span data-ttu-id="f27c5-113">클라우드에 배포된 모든 자산에는 승인된 데이터 분류가 지정되어 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f27c5-113">Any asset deployed to the cloud must have an approved data classification.</span></span>
- <span data-ttu-id="f27c5-114">보안 및 거버넌스 관련 요구 사항을 충분히 승인하고 구현할 때까지는 보호된 데이터 수준으로 식별된 자산을 클라우드에 배포할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="f27c5-114">No assets identified with a protected level of data may be deployed to the cloud, until sufficient requirements for security and governance can be approved and implemented.</span></span>
- <span data-ttu-id="f27c5-115">최소 네트워크 보안 요구 사항의 유효성 검사/제어를 수행할 수 있게 될 때까지 클라우드 환경은 완충 영역으로 표시되며, 다른 데이터 센터나 내부 네트워크와 유사한 연결 요구 사항을 충족해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f27c5-115">Until minimum network security requirements can be validated and governed, cloud environments are seen as a demilitarized zone and should meet similar connection requirements to other data centers or internal networks.</span></span>

<span data-ttu-id="f27c5-116">비용 관리:</span><span class="sxs-lookup"><span data-stu-id="f27c5-116">Cost Management:</span></span>

- <span data-ttu-id="f27c5-117">모든 자산은 추적을 위해 핵심 비즈니스 기능 중 하나에 속한 애플리케이션 소유자에게 할당해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f27c5-117">For tracking purposes, all assets must be assigned to an application owner within one of the core business functions.</span></span>
- <span data-ttu-id="f27c5-118">비용 관련 문제가 발생하면 재무 팀과 협력하여 추가 거버넌스 요구 사항을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="f27c5-118">When cost concerns arise, additional governance requirements will be established with the Finance team.</span></span>

<span data-ttu-id="f27c5-119">리소스 일관성:</span><span class="sxs-lookup"><span data-stu-id="f27c5-119">Resource Consistency:</span></span>

- <span data-ttu-id="f27c5-120">이 단계에서는 중요 업무용 워크로드가 배포되지 않으므로 제어할 SLA, 성능 또는 BCDR 요구 사항도 없습니다.</span><span class="sxs-lookup"><span data-stu-id="f27c5-120">Because no mission-critical workloads are deployed at this stage, there are no SLA, performance, or BCDR requirements to be governed.</span></span>
- <span data-ttu-id="f27c5-121">중요 업무용 워크로드가 배포되면 IT 운영 팀과 협의하여 추가 거버넌스 요구 사항을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="f27c5-121">When mission-critical workloads are deployed, additional governance requirements will be established with IT operations.</span></span>

## <a name="processes"></a><span data-ttu-id="f27c5-122">프로세스</span><span class="sxs-lookup"><span data-stu-id="f27c5-122">Processes</span></span>

<span data-ttu-id="f27c5-123">이러한 거버넌스 정책의 지속적인 모니터링 및 적용을 위한 예산은 할당되지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="f27c5-123">No budget has been allocated for ongoing monitoring and enforcement of these governance policies.</span></span> <span data-ttu-id="f27c5-124">그러므로 클라우드 거버넌스 팀은 임시 방식을 통해 정책 설명 준수를 모니터링합니다.</span><span class="sxs-lookup"><span data-stu-id="f27c5-124">Because of that, the Cloud Governance team has some ad hoc ways to monitor adherence to policy statements.</span></span>

- <span data-ttu-id="f27c5-125">**교육**: 클라우드 거버넌스 팀은 시간을 투자해 클라우드 채택 팀을 대상으로 이러한 정책을 지원하는 거버넌스 경험 관련 교육을 진행합니다.</span><span class="sxs-lookup"><span data-stu-id="f27c5-125">**Education**: The Cloud Governance team is investing time to educate the cloud adoption teams on the governance journeys that support these policies.</span></span>
- <span data-ttu-id="f27c5-126">**배포** 검토: 클라우드 거버넌스 팀은 자산을 배포하기 전에 클라우드 채택 팀과 함께 거버넌스 경험을 검토합니다.</span><span class="sxs-lookup"><span data-stu-id="f27c5-126">**Deployment** reviews: Before deploying any asset, the Cloud Governance team will review the governance journey with the cloud adoption teams.</span></span>
