---
title: Microsoft Graph용 Microsoft Identity Manager 관리 에이전트 | Microsoft Docs
author: fimguy
description: Microsoft Graph(미리 보기)는 외부 사용자 AD 계정 수명 주기 관리입니다. 이 시나리오에서 조직은 Azure AD 디렉터리에 게스트를 초대하고 온-프레미스 Windows 통합 인증 또는 Kerberos 기반 응용 프로그램에 대한 액세스 권한을 이 게스트에게 부여하려고 합니다.
keywords: ''
ms.author: davidste
manager: bhu
ms.date: 04/25/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: ac11a4dfb23944d50dbbcf0b0d70c915f186c159
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34479165"
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy-public-preview"></a>Azure 응용 프로그램 프록시(공개 미리 보기)를 사용하여 MIM(Microsoft Identity Manager) 2016 SP1과 Azure AD B2B(기업 간) 공동 작업
============================================================================================================================

미리 보기의 초기 시나리오는 외부 사용자 AD 계정 수명 주기 관리입니다.   이 시나리오에서 조직은 Azure AD 디렉터리에 게스트를 초대하고 [Azure AD 응용 프로그램](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) 프록시 또는 기타 게이트웨이 메커니즘을 통해 온-프레미스 Windows 통합 인증 또는 Kerberos 기반 응용 프로그램에 대한 액세스 권한을 이 게스트에게 부여하려고 합니다. Azure AD 응용 프로그램 프록시를 사용하려면 식별 및 위임용으로 각 사용자에게 고유한 AD DS 계정이 있어야 합니다.

## <a name="scenario-specific-supported-guidance"></a>시나리오별 지원 가이드

이 시나리오에서 조직은 Azure AD 디렉터리에 게스트를 초대하고, 온-프레미스 Windows에 대한 액세스 권한을 이 게스트에게 부여하려고 합니다. [Azure AD 응용 프로그램](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) 프록시 또는 기타 게이트웨이 메커니즘을 통해 온-프레미스 Windows 통합 인증 또는 Kerberos 기반 응용 프로그램에 대한 액세스 권한을 이 게스트에게 부여하려고 합니다. Azure AD 응용 프로그램 프록시를 사용하려면 식별 및 위임용으로 각 사용자에게 고유한 AD DS 계정이 있어야 합니다.

MIM 및 Azure 응용 프로그램 프록시를 사용한 B2B 구성에서의 몇 가지 가정

-   [그래프 관리 에이전트](microsoft-identity-manager-2016-connector-graph.md)를 이미 설치했습니다.

-   사용자 및 그룹을 Azure AD로 동기화하기 위해 온-프레미스 AD 및 Azure AD Connect를 설정했습니다.

    -   [Azure AD Connect](http://robsgroupsblog.com/blog/how-to-write-back-an-office-group-in-azure-active-directory-to-a-mail-enabled-security-group-in-an-on-premises-active-directory)를 사용하여 응용 프로그램 액세스를 제어하는 Office 그룹

-   응용 프로그램 프록시 커넥터 및 커넥터 그룹을 이미 설정했습니다. 그렇지 않은 경우 [여기](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector)를 방문하여 설치하고 구성할 수 있습니다.

-   Azure AD 앱 프록시를 통해 Windows 통합 인증 또는 개별 AD 계정을 사용하는 응용 프로그램을 하나 이상 게시했습니다.

-   Azure AD <https://docs.microsoft.com/azure/active-directory/active-directory-b2b-self-service-portal>에서 만든 하나 이상의 게스트를 초대하거나 초대했습니다.

-   Microsoft Identity Manager가 설치되어 있으며 서비스 및 포털과 Active Directory 관리 에이전트의 기본 구성이 완료되었습니다.
    <https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-deploy>

## <a name="b2b-end-to-end-deployment"></a>B2B 종단 간 배포

시나리오

Contoso Pharmaceuticals는 R&D 부서의 일부로 Trey Research Inc.를 운영합니다. Trey Research 직원은 Contoso Pharmaceuticals에서 제공하는 연구 보고 응용 프로그램에 액세스해야 합니다.

-   Contoso Pharmaceuticals는 독립 테넌트에 있으며 사용자 지정 도메인을 구성했습니다.

-   외부 사용자를 Contoso Pharmaceuticals 테넌트로 초대했습니다.
    이 사용자는 초대를 수락했으며 공유되는 리소스에 액세스할 수 있습니다.

-   앱 프록시를 통해 응용 프로그램을 게시했으며, 이 시나리오에서 MIM 서비스 및 포털을 사용하는 예로 게스트 사용자가 MIM 프로세스 Example Helpdesk 시나리오에 참여합니다.

## <a name="create-the-graph-management-agent"></a>그래프 관리 에이전트 만들기

참고: 커넥터를 만들기 전에 [그래프 관리 에이전트](microsoft-identity-manager-2016-connector-graph.md)를 검토했는지 확인하세요.

동기화 서비스 관리자 UI에서 **커넥터**와 **만들기**를 선택합니다.
**Graph(Microsoft)** 를 선택하고 설명하는 이름을 지정합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>연결

연결 페이지에서 Graph API 버전 프로덕션은 **V 1.0**으로, 비프로덕션은 **베타**로 지정해야 합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="capabilities"></a>기능

전역 매개 변수 페이지에서 델타 변경 로그 및 추가 LDAP 기능에 대한 DN을 구성합니다. 이 페이지는 LDAP 서버에서 제공하는 정보로 미리 채워져 있습니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="global-parameters"></a>전역 매개 변수

전역 매개 변수 페이지에서 델타 변경 로그 및 추가 LDAP 기능에 대한 DN을 구성합니다. 이 페이지는 LDAP 서버에서 제공하는 정보로 미리 채워져 있습니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>프로비저닝 계층 구조 구성

이 페이지는 DN 구성 요소(예: OU)를 프로비저닝되는 개체 형식(예: organizationalUnit)에 매핑하는 데 사용됩니다. 이 기본값을 그대로 두고 [다음]을 클릭합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>파티션 및 계층 구조 구성

파티션 및 계층 구조 페이지에서 가져오고 내보내려는 개체가 있는 모든 네임스페이스를 선택합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>개체 유형 선택

파티션 및 계층 구조 페이지에서 가져오고 내보내려는 개체가 있는 모든 네임스페이스를 선택합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>특성 선택

[특성 선택] 화면에서 B2B 사용자를 관리하는 데 필요한 특성을 선택합니다. “id” 특성이 필요합니다.

-   ID

-   displayName

-   mail

-   givenName

-   surname

-   userPrincipalName

-   userType

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>앵커 구성

[앵커 구성]을 사용한 앵커 구성은 필수 단계입니다. 기본적으로 id 특성을 매핑에 사용합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>커넥터 필터 구성

[커넥터 필터 구성] 페이지에서 특성 필터를 기반으로 개체를 필터링할 수 있습니다. B2B에 대한 이 시나리오의 목표는 게스트와 동일하고 멤버가 아닌 userType의 사용자만 가져오는 것입니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>조인 및 프로젝션 규칙 구성

조인 및 프로젝션 규칙 구성은 동기화 규칙에 의해 처리되며, 커넥터 자체에 대한 조인 및 프로젝션을 식별하지 않아도 됩니다. 기본값을 그대로 두고 [확인]을 클릭합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>특성 흐름 구성

조인 및 프로젝션과 마찬가지로 특성 흐름도 나중에 작성할 동기화 규칙에 의해 처리되므로 정의할 필요가 없습니다. 기본값을 그대로 두고 [확인]을 클릭합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>프로비전 해제 구성

프로비전 해제 구성을 사용하면 메타버스 개체가 삭제되는 경우 개체를 삭제할 수 있습니다. 이 테스트에서는 메타버스 개체를 Azure에 유지하는 것이 목표이므로 디스커넥터로 지정하고 가져오기 전용 테스트이므로 Azure로 아무것도 내보내지 않습니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>확장 구성

이 관리 에이전트에서 확장 구성은 옵션이지만 동기화 규칙을 사용하기 때문에 필요하지 않습니다. 이전에 특성 흐름에서 사전 규칙으로 결정한 경우에는 정의가 옵션이 됩니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="creating-mim-service-synchronization-rules"></a>MIM 서비스 동기화 규칙 만들기

아래 단계에서는 B2B 게스트 계정 및 특성 흐름의 매핑을 시작합니다. 여기서는 이미 Active Directory 관리 에이전트를 구성하고 FIM MA가 MIM 서비스 및 포털과 통신하도록 구성되어 있다고 가정합니다.

동기화 규칙을 만들기 전에 MV 디자이너를 사용하여 person 개체에 연결된 userPrincipalName이라는 특성을 만들어야 합니다.

동기화 클라이언트에서 메타버스 디자이너를 선택합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

그런 다음, Person 개체 형식을 선택합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

다음으로 [작업]에서 [특성 추가]를 클릭합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

마지막으로 다음 세부 정보를 작성합니다.

특성 이름: **userPrincipalName**

특성 유형: **문자열(인덱싱 가능)**

인덱싱됨 = **True**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

다음 단계에서는 FIM 서비스 관리 에이전트 및 Active Directory Domain Services 관리 에이전트의 최소 구성을 수행합니다.

구성에 대한 자세한 내용은 How Do I Provision Users to AD DS(AD DS로 사용자를 프로비전하는 방법)(<https://technet.microsoft.com/library/ff686263(v=ws.10).aspx>)에서 확인할 수 있습니다.

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>동기화 규칙: Azure Active Directory에서 MV 및 동기화 서비스 메타버스로 게스트 사용자 가져오기<br>

MIM 서비스 및 포털로 이동하여 동기화 규칙을 선택하고 새로 만들기를 클릭합니다.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png) “FIM에서 리소스 만들기”를 선택합니다. ![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

흐름 규칙:

| **초기 흐름만** | **존재 테스트로 사용** | **흐름(FIM 값 ⇒ 대상 특성)**                          |
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

이 동기화 규칙은 Active Directory에 사용자를 만듭니다.

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
| **Y**                 |                           | ["CN="+uid+",OU=B2BGuest,DC=scontoso,DC=com"⇒dn](javascript:void(0);) |
| **Y**                 |                           | [RandomNum(0,999)+userPrincipalName⇒unicodePwd](javascript:void(0);)  |
| **Y**                 |                           | [262656⇒userAccountControl](javascript:void(0);)                      |

### <a name="synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>동기화 규칙: MIM 로그인을 허용하도록 B2B 게스트 사용자 개체 SID 가져오기 

이 동기화 규칙은 Active Directory에 사용자를 만듭니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)

마지막으로 사용자를 초대하고 다음 순서로 관리를 실행합니다.

-   MIMMA 관리 에이전트의 전체 가져오기 및 동기화

-   ADMA_SCONTOSO_B2B 관리 에이전트의 전체 가져오기 및 동기화

-   B2B 그래프 관리 에이전트의 전체 가져오기 및 동기화

-   ADMA_SCONTOSO_B2B 관리 에이전트의 내보내기, 델타 가져오기 및 동기화

-   MIMMA 관리 에이전트의 내보내기, 델타 가져오기 및 동기화

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)

## <a name="finally-application-proxy-with-b2b-guest-and-logging-into-mim"></a>마지막으로 B2B 게스트를 사용하는 응용 프로그램 프록시 및 MIM에 로그인

이제 MIM에서 동기화 규칙을 만들었습니다. 앱 프록시 구성에서 클라우드 원칙을 사용하여 앱 프록시에서 KCD를 허용하도록 정의합니다.
다음으로 사용자를 수동으로 추가하여 사용자 및 그룹을 관리합니다. 사용자를 만든 다음에야 표시하는 옵션을 MIM에서 사용하여 office 그룹에 게스트를 추가합니다. 프로비저닝한 다음에는 이 문서에 설명되지 않은 추가 구성이 좀 더 필요합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)

| **초기 흐름만** | **존재 테스트로 사용** | **흐름(FIM 값 ⇒ 대상 특성)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName](javascript:void(0);)                     |
|                       |                           | ["CONTOSO"⇒domain](javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid](javascript:void(0);)                                      |

모두 구성된 후

마지막으로 B2B 사용자가 로그인하여 응용 프로그램을 확인합니다.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>다음 단계
----------

[How Do I Provision Users to AD DS](https://technet.microsoft.com/library/ff686263(v=ws.10).aspx)(AD DS로 사용자를 프로비전하는 방법)

[Functions Reference for FIM 2010](https://technet.microsoft.com/library/ff800820(v=ws.10).aspx)(FIM 2010에 대한 함수 참조)

[온-프레미스 응용 프로그램에 대한 보안 원격 액세스를 제공하는 방법](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

[Microsoft Graph용 Microsoft Identity Manager 관리 에이전트(미리 보기) 다운로드](http://go.microsoft.com/fwlink/?LinkId=717495)
