---
title: "Microsoft Identity Manager 2016을 배포하는 데 필요한 단계 | Microsoft 문서"
description: "환경 준비부터 포털 구성까지 Microsoft Identity Manager 2016 배포와 관련된 전체 단계 목록을 확인합니다."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/27/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3623bffb099a83d0eba47ba25e9777c3d590e529
ms.openlocfilehash: 804f957d5cf9f5fb09efa65944983b1d5013fff3
ms.lasthandoff: 01/24/2017


---

# <a name="deploy-mim-2016"></a>MIM 2016 배포
이 섹션의 문서에서는 이전에 FIM 또는 MIM을 배포하지 않은 새 서버에서 최종 사용자 셀프 서비스 시나리오를 위해 MIM(Microsoft Identity Manager) 2016을 배포하는 단계별 지침을 제공합니다.

> [!NOTE]
> 이 섹션에서는 MIM을 시작하고 MIM에 대해 알아보기 위한 용도로만 배포 토폴로지를 설명합니다.  [용량 계획 가이드](/microsoft-identity-manager/plan-design/capacity-planning-guide)는 프로덕션 배포를 위한 토폴로지에 대해 자세한 정보를 제공합니다.  프로덕션 규모 또는 사용을 위해 MIM을 배포하기 전에 해당 문서를 검토하는 것이 좋습니다.

권한 있는 액세스 관리 시나리오는 전용 요새 포리스트 환경이 필요하므로 다른 MIM 시나리오와는 다르게 배포됩니다.  Privileged Identity Management를 위해 MIM을 배포하는 방법에 대한 자세한 내용은 [Privileged Access Management를 위해 MIM 환경 구성](/microsoft-identity-manager/pam/configuring-mim-environment-for-pam)을 참조하세요.

MIM 2016 배포를 위한 프로세스는 이전 버전인 FIM 2010 R2의 프로세스와 매우 유사합니다. FIM 설명서를 참조하려는 경우 [Forefront Identity Manager 2010 R2 배포 가이드](https://technet.microsoft.com/library/jj134310)를 확인하세요.

## <a name="first-prepare-a-domain"></a>먼저 첫 번째 도메인을 준비합니다.
MIM은 AD(Active Directory)와 함께 작동하므로 다음 단계에 따라 AD 도메인 컨트롤러를 구성합니다.
- [도메인 설정](preparing-domain.md)

## <a name="next-prepare-an-identity-management-server"></a>다음으로, ID 관리 서버를 준비합니다.
도메인이 배치되고 구성되면 회사 ID 관리 서버를 준비합니다. 여기에는 다음 설치가 포함됩니다.
- [Windows Server 2012 R2](prepare-server-ws2012r2.md)
- [SQL Server 2014](prepare-server-sql2014.md)
- [SharePoint](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md)(선택 사항)

## <a name="finally-install-microsoft-identity-manager-2016-components"></a>마지막으로 Microsoft Identity Manager 2016 구성 요소를 설치합니다.
도메인과 서버를 설정하면 MIM 구성 요소를 설치하고 AD와 동기화하도록 구성할 준비가 된 것입니다.
- [MIM 동기화 서비스](install-mim-sync.md)
- [MIM 서비스 및 포털](install-mim-service-portal.md)
- [Active Directory와 MIM 서비스 동기화](install-mim-sync-ad-service.md)

