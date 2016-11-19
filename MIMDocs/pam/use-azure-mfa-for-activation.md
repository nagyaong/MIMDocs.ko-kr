---
title: "Azure MFA를 사용하여 PAM 활성화 | Microsoft 문서"
description: "사용자가 Privileged Access Management에서 역할을 활성화할 때 Azure MFA를 제2의 보안 계층으로 설정합니다."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5134a112-f73f-41d0-a5a5-a89f285e1f73
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: fa6d69038e5b2f0b933773381661929159198242


---

# <a name="using-azure-mfa-for-activation"></a>활성화에 Azure MFA 사용
PAM 역할을 구성할 때 역할 활성화를 요청하는 사용자에게 권한을 부여하는 방법을 선택할 수 있습니다. PAM 권한 부여 활동을 구현하는 선택 사항:

- 역할 소유자 승인
- MFA(Multi-Factor Authentication)

검사를 전혀 사용하지 않는 경우 후보 사용자가 자신의 역할에 대해 자동으로 활성화됩니다.

Microsoft Azure MFA(Multi-Factor Authentication)는 사용자가 모바일 앱, 전화 통화 또는 문자 메시지를 사용하여 로그인 시도를 확인해야 하는 인증 서비스입니다. Microsoft Azure Active Directory와 함께 사용할 수 있으며 클라우드 및 온-프레미스 엔터프라이즈 응용 프로그램을 위한 서비스로 사용할 수 있습니다. PAM 시나리오의 경우, Azure MFA에서는 이전에 후보 사용자가 Windows PRIV 인증에 사용한 방법에 관계 없이 권한 부여에 사용할 수 있는 추가 인증 메커니즘을 제공합니다.

## <a name="prerequisites"></a>필수 구성 요소

Azure MFA를 MIM과 함께 사용하려면 다음이 필요합니다.

- Azure MFA 서비스 연결에 사용되는, PAM을 제공하는 각 MIM 서비스에서의 인터넷 액세스
- Azure 구독
- 후보 사용자의 Azure Active Directory Premium 라이선스 또는 Azure MFA 라이선스 방법
- 모든 후보 사용자의 전화 번호

## <a name="creating-an-azure-mfa-provider"></a>Azure MFA 공급자 만들기

이 섹션에서는 Microsoft Azure Active Directory에 Azure MFA 공급자를 설정합니다.  Azure MFA를 독립 실행형 또는 Azure Active Directory Premium에 구성된 상태로 이미 사용하고 있는 경우에는 다음 섹션을 건너뜁니다.

1.  웹 브라우저를 열고 Azure 구독 관리자로 [Azure 클래식 포털](https://manage.windowsazure.com)에 연결합니다.

2.  왼쪽 아래 모서리에서 **새로 만들기**를 클릭합니다.

3.  **앱 서비스 > Active Directory > 다단계 인증 공급자 > 빨리 만들기**를 클릭합니다.

4.  **이름** 필드에 **PAM**을 입력하고 사용 모델 필드에서 활성화된 사용자별을 선택합니다. Azure AD 디렉터리가 이미 있는 경우에는 해당 디렉터리를 선택 합니다. 마지막으로 **만들기**를 클릭합니다.

## <a name="downloading-the-azure-mfa-service-credentials"></a>Azure MFA 서비스 자격 증명 다운로드

다음으로, Azure MFA에 연결하기 위해 PAM에 대한 인증 자료가 포함된 파일을 생성합니다.

1. 웹 브라우저를 열고 Azure 구독 관리자로 [Azure 클래식 포털](https://manage.windowsazure.com)에 연결합니다.

2.  Azure Portal 메뉴에서 **Active Directory**를 클릭한 다음 **Multi-Factor Authentication 공급자** 탭을 클릭합니다.

3.  PAM에 사용할 Azure MFA 공급자를 클릭한 다음 **관리**를 클릭합니다.

4.  새 창에서 왼쪽 패널의 **구성**아래에서 **설정**을 클릭합니다.

5.  **Azure Multi-Factor Authentication** 창에서 **다운로드** 아래에 있는 **SDK**를 클릭합니다.

6.  언어가 **SDK for ASP.net 2.0 C\#**인 파일의 경우 ZIP 열에서 **다운로드** 링크를 클릭합니다.

![Multi-Factor Authentication SDK 다운로드 - 스크린샷](media/PAM-Azure-MFA-Activation-Image-1.png)

7.  결과 ZIP 파일을 MIM 서비스가 설치되는 각 시스템에 복사합니다. 

>[!NOTE]
> 이 ZIP 파일에는 Azure MFA 서비스에 인증하는 데 사용되는 키 관련 자료가 들어있습니다.

## <a name="configuring-the-mim-service-for-azure-mfa"></a>Azure MFA용 MIM 서비스 구성

1.  MIM 서비스가 설치된 컴퓨터에 관리자나 MIM을 설치한 사용자로 로그인합니다.

2.  MIM 서비스가 설치된 디렉터리 아래에 새 디렉터리를 만듭니다(예: `C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service\\MfaCerts`).

3.  Windows 탐색기를 사용하여 이전 섹션에서 다운로드한 ZIP 파일의 **pf\\certs** 폴더로 이동하고 **cert\_key.p12** 파일을 새 디렉터리에 복사합니다.

4.  Windows 탐색기를 사용하여 ZIP 파일의 **pf** 폴더로 이동하고 워드패드와 같은 텍스트 편집기에서 **pf\_auth.cs** 파일을 엽니다.

5.  **LICENSE\_KEY**, **GROUP\_KEY**, **CERT\_PASSWORD**의 세 매개 변수를 찾습니다.

![pf\_auth.cs 파일에서 값 복사 - 스크린샷](media/PAM-Azure-MFA-Activation-Image-2.png)

6.  메모장을 사용하여 `C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service`에 있는 **MfaSettings.xml**을 엽니다.

7.  pf\_auth.cs 파일의 LICENSE\_KEY, GROUP\_KEY, CERT\_PASSWORD 매개 변수 값을 MfaSettings.xml 파일의 해당 xml 요소에 복사합니다.

8.  **<CertFilePath>** XML 요소에서 이전에 추출한 cert\_key.p12 파일의 전체 경로 이름을 지정합니다.

9.  **<username>** 요소에 사용자 이름을 입력합니다.

10.  **<DefaultCountryCode>** 요소에 사용자에게 전화를 거는 데 사용할 국가 코드(예: 미국 및 캐나다는 1)를 입력합니다. 이 값은 사용자가 국가 코드가 없는 전화 번호로 등록한 경우에 사용됩니다. 사용자의 전화 번호에 조직에 대해 구성된 것과 다른 국제 국가 코드가 있는 경우에는 등록할 전화 번호에 해당 국가 코드를 포함해야 합니다.

11.  MIM 서비스 폴더 `C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service`에 **MfaSettings.xml** 파일을 저장하고 덮어씁니다. 

> [!NOTE]
> 프로세스를 종료할 때 **MfaSettings.xml** 파일이나 복사본 또는 ZIP 파일을 공개적으로 읽을 수 없는지 확인해야 합니다.

## <a name="configure-pam-users-for-azure-mfa"></a>Azure MFA에 대해 PAM 사용자 구성

사용자가 Azure MFA가 필요한 역할을 활성화하려면 MIM에 사용자의 전화 번호를 저장해야 합니다. 이 특성을 설정하는 방법에는 두 가지가 있습니다.

첫 번째는 `New-PAMUser` 명령으로 CORP 도메인에 있는 사용자의 디렉터리 항목에서 MIM 서비스 데이터베이스로 전화 번호 특성을 복사하는 것입니다. 이 작업은 일회성입니다.

두 번째는 `Set-PAMUser` 명령으로 MIM 서비스 데이터베이스의 전화 번호 특성을 업데이트하는 것입니다. 예를 들어, 다음은 MIM 서비스에서 기존 PAM 사용자의 전화 번호를 바꿉니다. 해당 디렉터리 항목은 변경되지 않습니다.

```
Set-PAMUser (Get-PAMUser -SourceDisplayName Jen) -SourcePhoneNumber 12135551212
```


## <a name="configure-pam-roles-for-azure-mfa"></a>Azure MFA에 대해 PAM 역할 구성

PAM 역할의 모든 후보 사용자가 MIM 서비스 데이터베이스에 전화 번호를 저장하고 나면 Azure MFA를 사용하도록 역할을 구성할 수 있습니다. `New-PAMRole` 또는 `Set-PAMRole` 명령을 사용하여 이 작업을 수행합니다. 예:

```
Set-PAMRole (Get-PAMRole -DisplayName "R") -MFAEnabled 1
```

`Set-PAMRole` 명령에 "-MFAEnabled 0" 매개 변수를 지정하면 역할에 대해 Azure MFA를 사용하지 않도록 설정할 수 있습니다.

## <a name="troubleshooting"></a>문제 해결

권한 있는 액세스 관리 이벤트 로그에서 다음 이벤트를 찾을 수 있습니다.

| ID  | 심각도 | 생성 | 설명 |
|-----|----------|--------------|-------------|
| 101 | 오류       | MIM 서비스            | 사용자가 Azure MFA를 완료하지 않음(예: 전화에 응답하지 않음) |
| 103 | 정보 산업 | MIM 서비스            | 사용자가 활성화 과정에서 Azure MFA 완료                       |
| 825 | 경고     | PAM 모니터링 서비스 | 전화 번호가 변경됨                                |

전화 통화 실패(이벤트 101)에 대한 자세한 내용을 보려는 경우 Azure MFA에서 보고서를 보거나 다운로드할 수도 있습니다.

1.  웹 브라우저를 열고 Azure AD 전역 관리자로 [Azure 클래식 포털](https://manage.windowsazure.com)에 연결합니다.

2.  Azure 포털 메뉴에서 **Active Directory**를 선택한 다음 **다단계 인증 공급자** 탭을 선택합니다.

3.  PAM에 사용하는 Azure MFA 공급자를 선택한 다음 **관리**를 클릭합니다.

4.  새 창에서 **사용**을 클릭한 다음 **사용자 세부 정보**를 클릭합니다.

5.  시간 범위를 선택하고 추가 보고서 열에서 **이름** 옆의 확인란을 선택합니다. **CSV로 내보내기**를 클릭합니다.

6.  보고서가 생성되면 포털에서 볼 수 있으며, MFA 보고서가 큰 경우에는 CSV 파일로 다운로드할 수 있습니다. **AUTH TYPE** 열의 **SDK** 값은 PAM 활성화 요청과 관련된 행으로, MIM 또는 다른 온-프레미스 소프트웨어에서 시작되는 이벤트를 나타냅니다. **USERNAME** 필드는 MIM 서비스 데이터베이스의 사용자 개체 GUID입니다. 호출에 성공하지 못할 경우 **AUTHD** 열의 값은 **No**가 되고, **CALL RESULT** 열의 값에는 실패의 이유에 대한 세부 정보가 포함됩니다.



<!--HONumber=Nov16_HO2-->


