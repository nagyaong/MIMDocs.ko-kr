---
title: "1단계 PRIV 도메인 구성"
description: "스크립트를 사용하여 Privileged Identity Manager에서 관리할 기존 또는 새 ID로 CORP 도메인을 준비합니다."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: 24e91ed2f51206b03bec505fc0d28d25128d2c94
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# <a name="step-1-configuring-the-priv-domain"></a>1단계 PRIV 도메인 구성

>[!div class="step-by-step"]
[2단계 »](sp1-step2-configuring-corp-domain.md)

1. 관리자 권한으로 PRIVDC에 로그인합니다.
  * PRIV 전용 환경인 경우 CORPDC에 로그인합니다.
2. 관리자 권한으로 PowerShell을 실행합니다.
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. 메뉴 옵션 1(PRIV 포리스트 구성)을 선택합니다.


SQL/SharePoint 및 MIM을 관리하는 데 필요한 서비스 계정이 자동으로 생성됩니다(도메인에 아직 없는 경우). 스크립트를 실행하는 동안 이러한 서비스 계정을 만들기 위해 암호를 입력하라는 메시지가 표시됩니다.
PRIV 도메인이 Windows Server 2016이고 기능 수준이 Windows Server 2016 Technical Preview 5로 설정된 경우 PAM에 필요한 선택적 Active Directory ‘Privileged Access Management 기능’을 사용하도록 설정할지 묻는 메시지가 표시됩니다. ‘예’를 확인하여 계속 진행합니다.
Windows Server 2016 아래의 기능 수준에 대해, 추가 구성이 수행되지 않는다는 경고를 해제합니다. 관리자가 Windows Server 2016으로 기능 수준을 올리고 나면 PAMDeployment.ps1 및 PAM 포리스트 구성을 다시 실행해야 합니다.

>[!NOTE]
>다음 단계는 PRIVOnly 구성에 필요하지 않습니다.

$env:SYSTEMDRIVE\PAM에 생성된 SIDs.txt를 CORPDC의 유사한 폴더에 복사합니다. 이는 CORPDC에서 PRIV 사용자가 CORP 사용자 속성을 읽을 수 있는 권한을 설정하는 데 필요합니다.
스크립트가 완료되면 변경 내용을 적용하기 위해 컴퓨터를 다시 부팅하라는 메시지가 나타납니다.

>[!div class="step-by-step"]
[2단계 »](sp1-step2-configuring-corp-domain.md)
