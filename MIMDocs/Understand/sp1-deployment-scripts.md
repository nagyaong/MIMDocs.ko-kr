---
title: "MIM2016 SP1 PAM 배포 스크립트"
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
ms.sourcegitcommit: c7c5266f3d1c51e933855031f4128cbcb967d6e2
ms.openlocfilehash: 43a176ed2f1375eb98851064c460515ec09de132


---

# MIM2016 SP1 PAM 배포 스크립트

이 서비스 팩에는 PAM 배포를 보다 쉽게 수행할 수 있도록 배포 스크립트 집합이 도입되었습니다. 이러한 스크립트는 다운로드 센터에서 사용할 수 있습니다. 스크립트를 사용하기 전에 아래의 가정이 사용자 환경에 적용되는지 확인해야 합니다.

중요한 가정:
1. 모든 컴퓨터의 운영 체제는 Windows Server 2012 R2 이상입니다. Windows Server 2016 Technical Preview 5를 사용하려면 TP5 빌드와 함께 PRIV 도메인 컨트롤러를 설치해야 합니다.
2. 도메인 컨트롤러와 구성 요소 서버 간의 이름 확인이 자동으로 수행되도록 DNS를 구성해야 합니다.
3. SQL, SharePoint 및 MIM 설치를 위해 지정된 서버에서 로컬로 설치 바이너리를 사용할 수 있어야 합니다.
4. 환경에 CORPDC, PRIVDC 및 PAM 서버를 독립적으로 실행하는 전용(실제 또는 가상) 컴퓨터 3대가 있습니다.
5. 유효성 검사 옵션의 경우 이 단계를 실행할 전용 클라이언트 컴퓨터가 있는 것으로 가정합니다.

>[!NOTE] 스크립트를 실행하는 데 문제가 있는 경우 로그를 확인해야 할 수 있습니다. 모든 스크립트 로그는 %AppData%\MIMPAMInstall에 저장됩니다. 폴더를 Zip 파일로 압축하여 작업 및 오류에 대한 세부 정보와 함께 전자 메일(mim2016@microsoft.com)로 보내 주세요.



<!--HONumber=Sep16_HO4-->


