---
title: Microsoft Identity Manager 인증서 관리자 배포 | Microsoft Docs
description: Microsoft Identity Manager 2016 인증서 관리자 설치
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 7ab76d386d8633de8919167c6b8f26b5137323e5
ms.sourcegitcommit: 9e420840815adb133ac014a8694de9af4d307815
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "52825844"
---
# <a name="deploying-microsoft-identity-manager-certificate-manager-2016-mim-cm"></a>MIM CM(Microsoft Identity Manager 인증서 관리자) 2016 배포

MIM CM(Microsoft Identity Manager 인증서 관리자) 2016의 설치에는 여러 단계가 포함됩니다. 프로세스를 간소화하는 방법으로 이에 대해 자세히 알아보겠습니다. 실제 MIM CM 단계 전에 수행해야 하는 예비 단계가 있습니다. 예비 작업을 수행하지 않을 경우 설치가 실패할 수 있습니다.

아래 다이어그램은 사용할 수 있는 환경 유형에 대한 예를 보여 줍니다. 번호가 있는 시스템이 다이어그램 아래 목록에 포함되며 이 문서에서 설명하는 단계를 성공적으로 완료하는 데 필요합니다. 마지막으로, Windows 2016 Datacenter Server가 사용됩니다.

![환경 다이어그램](media/mim-cm-deploy/image001.png)

1. CORPDC – 도메인 컨트롤러
2. CORPCM – MIM CM 서버
3. CORPCA – 인증 기관
4. CORPCMR – MIM CM Rest API Web – CM Portal For Rest API – 나중에 사용됨
5. CORPSQL1 – SQL 2016 SP1
6. CORPWK1 – Windows 10 도메인 가입

## <a name="deployment-overview"></a>배포 개요

- 기본 운영 체제 설치

    랩은 windows 2016 Datacenter server로 구성됩니다.

    >[!NOTE]
    >MIM 2016에 대해 지원되는 플랫폼에 대한 자세한 내용은 [MIM 2016에 대해 지원되는 플랫폼](/microsoft-identity-manager/microsoft-identity-manager-2016-supported-platforms.md)이라는 제목의 문서를 확인하십시오.

1. 배포 전 단계

    - [스키마 확장](https://msdn.microsoft.com/library/ms676929(v=vs.85).aspx)

    - 서비스 계정 만들기

    - [인증서 템플릿 만들기](https://technet.microsoft.com/library/cc753370(v=ws.11).aspx)

    - IIS

    - Kerberos 구성

    - 데이터베이스 관련 단계

        - SQL 구성 요구 사항

        - 데이터베이스 사용 권한

2. 배포

## <a name="pre-deployment-steps"></a>배포 전 단계

MIM CM 구성 마법사에는 과정을 성공적으로 완료하기 위한 방법과 함께 정보가 제공되어야 합니다.

![다이어그램](media/mim-cm-deploy/image003.png)

### <a name="extending-the-schema"></a>스키마 확장

스키마를 확장하는 과정은 간단하지만 되돌릴 수 없는 특성으로 인해 주의하여 접근해야 합니다.

>[!NOTE]
>이 단계를 수행하려면 사용된 계정에 스키마 관리자 권한이 필요합니다.

1. MIM 미디어의 위치를 찾아 \\인증서 관리\\x64 폴더로 이동합니다.

2. 스키마 폴더를 CORPDC에 복사한 후 해당 위치로 이동합니다.

    ![다이어그램](media/mim-cm-deploy/image005.png)

3. resourceForestModifySchema.vbs 스크립트 단일 포리스트 시나리오를 실행합니다. 리소스 포리스트 시나리오에 대해 다음 스크립트를 실행합니다.
   - DomainA – 사용자가 있는 위치(userForestModifySchema.vbs)
   - ResourceForestB – CM 설치 위치(resourceForestModifySchema.vbs).

     >[!NOTE]
     >스키마 변경은 단방향 작업이며 포리스트 복구를 롤백해야 하므로 필요한 백업이 있는지 확인해야 합니다. 이 작업을 수행하여 스키마를 변경하는 방법에 대한 자세한 내용은 [Forefront Identity Manager 2010 인증서 관리 스키마 변경](https://technet.microsoft.com/library/jj159298(v=ws.10).aspx) 문서를 검토하세요.

     ![다이어그램](media/mim-cm-deploy/image007.png)

4. 스크립트를 실행하고 스크립트가 완료되면 성공 메시지를 받아야 합니다.

    ![성공 메시지](media/mim-cm-deploy/image009.png)

이제 AD의 스키마가 MIM CM을 지원하도록 확장됩니다.

### <a name="creating-service-accounts-and-groups"></a>서비스 계정 및 그룹 만들기

다음 표에는 MIM CM에 필요한 계정 및 사용 권한이 요약되어 표시됩니다. MIM CM에서 다음 계정을 자동으로 만들도록 허용하거나 설치 전에 만들 수 있습니다. 실제 계정 이름은 변경할 수 있습니다. 직접 계정을 만드는 경우 사용자 계정 이름을 해당 기능과 쉽게 일치시킬 수 있는 방법으로 사용자 계정의 이름을 지정하는 것이 좋습니다.

Users:

![다이어그램](media/mim-cm-deploy/image010.png)

![다이어그램](media/mim-cm-deploy/image012.png)

| **역할**                   | **사용자 로그온 이름** |
|----------------------------|---------------------|
| MIM CM 에이전트               | MIMCMAgent          |
| MIM CM 키 복구 에이전트  | MIMCMKRAgent        |
| MIM CM 권한 부여 에이전트 | MIMCMAuthAgent      |
| MIM CM CA Manager 에이전트    | MIMCMManagerAgent   |
| MIM CM 웹 풀 에이전트      | MIMCMWebAgent       |
| MIM CM 등록 에이전트    | MIMCMEnrollAgent    |
| MIM CM 업데이트 서비스      | MIMCMService        |
| MIM 설치 계정        | MIMINSTALL          |
| 지원 센터            | CMHelpdesk1-2       |
| CM Manager                 | CMManager1-2        |
| 구독자 사용자            | CMUser1-2           |

Groups:

| **역할**               | **그룹**         |
|------------------------|-------------------|
| CM 지원 센터 멤버    | MIMCM-Helpdesk    |
| CM Manager 멤버     | MIMCM-Managers    |
| CM 구독자 멤버 | MIMCM-Subscribers |

Powershell: 에이전트 계정.

```powershell
import-module activedirectory
## Agent accounts used during setup
$cmagents = @{
"MIMCMKRAgent" = "MIM CM Key Recovery Agent"; 
"MIMCMAuthAgent" = "MIM CM Authorization Agent"
"MIMCMManagerAgent" = "MIM CM CA Manager Agent";
"MIMCMWebAgent" = "MIM CM Web Pool Agent";
"MIMCMEnrollAgent" = "MIM CM Enrollment Agent";
"MIMCMService" = "MIM CM Update Service";
"MIMCMAgent" = "MIM CM Agent";
}
##Groups Used for CM Management
$cmgroups = @{
"MIMCM-Managers" = "MIMCM-Managers"
"MIMCM-Helpdesk" = "MIMCM-Helpdesk"
"MIMCM-Subscribers" = "MIMCM-Subscribers" 
}
##Users Used during testlab
$cmusers = @{
"CMManager1" = "CM Manager1"
"CMManager2" = "CM Manager2"
"CMUser1" = "CM User1"
"CMUser2" = "CM User2"
"CMHelpdesk1" = "CM Helpdesk1"
"CMHelpdesk2" = "CM Helpdesk2"
}

## OU Paths
$aoupath = "OU=Service Accounts,DC=contoso,DC=com" ## Location of Agent accounts
$oupath = "OU=CMLAB,DC=contoso,DC=com" ## Location of Users and Groups for CM Lab


#Create Agents – Update UserprincipalName
$cmagents.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -UserPrincipalName ($_.Name + "@contoso.com")  -Path $aoupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}


#Create Users
$cmusers.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -Path $oupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}
```

### <a name="update-corpcm-server-local-policy-for-agent-accounts"></a>에이전트 계정에 대해 **CORPCM** 서버 로컬 정책 업데이트 

| **사용자 로그온 이름** | **설명 및 사용 권한**   |
|------|---------------------|
| MIMCMAgent          | 다음과 같은 서비스를 제공합니다. </br>- CA에서 암호화된 개인 키를 검색합니다. </br>- FIM CM 데이터베이스에 있는 스마트 카드 PIN 정보를 보호합니다. </br>- FIM CM과 CA 간 통신을 보호합니다. </br></br> 이 사용자 계정에는 다음 액세스 제어 설정이 필요합니다.</br>-   **로컬 로그온 허용** 사용자 권한.</br>-   **인증서 발급 및 관리** 사용자 권한. </br>- 다음 위치의 시스템 임시 폴더에서 읽기 및 쓰기 권한: %WINDIR%\\Temp.</br>- 디지털 서명 및 암호화 인증서가 사용자 저장소에 발급되어 설치되어 있음
|MIMCMKRAgent        | CA에서 보관된 개인 키를 복구합니다. 이 사용자 계정에는 다음 액세스 제어 설정이 필요합니다.</br> -   **로컬 로그온 허용** 사용자 권한.</br>- 로컬 **관리자** 그룹의 구성원 </br>- **KeyRecoveryAgent** 인증서 템플릿에 사용 권한 등록 </br>- 키 복구 에이전트 인증서가 사용자 저장소에 발급되어 설치되어 있음. 인증서는 CA에서 키 복구 에이전트 목록에 추가되어야 합니다. </br>- 다음 위치의 시스템 임시 폴더에서 읽기 권한 및 쓰기 권한: ```%WINDIR%\\Temp.```                                                                                                                     |
| MIMCMAuthAgent      | 사용자 권한과 사용자 및 그룹에 대한 권한을 결정합니다. 이 사용자 계정에는 다음 액세스 제어 설정이 필요합니다. </br>- Windows 2000 이전 Compatible Access 도메인 그룹의 구성원 </br> - **보안 감사 생성** 사용자 권한 부여             |
| MIMCMManagerAgent   | CA 관리 작업을 수행합니다. </br> 이 사용자에게는 CA 관리 권한을 할당해야 합니다.        |
| MIMCMWebAgent       | IIS 응용 프로그램 풀에 대한 ID를 제공합니다. FIM CM은 이 사용자 자격 증명을 사용하는 Microsoft Win32® 응용 프로그래밍 인터페이스 프로세스 내에서 실행됩니다. </br> 이 사용자 계정에는 다음 액세스 제어 설정이 필요합니다.</br> - 로컬 **IIS_WPG, windows 2016 = IIS_IUSRS** 그룹의 구성원 </br>- 로컬 **관리자** 그룹의 구성원</br>- **보안 감사 생성** 사용자 권한 부여 </br>- **운영 체제의 일부로 작동** 사용자 권한 부여 </br>- **프로세스 수준 토큰 바꾸기** 사용자 권한 부여</br>- IIS 응용 프로그램 풀 **CLMAppPool**의 ID로 할당 </br>- **HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\CLM\\v1.0\\Server\\WebUser** 레지스트리 키에서 읽기 권한 부여 </br>- 이 계정 역시 위임을 위해 신뢰할 수 있어야 합니다.|
| MIMCMEnrollAgent    | 사용자를 대신하여 등록을 수행합니다. 이 사용자 계정에는 다음 액세스 제어 설정이 필요합니다.</br>- 등록 에이전트 인증서가 사용자 저장소에 발급되어 설치되어 있음.</br>-   **로컬 로그온 허용** 사용자 권한. </br>- **등록 에이전트** 인증서 템플릿에서 등록 권한(또는 사용된 경우 사용자 지정 템플릿)                 |

### <a name="creating-certificate-templates-for-mim-cm-service-accounts"></a>MIM CM 서비스 계정에 대한 인증서 템플릿 만들기

MIM CM에 사용되는 세 가지 서비스 계정에는 인증서가 필요하며 구성 마법사에는 인증서를 요청하는 데 사용해야 하는 인증서 템플릿의 이름을 제공해야 합니다.

인증서가 필요한 서비스 계정은 다음과 같습니다.

- MIMCMAgent: 이 계정에는 사용자 인증서가 필요합니다.

- MIMCMEnrollAgent: 이 계정에는 등록 에이전트 인증서가 필요합니다.

- MIMCMKRAgent: 이 계정에는 **키 복구 에이전트** 인증서가 필요합니다.

AD에 템플릿이 이미 있지만 MIM CM으로 작업하기 위해서는 사용자 고유의 버전을 만들어야 합니다. 원본 기본 템플릿에서 수정이 필요하기 때문입니다.

위의 세 가지 계정 모두 조직 내에서 상승된 권한을 포함하며 주의깊게 처리해야 합니다.

#### <a name="create-the-mim-cm-signing-certificate-template"></a>MIM CM 서명 인증서 템플릿 만들기

1. **관리 도구**에서 **인증 기관**을 엽니다.

2. **인증 기관** 콘솔의 탐색 콘솔 트리에서 **Contoso-CorpCA**를 확장한 후 **인증서 템플릿**을 클릭합니다.

3. **인증서 템플릿**을 마우스 오른쪽 단추로 클릭하고 **관리**를 클릭합니다.

4. **인증서 템플릿 콘솔**의 **세부 정보** 창에서 **사용자**를 선택하고 마우스 오른쪽 단추로 클릭한 후 **템플릿 복제**를 클릭합니다.

5. **템플릿 복제** 대화 상자에서 **Windows Server 2003 Enterprise**를 선택한 후 **확인**을 클릭합니다.

    ![결과 변경 내용 표시](media/mim-cm-deploy/image014.png)

    >[!NOTE]
    >MIM CM은 버전 3 인증서 템플릿을 기반으로 하는 인증서와 함께 작동하지 않습니다. Windows Server® 2003 Enterprise(버전 2) 인증서 템플릿을 만들어야 합니다. 자세한 내용은 [V3 세부 정보](https://blogs.msdn.microsoft.com/ms-identity-support/2016/07/14/faq-for-fim-2010-to-support-sha2-kspcng-and-v3-certificate-templates-for-issuing-user-and-agent-certificates-and-mim-2016-upgrade)를 참조하세요.

6. **새 템플릿의 속성** 대화 상자의 **일반** 탭에서 **템플릿 표시 이름** 상자에 **MIM CM 서명**을 입력합니다. **유효 기간**을 **2년**으로 변경한 후 **Active Directory에 인증서 게시** 확인란을 선택 취소합니다.

7. **요청 처리** 탭에서 **개인 키를 내보낼 수 있음** 확인란이 선택되어 있는지 확인한 후 **암호화 탭**을 클릭합니다.

8. **암호화 선택** 대화 상자에서 **Microsoft Enhanced Cryptographic Provider v1.0**은 사용하지 않도록, **Microsoft Enhanced RSA and AES Cryptographic Provider**는 사용하도록 설정한 후 **확인**을 클릭합니다.

9. **주체 이름** 탭에서 **주체 이름에 전자 메일 이름 포함** 및 **전자 메일 이름** 확인란을 선택 해제합니다.

10. **확장** 탭의 **이 템플릿에 포함된 확장** 목록에서 **응용 프로그램 정책**이 선택되어 있는지 확인한 후 **편집**을 클릭합니다.

11. **응용 프로그램 정책 확장 편집** 대화 상자에서 **파일 시스템 암호화** 및 **전자 메일 보안** 응용 프로그램 정책을 선택합니다. **제거**, **확인**을 차례로 클릭합니다.

12. **보안** 탭에서 다음 단계를 수행합니다.

    - **관리자**를 제거합니다.

    - **Domain Admins**를 제거합니다.

    - **도메인 사용자**를 제거합니다.

    - **Enterprise Admins**에는 **읽기** 및 **쓰기** 권한만 할당합니다.

    - **MIMCMAgent**를 추가합니다.

    - **MIMCMAgent**에 **읽기** 및 **등록** 권한을 할당합니다.

13. **새 템플릿의 속성** 대화 상자에서 **확인**을 클릭합니다.

14. **인증서 템플릿 콘솔**이 열립니다.

#### <a name="create-the-mim-cm-enrollment-agent-certificate-template"></a>MIM CM 등록 에이전트 인증서 템플릿 만들기

1. **인증서 템플릿 콘솔**의 **세부 정보** 창에서 **등록 에이전트**를 선택하고 마우스 오른쪽 단추로 클릭한 후 **템플릿 복제**를 클릭합니다.

2. **템플릿 복제** 대화 상자에서 **Windows Server 2003 Enterprise**를 선택한 후 **확인**을 클릭합니다.

3. **새 템플릿의 속성** 대화 상자의 **일반** 탭에서 **템플릿 표시 이름** 상자에 **MIM CM 등록 에이전트**를 입력합니다. **유효 기간**이 **2년**인지 확인합니다.

4. **요청 처리** 탭에서 **개인 키를 내보낼 수 있음**을 사용하도록 설정한 후 **CSP 또는 암호화 탭**을 클릭합니다.

5. **CSP 선택** 대화 상자에서 **Microsoft Base Cryptographic Provider v1.0** 및 **Microsoft Enhanced Cryptographic Provider v1.0**은 사용하지 않도록, **Microsoft Enhanced RSA and AES Cryptographic Provider**는 사용하도록 설정한 후 **확인**을 클릭합니다.

6. **보안** 탭에서 다음을 수행합니다.

    - **관리자**를 제거합니다.

    - **Domain Admins**를 제거합니다.

    - **Enterprise Admins**에는 **읽기** 및 **쓰기** 권한만 할당합니다.

    - **MIMCMEnrollAgent**를 추가합니다.

    - **MIMCMEnrollAgent**에 **읽기** 및 **등록** 권한을 할당합니다.

7. **새 템플릿의 속성** 대화 상자에서 **확인**을 클릭합니다.

8. **인증서 템플릿 콘솔**이 열립니다.

#### <a name="create-the-mim-cm-key-recovery-agent-certificate-template"></a>MIM CM 키 복구 에이전트 인증서 템플릿 만들기

1. **인증서 템플릿 콘솔**의 **세부 정보** 창에서 **키 복구 에이전트**를 선택하고 마우스 오른쪽 단추로 클릭한 후 **템플릿 복제**를 클릭합니다.

2. **템플릿 복제** 대화 상자에서 **Windows Server 2003 Enterprise**를 선택한 후 **확인**을 클릭합니다.

3. **새 템플릿의 속성** 대화 상자의 **일반** 탭에서 **템플릿 표시 이름** 상자에 **MIM CM 키 복구 에이전트**를 입력합니다. **암호화 탭**에서 **유효 기간**이 **2년**인지 확인합니다.

4. **공급자 선택** 대화 상자에서 **Microsoft Enhanced Cryptographic Provider v1.0**은 사용하지 않도록, **Microsoft Enhanced RSA and AES Cryptographic Provider**는 사용하도록 설정한 후 **확인**을 클릭합니다.

5. **발급 요구 사항** 탭에서 **CA 인증서 관리자 승인**이 **사용 안 함**인지 확인합니다.

6. **보안** 탭에서 다음을 수행합니다.

    - **관리자**를 제거합니다.

    - **Domain Admins**를 제거합니다.

    - **Enterprise Admins**에는 **읽기** 및 **쓰기** 권한만 할당합니다.

    - **MIMCMKRAgent**를 추가합니다.

    - **KRAgent**에 **읽기** 및 **등록** 권한을 할당합니다.

7. **새 템플릿의 속성** 대화 상자에서 **확인**을 클릭합니다.

8. **인증서 템플릿 콘솔**을 닫습니다.

#### <a name="publish-the-required-certificate-templates-at-the-certification-authority"></a>인증 기관에서 필요한 인증서 템플릿 게시

1. **인증 기관** 콘솔을 복원합니다.

2. **인증 기관** 콘솔에서 **인증서 템플릿**을 마우스 오른쪽 단추로 클릭하고 **새로 만들기**를 가리킨 다음 **발급할 인증서 템플릿**을 클릭합니다.

3. **인증서 템플릿 사용** 대화 상자에서 **MIM CM 등록 에이전트**, **MIM CM 키 복구 에이전트** 및 **MIM CM 서명**을 선택합니다. **확인**을 클릭합니다.

4. 콘솔 트리에서 **인증서 템플릿**을 클릭합니다.

5. 세 개의 새로운 템플릿이 **세부 정보** 창에 나타나는지 확인한 후 **인증 기관**을 닫습니다.

    ![MIM CM 서명](media/mim-cm-deploy/image016.png)

6. 열려 있는 창을 모두 닫고 로그오프합니다.

### <a name="iis-configuration"></a>IIS 구성

CM에 대한 웹 사이트를 호스팅하기 위해 IIS를 설치하고 구성합니다.

#### <a name="install-and-configure-iis"></a>IIS 설치 및 구성

1. **CORLog in**에 **MIMINSTALL** 계정으로 로그인

    >[!IMPORTANT]
    >MIM 설치 계정은 로컬 관리자여야 합니다.

2. PowerShell을 열고 다음 명령을 실행

    `Install-WindowsFeature –ConfigurationFilePath`

>[!NOTE]
>이름이 기본 웹 사이트인 사이트는 기본적으로 IIS 7을 사용하여 설치됩니다. 해당 사이트 이름이 변경되었거나 해당 이름의 사이트를 제거한 경우 MIM CM을 설치할 수 있으려면 기본 웹 사이트를 사용할 수 있어야 합니다.

#### <a name="configuring-kerberos"></a>Kerberos 구성

MIMCMWebAgent 계정으로 MIM CM 포털을 실행 중입니다. 기본적으로 IIS에는 커널 모드 인증이 사용됩니다. Kerberos 커널 모드 인증을 사용하지 않도록 설정하고 대신 MIMCMWebAgent 계정에서 SPN을 구성합니다. 일부 명령에는 active directory 및 CORPCM 서버에서 상승된 권한이 필요합니다.

![다이어그램](media/mim-cm-deploy/image020.png)

```powershell
#Kerberos settings
#SPN
SETSPN -S http/cm.contoso.com contoso\MIMCMWebAgent
#Delegation for certificate authority
Get-ADUser CONTOSO\MIMCMWebAgent | Set-ADObject -Add @{"msDS-AllowedToDelegateTo"="rpcss/CORPCA","rpcss/CORPCA.contoso.com"}
```

**CORPCM에서 IIS 업데이트**

![다이어그램](media/mim-cm-deploy/image022.png)

```powershell
add-pssnapin WebAdministration

Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name enabled -Value $true
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useKernelMode -Value $false
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useAppPoolCredentials -Value $true
```

>[!NOTE]
>“cm.contoso.com”에 대한 DNS A 레코드를 추가하고 CORPCM IP를 가리키도록 해야 합니다.

#### <a name="requiring-ssl-on-the-mim-cm-portal"></a>MIM CM 포털에서 SSL 요구

MIM CM 포털에서 SSL을 요구하는 것이 좋습니다. 그렇지 않으면 마법사에서 경고 메시지가 표시됩니다.

1. **cm.contoso.com**에 대해 웹 인증서를 등록하고 기본 사이트에 할당합니다.

2. **IIS 관리자**를 열고 **인증서 관리**로 이동합니다.

3. [기능 보기]에서 [SSL 설정]을 두 번 클릭합니다.

4. [SSL 설정] 페이지에서 **SSL 필요**를 선택합니다.

5. [작업] 창에서 **적용**을 클릭합니다.

### <a name="database-configuration-corpsql-for-mim-cm"></a>MIM CM에 대한 데이터베이스 구성 **CORPSQL**

1. CORPSQL01 서버에 연결되어 있는지 확인합니다.

2. SQL DBA로 로그인했는지 확인합니다.

3. 다음 T-SQL 스크립트를 실행하여 구성 단계로 이동할 때 CONTOSO\\MIMINSTALL 계정이 데이터베이스를 만들도록 허용합니다.

    >[!NOTE]
    >종료 및 정책 모듈이 준비되면 SQL로 다시 돌아와야 합니다.

    ```sql
    create login [CONTOSO\\MIMINSTALL] from windows;
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'dbcreator';
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'securityadmin';  
    ```

![MIM CM 구성 마법사 오류 메시지](media/mim-cm-deploy/image024.png)

## <a name="deployment-of-microsoft-identity-manager-2016-certificate-management"></a>Microsoft Identity Manager 2016 인증서 관리자 배포

1. CORPCM 서버에 연결되어 있고 **MIMINSTALL** 계정이 **로컬 관리자** 그룹의 멤버인지 확인합니다.

2. Contoso\\MIMINSTALL로 로그인했는지 확인합니다.

3. Microsoft Identity Manager SP1 ISO를 탑재합니다.

4. **인증서 관리\\x64** 디렉터리를 **엽니다**.

5. **x64** 창에서 **설치**를 마우스 오른쪽 단추로 클릭하고 **관리자 권한으로 실행**을 클릭합니다.

6. [Microsoft Identity Manager 인증서 관리 설치 마법사 시작] 페이지에서 **다음**을 클릭합니다.

7. [최종 사용자 사용권 계약] 페이지에서 사용 조건을 읽고, 동의함 **확인란**을 설정한 후 [다음]을 클릭합니다.

8. [사용자 지정 설치] 페이지에서 **MIM CM 포털** 및 **MIM CM 업데이트 서비스 구성 요소**가 설치되도록 설정되었는지 확인한 후 **다음**을 클릭합니다.

9. 가상 웹 폴더 페이지에서 가상 폴더 이름이 **CertificateManagement**인지 확인한 후 **다음을 클릭합니다**.

10. [Microsoft Identity Manager 인증서 관리 설치] 페이지에서 **설치**를 클릭합니다.

11. **완료된** [Microsoft Identity Manager 인증서 관리 설치 마법사] 페이지에서 **마침**을 클릭합니다.

![MIM CM 마법사 완료됨](media/mim-cm-deploy/image026.png)

### <a name="configuration-wizard-of-microsoft-identity-manager-2016-certificate-management"></a>Microsoft Identity Manager 2016 인증서 관리의 구성 마법사

CORPCM에 로그인하기 전에 MIMINSTALL을 구성 마법사에 대한 **domain Admins, Schema Admins 및 로컬 관리자** 그룹에 추가합니다. 구성이 완료되면 나중에 제거할 수 있습니다.

![오류 메시지](media/mim-cm-deploy/image028.png)

1. **시작** 메뉴에서 **인증서 관리 구성 마법사**를 클릭합니다. **관리자 권한**으로 실행합니다.

2. **구성 마법사 시작** 페이지에서 **다음**을 클릭합니다.

3. **CA 구성** 페이지에서 선택한 CA가 **Contoso-CORPCA-CA**인지 확인하고 선택한 서버가 **CORPCA.CONTOSO.COM**인지 확인한 후 **다음**을 클릭합니다.

4. **Microsoft® SQL Server® Database 설치** 페이지의 **SQL Server의 이름** 상자에 **CORPSQL1**을 입력하고 **Use my credentials to create the database(내 자격 증명을 사용하여 데이터베이스 만들기)** 확인란을 사용한 후 **다음**을 클릭합니다.

5. **데이터베이스 설정** 페이지에서 **FIMCertificateManagement**의 기본 데이터베이스 이름을 적용하고 **SQL 통합 인증**이 선택되어 있는지 확인한 후 **다음**을 클릭합니다.

6. **Active Directory 설정** 페이지에서 서비스 연결 지점에 대해 제공된 기본 이름을 적용한 후 **다음**을 클릭합니다.

7. **인증 방법** 페이지에서 **windows 통합 인증**이 선택되었는지 확인하고 **다음**을 클릭합니다.

8. **에이전트 – FIM CM** 페이지에서 **FIM CM 기본 설정 사용** 확인란을 선택 해제한 후 **사용자 지정 계정**을 클릭합니다.

9. **에이전트 – FIM CM** 다중 탭 대화 상자의 각 탭에서 다음 정보를 입력합니다.

   - 사용자 이름: **Update**

   - 암호: **Pass\@word1**

   - 암호 확인: **Pass\@word1**

   - 기존 사용자 사용: **사용**

     >[!NOTE]
     >이러한 계정을 이전에 만들었습니다. 8단계의 프로시저가 6가지 모든 에이전트 계정 탭에 대해 반복되었는지 확인합니다.

     ![MIM CM 계정](media/mim-cm-deploy/image030.png)

10. 모든 에이전트 계정 정보가 완료되면 **확인**을 클릭합니다.

11. **에이전트 – MIM CM** 페이지에서 **다음**을 클릭합니다.

12. **서버 인증서 설정** 페이지에서 다음 인증서 템플릿을 사용하도록 설정합니다.

    - 복구 에이전트 키 복구 에이전트 인증서에 사용할 인증서 템플릿: **MIMCMKeyRecoveryAgent**.

    - FIM CM 에이전트 인증서에 사용할 인증서 템플릿: **MIMCMSigning**.

    - 등록 에이전트 인증서에 사용할 인증서 템플릿: **FIMCMEnrollmentAgent**.

13. **서버 인증서 설정** 페이지에서 **다음**을 클릭합니다.

14. **Setup E-mail Server, Document Printing(전자 메일 서버 설정, 문서 인쇄)** 페이지의 **Specify the name of the SMTP server you want to use to e-mail registration notifications(전자 메일 등록 알림에 사용할 SMTP 서버 이름 지정)** 상자에서 **다음**을 클릭합니다.

15. **구성 준비 완료** 페이지에서 **구성**을 클릭합니다.

16. **구성 마법사 – Microsoft Forefront Identity Manager 2010 R2** 경고 대화 상자에서 **확인**을 클릭하여 IIS 가상 디렉터리에 SSL이 사용되지 않은 것을 확인합니다.

    ![media/image17.png](media/mim-cm-deploy/image032.png)

    >[!NOTE] 
    >구성 마법사의 실행이 완료될 때까지 [마침] 단추를 클릭하지 마세요. 마법사 로깅은 다음 위치에서 볼 수 있습니다. **%programfiles%\\Microsoft Forefront Identity Management\\2010\\Certificate Management\\config.log**

17. **마침**을 클릭합니다.

    ![MIM CM 마법사 완료됨](media/mim-cm-deploy/image033.png)

18. 열려 있는 창을 모두 닫습니다.

19. `https://cm.contoso.com/certificatemanagement`를 브라우저의 로컬 인트라넷 영역에 추가합니다.

20. 서버 CORPCM `https://cm.contoso.com/certificatemanagement`에서 사이트 방문  

    ![다이어그램](media/mim-cm-deploy/image035.png)

### <a name="verify-the-cng-key-isolation-service"></a>CNG 키 격리 서비스 확인

1. **관리 도구**에서 **서비스**를 엽니다.

2. **세부 정보** 창에서 **CNG 키 격리**를 두 번 클릭합니다.

3. **일반** 탭에서 **시작 유형**을 **자동**으로 변경합니다.

4. **일반** 탭에서 시작됨 상태가 아닌 서비스를 시작합니다.

5. **일반** 탭에서 **확인**을 클릭합니다.

### <a name="installing-and-configuring-the-ca-modules-"></a>CA 모듈 설치 및 구성

이 단계에서는 인증 기관에 FIM CM CA 모듈을 설치 및 구성합니다.

1. 관리 작업에 대한 사용자 권한만 검사하도록 FIM CM 구성

2. **C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Certificate Management\\web** 창에서 **web.config**의 복사본을 **web.1.config** 이름으로 만듭니다.

3. **웹** 창에서 **Web.config**를 마우스 오른쪽 단추로 클릭하고 **열기**를 클릭합니다.

    >[!Note]
    >Web.config 파일이 메모장에서 열립니다.

4. 파일이 열리면 CTRL + F 키를 누릅니다.

5. **찾기 및 바꾸기** 대화 상자의 **찾을 내용** 상자에 **UseUser**를 입력한 후 **다음 찾기**를 세 번 클릭합니다.

6. **찾기 및 바꾸기** 대화 상자를 닫습니다.

7. **\<add key=”Clm.RequestSecurity.Flags” value=”UseUser,UseGroups” /\>** 줄에 위치해야 합니다. **\<add key=”Clm.RequestSecurity.Flags” value=”UseUser” /\>** 를 읽도록 줄을 변경합니다.

8. 파일을 닫고 모든 변경 내용을 저장합니다.

9. SQL Server에서 CA 컴퓨터에 대한 계정을 만듭니다.\<스크립트 없음\>

10. **CORPSQL01** 서버에 연결되어 있는지 확인합니다.

11. **DBA**로 로그인했는지 확인합니다.

12. **시작** 메뉴에서 **SQL Server Management Studio**를 시작합니다.

13. **서버에 연결** 대화 상자의 **서버 이름** 상자에서 **CORPSQL01**을 선택한 후 **연결**을 클릭합니다.

14. 콘솔 트리에서 **보안**을 확장한 후 **로그인**을 클릭합니다.

15. **로그인**을 마우스 오른쪽 단추로 클릭한 후 **새 로그인**을 클릭합니다.

16. **일반** 페이지의 **로그인 이름** 상자에 **contoso\\CORPCA\$** 를 입력합니다. **Windows 인증**을 선택합니다. 기본 데이터베이스는 **FIMCertificateManagement**입니다.

17. 왼쪽 창에서 **사용자 매핑**을 선택합니다. 오른쪽 창에서 **FIMCertificateManagement** 옆에 있는 **맵** 열의 확인란을 클릭합니다. **데이터베이스 역할 멤버 자격: FIMCertificateManagement** 목록에서 **clmApp** 역할을 사용하도록 설정합니다.

18. **확인**을 클릭합니다.

19. **Microsoft SQL Server Management Studio**를 닫습니다.

### <a name="install-the-fim-cm-ca-modules-on-the-certification-authority"></a>인증 기관에서 FIM CM CA 모듈 설치

1. **CORPCA** 서버에 연결되어 있는지 확인합니다.

2. **X64** 창에서 **Setup.exe**를 마우스 오른쪽 단추로 클릭하고 **관리자 권한으로 실행**을 클릭합니다.

3. **Microsoft Identity Manager 인증서 관리 설치 마법사 시작** 페이지에서 **다음**을 클릭합니다.

4. **최종 사용자 사용권 계약** 페이지에서 사용 조건을 읽습니다. **동의함** 확인란을 선택하고 **다음**을 클릭합니다.

5. **사용자 지정 설치** 페이지에서 **MIM CM 포털**을 선택한 후 **이 기능을 사용할 수 없게 됩니다.** 를 클릭합니다.

6. **사용자 지정 설치** 페이지에서 **MIM CM 업데이트 서비스**를 선택한 후 **이 기능을 사용할 수 없게 됩니다.** 를 클릭합니다.

    >[!Note]
    >이렇게 하면 MIM CM CA 파일만 설치용으로 사용할 수 있습니다.

7. **사용자 지정 설치** 페이지에서 **다음**을 클릭합니다.

8. **Microsoft Identity Manager 인증서 관리 설치** 페이지에서 **설치**를 클릭합니다.

9. **완료된 Microsoft Identity Manager 인증서 관리 설치 마법사 시작** 페이지에서 **마침**을 클릭합니다.

10. 열려 있는 창을 모두 닫습니다.

### <a name="configure-the-mim-cm-exit-module"></a>MIM CM 끝내기 모듈 구성

1. **관리 도구**에서 **인증 기관**을 엽니다.

2. 콘솔 트리에서 **contoso-CORPCA-CA**를 마우스 오른쪽 단추로 클릭한 다음 **속성**을 클릭합니다.

3. **끝내기 모듈** 탭에서 **FIM CM 끝내기 모듈**을 선택한 후 **속성**을 클릭합니다.

4. **CM 데이터베이스 연결 문자열 지정** 상자에 **Connect Timeout=15;Persist Security Info=True; Integrated Security=sspi;Initial Catalog=FIMCertificateManagement;Data Source=CORPSQL01**을 입력합니다. **연결 문자열 암호화** 확인란을 설정된 상태로 두고 **확인**을 클릭합니다.
5. **Microsoft FIM 인증서 관리** 메시지 상자에서 **확인**을 클릭합니다.

6. **contoso-CORPCA-CA 속성** 대화 상자에서 **확인**을 클릭합니다.

7. **contoso-CORPCA-CA** *,* 를 마우스 오른쪽 단추로 클릭하고 **모든 작업**을 가리킨 후 **서비스 중지**를 클릭합니다. Active Directory 인증서 서비스가 중지될 때까지 기다립니다.

8. **contoso-CORPCA-CA** *,* 를 마우스 오른쪽 단추로 클릭하고 **모든 작업**을 가리킨 후 **서비스 시작**을 클릭합니다.

9. **인증 기관** 콘솔을 최소화합니다.

10. **관리 도구**에서 **이벤트 뷰어**를 엽니다.

11. 콘솔 트리에서 **응용 프로그램 및 서비스 로그**를 확장한 후 **FIM 인증서 관리**를 클릭합니다.

12. 이벤트 목록에서 인증서 서비스를 마지막으로 다시 시작한 후 최신 이벤트에 **경고** 또는 **오류** 이벤트가 포함되지 *않은지* 확인합니다.

    >[!NOTE] 
    >마지막 이벤트는 `SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit`의 설정을 사용하여 끝내기 모듈이 로드되었음을 명시합니다.

13. **이벤트 뷰어**를 최소화합니다.

### <a name="copy-the-mimcmagent-certificates-thumbprint-to-windows-clipboard"></a>MIMCMAgent 인증서의 지문을 Windows® 클립보드에 복사

1. **인증 기관** 콘솔을 복원합니다.

2. 콘솔 트리에서 **contoso-CORPCA-CA**를 확장한 후 **발급된 인증서**를 클릭합니다.

3. **세부 정보** 창에서 **요청자 이름** 열에 **CONTOSO\\MIMCMAgent**가 있고 **인증서 템플릿** 열에 **FIM CM 서명**이 있는 인증서를 두 번 클릭합니다.

4. **세부 정보** 탭에서 **지문** 필드를 선택합니다.

5. 지문을 선택하고 CTRL + C 키를 누릅니다.

    >[!NOTE]
    >지문 문자 목록에는 선행 공백을 포함하지 **마세요**.

6. **인증서** 대화 상자에서 **확인**을 클릭합니다.

7. **시작** 메뉴의 **프로그램 및 파일 검색** 상자에 **메모장**을 입력한 후 ENTER 키를 누릅니다.

8. **메모장**의 **편집** 메뉴에서 **붙여넣기**를 클릭합니다.

9. **편집** 메뉴에서 **바꾸기**를 클릭합니다.

10. **찾을 내용** 상자에 공백 문자를 입력한 후 **모두 바꾸기**를 클릭합니다.

    >[!Note]
    >그러면 지문의 문자 사이에 모든 공백이 제거됩니다.

11. **바꾸기** 대화 상자에서 **취소**를 클릭합니다.

12. 변환된 *thumbprintstring*을 선택한 후 CTRL+C 키를 누릅니다.

13. 변경 내용을 저장하지 않고 **메모장**을 닫습니다.

### <a name="configure-the-fim-cm-policy-module"></a>FIM CM 정책 모듈 구성

1. **인증 기관** 콘솔을 복원합니다.

2. **contoso-CORPCA-CA**를 마우스 오른쪽 단추로 클릭한 다음 **속성**을 클릭합니다.

3. **contoso-CORPCA-CA 속성** 대화 상자의 **정책 모듈** 탭에서 **속성**을 클릭합니다.

    - **일반** 탭에서 **Pass non-FIM CM requests to the default policy module for processing(처리를 위해 FIM CM이 아닌 요청을 기본 정책 모듈로 전달)** 이 선택되어 있는지 확인합니다.

    - **서명 인증서** 탭에서 **추가**를 클릭합니다.

    - 인증서 대화 상자에서 **Please specify hex-encoded certificate hash(16진수로 인코딩된 인증서 해시를 지정하세요)** 상자를 마우스 오른쪽 단추로 클릭한 후 **붙여넣기**를 클릭합니다.

    - **인증서** 대화 상자에서 **확인**을 클릭합니다.

        >[!Note]
        >**확인** 단추가 활성화되지 않으면 clmAgent 인증서에서 지문을 복사할 때 지문 문자열에 실수로 숨겨진 문자를 포함한 것입니다. 이 연습에서는 **작업 4: MIMCMAgent 인증서의 지문을 Windows 클립보드에 복사**부터 시작하여 모든 단계를 반복합니다.

4. **구성 속성** 대화 상자에서 지문이 **유효한 서명 인증서** 목록에 나타나는지 확인한 후, **확인**을 클릭합니다.

5. **FIM 인증서 관리** 메시지 상자에서 **확인**을 클릭합니다.

6. **contoso-CORPCA-CA 속성** 대화 상자에서 **확인**을 클릭합니다.

7. **contoso-CORPCA-CA** *,* 를 마우스 오른쪽 단추로 클릭하고 **모든 작업**을 가리킨 후 **서비스 중지**를 클릭합니다.

8. Active Directory 인증서 서비스가 중지될 때까지 기다립니다.

9. **contoso-CORPCA-CA** *,* 를 마우스 오른쪽 단추로 클릭하고 **모든 작업**을 가리킨 후 **서비스 시작**을 클릭합니다.

10. **인증 기관** 콘솔을 닫습니다.

11. 열려 있는 창을 모두 닫고 로그오프합니다.

**배포의 마지막 단계**에서는 CONTOSO\\MIMCM-Manager가 템플릿을 배포 및 만들 수 있고 스키마 및 Domain Admins가 아니어도 시스템을 구성할 수 있는지 확인하려고 합니다. 다음 스크립트는 dsacls를 사용하여 인증서 템플릿에 대한 권한을 ACL로 지정합니다. 포리스트의 각 기존 인증서 템플릿에 대한 보안 읽기 및 쓰기 권한을 변경할 수 있는 전체 권한이 있는 계정으로 실행하세요.

첫 번째 단계: **서비스 연결 지점 및 대상 그룹 사용 권한 구성 및 프로필 템플릿 관리 위임**

1. SCP(서비스 연결 지점)에 대한 사용 권한을 구성합니다.

2. 위임된 프로필 템플릿 관리를 구성합니다.

3. SCP(서비스 연결 지점)에 대한 사용 권한을 구성합니다. **\<스크립트 없음\>**

4.   **CORPDC** 가상 서버에 연결되어 있는지 확인합니다.

5. **contoso\\corpadmin**으로 로그온합니다.

6. **관리 도구**에서 **Active Directory 사용자 및 컴퓨터**를 엽니다.

7. **Active Directory 사용자 및 컴퓨터**의 **보기** 메뉴에서 **고급 기능**이 사용되었는지 확인합니다.

8. 콘솔 트리에서 **Contoso.com** \| **System** \| **Microsoft** \| **Certificate Lifecycle Manager**를 확장한 후 **CORPCM**을 클릭합니다.

9. **CORPCM**을 마우스 오른쪽 단추로 클릭한 다음 **속성**을 클릭합니다.

10. **CORPCM 속성** 대화 상자의 **보안** 탭에서 해당 권한을 가진 다음 그룹을 추가합니다.

    | Group          | 사용 권한      |
    |----------------|------------------|
    | mimcm-Managers | 읽기 </br> FIM CM 감사</br> FIM CM 등록 에이전트</br> FIM CM 요청 등록</br> FIM CM 요청 복구</br> FIM CM 요청 갱신</br> FIM CM 요청 철회 </br> FIM CM 요청 스마트 카드 차단 해제 |
    | mimcm-HelpDesk | 읽기</br> FIM CM 등록 에이전트</br> FIM CM 요청 철회</br> FIM CM 요청 스마트 카드 차단 해제 |

11. **CORPDC 속성** 대화 상자에서 **확인**을 클릭합니다.

12. **Active Directory 사용자 및 컴퓨터**를 열려 있는 상태로 둡니다.

**하위 사용자 개체에 대한 사용 권한을 구성**합니다.

1. 여전히 **Active Directory 사용자 및 컴퓨터** 콘솔에 있는지 확인합니다.

2. 콘솔 트리에서 **Contoso.com**을 마우스 오른쪽 단추로 클릭하고 **속성**을 클릭합니다.

3. **보안** 탭에서 **고급**을 클릭합니다.

4. **Contoso에 대한 고급 보안 설정** 대화 상자에서 **추가**를 클릭합니다.

5. **사용자, 컴퓨터, 서비스 계정 또는 그룹 선택** 대화 상자에서 **선택할 개체 이름 입력** 상자에 **mimcm-Managers**를 입력한 후 **확인**을 클릭합니다.

6. **Permission Entry for Contoso(Contoso에 대한 권한 항목)** 대화 상자의 **적용 대상** 목록에서 **하위 사용자 개체**를 선택한 후 다음 **사용 권한**에 대해 **허용** 확인란을 사용합니다.

    - **모든 속성 읽기**

    - **읽기 권한**

    - **FIM CM 감사**

    - **FIM CM 등록 에이전트**

    - **FIM CM 요청 등록**

    - **FIM CM 요청 복구**

    - **FIM CM 요청 갱신**

    - **FIM CM 요청 철회**

    - **FIM CM 요청 스마트 카드 차단 해제**

7. **Permission Entry for Contoso(Contoso에 대한 권한 항목)** 대화 상자에서 **확인**을 클릭합니다.

8. **Contoso에 대한 고급 보안 설정** 대화 상자에서 **추가**를 클릭합니다.

9. **사용자, 컴퓨터, 서비스 계정 또는 그룹 선택** 대화 상자에서 **선택할 개체 이름 입력** 상자에 **mimcm-HelpDesk**를 입력한 후 **확인**을 클릭합니다.

10. **Permission Entry for Contoso(Contoso에 대한 권한 항목)** 대화 상자의 **적용 대상** 목록에서 **하위 사용자 개체**를 선택한 후, 다음 **사용 권한**에 대해 **허용** 확인란을 선택합니다.

    - **모든 속성 읽기**

    - **읽기 권한**

    - **FIM CM 등록 에이전트**

    - **FIM CM 요청 철회**

    - **FIM CM 요청 스마트 카드 차단 해제**

11. **Permission Entry for Contoso(Contoso에 대한 권한 항목)** 대화 상자에서 **확인**을 클릭합니다.

12. **Contoso에 대한 고급 보안 설정** 대화 상자에서 **확인**을 클릭합니다.

13. **contoso.com 속성** 대화 상자에서 **확인**을 클릭합니다.

14. **Active Directory 사용자 및 컴퓨터**를 열려 있는 상태로 둡니다.

**하위 사용자 개체에 대한 사용 권한을 구성합니다.\<스크립트 없음\>**

1. 여전히 **Active Directory 사용자 및 컴퓨터** 콘솔에 있는지 확인합니다.

2. 콘솔 트리에서 **Contoso.com**을 마우스 오른쪽 단추로 클릭하고 **속성**을 클릭합니다.

3. **보안** 탭에서 **고급**을 클릭합니다.

4. **Contoso에 대한 고급 보안 설정** 대화 상자에서 **추가**를 클릭합니다.

5. **사용자, 컴퓨터, 서비스 계정 또는 그룹 선택** 대화 상자에서 **선택할 개체 이름 입력** 상자에 **mimcm-Managers**를 입력한 후 **확인**을 클릭합니다.

6. **Permission Entry for Contoso(Contoso에 대한 권한 항목)** 대화 상자의 **적용 대상** 목록에서 **하위 사용자 개체**를 선택한 후 다음, **사용 권한**에 대해 **허용** 확인란을 사용합니다.

    - **모든 속성 읽기**

    - **읽기 권한**

    - **FIM CM 감사**

    - **FIM CM 등록 에이전트**

    - **FIM CM 요청 등록**

    - **FIM CM 요청 복구**

    - **FIM CM 요청 갱신**

    - **FIM CM 요청 철회**

    - **FIM CM 요청 스마트 카드 차단 해제**

7. **Permission Entry for CONTOSO(CONTOSO에 대한 권한 항목)** 대화 상자에서 **확인**을 클릭합니다.

8. **CONTOSO에 대한 고급 보안 설정** 대화 상자에서 **추가**를 클릭합니다.

9. **사용자, 컴퓨터, 서비스 계정 또는 그룹 선택** 대화 상자에서 **선택할 개체 이름 입력** 상자에 **mimcm-HelpDesk**를 입력한 후 **확인**을 클릭합니다.

10. **Permission Entry for Contoso(Contoso에 대한 권한 항목)** 대화 상자의 **적용 대상** 목록에서 **하위 사용자 개체**를 선택한 후, 다음 **사용 권한**에 대해 **허용** 확인란을 선택합니다.

    - **모든 속성 읽기**

    - **읽기 권한**

    - **FIM CM 등록 에이전트**

    - **FIM CM 요청 철회**

    - **FIM CM 요청 스마트 카드 차단 해제**

11. **Permission Entry for contoso(contoso에 대한 권한 항목)** 대화 상자에서 **확인**을 클릭합니다.

12. **Contoso에 대한 고급 보안 설정** 대화 상자에서 **확인**을 클릭합니다.

13. **contoso.com 속성** 대화 상자에서 **확인**을 클릭합니다.

14. **Active Directory 사용자 및 컴퓨터**를 열려 있는 상태로 둡니다.

두 번째 단계: **인증서 템플릿 관리 권한 위임 \<스크립트\>**

- 인증서 템플릿 컨테이너에 대한 권한을 위임합니다.

- OID 컨테이너에 대한 권한을 위임합니다.

- 기존 인증서 템플릿에 대한 권한을 위임합니다.

인증서 템플릿 컨테이너에 대한 권한을 정의합니다.

1. **Active Directory 사이트 및 서비스** 콘솔을 복원합니다.

2. 콘솔 트리에서 **서비스**, **공개 키 서비스**를 차례로 확장한 다음 **인증서 템플릿**을 클릭합니다.

3. 콘솔 트리에서 **인증서 템플릿**을 마우스 오른쪽 단추로 클릭한 후 **제어 위임**을 클릭합니다.

4. **제어 위임** 마법사에서 **다음**을 클릭합니다.

5. **사용자 또는 그룹** 페이지에서 **추가**를 클릭합니다.

6. **사용자, 컴퓨터 또는 그룹 선택** 대화 상자의 **선택할 개체 이름 입력** 상자에 **mimcm-Managers**를 입력한 후, **확인**을 클릭합니다.

7. **사용자 또는 그룹** 페이지에서 **다음**을 클릭합니다.

8. **위임할 작업** 페이지에서 **위임할 사용자 지정 작업 만들기**를 클릭한 후 **다음**을 클릭합니다.

9.  **Active Directory 개체 형식** 페이지에서 **이 폴더, 이 폴더에 있는 기존 개체 및 이 폴더에 새 개체 만들기**가 선택되었는지 확인한 후, **다음**을 클릭합니다.

10. **사용 권한** 페이지의 **사용 권한** 목록에서 **모든 권한** 확인란을 선택한 후 **다음**을 클릭합니다.

11. **Completing the Delegation of Control Wizard(컨트롤 마법사 위임 완료)** 페이지에서 **마침**을 클릭합니다.

OID 컨테이너에 대한 권한을 정의합니다.

1. 콘솔 트리에서 **OID**를 마우스 오른쪽 단추로 클릭한 다음 **속성**을 클릭합니다.

2. **OID 속성** 대화 상자의 **보안** 탭에서 **고급**을 클릭합니다.

3. **OID에 대한 고급 보안 설정** 대화 상자에서 **추가**를 클릭합니다.

4. **사용자, 컴퓨터, 서비스 계정 또는 그룹 선택** 대화 상자에서 **선택할 개체 이름 입력** 상자에 **mimcm-Managers**를 입력한 후 **확인**을 클릭합니다.

5. **Permissions Entry for OID(OID에 대한 권한 항목)** 대화 상자에서 해당 사용 권한이 **이 개체 및 모든 하위 개체**에 적용되는지 확인한 후 **모든 권한**, **확인**을 차례로 클릭합니다.

6. **OID에 대한 고급 보안 설정** 대화 상자에서 **확인**을 클릭합니다.

7. **OID 속성** 대화 상자에서 **확인**을 클릭합니다.

8. **Active Directory 사이트 및 서비스**를 닫습니다.

**스크립트: OID, 프로필 템플릿 및 인증서 템플릿 컨테이너에 대한 사용 권한**

![다이어그램](media/mim-cm-deploy/image021.png)

```powershell
import-module activedirectory
$adace = @{
"OID" = "AD:\\CN=OID,CN=Public Key Services,CN=Services,CN=Configuration,DC=contoso,DC=com";
"CT" = "AD:\\CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=contoso,DC=com";
"PT" = "AD:\\CN=Profile Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=contoso,DC=com"
}
$adace.GetEnumerator() | **Foreach-Object** {
$acl = **Get-Acl** *-Path* $_.Value
$sid=(**Get-ADGroup** "MIMCM-Managers").SID
$p = **New-Object** System.Security.Principal.SecurityIdentifier($sid)
##https://msdn.microsoft.com/library/system.directoryservices.activedirectorysecurityinheritance(v=vs.110).aspx
$ace = **New-Object** System.DirectoryServices.ActiveDirectoryAccessRule
($p,[System.DirectoryServices.ActiveDirectoryRights]"GenericAll",[System.Security.AccessControl.AccessControlType]::Allow,
[DirectoryServices.ActiveDirectorySecurityInheritance]::All)
$acl.AddAccessRule($ace)
**Set-Acl** *-Path* $_.Value *-AclObject* $acl
}
```

**스크립트: 기존 인증서 템플릿에 대한 권한 위임**  

![다이어그램](media/mim-cm-deploy/image039.png)

```shell
dsacls "CN=Administrator,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CAExchange,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CEPEncryption,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ClientAuth,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CodeSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CrossCA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CTLSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DirectoryEmailReplication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainController,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainControllerAuthentication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFS,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFSRecovery,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgentOffline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUser,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUserSignature,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMEnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMKeyRecoveryAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOffline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOnline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KerberosAuthentication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KeyRecoveryAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Machine,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=MachineEnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OCSPResponseSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OfflineRouter,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=RASAndIASServer,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardLogon,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardUser,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SubCA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=User,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=UserSignature,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=WebServer,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Workstation,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO
```
