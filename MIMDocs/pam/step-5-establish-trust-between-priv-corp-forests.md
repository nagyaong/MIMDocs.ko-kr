---
title: "PAM 배포 5단계 – 포리스트 링크 | 문서"
description: "PRIV의 권한 있는 사용자가 CORP의 리소스에 계속 액세스할 수 있도록 PRIV 및 CORP 포리스트 간에 트러스트를 설정합니다."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: eef248c4-b3b6-4b28-9dd0-ae2f0b552425
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1239ca2c0c6d376420723da01d7aa42821f5980f
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# 5단계 - PRIV 및 CORP 포리스트 간에 트러스트 설정
<a id="step-5--establish-trust-between-priv-and-corp-forests" class="xliff"></a>

>[!div class="step-by-step"]
[« 4단계](step-4-install-mim-components-on-pam-server.md)
[6단계 »](step-6-transition-group-to-pam.md)


contoso.local과 같은 각 CORP 도메인에 대해 PRIV 및 CONTOSO 도메인 컨트롤러는 트러스트에 구속되어야 합니다. 이렇게 하면 PRIV 도메인의 사용자가 CORP 도메인의 리소스에 액세스할 수 있습니다.

## 각 도메인 컨트롤러를 상대 도메인 컨트롤러에 연결
<a id="connect-each-domain-controller-to-its-counterpart" class="xliff"></a>

트러스트를 설정하기 전에, 다른 도메인 컨트롤러/DNS 서버의 IP 주소를 기반으로 상대방 도메인 컨트롤러의 DNS 이름 확인을 위해 각 도메인 컨트롤러가 구성되어야 합니다.

1.  도메인 컨트롤러 또는 MIM 소프트웨어가 설치된 서버를 가상 컴퓨터로 배포한 경우 해당 컴퓨터에 도메인 명명 서비스를 제공하는 다른 DNS 서버가 없는지 확인합니다.
    - 가상 컴퓨터에 공용 네트워크에 연결된 네트워크 인터페이스를 포함하여 여러 개의 네트워크 인터페이스가 있는 경우 일시적으로 연결을 사용하지 않도록 설정하거나 Windows 네트워크 인터페이스 설정을 재정의해야 할 수 있습니다. DHCP에서 제공하는 DNS 서버 주소를 가상 컴퓨터에서 사용하지 않는지 확인해야 합니다.

2.  각 기존 CORP 도메인 컨트롤러가 PRIV 포리스트에 이름을 라우팅할 수 있는지 확인합니다. CORPDC와 같이 PRIV 포리스트 외부의 각 도메인 컨트롤러에서 PowerShell을 시작하고 다음 명령을 입력합니다.

    ```
    nslookup -qt=ns priv.contoso.local.
    ```
    PRIV 도메인에 대한 이름 서버 레코드가 올바른 IP 주소와 함께 출력에 표시되는지 확인합니다.

3.  도메인 컨트롤러가 PRIV 도메인을 라우팅할 수 없는 경우 **DNS 관리자**(**시작** > **응용 프로그램 도구** > **DNS**에 있음)를 사용하여 PRIVDC의 IP 주소로 PRIV 도메인의 DNS 이름을 전달하도록 구성합니다. 상위 도메인(예: contoso.local)인 경우 이 도메인 컨트롤러와 해당 도메인에 대한 노드(예: **CORPDC** > **정방향 조회 영역** > **contoso.local**)를 확장하고 이름이 **priv**인 키가 NS(이름 서버) 형식으로 존재하는지 확인합니다.

    ![priv 키에 대한 파일 구조 - 스크린샷](./media/PAM_GS_DNS_Manager.png)

## PAMSRV에 트러스트 설정
<a id="establish-trust-on-pamsrv" class="xliff"></a>

PAMSRV에서 각 도메인(예: CORPDC)과 단방향 트러스트를 설정하여 CORP 도메인 컨트롤러가 PRIV 포리스트를 트러스트하도록 합니다.

1. PRIV 도메인 관리자(PRIV\Administrator)로 PAMSRV에 로그인합니다.

2.  PowerShell을 시작합니다.

3.  각 기존 포리스트에 대해 다음 PowerShell 명령을 입력합니다. 메시지가 표시되면 CORP 도메인 관리자에 대한 자격 증명(CONTOSO\Administrator)을 입력합니다.

    ```
    $ca = get-credential
    New-PAMTrust -SourceForest "contoso.local" -Credentials $ca
    ```

4.  기존 포리스트의 각 도메인에 대해 다음 PowerShell 명령을 입력합니다. 메시지가 표시되면 CORP 도메인 관리자에 대한 자격 증명(CONTOSO\Administrator)을 입력합니다.

    ```
    $ca = get-credential
    New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials $ca
    ```

## Active Directory에 포리스트 읽기 권한 부여
<a id="give-forests-read-access-to-active-directory" class="xliff"></a>

각 기존 포리스트에 대해 PRIV 관리자 및 모니터링 서비스에 의한 AD의 읽기 권한을 사용하도록 설정합니다.

1.  해당 포리스트(Contoso\Administrator)에서 최상위 도메인의 도메인 관리자로 기존 CORP 포리스트 도메인 컨트롤러(CORPDC)에 로그인합니다.  
2.  **Active Directory 사용자 및 컴퓨터**를 시작합니다.  
3.  도메인 **contoso.local**을 마우스 오른쪽 단추로 클릭하고 **위임 컨트롤**을 선택합니다.  
4.  선택한 사용자 및 그룹 탭에서 **추가**를 클릭합니다.  
5.  사용자, 컴퓨터 또는 그룹 선택 창에서 **위치**를 클릭하고 위치를 *priv.contoso.local*로 변경합니다.  개체 이름에서 *Domain Admins*를 입력하고 **이름 확인**을 클릭합니다. 팝업이 표시되면 사용자 이름 *priv\administrator* 및 암호를 입력합니다.  
6.  Domain Admins 뒤에 "*; MIMMonitor*"를 추가합니다. **Domain Admins** 및 **MIMMonitor** 이름에 밑줄이 표시되면 **확인**을 클릭하고 **다음**을 클릭합니다.  
7.  일반 작업 목록에서 **모든 사용자 정보 읽기**를 선택한 후 **다음**과 **마침**을 차례로 클릭합니다.  
8.  Active Directory 사용자 및 컴퓨터를 닫습니다.

9.  PowerShell 창을 엽니다.  
10.  `netdom`을 사용하여 SID 기록은 사용하도록 설정되고 SID 필터링은 사용하지 않도록 설정되었는지 확인합니다. 종류:  
    ```
    netdom trust contoso.local /quarantine /domain priv.contoso.local
    netdom trust /enablesidhistory:yes /domain priv.contoso.local
    ```
    출력에 **이 트러스트에 대해 SID 기록을 사용하도록 설정하는 중입니다.** 또는 **이 트러스트에 대해 SID 기록을 이미 사용할 수 있습니다.**가 나타나야 합니다.

    또한 출력에 **이 트러스트에 대해 SID 필터링을 사용할 수 없습니다.**가 나타나야 합니다. 자세한 내용은 [SID 필터 격리 사용 안 함](http://technet.microsoft.com/library/cc772816.aspx)을 참조하세요.

## 모니터링 및 구성 요소 서비스 시작
<a id="start-the-monitoring-and-component-services" class="xliff"></a>

1.  PRIV 도메인 관리자(PRIV\Administrator)로 PAMSRV에 로그인합니다.

2.  PowerShell을 시작합니다.

3.  다음 PowerShell 명령을 입력합니다.

    ```
    net start "PAM Component service"
    net start "PAM Monitoring service"
    ```

다음 단계에서는 PAM으로 그룹을 이동합니다.

>[!div class="step-by-step"]
[« 4단계](step-4-install-mim-components-on-pam-server.md)
[6단계 »](step-6-transition-group-to-pam.md)
