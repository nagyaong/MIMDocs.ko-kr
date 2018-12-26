---
title: 관리자 권한 없이 Microsoft Identity Manager 셀프 서비스 스마트 카드 갱신 | Microsoft 문서
description: 관리자가 사용자의 컴퓨터에 액세스하지 않고도 사용자가 인증서 관리자를 사용할 수 있도록 사용자의 스마트 카드를 등록하는 방법을 알아봅니다.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: bfabc562-a2f0-4cff-ac31-36927f41e102
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4d66c566912f186bce175dde9f16346942afd72e
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358220"
---
# <a name="enroll-smart-cards-for-non-administrators"></a>관리자가 아닌 사용자의 스마트 카드 등록
사용자가 사용하는 컴퓨터의 로컬 관리자가 아니면 기본적으로 해당 컴퓨터에 스마트 카드를 등록할 수 없습니다. 다음 절차를 사용하여 이 제한을 해결할 수 있습니다.

## <a name="enabling-smart-card-renewal-for-non-admins-in-mim-2016-certificate-manager"></a>MIM 2016 인증서 관리자에서 관리자가 아닌 사용자가 스마트 카드를 갱신하도록 설정합니다.

1.  **appx 파일의 압축을 풉니다.**

    서명 인증서를 가져옵니다. 단계에 따라 [내부 PKI를 사용하여 Windows 8 애플리케이션에 서명](http://blogs.technet.com/b/deploymentguys/archive/2013/06/14/signing-windows-8-applications-using-an-internal-pki.aspx)합니다. "애플리케이션에 서명"이 표시되면 중지합니다. 내보낸 pfx 파일의 이름을 지정합니다. .cer 파일로도 내보내고 이 파일을 새 서명 인증서의 cer 파일을 사용하는 클라이언트에 가져옵니다.

    다음을 실행하여 appx 파일의 압축을 풉니다.

    `makeappx unpack /l /p <app package name>.appx /d ./appx`

    `ren <app package name>.appx <app package name>.appx.old`

    `cd appx`

2.  **구성 파일 수정**

    이름이 `CustomDataExample.xml custom.data`인 파일의 이름을 바꿉니다. CM 앱이 이 파일 이름을 찾습니다.

    custom.data 파일을 편집하고 다음을 수정합니다.

    1.  &lt;NonAdmin&gt; 요소에서 Value 특성 값을 "True"로 변경합니다.

    2.  파일을 저장하고 편집기를 종료합니다.

    3.  이름이 AppxSignature.p7x인 파일을 삭제합니다.

    4.  이름이 AppxManifest.xml인 파일을 편집합니다.

    5.  &lt;Identity&gt; 요소에서 Publisher 특성 값을 서명 인증서의 주체로 수정합니다. 예: "CN=ABCD"

        여기서 주체는 앱에 서명할 때 사용한 서명 인증서의 주체와 동일해야 합니다.

    6.  파일을 저장하고 편집기를 종료합니다.

3.  **다시 압축하고 앱 패키지(appx 파일)에 서명합니다.**

    다음을 실행하여 appx 파일을 압축하고 서명합니다.

    `cd ..`

    `makeappx pack /l /d .\appx /p <app package name>.appx`

    `signtool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.appx`

4.  프로필 템플릿을 복제하고 초기 관리자 키를 추가하여 MIM 서버를 구성합니다.

    1.  CM 포털에 관리자 권한이 있는 사용자로 로그인합니다.

    2.  **관리** &gt; **프로필 템플릿 관리**로 이동하여 만든 프로필 템플릿 옆에 있는 확인란이 선택되어 있는지 확인한 다음 Copy a selected profile template(선택한 프로필 템플릿 복사)을 클릭합니다.

    3.  프로필 템플릿의 이름을 입력하고, "nonAdmin"을 추가하고 **확인**을 클릭합니다.

    4.  프로필 템플릿의 일반 설정이 표시되면 아래로 스크롤하여 **스마트 카드 구성** 아래에서 **설정 변경**을 클릭합니다.

    5.  **Admin key initial value (hex)** (관리자 키 초기 값(16진수)) 아래에서 기본 관리자 키를 입력합니다. "010203040506070801020304050607080102030405060708"

    6.  아래로 스크롤하여 **확인**을 클릭합니다.

5.  **클라이언트 컴퓨터에 관리자가 아닌 계정 만들기**

    관리자가 아닌 사용자는 TPM에서 가상 스마트 카드를 만들 수 없으므로 관리자가 대신 만들어야 합니다.

6.  **TpmVscMgr을 사용하여 가상 스마트 카드 만들기**

    관리자 계정으로 다음을 실행하여 컴퓨터에 빈 가상 스마트 카드를 만듭니다. 이 작업은 Intune, SCCM 또는 그룹 정책을 통해 수행할 수 있습니다.

    `TpmVscMgr create /name MyVSC /pin default /adminkey default /generate`

7.  **관리자가 아닌 계정에서 CM 앱 설치**

8.  **CM 앱을 시작하고 가상 스마트 카드 등록**
