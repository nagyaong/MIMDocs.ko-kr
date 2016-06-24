---
# required metadata

title: 도메인 설정 | Microsoft Identity Manager
description: MIM 2016을 설치하기 전에 Active Directory 도메인 컨트롤러를 만듭니다.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# 도메인 설정

>[!div class="step-by-step"]  
[Windows Server 2012 R2 »](prepare-server-ws2012r2.md)

## 사용자 계정 및 그룹 만들기

MIM을 사용하려면 Active Directory가 이미 설치되어 있어야 합니다. 사용자 환경에 관리할 수 있는 도메인에 대한 도메인 컨트롤러가 있어야 합니다.

> [!NOTE]
> 아래의 모든 예제에서 **mimservername**은 도메인 컨트롤러의 이름을 나타내고 **contoso**는 도메인 이름을 나타내며 **Pass@word1**은 예제 암호를 나타냅니다.

1. 도메인 컨트롤러에 도메인 관리자(*예: Contoso\Administrator*)로 로그인합니다.

2. MIM 서비스에 대한 다음 사용자 계정을 만듭니다. PowerShell을 시작하고 다음 PowerShell 스크립트를 입력하여 도메인을 업데이트합니다.

        ```
        import-module activedirectory
        $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
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
        ```

2.  모든 그룹에 보안 그룹을 만듭니다.

    ```
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global      –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global       –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global         –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global      –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordReset –GroupCategory Security –GroupScope Global          –SamAccountName MIMSyncPasswordReset
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMService
    ```

3.  SPN을 추가하여 서비스 계정에 대해 Kerberos 인증을 사용하도록 설정합니다.

    ```
    setspn -S http/mimservername.contoso.local Contoso\SharePoint
    setspn -S http/mimservername Contoso\SharePoint
    setspn -S MIMService/mimservername.contoso.local Contoso\MIMService
    setspn -S MIMSync/mimservername.contoso.local Contoso\MIMSync
    ```

>[!div class="step-by-step"]  
[Windows Server 2012 R2 »](prepare-server-ws2012r2.md)


<!--HONumber=Apr16_HO2-->


