---
title: "7단계 SID 기록/SID 필터링 설정"
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
ms.openlocfilehash: a98d83a22c61ef534fcc02725e4cd500be10cc8a


---

# <a name="step-7-set-up-sid-historysid-filtering"></a>7단계 SID 기록/SID 필터링 설정

>[!div class="step-by-step"]
[« 6단계](sp1-step6-setup-pam-trust.md)
[8 단계»](sp1-step8-pam-deployment-verification.md)

**PRIV 전용 환경에서는 이 단계를 수행할 필요가 없습니다.** MIMAdmin 계정으로 PAM 서버에 로그인합니다.

1. 관리자 권한으로 CORP DC에 로그인합니다.
2. 관리자 권한으로 PowerShell을 실행합니다.
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. 메뉴 옵션 8(SID 기록/SID 필터링 설정)을 선택합니다.

실행을 완료하면 다음과 같은 메시지가 표시됩니다.<br/></br>
SID 필터링의 경우: <br/></br>
“SID를 필터링하지 않을 트러스트를 설정하는 중입니다.” 또는 “이 트러스트에 대해 SID 필터링을 사용할 수 없습니다.” </br></br>
SID 기록의 경우: </br></br>
“이 트러스트에 대해 SID 기록을 사용하도록 설정하는 중입니다.” 또는 “이 트러스트에 대해 SID 기록을 이미 사용할 수 있습니다.”

>[!div class="step-by-step"]
[« 6단계](sp1-step6-setup-pam-trust.md)
[8 단계»](sp1-step8-pam-deployment-verification.md)



<!--HONumber=Nov16_HO2-->


