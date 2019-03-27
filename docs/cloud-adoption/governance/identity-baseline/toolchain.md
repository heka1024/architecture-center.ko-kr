---
title: 'CAF: Azure의 ID 기준 도구'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Azure의 ID 기준 도구
author: BrianBlanchard
ms.openlocfilehash: 81b0fa9cfee597da98d8b983fb155eac82d97bf8
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58243674"
---
# <a name="identity-baseline-tools-in-azure"></a>Azure의 ID 기준 도구

[ID 기준](overview.md)은 [5개 클라우드 거버넌스 분야](../governance-disciplines.md) 중 하나입니다. 이 분야는 애플리케이션 또는 워크로드를 호스팅하는 클라우드 공급자에 관계없이 사용자 ID의 일관성과 연속성을 보장하는 정책을 설정하는 방법에 중점을 둡니다.

하이브리드 ID에 대한 검색 가이드에 포함된 도구는 다음과 같습니다.

**Active Directory(온-프레미스):** Active Directory는 사용자 자격 증명을 저장하고 유효성을 검사하기 위해 엔터프라이즈에서 가장 자주 사용되는 ID 공급자입니다.

**Azure Active Directory:** Active Directory와 동등한 SaaS(Software as a Service)이며, 온-프레미스 Active Directory와 페더레이션할 수 있습니다.

**Active Directory(IaaS):** Azure의 가상 머신에서 실행되는 Active Directory 애플리케이션의 인스턴스입니다.

ID는 IT 보안에 대한 제어 평면입니다. 따라서 인증은 클라우드에 대한 조직의 액세스 보호입니다. 조직에서는 보안을 강화하고 클라우드 앱을 침입자들로부터 안전하게 보호하는 ID 제어 평면을 필요로 합니다.

## <a name="cloud-authentication"></a>클라우드 인증

올바른 인증 방법을 선택하는 것은 앱에서 클라우드로 이동하려는 조직이 첫 번째로 고려해야 할 사항입니다.

이 방법을 선택하면 Azure AD에서 사용자의 로그인 프로세스를 처리합니다. 원활한 SSO(Single Sign-On)와 결합되어 있으므로, 사용자는 자격 증명을 다시 입력하지 않고도 클라우드 앱에 로그인할 수 있습니다. 클라우드 인증을 사용할 경우 다음 두 가지 옵션 중에서 선택할 수 있습니다.

**Azure AD 암호 해시 동기화**: Azure AD에서 온-프레미스 디렉터리 개체에 대한 인증을 사용하는 가장 간단한 방법입니다. 온-프레미스 서버가 중단되는 경우 모든 방법에서 백업 장애 조치 인증 방법으로 사용할 수도 있습니다.

**Azure AD 통과 인증**: 하나 이상의 온-프레미스 서버에서 실행되는 소프트웨어 에이전트를 사용하여 Azure AD 인증 서비스에 대한 영구 암호 유효성 검사를 제공합니다.

> [!NOTE]
> 온-프레미스 사용자 계정 상태, 암호 정책 및 로그인 시간을 즉시 적용해야 하는 보안 요구 사항이 있는 회사에서는 통과 인증 방법을 고려해야 합니다.

**페더레이션 인증:**

이 방법을 선택하면 Azure AD에서 인증 프로세스를 별도의 신뢰할 수 있는 인증 시스템(예: 온-프레미스 AD FS(Active Directory Federation Services) 또는 신뢰할 수 있는 타사 페더레이션 공급자)으로 전달하여 사용자 암호의 유효성을 검사합니다.

[Azure Active Directory에 적합한 인증 방법 선택](/azure/security/azure-ad-choose-authn) 문서에는 조직에 가장 적합한 솔루션을 선택하는 데 도움이 되는 의사 결정 트리가 포함되어 있습니다.

다음 표에는 이 거버넌스 분야를 지원하는 정책과 프로세스를 완성하는 데 도움이 되는 네이티브 도구가 나와 있습니다.

<!-- markdownlint-disable MD033 -->

|고려 사항|암호 해시 동기화 + 원활한 SSO|통과 인증 + 원활한 SSO|AD FS로 페더레이션|
|:-----|:-----|:-----|:-----|
|인증은 어디서 수행되나요?|클라우드|클라우드에서 온-프레미스 인증 에이전트와 보안 암호 확인을 교환한 후|온-프레미스|
|프로비저닝 시스템인 Azure AD Connect 이외의 온-프레미스 서버 요구 사항은 무엇인가요?|없음|각 추가 인증 에이전트마다 서버 1개|둘 이상의 AD FS 서버<br><br>경계/DMZ 네트워크에 둘 이상의 WAP 서버|
|프로비저닝 시스템 이외의 온-프레미스 인터넷 및 네트워킹 요구 사항은 무엇인가요?|없음|인증 에이전트를 실행하는 서버의[아웃바운드 인터넷 액세스](/azure/active-directory/hybrid/how-to-connect-pta-quick-start)|경계에 있는 WAP 서버에 대한 [인바운드 인터넷 액세스](/windows-server/identity/ad-fs/overview/ad-fs-requirements)<br><br>경계에 있는 WAP 서버에서 AD FS 서버로의 인바운드 네트워크 액세스<br><br>네트워크 부하 분산|
|SSL 인증서 요구 사항이 있나요?|아니오|아니요|예|
|상태 모니터링 솔루션이 있나요?|필요하지 않음|[Azure Active Directory 관리 센터](/azure/active-directory/hybrid/tshoot-connect-pass-through-authentication)에서 제공한 에이전트 상태|[Azure AD Connect Health](/azure/active-directory/hybrid/how-to-connect-health-adfs)|
|사용자가 회사 네트워크 내의 도메인 가입 디바이스에서 Single Sign-On 방식으로 클라우드 리소스에 액세스할 수 있나요?|[원활한 SSO](/azure/active-directory/hybrid/how-to-connect-sso)의 경우 예|[원활한 SSO](/azure/active-directory/hybrid/how-to-connect-sso)의 경우 예|예|
|지원되는 로그인 유형은 무엇인가요?|UserPrincipalName + 암호<br><br>[원활한 SSO](/azure/active-directory/hybrid/how-to-connect-sso)를 사용하는 Windows 통합 인증<br><br>[대체 로그인 ID](/azure/active-directory/hybrid/how-to-connect-install-custom)|UserPrincipalName + 암호<br><br>[원활한 SSO](/azure/active-directory/hybrid/how-to-connect-sso)를 사용하는 Windows 통합 인증<br><br>[대체 로그인 ID](/azure/active-directory/hybrid/how-to-connect-pta-faq)|UserPrincipalName + 암호<br><br>sAMAccountName + 암호<br><br>Windows 통합 인증<br><br>[인증서 및 스마트 카드 인증](/windows-server/identity/ad-fs/operations/configure-user-certificate-authentication)<br><br>[대체 로그인 ID](/windows-server/identity/ad-fs/operations/configuring-alternate-login-id)|
|비즈니스용 Windows Hello가 지원되나요?|[키 신뢰 모델](/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br><br>[Intune을 사용하는 인증서 신뢰 모델](https://blogs.technet.microsoft.com/microscott/setting-up-windows-hello-for-business-with-intune/)|[키 신뢰 모델](/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br><br>[Intune을 사용하는 인증서 신뢰 모델](https://blogs.technet.microsoft.com/microscott/setting-up-windows-hello-for-business-with-intune/)|[키 신뢰 모델](/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br><br>[인증서 신뢰 모델](/windows/security/identity-protection/hello-for-business/hello-key-trust-adfs)|
|다단계 인증 옵션은 무엇인가요?|[Azure MFA](/azure/multi-factor-authentication/)<br><br>[조건부 액세스를 사용하는 사용자 지정 컨트롤*](/azure/active-directory/conditional-access/controls#custom-controls-1)|[Azure MFA](/azure/multi-factor-authentication/)<br><br>[조건부 액세스를 사용하는 사용자 지정 컨트롤*](/azure/active-directory/conditional-access/controls#custom-controls-1)|[Azure MFA](/azure/multi-factor-authentication/)<br><br>[Azure MFA 서버](/azure/active-directory/authentication/howto-mfaserver-deploy)<br><br>[타사 MFA](/windows-server/identity/ad-fs/operations/configure-additional-authentication-methods-for-ad-fs)<br><br>[조건부 액세스를 사용하는 사용자 지정 컨트롤*](/azure/active-directory/conditional-access/controls#custom-controls-1)|
|지원되는 사용자 계정 상태는 무엇인가요?|비활성화된 계정<br>(최대 30분 지연)|비활성화된 계정<br><br>계정 잠김<br><br>계정이 만료됨<br><br>암호 만료됨<br><br>로그인 시간|비활성화된 계정<br><br>계정 잠김<br><br>계정이 만료됨<br><br>암호 만료됨<br><br>로그인 시간|
|조건부 액세스 옵션이란 무엇인가요?|[Azure AD 조건부 액세스](/azure/active-directory/active-directory-conditional-access-azure-portal)|[Azure AD 조건부 액세스](/azure/active-directory/active-directory-conditional-access-azure-portal)|[Azure AD 조건부 액세스](/azure/active-directory/active-directory-conditional-access-azure-portal)<br><br>[AD FS 클레임 규칙](https://adfshelp.microsoft.com/AadTrustClaims/ClaimsGenerator)|
|레거시 프로토콜 차단이 지원되나요?|[예](/azure/active-directory/active-directory-conditional-access-conditions#legacy-authentication)|[예](/azure/active-directory/active-directory-conditional-access-conditions#legacy-authentication)|[예](/windows-server/identity/ad-fs/operations/access-control-policies-w2k12)|
|로그인 페이지에서 로고, 이미지 및 설명을 사용자 지정할 수 있나요?|[예(Azure AD Premium의 경우)](/azure/active-directory/customize-branding)|[예(Azure AD Premium의 경우)](/azure/active-directory/customize-branding)|[예](/azure/active-directory/connect/active-directory-aadconnect-federation-management#customlogo)|
|지원되는 고급 시나리오는 무엇인가요?|[스마트 암호 잠금](/azure/active-directory/active-directory-secure-passwords)<br><br>[유출된 자격 증명 보고서](/azure/active-directory/active-directory-reporting-risk-events)|[스마트 암호 잠금](/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-smart-lockout)|다중 사이트 낮은 대기 시간 인증 시스템<br><br>[AD FS 엑스트라넷 잠금](/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-soft-lockout-protection)<br><br>[타사 ID 시스템과 통합](/azure/active-directory/connect/active-directory-aadconnect-federation-compatibility)|

<!-- markdownlint-enable MD033 -->

> [!NOTE]
> 현재, Azure AD 조건부 액세스의 사용자 지정 컨트롤은 디바이스 등록을 지원하지 않습니다.

## <a name="next-steps"></a>다음 단계

[하이브리드 ID 디지털 변환 프레임워크](https://resources.office.com/ww-landing-M365E-EMS-IDAM-Hybrid-Identity-WhitePaper.html?LCID=EN-US)는 이러한 각 구성 요소를 선택하고 통합하기 위한 다양한 조합과 솔루션에 대해 간략하게 설명합니다.

[Azure AD Connect 도구](https://aka.ms/aadconnectwiz)를 사용하면 온-프레미스 디렉터리를 Azure AD와 통합할 수 있습니다.
