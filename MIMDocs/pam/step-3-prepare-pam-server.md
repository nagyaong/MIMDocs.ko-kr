---
title: "PAM 배포 3단계 – PAM 서버 | Microsoft 문서"
description: "Privileged Access Management 배포를 위한 SQL 및 SharePoint를 모두 호스트하는 PAM 서버를 준비합니다."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 68ec2145-6faa-485e-b79f-2b0c4ce9eff7
ROBOTS: noindex,nofollow
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: 618b834452aa07a9f31582994fe32129a49f4249
ms.lasthandoff: 11/10/2016


---

# <a name="step-3--prepare-a-pam-server"></a>3단계 – PAM 서버 준비

>[!div class="step-by-step"]
[« 2단계](step-2-prepare-priv-domain-controller.md)
[4단계 »](step-4-install-mim-components-on-pam-server.md)

## <a name="install-windows-server-2012-r2"></a>Windows Server 2012 R2 설치
세 번째 가상 컴퓨터에 Windows Server 2012 R2, 특히 Windows Server 2012 R2 Standard(GUI 포함 서버) x64를 설치하여 *PAMSRV*를 만듭니다. 이 컴퓨터에 SQL Server와 SharePoint 2013이 설치되므로 최소 8GB의 RAM이 필요합니다.

1. **Windows Server 2012 R2 Standard(GUI 포함 서버) x64**를 선택합니다.

    ![Windows Server Standard(GUI 포함) 선택 - 스크린샷](media/PAM_GS_Select_WS2012.png)

2. 사용 조건을 검토하고 이에 동의합니다.

3.  디스크가 비어 있으므로 **사용자 지정: Windows만 설치** 를 선택하고 **초기화되지 않은 디스크 공간**을 사용합니다.

4.  해당 새 컴퓨터에 해당 관리자로 로그인합니다.  제어판에서 가상 네트워크의 고정 IP 주소를 지정하고 DNS 쿼리를 PRIVDC의 IP 주소로 보내도록 해당 네트워크 인터페이스를 구성한 다음 컴퓨터 이름을 *PAMSRV*로 설정합니다.  이 경우 서버를 다시 시작해야 합니다.

5.  가상 네트워크가 인터넷 연결을 제공하지 않는 경우 인터넷에 대한 연결을 제공하는 컴퓨터에 또 다른 네트워크 인터페이스를 추가합니다.  이 네트워크 인터페이스는 SharePoint 설치에 필요하며 이 단계가 완료되면 사용하지 않도록 설정할 수 있습니다.

6.  서버가 다시 시작되면 관리자로 로그인합니다. 제어판에서 업데이트를 확인하도록 컴퓨터를 구성하고 필요한 업데이트를 설치합니다.  이 경우 서버를 다시 시작해야 할 수 있습니다.

7.  서버가 다시 시작하면 관리자로 로그인하여 제어판을 열고 PAMSRV를 PRIV 도메인(priv.contoso.local)에 연결합니다.  이를 위해 PRIV 도메인 관리자(PRIV\\Administrator)의 사용자 이름 및 자격 증명을 제공해야 합니다. 환영 메시지가 표시되면 대화 상자를 닫고 이 서버를 다시 시작합니다.


### <a name="add-the-web-server-iis-and-application-server-roles"></a>웹 서버(IIS) 및 응용 프로그램 서버 역할 추가
웹 서버(IIS), 응용 프로그램 서버 역할, .NET Framework 3.5 기능, Windows PowerShell용 Active Directory 모듈, SharePoint에서 필요한 다른 기능을 추가합니다.

1.  PRIV 도메인 관리자(PRIV\Administrator)로 로그인하고 PowerShell을 시작합니다.

2.  다음 명령을 입력합니다. .NET Framework 3.5 기능에 대한 원본 파일에 대해 다른 위치를 지정해야 할 수 있습니다. 이러한 기능은 일반적으로 Windows Server가 설치될 때는 없지만 OS 설치 디스크 원본 폴더의 SxS(Side-by-Side) 폴더(예: “*d:\Sources\SxS”)에 있습니다.

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,
    rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,
    Windows-Identity-Foundation,Server-Media-Foundation,
    Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

### <a name="configure-the-server-security-policy"></a>서버 보안 정책 구성
새로 만든 계정이 서비스로 실행될 수 있도록 서버 보안 정책을 구성합니다.

1.  **로컬 보안 정책** 프로그램을 시작합니다.   
2.  **로컬 정책** > **사용자 권한 할당**으로 이동합니다.  
3.  세부 정보 창에서 **서비스로 로그온**을 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다.  
4.  **사용자 또는 그룹 추가**를 클릭하고 사용자 및 그룹 이름에 *priv\mimmonitor; priv\MIMService; priv\SharePoint; priv\mimcomponent; priv\SqlServer*를 입력합니다. **이름 확인**을 클릭하고 **확인**을 클릭합니다.  

5.  **확인**을 클릭하여 속성 창을 닫습니다.  
6.  세부 정보 창에서 **네트워크에서 이 컴퓨터 액세스 거부**를 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다.  
7.  **사용자 또는 그룹 추가**를 클릭하고 사용자 및 그룹 이름에 *priv\mimmonitor; priv\MIMService; priv\mimcomponent*를 입력한 다음 **확인**을 클릭합니다.  
8.  **확인**을 클릭하여 속성 창을 닫습니다.  

9. 세부 정보 창에서 **로컬 로그온 거부**를 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다.  
10. **사용자 또는 그룹 추가**를 클릭하고 사용자 및 그룹 이름에 *priv\mimmonitor; priv\MIMService; priv\mimcomponent*를 입력한 다음 **확인**을 클릭합니다.  
11. **확인**을 클릭하여 속성 창을 닫습니다.  
12. 로컬 보안 정책 창을 닫습니다.  

13. 제어판을 열고 **사용자 계정**으로 전환합니다.  
14. **이 컴퓨터에 다른 사용자가 액세스하도록 허용**을 클릭합니다.  
15. **추가**를 클릭하고 도메인 *PRIV*에 사용자 *MIMADMIN*을 입력하고 마법사의 다음 화면에서 **이 사용자를 관리자로 추가**를 클릭합니다.  
16. **추가**를 클릭하고 도메인 *PRIV*에 사용자 *SharePoint*를 입력하고 마법사의 다음 화면에서 **이 사용자를 관리자로 추가**를 클릭합니다.  
17. 제어판을 닫습니다.  

### <a name="change-the-iis-configuration"></a>IIS 구성 변경
응용 프로그램이 Windows 인증 모드를 사용할 수 있도록 IIS 구성을 변경하는 두 가지 방법이 있습니다. MIMAdmin으로 로그인하고 다음 옵션 중 하나를 따라야 합니다.

PowerShell을 사용하려면 다음을 수행합니다.
1.  PowerShell을 마우스 오른쪽 단추로 클릭하고 **관리자 권한으로 실행**을 선택합니다.  
2.  IIS를 중지하고 다음 명령을 사용하여 응용 프로그램 호스트 설정의 잠금을 해제합니다.  
    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

메모장과 같은 텍스트 편집기를 사용하려면 다음을 수행합니다.   
1. **C:\Windows\System32\inetsrv\config\applicationHost.config** 파일을 엽니다.   
2. 해당 파일의 82번째 줄까지 아래로 스크롤합니다. **overrideModeDefault**의 태그 값은 **<section name="windowsAuthentication" overrideModeDefault="Deny" />**입니다.  
3. **overrideModeDefault**의 값을 *Allow*로 변경합니다.  
4. 파일을 저장하고 `iisreset /START` PowerShell 명령을 사용하여 IIS를 다시 시작합니다.

## <a name="install-sql-server"></a>SQL Server 설치
배스천 환경에 아직 SQL Server가 없는 경우 SQL Server 2012(서비스 팩 1 이상) 또는 SQL Server 2014를 설치합니다. 다음 단계에서는 SQL 2014로 가정합니다.

1. MIMAdmin으로 로그인해야 합니다.
2. PowerShell을 마우스 오른쪽 단추로 클릭하고 **관리자 권한으로 실행**을 선택합니다.   
3. SQL Server 설치 프로그램이 위치한 디렉터리로 이동합니다.  
4. 다음 명령을 입력합니다.  
    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="PRIV\SqlServer" /SQLSVCPASSWORD="Pass@word1" /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="PRIV\MIMAdmin"
    ```

## <a name="install-sharepoint-foundation-2013"></a>SharePoint Foundation 2013 설치

SharePoint Foundation 2013 SP1 설치 관리자를 사용하여 SharePoint의 소프트웨어 필수 조건을 PAMSRV에 설치합니다.

> [!NOTE]
> 필수 구성 요소를 다운로드하려면 설치 관리자가 인터넷에 연결되어 있어야 합니다. 필수 구성요소가 설치되고 나면 서버가 다시 시작됩니다.

1. PowerShell을 마우스 오른쪽 단추로 클릭하고 **관리자 권한으로 실행**을 선택합니다.  
2. SharePoint의 압축을 푼 디렉터리로 변경합니다.  
3. `.\prerequisiteinstaller.exe` 명령을 입력합니다.

SharePoint 필수 조건이 설치된 후 SharePoint Foundation 2013 SP1을 설치합니다.

1.  PowerShell을 마우스 오른쪽 단추로 클릭하고 **관리자 권한으로 실행**을 선택합니다.  
2.  SharePoint의 압축을 푼 디렉터리로 변경합니다.  
3.  `.\setup.exe` 명령을 입력합니다.  
4.  **전체 서버** 형식을 선택합니다.  
5.  설치가 완료된 후 선택하여 마법사를 실행합니다.  

### <a name="configure-sharepoint"></a>SharePoint 구성
SharePoint 제품 구성 마법사를 실행하여 SharePoint를 구성합니다.

1.  서버 팜에 연결 탭에서 **새 서버 팜 만들기**로 변경합니다.  
2.  **PAMSRV**를 구성 데이터베이스에 대한 데이터베이스 서버로 지정하고 **PRIV\SharePoint**를 SharePoint가 사용할 데이터베이스 액세스 계정으로 지정합니다.  
3.  암호를 팜 보안 암호로 지정합니다(나중에 이 연습에서 사용되지 않음).  
4.  지금은 SharePoint 구성 마법사의 나머지 기본 설정을 사용하여 단일 서버 팜을 만듭니다.    
5.  구성 마법사가 구성 작업 10/10을 완료할 때 **마침**을 클릭하면 웹 브라우저가 열립니다.  
6.  Internet Explorer 팝업에서 도메인 관리자(PRIV\MIMAdmin)로 인증하여 계속 진행합니다.  
7.  웹앱 내에서 마법사를 시작하여 SharePoint 팜을 구성합니다.  
8.  기존의 관리되는 계정(PRIV\SharePoint)을 사용하려면 선택하고 모든 선택적 서비스를 선택 취소하여 사용하지 않도록 설정한 후 **다음**을 클릭합니다.  
9. 사이트 모음 만들기 창이 표시되면 **건너뛰기**와 **마침**을 차례로 클릭합니다.  

## <a name="create-a-sharepoint-foundation-2013-web-application"></a>SharePoint Foundation 2013 웹 응용 프로그램 만들기
마법사가 완료된 후 PowerShell을 사용하여 MIM 포털을 호스트할 SharePoint Foundation 2013 웹 응용 프로그램을 만듭니다. 이 연습은 데모용이므로 SSL은 사용하도록 설정되지 않습니다.

1.  SharePoint 2013 관리 셸을 마우스 오른쪽 단추로 클릭하고 **관리자 권한으로 실행**을 선택한 후 다음 PowerShell 스크립트를 실행합니다.

    ```
    $dbManagedAccount = Get-SPManagedAccount -Identity PRIV\SharePoint
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool"            -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://PAMSRV.priv.contoso.local
    ```

2. Windows 기본 인증 방법이 사용 중이라는 경고 메시지가 표시되며 최종 명령을 반환하는 데 몇 분 정도 걸릴 수 있습니다.  완료되면 출력에 새 포털의 URL이 표시됩니다.

> [!NOTE]
> 다음 단계에서 사용하도록 SharePoint 2013 관리 셸 창을 계속 열어둡니다.

## <a name="create-a-sharepoint-site-collection"></a>Sharepoint 사이트 모음 만들기
이제 MIM 포털을 호스트하기 위해 해당 웹 응용 프로그램과 관련된 SharePoint 사이트 모음을 만듭니다.

1.  **SharePoint 2013 관리 셸**이 아직 열려 있지 않은 경우 이를 시작하고 다음 PowerShell 스크립트를 실행합니다.

    ```
    $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
    $w = Get-SPWebApplication http://pamsrv.priv.contoso.local:82
    New-SPSite -Url $w.Url -Template $t -OwnerAlias PRIV\MIMAdmin                -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias PRIV\BackupAdmin
    $s = SpSite($w.Url)
    $s.AllowSelfServiceUpgrade = $false
    $s.CompatibilityLevel
    ```

    **CompatibilityLevel** 변수가 *14*로 설정되어 있는지 확인합니다. *15*를 반환하는 경우 사이트 컬렉션이 2010 환경 버전에 대해 만들어지지 않으므로 사이트 모음을 삭제하고 다시 만듭니다.

2.  **SharePoint 2013 관리 셸**에서 다음 PowerShell 명령을 실행합니다. 이렇게 하면 SharePoint 서버 쪽 viewstate 및 SharePoint 작업 **상태 분석 작업(시간별, Microsoft SharePoint Foundation 타이머, 모든 서버)**을 사용할 수 없습니다.

    ```
    $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
    $contentService.ViewStateOnServer = $false;
    $contentService.Update();
    Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
    ```

## <a name="change-update-settings"></a>업데이트 설정 변경

1. 제어판을 열고 **Windows 업데이트**로 이동한 후 **설정 변경**을 클릭합니다.  
2. Windows 업데이트에서 업데이트를 수신하고 Microsoft 업데이트에서 기타 제품을 수신하도록 설정을 변경합니다.  
3. 계속하기 전에 새 업데이트가 있는지 확인하고 보류 중인 모든 중요 업데이트가 설치되어 있는지 확인합니다.

## <a name="set-the-website-as-the-local-intranet"></a>로컬 인트라넷으로 웹 사이트 설정

1. Internet Explorer를 시작하고 새 웹 브라우저 탭 열기
2. http://pamsrv.priv.contoso.local:82/로 이동한 후 PRIV\MIMAdmin으로 로그인합니다.  "MIM 포털"이라는 빈 SharePoint 사이트가 나타납니다.  
3. Internet Explorer에서 **인터넷 옵션**을 열고 **보안** 탭으로 변경한 후 **로컬 인트라넷**을 선택한 다음 URL `http://pamsrv.priv.contoso.local:82/`를 추가합니다.

로그인이 실패하는 경우 [2단계](step-2-prepare-priv-domain-controller.md) 앞부분에서 만든 Kerberos SPN을 업데이트해야 할 수 있습니다.

## <a name="start-the-sharepoint-administration-service"></a>SharePoint 관리 서비스 시작

아직 실행 중이 아닌 경우 **서비스**(관리 도구에 있음)를 사용하여 **SharePoint 관리** 서비스를 시작합니다.

4단계에서 PAM 서버에 MIM 구성 요소 설치를 시작합니다.

>[!div class="step-by-step"]
[« 2단계](step-2-prepare-priv-domain-controller.md)
[4단계 »](step-4-install-mim-components-on-pam-server.md)

