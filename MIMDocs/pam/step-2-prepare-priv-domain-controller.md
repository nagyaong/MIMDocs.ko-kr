---
title: PAM 배포 2단계 - PRIV DC | Microsoft 문서
description: Privileged Access Management가 격리되는 배스천 환경을 제공하는 PRIV 도메인 컨트롤러를 준비합니다.
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/14/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 0e9993a0-b8ae-40e2-8228-040256adb7e2
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 79aa0937ce21bd0c2424c597337e5ab0aa66d3c4
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333635"
---
# <a name="step-2---prepare-the-first-priv-domain-controller"></a>2단계 - 첫 번째 PRIV 도메인 컨트롤러 준비

> [!div class="step-by-step"]
> [« 1단계](step-1-prepare-corp-domain.md)
> [3단계 »](step-3-prepare-pam-server.md)

이 단계에서는 관리자 인증에 배스천 환경을 제공하는 새 도메인을 만듭니다.  이 포리스트에는 하나 이상의 도메인 컨트롤러와 하나 이상의 구성원 서버가 필요합니다. 구성원 서버는 다음 단계에서 구성합니다.

## <a name="create-a-new-privileged-access-management-domain-controller"></a>새 Privileged Access Management 도메인 컨트롤러 만들기

이 섹션에서는 새 포리스트의 도메인 컨트롤러 역할을 수행할 가상 머신을 설정합니다.

### <a name="install-windows-server-2012-r2"></a>Windows Server 2012 R2 설치

소프트웨어가 설치되지 않은 또 하나의 새 가상 머신에 Windows Server 2012 R2를 설치하여 “PRIVDC” 컴퓨터를 만듭니다.

1. Windows Server의 사용자 지정(업그레이드되지 않음) 설치를 수행하려면 선택합니다. 설치할 때 **Windows Server 2012 R2 Standard(GUI 포함 서버) x64**를 지정합니다. **데이터 센터 또는 Server Core**를 _선택하지 마세요_.

2. 사용 조건을 검토하고 이에 동의합니다.

3. 디스크가 비어 있으므로 **사용자 지정: Windows만 설치**를 선택하고 초기화되지 않은 디스크 공간을 사용합니다.

4. 운영 체제 버전을 설치한 후 이 새 컴퓨터에 새 관리자로 로그인합니다. 제어판에서 컴퓨터 이름을 *PRIVDC*로 설정하고 가상 네트워크의 고정 IP 주소를 지정한 다음 DNS 서버를 이전 단계에서 설치한 도메인 컨트롤러의 DNS 서버가 되도록 구성합니다. 이 경우 서버를 다시 시작해야 합니다.

5. 서버가 다시 시작되면 관리자로 로그인합니다. 제어판에서 업데이트를 확인하도록 컴퓨터를 구성하고 필요한 업데이트를 설치합니다. 이 경우 서버를 다시 시작해야 할 수 있습니다.

### <a name="add-roles"></a>역할 추가

AD DS(Active Directory 도메인 서비스) 및 DNS 서버 역할을 추가합니다.

1. 관리자 권한으로 PowerShell을 시작합니다.

2. Windows Server Active Directory 설치를 준비하려면 다음 명령을 입력합니다.

   ```PowerShell
   import-module ServerManager

   Install-WindowsFeature AD-Domain-Services,DNS –restart –IncludeAllSubFeature -IncludeManagementTools
   ```

### <a name="configure-registry-settings-for-sid-history-migration"></a>SID 기록 마이그레이션에 대한 레지스트리 설정을 구성합니다.

PowerShell을 시작하고 SAM(보안 계정 관리자) 데이터베이스에 대한 RPC(원격 프로시저 호출) 액세스를 허용하도록 원본 도메인을 구성하는 다음 명령을 입력합니다.

```PowerShell
New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1
```

## <a name="create-a-new-privileged-access-management-forest"></a>새 Privileged Access Management 포리스트 만들기

다음으로, 서버를 새 포리스트의 도메인 컨트롤러로 승격합니다.

이 문서에서 이름 priv.contoso.local은 새 포리스트의 도메인 이름으로 사용됩니다.  포리스트의 이름은 중요하지 않으며 조직의 기존 포리스트 이름에 종속될 필요가 없습니다. 그러나 새 포리스트의 도메인 및 NetBIOS 이름은 모두 고유해야 하며 조직에 있는 다른 도메인의 경우와 고유해야 합니다.  

### <a name="create-a-domain-and-forest"></a>도메인 및 포리스트 만들기

1. PowerShell 창에서 다음 명령을 입력하여 새 도메인을 만듭니다.  이렇게 하면 이전 단계에서 만든 상위 도메인(contoso.local)에서 DNS 위임도 만듭니다.  나중에 DNS를 구성하려면 `CreateDNSDelegation -DNSDelegationCredential $ca` 매개 변수를 생략합니다.

   ```PowerShell
   $ca= get-credential
   Install-ADDSForest –DomainMode 6 –ForestMode 6 –DomainName priv.contoso.local –DomainNetbiosName priv –Force –CreateDNSDelegation –DNSDelegationCredential $ca
   ```

2. 팝업이 표시되면 CORP 포리스트 관리자에 대한 자격 증명(예: 1단계의 사용자 이름 CONTOSO\\Administrator 및 해당 암호)을 입력합니다.

3. 사용할 안전 모드 관리자 암호를 입력하라는 메시지가 PowerShell 창에 표시됩니다. 새 암호를 두 번 입력합니다. DNS 위임 및 암호화 설정에 대한 경고 메시지가 표시되지만 이는 정상입니다.

포리스트 만들기가 완료되면 서버가 자동으로 다시 시작합니다.

### <a name="create-user-and-service-accounts"></a>사용자 및 서비스 계정 만들기

MIM 서비스 및 포털 설정을 위한 사용자 및 서비스 계정을 만듭니다. 이러한 계정은 priv.contoso.local 도메인의 사용자 컨테이너에서 사용됩니다.

1. 서버를 다시 시작한 후 도메인 관리자(PRIV\\Administrator)로 PRIVDC에 로그인합니다.

2. PowerShell을 시작하고 다음 명령을 입력합니다. 암호 'Pass@word1'은 예시일 뿐이며 계정에 다른 암호를 사용합니다.

   ```PowerShell
   import-module activedirectory

   $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

   New-ADUser –SamAccountName MIMMA –name MIMMA

   Set-ADAccountPassword –identity MIMMA –NewPassword $sp

   Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName MIMMonitor –name MIMMonitor -DisplayName MIMMonitor

   Set-ADAccountPassword –identity MIMMonitor –NewPassword $sp

   Set-ADUser –identity MIMMonitor –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName MIMComponent –name MIMComponent -DisplayName MIMComponent

   Set-ADAccountPassword –identity MIMComponent –NewPassword $sp

   Set-ADUser –identity MIMComponent –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName MIMSync –name MIMSync

   Set-ADAccountPassword –identity MIMSync –NewPassword $sp

   Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName MIMService –name MIMService

   Set-ADAccountPassword –identity MIMService –NewPassword $sp

   Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName SharePoint –name SharePoint

   Set-ADAccountPassword –identity SharePoint –NewPassword $sp

   Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName SqlServer –name SqlServer

   Set-ADAccountPassword –identity SqlServer –NewPassword $sp

   Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName BackupAdmin –name BackupAdmin

   Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp

   Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1

   New-ADUser -SamAccountName MIMAdmin -name MIMAdmin

   Set-ADAccountPassword –identity MIMAdmin  -NewPassword $sp

   Set-ADUser -identity MIMAdmin -Enabled 1 -PasswordNeverExpires 1

   Add-ADGroupMember "Domain Admins" SharePoint

   Add-ADGroupMember "Domain Admins" MIMService
   ```

### <a name="configure-auditing-and-logon-rights"></a>감사 및 로그온 권한 구성

PAM 구성을 포리스트에서 설정하려면 감사를 설정해야 합니다.

1. 도메인 관리자(PRIV\\Administrator)로 로그인해야 합니다.

2. **시작** > **관리 도구** > **그룹 정책 관리**로 이동합니다.

3. **포리스트: priv.contoso.local** > **도메인** > **priv.contoso.local** > **도메인 컨트롤러** > **기본 도메인 컨트롤러 정책**으로 이동합니다. 경고 메시지가 나타납니다.

4. **기본 도메인 컨트롤러 정책**을 마우스 오른쪽 단추로 클릭하고 **편집**을 선택합니다.

5. 그룹 정책 관리 편집기 콘솔 트리에서 **컴퓨터 구성** > **정책** > **Windows 설정** > **보안 설정** > **로컬 정책** > **감사 정책**으로 이동합니다.

6. 세부 정보 창에서 **계정 관리 감사**를 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다. **이 정책 설정 정의**를 클릭하고 **성공** 확인란을 선택한 후 **실패** 확인란을 클릭하고 **적용** , **확인**을 차례로 클릭합니다.

7. 세부 정보 창에서 **디렉터리 서비스 액세스 감사**를 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다. **이 정책 설정 정의**를 클릭하고 **성공** 확인란을 선택한 후 **실패** 확인란을 클릭하고 **적용** , **확인**을 차례로 클릭합니다.

8. **컴퓨터 구성** > **정책** > **Windows 설정** > **보안 설정** > **계정 정책** > **Kerberos 정책**으로 이동합니다.

9. 세부 정보 창에서 **사용자 티켓 최대 수명**을 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다. **이 정책 설정 정의**를 클릭하고 시간을 *1*로 설정한 후 **적용** , **확인**을 차례로 클릭합니다. 창의 다른 설정도 변경됩니다.

10. 그룹 정책 관리 창에서 **기본 도메인 정책**을 선택하고 마우스 오른쪽 단추를 클릭하고 **편집**을 선택합니다.

11. **컴퓨터 구성** > **정책** > **Windows 설정** > **보안 설정** > **로컬 정책**을 확장하고 **사용자 권한 할당**을 선택합니다.

12. 세부 정보 창에서 **일괄 작업으로 로그온 거부**를 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다.

13. **이 정책 설정 정의** 확인란을 선택하고 **사용자 또는 그룹 추가**를 클릭한 다음 사용자 및 그룹 이름 필드에 *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent*를 입력하고 **확인**을 클릭합니다.

14. **확인**을 클릭하여 창을 닫습니다.

15. 세부 정보 창에서 **원격 데스크톱 서비스를 통한 로그온 거부**를 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다.

16. **이 정책 설정 정의** 확인란을 클릭하고 **사용자 또는 그룹 추가**를 클릭한 다음 사용자 및 그룹 이름 필드에 *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent*를 입력하고 **확인**을 클릭합니다.

17. **확인**을 클릭하여 창을 닫습니다.

18. 그룹 정책 관리 편집기 창 및 그룹 정책 관리 창을 닫습니다.

19. 관리자로 PowerShell 창을 시작하고 다음 명령을 입력하여 그룹 정책 설정에서 DC를 업데이트합니다.

    ```cmd
    gpupdate /force /target:computer
    ```

    잠시 후 "컴퓨터 정책 업데이트가 완료되었습니다."라는 메시지와 함께 완료됩니다.


### <a name="configure-dns-name-forwarding-on-privdc"></a>PRIVDC에서 DNS 이름 전달 구성

PRIVDC에서 PowerShell을 사용하여 PRIV 도메인이 다른 기존 포리스트를 인식하도록 DNS 이름 전달을 구성합니다.

1. PowerShell을 시작합니다.

2. 각 기존 포리스트의 맨 위에 있는 각 도메인에 다음 명령을 입력하여 기존 DNS 도메인(예: contoso.local) 및 해당 도메인의 마스터 서버 IP 주소를 지정합니다.  

   이전 단계에서 하나의 도메인 contoso.local을 만든 경우에는 CORPDC 컴퓨터의 가상 네트워크 IP 주소로 *10.1.1.31*을 지정합니다.

   ```PowerShell
   Add-DnsServerConditionalForwarderZone –name "contoso.local" –masterservers 10.1.1.31
   ```

> [!NOTE]
> 다른 포리스트 또한 이 도메인 컨트롤러에 PRIV 포리스트에 대한 DNS 쿼리를 라우팅할 수 있어야 합니다.  기존 Active Directory 포리스트가 여러 개인 경우 각 해당 포리스트에 DNS 조건부 전달자도 추가해야 합니다.

### <a name="configure-kerberos"></a>Kerberos 구성

1. PowerShell을 사용하여 SharePoint, PAM REST API와 MIM 서비스가 Kerberos 인증을 사용할 수 있도록 SPN을 추가합니다.

   ```cmd
   setspn -S http/pamsrv.priv.contoso.local PRIV\SharePoint
   setspn -S http/pamsrv PRIV\SharePoint
   setspn -S FIMService/pamsrv.priv.contoso.local PRIV\MIMService
   setspn -S FIMService/pamsrv PRIV\MIMService
   ```

> [!NOTE]
> 이 문서의 다음 단계에는 MIM 2016 서버 구성 요소를 단일 컴퓨터에 설치하는 방법을 설명합니다. 고가용성을 위해 다른 서버를 추가하려는 경우 [FIM 2010: Kerberos 인증 설정](http://social.technet.microsoft.com/wiki/contents/articles/3385.fim-2010-kerberos-authentication-setup.aspx)에 설명된 대로 추가 Kerberos 구성이 필요합니다.

### <a name="configure-delegation-to-give-mim-service-accounts-access"></a>MIM 서비스 계정 액세스를 제공하는 위임 구성

PRIVDC에서 도메인 관리자로 다음 단계를 수행합니다.

1. **Active Directory 사용자 및 컴퓨터**를 시작합니다.
2. 도메인 **priv.contoso.local**을 마우스 오른쪽 단추로 클릭하고 **위임 컨트롤**을 선택합니다.
3. 선택한 사용자 및 그룹 탭에서 **추가**를 클릭합니다.
4. 사용자, 컴퓨터 또는 그룹 선택 창에서 *mimcomponent; mimmonitor; mimservice*를 입력하고 **이름 확인**을 클릭합니다. 이름에 밑줄이 표시되면 **확인**을 클릭하고 **다음**을 클릭합니다.
5. 일반 작업 목록에서 **사용자 계정 만들기, 삭제 및 관리**와 **그룹의 구성원 자격 수정**을 선택한 후 **다음**을 클릭하고 **마침**을 클릭합니다.

6. 도메인 **priv.contoso.local**을 마우스 오른쪽 단추로 클릭하고 **위임 컨트롤**을 선택합니다.
7. 선택한 사용자 및 그룹 탭에서 **추가**를 클릭합니다.  
8. 사용자, 컴퓨터 또는 그룹 선택 창에서 *MIMAdmin*을 입력하고 **이름 확인**을 클릭합니다. 이름에 밑줄이 표시되면 **확인**을 클릭하고 **다음**을 클릭합니다.
9. **사용자 지정 작업**을 선택하고 **일반 사용 권한**을 사용하여 **이 폴더**에 적용합니다.
10. 사용 권한 목록에서 다음을 선택합니다.
    - **읽기**
    - **쓰기**
    - **모든 자식 개체 만들기**
    - **모든 자식 개체 삭제**
    - **모든 속성 읽기**
    - **모든 속성 쓰기**
    - **SID 기록 마이그레이션** **다음**, **마침**을 차례로 클릭합니다.

11. 다시 도메인 **priv.contoso.local**을 마우스 오른쪽 단추로 클릭하고 **위임 컨트롤**을 선택합니다.  
12. 선택한 사용자 및 그룹 탭에서 **추가**를 클릭합니다.  
13. 사용자, 컴퓨터 또는 그룹 선택 창에서 *MIMAdmin*을 입력하고 **이름 확인**을 클릭합니다. 이름에 밑줄이 표시되면 **확인**을 클릭하고 **다음**을 클릭합니다.  
14. **사용자 지정 작업**을 선택하고 **이 폴더**에 적용한 다음 **사용자 개체만**을 클릭합니다.    
15. 사용 권한 목록에서 **암호 변경** 및 **암호 다시 설정**을 선택합니다. 그런 후 **다음** 을 클릭하고 **마침**을 클릭합니다.  
16. Active Directory 사용자 및 컴퓨터를 닫습니다.

17. 명령 프롬프트를 엽니다.  
18. PRIV 도메인의 AdminSDHolder 개체에 대한 액세스 제어 목록을 검토합니다. 예를 들어 도메인이 "priv.contoso.local"이면 다음 명령을 입력합니다.
    ```cmd
    dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local"
    ```
19. MIM 서비스 및 MIM 구성 요소 서비스가 이 ACL에서 보호하는 그룹의 구성원 자격을 업데이트할 수 있도록 필요에 따라 액세스 제어 목록을 업데이트합니다.  다음 명령을 입력합니다.

```cmd
dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimservice:WP;"member"
dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimcomponent:WP;"member"
```

20. 이러한 변경 내용이 적용되도록 PRIVDC 서버를 다시 시작합니다.

## <a name="prepare-a-priv-workstation"></a>PRIV 워크스테이션 준비

PRIV 리소스(예: MIM)의 유지 관리를 수행하기 위한 PRIV 도메인에 가입할 워크스테이션 컴퓨터가 아직 없는 경우 다음 지침에 따라 워크스테이션을 준비합니다.  

### <a name="install-windows-81-or-windows-10-enterprise"></a>Windows 8.1 또는 Windows 10 Enterprise 설치

소프트웨어가 설치되지 않은 또 하나의 새 가상 머신에 Windows 8.1 Enterprise 또는 Windows 10 Enterprise를 설치하여 *“PRIVWKSTN”* 컴퓨터를 만듭니다.

1. 설치하는 동안 기본 설정을 사용합니다.

2. 설치할 때 인터넷에 연결하지 못할 수도 있습니다. **로컬 계정 만들기**를 클릭합니다. 다른 사용자 이름을 지정합니다. "Administrator"나 "Jen"은 사용하지 마세요.

3. 제어판에서 이 컴퓨터에 가상 네트워크의 고정 IP 주소를 지정하고 인터페이스의 기본 DNS 서버가 PRIVDC 서버의 DNS 서버가 되도록 설정합니다.

4. 제어판에서 PRIVWKSTN 컴퓨터를 priv.contoso.local 도메인에 도메인 가입합니다. 그러려면 PRIV 도메인 관리자 자격 증명을 제공해야 합니다. 이 작업이 완료되면 PRIVWKSTN 컴퓨터를 다시 시작합니다.

자세한 내용은 [권한 있는 액세스 워크스테이션 보안](https://technet.microsoft.com/library/mt634654.aspx)을 참조하세요.

다음 단계에서는 PAM 서버를 준비합니다.

> [!div class="step-by-step"]
> [« 1단계](step-1-prepare-corp-domain.md)
> [3단계 »](step-3-prepare-pam-server.md)
