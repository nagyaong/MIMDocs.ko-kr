---
title: "PAM 배포 1단계 - CORP 도메인 | Microsoft 문서"
description: "Privileged Identity Manager에서 관리할 CORP 도메인을 기존 또는 새 ID를 사용하여 준비"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1164e7efb70d911497b08248b68f8d929bc6d3fb
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# <a name="step-1---prepare-the-host-and-the-corp-domain"></a>1단계 - 호스트 및 CORP 도메인 준비

>[!div class="step-by-step"]
[2단계 »](step-2-prepare-priv-domain-controller.md)


이 단계에서는 배스천 환경을 호스트할 준비를 합니다. 필요한 경우, 배스천 환경에서 관리하는 ID로 새 도메인 및 포리스트(*CORP* 포리스트)에 도메인 컨트롤러 및 구성원 워크스테이션도 만듭니다. 이 CORP 포리스트는 관리될 리소스가 있는 기존 포리스트를 시뮬레이트합니다. 이 문서는 보호할 예제 리소스, 파일 공유를 포함합니다.

Windows Server 2012 R2 이상을 실행하는 도메인 컨트롤러가 있는 기존 AD(Active Directory) 도메인이 이미 있으면 도메인 관리자인 경우 해당 도메인을 대신 사용할 수 있습니다.  

## <a name="prepare-the-corp-domain-controller"></a>CORP 도메인 컨트롤러 준비

이 섹션에서는 CORP 도메인에 대한 도메인 컨트롤러를 설정하는 방법을 설명합니다. CORP 도메인의 관리자는 배스천 환경에서 관리됩니다. 이 예제에 사용된 CORP 도메인의 DNS(Domain Name System) 이름은 *contoso.local*입니다.

### <a name="install-windows-server"></a>Windows Server 설치

가상 컴퓨터에 Windows Server 2012 R2 또는 Windows Server 2016 Technical Preview 4 이상을 설치하여 *CORPDC*라는 컴퓨터를 만듭니다.

1. **Windows Server 2012 R2 Standard(GUI 포함 서버) x64** 또는 **Windows Server 2016 Technical Preview(데스크톱 환경 포함 서버)**를 선택합니다.

2. 사용 조건을 검토하고 이에 동의합니다.

3. 디스크가 비어 있으므로 **사용자 지정: Windows만 설치**를 선택하고 초기화되지 않은 디스크 공간을 사용합니다.

4. 해당 새 컴퓨터에 해당 관리자로 로그인합니다. 제어판으로 이동합니다. 컴퓨터 이름을 *CORPDC*로 설정하고 가상 네트워크의 고정 IP 주소를 지정합니다. 서버를 다시 시작합니다.

5. 서버가 다시 시작되면 관리자로 로그인합니다. 제어판으로 이동합니다. 업데이트를 확인하고 필요한 업데이트를 설치하도록 컴퓨터를 구성합니다. 서버를 다시 시작합니다.

### <a name="add-roles-to-establish-a-domain-controller"></a>역할을 추가하여 도메인 컨트롤러 설정

이 섹션에서는 AD DS(Active Directory 도메인 서비스), DNS 서버 및 파일 서버(파일 및 저장소 서비스 섹션의 일부) 역할을 추가하고 이 서버의 수준을 새 포리스트 contoso.local의 도메인 컨트롤러로 올립니다.

> [!NOTE]  
> 이미 CORP 도메인으로 사용할 도메인이 있고 해당 도메인이 도메인 컨트롤러로 Windows Server 2012 R2 이상을 사용하는 경우 [데모용 추가 사용자 및 그룹 만들기](#create-additional-users-and-groups-for-demonstration-purposes)로 건너뛸 수 있습니다.

1. 관리자로 로그인한 상태에서 PowerShell을 시작합니다.

2. 다음 명령을 입력합니다.

  ```
  import-module ServerManager

  Add-WindowsFeature AD-Domain-Services,DNS,FS-FileServer –restart –IncludeAllSubFeature -IncludeManagementTools

  Install-ADDSForest –DomainMode Win2008R2 –ForestMode Win2008R2 –DomainName contoso.local –DomainNetbiosName contoso –Force -NoDnsOnNetwork
  ```

  그러면 사용할 안전 모드 관리자 암호를 입력하라는 메시지가 표시됩니다. DNS 위임 및 암호화 설정에 대한 경고 메시지가 표시되지만 이는 정상입니다.

3. 포리스트 만들기가 완료되면 로그아웃합니다. 서버가 자동으로 다시 시작됩니다.

4. 서버를 다시 시작한 후 도메인 관리자로 CORPDC에 로그인합니다. 도메인 관리자는 일반적으로 CONTOSO\\Administrator이며 암호는 CORPDC에 Windows를 설치할 때 만든 것입니다.

### <a name="create-a-group"></a>그룹 만들기

그룹이 아직 없는 경우 Active Directory에서 감사용 그룹을 만듭니다. 그룹의 이름은 NetBIOS 도메인 이름과 세 개의 달러 기호(예: *CONTOSO$$$*)로 구성되어야 합니다.

각 도메인에 대해 도메인 관리자로 도메인 컨트롤러에 로그인하고 다음 단계를 수행합니다.

1. PowerShell을 시작합니다.

2. 다음 명령을 입력하되 "CONTOSO"를 도메인의 NetBIOS 이름으로 바꿉니다.

  ```
  import-module activedirectory

  New-ADGroup –name 'CONTOSO$$$' –GroupCategory Security –GroupScope DomainLocal –SamAccountName 'CONTOSO$$$'
  ```

경우에 따라 이미 그룹이 있을 수 있습니다. 도메인이 AD 마이그레이션 시나리오에도 사용된 경우 이는 정상입니다.

### <a name="create-additional-users-and-groups-for-demonstration-purposes"></a>데모용 추가 사용자 및 그룹 만들기

새 CORP 도메인을 만든 경우 PAM 시나리오를 시연하려면 추가 사용자 및 그룹을 만들어야 합니다. 데모용 사용자 및 그룹은 도메인 관리자가 될 수 없거나 AD의 adminSDHolder 설정에 의해 제어됩니다.

> [!NOTE]
> 이미 CORP 도메인으로 사용할 도메인이 있고 데모용으로 사용할 수 있는 사용자 및 그룹이 있는 경우에는 [감사 구성](#configure-auditing) 섹션으로 건너뛸 수 있습니다.

*CorpAdmins*라는 보안 그룹 및 *Jen*이라는 사용자를 만들겠습니다. 원하면 다른 이름을 사용할 수 있습니다.

1. PowerShell을 시작합니다.

2. 다음 명령을 입력합니다. ‘Pass@word1’ 암호를 다른 암호 문자열로 바꿉니다.

  ```
  import-module activedirectory

  New-ADGroup –name CorpAdmins –GroupCategory Security –GroupScope Global –SamAccountName CorpAdmins

  New-ADUser –SamAccountName Jen –name Jen

  Add-ADGroupMember –identity CorpAdmins –Members Jen

  $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

  Set-ADAccountPassword –identity Jen –NewPassword $jp

  Set-ADUser –identity Jen –Enabled 1 -DisplayName "Jen"
  ```

### <a name="configure-auditing"></a>감사 구성

해당 포리스트에 PAM 구성을 설정하려면 기존 포리스트에서 감사를 사용하도록 설정해야 합니다.  

각 도메인에 대해 도메인 관리자로 도메인 컨트롤러에 로그인하고 다음 단계를 수행합니다.

1. **시작** > **관리 도구**(또는 Windows Server 2016, **Windows 관리 도구**)로 이동한 후 **그룹 정책 관리**를 시작합니다.

2. 이 도메인에 대한 도메인 컨트롤러 정책으로 이동합니다.  contoso.local에 대한 새 도메인을 만든 경우 **포리스트: contoso.local** > **도메인** > **contoso.local** > **도메인 컨트롤러** > **기본 도메인 컨트롤러 정책**으로 이동합니다. 정보 메시지가 표시됩니다.

3. **기본 도메인 컨트롤러 정책**을 마우스 오른쪽 단추로 클릭하고 **편집**을 선택합니다. 새 창이 나타납니다.

4. 그룹 정책 관리 편집기 창의 기본 도메인 컨트롤러 정책 트리에서 **컴퓨터 구성** > **정책** > **Windows 설정** > **보안 설정** > **로컬 정책** > **감사 정책**으로 이동합니다.

5. 세부 정보 창에서 **계정 관리 감사**를 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다. **이 정책 설정 정의**를 클릭하고 **성공** 확인란 및 **실패** 확인란을 선택한 다음 **적용** 및 **확인**을 클릭합니다.

6. 세부 정보 창에서 **디렉터리 서비스 액세스 감사**를 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다. **이 정책 설정 정의**를 클릭하고 **성공** 확인란 및 **실패** 확인란을 선택한 다음 **적용** 및 **확인**을 클릭합니다.

7. 그룹 정책 관리 편집기 창 및 그룹 정책 관리 창을 닫습니다.

8. PowerShell 창을 시작하고 다음을 입력하여 감사 설정을 적용합니다.

  ```
  gpupdate /force /target:computer
  ```

잠시 후 **컴퓨터 정책 업데이트가 완료되었습니다**라는 메시지가 표시됩니다.

### <a name="configure-registry-settings"></a>레지스트리 설정 구성

이 섹션에서는 Privileged Access Management 그룹 만들기에 사용되는 sID 기록 마이그레이션에 필요한 레지스트리 설정을 구성합니다.

1. PowerShell을 시작합니다.

2. 다음 명령을 입력하여 SAM(보안 계정 관리자) 데이터베이스에 대한 RPC(원격 프로시저 호출) 액세스를 허용하도록 원본 도메인을 구성합니다.

  ```
  New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1

  Restart-Computer
  ```

CORPDC 도메인 컨트롤러가 다시 시작됩니다. 이 레지스트리 설정에 대한 자세한 내용은 [ADMTv2를 사용하여 포리스트 간 sIDHistory 마이그레이션 문제를 해결하는 방법](http://support.microsoft.com/kb/322970)을 참조하세요.

## <a name="prepare-a-corp-workstation-and-resource"></a>CORP 워크스테이션 및 리소스 준비

도메인에 가입한 워크스테이션 컴퓨터가 아직 없는 경우 다음 지침에 따라 준비합니다.  

> [!NOTE]
> 이미 도메인에 가입한 워크스테이션이 있으면 [데모용 리소스 만들기](#create-a-resource-for-demonstration-purposes)로 건너뜁니다.

### <a name="install-windows-81-or-windows-10-enterprise-as-a-vm"></a>VM으로 Windows 8.1 또는 Windows 10 Enterprise 설치

소프트웨어가 설치되지 않은 또 하나의 새 가상 컴퓨터에 Windows 8.1 Enterprise 또는 Windows 10 Enterprise를 설치하여 *CORPWKSTN* 컴퓨터를 만듭니다.

1. 설치하는 동안 기본 설정을 사용합니다.

2. 설치할 때 인터넷에 연결하지 못할 수도 있습니다. **로컬 계정 만들기**를 선택합니다. 다른 사용자 이름을 지정합니다. "Administrator"나 "Jen"은 사용하지 마세요.

3. 제어판에서 이 컴퓨터에 가상 네트워크의 고정 IP 주소를 지정하고 인터페이스의 기본 DNS 서버가 CORPDC 서버의 DNS 서버가 되도록 설정합니다.

4. 제어판에서 CORPWKSTN 컴퓨터를 contoso.local 도메인에 도메인 가입합니다. Contoso 도메인 관리자 자격 증명을 제공해야 합니다. 그런 다음 이 작업이 완료되면 CORPWKSTN 컴퓨터를 다시 시작합니다.

### <a name="create-a-resource-for-demonstration-purposes"></a>데모용 리소스 만들기

PAM을 사용하여 보안 그룹 기반 액세스 제어를 시연하려면 리소스가 필요합니다.  리소스가 아직 없는 경우 데모용 파일 폴더를 사용할 수 있습니다.  그러면 contoso.local 도메인에 만든 "CorpAdmins" AD 개체와 "Jen"을 사용합니다.

1. CORPWKSTN 워크스테이션에 연결합니다. **사용자 전환** 아이콘을 클릭한 다음 **다른 사용자**를 클릭합니다. 사용자 CONTOSO\\Jen이 CORPWKSTN에 로그인할 수 있는지 확인합니다.

2. *CorpFS*라는 새 폴더를 만들어 *CorpAdmins* 그룹과 공유합니다.

3. 관리자 권한으로 PowerShell을 엽니다.

4. 다음 명령을 입력합니다.

  ```
  mkdir c:\corpfs

  New-SMBShare –Name corpfs –Path c:\corpfs –ChangeAccess CorpAdmins

  $acl = Get-Acl c:\corpfs

  $car = New-Object System.Security.AccessControl.FileSystemAccessRule( "CONTOSO\CorpAdmins", "FullControl", "Allow")

  $acl.SetAccessRule($car)

  Set-Acl c:\corpfs $acl
  ```

다음 단계에서는 PRIV 도메인 컨트롤러를 준비합니다.

>[!div class="step-by-step"]
[2단계 »](step-2-prepare-priv-domain-controller.md)
