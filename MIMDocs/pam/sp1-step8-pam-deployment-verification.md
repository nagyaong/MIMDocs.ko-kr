---
title: 8단계 PAM 배포 확인
description: 스크립팅된 PAM 배포에는 PAM 시나리오를 실행하여 PAM 배포가 예상대로 작동하는지 확인할 수 있는 확인 스크립트가 포함되어 있습니다.
keywords: ''
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 53ffcd4832ded5dce0878715cc716559c3883d6a
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332989"
---
# <a name="step-8-pam-deployment-verification"></a>8단계 PAM 배포 확인

> [!div class="step-by-step"]
> [« 7단계](sp1-step7-setup-sidhistory-sidfiltering.md)
> [추록 »](sp1-pam-deployment-addendum.md)

PAM 시나리오를 실행하여 PAM 배포가 예상대로 작동하는지 확인할 수 있는 확인 스크립트가 배포 패키지에 들어 있습니다.
배포 확인을 사용하려면 PAMDeploymentConfig.xml 섹션(<PamValidation/>)을 수정합니다.

>[!NOTE]
>유효성 검사를 수행하려면 클라이언트 컴퓨터 도메인이 PAM 클라이언트 쪽 구성 요소가 설치된 CORP 도메인에 가입되어 있어야 합니다. 클라이언트 설치 방법에 대한 스크립트는 부록을 참조하세요.

PAMDeploymentConfig.xml의 <PAMValidationClient/> 태그에서 클라이언트 컴퓨터 이름을 업데이트해야 합니다. <PAMValidation/> 노드의 나머지 데이터는 이 유효성 검사에서 새로 생성되므로 기존 사용자/그룹과 충돌하는 경우에만 편집하면 됩니다.
다음 단계를 사용하여 유효성 검사를 수행합니다.

1단계:

1. CORP 도메인 관리자로 CORPDC에 로그인합니다.
2. 관리자 권한으로 PowerShell을 실행합니다.
3. cd $env:SYSTEMDRIVE\PAM
4. Import-module .\PAMValidation.psm1
5. Create-PAMValidationonCORPDCConfig

유효성 검사에 필요한 그룹 및 사용자가 생성됩니다.

2단계:

1. MIMAdmin으로 PAM 서버에 로그인합니다.
2. 관리자 권한으로 PowerShell을 실행합니다.
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. move-PAMVAlidationUsersToPAM

이 단계에서는 사용자 및 그룹을 PAM 환경으로 마이그레이션합니다.

3단계:

1. 로컬 관리자로 CORP 클라이언트에 로그온합니다.
2. 관리자 권한으로 PowerShell을 실행합니다.
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. Enable-PAMUsersCORPClientRemote


이 단계에서는 CORPAdmin 자격 증명을 묻는 메시지를 표시합니다. 자격 증명을 제공하면 필요한 사용자가 ‘Remote Desktop Users’ 및 ‘Remote Management Users’ 그룹에 추가됩니다.
CORP 클라이언트에서 다음 명령을 사용하여 유효성을 검사할 PRIV 사용자로 PowerShell을 엽니다. </br></br>
**Runas /u:<PRIV domain>\PRIV.pamRequestor powershell.exe**  </br></br>
PowerShell 창에서 다음을 입력합니다.

1. cd $env:SYSTEMDRIVE\PAM
2. import-module .\PAMValidation.psm1
3. test-PAMValidationScenarioNoApprovalRequest


  요청 상태가 표시됩니다.
  처음에는 사용자에게 리소스에 대한 액세스 권한이 없습니다. 사용자를 역할에 JIT(Just-In-Time) 추가하면 사용자에게 권한이 부여됩니다. 요청 기간이 만료되면 사용자의 액세스 권한이 사라집니다.
  이 스크립트에서는 만료할 요청에 기본값(11분)을 사용합니다.

> [!div class="step-by-step"]
> [« 7단계](sp1-step7-setup-sidhistory-sidfiltering.md)
> [추록 »](sp1-pam-deployment-addendum.md)
