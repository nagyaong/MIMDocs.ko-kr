---
title: 3단계 SQL 구성
description: 이 문서는 스크립트를 사용하여 권한 있는 ID 관리자를 구성하는 방법을 설명하는 일련의 문서 중 3단계로, SQL Server 구성 단계를 설명합니다.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 69f86c5366f7b662f3a11fa4ac8d44159421f909
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379400"
---
# <a name="step-3-configuring-sql"></a>3단계 SQL 구성

> [!div class="step-by-step"]
> [« 2단계](sp1-step2-configuring-corp-domain.md)
> [4단계 »](sp1-step4-configuring-sharepoint.md)

아래 단계를 계속 진행하기 전에 SQL Server 2012 SP1 이상 또는 SQL server 2014를 사용하고 있는지 확인합니다. 도메인에 가입된 서버의 경우 MIMAdmin 계정을 사용하여 로그인하고, 그렇지 않으면 로컬 관리자로 로그인합니다.
1. 관리자 권한으로 PowerShell을 실행합니다.
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. 메뉴 옵션 3(SQL Server 설치)을 선택합니다.

   서버가 PRIV 도메인에 아직 가입되어 있지 않은 경우 자격 증명을 묻고 서버를 도메인에 가입하라는 메시지가 나타납니다.
   도메인 가입하면 컴퓨터가 다시 부팅됩니다. 다시 부팅이 완료되면 MIMAdmin 계정으로 서버에 로그온합니다.

5. 관리자 권한으로 PowerShell을 실행합니다.
6. cd $env:SYSTEMDRIVE\PAM
7. .\PAMDeployment.ps1
8. 메뉴 옵션 3(SQL Server 설치)을 선택합니다.

메시지가 나타나면 MIMAdmin 서비스 계정의 암호를 제공하고 설치를 계속합니다. 완료되면 4단계로 이동합니다.

> [!div class="step-by-step"]
> [« 2단계](sp1-step2-configuring-corp-domain.md)
> [4단계 »](sp1-step4-configuring-sharepoint.md)
