---
title: "스크립트를 사용하여 PAM 구성"
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
ms.sourcegitcommit: 96c734ade75f5c206858387cf45106761bc0a881
ms.openlocfilehash: a1e4e5561bf8d38c56c3d27249d94f4bf7103b8c


---

# 스크립트를 사용하여 PAM 구성

SQL과 SharePoint를 별도의 서버에 설치하려면 아래 지침에 따라 서버를 구성해야 합니다. SQL, SharePoint 및 PAM 구성 요소가 동일한 컴퓨터에 설치된 경우 해당 컴퓨터에서 아래 단계를 실행해야 합니다.

아래 단계에서는 PRIV 도메인이 이미 설정되어 있는 것으로 가정합니다. PRIV 도메인 구성 지침은 이 문서의 끝에 있는 부록을 참조하세요.

단계:

1. 모든 컴퓨터의 %SYSTEMDRIVE%\PAM 폴더에 압축 파일 “PAMDeploymentScripts.zip”의 압축을 풉니다.
2. 컴퓨터 중 하나에서 **PAMDeploymentConfig.xml** 파일을 열고 아래 차트 또는 XML 파일 자체 내의 지침을 사용하여 세부 정보를 업데이트합니다. PRIV 및 CORP 포리스트가 이미 설치된 경우 **DNSName** 및 **NetbiosName**만 업데이트하면 됩니다.
3. Roles 섹션에서 SQL, SharePoint 및 MIM 역할에 대한 **service account**, **machine details** 및 **location of the installation binaries**를 업데이트합니다.
    1. MIM 바이너리 위치는 “Service and Portal” 폴더가 포함된 디렉터리를 가리켜야 합니다. Client 바이너리 위치는 “Add-ins and Extensions.msi”가 포함된 디렉터리를 가리켜야 합니다.

4. PRIVOnly 환경인 경우 PRIVOnly 태그를 True로 설정해야 합니다.
    1. PRIVOnly 환경의 경우 PRIV 도메인의 **DNSName** 및 **NetbiosName**을 CORP 도메인과 일치하도록 업데이트합니다. 기본 템플릿 파일에서는 PRIV 및 CORP 구성을 가정하므로 컴퓨터 접미사가 SQL, SharePoint 및 MIM을 설치할 컴퓨터에 올바른지 확인합니다.
    2. PRIVOnly 환경에 대한 자세한 내용을 보려면 여기를 클릭합니다.

5. 모든 컴퓨터, CORPDC, PRIVDC, PAM 서버, SQL Server 및 SharePoint Server의 %SYSTEMDRIVE%\PAM 폴더에 동일한 PAMDeploymentConfig.xml을 복사합니다.


## 배포 워크시트

PAMDeploymentConfig.xml 업데이트를 계속 진행하기 전에 업데이트된 복사본을 모든 컴퓨터에 배치합니다.

### Setup

|컴퓨터   | 실행 권한   |명령   |
|---|---|---|
|  PRIVDC |PRIV 도메인 관리자   | .\PAMDeployment.ps1 메뉴 옵션 1(PRIV 포리스트 구성) 선택   |
|   |   |  위 단계에서는 SIDs.txt를 생성합니다. 다음 단계를 실행하기 전에 이 파일을 CORPDC의 $envDrive:PAM에 복사해야 합니다. |
| CORPDC  |CORPDC 도메인 관리자   | .\PAMDeployment.ps1 메뉴 옵션 2(CORP 포리스트 구성) 선택   |
| PAMServer(또는 SQL Server)   |CORPDC 도메인 관리자   |  .\PAMDeployment.ps1 메뉴 옵션 2(CORP 포리스트 구성) 선택  |
|  PAMServer |  로컬 관리자(도메인 가입 후 MIM 관리자) |  .\PAMDeployment.ps1 메뉴 옵션 4(SharePoint 설치) 선택  |
| PAMServer  | 로컬 관리자(도메인 가입 후 MIM 관리자)  | .\PAMDeployment.ps1 메뉴 옵션 5(MIM PAM 설치) 선택   |
|  PAMServer |MIM 관리자   | .\PAMDeployment.ps1 메뉴 옵션 6(PAM 트러스트 설정) 선택.\PAMDeployment.ps1 메뉴 옵션 6 (PAM 트러스트 설정) |

### 유효성 검사

|  컴퓨터 | 실행 권한   | 명령   |
|---|---|---|
| CORPClient  | CORP 사용자(로컬 관리자)  |   .\PAMDeployment.ps1 메뉴 옵션 7(MIM PAM 클라이언트 설치) 선택  |
| CORPDC  | CORPDC 도메인 관리자   | Import-module .\PAMValidation.psm1, Create-PAMValidationCORPDCConfig   |
| PAMServer   | MIM 관리자  | Import-module .\PAMValidation.psm1, Move-PAMValidationUsersToPAM  |
| CORPClient  | CORP 사용자(로컬 관리자)   |   Import-module .\PAMValidation.psm1, Enable-PAMUsersCORPClientRemote |
|  CORPClient | <PRIV>\PRIV.pamRequestor 사용자 및 PRIVOnly의 경우 <CORP>\pamrequestor   | Import-module .\PAMValidation.psm1, Test-PAMValidationScenarioNoApprovalRequest  |



<!--HONumber=Sep16_HO4-->

