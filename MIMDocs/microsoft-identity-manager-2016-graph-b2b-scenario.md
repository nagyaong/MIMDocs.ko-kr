---
title: B2B에 대한 Microsoft Graph용 Microsoft Identity Manager 커넥터 구성 | Microsoft Docs
author: billmath
description: Microsoft Graph 커넥터는 외부 사용자 AD 계정 수명 주기 관리입니다. 이 시나리오에서 조직은 Azure AD 디렉터리에 게스트를 초대하고 온-프레미스 Windows 통합 인증 또는 Kerberos 기반 응용 프로그램에 대한 액세스 권한을 이 게스트에게 부여하려고 합니다.
keywords: ''
ms.author: billmath
manager: mtillman
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 139c58510117ad422529a4ff0facd23040023713
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358774"
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy"></a>Azure 응용 프로그램 프록시를 사용하여 MIM(Microsoft Identity Manager) 2016 SP1과 Azure AD B2B(기업 간) 공동 작업
============================================================================================================================

초기 시나리오는 외부 사용자 AD 계정 수명 주기 관리입니다.   이 시나리오에서 조직은 Azure AD 디렉터리에 게스트를 초대하고 [Azure AD 응용 프로그램](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) 프록시 또는 기타 게이트웨이 메커니즘을 통해 온-프레미스 Windows 통합 인증 또는 Kerberos 기반 응용 프로그램에 대한 액세스 권한을 이 게스트에게 부여하려고 합니다. Azure AD 응용 프로그램 프록시를 사용하려면 식별 및 위임용으로 각 사용자에게 고유한 AD DS 계정이 있어야 합니다.

## <a name="scenario-specific-guidance"></a>시나리오별 지침

MIM 및 Azure AD 응용 프로그램 프록시를 사용한 B2B 구성에서의 몇 가지 가정:

-   온-프레미스 AD를 이미 배포했고, Microsoft Identity Manager가 설치되었으며 MIM 서비스의 기본 구성, MIM 포털, AD MA(Active Directory 관리 에이전트) 및FIM MA(FIM 관리 에이전트)가 설치되었습니다.
    <https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-deploy>

-   [Graph 커넥터](microsoft-identity-manager-2016-connector-graph.md)를 다운로드하고 설치하는 방법에 대한 문서의 지침을 이미 수행했습니다.

-   사용자 및 그룹을 Azure AD로 동기화하기 위해 Azure AD Connect를 구성했습니다.

-   응용 프로그램 제어를 위해 Office 그룹을 [온-프레미스 AD DS에 다시](http://robsgroupsblog.com/blog/how-to-write-back-an-office-group-in-azure-active-directory-to-a-mail-enabled-security-group-in-an-on-premises-active-directory) 동기화하도록 Azure AD Connect를 구성했습니다.

-   응용 프로그램 프록시 커넥터 및 커넥터 그룹을 이미 설정했습니다. 그렇지 않은 경우 [여기](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector)를 방문하여 설치하고 구성할 수 있습니다.

-   Azure AD 앱 프록시를 통해 Windows 통합 인증 또는 개별 AD 계정을 사용하는 응용 프로그램을 이미 하나 이상 게시했습니다.

-   Azure AD에 하나 이상의 사용자가 만들어지는 하나 이상의 게스트를 초대하거나 초대했습니다. <https://docs.microsoft.com/azure/active-directory/active-directory-b2b-self-service-portal>



## <a name="b2b-end-to-end-deployment-example-scenario"></a>B2B 종단 간 배포 예제 시나리오

이 가이드는 다음 시나리오를 기반으로 합니다.

Contoso Pharmaceuticals는 R&D 부서의 일부로 Trey Research Inc.를 운영합니다. Trey Research 직원은 Contoso Pharmaceuticals에서 제공하는 연구 보고 응용 프로그램에 액세스해야 합니다.

-   Contoso Pharmaceuticals는 고유한 테넌트에 있으며 사용자 지정 도메인을 구성했습니다.

-   외부 사용자를 Contoso Pharmaceuticals 테넌트로 초대했습니다.
    이 사용자는 초대를 수락했으며 공유되는 리소스에 액세스할 수 있습니다.

-   Contoso Pharmaceuticals는 앱 프록시를 통해 응용 프로그램을 게시했습니다. 이 시나리오에서 예제 응용 프로그램은 MIM 포털입니다. 이렇게 하면 게스트 사용자는 도움말 데스크 시나리오와 같은 MIM 프로세스에 참여하거나 MIM의 그룹에 대한 액세스를 요청할 수 있습니다.


## <a name="configure-ad-and-azure-ad-connect-to-exclude-users-added-from-azure-ad"></a>Azure AD에서 추가된 사용자를 제외하도록 AD 및 Azure AD Connect를 구성합니다.

기본적으로 Azure AD Connect는 Active Directory에서 관리자가 아닌 사용자는 Azure AD로 동기화되어야 한다고 가정합니다.  Azure AD Connect에서 온-프레미스 AD의 사용자와 일치하는 Azure AD에서 기존 사용자를 찾는 경우 Azure AD Connect는 두 개의 계정을 일치시키고 사용자의 이전 동기화라고 가정하며, 온-프레미스 AD에 권한을 부여합니다.  그러나 이 기본 동작은 사용자 계정이 Azure AD에서 비롯되는 B2B 흐름에 적합하지 않습니다. 

따라서 Azure AD에서 MIM에 의해 AD DS로 가져온 사용자는 Azure AD가 해당 사용자를 Azure AD에 다시 동기화하도록 시도하지 않는 방식으로 저장되어야 합니다.
이 작업을 수행하는 한 가지 방법은 AD DS에서 새 조직 구성 단위를 만들고, 해당 조직 구성 단위를 제외하도록 Azure AD Connect를 구성하는 것입니다.  

[Azure AD Connect 동기화: 필터링 구성](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-configure-filtering)에서 자세한 정보를 찾을 수 있습니다. 
 

## <a name="create-the-azure-ad-application"></a>Azure AD 응용 프로그램 만들기 


참고: MIM 동기화에서 그래프 커넥터에 대한 관리 에이전트를 만들기 전에 [Graph 커넥터](microsoft-identity-manager-2016-connector-graph.md) 배포에 대한 가이드를 검토했고, 클라이언트 ID 및 비밀을 사용하여 응용 프로그램을 만들었는지 확인합니다.
응용 프로그램에 이러한 사용 권한 중 하나 이상에 대한 권한이 부여되었는지 확인합니다. `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` 또는 `Directory.ReadWrite.All` 

## <a name="create-the-new-management-agent"></a>새 관리 에이전트 만들기


동기화 서비스 관리자 UI에서 **커넥터**와 **만들기**를 선택합니다.
**Graph(Microsoft)** 를 선택하고 설명하는 이름을 지정합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>연결

연결 페이지에서 Graph API 버전을 지정해야 합니다. 프로덕션 준비 PAI는 **V 1.0**이며, 비프로덕션은 **베타**입니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="global-parameters"></a>전역 매개 변수

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>프로비저닝 계층 구조 구성

이 페이지는 DN 구성 요소(예: OU)를 프로비저닝되는 개체 형식(예: organizationalUnit)에 매핑하는 데 사용됩니다. 이 시나리오에는 필요하지 않으므로 기본값을 그대로 두고 다음을 클릭합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>파티션 및 계층 구조 구성

파티션 및 계층 구조 페이지에서 가져오고 내보내려는 개체가 있는 모든 네임스페이스를 선택합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>개체 유형 선택

개체 유형 페이지에서 가져오려는 개체 유형을 선택합니다. 최소한 '사용자'를 선택해야 합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>특성 선택

특성 선택 화면에서, AD에서 B2B 사용자를 관리하는 데 필요한 특성을 Azure AD에서 선택합니다. “ID” 특성이 필요합니다.  `userPrincipalName` 및 `userType` 특성은 이 구성의 뒷부분에서 사용됩니다.  다음을 포함한 다른 특성은 선택적입니다.

-   `displayName`

-   `mail`

-   `givenName`

-   `surname`

-   `userPrincipalName`

-   `userType`

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>앵커 구성

앵커 구성 화면에서 앵커 특성 구성은 필수 단계입니다. 기본적으로 ID 특성을 사용자 매핑에 사용합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>커넥터 필터 구성

커넥터 필터 구성 페이지에서 MIM을 통해 특성 필터를 기반으로 개체를 필터링할 수 있습니다. B2B에 대한 이 시나리오에서 목표는 `member`와 동일한 사용자 유형이 있는 사용자가 아닌 `Guest`와 동일한 `userType` 특성의 값이 있는 사용자를 가져오는 것입니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>조인 및 프로젝션 규칙 구성

이 가이드는 동기화 규칙을 만든다고 가정합니다.  조인 및 프로젝션 규칙 구성은 동기화 규칙에 의해 처리되므로 커넥터 자체에 대한 조인 및 프로젝션을 식별하지 않아도 됩니다. 기본값을 그대로 두고 [확인]을 클릭합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>특성 흐름 구성

이 가이드는 동기화 규칙을 만든다고 가정합니다.  MIM 동기화에서 특성 흐름은 나중에 만들어지는 동기화 규칙에 의해 처리되므로 이를 정의하는 데 프로젝션이 필요하지 않습니다. 기본값을 그대로 두고 [확인]을 클릭합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>프로비전 해제 구성

프로비전 해제를 구성하는 설정을 통해 메타버스 개체가 삭제되는 경우 개체를 삭제하도록 MIM 동기화를 구성할 수 있습니다. 이 시나리오에서는 메타버스 개체를 Azure AD에 유지하는 것이 목표이므로 디스커넥터로 지정합니다. 이 시나리오에서는 Azure AD로 아무것도 내보내지 않으며, 커넥터는 가져오기 전용으로 구성되었습니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>확장 구성

이 관리 에이전트에서 확장 구성은 옵션이지만 동기화 규칙을 사용하기 때문에 필요하지 않습니다. 이전에 특성 흐름에서 사전 규칙을 사용하도록 결정한 경우에는 규칙 확장을 정의하는 옵션이 있습니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="extending-the-metaverse-schema"></a>메타버스 스키마 확장


동기화 규칙을 만들기 전에 MV 디자이너를 사용하여 person 개체에 연결된 userPrincipalName이라는 특성을 만들어야 합니다.

동기화 클라이언트에서 메타버스 디자이너를 선택합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

그런 다음, Person 개체 형식을 선택합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

다음으로 [작업]에서 [특성 추가]를 클릭합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

그런 다음, 다음 세부 정보를 완료합니다.

특성 이름: **userPrincipalName**

특성 유형: **문자열(인덱싱 가능)**

인덱싱됨 = **True**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

## <a name="creating-mim-service-synchronization-rules"></a>MIM 서비스 동기화 규칙 만들기

아래 단계에서는 B2B 게스트 계정 및 특성 흐름의 매핑을 시작합니다. 여기서는 이미 Active Directory MA를 구성했고 FIM MA가 MIM 서비스 및 포털에 사용자를 가져오도록 구성되어 있다고 가정합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

다음 단계에서는 FIM MA 및 AD MA에 최소 구성을 추가해야 합니다.

구성에 대한 자세한 내용은 How Do I Provision Users to AD DS(AD DS로 사용자를 프로비전하는 방법)(<https://technet.microsoft.com/library/ff686263(v=ws.10).aspx>)에서 확인할 수 있습니다.

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>동기화 규칙: Azure Active Directory에서 MV 및 동기화 서비스 메타버스로 게스트 사용자 가져오기<br>

MIM 포털로 이동하고, 동기화 규칙을 선택하고, 새로 만들기를 클릭합니다.  그래프 커넥터를 통해 B2B 흐름에 대한 인바운드 동기화 규칙을 만듭니다.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png)

관계 조건 단계에서 "FIM에서 리소스 만들기"를 선택해야 합니다.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

다음 인바운드 특성 흐름 규칙을 구성합니다.  이 시나리오의 뒷부분에서 사용되므로 `accountName`, `userPrincipalName` 및 `uid` 특성을 채워야 합니다.

| **초기 흐름만** | **존재 테스트로 사용** | **흐름(원본 값 ⇒ FIM 특성)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [displayName⇒displayName](javascript:void(0);)                        |
|                       |                           | [Left(id,20)⇒accountName](javascript:void(0);)                        |
|                       |                           | [id⇒uid](javascript:void(0);)                                         |
|                       |                           | [userType⇒employeeType](javascript:void(0);)                          |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [surname⇒sn](javascript:void(0);)                                     |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
|                       |                           | [id⇒cn](javascript:void(0);)                                          |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [mobilePhone⇒mobilePhone](javascript:void(0);)                        |

### <a name="synchronization-rule-create-guest-user-account-to-active-directory"></a>동기화 규칙: Active Directory에 대한 게스트 사용자 계정 만들기 

이 동기화 규칙은 Active Directory에 사용자를 만듭니다.  `dn`에 대한 흐름은 사용자를 Azure AD Connect에서 제외된 조직 구성 단위에 배치해야 합니다.  또한 AD 암호 정책을 충족하도록 `unicodePwd`에 대한 흐름을 업데이트합니다. 사용자는 암호를 알 필요가 없습니다.  `userAccountControl`에 대한 `262656` 값은 플래그 `SMARTCARD_REQUIRED` 및 `NORMAL_ACCOUNT`를 인코딩합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/3463e11aeb9fb566685e775d4e1b825c.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e53c7ac0f376382d890e79dad597c284.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/1c4fad7aa68dc9697fda8f811e9ad37b.png)

흐름 규칙:

| **초기 흐름만** | **존재 테스트로 사용** | **흐름(FIM 값 ⇒ 대상 특성)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [accountName⇒sAMAccountName](javascript:void(0);)                     |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [sn⇒sn](javascript:void(0);)                                          |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
| **Y**                 |                           | ["CN="+uid+",OU=B2BGuest,DC=contoso,DC=com"⇒dn](javascript:void(0);) |
| **Y**                 |                           | [RandomNum(0,999)+userPrincipalName⇒unicodePwd](javascript:void(0);)  |
| **Y**                 |                           | [262656⇒userAccountControl](javascript:void(0);)                      |

### <a name="optional-synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>선택 사항: 동기화 규칙: MIM 로그인을 허용하도록 B2B 게스트 사용자 개체 SID 가져오기 

이 인바운드 동기화 규칙은 Active Directory에서 MIM으로 사용자의 SID 특성을 다시 가져오므로 사용자는 MIM 포털에 액세스할 수 있습니다.  MIM 포털에서 사용자는 MIM 서비스 데이터베이스에 `samAccountName`, `domain` 및 `objectSid` 특성을 채워야 합니다.

`objectSid` 특성은 MIM에서 사용자를 만들 때 AD에서 자동으로 설정되므로 원본 외부 시스템을 `ADMA`로 구성합니다.
 
사용자가 MIM 서비스에서 만들어지도록 구성한 경우 직원 SSPR 관리 정책 규칙을 위한 모든 집합의 범위에 있지 않도록 합니다.  B2B 흐름에 의해 생성된 사용자를 제외하도록 집합 정의를 변경해야 합니다. 

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)


| **초기 흐름만** | **존재 테스트로 사용** | **흐름(원본 값 ⇒ FIM 특성)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName](javascript:void(0);)                     |
|                       |                           | ["CONTOSO"⇒domain](javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid](javascript:void(0);)                                      |


## <a name="run-the-synchronization-rules"></a>동기화 규칙 실행

다음으로 사용자를 초대한 다음, 다음 순서로 관리 에이전트 동기화 규칙을 실행합니다.

-   `MIMMA` 관리 에이전트의 전체 가져오기 및 동기화  이렇게 하면 MIM 동기화에 최신 동기화 규칙이 구성됩니다.

-   `ADMA` 관리 에이전트의 전체 가져오기 및 동기화  이렇게 하면 MIM 및 Active Directory가 일치합니다.  이 시점에서 게스트에 대한 보류 중인 내보내기가 아직 없습니다.

-   B2B 그래프 관리 에이전트의 전체 가져오기 및 동기화  게스트 사용자를 메타버스로 가져옵니다.  이 시점에서 하나 이상의 계정이 `ADMA`에 대한 내보내기를 보류합니다.  보류 중인 내보내기가 없는 경우 게스트 사용자를 커넥터 공간으로 가져왔고, AD 계정을 제공하도록 규칙이 구성되었는지 확인합니다.

-   `ADMA` 관리 에이전트의 내보내기, 델타 가져오기 및 동기화  내보내기에 실패한 경우 규칙 구성을 확인하고 누락된 스키마 요구 사항이 있는지 확인합니다. 

-   `MIMMA` 관리 에이전트의 내보내기, 델타 가져오기 및 동기화  완료되면 더 이상 보류 중인 내보내기가 없습니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)


## <a name="optional-application-proxy-for-b2b-guests-logging-into-mim-portal"></a>선택 사항: MIM 포털로 로그인하는 B2B 게스트에 대한 응용 프로그램 프록시

이제 MIM에서 동기화 규칙을 만들었습니다. 앱 프록시 구성에서 클라우드 원칙을 사용하여 앱 프록시에서 KCD를 허용하도록 정의합니다.
다음으로 사용자를 수동으로 추가하여 사용자 및 그룹을 관리합니다. 사용자를 만든 다음에야 표시하는 옵션을 MIM에서 사용하여 office 그룹에 게스트를 추가합니다. 프로비저닝한 다음에는 이 문서에 설명되지 않은 추가 구성이 좀 더 필요합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)


모두 구성되면 B2B 사용자가 로그인하여 응용 프로그램을 확인합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>다음 단계
----------

[How Do I Provision Users to AD DS](https://technet.microsoft.com/library/ff686263(v=ws.10).aspx)(AD DS로 사용자를 프로비전하는 방법)

[Functions Reference for FIM 2010](https://technet.microsoft.com/library/ff800820(v=ws.10).aspx)(FIM 2010에 대한 함수 참조)

[온-프레미스 응용 프로그램에 대한 보안 원격 액세스를 제공하는 방법](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

[Microsoft Graph용 Microsoft Identity Manager 커넥터 다운로드](http://go.microsoft.com/fwlink/?LinkId=717495)
