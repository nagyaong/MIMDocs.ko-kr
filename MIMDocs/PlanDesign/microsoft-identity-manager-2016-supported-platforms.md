---
title: "지원되는 소프트웨어 플랫폼 | Microsoft 문서"
description: "각 MIM 2016 구성 요소와 호환되는 제품 및 버전을 찾습니다."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 03/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: e8783220723ba9e0e84b4824c79793be6516ab4a
ms.openlocfilehash: 11f10cefa08b2bb13164ddfd7df39b914c87189a
ms.lasthandoff: 03/28/2017


---

# <a name="supported-platforms-for-mim-2016"></a>MIM 2016에 대해 지원되는 플랫폼

다음 표에서는 Microsoft Identity Manager 2016의 각 구성 요소에 대한 지원되는 플랫폼 및 버전을 설명합니다. *로 표시된 버전은 MIM 2016 서비스 팩 1에서만 지원됩니다.


| **MIM 구성 요소** | **플랫폼** | **버전** |
|-------------------|--------------|-------------|
| **MIM 동기화** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 * |
| | MIM 동기화 데이터베이스 | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | 사용자 프로비저닝, PCNS 및 GAL 동기화를 위한 Active Directory(선택 사항)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | 사서함 프로비전 및 GAL 동기화를 위한 Exchange(선택 사항)|Exchange Server 2007 SP3<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1<br/>Exchange Server 2016* |
| | 개발 환경(선택 사항) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Visual Studio 2017* |
| | 추가 연결된 시스템(선택 사항) | Active Directory 도메인 서비스<br/>Active Directory<br/>Lightweight Directory Services<br/>SQL Server 2000 이상<br/>SharePoint Server 2013<br/> SharePoint Server 2016 * <br/> 기타 타사 제품 |
| **MIM 서비스** (PAM 시나리오 제외) | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | MIM 서비스 데이터베이스 | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | MIM 서비스 승인 및 그룹 관리 메일을 위한 Exchange(선택 사항) | Exchange Server 2007 SP3(설치된 Exchange 관리 콘솔 포함)<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online*(알림에만 해당) |
| **MIM 서비스 및 포털** (PAM 시나리오에만 해당)| Windows Server | Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | 배스천 환경 PAM 포리스트를 위한 Active Directory | Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | 기존 포리스트에 대한 Active Directory | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016 * |
| | MIM 서비스 데이터베이스 | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | SharePoint | SharePoint Foundation 2010<br/>SharePoint Foundation 2013 SP1 <br/> SharePoint 2016 * |
| | MIM 서비스 승인 및 그룹 관리 메일을 위한 메일 서버(선택 사항) | Exchange Server 2007 SP3(설치된 Exchange 관리 콘솔 포함)<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online*(알림에만 해당) |
| | 브라우저 | 모든 주요 브라우저 |
| **MIM 서비스 보고** | Windows Server | Windows Server 2012 <br/> Windows Server 2016 * |
| | 데이터 웨어하우스 | System Center 2012 Service Manager <br/> System Center 2012 R2 Service Manager </br> System Center 2016 Service Manager*(4.4.1459 포함)<br/> [System Center 2016에 대한 SQL Server 버전 호환성](https://technet.microsoft.com/en-us/system-center-docs/system-requirements/sql-server-version-compatibility)
 |
| **MIM 암호 다시 설정 및 등록 포털** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | 웹 브라우저 | 모든 주요 브라우저 |
| **MIM 추가 기능 및 확장** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
| | Outlook 통합(선택 사항) | Outlook 2007 SP2<br/>Outlook 2010<br/>Outlook 2013 <br/> Outlook 2016(Windows 10) * |
| | PAM PowerShell 요청자 cmdlet(선택 사항) | Windows 8.1<br/>Windows 10 |
| **MIM 인증서 관리**(서버 및 CA 통합) | Windows server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | 인증 기관 | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | MIM CM 데이터베이스 | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| **MIM 인증서 관리**(응용 프로그램) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **MIM 인증서 관리**(클라이언트 및 대량 클라이언트) | Windows | Windows 7 |
| **MIM BHOLD 제품군** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | BHOLD 데이터베이스 | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 <br/> SQL Server 2014 * <br/> SQL Server 2016 * |
| | 메일 서버(선택 사항) | Exchange Server 2007 SP3<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * |
| | 웹 브라우저 | Internet Explorer 7, 8, 9, 10 또는 11(Silverlight 포함) |

