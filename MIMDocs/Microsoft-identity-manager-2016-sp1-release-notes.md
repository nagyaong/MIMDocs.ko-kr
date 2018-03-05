---
title: "Microsoft Identity Manager 2016 서비스 팩 1 | Microsoft 문서"
description: "MIM 2016을 사용하여 클라우드 및 온-프레미스에서 보다 안전하고 편리한 ID 관리 환경을 만드는 방법을 이해합니다."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 98d21076801bbe60b7a2d2d5b7e1c41d4bce1b4a
ms.sourcegitcommit: 8edd380f54c3e9e83cfabe8adfa31587612e5773
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/19/2017
---
# <a name="whats-new-for-microsoft-identity-manager-2016-service-pack-1"></a>Microsoft Identity Manager 2016 서비스 팩 1의 새로운 기능 #

Microsoft Identity Manager 서비스 및 업데이트의 정규 릴리스 주기의 일부로 [Microsoft Identity Manager(MIM) 2016 SP1(서비스 팩 1)](https://msdn.microsoft.com/subscriptions/downloads/?fileid=70212#searchTerm=&Languages=en&PageSize=10&PageIndex=0&FileId=70212)이 출시되었습니다. 이 문서에서는 이 릴리스에 포함된 업데이트, 개선 사항, 기능 및 변경 내용을 간략하게 설명합니다.

MIM SP1의 프로덕션 배포 중에 문제가 발생한 경우 Microsoft 고객 지원에 문의하세요.

여러분의 소중한 의견도 듣고 싶습니다! 제품 팀에 대한 의견이나 궁금한 사항이 있는 경우 메일([mim2016@microsoft.com](mailto:mim2016@microsoft.com))로 보내 주시기 바랍니다.



## <a name="updates-in-this-service-pack"></a>이 서비스 팩의 업데이트 내용 #

### <a name="mim"></a>MIM

- **최종 사용자 셀프 서비스를 위한 MIM 포털 브라우저 간 호환:** 이 서비스 팩에는 대부분의 주요 브라우저에 대한 지원이 도입되었습니다. 이제 사용자는 Microsoft Edge, Chrome 및 Safari에서 셀프 서비스 그룹 및 프로필 관리를 위해 MIM 포털에 액세스하고 상호 작용할 수 있습니다.

- **Exchange Online에 대한 MIM 서비스 지원:** MIM 서비스는 오랜 전부터 승인 및 알림을 위해 전자 메일을 보내고 받을 수 있도록 지원해 왔습니다. SP1 MIM 이전에는 Exchange Server 또는 SMTP만 지원되었습니다. 서비스 팩 1에서는 MIM 서비스에서 Office365 Exchange 온라인 계정을 사용하여 전자 메일 알림뿐 아니라 요청을 보내고 받을 수 있습니다.

- **업로드 시 이미지 파일 형식 유효성 검사:** 이제 MIM에서 포털에 업로드되는 이미지의 파일 형식에 대한 유효성을 검사할 수 있습니다.

### <a name="privileged-access-managementpam"></a>PAM(Privileged Access Management) 사용

- **PAM "PRIV"(배스천) 포리스트에서 Windows Server 2016 기능 수준 지원:** Windows Server 2016의 Active Directory 도메인 서비스 포리스트 기능 수준에서 실행되는 도메인 컨트롤러가 있는 환경에서 MIM PAM 서비스를 구성할 수 있습니다. 구성한 후에는 사용자의 Kerberos 티켓 시간이 해당 역할의 남은 활성화 시간으로 제한됩니다.

    >[!Note]
    CORP 도메인에서 Windows Server 2012 R2의 포리스트 기능 수준을 유지하려면 CORP 도메인 컨트롤러에 [KB 2919442](https://support.microsoft.com/en-us/kb/2919442) 및 [KB 2919355](https://support.microsoft.com/en-us/kb/2919355)를 설치하는 것이 좋습니다.

- **“PRIV”(배스천) 포리스트 전용 그룹으로 권한 있는 계정 승격:** 이제 관리자는 MIM 서비스에 “PRIV” 포리스트 전용 그룹 및 사용자를 알릴 수 있습니다. 이 경우 해당 그룹 및 사용자를 PAM 역할에 포함할 수 있습니다.  그런 다음 역할에 대해 활성화하고 “PRIV” 포리스트 그룹의 구성원으로 할당할 수 있습니다.

- **PAM 배포 스크립트:** PAM 배포 스크립트를 통해 관리자는 PAM 환경 설치를 간소화할 수 있습니다.

- **인증 정책 사일로 구성용 PAM Cmdlet:** 서비스 팩 1에는 배스천 포리스트의 보안을 강화하기 위해 새 Cmdlet이 도입되었습니다. 이러한 Cmdlet은 인증 정책 템플릿에 바인딩된 인증 정책 사일로를 자동으로 만듭니다.

    >[!Note]
    이러한 Cmdlet은 배포 스크립트의 일부로 자동으로 실행됩니다.


## <a name="platform-support"></a>플랫폼 지원
업데이트된 플랫폼 지원 정보는 [MIM 2016에 대해 지원되는 플랫폼](microsoft-identity-manager-2016-supported-platforms.md)에서 확인할 수 있습니다.  이 서비스 팩에서 지원되는 새 플랫폼에는 SQL Server 2016, SharePoint 2016 등이 포함됩니다.

## <a name="issues-fixed-in-this-release-from-mim-2016-general-availability"></a>MIM 2016 GA의 이 릴리스에서 해결된 문제

### <a name="pam"></a>PAM
- New-PAMGroup이 PRIV 포리스트의 도메인 로컬 그룹에 대한 MIM 개체를 만들지 못함
- New-PAMDomainConfiguration이 “netdom” 오류 메시지와 함께 실패함
- PAM 모니터링 서비스에 PRIV 포리스트의 그룹에 대한 경고가 기록됨

## <a name="how-to-upgrade-to-service-pack-1"></a>서비스 팩 1로 업그레이드하는 방법

Microsoft Identity Manager 2016 서비스 팩 1로 업그레이드하는 고객은 해당 배포에 적용되는 모든 서비스에서 아래 지침을 따라야 합니다.

>[!Note]
>Forefront Identity Manager 2010 R2 SP1 이하를 실행하는 고객은 먼저 2015년 8월에 릴리스된 Microsoft Identity Manager 2016으로 업그레이드한 다음 아래 단계를 따라야 합니다.

시작하기 전에

MIM 서비스 및 포털을 업그레이드하기 전에 MIM 동기화 엔진을 업그레이드해야 합니다.
MIMService 및 MIM 동기화 데이터베이스를 백업해야 합니다.

  1. 업그레이드할 Microsoft Identity Manager 구성 요소를 제거합니다.
  2. 제거가 완료되면 "FIMSplash.htm" 설치 미디어에 있는 시작 페이지를 엽니다.
  3. 업그레이드할 MIM 구성 요소를 선택합니다.
  4. 메시지에 따라 설치를 계속합니다.
    * MIM 서비스 및 포털 설치: Exchange Online을 메일 계정으로 선택한 경우 다음 화면에서 Exchange Online 계정의 전자 메일 주소와 자격 증명을 입력합니다.
