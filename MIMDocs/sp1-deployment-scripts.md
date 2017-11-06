---
title: "MIM2016 SP1 PAM 배포 스크립트"
description: "이 페이지는 스크립트를 사용한 권한 있는 ID 관리자 구성을 다루는 일련의 문서 중 일부입니다. 여기에는 환경에 대한 가정 목록이 포함되어 있습니다."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 10/17/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: 77a222c0a36f4e244a5114eddfc0edadb168d1cd
ms.sourcegitcommit: 06add1a636720f74bc0c0f25b4100b19f1bd31da
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2017
---
# <a name="mim2016-sp1-pam-deployment-scripts"></a>MIM2016 SP1 PAM 배포 스크립트

이 서비스 팩에는 PAM 배포를 보다 쉽게 수행할 수 있도록 배포 스크립트 집합이 도입되었습니다. 이러한 스크립트는 다운로드 센터에서 사용할 수 있습니다. 스크립트를 사용하기 전에 아래의 요구 사항을 충족하는지 확인해야 합니다.

1. 모든 서버의 운영 체제는 Windows Server 2012 R2 이상입니다.
2. 도메인 컨트롤러와 구성 요소 서버 간에 이름 확인을 사용하도록 DNS를 구성해야 합니다.
3. SQL, SharePoint 및 MIM 설치를 위해 지정된 서버에서 로컬로 설치 바이너리를 사용할 수 있어야 합니다.
4. 환경에 CORPDC, PRIVDC 및 PAM 서버를 독립적으로 실행하는 전용(실제 또는 가상) 컴퓨터 3대가 있습니다.
5. 유효성 검사 옵션에는 전용 워크스테이션이 필요합니다.

>[!NOTE]
>스크립트를 실행하는 데 문제가 있는 경우 로그를 확인해야 할 수 있습니다. 모든 스크립트 로그는 %AppData%\MIMPAMInstall에 저장됩니다. 폴더를 Zip 파일로 압축하여 지원 사례에서 작업 및 오류에 대한 세부 정보와 함께 이를 포함하세요.

PAM 배포 스크립트를 통해 시작할 준비가 되셨나요? [스크립트를 사용하여 PAM 구성](./pam/sp1-pam-configure-using-scripts.md)
