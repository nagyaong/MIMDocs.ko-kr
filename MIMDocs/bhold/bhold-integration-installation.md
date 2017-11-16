---
title: "BHOLD FIM/MIM 통합 설치 | Microsoft Docs"
description: "BHOLD 통합 모듈은 MIM 및 FIM에 셀프 서비스 역할 관리를 추가합니다."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: ef68de19bd0eabd6d9203469ecc991d496f05846
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/14/2017
---
# <a name="bhold-fimmim-integration-installation"></a>BHOLD FIM/MIM 통합 설치

BHOLD FIM 통합 모듈은 MIM(Microsoft Identity Manager)에 셀프 서비스 역할 관리를 추가하여 사용자가 추가 역할을 요청할 수 있고 해당 역할에서 수행할 수 있는 사용자에게 적용할 수 있게 합니다. BHOLD FIM 통합 모듈은 FIM 포털을 확장하여 전체 FIM 관리의 일환으로 사용자의 역할을 쉽게 관리할 수 있습니다. 이 항목에서는 BHOLD FIM 통합 모듈을 설치하고 사용할 수 있도록 네트워크 인프라를 구성해야 하는 방법을 설명합니다. 또한 BHOLD FIM 통합 모듈을 설치한 후에 필요한 BHOLD FIM 통합 모듈 및 구성을 설치하는 방법을 다룹니다.

## <a name="bhold-fim-integration-installation-requirements"></a>BHOLD FIM 통합 설치 요구 사항

BHOLD FIM 통합 모듈은 FIM 포털 및 FIM 서비스를 확장하여 사용자가 FIM 포털 내에서 해당 역할을 관리하도록 허용합니다. 이러한 이유로 BHOLD FIM 통합 모듈을 설치하기 전에 반드시 BHOLD Core 모듈 및 필요한 FIM 기능을 설치하고 구성해야 합니다.
BHOLD FIM 통합 모듈을 설치하기 전에 컴퓨터에 설치되어야 하는 소프트웨어 구성 요소는 다음과 같습니다.

- Microsoft Identity Manager 2016 포털 및 서비스
- Microsoft Silverlight 3 이상
- IIS(인터넷 정보 서비스) 및 ASP.NET
- Microsoft Silverlight 도구

또한 BHOLD Core 및 액세스 관리 커넥터 모듈은 환경의 서버에 배포되어야 하고 FIM은 하나 이상의 BHOLD 관리 에이전트로 구성되어야 합니다. BHOLD Core 모듈을 설치하고 구성하는 방법에 대한 정보는 [BHOLD Core 설치](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx)를 참조하세요. 액세스 관리 커넥터 모듈을 설치하고 사용하는 방법에 대한 정보는 [액세스 관리 커넥터 설치](https://technet.microsoft.com/en-us/library/jj874042(v=ws.10).aspx) 및 [테스트 랩 가이드: BHOLD 액세스 관리 커넥터](https://technet.microsoft.com/en-us/library/jj853085(v=ws.10).aspx)를 참조하세요.

>[!IMPORTANT]
FIM 서비스 데이터베이스의 이름은 FIMService여야 합니다. FIM이 기본 FIM 서비스 데이터베이스 이름으로 설치되지 않은 경우 BHOLD FIM 통합 설정에 실패합니다.

## <a name="before-you-begin"></a>시작하기 전에

BHOLD FIM 통합 모듈의 설치를 시작하기 전에 C: 디스크 드라이브(C:\BHOLD)의 루트 디렉터리에 BHOLD 디렉터리를 만들어야 합니다.

또한 BHOLD FIM 통합 설치 마법사가 설치를 완료하는 데 필요한 정보를 제공하도록 준비해야 합니다. 다음 워크시트를 사용하면 해당 정보를 기록하여 필요한 경우 제공할 수 있습니다.

### <a name="bholdfim-account-settings"></a>BHOLDFim 계정 설정

| **항목**                            | **설명**                                                                                                                                                                                                               | **값**                                                                                                                                                                                                                                                                                                            |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **도메인에서 보안 공급자 사용** | 이 항목을 선택하면 Active Directory Domain Services 보안이 BHOLD Core에 대한 액세스 권한을 제어하도록 지정합니다.                                                                                                                    | 해당 확인란을 선택합니다. **중요:** 이 확인란을 선택하지 않으면 설치에 실패합니다.                                                                                                                                                                                                                   |
| **도메인**                          | BHOLD Core를 설치할 때 만든 **서비스 계정**을 포함하는 도메인을 지정합니다. 자세한 내용은 [BHOLD Core 설치](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx)를 참조하세요. | 도메인 이름은 마법사에 의해 자동으로 제공됩니다. 올바르지 않은 경우 이름을 변경합니다. **중요:** FQDN(정규화된 도메인 이름)이 아닌 NetBIOS(짧음) 이름을 사용하여 도메인 이름을 지정합니다. 예를 들어, 도메인의 FQDN이 fabrikam.com인 경우 도메인 이름을 FABRIKAM으로 지정합니다. |
| **사용자 이름**                        | BHOLD Core 서비스 사용자 계정의 로그 이름을 지정합니다.                                                                                                                                                              | 여기에 사용자 계정 이름을 입력합니다.                                                                                                                                                                                                                                                                                    |
| **암호**                        | 서비스 사용자 계정의 암호를 지정합니다.                                                                                                                                                                           | 여기에 암호를 입력합니다. **중요:** 숨겨진 안전한 위치에 이 암호를 보관해야 합니다.                                                                                                                                                                                                                  |

### <a name="fim-service-settings"></a>FIM 서비스 설정

| **항목**            | **설명**                                                                                                                                                                                                                               | **값**                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **사용자**            | FIM에 대한 관리자 권한이 있는 계정의 로그온 이름을 지정합니다. BHOLD Core의 루트 사용자와 연결된 계정을 사용하지 않는 것이 좋습니다(기본적으로 BHOLD Core를 설치하는 데 사용되는 계정). | 여기에 사용자 계정 이름을 입력합니다.                                                                   |
| **암호**        | FIM 관리 사용자 계정의 암호를 지정합니다.                                                                                                                                                                                 | 여기에 암호를 입력합니다. **중요:** 숨겨진 안전한 위치에 이 암호를 보관해야 합니다. |
| **FIM 데이터베이스**    | FIM 서비스 데이터베이스의 이름을 지정합니다.                                                                                                                                                                                               | FIMService                                                                                          |
| **웹 사이트 P/포트** | FIM 포털 서버와 웹 사이트 포트의 이름 또는 IP 주소를 지정합니다.                                                                                                                                                               | 서버 이름 또는 주소와 포트를 여기에 입력합니다.                                                     |

### <a name="bhold-core-connection"></a>BHOLD Core 연결

| **항목**               | **설명**                                                                                                                                                                                                                                                                                                                                                                               | **값**                                                                                           |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **도메인**             | 아래에서 **사용자**에 지정된 계정의 도메인 이름을 지정합니다. NetBIOS(짧음) 양식으로 도메인을 지정합니다.                                                                                                                                                                                                                                                                   | 여기에 사용자 계정 도메인 이름을 입력할까요?                                                            |
| **사용자**               | 모든 사용자 및 역할의 **감독자인 BHOLD 사용자** 계정의 로그온 이름을 지정합니다. 이 계정에는 사용자 역할을 연결하고 연결을 해제할 수 있는 사용 권한이 있습니다. BHOLD Core의 루트 사용자와 연결된 계정을 사용하지 않는 것이 좋습니다(기본적으로 BHOLD Core를 설치하는 데 사용되는 계정). 이 계정은 FIM을 연결하는 데 사용하는 동일한 계정일 수 있습니다. | 여기에 사용자 계정 이름을 입력합니다.                                                                   |
| **암호**           | **사용자**에 지정된 사용자 계정의 암호를 지정합니다.                                                                                                                                                                                                                                                                                                                             | 여기에 암호를 입력합니다. **중요:** 숨겨진 안전한 위치에 이 암호를 보관해야 합니다. |
| **IP/컴퓨터 주소** | BHOLD Core 웹 사이트 서버의 IP 주소를 지정합니다. 서버 이름을 사용하지 마십시오.                                                                                                                                                                                                                                                                                                        | IP 주소를 여기에 입력합니다.                                                                          |
| **포트 번호**        | BHOLD Core 웹 사이트의 포트 번호를 지정합니다.                                                                                                                                                                                                                                                                                                                                          | 포트 번호를 여기에 입력합니다.                                                                         |

## <a name="bhold-fim-integration-setup"></a>BHOLD FIM 통합 설정

BHOLD FIM 통합 모듈을 설치하려면 도메인 관리자 그룹의 구성원으로 로그온하고 다음 파일을 다운로드하고 BHOLD FIM 통합 모듈을 설치하려는 서버에서 관리자 권한으로 실행합니다.

- BholdFIMIntegration*\<Version\>*\_Release.msi

*\<Version\>*을 설치하는 BHOLD FIM 통합 릴리스의 버전 번호로 바꿉니다.

프로그램 파일을 관리자 권한으로 실행하려면 파일을 마우스 오른쪽 단추로 클릭하고 **관리자 권한으로 실행**을 클릭합니다.

![running msi](media/bhold-integration-installation/cmd.png)

## <a name="post-installation-tasks"></a>설치 후 작업

BHOLD FIM 통합을 설치한 후에 BHOLD 서비스 계정 사이트 소유자 사용 권한을 부여하도록 Microsoft SharePoint를 구성해야 합니다. 또한 FIM 포털이 SSL(Secure Sockets Layer)을 사용하도록 구성된 경우 FIM 포털에 추가된 BHOLD 페이지의 주소에 대한 참조를 포함하는 파일을 수정해야 합니다.

### <a name="configuring-sharepoint"></a>SharePoint 구성

제대로 작동시키려면 BHOLD FIM 통합에는 Microsoft SharePoint에서 사이트 멤버 사용 권한이 있는 BHOLD 서비스 계정이 필요합니다.

### <a name="to-grant-site-member-permissions-to-the-bhold-service-account"></a>BHOLD 서비스 계정에 사이트 멤버 사용 권한을 부여하려면

1.  BHOLD FIM 통합을 관리자 권한으로 실행하는 서버에 로그온합니다.

2.  **시작**을 클릭한 다음 **Internet Exporer**를 클릭합니다.

3.  SSL 보안을 사용하도록 SharePoint를 구성한 경우 주소 표시줄에 <https://localhost>를 입력합니다. 그렇지 않은 경우 <http://localhost>를 입력합니다.

4.  **팀 사이트** 페이지의 왼쪽에서 **사용자 및 그룹**을 클릭합니다.

5.  **그룹** 아래에서 **팀 사이트 멤버**를 클릭하고 가운데 창 도구 모음에서 **새로 만들기**를 클릭하고 **사용자 추가**를 클릭합니다.

6.  **사용자 추가: 팀 사이트** 페이지의 **사용자/그룹**에서 BHOLDApplicationGroup을 입력한 다음 **사용자/그룹** 상자 아래에서 이름 확인 단추를 클릭합니다. 그룹 이름은 도메인 이름을 포함하도록 해야 합니다.

7.  **사용자에게 직접 권한 부여**를 클릭하고 **모든 권한 - 모든 권한 있음**을 선택하고 **확인**을 클릭합니다.

8.  BHOLDApplicationGroup이 **사용 권한: 팀 사이트**에 나열되어 있는지 확인하고 Internet Explorer를 닫습니다.

![running msi](media/bhold-integration-installation/sharepoint.png)

### <a name="configuring-bhold-to-support-ssl"></a>SSL을 지원하도록 BHOLD 구성

FIM 포털이 SSL 보안을 사용하도록 구성된 경우 BHOLD 페이지에 대한 링크가 열리도록 FIM 서버에서 파일을 수정해야 합니다. 파일이 다음 폴더에 있습니다. ```C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\TEMPLATE\LAYOUTS\BHOLD```

다음 표는 편집할 파일 및 원본과 변경된 버전 문자열을 나열합니다.

| **파일**                  | **원본 문자열**                                                                                                                   | **변경된 문자열**                                                                                                                                |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| Analytics.aspx            |   http://<BHOLD_Server>/bhold/Analytics/index_fim.html | https://<BHOLD_Server_FQDN>/bhold/Analytics/index_fim.html       |
| AttestationCampaigns.aspx |    http://<BHOLD_Server>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | https://<BHOLD_Server_FQDN>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | 
| AttestationMain.aspx      |  http://<BHOLD_Server>/bhold/Attestation/Dashboard.aspx?hideMenu=1        | https://<BHOLD_Server_FQDN>/bhold/Attestation/Dashboard.aspx?hideMenu=1 |
| Reporting.aspx            | http://<BHOLD_Server>/bhold/Reporting/index_fim.html |  https://<BHOLD_Server_FQDN>/bhold/Reporting/index_fim.html |
| Selfservice.aspx          | RoleExchangePoint=http://\<*FIM_Server*\>: \<*FIM_Port*\>/BHOLD/RoleExchangePoint/ BHOLDRoleExchangePoint.svc,TransportMode=Transport | RoleExchangePoint=https://\<*FIM_Server_FQDN*\>: \<*FIM_SSL_Port\>*\>/BHOLD/RoleExchangePoint/ BHOLDRoleExchangePoint.svc,TransportMode=Transport |

위치:

-   *\<BHOLD_Server\>*는 파일의 원래 버전에 있는 BHOLD 서버 이름을 지정합니다.

-   *\<MIM_Server\>*는 파일의 원래 버전에 있는 FIM 서버 이름을 지정합니다.

-   *\<BHOLD_Server_FQDN\>*은 BHOLD 서버의 FQDN(정규화된 도메인 이름)을 지정합니다.

-   *\<MIM_Port\>*는 파일의 원래 버전에 있는 FIM 서버의 포트 번호를 지정합니다.

-   *\<MIM_Server_FQDN\>*은 FIM 서버의 FQDN을 지정합니다.

-   *\<MIM_SSL_Port\>*는 FIM 서버의 SSL에서 사용할 다른 포트를 지정합니다.

### <a name="enable-approval-workflows-in-bhold-core"></a>BHOLD Core에서 승인 워크플로 사용

FIM 및 BHOLD이 셀프 서비스에 통합된 경우 FIM 서비스에서 승인을 위한 워크플로가 실행됩니다. 이 옵션은 사용자가 배포 목록을 조인하는 요청을 제출하는 경우와 같이 FIM 포털에 배치된 요청의 워크플로 모델과 유사합니다. 그러나 BHOLD 역할 워크플로와 FIM 서비스에서 호스트되는 다른 워크플로 간에는 주요 차이점이 있습니다. BHOLD의 경우에 역할 요청을 승인해야 하는 사용자를 지정하는 워크플로 매개 변수는 FIM 서비스 데이터베이스의 워크플로 정의에 저장되지 않고 BHOLD에서 위치합니다. 첫 번째 요청이 이루어지는 경우 이러한 매개 변수가 BHOLD에 의한 FIM 서비스에 제공되고 작업 워크플로가 다시 BHOLD Core로 결과를 전송합니다.

BHOLD는 세 가지 방법 중 하나로 셀프 서비스 요청에 대한 승인자를 선택합니다.

-   **승인자인 줄 관리자: 조직 구성 단위(OrgUnit)에 대한 역할 기반 선택** 역할에 승인자 또는 에스컬레이터로 설정된 roletype이라는 특성이 있는 경우 및 해당 역할이 OrgUnit 컨텍스트에서 하나 이상의 사용자와 연결된 경우 해당 OrgUnit 내 사용자로부터의 요청은 승인자 또는 에스컬레이터 roletype을 가진 역할에 연결된 사용자 중 하나에 의해 승인되어야 합니다.

-   **승인자인 줄 관리자: OrgUnit에 대한 특성 기반 선택** 각 OrgUnit에는 OrgUnit에서 다른 사용자에 대한 역할 할당을 승인할 수 있는 사용자의 별칭을 지정하는 하나 이상의 특성이 있을 수 있습니다. 이러한 특성은 approver1, approver2 등으로 명명됩니다. OrgUnit의 사용자가 역할 할당을 요청하면 BHOLD는 (FIM을 통해) OrgUnit 승인자 특성에서 지정한 사용자에게 요청을 라우팅합니다. OrgUnit이 이러한 특성 집합을 포함하지 않는 경우 BHOLD는 루트 OrgUnit까지 부모 OrgUnit을 확인합니다.

-   **승인자인 역할 관리자: 역할에 대한 특성 기반 선택** 역할에는 역할 할당을 승인할 수 있는 사용자의 별칭을 지정하는 하나 이상의 특성(approver1 등으로 명명됨)이 있을 수 있습니다. 사용자가 이러한 승인자 특성을 설정한 역할을 할당하도록 요청하는 경우 BHOLD는 특성에서 지정한 사용자에게 요청을 라우팅합니다.

이러한 방법 중 하나로 셀프 서비스 역할 요청에 대한 승인자를 지정하지 않으면 기본적으로 BHOLD는 승인을 요구하지 않고 자동으로 역할을 할당합니다. 이러한 이유로 BHOLD FIM 통합을 설치한 직후에 루트 계정 등 승인자의 별칭으로 루트 OrgUnit을 구성해야 합니다. 이렇게 하면 포괄적인 승인 정책을 구현하기 전에 사용자가 의도하지 않게 역할을 부여하지 않도록 방지합니다.

#### <a name="to-configure-an-approver-for-the-root-orgunit"></a>루트 OrgUnit에 대한 승인자를 구성하려면

1.  관리자로 BHOLD Core 서버에 로그온합니다.

2.  **시작**을 클릭한 다음 **Internet Exporer**를 클릭합니다.

3.  Internet Explorer 주소 표시줄에 <http://localhost:5151/bhold/core>를 입력한 다음 Enter 키를 누릅니다.

4.  BHOLD Core 홈 페이지의 **특성 기본값** 아래에서 **특성 유형**을 클릭합니다.

5.  **특성 유형** 페이지에서 **추가**를 클릭합니다.

6.  **특성 유형 추가 페이지**의 **ID**에서 approver1을 입력하고, **데이터 형식** 목록에서 **영숫자**를 클릭하고 **최대 길이**에서 255를 입력하고 **값 목록**에서 **아니요**를 클릭하고 **영어**에서 Approver 1을 입력하고 **확인**을 클릭하고, **완료**를 클릭합니다.

7.  왼쪽 창의 **특성 기본값** 아래에서 **특성 유형 집합**을 클릭합니다.

8.  **특성 유형 집합** 페이지에서 **추가**를 클릭합니다.

9.  **특성 유형 집합 추가** 페이지의 **설명**에서 OrgUnit 특성을 입력한 다음 **확인**을 클릭합니다.

10. **OrgUnit 특성** 페이지에서 **특성 유형**을 확장하고 **수정**을 클릭합니다.

11. **특성 유형** 목록에서 **approver1**을 클릭하고 **추가**를 클릭하고 **완료**를 클릭합니다.

12. 왼쪽 창에서 **개체 형식**을 클릭합니다.

13. **개체 형식** 페이지에서 **OrgUnit**을 클릭합니다.

14. **개체 형식/OrgUnit** 페이지에서 **특성 유형 집합**을 확장하고 **수정**을 클릭합니다.

15. **특성 유형 집합/OrgUnit 연결** 페이지의 **순서**에 10을 입력하고 **특성 유형 집합** 목록에서 **OrgUnit 특성**을 클릭하고 **추가**를 클릭하고 **완료**를 클릭합니다.

16. 왼쪽 창의 **모델** 아래에서 **조직 구성 단위**를 클릭합니다.

17. **조직 구성 단위** 페이지에서 **루트**를 클릭합니다.

18. **조직 구성 단위/루트** 페이지에서 **수정**을 클릭합니다.

19. **조직 구성 단위 특성/루트 수정** 페이지의 **승인자**에서 역할 할당 요청을 승인할 사용자의 도메인과 사용자 이름을 *\<domain\>*\\*\<user\>* 양식으로 입력합니다. 여기서  *\<domain\>*은 NetBIOS(짧음) 도메인 이름이고 *\<user\>*는 사용자의 로그온 이름입니다.
20. **확인**을 클릭합니다.

>[!IMPORTANT]
도메인과 사용자 이름은 BHOLD Core 데이터베이스에 있는 사용자의 기본 별칭과 일치해야 합니다.

조직 구성 단위에 대한 승인자를 지정하는 대신 BHOLD Core 데이터베이스에서 제안된 역할에 대한 승인자를 지정할 수 있습니다. 이렇게 하려면 approver1 특성을 만들고. 역할 개체 유형과 연관된 특성 유형 집합에 추가하고, 승인자를 지정하도록 제안된 각 역할을 수정합니다.

워크플로 보안 수준을 높이려면 승인자 외에도 OrgUnits에 대한 다음 특성 및 역할을 만들고 채워서 추가 승인 모드 및 사용자를 지정해야 합니다.

- escalator*\<n\>*

- owner*\<n\>*

- securityOfficer*\<n\>*

- notification*\<n\>*

여기서 *\<n\>*는 동일한 형식으로 여러 특성을 제공하는 선택적 숫자 접미사를 나타냅니다.

### <a name="verify-approval-workflows-configured-in-the-fim-service"></a>FIM 서비스에 구성된 승인 워크플로 확인

BHOLD FIM 통합 설치는 집합, 워크플로 정의 및 FIM 서비스에 대한 MPR(관리 정책 규칙)을 만듭니다. 관리자 집합 또는 요청을 수행할 수 있는 사용자 집합을 변경하도록 FIM 배포를 사용자 지정한 경우 MPR이 올바른 사용자 집합을 참조해야 합니다.

>[!NOTE]
FIM 포털의 사용자가 BHOLD에서 제공하는 셀프 서비스 기능을 사용하기 전에 사용자 계정이 FIM 동기화 서비스의 BHOLD 데이터베이스에 동기화되어야 합니다. 특히, 셀프 서비스 요청을 수행하거나 셀프 서비스 요청에 대한 승인자 또는 에스컬레이터로 지정된 각 사용자의 경우 BHOLD Core 데이터베이스와 FIM 서비스 데이터베이스에 사용자 레코드가 있어야 합니다.

## <a name="next-steps"></a>다음 단계

- FIM 포털 및 다른 FIM 기능을 설치하는 방법에 대한 정보는 Microsoft Forefront 기술 라이브러리에서 [계획 및 아키텍처](https://technet.microsoft.com/library/ee808044.aspx)를 참조하세요.
- [BHOLD 설치 가이드](bhold-installation-guide.md)
- [BHOLD 개발자 참조](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD 버전 기록](../reference/version-bhold-history.md)
