---
title: 지원되는 커넥터 | Microsoft 문서
description: 커넥터를 사용하여 MIM과 연결된 데이터 원본 간의 데이터 전송을 관리할 수 있습니다.
keywords: ''
author: fimguy
ms.author: fimguy
manager: bhu
ms.date: 1/24/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 7b685ffb6f2a52bd2782395e4c1f26501ffe3101
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34479350"
---
# <a name="connect-to-your-directories"></a>디렉터리에 연결

커넥터는 연결된 특정 데이터 원본을 MIM(Microsoft Identity Manager) SP1에 연결합니다. 커넥터는 데이터를 연결된 데이터 원본에서 MIM으로 이동합니다. 또한 MIM의 데이터가 수정되는 경우 커넥터는 데이터를 연결된 데이터 원본으로 내보내 MIM과 동기화된 상태로 유지합니다. 일반적으로 연결된 각 디렉터리에는 커넥터가 하나 이상 있습니다.

Forefront Identity Manager에서는 커넥터를 관리 에이전트라고 했습니다. 이 용어가 일부 문서나 제품의 일부에서 여전히 사용되지만 두 용어는 동일한 개념을 가리킵니다.

이 문서에서는 MIM에 포함되어 있고 지원되는 커넥터만 다루지만 Extensible Connectivity 2.0용 커넥터를 사용하면 더 많은 데이터 원본과 연결할 수 있습니다. 이 방법으로 자체 커넥터를 만든 파트너도 있으며 전체 목록은 Wiki의 [FIM 2010: Management Agents from Partners](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx)(FIM 2010: 파트너의 관리 에이전트)에서 확인할 수 있습니다.

## <a name="supported-connectors-in-mim-2016-sp1"></a>MIM 2016 SP1에서 지원되는 커넥터

| 이름 | 지원되는 연결된 데이터 원본 버전 및 기술 링크 |
| ---- | ----------------------------------------------- |
| Active Directory 도메인 서비스 | Active Directory 2012, 2016 |
| ADLDS(Active Directory Lightweight Directory Services) | ADLDS(Active Directory Lightweight Directory Services) |
| Active Directory GAL(전체 주소 목록) | Active Directory GAL(전체 주소 목록) – Exchange 2013 , 2016 |
| Extensible Connectivity 2.0 | 모든 호출 기반 또는 파일 기반 데이터 원본 |
| FIM 서비스 | FIM 서비스 관리 에이전트(동기화 서비스)는 설치된 “Forefront Identity Manager 서비스”와 같은 버전이어야 함 |
| IBM DB2 유니버설 데이터베이스 | IBM DB2 버전 9.5 또는 9.7, IBM DB2 OLEDB v9.5 FP5 또는 v9.7 FP1 |
| IBM 디렉터리 서버 | IBM Tivoli 디렉터리 서버 6.x |
| Novell eDirectory | Novell eDirectory 버전 8.7.3, 8.8.5 및 8.8.6 |
| Oracle 데이터베이스 | Oracle Database 10g 또는 11g, 64비트 클라이언트 |
| Microsoft SQL Server | SQL Server 2012, 2014, 2016 |
| Oracle(이전의 Sun 및 Netscape) 디렉터리 서버 | Sun 디렉터리 서버 6.x, 7.x 및 Oracle 11 |
| [FIM 2010 R2용 Windows PowerShell 커넥터](https://msdn.microsoft.com/library/dn640417.aspx) | Windows PowerShell 2.0 이상 |
| [FIM 2010 R2용 Microsoft Azure Active Directory 커넥터](https://msdn.microsoft.com/library/dn511001.aspx) | Microsoft Azure Active Directory |
| [FIM 2010 R2용 일반 LDAP 커넥터](https://msdn.microsoft.com/library/dn510997.aspx) | [LDAP v3 서버(RFC 4510 규격)](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-genericldap) |
| [Generic SQL Connector for FIM 2010 R2 / MIM](https://msdn.microsoft.com/library/dn510997.aspx)(FIM 2010 R2/MIM용 일반 SQL 커넥터) | [커넥터는 모든 64비트 ODBC 드라이버와 함께 지원됨](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-genericsql) |
| [Lotus Domino용 커넥터](https://msdn.microsoft.com/library/hh859750.aspx) | Lotus Notes 릴리스 v8.5.x |
| [SharePoint Services 커넥터 UPA](https://msdn.microsoft.com/library/dn511003.aspx) | UPA(사용자 프로필 서비스 응용 프로그램)를 포함하는 SharePoint Server 2013 또는 2016 |
| [웹 서비스용 커넥터](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | [SAP ECC 5.0 또는 6.0, Oracle PeopleSoft 9.1, Oracle eBusiness 12.1](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) |
| [특성-값 쌍 텍스트 파일](https://technet.microsoft.com/library/cc708644(v=ws.10).aspx) | 특성-값 쌍 텍스트 파일 |
| [구분된 텍스트 파일](https://technet.microsoft.com/library/cc720612(v=ws.10).aspx) | 구분된 텍스트 파일 |
| [DSML(Directory Services Markup Language)](https://technet.microsoft.com/library/cc720660(v=ws.10).aspx) | DSML(Directory Services Markup Language) 2.0 |
| [고정 너비 텍스트 파일](https://technet.microsoft.com/library/cc720633(v=ws.10).aspx) | 고정 너비 텍스트 파일 |
| [LDIF(LDAP 데이터 교환 형식)](https://technet.microsoft.com/library/cc708662(v=ws.10).aspx) | LDIF(LDAP 데이터 교환 형식) |

## <a name="related-topics"></a>관련 항목

[FIM 2010 R2의 관리 에이전트](https://technet.microsoft.com/library/jj133885.aspx)
