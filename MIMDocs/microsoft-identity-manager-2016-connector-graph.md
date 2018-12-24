---
title: Microsoft Graph용 Microsoft Identity Manager 커넥터 | Microsoft Docs
author: fimguy
description: Microsoft Graph용 Microsoft Identity Manager 커넥터는 외부 사용자 AD 계정 수명 주기 관리를 활성화합니다. 이 시나리오에서 조직은 Azure AD 디렉터리에 게스트를 초대하고 온-프레미스 Windows 통합 인증 또는 Kerberos 기반 응용 프로그램에 대한 액세스 권한을 이 게스트에게 부여하려고 합니다.
keywords: ''
ms.author: billmath
manager: mtillman
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 2e376bcc88518b911f93ce9cd4ab920eb428815b
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358655"
---
<a name="microsoft-identity-manager-connector-for-microsoft-graph"></a>Microsoft Graph용 Microsoft Identity Manager 커넥터
=======================================================================================

<a name="summary"></a>요약 
=======

[Microsoft Graph용 Microsoft Identity Manager 커넥터](http://go.microsoft.com/fwlink/?LinkId=717495)에서는 Azure AD Premium 고객을 위한 추가 통합 시나리오를 사용할 수 있습니다.  [Microsoft Graph API](https://developer.microsoft.com/en-us/graph/) v1 및 베타에서 가져온 MIM 동기화 메타버스 추가 개체에 표면화합니다.

<a name="scenarios-covered"></a>적용되는 시나리오
=================

<a name="b2b-account-lifecycle-management"></a>B2B 계정 수명 주기 관리
--------------------------------

Microsoft Graph용 Microsoft Identity Manager 커넥터의 초기 시나리오는 외부 사용자에 대한 AD DS 계정 수명 주기 관리를 자동화하는 데 도움이 되는 커넥터입니다. 이 시나리오에서 조직은 Azure AD Connect를 사용하여 AD DS에서 Azure AD로 직원을 동기화하고, Azure AD 디렉터리로 게스트를 초대합니다. 게스트를 초대하면 외부 사용자 개체가 해당 조직의 AD DS가 아닌 해당 조직의 Azure AD 디렉터리에 있게 됩니다. 그런 다음, 조직은 [Azure AD 응용 프로그램 프록시](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) 또는 기타 게이트웨이 메커니즘을 통해 온-프레미스 Windows 통합 인증 또는 Kerberos 기반 응용 프로그램에 대한 액세스 권한을 이 게스트에게 부여하려고 합니다. Azure AD 응용 프로그램 프록시를 사용하려면 식별 및 위임용으로 각 사용자에게 고유한 AD DS 계정이 있어야 합니다.  

게스트에 대한 AD DS 계정을 자동으로 만들고 유지 관리하도록 MIM 동기화를 구성하는 방법을 알아보려면 [Azure 응용 프로그램 프록시를 사용하여 MIM 2016 SP1과 Azure AD B2B(기업 간) 공동 작업](~/microsoft-identity-manager-2016-graph-b2b-scenario.md) 문서를 참조하세요.  해당 문서는 커넥터에 필요한 동기화 규칙을 보여줍니다.

<a name="other-identity-management-scenarios"></a>기타 ID 관리 시나리오
---------------

Azure AD에 대한 사용자 및 그룹 동기화 외에 Azure AD에서 사용자, 그룹 및 연락처 개체의 만들기, 읽기, 업데이트 및 삭제를 포함하는 다른 특정 ID 관리 시나리오에 커넥터를 사용할 수 있습니다. 잠재적 시나리오를 평가할 때 이 커넥터는 데이터 흐름 겹침, Azure AD Connect 배포와의 실제 또는 잠재적 동기화 충돌을 발생시키는 시나리오에서 작동될 수 없습니다.  [Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594)는 온-프레미스 디렉터리에서 Azure AD로 사용자 및 그룹을 동기화하여 Azure AD와 온-프레미스 디렉터리를 통합하는 권장되는 방법입니다.  Azure AD Connect에는 많은 동기화 기능이 있으며 MIM에서 생성된 개체에 사용할 수 없는 암호 및 디바이스 쓰기 저장과 같은 시나리오를 활성화합니다. 예를 들어 데이터를 AD DS로 가져오는 경우 해당 개체를 Azure AD 디렉터리에 다시 일치시키도록 시도하는 Azure AD Connect에서 제외되었는지 확인합니다.  또는 Azure AD Connect에서 만든 Azure AD 개체를 변경하는데 이 커넥터를 사용할 수 없습니다.



<a name="preparing-to-use-the-connector-for-microsoft-graph"></a>Microsoft Graph에 커넥터를 사용하도록 준비
=============================================================

<a name="authorizing-the-connector-to-retrieve-or-manage-objects-in-your-azure-ad-directory"></a>Azure AD 디렉터리에서 개체를 검색하거나 관리하도록 커넥터에 권한 부여
----------------------------------------------------

1.  커넥터를 사용하려면 Microsoft Graph를 통해 Azure AD 개체에서 작동하는 데 적절한 권한을 부여할 수 있도록 Azure AD에 웹앱/API 응용 프로그램을 만들어야 합니다.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

그림 1. 새 응용 프로그램 등록

2.  Azure Portal에서 만든 응용 프로그램을 열고, MA의 연결 페이지에서 나중에 사용하도록 클라이언트 ID로 응용 프로그램 ID를 저장합니다.

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

그림 2. 응용 프로그램 ID

3.  [모든 설정] -\> [키]를 열어 새 클라이언트 암호를 생성합니다. 일부 키 설명을 설정하고 필요한 기간을 선택합니다. 변경 내용을 저장합니다. 이 페이지를 종료하면 암호 값을 사용할 수 없습니다.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

그림 3. 새 클라이언트 암호

4.  “필수 사용 권한”을 열어 응용 프로그램에 “Microsoft Graph API”를 추가합니다.

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

그림 4. 새 API 추가

시나리오에 따라 “Microsoft Graph API”를 사용할 수 있도록 응용 프로그램에 다음 권한을 추가해야 합니다.

| 개체 작업 | 필요한 권한                                                                  | 권한 유형 |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| 그룹 가져오기          | `Group.Read.All` 또는 `Group.ReadWrite.All`                                                | 응용 프로그램     |
| 사용자 가져오기           | `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` 또는 `Directory.ReadWrite.All` | 응용 프로그램     |

필요한 권한에 대한 자세한 내용은 [여기](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference)에서 확인할 수 있습니다.

5. 응용 프로그램에 필요한 권한을 부여합니다.


<a name="installing-the-connector"></a>커넥터 설치
========================

6.  커넥터를 설치하기 전에 동기화 서버에 다음이 있는지 확인합니다. 

 - Microsoft .NET 4.5.2 Framework 이상
 - Microsoft Identity Manager 2016 SP1, 핫픽스 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) 이상을 사용해야 합니다.

7. Microsoft Identity Manager 2016 SP1용 다른 커넥터 외에 Microsoft Graph용 커넥터는 [Microsoft 다운로드 센터](https://www.microsoft.com/en-us/download/details.aspx?id=51495)에서 다운로드할 수 있습니다.

8.  MIM 동기화 서비스를 다시 시작합니다.
 
<a name="connector-configuration"></a>커넥터 구성
=======================


9.  동기화 서비스 관리자 UI에서 **커넥터**와 **만들기**를 선택합니다.
**Graph(Microsoft)** 를 선택하고. 커넥터를 만들고 설명하는 이름을 지정합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)


10. MIM 동기화 서비스 UI에서 응용 프로그램 ID 및 생성된 클라이언트 비밀을 지정합니다. MIM 동기화에 구성된 각 관리 에이전트에는 동일한 응용 프로그램에 대해 병렬로 가져오기를 실행하지 않도록 Azure AD에서 자체 응용 프로그램이 있어야 합니다.


![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

그림 5. 연결 페이지

연결 페이지(그림 5)에는 사용되는 Graph API 버전과 테넌트 이름이 포함되어 있습니다. 클라이언트 ID 및 클라이언트 비밀은 Azure AD에서 만들어야 하는 WebAPI 응용 프로그램의 응용 프로그램 ID 및 키 값을 나타냅니다.

11. 글로벌 매개 변수 페이지에서 필요한 변경 내용을 만듭니다.

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

그림 6. 전역 매개 변수 페이지

전역 매개 변수 페이지에는 다음 설정이 포함되어 있습니다.

- DateTime 형식 - Edm.DateTimeOffset 유형의 특성에 사용되는 형식입니다. 모든 날짜는 가져오는 동안 해당 형식을 사용하여 문자열로 변환됩니다. 설정 형식은 모든 특성에 대해 적용되며, 날짜를 저장합니다.

 - HTTP 시간 제한(초) - WebAPI 응용 프로그램에 대한 각 HTTP 호출 중에 사용될 시간 제한(초)입니다.

 - Force change password for created user at next sign(다음 로그인 시 생성된 사용자의 암호 강제 변경) - 이 옵션은 내보내기 중 생성되는 새 사용자에 사용됩니다. 이 옵션을 사용하면 [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) 속성이 true로 설정되고, 그러지 않으면 false입니다.

<a name="configuring-the-connector-schema-and-operations"></a>커넥터 스키마 및 작업 구성
=========================

12.   스키마를 구성합니다.  커넥터는 다음과 같은 개체 형식 목록을 지원합니다.

-   사용자

    -   전체/델타 가져오기

    -   내보내기(추가, 업데이트, 삭제)

-   Group

    -   전체/델타 가져오기

    -   내보내기(추가, 업데이트, 삭제)


다음은 지원되는 특성 유형 목록입니다.

-   `Edm.Boolean`

-   `Edm.String`

-   `Edm.DateTimeOffset`(커넥터 공간의 문자열)

-   `microsoft.graph.directoryObject`(지원되는 모든 개체에 대한 커넥터 공간의 참조)

-   `microsoft.graph.contact`

위의 목록의 모든 유형에 대해 다중값 특성(컬렉션)도 지원됩니다.

커넥터는 앵커에는 ‘`id`’ 특성을, 모든 개체에는 DN을 사용합니다.  따라서 Graph API에서 해당 'id' 특성을 변경하는 개체를 허용하지 않으므로 이름 바꾸기는 필요하지 않습니다.


<a name="access-token-lifetime"></a>액세스 토큰 수명
=====================

그래프 응용 프로그램에서 Graph API에 액세스하려면 액세스 토큰이 필요합니다. 커넥터는 각 가져오기 반복에 대해 새 액세스 토큰을 요청합니다. 가져오기 반복은 페이지 크기에 따라 달라집니다. 예를 들면 다음과 같습니다.

-   Azure AD에 10000개의 개체가 포함되어 있음

-   커넥터에 구성된 페이지 크기가 5,000임

이 경우 가져오기 중 두 번의 반복이 있으며, 각 반복에서 동기화할 개체를 5000개 반환합니다. 따라서 새 액세스 토큰이 두 번 요청됩니다.

내보내기 중에는 추가/업데이트/삭제해야 하는 각 개체에 대해 새 액세스 토큰을 요청합니다.

<a name="troubleshooting"></a>문제 해결
===============

**로그 사용 설정**

그래프에 문제가 있는 경우 로그를 사용하여 문제를 현지화할 수 있습니다. 따라서 추적을 [일반 커넥터와 같은 방식으로](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx) 사용할 수 있습니다. 또는 다음을 `miiserver.exe.config`(`system.diagnostics/sources` 섹션 내)에 추가하여 사용할 수 있습니다.


\<source name="ConnectorsLog" switchValue="Verbose"\>

\<listeners\>

>   \<add initializeData="ConnectorsLog"   type="System.Diagnostics.EventLogTraceListener, System, Version=4.0.0.0,   Culture=neutral, PublicKeyToken=b77a5c561934e089"   name="ConnectorsLogListener" traceOutputOptions="LogicalOperationStack,   DateTime, Timestamp, Call stack" /\>

\<remove name="Default" /\>

\</listeners\>

\</source\>

>[!NOTE]
>‘별도의 프로세스로 이 관리 에이전트 실행'이 활성화된 경우 `miiserver.exe.config` 대신 `dllhost.exe.config`를 사용해야 합니다.

**액세스 토큰이 만료됨 오류**

커넥터에서 HTTP 오류 401 권한 없음, “액세스 토큰이 만료되었습니다.” 메시지를 반환할 수 있습니다.

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

그림 7. “액세스 토큰이 만료되었습니다.” 오류

이 문제의 원인은 Azure 측의 액세스 토큰 수명에 대한 구성일 수 있습니다. 기본적으로 액세스 토큰은 1시간 후에 만료됩니다. 만료 시간을 늘리려면 [이 문서](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes)를 참조하세요.

[Azure AD PowerShell Module Public Preview release](https://www.powershellgallery.com/packages/AzureADPreview)(Azure AD PowerShell 모듈 공개 미리 보기 릴리스) 사용 예

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

New-AzureADPolicy -Definition \@('{"TokenLifetimePolicy":{"Version":1, **"AccessTokenLifetime":"5:00:00"**}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault \$true -Type "TokenLifetimePolicy"

<a name="next-steps"></a>다음 단계
----------
- [그래프 탐색기(HTTP 호출 문제 해결에 적합)]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Versioning, support, and breaking change policies for Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)(Microsoft Graph의 버전 관리, 지원 및 주요 변경 정책)
- [Microsoft Graph용 Microsoft Identity Manager 커넥터 다운로드](http://go.microsoft.com/fwlink/?LinkId=717495)

<a name="scenario-specific-guides"></a>시나리오별 지침
----------------------------------
[MIM B2B 종단 간 배포]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
