---
# required metadata

title: 암호 변경 알림 서비스 배포 | Microsoft Identity Manager
description: 도메인 컨트롤러에서 MIM 암호 변경 알림 서비스를 설치하고 구성하는 단계를 알아봅니다.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 97edae12-6f86-4f9f-8620-a95a096e482a

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# 도메인 컨트롤러에 MIM 암호 변경 알림 서비스 배포

## 암호 변경 알림 서비스 설치
PCNS(암호 변경 알림 서비스)는 도메인 컨트롤러에 설치하는 서비스로, 이를 통해 다른 공급업체의 디렉터리 서버 같은 다른 시스템에 대해 MIM에서 암호 동기화를 사용하도록 설정할 수 있습니다. 암호 동기화를 위해 각 도메인 컨트롤러 서버에 PCNS를 설치합니다.

1.  Active Directory 도메인 서비스의 역할을 가진 Windows Server에서 실행 중인 서버에 도메인 관리자로 로그인합니다.

2.  암호 변경 알림 서비스 설치 폴더를 컴퓨터에 복사합니다.

3.  *Password Change Notification Service.msi* 파일을 찾아서 마우스 오른쪽 단추로 클릭하고 바로 가기를 만듭니다.

4.  바로 가기 파일을 찾아 마우스 오른쪽 단추로 클릭하고 해당 **속성**을 표시합니다.

5.  대상 필드에서 msi 파일의 경로 앞에 프리앰블 *msiexec.exe /i*를 추가하고 msi 경로 뒤에 접미사 *SCHEMAONLY = TRUE*를 추가합니다. 예를 들어 설치 폴더가 *C:\PCNS*인 경우 실행할 명령은 다음과 같습니다(모두 한 줄에 표시).

    ```
    msiexec.exe /i "C:\PCNS\x64\Password Change Notification Service.msi" SCHEMAONLY=TRUE
    ```

6.  변경 사항을 바로 가기 파일에 저장합니다.

7.  바로 가기 파일을 실행하여 스키마 확장 모드에서 PCNS 설치를 시작합니다. 다음 화면이 나타나면 **다음**을 클릭합니다.

8.  이제 설치에서 암호 변경 알림 서비스를 위해 Active Directory 스키마를 업데이트한다는 알림이 표시됩니다. **확인** 을 클릭하여 스키마 업데이트를 계속 진행합니다.

9. 스키마 확장 프로세스가 완료되고 다음 화면이 표시되면 **마침**을 클릭합니다.

10. *Password Change Notification Service.msi* 파일을 다시 실행합니다. 이번에는 직접 실행하는 것이므로 실행 문자열이 필요하지 않습니다.  다음 화면이 나타나면 **다음**을 클릭합니다.

11. 사용권 계약에 동의하고 **다음**을 클릭합니다.

12. 클릭하여 설치를 시작합니다.

13. 설치가 성공적으로 완료되었다는 화면이 나타나면 **마침**을 클릭합니다.

14. MIM 암호 변경 알림 서비스에 대한 구성 변경 사항을 적용하려면 컴퓨터를 다시 시작합니다. 표시되는 팝업에서 **예** 를 클릭하여 다시 시작하거나 나중에 다시 시작할 수 있습니다.

## 암호 변경 알림 서비스 구성
도메인 관리자로 DC 서버에 다시 연결되면 *C:\Program Files\Microsoft Password Change Notification*으로 이동합니다. *pcnscfg.exe*를 실행합니다.


<!--HONumber=Apr16_HO2-->


