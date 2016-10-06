---
title: "2단계 CORP 도메인 구성"
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
ms.openlocfilehash: 7919fa3fdbb6613fbfa636ddb34762faa85925b9


---

# 2단계 CORP 도메인 구성

SIDs.txt를 CORPDC에 복사한 후(**PRIVOnly 배포에는 필요하지 않음**)

1. 관리자 권한으로 CORPDC에 로그인합니다.
2. 관리자 권한으로 PowerShell을 실행합니다.
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. 메뉴 옵션 2(CORPDC 포리스트 구성)를 선택합니다.



<!--HONumber=Sep16_HO4-->


