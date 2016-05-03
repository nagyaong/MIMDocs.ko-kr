---
# required metadata

title: ID 관리 서버 설치&#58; Windows Server 2012 R2 | Microsoft Identity Manager
description: MIM 2016과 함께 작동하도록 Windows Server 2012 RS를 준비하는 데 필요한 단계 및 최소 요구 사항을 알아봅니다.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# ID 관리 서버 설치: Windows Server 2012 R2

>[!div class="step-by-step"]
[« 도메인 준비](preparing-domain.md)
[SQL Server 2014 »](prepare-server-sql2014.md)

> [!NOTE]
> 아래의 모든 예제에서 **mimservername**은 도메인 컨트롤러의 이름을 나타내고 **contoso**는 도메인 이름을 나타내며 **Pass@word1**은 예제 암호를 나타냅니다.

## 도메인에 Windows Server 2012 R2 가입

1. 최소 8GB의 RAM이 있는 Windows Server 2012 R2 컴퓨터를 만듭니다. 설치할 때 “Windows Server 2012 R2 Standard(GUI 포함 서버) x64” 버전을 지정합니다.

2. 해당 관리자로 새 컴퓨터에 로그인합니다.

3. 제어판을 사용하여 네트워크에서 컴퓨터의 고정 IP 주소를 지정합니다. DNS 쿼리를 이전 단계의 도메인 컨트롤러의 IP 주소로 보내도록 해당 네트워크 인터페이스를 구성한 다음 컴퓨터 이름을 **CORPIDM**으로 설정합니다.  이 경우 서버를 다시 시작해야 합니다.

4. 컴퓨터가 인터넷 연결을 제공하지 않는 가상 네트워크에 있는 경우 인터넷 연결을 제공하는 컴퓨터에 네트워크 인터페이스를 추가합니다.  이 네트워크 인터페이스는 SharePoint 설치에 필요하며 이 단계가 완료되면 사용하지 않도록 설정할 수 있습니다.

5. 제어판을 열고 마지막 단계에서 구성한 도메인(*contoso.local*)에 컴퓨터를 가입합니다.  이를 위해 *Contoso\Administrator*와 같은 도메인 관리자의 사용자 이름 및 자격 증명을 입력해야 합니다.  환영 메시지가 표시되면 대화 상자를 닫고 이 서버를 다시 시작합니다.

6. *Contoso\Administrator*와 같은 도메인 관리자로 *CorpIDM* 컴퓨터에 로그인합니다.

7. 관리자로 PowerShell 창을 시작하고 다음 명령을 입력하여 그룹 정책 설정에 따라 컴퓨터를 업데이트합니다.

    ```
    gpupdate /force /target:computer
    ```

    잠시 후 "컴퓨터 정책 업데이트가 완료되었습니다."라는 메시지와 함께 완료됩니다.

8. **웹 서버(IIS)** 및 **응용 프로그램 서버** 역할, **.NET Framework** 3.5, 4.0 및 4.5 기능, Windows PowerShell용 **Active Directory** 모듈을 추가합니다.

    ![PowerShell 기능 이미지](media/MIM-DeployWS2.png)

9. PowerShell에서 계속 관리자 권한으로 다음 명령을 실행합니다. **.NET Framework** 3.5 기능을 위해 원본 파일에 대한 다른 위치를 지정해야 할 수 있습니다. 이러한 기능은 일반적으로 Windows Server가 설치될 때 없지만 OS 설치 디스크 원본 폴더의 SxS(Side-by-Side) 폴더(예: “*d:\Sources\SxS\*”)에 있습니다.

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

## 서버 보안 정책 구성

새로 만든 계정이 서비스로 실행될 수 있도록 서버 보안 정책을 설정합니다.

1. 로컬 보안 정책 프로그램을 시작합니다.

2. **로컬 정책 > 사용자 권한 할당**으로 이동합니다.

3. 세부 정보 창에서 **서비스로 로그온**을 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다.

    ![로컬 보안 정책 이미지](media/MIM-DeployWS3.png)

4. **사용자 또는 그룹 추가**를 클릭하고 다음 상자에서 `contoso\mimsync; contoso\mimma; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\mimsspr`을 입력한 다음 **이름 확인**을 클릭하고 **확인**을 클릭합니다.

5. **확인**을 클릭하여 **서비스로 로그온 속성** 창을 닫습니다.

6.  세부 정보 창에서 **네트워크에서 이 컴퓨터 액세스 거부**를 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다.

7. **사용자 또는 그룹 추가**를 클릭하고 다음 상자에서 `contoso\MIMSync; contoso\MIMService`을 입력한 다음 **확인**을 클릭합니다.

8. **확인**을 클릭하여 **네트워크에서 이 컴퓨터 액세스 거부 속성** 창을 닫습니다.

9. 세부 정보 창에서 **로컬 로그온 거부**를 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다.

10. **사용자 또는 그룹 추가**를 클릭하고 다음 상자에서 `contoso\MIMSync; contoso\MIMService`을 입력한 다음 **확인**을 클릭합니다.

11. **확인** 을 클릭하여 **로컬 로그온 거부 속성** 창을 닫습니다.

12. 로컬 보안 정책 창을 닫습니다.


## IIS Windows 인증 모드 변경

1.  PowerShell 창을 엽니다.

2.  *iisreset /STOP* 명령을 사용하여 IIS를 중지합니다.

        ```
        iisreset /STOP
        C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
        iisreset /START
        ```

>[!div class="step-by-step"]  
[« 도메인 준비](preparing-domain.md)
[SQL Server 2014 »](prepare-server-sql2014.md)


<!--HONumber=Apr16_HO2-->


