---
title: PAM 배포 6단계 – 그룹 이동 | Microsoft 문서
description: 그룹을 Privilege Access Management를 사용하여 관리할 수 있도록 PRIV 포리스트로 마이그레이션합니다.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 7b689eff-3a10-4f51-97b2-cb1b4827b63c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 0473ea9e01a44aeb4acbebe01e6e6f989c8581d0
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379314"
---
# <a name="step-6--transition-a-group-to-privileged-access-management"></a>6단계 – 그룹을 권한 있는 액세스 관리로 전환

> [!div class="step-by-step"]
> [« 5단계 ](step-5-establish-trust-between-priv-corp-forests.md)
> [7단계 »](step-7-elevate-user-access.md)

PowerShell cmdlet을 사용하여 PRIV 포리스트의 권한 있는 계정 만들기를 수행했습니다. 이러한 cmdlet은 다음 기능을 수행합니다.

- CORP 포리스트의 그룹과 동일한 SID(보안 식별자)를 사용하여 PRIV 포리스트에 새 그룹을 만듭니다.  
- PRIV 포리스트의 그룹에 해당하는 MIM 서비스 데이터베이스에 개체를 만듭니다.  
- 각 사용자 계정에 대해 MIM 서비스 데이터베이스에 두 개체를 만듭니다. 이는 CORP 포리스트의 사용자와 PRIV 포리스트의 새 사용자 계정에 해당합니다.  
- MIM 서비스 데이터베이스에 PAM 역할 개체를 만듭니다.  

cmdlet은 각 그룹에 대해 한 번, 그룹의 각 구성원에 대해서 한 번 실행되어야 합니다. 마이그레이션 cmdlet은 CORP 포리스트의 사용자 또는 그룹을 변경하거나 수정하지 않습니다. 변경이나 수정은 PAM 관리자가 나중에 수동으로 실행합니다.

1. 직접 또는 PRIV 워크스테이션에서 *PRIV\MIMAdmin*으로 PAMSRV에 로그인합니다.

2.  PowerShell을 시작하고 다음 명령을 입력합니다.

```PowerShell
   Import-Module MIMPAM
   Import-Module ActiveDirectory
```

3. 데모용으로 PRIV에서 기존 포리스트의 사용자 계정에 대한 해당 사용자 계정을 만듭니다.

   PowerShell에 다음 명령을 입력합니다.  이전에 contoso.local에 사용자를 만들 때 이름 *Jen*을 사용하지 않은 경우에는 명령의 매개 변수를 적절하게 변경합니다. 암호 'Pass@word1'은 예시일 뿐이며 고유 암호 값으로 변경해야 합니다.

   ```PowerShell
       $sj = New-PAMUser –SourceDomain CONTOSO.local –SourceAccountName Jen
       $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
       Set-ADAccountPassword –identity priv.Jen –NewPassword $jp
       Set-ADUser –identity priv.Jen –Enabled 1
   ```

4. 데모용으로 CONTOSO에서 PRIV 도메인으로 그룹 및 해당 구성원, Jen을 복사합니다.

    다음 명령을 실행하여 CORP 도메인 관리자(CONTOSO\Administrator) 암호를 지정합니다(요구될 경우).

   ```PowerShell
        $ca = get-credential –UserName CONTOSO\Administrator –Message "CORP forest domain admin credentials"
        $pg = New-PAMGroup –SourceGroupName "CorpAdmins" –SourceDomain CONTOSO.local                 –SourceDC CORPDC.contoso.local –Credentials $ca
        $pr = New-PAMRole –DisplayName "CorpAdmins" –Privileges $pg –Candidates $sj
   ```

    참조를 위해 **New-PAMGroup** 명령은 다음 매개 변수를 사용합니다.

     -   NetBIOS 형식의 CORP 포리스트 도메인 이름  
     -   해당 도메인에서 복사할 그룹의 이름  
     -   CORP 포리스트 도메인 컨트롤러 NetBIOS 이름  
     -   CORP 포리스트의 도메인 관리자 사용자의 자격 증명  

5. (선택 사항) CORPDC에 Jen의 계정이 여전히 있는 경우 **CONTOSO CorpAdmins** 그룹에서 제거합니다.  이 작업은 데모용으로만 필요하며, PRIV 포리스트에서 만든 계정과 사용 권한을 연결하는 방법을 설명합니다.

   1.  *CONTOSO\Administrator*로 CORPDC에 로그인합니다.

   2.  PowerShell을 시작하고 다음 명령을 실행하여 변경 내용을 확인합니다.

       ```PowerShell
       Remove-ADGroupMember -identity "CorpAdmins" -Members "Jen"
       ```


크로스 포리스트 액세스 권한이 사용자의 관리자 계정에 유효한지 시연하려면 다음 단계를 진행합니다.

> [!div class="step-by-step"]
> [« 5단계 ](step-5-establish-trust-between-priv-corp-forests.md)
> [7단계 »](step-7-elevate-user-access.md)
