---
title: "Azure에서 Identity Manager 하이브리드 보고 식별 | Microsoft Identity Manager"
description: "Azure Active Directory 하이브리드 보고를 통해 클라우드 이벤트와 온-프레미스 이벤트를 모두 포함하는 사용자 지정 보고서를 만들 수 있습니다."
keywords: 
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: aea1c50e2f02ed5b9b745c1ec24410d1e7c0fc10
ms.openlocfilehash: 03b75e428c920f68b1816a934cc4ffed8cb47be0


---

# Azure의 ID 관리자 하이브리드 보고
이제 Azure 구독이 있으면 온-프레미스와 클라우드에 둘 다 있는 이벤트의 보고서를 쉽게 만들 수 있습니다. 그러면 Azure 포털에서 보고서를 볼 수 있습니다. 그러나 보고서가 Azure Active Directory 작업과 결합되는 것이 더 좋습니다. ID 관리자 하이브리드 보고를 사용하면 Azure AD 관리 포털이 클라우드와 온-프레미스 작업 둘 다에 대해 ID 관리 작업 보고서를 표시할 수 있습니다. 이 보고 기능은 다음을 제공합니다.

-   환경이 통합됩니다. 클라우드와 온-프레미스 둘 다에 대해 IAM 작업에 대한 보고서를 통합합니다.

-   비용이 절감됩니다. 온-프레미스 보고 데이터 웨어하우스 인프라가 필요하지 않습니다.

-   사용자의 데이터가 사용자에게 있습니다. 보고 데이터는 온-프레미스 ID 관리자 또는 Azure AD에서 쉽게 내보낼 수 있으며 사용자 지정 보기 보고서를 생성하는 데 사용할 수 있습니다.

## Azure AD 하이브리드 보고란 무엇인가요?
하이브리드 보고를 사용하면 Azure AD 관리 포털이 통합된 ID 관리 작업 보고서를 표시할 수 있습니다. 이는 작업이 수행된 위치가 ID 관리자인지 또는 Azure AD인지 관계 없습니다. 예를 들어 지난 달에 SSPR(셀프 서비스 암호 재설정)에 등록한 사용자를 파악하려는 경우 Azure AD 관리 포털에서 해당 사용자를 모두 볼 수 있습니다. 이 보고서에는 [응용 프로그램 액세스 패널](https://myapps.microsoft.com)과 Identity Manager 모두에서 SSPR에 등록된 사용자가 표시됩니다.

![Azure암호 재설정 활동 이미지](media/MIM-Hybrid-passwordreset.jpg)

## 하이브리드 보고를 사용해야 하는 이유는 무엇입니까?
하이브리드 보고를 통해 IT 전문가는 몇 가지 일반적인 ID 관리 보고 문제를 해결할 수 있습니다.

1.  다른 시스템에서 수행된 ID 관리 작업을 보고합니다. 이제 Azure AD 관리 포털에서 Azure AD 및 ID 관리자의 작업에서 나온 ID 관리 보고서를 볼 수 있습니다.

2.  보고 데이터를 내보내고 사용자 지정 보고서를 만듭니다. Azure AD의 보고서 외에, 이 새 기능을 사용하여 ID 관리자 작업을 반영하는 Windows 이벤트를 추가했습니다. 이를 통해 SIEM 시스템으로 통합, ID 관리자 작업 보기 및 사용자 지정 보고서 만들기가 전보다 훨씬 쉬워졌습니다.

3.  보고 시스템 인프라 비용을 최소화합니다. 이 새 기능을 배포하려면 몇 분 정도 소요됩니다. 필요한 작업은 ID 관리자 서버에 보고 에이전트를 설치하는 것이 전부입니다.

보고 에이전트는 Azure AD 관리 포털의 디렉터리 구성 화면에서 다운로드합니다.

![MIM 보고 에이전트 다운로드 이미지](media/MIM-Hybrid-downloadReportAgent.jpg)

## 어떤 방식으로 작동합니까?
보고 에이전트가 설치된 다음 ID 관리자의 활동 데이터가 Windows 이벤트 로그로 전송됩니다. 보고 에이전트가 이벤트를 처리하여 Azure에 업로드합니다. Azure에서 활동 데이터가 현재는 1개월간 저장됩니다. 보고서를 검색할 때 필요한 보고서에 대해 작업 이벤트의 구문이 분석되고 필터링됩니다. 마지막으로 Azure 관리 포털이 보고 데이터를 검색하고 이 데이터를 작업 보고서로 렌더링합니다.

![하이브리드 보고 다이어그램](media/MIM-Hybrid-howitworks.png)



<!--HONumber=Apr16_HO2-->


