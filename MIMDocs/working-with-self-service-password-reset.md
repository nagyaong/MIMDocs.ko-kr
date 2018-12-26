---
title: 셀프 서비스 암호 재설정 사용 | Microsoft Docs
description: SSPR이 다단계 인증과 함께 작동하는 방식을 포함하여 MIM 2016 셀프 서비스 암호 재설정의 새로운 기능을 확인합니다.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/30/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 3a86569a8de77f4cf4d5aeafe0cd01dab40232b3
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358469"
---
# <a name="self-service-password-reset-deployment-options"></a>셀프 서비스 암호 재설정 배포 옵션

[Azure Active Directory Premium](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-licensing)에 대한 라이선스가 있는 신규 고객의 경우 [Azure AD 셀프 서비스 암호 재설정](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks.md)을 사용하여 최종 사용자 환경을 제공하는 것이 좋습니다.  Azure AD 셀프 서비스 암호 재설정은 사용자가 자신의 암호를 재설정할 수 있도록 웹 기반 및 Windows 통합 환경을 제공하며 대체 이메일 및 Q&A 게이트와 같은 MIM과 동일한 기능을 대부분 지원합니다.  Azure AD 셀프 서비스 암호 재설정을 배포할 때 Azure AD Connect는 [새 암호를 AD DS에 다시 쓰는 것](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-writeback.md)을 지원하고 MIM [암호 변경 알림 서비스](deploying-mim-password-change-notification-service-on-domain-controller.md)를 사용하여 암호를 다른 시스템(예: 다른 공급업체의 디렉터리 서버)에도 전달할 수 있습니다.  [암호 관리](infrastructure/mim2016-password-management.md)를 위한 MIM 배포를 위해서 MIM 서비스 또는 MIM 셀프 서비스 암호 재설정이 필요하지 않으며 등록 포털을 배포하지 않아도 됩니다.  대신, 다음 단계를 수행하세요.

- 먼저, Azure AD 및 AD DS가 아닌 다른 디렉터리에 암호를 보내야 하는 경우 Active Directory Domain Services 및 추가 대상 시스템에 대한 커넥터가 있는 MIM 동기화를 배포하고 [암호 관리](infrastructure/mim2016-password-management.md)를 위한 MIM을 구성한 다음, [암호 변경 알림 서비스](deploying-mim-password-change-notification-service-on-domain-controller.md)를 배포합니다.
- 그런 다음, Azure AD가 아닌 디렉터리에 암호를 보내야 하는 경우 [새 암호를 AD DS에 다시 쓰기](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-writeback.md) 위한 Azure AD Connect를 구성합니다.
- 필요에 따라 [사용자를 미리 등록](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-authenticationdata.md)합니다.
- 마지막으로, [Azure AD 셀프 서비스 암호 재설정을 최종 사용자에게 롤아웃](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-deployment.md)합니다.

이전에 셀프 서비스 암호 재설정을 위해 FIM(Forefront Identity Manager)을 배포했고 Azure Active Directory Premium에 대한 라이선스가 있는 기존 고객이라면  Azure AD 셀프 서비스 암호 재설정으로 전환을 계획하는 것이 좋습니다.  [사용자의 보조 이메일 또는 휴대폰 번호를 PowerShell을 통해 동기화하거나 설정](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-authenticationdata.md)하여 최종 사용자를 다시 등록할 필요 없이 Azure AD 셀프 서비스 암호 재설정으로 전환할 수 있습니다. 사용자가 Azure AD 셀프 서비스 암호 재설정에 등록되면 FIM 암호 재설정 포털을 서비스 해제할 수 있습니다.

해당 사용자에 대해 Azure AD 셀프 서비스 암호 재설정을 아직 배포하지 않은 고객의 경우 MIM은 셀프 서비스 암호 재설정 포털도 제공합니다.  FIM과는 달리, MIM 2016은 다음과 같이 변경되었습니다.

- 사용자는 MIM 셀프 서비스 암호 재설정 포털 및 Windows 로그인 화면에서 암호를 변경하지 않고도 계정의 잠금을 해제할 수 있습니다.
- 새 인증 게이트인 전화 게이트가 MIM에 추가되었습니다. 따라서 MFA(Microsoft Azure Multi-Factor Authentication) 서비스를 통해 전화를 통한 사용자 인증을 수행할 수 있습니다.

MIM 2016 릴리스 빌드 버전 4.5.26.0까지는 고객이 Azure MFA SDK(Azure Multi-Factor Authentication 소프트웨어 개발 키트)를 다운로드해야 합니다.  해당 SDK는 더 이상 사용되지 않으며 Azure MFA SDK는 기존 고객을 위해 사용 중지 날짜인 2018년 11월 14일까지 지원됩니다. 그때까지 고객은 Azure MFA SDK를 다운로드할 수 없으므로 생성된 MFA 서비스 자격 증명 패키지를 받으려면 Azure 고객 지원팀에 문의해야 합니다. 

#### <a name="new-update-current-azure-mfa-configuration-to-azure-multi-factor-authentication-server"></a>새 기능! 현재 Azure MFA 구성을 Azure Multi-Factor Authentication 서버로 업데이트

이 [문서](working-with-mfaserver-for-mim.md)에서는 다단계 인증에 Azure Multi-Factor Authentication 서버를 사용하여 배포 MIM 셀프 서비스 암호 재설정 포털 및 PAM 구성을 업데이트하는 방법에 대해 설명합니다.

## <a name="deploying-mim-self-service-password-reset-portal-using-azure-mfa-for-multi-factor-authentication"></a>Multi-Factor Authentication에 Azure MFA를 사용하여 MIM 셀프 서비스 암호 재설정 포털 배포

다음 섹션에서는 다단계 인증에 Azure MFA를 사용하여 MIM 셀프 서비스 암호 재설정 포털을 배포하는 방법에 대해 설명합니다.  이러한 단계는 사용자에게 Azure AD 셀프 서비스 암호 재설정을 사용하지 않는 고객에게만 필요합니다.

Microsoft Azure Multi-Factor Authentication은 사용자가 모바일 앱, 전화 통화 또는 문자 메시지를 사용하여 로그인 시도를 확인해야 하는 인증 서비스입니다. Microsoft Azure Active Directory와 함께 사용할 수 있으며 클라우드 및 온-프레미스 엔터프라이즈 애플리케이션을 위한 서비스로 사용할 수 있습니다.

Azure MFA는 셀프 서비스 로그인 지원을 위해 MIM에서 수행하는 메커니즘처럼, 기존 인증 프로세스를 강화할 수 있는 추가 인증 메커니즘을 제공합니다.

Azure MFA를 사용하는 경우 사용자가 해당 계정 및 리소스에 대한 액세스 권한을 다시 얻으려고 시도할 때 해당 ID를 확인하기 위해 시스템을 인증합니다. SMS 또는 전화 통화를 통해 인증할 수 있습니다.   인증이 강력할수록 액세스 권한을 얻으려는 사용자가 ID를 소유하는 실제 사용자일 신뢰성도 높아집니다. 인증되고 나면 사용자는 새 암호를 선택하여 이전 암호를 바꿀 수 있습니다.

## <a name="prerequisites-to-set-up-self-service-account-unlock-and-password-reset-using-mfa"></a>MFA를 사용하여 셀프 서비스 계정 잠금 해제 및 암호 재설정을 설정하기 위한 필수 구성 요소

이 섹션에서는 다음 구성 요소 및 서비스를 포함하여 Microsoft Identity Manager 2016 [MIM 동기화, MIM 서비스 및 MIM 포털 구성 요소](microsoft-identity-manager-deploy.md)를 다운로드하고 배포를 완료했다고 가정합니다.

-   Windows Server 2008 R2 이상이 지정된 도메인(“corporate”도메인)과 함께 AD 도메인 서비스 및 도메인 컨트롤러를 포함하는 Active Directory 서버로 설정되었습니다.

-   그룹 정책이 계정 잠금에 대해 정의되었습니다.

-   MIM 2016 동기화 서비스(동기화)가 AD 도메인에 도메인 가입된 서버에 설치되고 실행됩니다.

-   SSPR 등록 포털 및 SSPR 재설정 포털을 포함하는 MIM 2016 서비스 &amp; 포털이 서버에 설치되고 실행됩니다(동기화와 함께 배치 가능).

-   다음을 비롯한 MIM 동기화가 AD-FIM ID 동기화에 대해 구성되어 있습니다.

    -   AD DS에 연결하고 ID 데이터를 Active Directory에서 가져오고 Active Directory에 내보낼 수 있도록 ADMA(Active Directory 관리 에이전트) 구성

    -   FIM 서비스 DB에 연결하고 ID 데이터를 FIM 데이터베이스에서 가져오고 FIM 데이터베이스에 내보낼 수 있도록 MIM MA(MIM 관리 에이전트) 구성

    -   MIM 서비스에서 사용자 데이터 동기화를 허용하고 동기화 기반 작업이 용이하도록 MIM 포털에 동기화 규칙 구성

-   SSPR Windows 로그인 통합 클라이언트를 포함하는 MIM 2016 추가 기능 &amp; 확장이 서버 또는 별도 클라이언트 컴퓨터에 배포됩니다.

이 시나리오에서는 사용자에 대한 MIM CAL과 Azure MFA에 대한 구독이 있어야 합니다.

## <a name="prepare-mim-to-work-with-multi-factor-authentication"></a>다단계 인증과 함께 작동하도록 MIM 준비
암호 재설정 및 계정 잠금 해제 기능을 지원하도록 MIM 동기화를 구성합니다. 자세한 내용은 [FIM 추가 기능 및 확장 설치](https://technet.microsoft.com/library/ff512688%28v=ws.10%29.aspx), [FIM SSPR 설치](https://technet.microsoft.com/library/hh322891%28v=ws.10%29.aspx), [SSPR 인증 게이트](https://technet.microsoft.com/library/jj134288%28v=ws.10%29.aspx) 및 [SSPR 테스트 랩 가이드](https://technet.microsoft.com/library/hh826057%28v=ws.10%29.aspx)를 참조하세요.

다음 섹션에서는 Microsoft Azure Active Directory에 Azure MFA 공급자를 설정합니다. 이 작업 중에는 Azure MFA에 연결하기 위해 MFA에서 필요로 하는 인증 자료를 포함하는 파일을 생성합니다.  계속 진행하려면 Azure 구독이 필요합니다.

### <a name="register-your-multi-factor-authentication-provider-in-azure"></a>Azure에서 다단계 인증 공급자 등록

1.  [MFA 공급자](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider.md)를 만듭니다.

2. 지원 사례를 열고 ASP.net 2.0 C#에 대한 직접 SDK를 요청합니다. 직접 SDK는 더 이상 사용되지 않으므로 SDK는 MFA를 통해 MIM의 현재 사용자에게만 제공됩니다. 새 고객은 MFA 서버와 통합될 MIM의 다음 버전을 채택해야 합니다.

3. 결과 ZIP 파일을 MIM 서비스가 설치되는 각 시스템에 복사합니다.  이 ZIP 파일에는 Azure MFA 서비스에 인증하는 데 사용되는 키 관련 자료가 들어있습니다.

### <a name="update-the-configuration-file"></a>구성 파일 업데이트

1. MIM 서비스가 설치된 컴퓨터에 MIM을 설치한 사용자로 로그인합니다.

2. MIM 서비스가 설치된 디렉터리 아래에 새 디렉터리를 만듭니다(예: **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts**).

3. Windows 탐색기를 사용하여 이전 섹션에서 다운로드한 ZIP 파일의 **\pf\certs** 폴더로 이동하고 **cert_key.p12** 파일을 새 디렉터리에 복사합니다.

4.  SDK zip 파일의 **\pf** 폴더에서 **pf_auth.cs** 파일을 엽니다.

5.  다음 세 매개 변수를 찾습니다. `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD`.

    ![pf_auth.cs 코드 이미지](media/MIM-SSPR-pFile.png)

6.  **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service**에서 **MfaSettings**.xml 파일을 엽니다.

7.  pf_aut.cs 파일의 `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD` 매개 변수 값을 MfaSettings.xml 파일의 해당 xml 요소에 복사합니다.

8.  SDK zip 파일에서 \pf\certs 아래에 **cert_key.p12** 파일을 추출하고 이 파일에 대한 전체 경로를 MfaSettings.xml 파일의 `<CertFilePath>` xml 요소에 입력합니다.

9. `<username>` 요소에 사용자 이름을 입력합니다.

10. `<DefaultCountryCode>` 요소에 기본 국가 코드를 입력합니다. 국가 코드가 없는 사용자에 대해 전화 번호가 등록된 경우 이 국가 코드를 받게 됩니다. 사용자가 국제 국가 코드를 사용하는 경우 등록된 전화 번호에 이 코드가 포함되어야 합니다.

11. MfaSettings.xml 파일을 동일한 위치에 동일한 이름으로 저장합니다.

#### <a name="configure-the-phone-gate-or-the-one-time-password-sms-gate"></a>전화 게이트 또는 일회용 암호 SMS 게이트 구성

1.  Internet Explorer를 시작하고 MIM 포털로 이동하여 MIM 관리자로 인증한 다음, 왼쪽 탐색 모음에서  **워크플로** 를 클릭합니다.

    ![MIM 포털 탐색 이미지](media/MIM-SSPR-workflow.jpg)

2.  **암호 다시 설정 인증 워크플로**를 선택합니다.

    ![MIM 포털 워크플로 이미지](media/MIM-SSPR-PwdResetAuthNworkflow.jpg)

3.  **활동** 탭을 클릭하고 **활동 추가**까지 아래로 스크롤합니다.

4.  **전화 게이트** 또는 **일회용 암호 SMS 게이트**를 선택하고 **선택** 및 **확인**을 차례로 클릭합니다.

이제 조직의 사용자가 암호 재설정을 위해 등록할 수 있습니다.  이 과정에서 사용자는 회사 전화 번호나 휴대폰 번호를 입력하게 되며, 이를 통해 시스템은 사용자에게 전화할(또는 SMS 메시지 보내기) 방법을 파악할 수 있습니다.

#### <a name="register-users-for-password-reset"></a>암호 재설정을 위한 사용자 등록

1.  사용자는 웹 브라우저를 시작하고 MIM 암호 재설정 등록 포털로 이동합니다.  일반적으로 이 포털은 Windows 인증으로 구성됩니다.  포털 내에서 사용자의 사용자 이름과 암호를 다시 제공하여 본인의 ID를 확인합니다.

    암호 등록 포털을 시작하고 해당 사용자 이름과 암호를 사용하여 인증해야 합니다.

2.  **전화 번호** 또는 **휴대폰**  필드에 국가 코드, 공백, 전화번호를 입력하고 **다음**을 클릭해야 합니다.

    ![MIM 전화 확인 이미지](media/MIM-SSPR-PhoneVerification.JPG)

    ![MIM 휴대폰 확인 이미지](media/MIM-SSPR-mobilephoneverification.JPG)

## <a name="how-does-it-work-for-your-users"></a>사용자를 위해 이 기능이 어떻게 작동하나요?
이제 모든 항목이 구성되고 실행되고 있으므로 휴가 직전에 암호를 바꾼 사용자가 휴가에서 돌아와 암호를 기억하지 못하는 경우 어떤 과정을 거쳐 암호를 재설정할 수 있는지 알아보겠습니다.

사용자는 Windows 로그인 화면이나 셀프 서비스 포털 중 한 가지 방법으로 암호 재설정 및 계정 잠금 해제 기능을 사용할 수 있습니다.

사용자는 조직 네트워크를 통해 MIM 서비스에 연결된 도메인 가입 컴퓨터에 MIM 추가 기능 및 확장을 설치하여 데스크톱 로그인 환경에서 잊은 암호를 복구할 수 있습니다.  다음 단계는 이 프로세스에 관한 것입니다.

#### <a name="windows-desktop-login-integrated-password-reset"></a>Windows 데스크톱 로그인과 암호 재설정의 통합

1.  사용자가 잘못된 암호를 여러 번 입력하는 경우, 로그인 화면에 **Problems logging in?** (로그인에 문제가 있나요?)를 클릭하는 옵션이 표시됩니다. .

    ![로그인 화면 이미지](media/MIM-SSPR-problemsloggingin.JPG)

    이 링크를 클릭하면 사용자가 암호를 변경하거나 계정의 잠금을 해제할 수 있는 MIM 암호 재설정 화면으로 이동합니다.

    ![MIM 암호 재설정 이미지](media/MIM-SSPR-keepcurrentorsetnewpwd.JPG)

2.  인증이 진행됩니다. MFA가 구성된 경우 사용자는 전화를 받게 됩니다.

3.  Azure MFA에서 사용자가 서비스에 등록할 때 입력한 전화 번호로 전화를 거는 작업이 백그라운드에서 이루어집니다.

4.  사용자가 전화를 받으면 우물정자(#)를 누르라는 안내가 나옵니다. 그런 다음 사용자가 포털에서 **다음** 을 클릭합니다.

    다른 게이트도 설정한 경우 다음 화면에 추가 정보를 제공하라는 메시지가 표시됩니다.

    > [!NOTE]
    > 우물정자(#)를 누르기 전에 성급하게 **다음**을 클릭하면 인증에 실패합니다.

5.  인증이 성공하면 사용자에게 두 가지 옵션이 제공됩니다. 계정의 잠금을 해제하고 현재 암호를 유지하거나 새 암호를 설정할 수 있습니다.

6.  이때 사용자는 새 암호를 두 번 입력해야 하고 그런 다음 암호가 재설정됩니다.

#### <a name="access-from-the-self-service-portal"></a>셀프 서비스 포털에서 액세스

1.  사용자가 웹 브라우저를 열고, **암호 재설정 포털** 로 이동하고, 사용자 이름을 입력한 후, **다음**을 클릭합니다.

    MFA가 구성된 경우 사용자는 전화를 받게 됩니다. Azure MFA에서 사용자가 서비스에 등록할 때 입력한 전화 번호로 전화를 거는 작업이 백그라운드에서 이루어집니다.

    사용자가 전화를 받으면 우물정자(#)를 누르라는 안내가 나옵니다. 그런 다음 사용자가 포털에서 **다음** 을 클릭합니다.

2.  다른 게이트도 설정한 경우 다음 화면에 추가 정보를 제공하라는 메시지가 표시됩니다.

    > [!NOTE]
    > 우물정자(#)를 누르기 전에 성급하게 **다음**을 클릭하면 인증에 실패합니다.

3.  사용자는 암호를 재설정하는 방법과 계정의 잠금을 해제하는 방법 중에서 선택해야 합니다. 계정의 잠금을 해제하기로 선택하면 계정이 잠금 해제됩니다.

    ![MIM 로그인 도우미 계정 잠금 해제 이미지](media/MIM-SSPR-accountUnlock.JPG)

4.  인증이 성공하면 사용자에게 두 가지 옵션이 제공됩니다. 현재 암호를 유지하거나 새 암호를 설정하는 것입니다.

5.  ![MIM 계정
6.  잠금 해제 성공 이미지](media/MIM-SSPR-account-unlock.JPG)

6.  사용자가 암호를 다시 설정하기 위해서는 새 암호를 두 번 입력하고 **다음** 을 클릭하여 암호를 변경하면 됩니다.
