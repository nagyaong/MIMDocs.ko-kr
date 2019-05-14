---
title: Microsoft Identity Manager 라이선스 및 다운로드 | Microsoft Docs
description: 이 문서에서는 소프트웨어를 다운로드할 위치에 대한 포인터가 포함된 MIM(Microsoft Identity Manager) 2016 라이선스에 대한 방법을 간략하게 설명합니다.
keywords: ''
author: markwahl-msft
ms.author: markwahl-msft
manager: femila
ms.date: 02/25/2019
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.reviewer: billmath
ms.suite: ems
ms.openlocfilehash: 7c5ab987801c63dca2457025442a35560dca3b86
ms.sourcegitcommit: 225fca802d6b59bdc98d50020b297eb6393f70eb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/26/2019
ms.locfileid: "64518744"
---
# <a name="microsoft-identity-manager-2016-licensing-and-downloads"></a>Microsoft Identity Manager 2016 라이선스 및 다운로드

이 문서에서는 소프트웨어를 다운로드할 위치에 대한 포인터가 포함된 MIM(Microsoft Identity Manager) 2016 라이선스에 대한 방법을 간략하게 설명합니다.

## <a name="licensing-mim-for-your-organization"></a>조직의 MIM 라이선스

Microsoft Identity Manager 2016은 사용자별로 라이선스가 부여됩니다.  라이선스에 대한 세부 정보는 [라이선스 조건](https://www.microsoft.com/en-us/licensing/product-licensing/products.aspx) 페이지에서 다운로드할 수 있는 제품 약관 및 문서에 포함되어 있습니다.

### <a name="licensing-for-azure-ad-premium-customers"></a>Azure AD Premium 고객에 대한 라이선스

Microsoft Identity Manager 2016은 Enterprise Mobility + Security의 일부인 Azure Active Directory Premium(P1 및 P2)에 포함되어 있습니다.

Azure AD Premium은 [Microsoft 기업 계약](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx), [오픈 볼륨 라이선스 프로그램](https://www.microsoft.com/en-us/licensing/licensing-programs/open-license.aspx) 및 [클라우드 솔루션 공급자](https://go.microsoft.com/fwlink/?LinkId=614968&clcid=0x409) 프로그램을 통해 제공됩니다. Azure 및 Office 365 구독자는 온라인에서 Active Directory Premium P1 및 P2를 구입할 수도 있습니다.  [Azure Active Directory 가격 책정](https://azure.microsoft.com/en-us/pricing/details/active-directory/)을 자세히 읽어보세요.

### <a name="mim-cals"></a>MIM CAL

사용자를 위한 Azure Active Directory Premium 구독이 없고 동기화 이상의 추가 MIM 기능을 사용하는 경우 MIM에서 ID를 관리하는 각 사용자에 대해 [CAL(클라이언트 액세스 라이선스)](https://www.microsoft.com/en-us/licensing/product-licensing/client-access-license.aspx)이 필요합니다. 외부 사용자(예: 비즈니스 파트너, 외부 계약 업체, 고객)가 MIM에 액세스할 수 있도록 하려면 각 외부 사용자에 대한 CAL을 취득하거나 EC(외부 커넥터) 라이선스를 취득합니다. ID가 Microsoft Identity Manager 동기화 서비스에만 있고 다른 MIM 구성 요소에서 관리되지 않는 사용자에게는 Microsoft Identity Manager 2016 CAL이 필요하지 않습니다.

### <a name="licenses-for-platform-components"></a>플랫폼 구성 요소용 라이선스

Microsoft Identity Manager 2016의 서버 소프트웨어를 Windows Server 추가 기능으로 사용하려면 Windows Server가 필요합니다. 그리고 MIM 배포에는 SQL Server 설치도 필요합니다.  Windows Server 및 SQL Server 라이선스는 MIM에 포함되지 않습니다.

## <a name="obtaining-mim-software"></a>MIM 소프트웨어 가져오기

MIM을 새로 설치하거나 이전 버전에서 업그레이드를 시작하기 전에 최선 버전이 있는지 확인합니다.

새로 설치를 시작하는 경우 시나리오와 관련된 각 MIM 구성 요소에 대한 설치 파일을 다운로드해야 합니다. 그런 다음, 해당 파일에 대한 업데이트를 다운로드한 다음, 다운로드 센터에서 별도로 다운로드한 추가 구성 요소를 다운로드합니다.


| 시나리오 | 구성 요소 | 시나리오에 필요하나요? | DVD ISO 폴더 이름 | 설명 |
|----------|-----------|---------------------   |-------------------|----------|--------------|
|동기화| 동기화 서비스(AD에 대한 커넥터 포함) | 예 | `Synchronization Service` | |
| 동기화 | PCNS | 아니요 | `Password Change Notification Service` |  도메인 컨트롤러에 설치하려면 |
| 동기화 | LDAP, SQL, 웹 서비스, PowerShell, Lotus Domino, 그래프용 커넥터 | 아니요 | 해당 없음 | 다운로드 센터를 통해 배포 |
| 권한 있는 액세스 관리 | MIM 서비스 | 예 | `Service and Portal` | |
| 셀프 서비스 | MIM 서비스, MIM 포털 | 예 | `Service and Portal` | |
| 셀프 서비스 | 추가 기능 및 확장 | 아니요 | `Add-ins and extensions` | 최종 사용자 PC에 설치하려면 |
| 셀프 서비스 | SCSM 보고 | 아니요 | `Data Warehouse Support Scripts` | |
| 셀프 서비스 | 하이브리드 보고 에이전트 | 아니요 | 해당 없음 | 다운로드 센터를 통해 배포 |
| 셀프 서비스 | 언어 팩 | 아니요 | `LANGUAGE Packs` | |
| 인증서 관리 | CM | 예 | `Certificate Management` | |
| 인증서 관리 | CM 대량 클라이언트 | 아니요 | `CM Bulk Client` | |
| 인증서 관리 | CM 클라이언트 | 아니요 | `CM Client`  | |
| 인증서 관리 | Windows용 CM 앱 | 아니요 | `FIMCMModernApp*` | | |

### <a name="obtaining-windows-installer-packages"></a>Windows Installer 패키지 가져오기

새 설치의 경우 대부분의 조직은 [볼륨 라이선스 서비스 센터](https://www.microsoft.com/licensing/servicecenter/default.aspx)에서 MIM 설치 패키지를 다운로드합니다. 


DVD ISO 파일에는 각 MIM 구성 요소마다 하나의 폴더가 포함되어 있습니다. 동기화 서비스, 서비스 및 포털 등 다운로드는 한 소프트웨어를 다른 컴퓨터에 설치하려면 전체 ISO 파일 또는 구성 요소에 대한 폴더를 복사해야 합니다. 파일의 나머지 부분 및 하위 폴더가 없는 폴더에서 MSI 파일만 복사하지 않습니다.

볼륨 라이선스 서비스 센터에 액세스할 수 없는 경우 적절한 개발자 구독을 한 고객은 [Visual Studio 내 혜택 다운로드](https://my.visualstudio.com/Downloads?q=Microsoft%20Identity%20Manager%202016%20with%20Service%20Pack%201&pgroup=)에서 MIM 2016 SP1을 ISO 파일로 다운로드할 수도 있습니다.  "서비스 팩 1이 있는 Microsoft Identity Manager 2016"을 검색합니다.  

볼륨 라이선스 서비스 센터에 액세스할 수 없는 경우 제한된 시간 동안만 MIM 소프트웨어를 사용해 보려면 [MIM 2016의 평가판](https://www.microsoft.com/en-us/download/details.aspx?id=48244)을 다운로드할 수 있습니다. 이 소프트웨어는 프로덕션용이 아니며 처음 설치 후 180일 동안은 작동을 멈추고 업그레이드할 수 없습니다. 평가판을 설치하려면 Windows Server 2008 R2, Windows Server 2012 또는 Windows Server 2012 R2가 필요합니다.  MIM을 처음 사용하고 기술을 배우는 경우 모든 MIM 시나리오에는 Active Directory 도메인, Windows Server 및 SQL Server가 있어야 합니다. Windows Server 또는 SQL Server가 아직 없는 경우 [SQL Server 2016 및 Windows Server 2016로 VM 프로비전](https://azure.microsoft.com/en-us/blog/azure-images-sql-server-2016-on-windows-server-2016/)을 시도할 수 있습니다.

### <a name="obtaining-updates"></a>업데이트를 가져오는 중

MSI에서 MIM을 설치한 후에는 다음에 필요한 핫픽스를 설치해야 합니다.

설치 관리자 패치 파일의 다운로드 사이트에 대한 링크가 있는 최신 업데이트 릴리스에 대한 [Identity Manager 버전 릴리스 기록](./reference/version-history.md)을 확인합니다.

필요한 업데이트 파일을 확인하기 위해 이 표에는 업데이트의 해당 패치(MSP) 파일의 구성 요소와 이름이 나열되어 있습니다.

| 시나리오 | 구성 요소 | DVD ISO 폴더 이름 | 해당 업데이트 패치 파일 이름 |
|----------|-----------|-   |-------------------|----------|--------------|
|동기화| 동기화 서비스 | `Synchronization Service` | `FIMSyncService_x64*.msp` |
| 셀프 서비스 | MIM 서비스, MIM 포털 | `Service and Portal` | `FIMService_x64*msp` |
| 셀프 서비스 | 추가 기능 및 확장 | `Add-ins and extensions` | `FIMAddinsExtensions*msp` |
| 셀프 서비스 | 언어 팩 | `LANGUAGE Packs` | `LANGUAGE Packs.zip` |
| 액세스 관리(BHOLD) | BHOLD | `BHOLD` | `AccessManagementConnector.msi`, `BHOLD*.msi` |
| 인증서 관리 | CM |  `Certificate Management` | `FIMCM*.msp` |
| 인증서 관리 | CM 대량 클라이언트 |  `CM Bulk Client` |`FIMCMBulkClient*msp` |
| 인증서 관리 | CM 클라이언트 | CM 클라이언트 |`FIMCMClient*msp` |

MSP 파일을 설치하기 전에 업데이트와 관련된 릴리스 정보를 읽어야 합니다.

[BHOLD](https://www.microsoft.com/en-us/download/details.aspx?id=55950)에 대한 업데이트는 MSP 파일로 배포되지 않으며 MSI 설치 관리자로만 배포됩니다.

### <a name="additional-downloads"></a>추가 다운로드

다음 다운로드도 관련이 있을 수 있습니다.

- [MIM 하이브리드 보고 에이전트](https://www.microsoft.com/download/details.aspx?id=55112)

- [일반 LDAP 커넥터, 일반 SQL 커넥터, 그래프 커넥터, Lotus Domino 커넥터, PowerShell 커넥터, Web Services 커넥터](http://go.microsoft.com/fwlink/?LinkId=717495)

- [SharePoint 사용자 프로필 저장소용 커넥터](https://www.microsoft.com/en-us/download/details.aspx?id=41164)

- Active Directory 도메인이 아직 없고 실험을 위해 PAM 시나리오를 설정하는 경우 [MIM 2016 SP1 PAM 배포 스크립트](sp1-deployment-scripts.md)를 참조하세요.

## <a name="next-steps"></a>다음 단계

- [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md)에서 제공되는 시나리오에 대해 자세히 알아보세요.
- [용량 계획 가이드](capacity-planning-guide.md)를 읽으세요.
- [동기화 시나리오](microsoft-identity-manager-deploy.md) 또는 [권한 있는 액세스 관리 시나리오](./pam/privileged-identity-management-for-active-directory-domain-services.md)에 대한 MIM을 배포합니다.

