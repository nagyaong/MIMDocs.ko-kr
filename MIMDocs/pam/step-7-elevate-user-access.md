---
title: PAM 배포 7단계 – 사용자 액세스 | Microsoft Docs
description: 마지막 단계로 Privileged Access Management 배포가 성공했음을 보여 주기 위해 권한 있는 사용자에게 임시 액세스 권한을 부여합니다.
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 01/17/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5325fce2-ae35-45b0-9c1a-ad8b592fcd07
ms.openlocfilehash: 150e850e9184fef189b00e6aee3fab50939f47b9
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332819"
---
# <a name="step-7--elevate-a-users-access"></a>7단계 - 사용자의 액세스 권한 상승

> [!div class="step-by-step"]
> [« 6단계](step-6-transition-group-to-pam.md)


이 단계에서는 사용자가 MIM을 통해 역할에 대한 액세스를 요청할 수 있다는 것을 보여 줍니다.

## <a name="verify-that-jen-cannot-access-the-privileged-resource"></a>Jen이 권한 있는 리소스에 액세스할 수 없는지 확인

상승된 권한 없이는 Jen이 CORP 포리스트에서 권한 있는 리소스에 액세스할 수 없습니다.

1. CORPWKSTN에서 로그아웃하여 모든 캐시된 열린 연결을 제거합니다.
2. CORPWKSTN에 *CONTOSO\Jen*으로 로그인하고 **바탕 화면** 보기로 전환합니다.
3. DOS 명령 프롬프트를 엽니다.
4. `dir \\corpwkstn\corpfs` 명령을 입력합니다. 오류 메시지 **액세스가 거부되었습니다**가 표시됩니다.
5. 명령 프롬프트 창을 열어 둡니다.

## <a name="request-privileged-access-from-mim"></a>MIM에서 권한 있는 액세스를 요청합니다.

> [!NOTE]
> 워크스테이션은 PAW(권한 있는 워크스테이션)인 것이 좋습니다.  자세한 내용은 [PAW](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)를 참조하세요.

1. PRIVWKSTN에서 PRIV\priv.jen으로 로그온합니다.
2. **시작**, **실행**을 클릭하고 **PowerShell.exe**를 입력합니다.
3. 다음 명령을 입력합니다.

    ```cmd
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

2. 메시지가 나타나면 PRIV.Jen 계정에 대한 암호를 입력합니다. 새 명령 프롬프트 창이 나타납니다.
3. PowerShell 창이 나타나면 다음 명령을 입력합니다.

    > [!NOTE]
    > 이러한 명령을 실행한 후 다음 모든 단계는 시간이 중요합니다.

    ```PowerShell
    Import-module MIMPAM
    $r = Get-PAMRoleForRequest | ? { $_.DisplayName –eq "CorpAdmins" }
    New-PAMRequest –role $r
    klist purge
    ```

4. 완료되면 PowerShell 창을 닫습니다.
5. DOS 명령 창에 다음 명령을 입력합니다.

    ```cmd
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

6. PRIV.Jen 계정의 암호를 입력합니다. 새 명령 프롬프트 창이 나타납니다.

## <a name="validate-the-elevated-access"></a>권한이 상승된 액세스의 유효성을 검사합니다.
새로 열린 창에 다음 명령을 입력합니다.

```cmd
whoami /groups
dir \\corpwkstn\corpfs
```

**액세스가 거부되었습니다.** 오류 메시지와 함께 dir 명령이 실패하는 경우 트러스트 관계를 다시 확인합니다.

## <a name="activate-the-privileged-role"></a>권한 있는 역할 활성화

PAM 샘플 포털을 통해 권한 있는 액세스를 요청하여 활성화합니다.

1. CORPWKSTN에서 CORP\Jen으로 로그인되어 있는지 확인합니다.
2. DOS 명령 창에 다음 명령을 입력합니다.

    ```cmd
    runas /user:Priv.Jen@priv.contoso.local "c:\program files\Internet Explorer\iexplore.exe"
    ```

3. 메시지가 나타나면 PRIV.Jen 계정에 대한 암호를 입력합니다. 새 웹 브라우저 창이 나타납니다.
4. 으로 http://pamsrv.priv.contoso.local:8090 이동하여 샘플 포털의 웹 페이지가 표시되는지 확인합니다.
5. Internet Explorer에서 **도구** > **인터넷 옵션**을 선택하고 **보안** 탭을 클릭합니다.
6. **로컬 인트라넷 영역** > **사이트** > **고급**을 차례로 클릭하고 웹 사이트를 영역에 추가합니다.
7. **인터넷 옵션** 대화 상자를 닫습니다.
8. 왼쪽 탭에서 **활성화**를 클릭합니다. **PAM 역할**을 선택하고 **활성화**를 클릭합니다.

> [!Note]
> 이 환경에서는 [Privileged Access Management REST API Reference](/microsoft-identity-manager/reference/privileged-access-management-rest-api-reference)(Privileged Access Management REST API 참조)에 설명된 것처럼 PAM REST API를 사용하는 응용 프로그램을 개발하는 방법도 알아볼 수 있습니다.

## <a name="summary"></a>요약

이 연습의 단계를 완료했으면 Privileged Access Management 시나리오를 확인한 것입니다. 이 시나리오에서는 제한된 기간 동안 사용자 권한이 상승되므로 사용자는 보호된 리소스에 별도의 권한 있는 계정으로 액세스할 수 있습니다. 권한 상승 세션이 만료되는 즉시 권한 있는 계정은 더 이상 보호된 리소스에 액세스할 수 없습니다. 어떤 보안 그룹이 권한 있는 역할을 나타낼지에 대한 결정은 PAM 관리자에 의해 조정됩니다. 액세스 권한이 Privileged Access Management 시스템으로 마이그레이션되면 원래 사용자 계정으로 이전에 가능했던 액세스가 이제는 특별한 권한 있는 계정으로 로그인해야만 가능하고 요청 시 사용할 수 있습니다. 따라서 높은 권한 있는 그룹의 그룹 구성원 자격은 제한된 기간 동안 유효합니다.

> [!div class="step-by-step"]
> [« 6단계](step-6-transition-group-to-pam.md)
