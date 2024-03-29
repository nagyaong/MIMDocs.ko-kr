---
title: PAM 소프트웨어 요구 사항 | Microsoft 문서
description: Privileged Access Management를 성공적으로 배포하기 위한 하드웨어 및 소프트웨어 요구 사항 찾기
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/06/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4838e9e8a495866902a78e713bb3b226eaf9def1
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379814"
---
# <a name="hardware-and-software-requirements"></a>하드웨어 및 소프트웨어 요구 사항

Privileged Access Management의 하드웨어 요구 사항은 기본 소프트웨어 플랫폼의 하드웨어 요구 사항을 초과하지 않습니다. 메모리나 디스크 공간이 충분하고 네트워크에 연결되어 있으면 됩니다.

> [!IMPORTANT]
> 이 문서에서는 기본 배포에 대한 최소 요구 사항을 제공합니다. 성능, 확장성 또는 고가용성을 보여 주기 위한 것은 아닙니다. 대기업 또는 프로덕션 환경에 권장되는 배포 토폴로지를 나타내지는 않습니다.

## <a name="installing-from-software-packages"></a>소프트웨어 패키지에서 설치

TechNet Evaluation Center 또는 MSDN에서 다음 소프트웨어를 다운로드할 수 있습니다.

- Microsoft Identity Manager 2016
  - 서비스 및 포털: MIM 서비스와 MIM 포털 및 PAM 시나리오용 설치 관리자 포함
  - 추가 기능 및 확장: 요청자 PowerShell cmdlet용 설치 관리자 포함

다음 소프트웨어는 GitHub에서 다운로드할 수 있습니다.

- [PAMSamplePortal](https://github.com/Azure/identity-management-samples): REST API용 샘플 웹 응용 프로그램 포함

## <a name="required-software"></a>필수 소프트웨어

- Windows Server 2012 R2
- Windows 10 Enterprise
- SQL Server 2012 서비스 팩 1 또는 SQL Server 2014

## <a name="evaluation-software"></a>평가용 소프트웨어

Windows, SQL Server 또는 Windows Server에 대해 라이선스가 없는 경우 평가판을 다운로드할 수 있습니다.

### <a name="technet-evaluation-center"></a>TechNet Evaluation Center

- [Windows Server 2012 R2](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)
- [Windows 10 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)

### <a name="microsoft-download-center"></a>Microsoft 다운로드 센터

- [SQL Server](https://www.microsoft.com/download/details.aspx?id=29066)  
- [SharePoint Foundation 2013 SP1 및 필수 조건](https://www.microsoft.com/download/details.aspx?id=42039)

## <a name="hardware-requirements"></a>하드웨어 요구 사항

PAM의 각 구성 요소는 소프트웨어 제품의 시스템 요구 사항을 참조하세요.

CORPDC의 경우:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) 또는 이전 버전

CORPWKSTN의 경우:

- [Windows 10](https://technet.microsoft.com/windows/dn798752.aspx)

PRIVDC의 경우:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)

PAMSRV의 경우:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) 또는 [SQL Server 2014](https://msdn.microsoft.com/library/ms143506(v=sql.120).aspx)
