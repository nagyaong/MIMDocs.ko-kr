---
title: "Privileged Access Management를 위해 MIM 환경 구성 | Microsoft Identity Manager"
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: 9cf126d898c93faf89d7119136cce4e4963bb63d
ms.openlocfilehash: c9f2cf2ba1f42ea1513ae38d8089839d85ae5553


---

# 권한 있는 액세스 관리를 위해 MIM 환경 구성
7개의 단계를 완료하여 크로스 포리스트 액세스를 위한 환경을 설정하고 Active Directory와 Microsoft Identity Manager를 설치 및 구성하며 Just-In-Time 액세스 요청을 보여 줍니다.

다음 단계는 처음부터 시작하고 테스트 환경을 구축할 수 있도록 구성되어 있습니다. 기존 환경에 PAM을 적용할 경우 예제와 일치하는 새 계정을 만드는 대신 자체 도메인 컨트롤러 또는 사용자 계정을 사용할 수 있습니다.

1.  *CORPDC* 서버를 도메인 컨트롤러로, *CORPWKSTN* 을 구성원 워크스테이션으로 준비합니다.

2.  *PRIVDC* 서버를 도메인 컨트롤러로 준비합니다.

3.  *PRIV* 포리스트에 *PAMSRV* 서버를 준비합니다.

4.  *PAMSRV* 에 MIM 구성 요소를 설치하고 *CONTOSO* 포리스트 구성원 워크스테이션에 cmdlet을 설치하고 권한 있는 액세스 관리를 위해 MIM 구성 요소와 cmdlet을 준비합니다.

5.  *PRIV* 및 *CONTOSO* 포리스트 간에 트러스트를 설정합니다.

6.  Just-In-Time 권한 있는 액세스 관리를 위해 보호된 리소스에 대한 액세스 권한을 가진 권한 있는 보안 그룹과 구성원 계정을 준비합니다.

7.  보호된 리소스에 액세스할 수 있는 높은 권한 요청, 받기 및 사용하는 방법에 대해 설명합니다.

>[!div class="step-by-step"]
[[!div class="step-by-step"] [시작 »](step-1-prepare-corp-domain.md)](step-1-prepare-corp-domain.md)



<!--HONumber=Jul16_HO2-->


