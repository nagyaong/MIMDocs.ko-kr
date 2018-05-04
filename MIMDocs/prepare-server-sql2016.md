---
title: Microsoft Identity Manager 2016 SP1에 대해 SQL Server 구성 | Microsoft Docs
description: MIM 2016 설치를 위한 준비 단계로 SQL Server 2016을 설치합니다.
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 04/26/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: e2006ca2a74f8c974f6019004aeaefdc73069fa6
ms.sourcegitcommit: 32d9a963a4487a8649210745c97a3254645e8744
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/27/2018
---
# <a name="set-up-an-identity-management-server-sql-server-2016"></a>ID 관리 서버 설치: SQL Server 2016

>[!div class="step-by-step"]
[« Windows Server 2016](prepare-server-ws2016.md)
[SharePoint »](prepare-server-sharepoint.md)

> [!NOTE]
> 이 연습에서는 Contoso라는 회사의 샘플 이름과 값을 사용합니다. 해당 항목을 사용자의 정보로 바꿉니다. 예를 들면 다음과 같습니다.
> - 도메인 컨트롤러 이름 - **corpdc**
> - 도메인 이름 - **contoso**
> - MIM 서비스 서버 이름 - **corpservice**
> - MIM 동기화 서버 이름 - **corpsync**
> - SQL Server 이름 - **corpsql**
> - 암호 - **Pass@word1**

## <a name="install-sql-server-2016-standardenterprise-edition"></a>**SQL Server 2016 Standard/Enterprise Edition** 설치

1. 도메인 관리자로 **PowerShell** 을 시작합니다.

2. SQL Server 설치 프로그램이 위치한 디렉터리로 변경합니다.

3. 다음 명령을 입력합니다.

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"

More info SQL deployment accounts and services can be found [here](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/configure-windows-service-accounts-and-permissions?view=sql-server-2017)
> [!NOTE]
> SSMS is no longer included in SQL 2016 downlaod details can be found [here](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)    ```

>[!div class="step-by-step"]  
[« Windows Server 2016](prepare-server-ws2016.md)
[SharePoint »](prepare-server-sharepoint.md)