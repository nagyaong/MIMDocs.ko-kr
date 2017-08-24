---
title: "4단계 SharePoint 구성"
description: "스크립트를 사용한 PAM 구성의 4단계입니다. 이 단계에서는 PAM 배포의 일부로 사용할 수 있도록 SharePoint를 구성합니다."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: f8d033bec440c6efed26dd959acc713638258dd3
ms.sourcegitcommit: 8edd380f54c3e9e83cfabe8adfa31587612e5773
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/19/2017
---
# <a name="step-4-configuring-sharepoint"></a>4단계 SharePoint 구성

>[!div class="step-by-step"]
[« 3단계](sp1-step3-installing-configuring-sql.md)
[5단계 »](sp1-step5-configuring-pam.md)

SharePoint는 SharePoint Foundation 2013 SP1이어야 합니다.

도메인에 가입된 서버의 경우 MIMAdmin으로 로그인합니다.

1. 관리자 권한으로 PowerShell을 실행합니다.
2.  .\PAMDeployment.ps1
3.  메뉴 옵션 4(SharePoint 설치)를 선택합니다.


작업 그룹 서버의 경우

1. 관리자 권한으로 PowerShell을 실행합니다.
2.  cd $env:SYSTEMDRIVE\PAM
3.  .\PAMDeployment.ps1
4. 메뉴 옵션 4(SharePoint 설치)를 선택합니다.

SharePoint를 설치할 때 컴퓨터가 여러 번 다시 부팅됩니다. 다시 부팅될 때마다 SharePoint 설치를 다시 실행해야 하며 MIMAdmin 계정으로 로그인해야 합니다.
SharePoint를 설치하는 컴퓨터가 인터넷에 연결되어 있지 않아 필수 구성 요소를 다운로드할 수 없는 경우 독립적으로 다운로드하여 로컬 폴더에 저장할 수 있습니다. **<PrerequisitesBinaryLocation/>의 PAMConfiguration.xml 파일에서 이 로컬 폴더 경로를 업데이트해야 합니다.** 파일을 다운로드할 수 있는 링크는 부록 5를 참조하세요.
설치가 끝나면 SharePoint 구성 GUI가 열리고 SharePoint 설치를 완료하는 단계를 안내합니다. 서버 완료를 선택하고 UI의 나머지 과정을 진행합니다. 설치가 끝나면 구성 마법사를 실행할지 묻는 메시지가 표시됩니다. 아래 제공된 단계를 완료합니다.

1. **서버 팜에 연결** 탭에서 **새 서버 팜 만들기**로 변경합니다.
2. **SQLServer**를 구성 데이터베이스에 대한 데이터베이스 서버로 지정하고 SharePoint에서 사용할 데이터베이스 액세스 계정으로 **SharePoint ServiceAccount**를 지정합니다.
3. 암호를 팜 보안 암호로 지정합니다**(나중에 사용되지 않음)**.
4. SharePoint 구성 마법사의 나머지 기본 설정을 사용하여 단일 서버 팜을 만듭니다.

세부 정보는 [3단계 - PAM 서버 준비](/microsoft-identity-manager/pam/step-3-prepare-pam-server)의 **SharePoint 구성** 섹션에서 확인할 수 있습니다. 완료되면 “.\PAMDeployment.ps1” 스크립트를 다시 실행하고 옵션 4(SharePoint 설치)를 선택하여 이 단계를 완료합니다.

>[!div class="step-by-step"]
[« 3단계](sp1-step3-installing-configuring-sql.md)
[5단계 »](sp1-step5-configuring-pam.md)
