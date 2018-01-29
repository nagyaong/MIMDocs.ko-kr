---
title: "Azure AD의 하이브리드 보고란 무엇인가요? | Microsoft 문서"
description: "Azure Active Directory의 하이브리드 감사 활동 보고서를 통해 클라우드와 온-프레미스 모두에서 감사된 이벤트를 볼 수 있습니다."
keywords: 
author: fimguy
ms.author: fimguy
manager: bhu
ms.date: 09/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: fimguy
ms.suite: ems
ms.openlocfilehash: e2391be3d05f61335c134c104673a31ad7fc3830
ms.sourcegitcommit: 3d8a2493eae1218bfdb75a399ffa4adc8c2a8fdf
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/20/2018
---
# <a name="hybrid-identity-management-audit-reporting-in-azure-active-directory-public-preview-refresh"></a>Azure Active Directory의 하이브리드 ID 관리 감사 보고 공개 미리 보기 새로 고침
Azure AD(Azure Active Directory) 감사 활동 보고를 통해 온-프레미스 또는 클라우드의 ID 관리 활동을 모니터링할 수 있습니다. 단일 보고서에서 모든 ID 및 액세스 데이터를 관리하여 시간을 절약하고 전체 비용을 줄일 수 있습니다.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Azure Active Directory 하이브리드 보고란?
하이브리드 감사 보고를 통해 IT 전문가는 다음과 같은 일반적인 ID 관리 보고 문제를 해결할 수 있습니다.

* **서로 다른 시스템에서 ID 관리 활동 수집**. 하이브리드 보고서는 Azure AD 및 Identity Manager의 ID 관리 작업을 보여줍니다.

* **보고 데이터 내보내기 및 사용자 지정 보고서 만들기**. Azure Portal에서 보고서를 보는 것 외에 데이터를 내보내 사용자 지정 보기를 생성할 수도 있습니다.

* **보고 시스템 인프라 비용 절감**. 클라우드의 하이브리드 보고는 온-프레미스 데이터 웨어하우스 인프라와 관련된 비용을 제거하는 데 도움이 될 수 있습니다.

## <a name="how-does-it-work"></a>트래픽 관리자의 작동 방식

온-프레미스 데이터를 수집하려면 먼저 Identity Manager 2016 서버에 보고 에이전트를 설치합니다. [Microsoft Identity Manager 하이브리드 보고 에이전트를 다운로드](https://www.microsoft.com/download/details.aspx?id=55112)합니다.

하이브리드 보고는 다음 프로세스를 거칩니다.
1. 보고 에이전트를 설치하고 나면 Identity Manager 활동 데이터가 Windows 이벤트 로그로 전송됩니다.
2. 보고 에이전트는 10분마다 또는 Windows 이벤트 로그 서비스가 다시 시작될 때 델타 이벤트를 처리합니다. 그런 다음 에이전트는 이벤트를 Azure Portal에 업로드합니다.
3. Azure Portal은 수신된 데이터를 수신한 지 1시간 이내에 처리합니다.
4. 활동 데이터는 한 달 동안 Azure에 저장됩니다.
5. Azure Portal은 감사 보고 데이터를 검색하여 Azure 감사 보고 창에 표시합니다.

## <a name="next-steps"></a>다음 단계
다음에 대해 자세히 알아보세요.
- [Identity Manager 하이브리드 보고 작업](working-with-identity-manager-hybrid-reporting.md)
- [Azure Active Directory 포털의 감사 활동 보고서](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [보고 보존 정책](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)
- [Microsoft Azure 로그 통합(SIEM)](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)
- [Azure Active Directory Reporting API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)
