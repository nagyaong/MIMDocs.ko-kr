---
title: PAM 배포 4단계 – MIM 설치 | Microsoft 문서
description: Privileged Access Management 서버 및 워크스테이션에서 MIM 서비스 및 포털을 설치하고 구성합니다.
keywords: ''
author: barclayn
ms.author: barclayn
manager: barclayn
ms.date: 09/13/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: ef605496-7ed7-40f4-9475-5e4db4857b4f
ROBOTS: noindex,nofollow
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 81fe10b8fbf8ada08983c4bf3c58f85215cf1d66
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36290036"
---
# <a name="step-4--install-mim-components-on-pam-server-and-workstation"></a>4단계 – PAM 서버와 워크스테이션에 MIM 구성 요소 설치

> [!div class="step-by-step"]
> [« 3단계](step-3-prepare-pam-server.md)
> [5단계 »](step-5-establish-trust-between-priv-corp-forests.md)

PAMSRV에서 MIM 서비스 및 포털과 샘플 포털 웹 응용 프로그램을 설치할 수 있도록 PRIV\Administrator로 로그인합니다.

  > [!NOTE]
  > 사용자는 도메인 관리자여야 합니다. 도메인 관리자로 다음 명령을 실행하지 않으면 다음 단계에서 트러스트 유효성 확인 검사가 완료되지 않습니다.

MIM을 다운로드한 경우 새 폴더로 MIM 설치 보관 압축을 풉니다.

## <a name="run-the-service-and-portal-install-program"></a>서비스 및 포털 설치 프로그램을 실행합니다.

설치 관리자의 지침을 따라 설치를 완료합니다.

1. 구성 요소 기능을 선택할 때 MIM 서비스(Privileged Access Management 포함, MIM 보고 제외) 및 MIM 포털을 포함합니다.  

   ![사용자 지정 설치 - 스크린샷](./media/PAM_GS_MIM_2015_Service_Portal.png)

2. 공통 서비스 및 MIM 데이터베이스 연결을 구성할 때 **새 데이터베이스 만들기**를 지정합니다.

   > [!NOTE]
   > 고가용성을 위해 MIM 서비스를 여러 번 설치하는 경우 모든 후속 설치에 대해 **기존 데이터베이스 사용**을 지정합니다.

3. 메일 서버 연결을 구성할 때 메일 서버를 CORP 환경에 대한 Exchange 또는 SMTP 서버의 호스트 이름으로 설정하고(메일 서버가 없는 경우 corpdc.contoso.local 사용) **SSL 사용** 및 **Mail Server is Exchange Server 2007 or Exchange Server 2010(메일 서버가 Exchange Server 2007 또는 Exchange Server 2010임)** 확인란을 선택 취소합니다.

4. 새 자체 서명된 인증서를 생성하도록 선택합니다.

5. 다음 계정 자격 증명을 설정합니다.
   - 서비스 계정 이름: *MIMService*  
   - 서비스 계정 암호: <em>Pass@word1</em>(또는 2단계에서 만든 암호)  
   - 서비스 계정 도메인: *PRIV*  
   - 서비스 메일 계정: <em>MIMService@priv.contoso.local</em>  

6. 동기화 서버 호스트 이름에 대해 기본값을 그대로 사용하고 MIM 관리 에이전트 계정을 *PRIV\MIMMA*로 지정합니다. MIM 동기화 서비스가 없다는 경고 메시지가 표시됩니다. MIM 동기화 서비스는 이 시나리오에서 사용되지 않으므로 이는 정상입니다.

7. MIM 서비스 서버 주소를 *PAMSRV*로 설정합니다.

8. SharePoint 사이트 모음 URL로 *http://pamsrv.priv.contoso.local:82*를 지정합니다.

9. 등록 포털 URL은 비워 둡니다.

10. 방화벽에서 5725 및 5726 포트 열기에 대한 확인란 및 MIM 포털 사이트에 대해 모든 인증된 사용자 액세스 권한 부여에 대한 확인란을 선택합니다.

11. PAM REST API 호스트 이름은 비워 두고 포트 번호를 *8086*으로 설정합니다.

    ![PAM REST API에 대한 바인딩 정보 - 스크린샷](./media/PAM_GS_MIM_2015_Service_Portal_configure_application_pool.png)

12. SharePoint와 동일한 계정을 사용하도록 MIM PAM REST API 계정을 구성합니다(MIM 포털이 이 서버에 함께 있을 때).
    - 응용 프로그램 풀 계정 이름: *SharePoint*  
    - 응용 프로그램 풀 계정 암호: <em>Pass@word1</em>(또는 2단계에서 만든 암호)  
    - 응용 프로그램 풀 계정 도메인: *PRIV*  

    ![응용 프로그램 풀 계정 자격 증명 - 스크린샷](./media/PAM_GS_Configure_Component_Service.png)

    현재 구성에서 서비스 계정이 안전하지 않다는 경고가 나타날 수 있습니다. 하지만 괜찮습니다.

13. MIM PAM 구성 요소 서비스를 구성합니다.
    - 서비스 계정 이름: *MIMComponent*
    - 서비스 계정 암호: <em>Pass@word1</em>(또는 2단계에서 만든 암호)  
    - 서비스 계정 도메인: *PRIV*

    ![PAM 구성 요소 서비스 계정 자격 증명 - 스크린샷](./media/PAM_GS_Configure_MIM_PAM_component_service.png)

14. PAM 모니터링 서비스를 구성합니다.
    - 서비스 계정 이름: *MIMMonitor*  
    - 서비스 계정 암호: <em>Pass@word1</em>(또는 2단계에서 만든 암호)  
    - 서비스 계정 도메인: *PRIV*  

    ![PAM 모니터링 서비스 계정 자격 증명 - 스크린샷](./media/PAM_GS_Configur_PAM_Monitoring_service.png)

15. MIM 암호 포털에 대한 정보 입력 페이지에서 확인란을 비워 두고 계속 진행합니다. **다음**을 클릭하여 설치를 계속 진행합니다.

설치가 완료된 후 서버를 다시 부팅한 다음 MIM 포털이 활성화되어 있는지 확인하고 사용자가 MIM에서 자신의 개체 리소스를 볼 수 있도록 설정합니다.

## <a name="set-up-mim-portal-management-policy-rules"></a>MIM 포털 관리 정책 규칙 설정

1. PAMSRV를 다시 부팅한 후 PRIV\Administrator로 로그온합니다.

2. Internet Explorer를 시작하고 http://pamsrv.priv.contoso.local:82/identitymanagement에서 MIM 포털에 연결합니다. 이 페이지를 처음 연결하면 약간 시간이 걸릴 수 있습니다.

3. 필요한 경우 PRIV\Administrator로 Internet Explorer에 로그인합니다.

4. Internet Explorer에서 **인터넷 옵션**을 열고 **보안** 탭으로 변경하고 **로컬 인트라넷 영역**에 해당 사이트가 아직 없는 경우 추가합니다. 인터넷 옵션 대화 상자를 닫습니다.

5. Internet Explorer를 사용하여 MIM 포털을 보려면 **관리 정책 규칙**을 클릭합니다.

6. 관리 정책 규칙 **사용자 관리: 사용자가 자체 특성을 읽을 수 있음**을 검색합니다.

7. 이 관리 정책 규칙을 선택하고 **정책 사용 안 함**을 선택 취소하고 **확인**을 클릭한 다음 **제출**을 클릭합니다.

## <a name="verify-the-firewall-connections"></a>방화벽 연결 확인

방화벽은 5725, 5726, 8086 및 8090 TCP 포트에 대해 들어오는 연결을 허용해야 합니다.

1.  **고급 보안이 포함된 Windows 방화벽**(관리 도구에 있음)을 실행합니다.  
2.  **인바운드 규칙**을 클릭합니다.  
3.  다음 두 규칙이 나열되어 있는지 확인합니다.  
    - Forefront Identity Manager 서비스(STS)
    - Forefront Identity Manager 서비스(웹 서비스)  
4.  **새 규칙** > **포트** > **TCP**를 클릭하고 특정 로컬 포트 *8086* 및 *8090*을 입력합니다. 마법사에서 기본값을 적용하고 규칙 이름을 지정하고 **마침**을 클릭합니다.  
5.  마법사를 완료한 후 Windows 방화벽 응용 프로그램을 닫습니다.

6.  **제어판**을 시작합니다.  
7.  네트워크 및 인터넷 아래에서 **네트워크 상태 및 작업 보기**를 선택합니다.  
8.  priv.contoso.local과 도메인 네트워크가 나열된 활성 네트워크가 있는지 확인합니다.  
9. **제어판**을 닫습니다.

## <a name="set-up-the-sample-web-application"></a>샘플 웹 응용 프로그램 설정

이 섹션에서는 MIM PAM REST API에 대한 샘플 웹 응용 프로그램을 설치하고 구성합니다.

1. 샘플 웹 응용 프로그램 보관 파일에서 [ID 관리 샘플](https://github.com/Azure/identity-management-samples)을 zip 파일로 다운로드합니다.

2. 새 폴더 **C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal**에 **identity-management-samples-master\Privileged-Access-Management-Portal\src** 폴더 콘텐츠의 압축을 풉니다.

3. IIS에서 사이트 이름이 MIM Privileged Access Management 예제 포털이고, 실제 경로가 C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal이면서 포트 8090을 사용하는 새 웹 사이트를 만듭니다.  다음 PowerShell 명령을 사용하여 이를 수행할 수 있습니다.

   ```PowerShell
   New-WebSite -Name "MIM Privileged Access Management Example Portal" -Port 8090   -PhysicalPath "C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\"
   ```

4. 샘플 웹 응용 프로그램을 설정하면 사용자를 MIM PAM REST API로 리디렉션할 수 있습니다. 메모장과 같은 텍스트 편집기를 사용하여 **C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management REST API\web.config** 파일을 편집합니다. **<system.webServer>** 섹션에서 다음 줄을 추가합니다.

   ```XML
   <httpProtocol>
   <customHeaders>
     <add name="Access-Control-Allow-Credentials" value="true"  />
     <add name="Access-Control-Allow-Headers" value="content-type" />
     <add name="Access-Control-Allow-Origin" value="http://pamsrv:8090" />  
   </customHeaders>
   </httpProtocol>
   ```

5. 샘플 웹 응용 프로그램을 구성합니다. 메모장과 같은 텍스트 편집기를 사용하여 **C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\js\utils.js** 파일을 편집합니다. **pamRespApiUrl**의 값을 *http://pamsrv.priv.contoso.local:8086/api/pamresources/* 로 설정합니다.

6. 이러한 변경 내용을 적용하려면 다음 명령을 사용하여 IIS를 다시 시작합니다.

   ```cmd
   iisreset
   ```

7. (선택 사항) 사용자가 REST API에 인증할 수 있는지 확인합니다. PAMSRV에서 관리자로 웹 브라우저를 엽니다.  웹 사이트 URL http://pamsrv.priv.contoso.local:8086/api/pamresources/pamroles/로 이동하여 필요한 경우, 인증하고 다운로드가 수행되는지 확인합니다.

## <a name="install-the-mim-pam-requestor-cmdlets"></a>MIM PAM 요청자 cmdlet 설치

1단계에서 구성된 워크스테이션에 MIM PAM 요청자 cmdlet을 설치합니다.

1.  CORPWKSTN에 관리자로 로그인합니다.

2.  CORPWKSTN 컴퓨터에 **추가 기능 및 확장** 이 아직 없는 경우 다운로드합니다.

3.  **추가 기능 및 확장** 폴더의 보관 압축을 새 폴더에 풉니다.

4.  설치 관리자 **setup.exe**를 실행합니다.

5.  사용자 지정 설정에서 **PAM 클라이언트**를 설치하도록 지정하지만 **Outlook용 MIM 추가 기능** 또는 **MIM 암호 및 인증 확장 프로그램**은 설치하도록 지정하지 않습니다.

6.  PAM 서버 주소에서 PRIV MIM 서버의 호스트 이름을 *pamsrv.priv.contoso.local*로 지정합니다.

설치를 완료한 후 CORPWKSTN을 다시 시작하여 새 PowerShell 모듈 등록을 완료합니다.

다음 단계에서 PRIV 및 CORP 포리스트 간에 트러스트를 설정합니다.

> [!div class="step-by-step"]
> [« 3단계](step-3-prepare-pam-server.md)
> [5단계 »](step-5-establish-trust-between-priv-corp-forests.md)
