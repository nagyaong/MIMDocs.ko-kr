---
title: "6단계 PAM 트러스트 설정"
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
ms.openlocfilehash: 5bcf4f4ef201236746ec1bf75c1c8900841a6c79


---

# <a name="step-6-set-up-the-pam-trust"></a>6단계 PAM 트러스트 설정

>[!div class="step-by-step"]
[«5 단계](sp1-step5-configuring-pam.md)
[7 단계»](sp1-step7-setup-sidhistory-sidfiltering.md)

**PRIV 전용 환경에서는 이 단계를 수행할 필요가 없습니다.** MIMAdmin 계정으로 PAM 서버에 로그인합니다.

1. MIMAdmin 계정으로 PAM 서버에 로그인합니다.
2. 관리자 권한으로 PowerShell을 실행합니다.
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. 메뉴 옵션 6(PAM 트러스트 설정)을 선택합니다.

  메시지가 나타나면 CORP 관리자 계정의 자격 증명을 입력합니다. 자격 증명을 제공하면 트러스트가 설정되고 구성이 완료됩니다.

>[!div class="step-by-step"]
[«5 단계](sp1-step5-configuring-pam.md)
[7 단계»](sp1-step7-setup-sidhistory-sidfiltering.md)



<!--HONumber=Nov16_HO2-->


