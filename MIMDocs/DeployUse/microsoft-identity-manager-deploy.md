---
# required metadata

title: MIM 2016 배포 | Microsoft Identity Manager
description: 환경 준비부터 포털 구성까지 Microsoft Identity Manager 2016 배포와 관련된 전체 단계 목록을 확인합니다.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# MIM 2016 배포
이 섹션의 문서에서는 이전에 FIM 또는 MIM을 배포하지 않은 새 서버에서 최종 사용자 셀프 서비스 시나리오를 위해 MIM(Microsoft Identity Manager) 2016을 배포하는 단계별 지침을 제공합니다.

> [!NOTE]
> 이 섹션에서는 MIM을 시작하고 MIM에 대해 알아보기 위한 용도로만 배포 토폴로지를 설명합니다.  [용량 계획 가이드](/microsoft-identity-manager/PlanDesign/capacity-planning-guide) 는 프로덕션 배포를 위한 토폴로지에 대해 자세한 정보를 제공합니다.  프로덕션 규모 또는 사용을 위해 MIM을 배포하기 전에 해당 문서를 검토하는 것이 좋습니다.

<!---
Comment: Restore after PAM content is included

The privileged access management scenario is deployed differently than other MIM scenarios, as it requires a dedicated bastion forest environment.  If you want to learn more about deploying MIM for Privileged Identity Management, see [Getting Started with Privileged Access Management](privileged-access-management-get-started.md).
--->

## 먼저 첫 번째 도메인을 준비합니다.
MIM은 AD(Active Directory)와 함께 작동하므로 다음 단계에 따라 AD 도메인 컨트롤러를 구성합니다.
- [도메인 설정](preparing-domain.md)

## 다음으로, ID 관리 서버를 준비합니다.
도메인이 배치되고 구성되면 회사 ID 관리 서버를 준비합니다. 여기에는 다음 설치가 포함됩니다.
- [서버 설치: Windows Server 2012 R2](prepare-server-ws2012r2.md)
- [서버 설치: SQL Server 2014](prepare-server-sql2014.md)
- [서버 설치: SharePoint](prepare-server-sharepoint.md)
- [서버 설치: Exchange Server](prepare-server-exchange.md)(선택 사항)

## 마지막으로 Microsoft Identity Manager 2016 구성 요소를 설치합니다.
도메인과 서버를 설정하면 MIM 구성 요소를 설치하고 AD와 동기화하도록 구성할 준비가 된 것입니다.
- [MIM 설치: MIM 동기화 서비스](install-mim-sync.md)
- [MIM 설치: MIM 서비스 및 포털](install-mim-service-portal.md)
- [MIM 설치: Active Directory와 MIM 서비스 동기화](install-mim-sync-ad-service.md)


<!--HONumber=Apr16_HO2-->


