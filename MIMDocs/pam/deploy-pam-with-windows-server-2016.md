---
title: "Windows Server 2016을 사용하여 MIM 권한 있는 액세스 관리 배포 | Microsoft 문서"
description: "Sever 2016을 사용하여 권한 있는 액세스 관리 배포에 대한 자세한 정보"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 03/24/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 
translationtype: Human Translation
ms.sourcegitcommit: 77ecdb91ccfdb1afec830e9662163ab9a7ef250c
ms.openlocfilehash: dc68c4dcf2ae2d347e10930613bd32ca02031f8b
ms.lasthandoff: 03/24/2017


---



# <a name="deploy-mim-pam-with-windows-server-2016"></a>Windows Server 2016을 사용하여 MIM PAM 배포


이 시나리오에서는 MIM 2016 SP1에서 Windows Server 2016을 “PRIV” 포리스트에 대한 도메인 컨트롤러로 활용하도록 설정합니다.  이 시나리오를 구성한 후에는 사용자의 Kerberos 티켓 시간이 해당 역할의 남은 활성화 시간으로 제한됩니다. 

>[!Note]
Technical Preview 5 전의 Windows Server 2016 기술 미리 보기는 이 MIM 릴리스와 함께 사용할 수 없습니다.

## <a name="preparation"></a>준비

랩 환경에는 최소 두 개의 VM이 필요합니다.

-   Windows Server 2016를 실행하는 PRIV 도메인 컨트롤러를 호스트하는 VM

-   Windows Server 2016(권장) 또는 Windows Server 2012 R2를 실행하는 MIM 서비스를 호스트하는 VM

>[!NOTE]
랩 환경에 아직 "CORP" 도메인이 없는 경우 해당 도메인에 대한 추가 도메인 컨트롤러가 필요합니다. “CORP” 도메인 컨트롤러는 Windows Server 2016 또는 Windows Server 2012 R2를 실행할 수 있습니다.


**다음과 같은 경우를 제외하고** [시작 가이드](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services.md)에 설명된 대로 설치하세요.

-   새 CORP 도메인을 만드는 경우 [1단계 - CORP 도메인 컨트롤러 준비](/microsoft-identity-manager/pam/step-1-prepare-corp-domain.md)의 지침을 따를 때 Windows Server 2016 기능 수준에서 CORP 도메인을 선택적으로 구성할 수 있습니다. **이 옵션을 선택하는 경우 다음과 같이 조정하세요.**

    -   Windows Server 2016 미디어를 사용하는 경우 설치 옵션은 Windows Server 2016(데스크톱 환경을 사용하는 서버)입니다.

    -   다음과 같이 Install-ADDSForest 명령에 대한 인수에 도메인 및 포리스트 버전 번호로 7을 제공하여 CORP 포리스트 및 도메인에 대한 Windows Server 2016 기능 수준을 지정할 수 있습니다.
     ```
        Install-ADDSForest –DomainMode 7 –ForestMode 7 –DomainName contoso.local –DomainNetbiosName contoso –Force –NoDnsOnNetwork
        ```
    -   CORP 및 PRIV 도메인 컨트롤러가 Windows Server 2016 도메인 기능 수준인 경우 "새 사용자 및 그룹 만들기"에서 최종 명령(New-ADGroup -name 'CONTOSO\$\$\$' …)이 **필요하지 않습니다**.

    -   CORP 및 PRIV 도메인 컨트롤러가 Windows Server 2016 도메인 기능 수준인 경우 "감사 구성"(항목 #8) 및 "레지스트리 설정 구성"(항목 #10)에 설명된 변경 사항은 **권장되지만 필수는 아닙니다**.

-   CORPDC의 운영 체제로 Windows Server 2012 R2를 사용하도록 선택하는 경우 CORPDC에 핫픽스 2919442, 2919355 및 [업데이트 3155495](http://support.microsoft.com/kb/3156418)를 설치해야 합니다.

-   다음과 같은 조정을 제외하고, [2단계 - PRIV 도메인 컨트롤러 준비](/microsoft-identity-manager/pam/step-2-prepare-priv-domain-controller.md)의 지침을 따릅니다.

    -   Windows Server 2016 미디어를 사용하여 설치합니다. 설치 옵션은 Windows Server 2016(데스크톱 환경을 사용하는 서버)입니다.

    -   나중에 설명하는 Windows Server AD 기능을 사용하도록 허용하려면 "역할 추가" 지침(항목 #4)에서 **PowerShell 명령의 네 번째 줄에 도메인 및 포리스트 버전 번호를 7로 지정해야 합니다**.

        ```
        Install-ADDSForest -DomainMode 7 -ForestMode 7 -DomainName priv.contoso.local  -DomainNetbiosName priv -Force -CreateDNSDelegation -DNSDelegationCredential $ca
        ```  

    -   감사 및 로그온 권한을 구성할 때 그룹 정책 관리 프로그램은 Windows 관리 도구 폴더에 있습니다.

    -   SID 기록 마이그레이션(항목 #8)에 필요한 레지스트리 설정 구성은 **PRIV 도메인이 Windows Server 2016 도메인 기능 수준인 경우 필요하지 않습니다**.

    -   위임을 구성한 후, 그리고 서버를 다시 시작하기 전에 관리자로서 PowerShell 창을 시작하고 다음 명령을 입력하여 Windows Server 2016 Active Directory에서 권한 있는 액세스 관리 기능을 사용하도록 설정합니다.

    ```
    $of = get-ADOptionalFeature -filter "name -eq 'privileged access management feature'"
    Enable-ADOptionalFeature $of -scope ForestOrConfigurationSet -target "priv.contoso.local"
    ```

  -   위임을 구성한 후, 그리고 서버를 다시 시작하기 전에 MIM 관리자 및 MIM 서비스 계정에 섀도 주체를 만들고 업데이트할 수 있는 권한을 부여합니다.

     a. PowerShell 창을 시작하고 ADSIEdit을 입력합니다.

     b. [작업] 메뉴를 열고 "연결 대상"을 클릭합니다. [연결 지점 설정]에서 명명 컨텍스트를 “기본 명명 컨텍스트”에서 “구성”으로 변경하고 [확인]을 클릭합니다.

     c. 연결한 후 “ADSI 편집” 아래의 창 왼쪽에서 구성 노드를 확장하여 “CN=Configuration,DC=priv,....”를 확인합니다. CN=Configuration을 확장한 다음 CN=Services를 확장합니다.

     d. “CN=Shadow Principal Configuration”을 마우스 오른쪽 단추로 클릭하고 [속성]을 클릭합니다. 속성 대화 상자가 나타나면 [보안] 탭으로 변경합니다.

     e. 추가를 클릭합니다. 나중에 New-PAMGroup을 수행할 다른 MIM 관리자뿐만 아니라 “MIMService” 계정을 지정하여 추가 PAM 그룹을 만듭니다. 각 사용자에 대해 허용되는 사용 권한 목록에 “쓰기”, “모든 자식 개체 만들기” 및 “모든 자식 개체 삭제”를 추가합니다. 사용 권한을 추가합니다.

     f. 고급 보안 설정으로 변경합니다. MIMService 액세스를 허용하는 줄에서 [편집]을 클릭합니다. "적용 대상" 설정을 "이 개체 및 모든 하위 개체"로 변경합니다. 이 권한 설정을 업데이트하고 보안 대화 상자를 닫습니다.

     g. ADSI 편집을 닫습니다.

 -   위임을 구성한 후, 그리고 서버를 다시 시작하기 전에 MIM 관리자에게 인증 정책을 만들고 업데이트할 수 있는 권한을 부여합니다.

     a.  관리자 권한 **명령 프롬프트**를 시작하고 각 네 줄에 "mimadmin"에 대한 MIM 관리자 계정의 이름을 대체하는 다음 명령을 입력합니다.
    ```
       dsacls "CN=AuthN Policies,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:RPWPRCWD;;msDS-AuthNPolicy /i:s

       dsacls "CN=AuthN Policies,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:CCDC;msDS-AuthNPolicy

       dsacls "CN=AuthN Silos,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:RPWPRCWD;;msDS-AuthNPolicySilo /i:s

       dsacls "CN=AuthN Silos,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:CCDC;msDS-AuthNPolicySilo
    ```


-   이러한 조정과 함께 [3단계 - PAM 서버 준비](/microsoft-identity-manager/pam/step-3-prepare-pam-server.md)의 지침을 따릅니다.

    -   Windows Server 2016을 설치하는 경우 "ApplicationServer" 역할을 사용할 수 없습니다.

    -   Windows Server 2016에 MIM을 설치하는 경우 **SharePoint 2013을 설치할 수 없습니다**.

-   이러한 조정과 함께 [4단계 – PAM 서버와 워크스테이션에 MIM 구성 요소 설치](/microsoft-identity-manager/pam/step-4-install-mim-components-on-pam-server.md)의 지침을 따릅니다.

    -   MIM 설치에서 새 AD OU "PAM 개체"를 만들 때 MIM 서비스 및 PAM 구성 요소를 설치하는 사용자는 **AD의 PRIV 도메인에 대한 쓰기 액세스 권한이 있어야 합니다**.

    -   SharePoint가 설치되어 있지 않은 경우 MIM 포털을 설치하지 마세요.

-   이러한 조정과 함께 [5단계 - 트러스트 설정](/microsoft-identity-manager/pam/step-5-establish-trust-between-priv-corp-forests.md)의 지침을 따릅니다.

    -   단방향 트러스트를 설정할 때 처음 두 개의 PowerShell 명령(get-credential 및 New-PAMTrust)만 수행하고 **New-PAMDomainConfiguration 명령을 수행하지 마세요**.

    -   트러스트를 설정한 다음, PRIV \\관리자로 PRIVDC에 로그온하고, PowerShell을 시작하여 다음 명령을 입력합니다.
  ```
    netdom trust contoso.local /domain:priv.contoso.local /enablesidhistory:yes
     /usero:contoso\\administrator /passwordo:Pass\@word1

     netdom trust contoso.local /domain:priv.contoso.local /quarantine:no
     /usero:contoso\\administrator /passwordo:Pass\@word1  

     netdom trust contoso.local /domain:priv.contoso.local /enablepimtrust:yes
     /usero:contoso\\administrator /passwordo:Pass\@word1
  ```

-   항목 #5(트러스트 확인)는 **CORP 및 PRIV 도메인이 모두 Windows Server 2016 도메인 기능 수준에 있는 경우 필요하지 않습니다**.

## <a name="more-information"></a>추가 정보

- [Active Directory Domain Services에 대한 권한 있는 액세스 관리](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services.md)
- [권한 있는 액세스 관리를 위해 MIM 환경 구성](/microsoft-identity-manager/pam/configuring-mim-environment-for-pam.md)
- [스크립트를 사용하여 PAM 구성](/microsoft-identity-manager/pam/sp1-pam-configure-using-scripts.md)

