---
title: 2단계 CORP 도메인 구성
description: 이 문서에서는 corp 도메인을 구성하는 데 필요한 두 번째 단계를 설명합니다. 이 단계에는 sids.txt를 CORPDC에 복사한 후 스크립트를 실행하는 작업이 포함됩니다.
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
ms.openlocfilehash: 030ebf1f5d655cff712aac8acc393e7d3cc13696
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379484"
---
# <a name="step-2-configuring-the-corp-domain"></a>2단계 CORP 도메인 구성

> [!div class="step-by-step"]
> [« 1단계](sp1-step1-configuring-priv-domain.md)
> [3단계 »](sp1-step3-installing-configuring-sql.md)

SIDs.txt를 CORPDC에 복사한 후(**PRIVOnly 배포에는 필요하지 않음**)

1. 관리자 권한으로 CORPDC에 로그인합니다.
2. 관리자 권한으로 PowerShell을 실행합니다.
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. 메뉴 옵션 2(CORPDC 포리스트 구성)를 선택합니다.

> [!div class="step-by-step"]
> [« 1단계](sp1-step1-configuring-priv-domain.md)
> [3단계 »](sp1-step3-installing-configuring-sql.md)
