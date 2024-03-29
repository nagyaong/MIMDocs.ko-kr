---
title: 6단계 PAM 트러스트 설정
description: 스크립트를 사용한 PAM 구성의 6단계입니다. 이 섹션에서는 corp 및 priv 도메인 간에 필요한 트러스트를 설정하는 방법을 설명합니다.
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
ms.openlocfilehash: ad1482718693c9ae7004a71334013de68f7c20da
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379382"
---
# <a name="step-6-set-up-the-pam-trust"></a>6단계 PAM 트러스트 설정

> [!div class="step-by-step"]
> [«5 단계](sp1-step5-configuring-pam.md)
> [7 단계»](sp1-step7-setup-sidhistory-sidfiltering.md)

**PRIV 전용 환경에서는 이 단계를 수행할 필요가 없습니다.** MIMAdmin 계정으로 PAM 서버에 로그인합니다.

1. MIMAdmin 계정으로 PAM 서버에 로그인합니다.
2. 관리자 권한으로 PowerShell을 실행합니다.
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. 메뉴 옵션 6(PAM 트러스트 설정)을 선택합니다.

   메시지가 나타나면 CORP 관리자 계정의 자격 증명을 입력합니다. 자격 증명을 제공하면 트러스트가 설정되고 구성이 완료됩니다.

> [!div class="step-by-step"]
> [«5 단계](sp1-step5-configuring-pam.md)
> [7 단계»](sp1-step7-setup-sidhistory-sidfiltering.md)
