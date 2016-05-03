---
# required metadata

title: ID 관리 서버 설치&#58; Exchange | Microsoft Identity Manager
description: 선택적 단계로, MIM 2016에서 메일을 보내고 사서함을 만들 수 있도록 하려면 Exchange Server를 배포합니다. 
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 34a8c16e-3bed-4e16-939b-b9fe17dd834b

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# ID 관리 서버 설치: Exchange

>[!div class="step-by-step"]
[« SharePoint](prepare-server-sharepoint.md)
[MIM 동기화 서비스 »](install-mim-sync.md)

> [!NOTE]
> 아래의 모든 예제에서 **mimservername**은 도메인 컨트롤러의 이름을 나타내고 **contoso**는 도메인 이름을 나타내며 **Pass@word1**은 예제 암호를 나타냅니다.

## Microsoft Exchange Server 배포
MIM에서 메일 또는 프로비전 사서함을 보내고 받도록 구성하려는 경우 환경에 Exchange가 있어야 합니다. 아직 Exchange를 배포하지 않은 경우 평가용으로 평가판 버전을 설치할 수 있습니다.

1. Microsoft Office 2010 Filter Packs - 버전 2.0 + Microsoft Office 2010 Filter Packs - 버전 2.0 SP1을 다운로드하고 설치합니다.

    - [MS Office10 FP2.0](http://www.microsoft.com/en-us/download/details.aspx?id=17062)

    - [MS Office10 FP2.0 SP1](http://www.microsoft.com/en-us/download/details.aspx?id=26604)

2.  [Microsoft Unified Communications Managed API 4.0, 핵심 런타임 64비트](http://www.microsoft.com/en-us/download/details.aspx?id=34992)다운로드 및 설치

3.  [MS Exchange Server 2013 180일 평가판 버전](http://www.microsoft.com/en-us/evalcenter/evaluate-exchange-server-2013)다운로드 및 설치

>[!div class="step-by-step"]  
[« SharePoint](prepare-server-sharepoint.md)
[MIM 동기화 서비스 »](install-mim-sync.md)


<!--HONumber=Apr16_HO2-->


