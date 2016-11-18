---
title: "부록"
description: "스크립트를 사용하여 Privileged Identity Manager에서 관리할 기존 또는 새 ID로 CORP 도메인을 준비합니다."
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/27/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 689c2ef0e4e4a681a398ba7e94fb3def525937ea
ms.openlocfilehash: 482cfbbac3ea668ca4bf9d8a4a45469e61634f98


---
# 부록:

## 부록 1 PRIV 도메인 설정

$env:SYSTEMDRIVE\PAM 폴더에 압축 파일의 압축을 풀고 PAMDeploymentConfig.xml을 편집하여 PRIV 포리스트의 세부 정보를 제공합니다. DNS 이름, Netbios 이름, DC 이름, 데이터베이스/로그 경로 및 Sysvol 경로를 업데이트합니다. 또한 도메인 및 포리스트 모드를 업데이트합니다. Windows Server Technical Preview 5를 테스트하는 경우 DomainMode 및 ForestMode를 WinThreshold로 설정하세요.

1. 관리자 권한으로 PRIV 도메인 DC에 로그인합니다.
2. 관리자 권한으로 PowerShell을 실행합니다.
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. 메뉴 옵션 9(PRIV 포리스트 설정)를 선택합니다.


완료되면 DC가 자동으로 다시 부팅됩니다. DSRM(디렉터리 서비스 복원 모드) 관리자 암호는 다음 조건과 일치해야 합니다.

  * 암호 길이는 15자 이상이어야 합니다.
  * 암호에 하나 이상의 소문자를 포함해야 합니다.
  * 암호에 하나 이상의 대문자를 포함해야 합니다.
  * 암호에 하나 이상의 숫자 또는 특수 문자를 포함해야 합니다.

## 부록 2 CORP 도메인 설정

PAM을 처음 시작하고 테스트 환경을 설정하려는 경우에도 스크립트를 사용하여 CORP 도메인을 구성할 수 있습니다. $env:SYSTEMDRIVE\PAM 폴더에 압축 파일의 압축을 풀고 CORP 포리스트의 세부 정보를 추가하여 PAMDeploymentConfig.xml을 편집합니다. DNS 이름, Netbios 이름, DC 이름, 데이터베이스/로그 경로 및 Sysvol 경로를 업데이트합니다. 기능 수준은 Windows Server 2012 R2 이상이어야 합니다.

1. 관리자 권한으로 CORP 도메인 DC에 로그인합니다.
2. 관리자 권한으로 PowerShell을 실행합니다.
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. 메뉴 옵션 10(CORP 포리스트 설정)을 선택합니다.

완료되면 도메인 컨트롤러가 자동으로 다시 부팅됩니다.

## 부록 3 유효성 검사를 수행하기 위해 CORP 클라이언트 설치

구성 파일의 ClientBinaryLocation은 setup.exe가 있는 위치를 가리켜야 합니다.
로컬 관리자로 클라이언트에 로그인하여 관리자 권한 PowerShell 창에서 다음 명령을 실행합니다.

1. cd $env:SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. 메뉴 옵션 7(MIM PAM 클라이언트 설치)를 선택합니다.


컴퓨터가 도메인에 가입되지 않은 경우 도메인 가입을 수행하기 위해 CORP 관리자 자격 증명을 묻는 메시지가 표시됩니다. 도메인 가입 후 컴퓨터를 다시 부팅해야 합니다. 다시 로컬 관리자로 클라이언트에 로그인하여 관리자 권한 PowerShell 창에서 다음 명령을 실행합니다.

1. cd $env:SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. 메뉴 옵션 7(MIM PAM 클라이언트 설치)를 선택합니다.

위에 제공된 8단계를 진행합니다.

## 부록 4 문제가 발생한 경우

모든 스크립트 로그는 %AppData%\MIMPAMInstall에 저장됩니다. 폴더를 Zip 파일로 압축하여 작업 및 오류에 대한 세부 정보와 함께 전자 메일([mim2016@microsoft.com](mim2016@microsoft.com))로 보내 주세요.



<!--HONumber=Sep16_HO4-->

