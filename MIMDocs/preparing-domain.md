---
title: "Microsoft Identity Manager 2016에 대한 도메인 설정 | Microsoft 문서"
description: "MIM 2016을 설치하기 전에 Active Directory 도메인 컨트롤러를 만듭니다."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: bd9c0da17c97cfc15023ad624a249e0f4a2d0825
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# 도메인 설정
<a id="set-up-a-domain" class="xliff"></a>

>[!div class="step-by-step"]
[Windows Server 2012 R2 »](prepare-server-ws2012r2.md)

MIM(Microsoft Identity)은 AD(Active Directory) 도메인에서 작동합니다. AD가 이미 설치되어 있어야 하며 사용자 환경에 관리할 수 있는 도메인에 대한 도메인 컨트롤러가 있어야 합니다.

이 문서에서는 MIM과 함께 작동하도록 도메인을 준비하는 단계를 안내합니다.

## 사용자 계정 및 그룹 만들기
<a id="create-user-accounts-and-groups" class="xliff"></a>

MIM 배포의 모든 구성 요소에는 도메인에 자체 ID가 있어야 합니다. 여기에는 SharePoint 및 SQL은 물론 서비스와 동기화같은 MIM 구성 요소도 해당됩니다.

> [!NOTE]
> 이 연습에서는 Contoso라는 회사의 샘플 이름과 값을 사용합니다. 해당 항목을 사용자의 정보로 바꿉니다. 예를 들면 다음과 같습니다.
> - 도메인 컨트롤러 이름 - **mimservername**
> - 도메인 이름 - **contoso**
> - 암호 - **Pass@word1**

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

3.  모든 그룹에 보안 그룹을 만듭니다.

    ```
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordReset –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncPasswordReset
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMService
    ```

4.  SPN을 추가하여 서비스 계정에 대해 Kerberos 인증을 사용하도록 설정합니다.

    ```
    setspn -S http/mimservername.contoso.local Contoso\SharePoint
    setspn -S http/mimservername Contoso\SharePoint
    setspn -S FIMService/mimservername.contoso.local Contoso\MIMService
    setspn -S FIMSynchronizationService/mimservername.contoso.local Contoso\MIMSync
    ```

>[!div class="step-by-step"]
[Windows Server 2012 R2 »](prepare-server-ws2012r2.md)
