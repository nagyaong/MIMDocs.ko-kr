---
title: 지원되는 소프트웨어 플랫폼 | Microsoft 문서
description: 각 MIM 2016 구성 요소와 호환되는 제품 및 버전을 찾습니다.
keywords: ''
author: fimguy
ms.author: davidste
manager: davidste
ms.date: 04/11/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: ''
ms.suite: ems
ms.custom: mim
ms.openlocfilehash: 1054d611ae0b230005a0f79be69f5c6c2bba7af2
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34479301"
---
# <a name="supported-platforms-for-mim-2016"></a>MIM 2016에 대해 지원되는 플랫폼

다음 표에서는 Microsoft Identity Manager 2016의 각 구성 요소에 대한 지원되는 플랫폼 및 버전을 설명합니다. *로 표시된 버전은 최신 핫픽스가 적용된 MIM 2016 서비스 팩 1에서만 지원됩니다.


| **MIM 구성 요소** | **플랫폼** | **버전** |
|-------------------|--------------|--------------|
| **MIM 동기화** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 * |
| | 사용자 프로비저닝, PCNS 및 GAL 동기화를 위한 Active Directory 기능 수준 | Windows 2000 <br/>Windows Server 2003<br/>Windows Server 2008<br/>Windows Server 2008 R2<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 *
| | MIM 동기화 데이터베이스 | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | 사용자 프로비전, PCNS 및 GAL 동기화를 위한 Active Directory(선택 사항)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | 사서함 프로비전 및 GAL 동기화를 위한 Exchange(선택 사항)|Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1<br/>Exchange Server 2016* |
| | 개발 환경(선택 사항) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Visual Studio 2017* |
| | 추가 연결된 시스템(선택 사항) | Active Directory 도메인 서비스<br/>Active Directory<br/>Lightweight Directory Services<br/>SQL Server 2008 이상<br/>SharePoint Server 2013<br/> SharePoint Server 2016 * <br/> 기타 타사 제품 |
| **MIM 서비스 및 포털** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| |PAM 시나리오: Windows Server | Windows Server 2012 R2 <br/> Windows Server 2016 * |
| |PAM 시나리오: 배스천 환경 PAM 포리스트를 위한 Active Directory | Windows Server 2012 R2 <br/> Windows Server 2016 * |
| |PAM 시나리오: PAM 시나리오 기존(CORP) 포리스트를 위한 Active Directory | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016 * |
| | MIM 서비스 데이터베이스 | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 |
| | SharePoint | SharePoint Foundation 2010<br/>SharePoint Foundation 2013 SP1 <br/> SharePoint 2016 * |
| | MIM 서비스 승인 및 그룹 관리 메일을 위한 메일 서버(선택 사항) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online*([4.4.1749.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history#version-4417490) 빌드 이전 알림만) |
| | 브라우저 | 지원되는 모든 주요 브라우저*(모바일 장치 제한)|
| **MIM 서비스 보고** | Windows Server |  Windows Server 2008 R2 SP1<br/>Windows Server 2012 <br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | 데이터 웨어하우스 | System Center 2012 Service Manager <br/> System Center 2012 R2 Service Manager </br> System Center 2016 Service Manager*(4.4.1459 포함)<br/> [System Center 2016에 대한 SQL Server 버전 호환성](https://docs.microsoft.com/system-center/scsm/upgrade-to-sm-2016)
 |
| **MIM 암호 다시 설정 및 등록 포털** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | 웹 브라우저 | 지원되는 모든 주요 브라우저 |
| **MIM 추가 기능 및 확장** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
| | Outlook 통합(선택 사항) | Outlook 2010<br/>Outlook 2013 <br/> Outlook 2016(Windows 10) * |
| | PAM PowerShell 요청자 cmdlet(선택 사항) | Windows 8.1<br/>Windows 10 |
| **MIM 인증서 관리**(서버 및 CA 통합) | Windows server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | 인증 기관 | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | MIM CM 데이터베이스 | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| **MIM 인증서 관리**(응용 프로그램) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **MIM 인증서 관리**(대량 클라이언트) | Windows | Windows 7 |
| **MIM 인증서 관리**(클라이언트 ActiveX 기반 스마트 카드) | Windows | Windows 7 </br> Windows 8 </br> Windows 8.1 </br> Windows 10 |
| **MIM BHOLD 제품군** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | BHOLD 데이터베이스 | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 <br/> SQL Server 2014 * <br/> SQL Server 2016 * |
| | 메일 서버(선택 사항) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * |
| | 웹 브라우저 | Silverlight를 포함한 Internet Explorer 지원 브라우저 |
