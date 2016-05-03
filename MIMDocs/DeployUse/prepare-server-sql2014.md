---
# required metadata

title: ID 관리 서버 설치&#58; SQL Server 2014 | Microsoft Identity Manager
description: MIM 2016 설치를 위한 준비 단계로 SQL Server 2014를 설치합니다.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# ID 관리 서버 설치: SQL Server 2014

>[!div class="step-by-step"]
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)

> [!NOTE]
> 아래의 모든 예제에서 **mimservername**은 도메인 컨트롤러의 이름을 나타내고 **contoso**는 도메인 이름을 나타내며 **Pass@word1**은 예제 암호를 나타냅니다.

## **SQL Server 2014 Standard Edition** 설치

1. 도메인 관리자로 **PowerShell** 을 시작합니다.

2. SQL Server 설치 프로그램이 위치한 디렉터리로 변경합니다.

3. 다음 명령을 입력합니다.

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```

>[!div class="step-by-step"]  
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)


<!--HONumber=Apr16_HO2-->


