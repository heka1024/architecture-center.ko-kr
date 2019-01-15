---
title: 정적 콘텐츠 호스팅 패턴
titleSuffix: Cloud Design Patterns
description: 정적 콘텐츠를 클라이언트에 직접 제공할 수 있는 클라우드 기반 저장소 서비스에 배포합니다.
keywords: 디자인 패턴
author: dragon119
ms.date: 01/04/2019
ms.custom: seodec18
ms.openlocfilehash: cf4f65e935a01e4d84b3cc82b5779edb729bd80e
ms.sourcegitcommit: 036cd03c39f941567e0de4bae87f4e2aa8c84cf8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/05/2019
ms.locfileid: "54058185"
---
# <a name="static-content-hosting-pattern"></a><span data-ttu-id="d5f95-104">정적 콘텐츠 호스팅 패턴</span><span class="sxs-lookup"><span data-stu-id="d5f95-104">Static Content Hosting pattern</span></span>

<span data-ttu-id="d5f95-105">정적 콘텐츠를 클라이언트에 직접 제공할 수 있는 클라우드 기반 저장소 서비스에 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-105">Deploy static content to a cloud-based storage service that can deliver them directly to the client.</span></span> <span data-ttu-id="d5f95-106">이렇게 하면 잠재적으로 비용이 많이 들 수 있는 계산 인스턴스에 대한 필요성이 줄어듭니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-106">This can reduce the need for potentially expensive compute instances.</span></span>

## <a name="context-and-problem"></a><span data-ttu-id="d5f95-107">컨텍스트 및 문제점</span><span class="sxs-lookup"><span data-stu-id="d5f95-107">Context and problem</span></span>

<span data-ttu-id="d5f95-108">웹 애플리케이션에는 일반적으로 일부 정적 콘텐츠 요소가 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-108">Web applications typically include some elements of static content.</span></span> <span data-ttu-id="d5f95-109">이 정적 콘텐츠에는 HTML 페이지, 그리고 HTML 페이지의 일부(예: 인라인 이미지, 스타일시트 및 클라이언트 쪽 JavaScript 파일) 혹은 별도의 다운로드(예: PDF 문서)로 클라이언트에서 사용할 수 있는 이미지 및 문서와 같은 기타 리소스가 포함될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-109">This static content might include HTML pages and other resources such as images and documents that are available to the client, either as part of an HTML page (such as inline images, style sheets, and client-side JavaScript files) or as separate downloads (such as PDF documents).</span></span>

<span data-ttu-id="d5f95-110">웹 서버가 동적 렌더링 및 출력 캐시에 최적화되더라도 정적 콘텐츠 다운로드에 대한 요청을 계속 처리해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-110">Although web servers are optimized for dynamic rendering and output caching, they still have to handle requests to download static content.</span></span> <span data-ttu-id="d5f95-111">이를 위해 소비되는 처리 주기는 실상 다른 목적을 위해 보다 효율적으로 사용될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-111">This consumes processing cycles that could often be put to better use.</span></span>

## <a name="solution"></a><span data-ttu-id="d5f95-112">해결 방법</span><span class="sxs-lookup"><span data-stu-id="d5f95-112">Solution</span></span>

<span data-ttu-id="d5f95-113">대부분의 클라우드 호스팅 환경에서 일부 애플리케이션의 리소스 및 정적 페이지를 스토리지 서비스에 배치할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-113">In most cloud hosting environments, you can put some of an application's resources and static pages in a storage service.</span></span> <span data-ttu-id="d5f95-114">스토리지 서비스는 이러한 리소스에 대한 요청을 처리함으로써 다른 웹 요청을 처리하는 컴퓨팅 리소스에 대한 부하를 줄일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-114">The storage service can serve requests for these resources, reducing load on the compute resources that handle other web requests.</span></span> <span data-ttu-id="d5f95-115">클라우드에 호스트된 저장소의 비용은 일반적으로 계산 인스턴스의 경우보다 훨씬 저렴합니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-115">The cost for cloud-hosted storage is typically much less than for compute instances.</span></span>

<span data-ttu-id="d5f95-116">애플리케이션의 일부를 스토리지 서비스에 호스트할 때 주요 고려 사항은 애플리케이션의 배포와, 그리고 익명 사용자의 사용을 의도하지 않는 리소스의 보안과 관련되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-116">When hosting some parts of an application in a storage service, the main considerations are related to deployment of the application and to securing resources that aren't intended to be available to anonymous users.</span></span>

## <a name="issues-and-considerations"></a><span data-ttu-id="d5f95-117">문제 및 고려 사항</span><span class="sxs-lookup"><span data-stu-id="d5f95-117">Issues and considerations</span></span>

<span data-ttu-id="d5f95-118">이 패턴을 구현할 방법을 결정할 때 다음 사항을 고려하세요.</span><span class="sxs-lookup"><span data-stu-id="d5f95-118">Consider the following points when deciding how to implement this pattern:</span></span>

- <span data-ttu-id="d5f95-119">호스트된 저장소 서비스는 사용자가 정적 리소스를 다운로드하기 위해 액세스할 수 있는 HTTP 엔드포인트를 노출해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-119">The hosted storage service must expose an HTTP endpoint that users can access to download the static resources.</span></span> <span data-ttu-id="d5f95-120">일부 저장소 서비스는 HTTPS도 지원하므로 SSL이 필요한 리소스를 저장소 서비스에 호스트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-120">Some storage services also support HTTPS, so it's possible to host resources in storage services that require SSL.</span></span>

- <span data-ttu-id="d5f95-121">최대 성능 및 가용성을 위해, CDN(콘텐츠 전송 네트워크)을 사용하여 전 세계에 있는 여러 데이터 센터에 저장소 컨테이너의 콘텐츠를 캐시하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-121">For maximum performance and availability, consider using a content delivery network (CDN) to cache the contents of the storage container in multiple datacenters around the world.</span></span> <span data-ttu-id="d5f95-122">그렇지만 CDN 사용 비용을 지불해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-122">However, you'll likely have to pay for using the CDN.</span></span>

- <span data-ttu-id="d5f95-123">저장소 계정은 데이터 센터에 영향을 줄 수 있는 이벤트에 대한 복원력을 제공하기 위해 기본적으로 지리적으로 복제되는 경우가 많습니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-123">Storage accounts are often geo-replicated by default to provide resiliency against events that might affect a datacenter.</span></span> <span data-ttu-id="d5f95-124">즉, IP 주소는 변경될 수 있지만 URL은 동일하게 유지됩니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-124">This means that the IP address might change, but the URL will remain the same.</span></span>

- <span data-ttu-id="d5f95-125">스토리지 계정에 있는 콘텐츠도 있고, 호스트된 컴퓨팅 인스턴스에 있는 콘텐츠도 있는 경우, 애플리케이션을 배포하고 업데이트하는 것이 더 어려워집니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-125">When some content is located in a storage account and other content is in a hosted compute instance, it becomes more challenging to deploy and update the application.</span></span> <span data-ttu-id="d5f95-126">정적 콘텐츠에 스크립트 파일 또는 UI 구성 요소가 포함되어 있는 경우에 특히, 보다 쉬운 관리를 위해 별도 배포를 수행하고, 애플리케이션 및 콘텐츠의 버전을 관리해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-126">You might have to perform separate deployments, and version the application and content to manage it more easily&mdash;especially when the static content includes script files or UI components.</span></span> <span data-ttu-id="d5f95-127">그러나 정적 리소스만 업데이트해야 할 경우에는, 애플리케이션 패키지를 다시 배포하지 않고도 해당 리소스를 저장소 계정에 업로드하기만 하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-127">However, if only static resources have to be updated, they can simply be uploaded to the storage account without needing to redeploy the application package.</span></span>

- <span data-ttu-id="d5f95-128">저장소 서비스는 사용자 지정 도메인 이름의 사용을 지원하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-128">Storage services might not support the use of custom domain names.</span></span> <span data-ttu-id="d5f95-129">이 경우, 리소스가 링크를 포함하는 동적으로 생성된 콘텐츠와는 다른 도메인에 포함되므로, 리소스의 전체 URL을 링크에 지정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-129">In this case it's necessary to specify the full URL of the resources in links because they'll be in a different domain from the dynamically-generated content containing the links.</span></span>

- <span data-ttu-id="d5f95-130">저장소 컨테이너는 공용 읽기 액세스용으로 구성해야 하며, 공용 쓰기 액세스용으로 구성하지 않아야 합니다. 사용자가 콘텐츠를 업로드하지 못하게 해야 하기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-130">The storage containers must be configured for public read access, but it's vital to ensure that they aren't configured for public write access to prevent users being able to upload content.</span></span>

- <span data-ttu-id="d5f95-131">발레 키 또는 토큰을 사용해서 익명으로 사용해서는 안 되는 리소스에 대한 액세스를 제어하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-131">Consider using a valet key or token to control access to resources that shouldn't be available anonymously.</span></span> <span data-ttu-id="d5f95-132">자세한 내용은 [발레 키 패턴](./valet-key.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="d5f95-132">See the [Valet Key pattern](./valet-key.md) for more information.</span></span>

## <a name="when-to-use-this-pattern"></a><span data-ttu-id="d5f95-133">이 패턴을 사용해야 하는 경우</span><span class="sxs-lookup"><span data-stu-id="d5f95-133">When to use this pattern</span></span>

<span data-ttu-id="d5f95-134">이 패턴은 다음의 경우에 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-134">This pattern is useful for:</span></span>

- <span data-ttu-id="d5f95-135">정적 리소스를 일부 포함한 웹 사이트 및 애플리케이션에 대한 호스팅 비용을 최소화할 경우</span><span class="sxs-lookup"><span data-stu-id="d5f95-135">Minimizing the hosting cost for websites and applications that contain some static resources.</span></span>

- <span data-ttu-id="d5f95-136">정적 콘텐츠 및 리소스만으로 구성된 웹 사이트에 대한 호스팅 비용을 최소화할 경우.</span><span class="sxs-lookup"><span data-stu-id="d5f95-136">Minimizing the hosting cost for websites that consist of only static content and resources.</span></span> <span data-ttu-id="d5f95-137">호스팅 공급 기업의 스토리지 시스템 기능에 따라, 완전 정적인 웹 사이트를 스토리지 계정에 전적으로 호스트할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-137">Depending on the capabilities of the hosting provider's storage system, it might be possible to entirely host a fully static website in a storage account.</span></span>

- <span data-ttu-id="d5f95-138">다른 호스팅 환경 또는 온-프레미스 서버에서 실행되는 애플리케이션에 대한 정적 리소스 및 콘텐츠를 노출할 경우</span><span class="sxs-lookup"><span data-stu-id="d5f95-138">Exposing static resources and content for applications running in other hosting environments or on-premises servers.</span></span>

- <span data-ttu-id="d5f95-139">전 세계 여러 데이터 센터에 저장소 계정의 콘텐츠를 캐시하는 콘텐츠 전송 네트워크를 사용하여 둘 이상의 지리적 영역에 콘텐츠를 배치할 경우</span><span class="sxs-lookup"><span data-stu-id="d5f95-139">Locating content in more than one geographical area using a content delivery network that caches the contents of the storage account in multiple datacenters around the world.</span></span>

- <span data-ttu-id="d5f95-140">비용 및 대역폭 사용량을 모니터링할 경우.</span><span class="sxs-lookup"><span data-stu-id="d5f95-140">Monitoring costs and bandwidth usage.</span></span> <span data-ttu-id="d5f95-141">정적 콘텐츠의 일부나 전체에 대해 별도 저장소 계정을 사용하면 호스팅 및 런타임 비용에서 콘텐츠 비용을 보다 쉽게 분리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-141">Using a separate storage account for some or all of the static content allows the costs to be more easily separated from hosting and runtime costs.</span></span>

<span data-ttu-id="d5f95-142">이 패턴은 다음과 같은 상황에서는 유용하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-142">This pattern might not be useful in the following situations:</span></span>

- <span data-ttu-id="d5f95-143">애플리케이션은 정적 콘텐츠를 클라이언트에 배달하기 전에 몇 가지 처리를 수행해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-143">The application needs to perform some processing on the static content before delivering it to the client.</span></span> <span data-ttu-id="d5f95-144">예를 들어, 문서에 타임스탬프를 추가해야 할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-144">For example, it might be necessary to add a timestamp to a document.</span></span>

- <span data-ttu-id="d5f95-145">정적 콘텐츠의 양이 매우 작습니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-145">The volume of static content is very small.</span></span> <span data-ttu-id="d5f95-146">이 콘텐츠를 별도 저장소에서 가져올 때 발생하는 오버헤드가 계산 리소스에서 이 콘텐츠를 별도로 관리할 때 파생되는 비용 혜택보다 클 우려가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-146">The overhead of retrieving this content from separate storage can outweigh the cost benefit of separating it out from the compute resource.</span></span>

## <a name="example"></a><span data-ttu-id="d5f95-147">예</span><span class="sxs-lookup"><span data-stu-id="d5f95-147">Example</span></span>

<span data-ttu-id="d5f95-148">Azure Storage는 스토리지 컨테이너에서 직접 정적 콘텐츠 제공을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-148">Azure Storage supports serving static content directly from a storage container.</span></span> <span data-ttu-id="d5f95-149">파일은 익명 액세스 요청을 통해 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-149">Files are served through anonymous access requests.</span></span> <span data-ttu-id="d5f95-150">기본적으로 파일에는 `https://contoso.z4.web.core.windows.net/image.png`와 같이 `core.windows.net`의 하위 도메인에 URL이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-150">By default, files have a URL in a subdomain of `core.windows.net`, such as `https://contoso.z4.web.core.windows.net/image.png`.</span></span> <span data-ttu-id="d5f95-151">사용자 지정 도메인 이름을 구성하고 Azure CDN을 사용하여 HTTPS를 통해 파일에 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-151">You can configure a custom domain name, and use Azure CDN to access the files over HTTPS.</span></span> <span data-ttu-id="d5f95-152">자세한 내용은 [Azure Storage에서 정적 웹 사이트 호스팅](/azure/storage/blobs/storage-blob-static-website)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="d5f95-152">For more information, see [Static website hosting in Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span></span>

![스토리지 서비스에서 직접 애플리케이션의 정적 부분 전달](./_images/static-content-hosting-pattern.png)

<span data-ttu-id="d5f95-154">정적 웹 사이트 호스팅은 파일을 익명으로 액세스할 수 있도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-154">Static website hosting makes the files available for anonymous access.</span></span> <span data-ttu-id="d5f95-155">파일에 액세스 권한이 있는 사용자를 제어해야 하는 경우 Azure Blob Storage에 파일을 저장한 다음, [공유 액세스 서명](/azure/storage/common/storage-dotnet-shared-access-signature-part-1)을 생성하여 액세스를 제한할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-155">If you need to control who can access the files, you can store files in Azure blob storage and then generate [shared access signatures](/azure/storage/common/storage-dotnet-shared-access-signature-part-1) to limit access.</span></span>

<span data-ttu-id="d5f95-156">클라이언트에 전달된 페이지의 링크는 리소스의 전체 URL을 반드시 지정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-156">The links in the pages delivered to the client must specify the full URL of the resource.</span></span> <span data-ttu-id="d5f95-157">리소스가 공유 액세스 서명과 같이 발레 키를 사용하여 보호되는 경우 이 서명은 URL에 포함되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-157">If the resource is protected with a valet key, such as a shared access signature, this signature must be included in the URL.</span></span>

<span data-ttu-id="d5f95-158">정적 리소스에 대해 외부 스토리지를 사용하는 방법을 설명하는 샘플 애플리케이션을 [GitHub][sample-app]에서 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-158">A sample application that demonstrates using external storage for static resources is available on [GitHub][sample-app].</span></span> <span data-ttu-id="d5f95-159">이 샘플에서는 스토리지 계정을 지정하는 구성 파일과, 정적 콘텐츠를 포함하는 컨테이너가 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-159">This sample uses configuration files to specify the storage account and container that holds the static content.</span></span>

```xml
<Setting name="StaticContent.StorageConnectionString"
         value="UseDevelopmentStorage=true" />
<Setting name="StaticContent.Container" value="static-content" />
```

<span data-ttu-id="d5f95-160">StaticContentHosting.Web 프로젝트의 Settings.cs 파일에 있는 `Settings` 클래스는 이러한 값을 추출하고 클라우드 저장소 계정 컨테이너 URL을 포함하는 문자열 값을 작성하기 위한 메서드를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-160">The `Settings` class in the file Settings.cs of the StaticContentHosting.Web project contains methods to extract these values and build a string value containing the cloud storage account container URL.</span></span>

```csharp
public class Settings
{
  public static string StaticContentStorageConnectionString {
    get
    {
      return RoleEnvironment.GetConfigurationSettingValue(
                              "StaticContent.StorageConnectionString");
    }
  }

  public static string StaticContentContainer
  {
    get
    {
      return RoleEnvironment.GetConfigurationSettingValue("StaticContent.Container");
    }
  }

  public static string StaticContentBaseUrl
  {
    get
    {
      var account = CloudStorageAccount.Parse(StaticContentStorageConnectionString);

      return string.Format("{0}/{1}", account.BlobEndpoint.ToString().TrimEnd('/'),
                                      StaticContentContainer.TrimStart('/'));
    }
  }
}
```

<span data-ttu-id="d5f95-161">StaticContentUrlHtmlHelper.cs 파일의 `StaticContentUrlHtmlHelper` 클래스는 전달된 URL이 ASP.NET 루트 경로 문자(~)로 시작하는 경우 클라우드 저장소 계정에 대한 경로를 포함한 URL을 생성하는 `StaticContentUrl`이라는 메서드를 노출합니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-161">The `StaticContentUrlHtmlHelper` class in the file StaticContentUrlHtmlHelper.cs exposes a method named `StaticContentUrl` that generates a URL containing the path to the cloud storage account if the URL passed to it starts with the ASP.NET root path character (~).</span></span>

```csharp
public static class StaticContentUrlHtmlHelper
{
  public static string StaticContentUrl(this HtmlHelper helper, string contentPath)
  {
    if (contentPath.StartsWith("~"))
    {
      contentPath = contentPath.Substring(1);
    }

    contentPath = string.Format("{0}/{1}", Settings.StaticContentBaseUrl.TrimEnd('/'),
                                contentPath.TrimStart('/'));

    var url = new UrlHelper(helper.ViewContext.RequestContext);

    return url.Content(contentPath);
  }
}
```

<span data-ttu-id="d5f95-162">Views\Home 폴더의 Index.cshtml에는 `StaticContentUrl` 메서드를 사용하여 해당 `src` 특성에 대한 URL을 만드는 이미지 요소가 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-162">The file Index.cshtml in the Views\Home folder contains an image element that uses the `StaticContentUrl` method to create the URL for its `src` attribute.</span></span>

```html
<img src="@Html.StaticContentUrl("~/media/orderedList1.png")" alt="Test Image" />
```

## <a name="related-patterns-and-guidance"></a><span data-ttu-id="d5f95-163">관련 패턴 및 지침</span><span class="sxs-lookup"><span data-stu-id="d5f95-163">Related patterns and guidance</span></span>

- <span data-ttu-id="d5f95-164">[정적 콘텐츠 호스팅 샘플][sample-app].</span><span class="sxs-lookup"><span data-stu-id="d5f95-164">[Static Content Hosting sample][sample-app].</span></span> <span data-ttu-id="d5f95-165">이 패턴을 설명하는 샘플 애플리케이션입니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-165">A sample application that demonstrates this pattern.</span></span>
- <span data-ttu-id="d5f95-166">[발레 키 패턴](./valet-key.md).</span><span class="sxs-lookup"><span data-stu-id="d5f95-166">[Valet Key pattern](./valet-key.md).</span></span> <span data-ttu-id="d5f95-167">대상 리소스를 익명 사용자가 사용할 수 없도록 하려면 이 패턴을 사용하여 직접 액세스를 제한합니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-167">If the target resources aren't supposed to be available to anonymous users, use this pattern to restrict direct access.</span></span>
- <span data-ttu-id="d5f95-168">[Azure의 서버리스 웹 애플리케이션](../reference-architectures/serverless/web-app.md).</span><span class="sxs-lookup"><span data-stu-id="d5f95-168">[Serverless web application on Azure](../reference-architectures/serverless/web-app.md).</span></span> <span data-ttu-id="d5f95-169">서버리스 웹앱을 구현하기 위해 Azure Functions를 통해 정적 웹 사이트 호스팅을 사용하는 참조 아키텍처입니다.</span><span class="sxs-lookup"><span data-stu-id="d5f95-169">A reference architecture that uses static website hosting with Azure Functions to implement a serverless web app.</span></span>

[sample-app]: https://github.com/mspnp/cloud-design-patterns/tree/master/static-content-hosting
