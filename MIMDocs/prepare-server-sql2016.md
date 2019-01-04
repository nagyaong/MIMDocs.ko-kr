---
title: Microsoft Identity Manager 2016 SP1에 대해 SQL Server 구성 | Microsoft Docs
description: MIM 2016 설치를 위한 준비 단계로 SQL Server 2016을 설치합니다.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/26/2018
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2dc7c1ffecbd4f52ab031512f501922f458324a3
ms.sourcegitcommit: 4ae62943aea3aab622195896956c5c88ecd9e97c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/02/2019
ms.locfileid: "53815905"
---
# <a name="set-up-an-identity-management-server-sql-server-2016"></a>ID 관리 서버 설정: SQL Server 2016

> [!div class="step-by-step"]
> [« Windows Server 2016](prepare-server-ws2016.md)
> [SharePoint »](prepare-server-sharepoint.md)
> 
> [!NOTE]
> 이 연습에서는 Contoso라는 회사의 샘플 이름과 값을 사용합니다. 해당 항목을 사용자의 정보로 바꿉니다. 예를 들면 다음과 같습니다.
> - 도메인 컨트롤러 이름 - **corpdc**
> - 도메인 이름 - **contoso**
> - MIM 서비스 서버 이름 - **corpservice**
> - MIM 동기화 서버 이름 - **corpsync**
> - SQL Server 이름 - **corpsql**
> - 암호 - <strong>Pass@word1</strong>

## <a name="install-sql-server-2016-standardenterprise-edition"></a>**SQL Server 2016 Standard/Enterprise Edition** 설치

1. 도메인 관리자로 **PowerShell** 을 시작합니다.

2. SQL Server 설치 프로그램이 위치한 디렉터리로 변경합니다.

3. 다음 명령을 입력합니다.

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```
    
자세한 SQL 배포 계정 및 서비스 정보를 [여기](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-windows-service-accounts-and-permissions?view=sql-server-2017)에서 찾을 수 있습니다.
> [!NOTE]
> SSMS는 더 이상 SQL 2016에 포함되지 않습니다. 다운로드 세부 내용은 [여기](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)를 확인하세요.
> 
> [!div class="step-by-step"]  
> [« Windows Server 2016](prepare-server-ws2016.md)
> [SharePoint »](prepare-server-sharepoint.md)
