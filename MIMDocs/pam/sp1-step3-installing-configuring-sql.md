---
title: "3단계 SQL 구성"
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
ms.openlocfilehash: a7d456b1c2baf31ef2d7ca801a567cf42eaef52e


---
# 3단계 SQL 구성

아래 단계를 계속 진행하기 전에 SQL Server 2012 SP1 이상 또는 SQL server 2014를 사용하고 있는지 확인합니다. 도메인에 가입된 서버의 경우 MIMAdmin 계정을 사용하여 로그인하고, 그렇지 않으면 로컬 관리자로 로그인합니다.
1. 관리자 권한으로 PowerShell을 실행합니다.
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. 메뉴 옵션 3(SQL Server 설치)을 선택합니다.

  서버가 PRIV 도메인에 아직 가입되어 있지 않은 경우 자격 증명을 묻고 서버를 도메인에 가입하라는 메시지가 나타납니다.
  도메인 가입하면 컴퓨터가 다시 부팅됩니다. 다시 부팅이 완료되면 MIMAdmin 계정으로 서버에 로그온합니다.

1. 관리자 권한으로 PowerShell을 실행합니다.
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. 메뉴 옵션 3(SQL Server 설치)을 선택합니다.

메시지가 나타나면 MIMAdmin 서비스 계정의 암호를 제공하고 설치를 계속합니다. 완료되면 4단계로 이동합니다.



<!--HONumber=Sep16_HO4-->


