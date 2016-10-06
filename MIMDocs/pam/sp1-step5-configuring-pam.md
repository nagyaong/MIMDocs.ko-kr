---
title: "5단계 PAM 설치/구성"
description: "스크립트를 사용하여 Privileged Identity Manager에서 관리할 기존 또는 새 ID로 CORP 도메인을 준비합니다."
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/26/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 689c2ef0e4e4a681a398ba7e94fb3def525937ea
ms.openlocfilehash: a5d86991f1579f292d7d303148422cef746d008a


---
# 5단계 PAM 설치/구성

도메인에 가입된 PAM 서버의 경우 MIMAdmin으로 로그인하고, 그렇지 않으면 로컬 관리자로 로그인합니다.
1. 관리자 권한으로 PowerShell을 실행합니다.
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeploymnet.ps1
4. 메뉴 옵션 5(MIM PAM 설치)를 선택합니다.

>[!NOTE] 컴퓨터가 PRIV 도메인에 아직 가입되지 않은 경우 자격 증명을 묻는 메시지가 나타납니다. 도메인 가입하면 컴퓨터가 다시 부팅됩니다.

PAM 서버를 다시 부팅한 후 MIMAdmin 계정으로 컴퓨터에 다시 로그인합니다.

1. 관리자 권한으로 PowerShell을 실행합니다.
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. 메뉴 옵션 5(MIM PAM 설치)를 선택합니다.

  메시지가 나타나면 MIM 모니터 계정, MIM 구성 요소 계정, MIM MA 계정, MIM 서비스 계정, MIM 관리자 계정 및 SharePoint 계정의 암호를 입력합니다.
  설치가 완료되면 컴퓨터가 다시 부팅됩니다.



<!--HONumber=Sep16_HO4-->


