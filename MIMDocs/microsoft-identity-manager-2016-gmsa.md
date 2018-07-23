---
title: MIM 특정 서비스를 gMSA로 변환 | Microsoft Docs
description: gMSA를 구성하는 기본 단계를 설명하는 항목입니다.
author: fimguy
ms.author: billmath
manager: mtillman
ms.date: 06/27/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.openlocfilehash: 090e82cac6c734beb9767e2d2e6230320e44c26f
ms.sourcegitcommit: c6cb2556bb9f2256b959a3c95db7ca5bbfc2b437
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37065145"
---
# <a name="conversion-of-mim-specific-services-to-gmsa"></a>MIM 특정 서비스를 gMSA로 변환

이 가이드에서는 지원되는 서비스에 대해 gMSA를 구성하는 기본 단계를 안내합니다. 환경을 미리 구성한 경우, gMSA로 변환하는 프로세스는 매우 간단합니다.

핫픽스 필요: \<최신 KB에 대한 링크\>

지원됨:

-   MIM 동기화 서비스(FIMSynchronizationService)
-   MIM 서비스(FIMService)
-   MIM 암호 등록
-   MIM 암호 재설정
-   PAM 모니터링 서비스(PamMonitoringService)
-   PAM 구성 요소 서비스(PrivilegeManagementComponentService)

지원되지 않음:

-   MIM 포털은 지원되지 않음. 이 포털은 SharePoint 환경의 일부이므로 팜 모드에서 배포하고 [SharePointServer에서 자동 암호 변경을 구성](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change)해야 합니다.
-   모든 관리 에이전트
-   Microsoft 인증서 관리
-   BHOLD

<a name="general-information"></a>일반 정보 
--------------------

설정을 완료하고 이해하는 데 필요한 읽기 자료

-   [그룹 관리 서비스 계정 개요](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   <https://docs.microsoft.com/en-us/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps>

-   <https://technet.microsoft.com/en-us/library/jj128430(v=ws.11).aspx>

Windows 도메인 컨트롤러의 첫 번째 단계

1.  필요한 경우, KDS(키 배포 서비스) 루트 키를 도메인당 한 개만 만듭니다. 루트 키는 DC의 KDS 서비스에서 다른 정보와 함께 사용하여 암호를 생성합니다.

    -   Add-KDSRootKey –EffectiveImmediately

    -   “–EffectiveImmediately”는 모든 DC에 복제하는 데 최대 \~10시간까지 기다려야 한다는 의미입니다. 도메인 컨트롤러 두 개에 약 1시간 정도 소요됩니다.

![](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="synchronization-service"></a>동기화 서비스
-----------------------

1.  첫 번째 단계에서는 그룹 호출 “MIMSync_Servers”를 만들고 모든 동기화 서버를 이 그룹에 추가합니다.

![](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

2.  그런 다음, Windows PowerShell에서 이미 도메인에 가입된 컴퓨터 계정을 사용하는 도메인 관리자로 아래 명령을 실행합니다.

    -   New-ADServiceAccount -Name MIMSyncGMSAsvc -DNSHostName MIMSyncGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword “MIMSync_Servers”

![](media/f6deb0664553e01bcc6b746314a11388.png)

-   동기화에 대한 GSMA의 세부 정보를 가져옵니다.

![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

-   PCNS 서비스를 실행하는 경우 위임을 업데이트해야 합니다.

    -   Set-ADServiceAccount -Identity MIMSyncGMSAsvc -ServicePrincipalNames \@{Add=“PCNSCLNT/mimsync.contoso.com”}

3. 다음으로 동기화 서비스에서 변경 모드 설치 시 요청되는 암호화 키를 백업해야 합니다.

    -   동기화 서비스가 설치되는 서버에서 동기화 서비스 키 관리 도구를 찾습니다.

    -   기본적으로 **내보내기 키 집합**이 이미 선택되어 있습니다.

    -   **다음**을 클릭합니다.

    -   이제 기존 동기화 계정 정보를 입력하라는 메시지가 표시됩니다.

    -   FIM 동기화 계정 정보를 입력하고 확인합니다.

        -   계정 이름 - 초기 설치 중 사용된 동기화 서비스 계정의 계정 이름

        -   암호 - 동기화 서비스 계정의 암호

        -   도메인 - 동기화 서비스 계정이 속한 도메인

    -   **다음**을 클릭합니다.

    -   정보를 잘못 입력한 경우, 다음 오류가 표시됩니다.

    -   이제 계정 정보를 입력했으므로 백업 암호화 키의 대상(내보내기 파일 위치)을 변경하는 옵션이 표시됩니다.

        -   기본적으로 내보내기 파일 위치는 **C:\\Windows\\system32**\\miiskeys-1.bin입니다.

4. Microsoft Identity Manager SP1 동기화 서비스 빌드 4.4.1302.0을 설치합니다. 볼륨 라이선스 다운로드 센터 또는 MSDN 다운로드 사이트에서 찾을 수 있습니다. 설치를 완료했으면 키 집합 miiskeys.bin을 저장해야 합니다.

![](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)


5. 최신 [핫픽스 4.5.x.x](https://docs.microsoft.com/en-us/microsoft-identity-manager/reference/version-history) 이상을 설치합니다.

- 패치를 적용했으면 FIM 동기화 서비스를 중지합니다.
- 제어판 [프로그램 및 기능] Microsoft Identity Manager
- 동기화 서비스 [변경] -\> [다음] -\> [구성] -\> [다음]

![](media/dc98c011bec13a33b229a0e792b78404.png)

-  계정 이름 지우기
-  스크린샷과 같이 \$ 기호와 함께 서비스 계정 이름 **MIMSyncGMSA**를
- 입력합니다. 암호는 비워 둡니다.

![](media/38df9369bf13e1c3066a49ed20e09041.png)

- [다음]-\> [다음]-\> [설치]
- 저장된 .bin 파일에서 키 집합을 복원합니다.

![](media/44cd474323584feb6d8b48b80cfceb9b.png)

![](media/03e7762f34750365e963f0b90e43717c.png)
> [!NOTE]
> 추가된 SQL 권한은 로그인을 위해 만든 계정이므로 사용자가 변경 모드 권한을 적용하여 동기화 서비스 데이터베이스에서 계정 및 dbo를 추가하도록 해야 합니다.

## <a name="mim-service"></a>MIM 서비스
-----------

>[!IMPORTANT]
>처음으로 MIM 서비스 관련 계정을 gMSA 계정으로 변환하는 경우 다음 프로세스를 사용해야 합니다. 부록에 명시된 PowerShell cmdlet은 초기 구성이 수행된 다음 계정 정보를 변경할 때만 사용할 수 있습니다.*

1.  MIM 서비스, PAM Rest API, PAM 모니터링 서비스, PAM 구성 요소 서비스, SSPR 등록 포털, SSPR 재설정 포털에 대한 그룹 관리 계정을 만듭니다.

    -   gMSA 위임 및 SPN을 업데이트해야 합니다.
        -   Set-ADServiceAccount -Identity \<account\> -ServicePrincipalNames \@{Add=“\<SPN\>”}
        -   Delegation
            -   Set-ADServiceAccount -Identity \<gsmaaccount\> -TrustedForDelegation \$true
        -   제한된 위임
            -   \$delspns = ‘http/mim’, ‘http/mim.contoso.com’
            -   New-ADServiceAccount -Name \<gsmaaccount\> -DNSHostName \<gsmaaccount\>.contoso.com -PrincipalsAllowedToRetrieveManagedPassword \<group\> -ServicePrincipalNames \$spns -OtherAttributes \@{‘msDS-AllowedToDelegateTo’=\$delspns }

2.  동기화 그룹에서 MIM 서비스에 대한 계정을 추가합니다. SSPR에 필요합니다.

![](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

3.  **참고**.  Windows를 다시 시작한 후 시작되지 않는 Microsoft 키 배포 서비스로 인해 관리 계정을 사용하는 서비스가 서버를 다시 시작한 후 중지되는 알려진 문제가 있습니다. 서비스를 시작할 수 없으며 Windows도 다시 시작할 수 없습니다. 이 문제는 Windows Server 2012 R2 이상에서 재현 가능합니다. 이 문제의 해결 방법은 다음 명령을 실행하는 것입니다. 

-   **sc triggerinfo kdssvc start/networkon**

    이 명령을 실행하여 네트워크가 작동 중일 때(일반적으로 부트 주기 초기) Microsoft 키 배포 서비스를 시작합니다.

    <https://social.technet.microsoft.com/Forums/en-US/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS>에서 유사한 문제에 대한 토론을 참조하세요.

4.  MIM 서비스의 관리자 권한 MSI를 실행하고 [변경]을 선택합니다.

5.  “Configure main server connection”(주 서버 연결 구성) 페이지에서 “Use different account for Exchange (for managed accounts)”(Exchange에 다른 계정 사용(관리되는 계정의 경우)) 확인란을 선택합니다. 여기에서 사서함이 있는 이전 계정을 사용하거나 클라우드 사서함을 사용하는 옵션이 있습니다.

![](media/0cd8ce521ed7945c43bef6100f8eb222.png)

6.  “Configure MIM Service account”(MIM 서비스 계정 구성) 페이지에서 끝에 \$ 기호를 포함하여 서비스 계정을 입력합니다. 서비스 메일 계정 암호도 입력합니다. 서비스 계정 암호는 비활성화되어 있어야 합니다.

![](media/db0d543df6e1b0174a47135617c23fcb.png)

7.  LogonUser 기능이 관리되는 계정에 대해 작동하지 않으므로 다음 페이지에서 “Please check if Service Account is secure in its current configuration”(서비스 계정이 현재 구성에서 안전한지 확인하세요.)이라는 경고가 표시됩니다.

![cid:image007.png\@01D36EB7.562E6CF0](media/d350bc13751b2d0a884620db072ed019.png)

8.  “Configure Privileged Access Management REST API”(Privileged Access Management REST API 구성) 페이지에서 끝에 \$ 기호를 포함하여 응용 프로그램 풀 계정 이름을 입력하고 암호 필드는 비워 둡니다.

![](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

9.  “Configure PAM Component Service”(PAM 구성 요소 서비스 구성) 페이지에서 끝에 \$ 기호를 포함하여 서비스 계정 이름을 입력하고 암호 필드는 비워 둡니다.

![](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

![cid:image010.png\@01D36EB8.A295A3F0](media/9d2b52f6faed10601e7e2166a339fb47.png)

10.  “Configure Privileged Access Management Monitoring Service”(Privileged Access Management 모니터링 서비스 구성) 페이지에서 끝에 \$ 기호를 포함하여 서비스 계정 이름을 입력하고 암호 필드는 비워 둡니다.

![](media/d1e824248edf12a77fc9ffb011475164.png)

11.  “Configure MIM Password Registration Portal”(MIM 암호 등록 포털 구성) 페이지에서 끝에 \$ 기호를 포함하여 계정 이름을 입력하고 암호 필드는 비워 둡니다.

![](media/601e935cdfda298b61ae753a2a152996.png)

12.  “Configure MIM Password Reset Portal”(MIM 암호 재설정 포털 구성) 페이지에서 끝에 \$ 기호를 포함하여 계정 이름을 입력하고 암호 필드는 비워 둡니다.

![](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

13.  설치를 완료합니다.

참고:

-  설치 후 두 개의 새 키가 경로별로 레지스트리에 생성됩니다.
    - 암호화된 Exchange 암호를 저장하려는 경우 “HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Forefront Identity
    - Manager\\2010\\Service”. 하나는
    - Exchange Online용이고 다른 하나는 Exchange 온-프레미스용이며, 둘 중 하나는
    - 비어 있어야 합니다.

![cid:image014.jpg\@01D36F53.303D5190](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

- 암호를 업데이트하기 위해 스크립트를 제공했으며 [여기서 다운로드](microsoft-identity-manager-2016-gmsascript.md)하면 고객이 변경 모드 실행할 필요가 없습니다.

- Exchange 암호를 암호화하기 위해 설치 관리자는 추가 서비스를 만들고
    - 관리되는 계정으로 실행합니다. 설치하는 동안 다음 메시지가
    - 응용 프로그램 이벤트 로그에 추가됩니다.

![cid:image016.jpg\@01D36F53.303D5190](media/95b315454705cd4d939b55ac5ad910f5.jpg)
