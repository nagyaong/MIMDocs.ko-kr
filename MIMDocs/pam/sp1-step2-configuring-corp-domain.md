---
title: "2단계 CORP 도메인 구성"
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
ms.openlocfilehash: b7acc475c505b29559c510fd3fa350fed1c3157b


---

# <a name="step-2-configuring-the-corp-domain"></a>2단계 CORP 도메인 구성

>[!div class="step-by-step"]
[« 1단계](sp1-step1-configuring-priv-domain.md)
[3단계 »](sp1-step3-installing-configuring-sql.md)

SIDs.txt를 CORPDC에 복사한 후(**PRIVOnly 배포에는 필요하지 않음**)

1. 관리자 권한으로 CORPDC에 로그인합니다.
2. 관리자 권한으로 PowerShell을 실행합니다.
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. 메뉴 옵션 2(CORPDC 포리스트 구성)를 선택합니다.

>[!div class="step-by-step"]
[« 1단계](sp1-step1-configuring-priv-domain.md)
[3단계 »](sp1-step3-installing-configuring-sql.md)



<!--HONumber=Nov16_HO2-->


