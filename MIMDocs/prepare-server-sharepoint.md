---
title: Microsoft Identity Manager 2016에 대해 SharePoint 구성 | Microsoft 문서
description: MIM 포털 페이지를 호스트할 수 있도록 SharePoint Foundation을 설치 및 구성합니다.
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 04/26/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6922c3c2f66b6dbb0b0751420be9dd778206a3cf
ms.sourcegitcommit: 8316fa41f06f137dba0739a8700910116b5575d8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/04/2018
---
# <a name="set-up-an-identity-management-server-sharepoint"></a>ID 관리 서버 설치: SharePoint

>[!div class="step-by-step"]
[« SQL Server 2016](prepare-server-sql2016.md)
[Exchange Server »](prepare-server-exchange.md)

> [!NOTE]
> 이 연습에서는 Contoso라는 회사의 샘플 이름과 값을 사용합니다. 해당 항목을 사용자의 정보로 바꿉니다. 예를 들면 다음과 같습니다.
> - 도메인 컨트롤러 이름 - **corpdc**
> - 도메인 이름 - **contoso**
> - MIM 서비스 서버 이름 - **corpservice**
> - MIM 동기화 서버 이름 - **corpsync**
> - SQL Server 이름 - **corpsql**
> - 암호 - **Pass@word1**


## <a name="install-sharepoint-2016"></a>**SharePoint 2016** 설치

> [!NOTE]
> 필수 구성 요소를 다운로드하려면 설치 관리자가 인터넷에 연결되어 있어야 합니다. 컴퓨터가 인터넷 연결을 제공하지 않는 가상 네트워크에 있는 경우 인터넷 연결을 제공하는 컴퓨터에 네트워크 인터페이스를 추가합니다. 설치가 완료되면 비활성화할 수 있습니다.

SharePoint 2016을 설치하려면 다음 단계를 따르세요. 설치가 완료되면 서버가 다시 시작됩니다.

1.  사용할 SQL 데이터베이스 서버의 **corpservice** 및 **sysadmin**에서 **contoso\miminstall** 등의 로컬 관리자가 있는 도메인 계정으로 **PowerShell**을 시작합니다.

    -   SharePoint의 압축을 푼 디렉터리로 변경합니다.

    -   다음 명령을 입력합니다.

        ```
        .\prerequisiteinstaller.exe
        ```

2.  **SharePoint** 필수 조건이 설치된 후 다음 명령을 입력하여 **SharePoint 2016**을 설치합니다.

    ```
    .\setup.exe
    ```

3.  전체 서버 형식을 선택합니다.

4.  설치가 완료된 후 마법사를 실행합니다.

## <a name="run-the-wizard-to-configure-sharepoint"></a>마법사를 실행하여 SharePoint 구성

**SharePoint 제품 구성 마법사**에 나열된 단계에 따라 MIM과 함께 작동하도록 SharePoint를 구성합니다.

1. **서버 팜에 연결** 탭에서 새 서버 팜을 만들도록 변경합니다.

2. 이 서버를 구성 데이터베이스에 대한 데이터베이스 서버(예: **corpsql**)로 지정하고 *Contoso\SharePoint*를 SharePoint에서 사용할 데이터베이스 액세스 계정으로 지정합니다.
3. 팜 보안 암호로 사용할 암호를 만듭니다.

4. 구성 마법사에서 [MinRole](https://docs.microsoft.com/en-us/sharepoint/install/overview-of-minrole-server-roles-in-sharepoint-server-2016) 유형으로 **프런트 엔드**를 선택하는 것이 좋습니다.

5. 구성 마법사가 구성 작업 10/10을 완료할 때 [마침]을 클릭하면 웹 브라우저가 열립니다.

6. Internet Explorer 팝업이 표시되면 *Contoso\miminstall*(또는 동등한 관리자 계정)로 인증하여 계속 진행합니다.

7. 웹앱 내의 웹 마법사에서 **취소/건너뛰기**를 클릭합니다.


## <a name="prepare-sharepoint-to-host-the-mim-portal"></a>MIM 포털을 호스트할 SharePoint 준비

> [!NOTE]
> 처음에는 SSL이 구성되어 있지 않습니다. 이 포털에 액세스할 수 있으려면 SSL 또는 이와 동등한 것을 구성해야 합니다.

1. **SharePoint 2016 관리 셸**을 시작하고 다음 PowerShell 스크립트를 실행하여 **SharePoint 2016 웹 응용 프로그램**을 만듭니다.

    ```
    New-SPManagedAccount ##Will prompt for new account enter contoso\mimpool 
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\mimpool
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 80 -URL http://mim.contoso.com
    ```

    > [!NOTE]
    > Windows 기본 인증 방법이 사용 중이라는 경고 메시지가 표시되며 최종 명령을 반환하는 데 몇 분 정도 걸릴 수 있습니다. 완료되면 출력에 새 포털의 URL이 표시됩니다. 나중에 참조할 수 있도록 **SharePoint 2016 관리 셸** 창을 열어 둡니다.

2. SharePoint 2016 관리 셸을 시작한 후 다음 PowerShell 스크립트를 실행하여 해당 웹 응용 프로그램과 연결된 **SharePoint 사이트 모음**을 만듭니다.

  ```
    $t = Get-SPWebTemplate -compatibilityLevel 15 -Identity "STS#1"
    $w = Get-SPWebApplication http://mim.contoso.com/
    New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\miminstall -CompatibilityLevel 15 -Name "MIM Portal"
    $s = SpSite($w.Url)
    $s.CompatibilityLevel
  ```

  > [!NOTE]
  > *CompatibilityLevel* 변수의 결과가 “15”인지 확인합니다. 결과가 “15”가 아닌 경우 올바른 환경 버전에 대해 생성된 사이트 컬렉션이 아니므로 사이트 컬렉션을 삭제하고 다시 만듭니다.

3. **SharePoint 2016 관리 셸**에서 다음 PowerShell 명령을 실행하여 **SharePoint 서버 쪽 Viewstate** 및 SharePoint 작업 “상태 분석 작업(시간별, Microsoft SharePoint Foundation 타이머, 모든 서버)”을 사용하지 않도록 설정합니다.

  ```
  $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
  $contentService.ViewStateOnServer = $false;
  $contentService.Update();
  Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
  ```

4. ID 관리 서버에서 새 웹 브라우저 탭을 열고 http://mim.contoso.com/으로 이동하여 *contoso\miminstall*로 로그인합니다.  *MIM 포털* 이라는 빈 SharePoint 사이트가 나타납니다.

    ![http://mim.contoso.com/의 MIM 포털 이미지](media/prepare-server-sharepoint/MIM_DeploySP1new.png)

5. URL을 복사한 다음 Internet Explorer에서 **인터넷 옵션**을 열고 **보안** 탭으로 변경한 후 **로컬 인트라넷**을 선택하고 **사이트**를 클릭합니다.

    ![인터넷 옵션 이미지](media/MIM-DeploySP2.png)

6. **로컬 인트라넷** 창에서 **고급**을 클릭하고 **영역에 웹 사이트 추가** 텍스트 상자에 복사한 URL을 붙여 넣습니다. **추가**를 클릭한 다음 창을 닫습니다.

7. 아직 실행 중이 아닌 경우 **관리 도구** 프로그램을 열고 **서비스**로 이동하여 SharePoint 관리 서비스를 찾아서 시작합니다.

>[!div class="step-by-step"]  
[« SQL Server 2016](prepare-server-sql2016.md)
[Exchange Server »](prepare-server-exchange.md)
