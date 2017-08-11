---
title: "Microsoft Identity Manager 2016 포털 사용자 지정 | Microsoft Docs"
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: bebb299ece077a6ab2e0d75f3b30ad1776678303
ms.sourcegitcommit: 5ba5d916c0ca1e5aa501592af0cef714bfdc8afe
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/02/2017
---
# <a name="microsoft-identity-manager-2016-portal-customization"></a>Microsoft Identity Manager 2016 포털 사용자 지정


>[!Warning]
CSS 사용자 지정을 수행할 때 브라우저 캐시를 지워야 합니다.

MIM(Microsoft Identity Manager) 2016에서 배너 로고, 문자열 리소스 및 CSS 스타일시트를 비롯한 암호 포털의 선택한 요소를 사용자 지정할 수 있습니다.

이를 위해 사용자 지정 수준에 따라 몇 가지 항목이 필요합니다. 다음은 MIM 2016 암호 등록 및 재설정 포털의 사용자 지정과 관련된 항목 목록입니다.

-   Customizations 폴더 - MIM 2016에서 기본 폴더를 사용하기 전에 확인하는 폴더입니다. 사용자 지정될 각 포털에는 Customizations 폴더가 필요합니다. 설치 프로그램은 업그레이드, 변경 모드 설치 또는 복구 모드 설치 시 이 폴더를 덮어쓰지 않으므로 이 폴더에서만 사용자 지정을 수행해야 합니다.

-   Strings.resources - 포털에 나타나는 문자열을 수정할 수 있는 XML 기반 파일입니다. 이 파일은 Customizations 폴더에 있어야 합니다.

-   Style.css – 포털에서 사용자 지정에 사용되는 CSS 스타일시트입니다. 이 스타일시트를 생성하고 수정해서 로고를 변경하거나 사용자 지정된 내용으로 완전히 바꿀 수 있습니다.

암호 등록 및 암호 재설정 포털 사용자 지정에 대한 자세한 단계별 지침은 테스트 랩 가이드: MIM 2016 암호 등록 및 재설정 포털 사용자 지정 시연을 참조하세요.

>[!WARGNING] MIM에서 사용자 지정된 변경 내용을 인식하도록 하려면 iisreset를 실행하여 IIS를 다시 시작해야 합니다.


## <a name="creating-the-customizations-folder"></a>Customizations 폴더 만들기

시작 시 MIM은 기본 폴더를 사용하기 전에 Customizations 폴더에서 Strings.resources 파일을 찾습니다. 사용자 지정하려는 포털(즉, 암호 등록 포털 또는 암호 재설정 포털)에 대한 디렉터리 아래에 Customizations 폴더를 만들어야 합니다. 두 포털을 모두 사용자 지정하려는 경우 다음 각 노드 아래에 Customizations 폴더를 만들어야 합니다.

-   C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Password Registration Portal

-   C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Password Reset Portal

### <a name="to-create-the-customization-folder"></a>Customizations 폴더를 만들려면

1.  C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Password Registration Portal 폴더로 이동합니다.

2.  이름이 Customizations인 폴더를 만듭니다.

3.  Password Reset Portal 폴더의 한 수준 이전으로 가서 Customizations라는 폴더를 만듭니다.

## <a name="customizing-strings"></a>문자열 사용자 지정

Strings.resources 파일을 만들고 Customizations 폴더에 추가하여 포털 UI에 있는 많은 문자열을 사용자 지정할 수 있습니다. 사용자 지정하려는 각 포털에 대한 Strings.resources 파일을 만들어야 합니다.

### <a name="to-customize-strings"></a>문자열을 사용자 지정하려면

1.  메모장을 사용해서 아래의 다음 코드를 복사하고 Customizations 폴더에 Strings.resources로 저장합니다.

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <root>
     <resheader name="resmimetype">
       <value>text/microsoft-resx</value>
     </resheader>
     <resheader name="version">
       <value>2.0</value>
     </resheader>
     <resheader name="reader">
       <value>System.Resources.ResXResourceReader, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
    </resheader>
     <resheader name="writer">
       <value>System.Resources.ResXResourceWriter, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
     </resheader>

    <!-- Customizations begin here -->
     <data name=" QAGateResetTitle " xml:space="preserve">
       <value>Contoso Question and Answer Reset</value>
     </data>
     <data name="ResetPageTitle" xml:space="preserve">
       <value>Contoso Self-Service Password Reset</value>
     </data>
   </root>

   ```
2.  `<!-- Customizations begin here -->` 섹션 아래에서 사용자 지정하려는 문자열과 일치하도록 데이터 이름을 변경하고 해당 문자열의 값을 <value></value> 태그 사이에 입력합니다. 사용자 지정할 수 있는 문자열 및 기본값은 아래 목록을 참조하세요.

>[!NOTE]
**Strings.resources** 파일은 언어 중립적입니다. 언어별로 사용자 지정된 문자열을 만들려면 해당 언어 팩을 설치하고 파일을 Strings. `<language>-<culture>.resources` 형식으로 저장합니다(예: Strings.en-us.resources).

다음은 사용자 지정할 수 있는 포털 문자열의 목록입니다.

<a name="portal-strings"></a>포털 문자열
--------------

| Name                                                     | 기본값                                                                                                                                                                                                                                                                                                                                         | 설명                                                                                                                                                                                                                                            |
|---------------------------|-------------------|--------------|
| AboutLinkText                                            | 소개         |       |
| ButtonCancel                                             | 취소                                                                                 |     |
| ButtonFinish                                             | 마침    |    |
| ButtonNext                                               | 다음    |    |
| ButtonOk                                                 | 확인     |   |
| CancelFinishedMessage                                    | 세션이 더 이상 활성 상태가 아닙니다. 창을 닫거나 아래의 링크를 클릭하여 다시 시작할 수 있습니다.         |      |
| CancelFinishedTitle                                      | 세션이 종료됨                                                              |      |
| ErrorDescription_3000                                    | 오류가 발생했습니다. 다시 시도하고 문제가 계속되면 기술 지원팀 또는 시스템 관리자에게 문의하십시오. (오류 3000)       |          |
| ErrorDescription_3001                                    | 사용자 이름을 올바르게 입력했는지 확인하십시오. 여전히 암호를 재설정할 수 없다면      |           |
|                                                          | 기술 지원팀에 문의하십시오. (오류 3001)   |                   |
| ErrorDescription_3002                                    | 세션이 종료되었습니다. 다시 시작하려면 홈 페이지로 돌아가십시오. (오류 3002)                                     |                |
| ErrorDescription_3003                                    | Forefront Identity Manager에서 현재 사용자 계정을 인식할 수 없습니다. 기술 지원팀 또는 시스템 관리자에게 문의하십시오. (오류 3003)           |               |
| ErrorDescription_3004                                    | 암호 재설정을 등록할 수 있는 권한이 없습니다. 기술 지원팀 또는 시스템 관리자에게 문의하십시오. (오류 3004)     |              |
| ErrorDescription_3005                                    | 입력한 답 중 하나 이상이 암호 등록 중에 입력한 답과 일치하지 않습니다. 암호를 재설정하려면 지금 입력하는 답이 등록 시 입력한 답과 일치해야 합니다. 홈 페이지에서 다시 시작하거나 기술 지원팀 또는 시스템 관리자에게 문의할 수 있습니다. (오류 3005) |           |
| ErrorDescription_3006                                    | 입력한 암호가 올바르지 않습니다. 암호 재설정을 등록하려면 올바른 암호를 입력해야 합니다. (오류 3006)            |              |
| ErrorDescription_3007                                    | 일시적으로 암호를 재설정할 수 없습니다. 나중에 다시 시도하거나 기술 지원팀 또는 시스템 관리자에게 문의하십시오. (오류 3007)     |         |
| ErrorDescription_3008                                    | 오류가 발생했습니다. 다시 시도하고 문제가 계속되면 기술 지원팀 또는 시스템 관리자에게 문의하십시오. (오류 3008)        |          |
| ErrorDescription_3009                                    | 입력에 허용되지 않는 형식의 텍스트가 포함되어 있습니다. 다른 형식의 텍스트를 다시 입력하거나 기술 지원팀 또는 시스템 관리자에게 문의하십시오. (오류 3009)     |             |
| ErrorDescription_3010_Registration                       | 브라우저에서 스크립팅을 사용하도록 설정되어 있지 않습니다. 스크립팅을 사용하도록 설정하고 암호 등록 홈 페이지로 돌아가거나 기술 지원팀에 문의하십시오.      |               |
| ErrorDescription_3010_Reset                              | 브라우저에서 스크립팅을 사용하도록 설정되어 있지 않습니다. 스크립팅을 사용하도록 설정하고 암호 재설정 홈 페이지로 돌아가거나 기술 지원팀에 문의하십시오.     |            |
| ErrorDescription_3011                                    | 이 사이트는 쿠키를 사용합니다. 브라우저에서 쿠키를 허용하도록 구성한 후 다시 시도하거나 기술 지원팀에 문의하십시오.          |                                           |
| ErrorDescription_3012                                    | 입력한 데이터가 사용자에게 보낸 보안 코드와 일치하지 않습니다. 암호를 다시 재설정하거나 기술 지원팀에게 문의하십시오.      |                                                                                                                                                                                                                                                    |
| ErrorDescription_3013                                    | 보안 코드를 보낼 수 없습니다. 기술 지원팀에게 문의하십시오                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                                    |
| ErrorMessageDomainUsernameFormat                         | 사용자 이름을 올바른 형식으로 입력하십시오.                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                    |
| ErrorMessageDomainUsernameRequired                       | 계속하려면 사용자 이름을 입력하십시오.                                              |                         |
| ErrorMessagePasswordRequired                             | 암호를 입력하십시오.              |                                                |
| ErrorMessagePasswordsDoNotMatch                          | 두 암호가 일치하는지 확인하십시오.                                |           |
| ErrorPageDefaultHeading                                  | 응용 프로그램 오류                                            |               |
| ErrorPageServerTime                                      | 서버 시간: {0:T}                     | {0}은(는) 예외가 catch된 시간입니다. 'T'를 사용하면 전달된 시간의 형식이 "긴 시간"으로 지정됩니다. 결과적으로 시간, 분 및 초와 AM/PM 지정(현재 문화권에 따라 다름)을 표시합니다. |
| ErrorPageTitle                                           | Forefront Identity Management - 암호 오류                     |   |
| ErrorTitle_3000                                          | 오류                                  |  |
| ErrorTitle_3001                                          | Access Denied                          |  |
| ErrorTitle_3002                                          | 세션이 종료됨                          |  |
| ErrorTitle_3003                                          | 알 수 없는 사용자                      |  |
| ErrorTitle_3004                                          | 권한이 없는 사용자                      |  |
| ErrorTitle_3005                                          | 대답이 일치하지 않음                    |  |
| ErrorTitle_3006                                          | 잘못된 암호                     |  |  
| ErrorTitle_3007                                          | 일시적으로 액세스 거부됨              |  |
| ErrorTitle_3008                                          | 통신 오류                    |  |
| ErrorTitle_3009                                          | 입력이 금지됨                       |  |
| ErrorTitle_3010                                          | 브라우저 구성 오류            |  |
| ErrorTitle_3011                                          | 브라우저 구성 오류            |  |
| ErrorTitle_3012                                          | 확인 실패                    |  |
| ErrorTitle_3013                                          | 보안 코드를 보낼 수 없음   |  |
| FinalizeRegistrationHeading1                             | 암호를 재설정해야 하는 경우:        |   |
| FinalizeRegistrationSubHeading1                          | 암호 재설정 포털로 이동   |   |
| FinalizeRegistrationSubHeading2                          | 본인 여부 확인   |   |
| FinalizeRegistrationSubHeading3                          | 새 암호 선택    |     |
| FinishingDescription                                     | 새 암호 선택        |    |
| FinishingTitle                                           | 암호 재설정:      |     |
| GotoPortalPrefix                                         | 이동     |        |
| GotoPortalSuffix                                         | 홈 페이지     |      |
| LabelTroubleshootingLinkText                             | 세부 정보 보기           |    |
| LoadingText                                              | 로드 중...     |  |
| NoScriptTagErrorMessage                                  | 브라우저에서 스크립팅을 사용하도록 설정되어 있지 않습니다. 스크립팅을 사용하도록 설정하고 홈 페이지로 돌아가거나 기술 지원팀에 문의하십시오.      |   |
| PasswordResetOperationGeneralErrorMessage                | 암호를 재설정하는 동안 오류가 발생했습니다.       |   |
| PasswordResetOperationPolicyViolationErrorMessage            | 암호가 조직의 암호 정책을 준수하지 않습니다.             |       |
| PasswordResetOperationUserCantChangePasswordErrorMessage | 암호를 재설정하는 동안 오류가 발생했습니다. 사용자가 암호를 변경할 수 없습니다.    |   |
| PrivacyStatement                                         | 개인 정보 취급 방침                                                      |    |
| RegistrationDescription                                  | 셀프 서비스 암호 등록     |    |
| RegistrationMission                                      | 암호를 잊은 경우 기술 지원팀에 문의하지 않고 직접 암호를 재설정할 수 있습니다.   |      |
| RegistrationPageTitle                                    | Forefront Identity Management - 암호 등록                                          |   |
| RegistrationSteps                                        | 등록 프로세스를 시작하려면 ‘다음’을 클릭하십시오.   |      |
| RegistrationSuccessDescription                           | 이제 등록되었습니다.        |   |
| RegistrationSuccessTitle                                 | 완료됨:   |    |
| RegistrationSuccessTitle                                 | 암호 등록:    | |
| ResetDescription                                         | 셀프 서비스 암호 재설정:  |    |
| ResetEnterNamePrompt                                     | 아래에 사용자 이름을 입력하십시오.     |     |
| ResetEnterPassword                                       | 새 암호 입력:                                                  |   |
| ResetExample1                                            | contoso\\mmeyers                                                                            |      |
| ResetExample2                                            | mmeyers\@contoso.com     |      |
| ResetExamples                                            | 예:  |    |
| ResetPageTitle                                           | Forefront Identity Management - 암호 재설정       |     |
| ResetReenterPassword                                     | 암호 재입력:    | |
| ResetSuccessDescription                                  | 암호가 재설정됨    |    |
| ResetSuccessTitle                                        | 성공:                                |    |
| ResetUseNewPassword                                      | 이제 새 암호로 로그인할 수 있습니다.     |      |
| ResetUsernameTextFormat                                  | ({0}의 암호 재설정)       | {0}은(는) 사용자의 로그온입니다.             |
| ResetWelcomeTitle                                        | 암호 재설정:     |      |
| TroubleshootingEmailSubject                              | FIM 요청 처리 오류 세부 정보     |       |
| TroubleshootingLabelAttributes                           | 특성:    |    |
| TroubleshootingLabelCloseButton                          | 닫기       |    |
| TroubleshootingLabelCopyToClipboard                      | 클립보드에 복사     |     |
| TroubleshootingLabelCorrelationId                        | 상관 관계 ID:      |          |
| TroubleshootingLabelDetails                              | 세부 정보:                                                             |    |
| TroubleshootingLabelPostCopyClipboardMessage             | 정보가 클립보드로 복사되었습니다.           |    |
| TroubleshootingLabelRequestId                            | 요청 ID:                  |    |
| TroubleshootingLabelSendEmail                            | 전자 메일로 정보 보내기                            |    |
| TroubleshootingLabelSource                               | 이유:                                                                     |    |
| TroubleshootingLabelViewRequestDetails                   | 요청 세부 정보 보기        |              |                                                    
| TroubleshootingLinkText                                  | 문제 해결 정보          |                  | |



다음은 사용자 지정할 수 있는 인증 게이트 문자열의 목록입니다.

<a name="authentication-gate-strings"></a>인증 게이트 문자열
---------------------------

| Name                                          | 기본값                                                                                                                                                                                                                | 비고 |
|-----------|--------------|----------|
| OTPEmailRegistraionEmailTextboxLabel          | 전자 메일 주소:             |          |
| OTPEmailRegistrationEmailRequiredErrorMessage | 전자 메일 주소 필드는 비워둘 수 없습니다.     |          |
| OTPEmailRegistrationFooterReadOnly            | 전자 메일 주소를 업데이트하려면 조직에서 정의한 프로세스를 따르거나 기술 지원팀에 문의하십시오.                   |          |
| OTPEmailRegistrationFooterReadWrite           | 전자 메일 주소는 해당 조직에 의해 Forefront Identity Manager에 저장됩니다.                       |          |
| OTPEmailRegistrationGateTitle                 | 전자 메일 주소 확인   |          |
| OTPEmailRegistrationHeaderReadOnly            | 암호를 재설정해야 할 경우 전자 메일로 확인 보안 코드가 전송됩니다. 아래 표시된 전자 메일 주소가 올바르지 않은 경우 셀프 서비스 암호 재설정을 사용하도록 이를 업데이트해야 합니다. |          |
| OTPEmailRegistrationHeaderReadWrite           | 아래에 전자 메일 주소를 입력하십시오. 암호를 재설정해야 할 경우 전자 메일로 확인 코드가 전송됩니다.                 |          |
| OTPEmailResetGateTitle                        | ID 확인: 전자 메일 확인         |          |
| OTPEmailResetHeader                           | 아래에 보안 코드를 입력하십시오. 이 조직에 등록된 휴대폰으로 보안 코드가 전송되었습니다.                                                                                                             |          |
| OTPRegularExpressionErrorMessage              | 지정한 값이 예상한 형식과 일치하지 않습니다.                   |          |
| OTPResetOneTimePasswordRequiredErrorMessage   | 보안 코드 필드는 비워둘 수 없습니다.        |          |
| OTPResetVerificationLabel                     | 보안 코드:                    |          |
| OTPSmsRegistrationFooterReadOnly                      | 휴대폰 번호를 업데이트하려면 조직에서 정의한 프로세스를 따르거나 기술 지원팀에 문의하십시오.   ||
| OTPSmsRegistrationFooterReadWrite                     | 휴대폰 번호는 해당 조직에 의해 Forefront Identity Manager에 저장됩니다.                                                                                                                                                     |   |
| OTPSmsRegistrationGateTitle                           | 휴대폰 확인                      |   |
| OTPSmsRegistrationHeaderReadOnly                      | 암호를 재설정해야 할 경우 휴대폰으로 확인 보안 코드가 전송됩니다. 아래 표시된 휴대폰 번호가 올바르지 않은 경우 셀프 서비스 암호 재설정을 사용하도록 이를 업데이트해야 합니다. |   |
| OTPSmsRegistrationHeaderReadWrite                     | 아래에 휴대폰 전화 번호를 입력하십시오. 암호를 재설정해야 할 경우 휴대폰으로 확인 코드가 전송됩니다.                                                                                                     |   |
| OTPSmsRegistrationMobilePhoneRequiredErrorMessage     | 휴대폰 번호 필드는 비워둘 수 없습니다.      |   |
| OTPSmsRegistrationSMSTextBoxLabel                     | 휴대폰:                    |   |
| OTPSmsResetGateTitle                                  | ID 확인: 휴대폰         |   |
| OTPSmsResetHeader                                     | 아래에 보안 코드를 입력하십시오. 이 조직에 등록된 휴대폰으로 보안 코드가 전송되었습니다.                                                                                                                           |   |
| PasswordGateDescriptionText                           | 아래에 현재 암호를 입력하고 '다음'을 클릭하십시오.        |   |
| PasswordGateErrorMessagePasswordRequired              | 현재 암호를 입력하십시오.                  |   |
| PasswordGateGateTitle                                 | 현재 암호               |   |
| PasswordGatePasswordLabelText                         | 암호:                 |   |
| PasswordGateUsernameTextFormat                        | `<i>`(로그인한 계정: `<b>{0}</b>)</i>`                                                          |   |
| QAGateErrorNotEnoughQuestionsAnswered                 | 최소한 {0}개 이상의 질문에 답해야 합니다.        |   |
| QAGateIncorrectAnswer                                 | 답변이 올바르지 않습니다.       |   |
| QAGatePrivacyNotice                                   | 제공한 응답은 해당 조직에 의해 Forefront Identity Manager에 저장됩니다.                                                                                                                                                  |   |
| QAGateRegistrationNumberOfQuestionsExplanation_Format | 등록하려면 최소한 {0}개 이상의 질문에 답해야 합니다.     |   |
| QAGateRegistrationOneOrMoreAnswersFailedValidation    | 하나 이상의 응답이 정책을 준수하지 않습니다.      |   |
| QAGateRegistrationThisAnswerValidationFailed          | 이 응답은 정책을 준수하지 않습니다.      |   |
| QAGateRegistrationTitle                               | 답 등록         |   |
| QAGateResetNumberOfQuestionsExplanation_Format        | 다음 {1}개의 질문 중 {0}개에 답해야 합니다.   |   |
| QAGateResetTitle                                      | ID 확인: 답 전송                                  |   |



## <a name="customizing-the-logo-banner"></a>로고 배너 사용자 지정

포털 페이지의 기본 배너를 조직에 대해 사용자 지정할 수 있습니다.
로고 배너를 사용자 지정하려면

1.  사용자 지정 배너를 만들고 .png 파일로 저장합니다. 파일은 다음 권장 사항을 충족해야 합니다.

 - 크기: 490 X 50픽셀

 - 비트 수준: 32

2.  파일을 사용자 지정하려는 각 포털의 \\Customizations 폴더에 복사합니다.

3.  각 폴더에 Style.css 파일을 만듭니다. Customizations 폴더 및 새 로고를 가리키도록 합니다. 해당하는 경우 로고 이름을 변경할 수 있습니다(예: /Customizations/contosologo.png). 코드는 다음과 같이 나타납니다.

   **예제 1:**

  `.title-block{background:url(../Customizations/fimlogo.png) no-repeat scroll 0 0 transparent;}`

4.  Internet Explorer 6.0을 사용하는 경우 대체 불투명 로고를 제공하고 Style.css에 다음 코드를 추가해야 합니다.

  `.ie6 .title-block{background-image:url(../Customizations/fimlogo-ie6.png);}`

  **예제 2:**

  `.title-block{background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;}`

Internet Explorer 6.0을 사용하는 경우 대체 불투명 로고를 제공하고 Style.css에 다음 코드를 추가해야 합니다.

  `.ie6 .title-block{background-image:url(../Customizations/contosologo-ie6.png);}`

## <a name="customizing-image-for-smartphones"></a>스마트폰의 이미지 사용자 지정

다음을 사용하여 스마트폰의 이미지를 사용자 지정할 수 있습니다. 스마트폰 이미지를 사용자 지정하려면

1.  이미지를 만들고 .png 파일로 저장합니다. 파일은 다음 권장 사항을 충족해야 합니다.

    - 크기: 190 X 50픽셀
    - 비트 수준: 32

2.  파일을 사용자 지정하려는 각 포털의 \\Customizations 폴더에 복사합니다.

2.  각 폴더에 Style.css 파일을 만듭니다. Customizations 폴더 및 새 로고를 가리키도록 합니다. 해당하는 경우 로고 이름을 변경할 수 있습니다(예: /Customizations/contosologo.png). 코드는 다음과 같이 나타납니다.

  **예제 1:**

```css
@media only screen and (max-width: 480px)

   {

    .title-block

     {
       background: url("path_to_image/imagename.png") no-repeat scroll 0 0 transparent;
     }

   }
   ```

## <a name="customizing-style-sheets"></a>스타일시트 사용자 지정

사용자 지정된 CSS 스타일시트를 사용하여 암호 포털의 레이아웃 및 스타일을 수정할 수 있습니다. 사용자 지정된 CSS를 사용하려면

1.  사용자 지정된 CSS 파일을 만들고 Style.css로 저장합니다.

2.  파일을 사용자 지정하려는 각 포털의 \\Customizations 폴더에 복사합니다.

다음은 Style.css 파일의 기본 예제입니다.

```css
body
{
  font: 15px Algerian;
  color: \#303030;
  background: white;
}

.pad
{
  padding: 30px;
  padding-top: 50px;
  background: white;
}

.backgroundWhite
{
  border: \#e9e9e9 2px solid;
} .
title-block
{
background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;
}
```


>[!IMPORTANT]
MIM에서 사용자 지정된 변경 내용을 인식하도록 하려면 iisreset를 실행하여 IIS를 다시 시작해야 합니다.                                                                                                                                                                                       

다음은 Style.css 파일의 좀 더 수준 높은 예제입니다. 이 파일은 이러한 장치에 포털을 표시하기 위한 스마트폰 및 iPad 관련 정보를 제공합니다.

```css
/****************
BASE
*****************/

body {
    font-size: 14px; /*Customizeable- Body Font Size */
    background-color: #ced5ec; /*Customizeable- Backgound Color behind the product */
}

body, button, input, select, textarea {
    font-family: Segoe UI, Arial, Verdana, Sans-Serif, Helvetica; /*Customizeable- Body Font Family */
    color: #595959; /*Customizeable- Body Font Color */
}

/****************
LINKS
*****************/

a { color: #396faf; text-decoration: none; } /*Customizeable- Link Color and Underline */
a:visited { color: #396faf; text-decoration: none; } /*Customizeable- After Link is clicked color and underline */
a:hover { color: #6486ae; text-decoration: none; } /*Customizeable- Hover mouse over Link color and underline */
a:focus { outline: thin dotted; } /*Customizeable- Keyboard event to Link and Link is in focus outline*/
a:hover, a:active { outline: 0; } /*Customizeable- Hover and Active Link outline */

/****************
Typography
*****************/

hr { border-top: 1px solid #acd9ec; } /*Customizeable- Horizontal Rule Color Above the Footer */

/****************
Layout
*****************/
#wrapper {
    background: url(../images/bg-top-slice.png) repeat-x 0 0; /*Customizeable-remove this line to remove top gradient */
}

#container {
    background: url(../images/bg-bottom-slice.png) repeat-x 100% 100% transparent;  /*Customizeable-remove this line to remove bottom gradient */
}

.title-block {
    background: url("../images/fimlogo.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 600px or less in width. Logo must be 50px or less in height. */
    border-bottom: 2px solid #acd9ec;/*Customizeable- 2px border color under logo */
}

.ie6 .title-block {
    background-image: url(../images/fimlogo-ie6.png);   /*Customizeable- Can make a non-transparent image for IE6 only */
}

h2 {
    color: #578e4c; /*Customizeable- h2 page header color */
}

h3 {
    color: #999; /*Customizeable- h3 page header color */
}

input[type=text]:focus, input[type=password]:focus {
    border: #82bd3b 2px solid; /*Customizeable- Highlight color around textbox when cursor is inside */
}

.chromeButton, .chromeButton:visited {
    background-color: #333; /*Customizeable- Color of button */
    color: #fff; /*Customizeable- Color of text on the button */
    border: 1px solid #666; /*Customizeable- Border color of button */
}

.chromeButton:hover {
    background-color: #666; /*Customizeable- Hover color of button */
    border: 1px solid #999; /*Customizeable- Hover border color of button */
}

.qcol /*Style from QAgate.css */ {
    color: #7a7a7a; /*Customizeable- Font color of Q&A container */
    background-color:#e6e7e9; /*Customizeable- Background color of Q&A container */
}

/****************
Media Queries
*****************/

/* Smartphones ----------- */
@media only screen and (max-width: 480px) {
    body {
        font-size:12px; /*Customizeable- Body Font Size for devices */
    }

    .title-block {
        background: url("../images/fim-logo-portrait.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 190px (landscape) or less in width. Logo must be 50px or less in height. */
    }
    h2, h3 {
        font-size:14px; /*Customizeable- H2 and H3 Heading Size for devices */
    }
}


/* iPads (landscape) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : landscape)
{
}

/* iPads (portrait) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : portrait)
{
}
```


## <a name="common-customization-issues"></a>일반적인 사용자 지정 문제

다음 표에서는 FIM 서비스 및 포털 업그레이드 시 발생할 수 있는 일반적인 알려진 문제 목록입니다. 이 표에는 이러한 문제에 대한 해결 방법도 포함되어 있습니다.

|문제 |해결 방법                                                                    |
|------|------------------------------|
| 문자열을 사용자 지정했으나 UI에 반영되지 않았습니다.         | strings.resources에서 문자열을 사용자 지정하려면 항상 iisreset이 필요합니다.         |
| strings.resources를 변경했으나 문자열 변경 내용이 표시되지 않습니다.     | strings.resources 형식이 잘못되었으므로 포털에서 무시된 것일 수 있습니다. Windows 로그 - 응용 프로그램 및 서비스 로그 - Forefront Identity Manager에서 이벤트 로그를 확인합니다.                        |
| Style.css를 처음 추가했을 때 포털에 스타일 변경 내용이 표시되지 않았습니다.            | Style.css 파일을 처음 도입할 때 Iisreset을 수행해야 합니다.     |
| 새 스타일이 Style.css에서 추가/수정되었으나 브라우저에 변경 내용이 표시되지 않았습니다.      | 브라우저 캐시를 지우고 페이지를 새로 고칩니다. <br/> CSS 구문 확인    |
| path_to_sspr_portal\\css\\\*.css에서 CSS 폴더 내용을 직접 변경하거나 path_to_sspr_portal\\images\\fimlogo.png에서 배너 로고를 직접 변경했으나 업그레이드 시 이러한 변경 내용이 손실됩니다. | 시작하려면 이러한 파일을 변경하지 말아야 합니다. Customization 폴더는 Style.css에서 배너 로고 및 CSS 스타일시트 사용자 지정을 제공하는 수단으로만 사용하세요. Customization 폴더는 주요 업그레이드에 의해서도 의도적으로 덮어쓰여지지 않습니다. <br/><br/>ILSpy 및 리플렉터와 같은 도구를 사용하여 포털 어셈블리에서 문자열을 변경하지 않도록 합니다. strings.resources를 사용하여 기본 문자열을 재정의합니다. 업그레이드 시 어셈블리가 교체됩니다.  |
| 배너 로고는 포털에 표시되지 않지만 FIM 로고는 계속 표시됩니다.     | Style.css의 이미지 이름/경로가 유효하지 않거나 브라우저 캐시가 지워지지 않았습니다.          |
| IE6에서 배너 로고가 이상하게 보입니다.       | IE6용 불투명 이미지와 style.css의 특별한 스타일을 제공해야 합니다.        |
