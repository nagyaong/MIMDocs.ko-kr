---
# required metadata

title: MIM 2016 설치&#58; MIM 서비스 및 포털 | Microsoft Identity Manager
description: Microsoft Identity Manager 2016용 MIM 서비스 및 포털을 구성하고 설치하는 단계를 알아봅니다.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# MIM 2016 설치: MIM 서비스 및 포털

>[!div class="step-by-step"]
[« MIM 동기화 서비스](install-mim-sync.md)
[데이터베이스 동기화 »](install-mim-sync-ad-service.md)

> [!NOTE]
> 아래의 모든 예제에서 **mimservername**은 도메인 컨트롤러의 이름을 나타내고 **contoso**는 도메인 이름을 나타내며 **Pass@word1**은 예제 암호를 나타냅니다.

마지막 단계에서 MIM 설치 패키지를 설치하지 않은 경우 다음 단계에 따라 Microsoft Identity Manager 2016 구성 요소를 설치합니다.

1. ID 관리에 사용중인 CORPIDM 서버에 *contoso\Administrator*로 로그인합니다.

2. MIM 설치 패키지의 압축을 풀거나 MIM 이미지 DVD를 마운트합니다.

## 설치를 위한 MIM 서비스 및 포털 구성

1.  압축을 푼 **서비스 및 포털** 하위 폴더에서 **MIM 서비스 및 포털 설치 관리자**를 실행합니다.

2.  시작 화면에서 **다음**을 클릭합니다.

3.  최종 사용자 사용권 계약을 읽고 사용 조건에 동의하면 **다음**을 클릭합니다.

4.  **MIM 사용자 환경 개선 프로그램** 화면에서 **다음**을 클릭합니다.

5.  이 배포에 대한 구성 요소 기능을 선택할 때 MIM 서비스(MIM 보고 제외) 및 MIM 포털 기능을 포함해야 합니다. 또한 MIM 암호 재설정 포털 및 MIM 암호 변경 알림 서비스를 선택할 수 있습니다.

6.  **MIM 데이터베이스 연결 구성** 페이지에서 **새 데이터베이스 만들기**를 지정합니다.

    ![MIM 데이터베이스 연결 구성 이미지](media/MIM-Install10.png)

7.  **메일 서버 연결 구성**에서 Exchange Server 이름으로 **Mail Server**를 입력합니다. 구성된 메일 서버가 없는 경우 localhost를 메일 서버 이름으로 지정하고 상위 두 확인란을 선택 취소합니다.  **다음**을 클릭합니다.

    ![메일 서버 연결 구성 이미지](media/MIM-Install11.png)

8.  새 자체 서명된 인증서를 생성하도록 지정하거나 관련 인증서를 선택합니다.

9. 사용할 서비스 계정 이름(예: *MIMService*), 서비스 계정 암호(예: *Pass@word1*), 서비스 계정 도메인(예: *contoso*), 서비스 메일 계정(예: *contoso*)을 지정합니다.

    ![서비스 계정 구성 이미지](media/MIM-Install12.png)

10. 현재 구성에서 서비스 계정이 안전하지 않다는 경고가 나타날 수 있습니다.

11. 동기화 서버 위치에 대한 기본값을 적용하고 MIM 관리 에이전트 계정을 *contoso\MIMsync*로 지정합니다.

    ![MIM 서비스 및 포털 구성 이미지](media/MIM-Install13.png)

12. MIM 포털에 대한 MIM 서비스 서버 주소를 *CORPIDM*(이 컴퓨터의 이름)으로 지정합니다.

13. SharePoint 사이트 모음 URL을 *http://CorpIDM.contoso.local:82*로 지정합니다.

14. 암호 등록 URL을 *http://CorpIDM.contoso.local:8080*으로 지정합니다.

15. 암호 재설정 URL을 *http://CorpIDM.contoso.local:8088*로 지정합니다.

16. 방화벽에서 포트 5725 및 5726 열기에 대한 확인란 및 MIM 포털에 대해 모든 인증된 사용자 액세스 권한 부여에 대한 확인란을 선택합니다.

## MIM 암호 등록 포털 구성

1.  SSPR 등록을 위한 서비스 계정 이름을 *contoso\MIMSSPR*로 설정하고 암호를 *Pass@word1*로 설정합니다.

2.  MIM 암호 등록에 대한 호스트 이름을 *CORPIDM*으로 지정하고 포트를 **8080**으로 설정합니다. **방화벽에서 포트 열기** 옵션을 선택합니다.

    ![IIS에서 사용하는 구성 정보 입력 이미지](media/MIM-Install14.png)

3.  경고가 표시되면 읽고 **다음**을 클릭합니다.

4. 다음 MIM 암호 등록 포털 구성 화면에서 암호 등록 포털에 대한 MIM 서비스 서버 주소로 *http://CorpIDM.contoso.local*을 지정합니다.

## FIM 암호 재설정 포털 구성

1.  SSPR 등록을 위한 서비스 계정 이름을 *Contoso\MIMSSPRService*로 설정하고 암호를 *Pass@word1*로 설정합니다.

2.  MIM 암호 등록에 대한 호스트 이름을 *CORPIDM*으로 지정하고 포트를 **8080**으로 설정합니다. **방화벽에서 포트 열기** 옵션을 선택합니다.

    ![IIS에서 사용하는 구성 정보 입력 이미지](media/MIM-Install15.png)

3.  경고가 표시되면 읽고 **다음**을 클릭합니다.

4. 다음 MIM 암호 등록 포털 구성 화면에서 암호 등록 포털에 대한 MIM 서비스 서버 주소로 *CorpIDname http://CorpIDname.domain.local*을 지정합니다.

## MIM 서비스 및 포털 설치

사전 설치 정의가 모두 준비되면 **설치**를 클릭하여 선택한 **서비스 및 포털** 구성 요소 설치를 시작합니다.

![MIM 서비스 및 포털 설치 이미지](media/MIM-Install16.png)

설치가 완료된 후 MIM 포털이 활성화되었는지 확인합니다.

1. Internet Explorer를 시작하고 *http://corpidm.contoso.local:82/identitymanagement*에서 MIM 포털에 연결합니다. 이 페이지를 처음 방문할 때 약간 시간이 걸릴 수 있습니다.

    - 필요한 경우 *contoso\Administrator*로 Internet Explorer에 인증합니다.

2. Internet Explorer에서 **인터넷 옵션**을 열고 **보안** 탭으로 변경하고 **로컬 인트라넷** 영역에 해당 사이트가 아직 없는 경우 추가합니다.  **인터넷 옵션** 대화 상자를 닫습니다.

3. 사용자가 MIM에서 자신의 항목을 볼 수 있도록 설정합니다.

    1.  Internet Explorer를 사용하여 **MIM 포털**에서 **관리 정책 규칙**을 클릭합니다.

    2.  관리 정책 규칙 **사용자 관리: 사용자가 자체 특성을 읽을 수 있음**을 검색합니다.

    3.  이 관리 정책 규칙을 선택하고 **정책 사용 안 함**을 선택 취소합니다.

    4.  **확인** 을 클릭한 다음 **제출**을 클릭합니다.

4.  방화벽이 TCP 포트 5725 및 5726에 대한 수신 연결을 허용하는지 확인합니다.

    1.  **고급 보안**이 포함된 **관리 도구 » Windows 방화벽**을 시작합니다.

    2.  **인바운드 규칙**을 클릭합니다.

    3.  다음 두 규칙이 표시되는지 확인합니다.

        -   Forefront Identity Manager 서비스(STS)

        -   Forefront Identity Manager 서비스(웹 서비스)

    4.  마법사를 완료하고 **Windows 방화벽** 응용 프로그램을 닫습니다.

    5.  **제어판 » 네트워크 및 인터넷 » 네트워크 상태 및 작업 보기**를 시작합니다.

    6.  contoso.local이 도메인 네트워크로 나열된 활성 네트워크가 있는지 확인합니다.

    7.  **제어판**을 닫습니다.

> [!NOTE]
> 선택 사항: 이제 MIM 추가 기능 및 확장을 설치할 수 있습니다.

>[!div class="step-by-step"]  
[« MIM 동기화 서비스](install-mim-sync.md)
[데이터베이스 동기화 »](install-mim-sync-ad-service.md)


<!--HONumber=Apr16_HO2-->

