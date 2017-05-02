---
title: "MIM 인증서 관리자 Windows 응용 프로그램 배포 | Microsoft 문서"
description: "사용자가 자신의 액세스 권한을 관리할 수 있도록 인증서 관리자 앱을 배포하는 방법을 알아봅니다."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 66060045-d0be-4874-914b-5926fd924ede
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3623bffb099a83d0eba47ba25e9777c3d590e529
ms.openlocfilehash: d714a58796d3a86fc82ed1eb6dc29bdc45920933
ms.lasthandoff: 01/24/2017


---

# <a name="working-with-the-mim-certificate-manager"></a>MIM 인증서 관리자 작업
MIM 2016 및 인증서 관리자를 실행한 후 MIM 인증서 관리자 Windows 스토어 응용 프로그램을 배포할 수 있으므로 사용자가 실제 스마트 카드, 가상 스마트 카드 및 소프트웨어 인증서를 쉽게 관리할 수 있습니다. MIM CM 앱을 배포하는 단계는 다음과 같습니다.

1.  인증서 템플릿을 만듭니다.

2.  프로필 템플릿을 만듭니다.

3.  앱을 준비합니다.

4.  SCCM 또는 Intune을 통해 앱을 배포합니다.

## <a name="create-a-certificate-template"></a>인증서 템플릿 만들기
인증서 템플릿이 버전 3 이상이어야 한다는 점을 제외하고 일반적으로 수행하는 동일한 방식으로 CM 앱에 대한 인증서 템플릿을 만듭니다.

1.  AD CS(인증서 서버)를 실행 중인 서버에 로그인합니다.

2.  MMC를 엽니다.

3.  **파일 &gt; 스냅인 추가/제거**를 클릭합니다.

4.  사용 가능한 스냅인 목록에서 **인증서 템플릿**을 클릭하고 **추가**를 클릭합니다.

5.  이제 MMC의 **콘솔 루트** 아래에 **인증서 템플릿** 이 나타납니다. 두 번 클릭하면 모든 사용 가능한 인증서 템플릿이 표시됩니다.

6.  **스마트 카드 로그온** 템플릿을 마우스 오른쪽 단추로 클릭하고 **중복 템플릿**을 클릭합니다.

7.  호환성 탭의 인증 기관 아래에서 Windows Server 2008을 선택하고 인증서 수신 대상 아래에서 Windows 8.1/Windows Server 2012 R2를 선택합니다.
    사용자에게 버전 3 이상의 인증서 템플릿이 있고 버전 3만 인증서 관리자 앱에서 작동하므로 이 단계는 중요합니다. 인증서 템플릿을 처음 만들어서 저장할 때 버전이 설정되므로 이 방식으로 인증서 템플릿을 만들지 않은 경우 올바른 버전으로 수정하는 방법이 없으며 계속 진행하기 전에 새로 만들어야 합니다.

8.  **일반** 탭에서 **표시 이름** 필드에 앱 UI에 표시할 이름(예: **가상 스마트 카드 로그온**)을 입력합니다.

9. **요청 처리** 탭에서 **용도** 를 **서명 및 암호화** 로 설정하고 **...수행할 작업** 아래에서 **등록 중에 사용자에게 확인**을 선택합니다.

10. **암호화** 탭의 **공급자 범주**에서 **키 저장소 공급자 및 주체의 컴퓨터에서 사용할 수 있는 모든 공급자를 요청에서 사용할 수 있습니다**를 선택합니다.

    > [!NOTE]
    > 템플릿의 버전이 3이면 옵션으로 키 저장소 공급자만 표시됩니다. 표시되지 않는 경우 인증서 템플릿을 올바른 버전으로 만들지 못한 것입니다. 위의 5단계부터 다시 시작하세요.

11. **보안** 탭에서 **등록** 액세스를 제공하려는 보안 그룹을 추가합니다. 예를 들어 모든 사용자에게 액세스를 제공하려면 **인증된 사용자** 그룹을 선택한 다음 **등록 권한** 을 선택합니다.

12. **확인** 을 클릭하여 변경 내용을 마무리하고 새 템플릿을 만듭니다. 인증서 템플릿 목록에 새 템플릿이 표시되어야 합니다.

13. **파일**을 선택하고 **스냅인 추가/제거**를 클릭하여 MMC 콘솔에 인증 기관 스냅인을 추가합니다. 어떤 컴퓨터를 관리할지 묻는 메시지가 표시되면 **로컬 컴퓨터**를 선택합니다.

14. MMC의 왼쪽 창에서 **인증 기관(로컬)** 을 확장한 다음 인증 기관 목록 내에서 해당 CA를 확장합니다.

15. **인증서 템플릿**을 마우스 오른쪽 단추로 클릭하고 **새로 만들기 &gt; 인증서 템플릿**을 클릭하여 발급합니다.

16. 목록에서 만들어진 새 템플릿을 선택하고 **확인**을 클릭합니다.

## <a name="create-a-profile-template"></a>프로필 템플릿 만들기
프로필 템플릿을 만들 때 vSC를 만들거나 제거하고 데이터 컬렉션을 제거하도록 템플릿을 설정해야 합니다. CM 앱은 수집된 데이터를 처리할 수 없으므로 다음과 같이 사용하지 않도록 설정하는 것이 중요합니다.

1.  CM 포털에 관리자 권한이 있는 사용자로 로그인합니다.

2.  관리 &gt; 프로필 관리 템플릿으로 이동하고 MIM CM 샘플 스마트 카드 로그온 프로필 템플릿 옆에 있는 확인란이 선택되었는지 확인한 다음 선택한 프로필 템플릿 복사를 클릭합니다.

3.  프로필 템플릿의 이름을 입력하고 **확인**을 클릭합니다.

4.  다음 화면에서 **Add new certificate template** (새 인증서 템플릿 추가)을 클릭하고 CA 이름 옆에 있는 확인란을 선택합니다.

5.  프로필 템플릿 **로그온** 이름 옆에 있는 확인란을 선택하고 **추가**를 클릭합니다.

6.  스마트 카드 로그온 템플릿 옆에 있는 확인란을 선택한 다음 **Delete selected certificate templates** (선택한 인증서 템플릿 삭제), **확인**을 차례로 클릭하여 스마트 카드 로그온 템플릿을 제거합니다.

7.  맨 아래로 스크롤하여 **설정 변경**을 클릭합니다.

8.  **Create/Destroy virtual smart card**(가상 스마트 카드 만들기/제거) 및 **Diversify Admin Key**(관리자 키 다양화) 옆에 있는 확인란을 선택합니다.

9. **User PIN Policy** (사용자 PIN 정책) 아래의 **User Provided**(사용자가 제공)를 선택합니다.

10. 왼쪽 창에서 **Renew Policy(정책 갱신) &gt; 일반 설정 변경**을 클릭합니다. **Reuse card on renew** (갱신 시 카드 재사용)를 선택하고 **확인**을 클릭합니다.

11. 왼쪽 창에서 정책을 클릭하고 **Sample data item** (샘플 데이터 항목) 옆에 있는 확인란을 선택하여 각 정책 및 모든 정책에 대한 데이터 컬렉션 항목을 사용하지 않도록 설정해야 합니다. 그런 다음 **Delete data collection**(데이터 컬렉션 항목 삭제)을 클릭합니다. 그런 다음 **확인**을 클릭합니다.

## <a name="prepare-the-cm-app-for-deployment"></a>배포용 CM 앱 준비

1.  명령 프롬프트에서 다음 명령을 실행하여 앱의 압축을 풀고 appx라는 새 하위 폴더에 콘텐츠를 추출한 다음 원본 파일이 수정되지 않도록 복사본을 만듭니다.

    ```
    makeappx unpack /l /p <app package name>.appx /d ./appx
    ren <app package name>.appx <app package name>.appx.original
    cd appx
    ```

2.  appx 폴더에서 CustomDataExample.xml 파일 이름을 Custom.data로 변경합니다.

3.  Custom.data 파일을 열고 필요에 따라 매개 변수를 수정합니다.

    |||
    |-|-|
    |MIMCM URL|CM을 구성하는 데 사용한 포털의 FQDN입니다. 예: https://mimcmServerAddress/certificatemanagement|
    |ADFS URL|AD FS를 사용하려는 경우 AD FS URL을 삽입합니다. 예: https://adfsServerSame/adfs|
    |PrivacyUrl|인증서 등록을 위해 수집한 사용자 세부 정보로 수행할 작업을 설명하는 웹 페이지에 대한 URL을 포함할 수 있습니다.|
    |SupportMail|지원 문제에 대한 메일 주소를 포함할 수 있습니다.|
    |LobComplianceEnable|true 또는 false로 설정할 수 있습니다. 기본 설정은 true입니다.|
    |MinimumPinLength|기본 설정은 6입니다.|
    |NonAdmin|true 또는 false로 설정할 수 있습니다. 기본 설정은 false입니다. 컴퓨터의 관리자가 아닌 사용자가 인증서를 등록하고 갱신하도록 하려면 이 속성만 수정합니다.|

4.  파일을 저장하고 편집기를 종료합니다.

5.  패키지에 서명하면 서명 파일이 만들어집니다. 따라서 AppxSignature.p7x라는 원본 서명 파일을 삭제해야 합니다.

6.  AppxManifest.xml 파일은 서명 인증서의 주체 이름을 지정합니다. 이 파일을 열어서 편집합니다.

7.  이 섹션을 시작하기 전에 서명 인증서를 가져와야 합니다. MIM 2016 인증서 관리자에서 비관리자가 스마트 카드 갱신 사용 설정, 1단계를 참조하세요.

8.  &lt;Identity&gt; 요소에서 Publisher 특성의 값을 서명 인증서에 나열된 주체와 동일하게 수정합니다(예: "CN=SUBJECT").

9. 파일을 저장하고 편집기를 종료합니다.

10. 명령 프롬프트에서 다음 명령을 실행하여 appx 파일을 다시 압축하고 서명합니다.

    ```
    cd ..
    makeappx pack /l /d .\appx /p <app package name>.appx
    ```
    여기서 app package name은 복사본을 만들 때 사용한 이름과 같습니다.

    ```
    signtool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.ap
    px
    ```
    새 .appx 파일을 제공합니다. pfx 파일은 서명 인증서의 위치와 .pfx 파일의 암호를 제공합니다.

11. AD FS 인증을 사용하여 작업하려면

    -   가상 스마트 카드 응용 프로그램을 엽니다. 이 응용 프로그램을 사용하면 다음 단계에 필요한 값을 보다 쉽게 찾을 수 있습니다.

    -   응용 프로그램을 AD FS 서버에 클라이언트로 추가하고 서버에서 CM을 구성하기 위해 AD FS 서버에서 Windows PowerShell을 열고 명령 `ConfigureMimCMClientAndRelyingParty.ps1 –redirectUri <redirectUriString> -serverFQDN <MimCmServerFQDN>`을 실행합니다.

        다음은 ConfigureMimCMClientAndRelyingParty.ps1 스크립트입니다.

        ```
        # HELP

        <#
        .SYNOPSIS
                        Configure ADFS for CM client app and server.
        .DESCRIPTION
           What the Script does:
                                        a. Registers the MIM CM client app on the ADFS server.
                                        b. Registers the MIM CM server relying party (Tells the ADFS server that it issues tokens for the CM server).
                        For parameter information, see 'get-help -detailed'
        .PARAMETER redirectUri
                        The redirectUri for CM client app. Will be added to ADFS server.
                        It can be found as follows:
                        1. Open the settings panel. Under settings, there is a "Redirect Uri" text box (an ADFS server address must be configured in order for the text to display).
                        2. Open MIM CM client app. Navigate to 'C:\Users\<your_username>\AppData\Local\Packages\CmModernAppv.<version>\LocalState', and open 'Logs_Virtual Smart Card Certificate Manager_<version>'. Search for "Redirect URI".
        .PARAMETER serverFqdn
                        Your deployed MIM CM server’s FQDN.
        .EXAMPLE
           .\ConfigureMimCMClientAndRelyingParty.ps1 -redirectUri ms-app://s-1-15-2-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789/ -serverFqdn WIN-TRUR24L4CFS.corp.cmteam.com
        #>

        # Parameter declaration
        [CmdletBinding()]
        Param(
          [Parameter(Mandatory=$True)]
           [string]$redirectUri,

           [Parameter(Mandatory=$True)]
           [string]$serverFqdn
        )

        Write-Host "Configuring ADFS Objects for OAuth.."

        #Configure SSO to get persistent sign on cookie
        Set-ADFSProperties -SsoLifetime 2880

        #Configure Authentication Policy
        #Intranet to use Kerberos
        #Extranet to use U/P

        #Create Client Objects

        Write-Host "Creating Client Objects..."

        $existingClient = Get-ADFSClient -Name "MIM CM Modern App"

        if ($existingClient -ne $null)
        {
            Write-Host "Found existing instance of the MIM CM Modern App, removing"
            Remove-ADFSClient -TargetName "MIM CM Modern App"
            Write-Host "Client object removed"
        }

        Write-Host "Adding Client Object for MIM CM Modern App client"
        Add-ADFSClient -Name "MIM CM Modern App" -ClientId "70A8B8B1-862C-4473-80AB-4E55BAE45B4F" -RedirectUri $redirectUri
        Write-Host "Client Object for MIM CM Modern App client Created"

        #Create Relying Parties
        Write-Host "Creating Relying Party Objects"

        $existingRp = Get-ADFSRelyingPartyTrust -Name "MIM CM Service"
        if ($existingRp -ne $null)
        {
            Write-Host "Found existing instance of the MIM CM Service RP, removing"
            Remove-ADFSRelyingPartyTrust -TargetName "MIM CM Service"
            Write-Host "RP object Removed"
        }

        $authorizationRules =
        "@RuleTemplate = `"AllowAllAuthzRule`"
        => issue(Type = `"http://schemas.microsoft.com/authorization/claims/permit`", Value = `"true`");"

        $issuanceRules =
        "@RuleTemplate = `"LdapClaims`"
        @RuleName = `"Emit UPN and common name`"
        c:[Type == `"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`", Issuer == `"AD AUTHORITY`"]
        => issue(store = `"Active Directory`", types =
        (`"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`",
        `"http://schemas.xmlsoap.org/claims/CommonName`"), query =
        `";userPrincipalName,cn;{0}`", param = c.Value);

        @RuleTemplate = `"PassThroughClaims`"
        @RuleName = `"Pass through Windows Account Name`"
        c:[Type ==`"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`"] => issue(claim = c);"

        Write-Host "Creating RP Trust for MIM CM Service"
        Add-ADFSRelyingPartyTrust -Name "MIM CM Service" -Identifier ("https://"+$serverFqdn+"/certificatemanagement") -IssuanceAuthorizationRules $authorizationRules -IssuanceTransformRules $issuanceRules
        Write-Host "RP Trust for MIM CM Service has been created"
        ```

    -   redirectUri 및 serverFQDN의 값을 업데이트합니다.

    -   가상 스마트 카드 응용 프로그램에서 redirectUri를 찾으려면 응용 프로그램 설정 패널을 열고 **설정**을 클릭합니다. 리디렉션 URI는 AD FS 서버 주소 표시줄 아래에 나열되어야 합니다. URI는 ADFS 서버 주소를 구성한 경우에만 나타납니다.

    -   serverFQDN은 MIMCM 서버의 전체 컴퓨터 이름일 뿐입니다.

    -   **ConfigureMIimCMClientAndRelyingParty.ps1** 스크립트에 관련된 도움말을 보려면 `get-help  -detailed ConfigureMimCMClientAndRelyingParty.ps1`를 실행하세요.

## <a name="deploy-the-app"></a>앱 배포
CM 앱을 설정할 때 다운로드 센터에서 MIMDMModernApp_&lt;version&gt;_AnyCPU_Test.zip 파일을 다운로드하고 모든 콘텐츠를 추출합니다. .appx 파일이 설치 관리자입니다. [System Center Configuration Manager](https://technet.microsoft.com/library/dn613840.aspx)또는 [Intune](https://technet.microsoft.com/library/dn613839.aspx)을 사용하여 일반적인 Windows 스토어 앱 배포 방법으로 이 앱을 배포하여 앱을 테스트용으로 로드할 수 있습니다. 따라서 사용자가 이 앱을 회사 포털을 통해 액세스하거나 자신의 컴퓨터에 직접 푸시되도록 해야 합니다.

