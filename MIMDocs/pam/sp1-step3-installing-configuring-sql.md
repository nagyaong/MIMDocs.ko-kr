---
title: "3단계 SQL 구성"
description: "스크립트를 사용하여 Privileged Identity Manager에서 관리할 기존 또는 새 ID로 CORP 도메인을 준비합니다."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 10/25/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 365989693f844f117f76ee2b69db85df82f06f35
ms.openlocfilehash: 375a34e5255c90559fc0ffb3a80fc7c92ebd27a2


---
# <a name="step-3-configuring-sql"></a>3단계 SQL 구성

>[!div class="step-by-step"]
[« 2단계](sp1-step2-configuring-corp-domain.md)
[4단계 »](sp1-step4-configuring-sharepoint.md)

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

>[!div class="step-by-step"]
[« 2단계](sp1-step2-configuring-corp-domain.md)
[4단계 »](sp1-step4-configuring-sharepoint.md)



<!--HONumber=Nov16_HO2-->


