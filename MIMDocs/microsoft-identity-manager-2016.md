---
title: Microsoft Identity Manager 2016 | Microsoft 문서
description: MIM은 FIM 2010의 액세스 관리 기능을 포함하며 조직 내에서 사용자, 자격 증명, 정책 및 액세스를 관리하는 데 도움이 됩니다.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: abbd661fa1bef13ad92b916f8485934390905bf4
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358315"
---
# <a name="microsoft-identity-manager-2016"></a>Microsoft Identity Manager 2016

MIM(Microsoft Identity Manager) 2016은 [FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)의 ID 및 액세스 관리 기능을 기반으로 합니다. 이전의 프로세서처럼 MIM으로 조직 내의 사용자, 자격 증명, 정책, 액세스를 관리할 수 있습니다.  또한 MIM 2016에는 하이브리드 환경, 권한 있는 액세스 관리 기능, 새로운 플랫폼 지원이 추가되었습니다.

이외에 기존 ID 관리 기능이 [FIM](https://technet.microsoft.com/library/jj133868)에 추가되었습니다. MIM 2016은 다음과 같은 새로운 기능과 향상된 기능을 제공합니다.

- Privileged Identity Management
- 인증서 관리의 새로운 기능
  - [인증서 관리 REST API 참조](./reference/certificate-management-rest-api-reference.md)
  - 다중 포리스트 토폴로지에 대한 지원
  - [가상 스마트 카드용 Windows 앱](working-with-mim-certificate-manager.md)
  - 업데이트된 이벤트 및 문제 해결 기능 
- 이제 [셀프 서비스 시나리오](working-with-self-service-password-reset.md)에는 암호 재설정을 위한 계정 잠금 해제 및 Azure MFA(Multi-Factor Authentication) 게이트가 포함됩니다.

## <a name="hybrid-experience"></a>하이브리드 환경

사용자는 Microsoft Identity Manager 2016을 [Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-whatis)와 함께 사용하여 전체 환경을 제어할 수 있습니다. Azure AD의 하이브리드 보고 기능은 클라우드 및 온-프레미스 데이터를 한 곳에 표시합니다. 또한 [셀프 서비스 암호 재설정 포털](working-with-self-service-password-reset.md)은 Azure MFA(다단계 인증)를 지원합니다.

## <a name="privileged-identity-management"></a>Privileged Identity Management

Privileged Identity Management는 중요한 리소스에 작업 기반 임시 액세스를 제공하여 관리자 액세스 권한을 제어하고 관리합니다. 즉, 사용자에게 필요한 권한만 제공할 수 있어 사이버 공격자가 전체 관리 액세스 권한을 획득할 가능성이 줄어듭니다. 또한 권위 있는 ID 관리는 기존 Active Directory 포리스트에서 관리자 계정을 추출하고 격리합니다.

MIM은 Active Directory를 관리하는 온-프레미스 Privileged Identity Management 솔루션을 지원합니다. 시작하려면 [Privileged Access Management를 사용](./pam/privileged-identity-management-for-active-directory-domain-services.md)합니다.

## <a name="related-topics"></a>관련 항목

- Microsoft Identity Manager는 여전히 이전 버전인 Forefront Identity Manager와 밀접한 관련이 있습니다. FIM을 계속 사용하거나 추가 설명서를 참조하려는 경우, [FIM 2010 R2 설명서 로드맵](https://technet.microsoft.com/library/jj133885.aspx)을 확인하세요.
- [MIM 배포를 위한 토폴로지 고려 사항](topology-considerations.md) 이 문서에서는 구현을 고려할 수 있는 다양한 배포 토폴로지를 소개합니다.
- [용량 계획 가이드](capacity-planning-guide.md) 테스트 환경과 함께 이 가이드를 사용하여 배포에 적절한 범위를 파악할 수 있습니다.
