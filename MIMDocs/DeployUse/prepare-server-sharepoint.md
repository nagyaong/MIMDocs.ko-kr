---
# required metadata

title: ID 관리 서버 설치&#58; SharePoint | Microsoft Identity Manager
description: MIM 포털 페이지를 호스트할 수 있도록 SharePoint Foundation을 설치 및 구성합니다. 
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# ID 관리 서버 설치: SharePoint

>[!div class="step-by-step"]
[« SQL Server 2014](prepare-server-sql2014.md)
[Exchange Server »](prepare-server-exchange.md)

> [!NOTE]
> 아래의 모든 예제에서 **mimservername**은 도메인 컨트롤러의 이름을 나타내고 **contoso**는 도메인 이름을 나타내며 **Pass@word1**은 예제 암호를 나타냅니다.


## **SharePoint Foundation 2013 SP1** 설치

> [!NOTE]
> 설치 관리자가 해당 필수 조건을 다운로드하려면 인터넷 연결이 필요합니다.

설치를 마치면 서버가 다시 시작됩니다.

1.  도메인 관리자로 **PowerShell** 을 시작합니다.

    -   SharePoint의 압축을 푼 디렉터리로 변경합니다.

    -   다음 명령을 입력합니다.

        ```
        .\prerequisiteinstaller.exe
        ```

2.  **SharePoint** 필수 조건이 설치된 후 다음 명령을 입력하여 **SharePoint Foundation 2013 SP1** 을 설치합니다.

    ```
    .\setup.exe
    ```

3.  전체 서버 형식을 선택합니다.

4.  설치가 완료된 후 마법사를 실행합니다.

## 마법사를 실행하여 SharePoint 구성

**SharePoint 제품 구성 마법사**에 나열된 단계에 따라 MIM과 함께 작동하도록 SharePoint를 구성합니다.

1. **서버 팜에 연결** 탭에서 새 서버 팜을 만들도록 변경합니다.

2. 이 서버를 구성 데이터베이스에 대한 데이터베이스 서버로 지정하고 SharePoint에서 사용할 데이터베이스 액세스 계정으로 *Contoso\SharePoint*를 지정합니다.

3. 암호를 팜 보안 암호로 지정합니다(나중에 이 랩 환경에서 사용되지 않음).

4. 구성 마법사가 구성 작업 10/10을 완료할 때 마침을 클릭하면 웹 브라우저가 열립니다.

5. IInternet Explorer 팝업에서 *Contoso\Administrator*(또는 해당하는 도메인 관리자 계정)로 인증하여 계속 진행합니다.

6. 웹앱 내에서 마법사를 시작하여 SharePoint 팜을 구성합니다.

7. 기존의 관리되는 계정(*Contoso\SharePoint*)을 사용하는 옵션을 선택하고 **다음**을 클릭합니다.

8. **Creating a Site Collection** (사이트 컬렉션 만들기) 창에서 **건너뛰기**를 클릭합니다.  **마침**을 클릭합니다.

## MIM 포털을 호스트할 SharePoint 준비

1. **SharePoint Foundation 2013 웹 응용 프로그램**을 만듭니다.

    > [!NOTE]
    > 처음에는 SSL이 구성되어 있지 않습니다. 이 포털에 액세스할 수 있으려면 SSL 또는 이와 동등한 것을 구성해야 합니다.

    1. **SharePoint 2013 관리 셸** 을 시작하고 다음 PowerShell 스크립트를 실행합니다.

        ```
        $dbManagedAccount = Get-SPManagedAccount -Identity contoso\SharePoint
        New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool"
        -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://corpidm.contoso.local
        ```

        2. Windows 기본 인증 방법이 사용 중이라는 경고 메시지가 표시되며 최종 명령을 반환하는 데 몇 분 정도 걸릴 수 있습니다.  완료되면 출력에 새 포털의 URL이 표시됩니다.  후속 작업에 필요하므로 **SharePoint 2013 관리 셸** 창을 계속 열어둡니다.

2. 해당 웹 응용 프로그램과 연결된 **SharePoint 사이트 모음**을 만듭니다.

    1. SharePoint 2013 관리 셸을 시작하고 다음 PowerShell 스크립트를 실행합니다.

        ```
        $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
        $w = Get-SPWebApplication http://corpidm.contoso.local:82
        New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\Administrator
        -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias contoso\BackupAdmin
        $s = SpSite($w.Url)
        $s.AllowSelfServiceUpgrade = $false
        $s.CompatibilityLevel
        ```

        2. *CompatibilityLevel* 변수의 결과가 "14"인지 확인합니다  자세한 내용은 [SharePoint Foundation 2013에 FIM 2010 R2 설치](http://technet.microsoft.com/library/jj863242.aspx)를 참조하세요. 결과가 "15"인 경우 사이트 컬렉션이 2010 환경 버전에 대해 만들어지지 않으므로 사이트 컬렉션을 삭제하고 다시 만듭니다.

3. **SharePoint 2013 관리 셸**에서 다음 PowerShell 명령을 실행하여 **SharePoint 서버 쪽 Viewstate** 및 SharePoint 작업 "상태 분석 작업(시간별, Microsoft SharePoint Foundation 타이머, 모든 서버)"를 사용하지 않도록 설정합니다.

    ```
    $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
    $contentService.ViewStateOnServer = $false;
    $contentService.Update();
    Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
    ```

4. ID 관리 서버에서 새 웹 브라우저 탭을 열고 http://localhost:82/로 이동하여 *contoso\Administrator*로 로그인합니다.  *MIM 포털* 이라는 빈 SharePoint 사이트가 나타납니다.

    ![MIM 포털(http://localhost:82/) 이미지](media/MIM-DeploySP1.png)

5. URL을 복사한 다음 Internet Explorer에서 **인터넷 옵션**을 열고 **보안** 탭으로 변경한 후 **로컬 인트라넷**을 선택하고 **사이트**를 클릭합니다.

    ![인터넷 옵션 이미지](media/MIM-DeploySP2.png)

6. **로컬 인트라넷** 창에서 **고급**을 클릭하고 **영역에 웹 사이트 추가** 텍스트 상자에 복사한 URL을 붙여 넣습니다. **추가**를 클릭한 다음 창을 닫습니다.

7. 아직 실행 중이 아닌 경우 **관리 도구** 프로그램을 열고 **서비스**로 이동하여 SharePoint 관리 서비스를 찾아서 시작합니다.

>[!div class="step-by-step"]  
[« SQL Server 2014](prepare-server-sql2014.md)
[Exchange Server »](prepare-server-exchange.md)


<!--HONumber=Apr16_HO2-->

