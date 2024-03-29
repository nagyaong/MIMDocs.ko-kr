---
title: Microsoft Identity Manager 2016에 대한 도메인 설정 | Microsoft 문서
description: MIM 2016을 설치하기 전에 Active Directory 도메인 컨트롤러를 만듭니다.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/26/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 35845cc9bb4358f3f837b8a007de15da972c980d
ms.sourcegitcommit: 65e11fd639464ed383219ef61632decb69859065
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68701320"
---
# <a name="set-up-a-domain"></a>도메인 설정

> [!div class="step-by-step"]
> [Windows Server 2016 »](prepare-server-ws2016.md)

MIM(Microsoft Identity)은 AD(Active Directory) 도메인에서 작동합니다. AD가 이미 설치되어 있어야 하며 사용자 환경에 관리할 수 있는 도메인에 대한 도메인 컨트롤러가 있어야 합니다.

이 문서에서는 MIM과 함께 작동하도록 도메인을 준비하는 단계를 안내합니다.

## <a name="create-user-accounts-and-groups"></a>사용자 계정 및 그룹 만들기

MIM 배포의 모든 구성 요소에는 도메인에 자체 ID가 있어야 합니다. 여기에는 SharePoint 및 SQL은 물론 서비스와 동기화같은 MIM 구성 요소도 해당됩니다.

> [!NOTE]
> 이 연습에서는 Contoso라는 회사의 샘플 이름과 값을 사용합니다. 해당 항목을 사용자의 정보로 바꿉니다. 예를 들면 다음과 같습니다.
> - 도메인 컨트롤러 이름 - **corpdc**
> - 도메인 이름 - **contoso**
> - MIM 서비스 서버 이름 - **corpservice**
> - MIM 동기화 서버 이름 - **corpsync**
> - SQL Server 이름 - **corpsql**
> - 암호 - <strong>Pass@word1</strong>

1. 도메인 컨트롤러에 도메인 관리자(*예: Contoso\Administrator*)로 로그인합니다.

2. MIM 서비스에 대한 다음 사용자 계정을 만듭니다. PowerShell을 시작하고 다음 PowerShell 스크립트를 입력하여 도메인을 업데이트합니다.

    ```PowerShell
    import-module activedirectory
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    New-ADUser –SamAccountName MIMINSTALL –name MIMMA
    Set-ADAccountPassword –identity MIMINSTALL –NewPassword $sp
    Set-ADUser –identity MIMINSTALL –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMMA –name MIMMA
    Set-ADAccountPassword –identity MIMMA –NewPassword $sp
    Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMSync –name MIMSync
    Set-ADAccountPassword –identity MIMSync –NewPassword $sp
    Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMService –name MIMService
    Set-ADAccountPassword –identity MIMService –NewPassword $sp
    Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMSSPR –name MIMSSPR
    Set-ADAccountPassword –identity MIMSSPR –NewPassword $sp
    Set-ADUser –identity MIMSSPR –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName SharePoint –name SharePoint
    Set-ADAccountPassword –identity SharePoint –NewPassword $sp
    Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName SqlServer –name SqlServer
    Set-ADAccountPassword –identity SqlServer –NewPassword $sp
    Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName BackupAdmin –name BackupAdmin
    Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp
    Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMpool –name BackupAdmin
    Set-ADAccountPassword –identity MIMPool –NewPassword $sp
    Set-ADUser –identity MIMPool –Enabled 1 -PasswordNeverExpires 1
    ```

3.  모든 그룹에 보안 그룹을 만듭니다.

    ```PowerShell
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordReset –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncPasswordReset
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMService
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMInstall
    ```

4.  SPN을 추가하여 서비스 계정에 대해 Kerberos 인증을 사용하도록 설정합니다.

    ```CMD
    setspn -S http/mim.contoso.com Contoso\mimpool
    setspn -S http/mim Contoso\mimpool
    setspn -S http/passwordreset.contoso.com Contoso\mimsspr
    setspn -S http/passwordregistration.contoso.com Contoso\mimsspr
    setspn -S FIMService/mim.contoso.com Contoso\MIMService
    setspn -S FIMService/corpservice.contoso.com Contoso\MIMService
    ```
5.  설치 중에 적절한 이름 확인을 위해 다음 DNS 'A' 레코드를 추가해야 합니다.

- mim.contoso.com corpservice의 물리적 IP 주소를 가리킵니다.
- passwordreset.contoso.com corpservice의 물리적 IP 주소를 가리킵니다.
- passwordregistration.contoso.com corpservice의 물리적 IP 주소를 가리킵니다.

> [!div class="step-by-step"]
> [Windows Server 2016 »](prepare-server-ws2016.md)
