---
title: "하이브리드 보고란? | Microsoft Identity Manager"
description: "Azure Active Directory 하이브리드 보고를 통해 클라우드 이벤트와 온-프레미스 이벤트를 모두 포함하는 사용자 지정 보고서를 만들 수 있습니다."
keywords: 
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b3ab1b9376c9b613739d87c812f4b16a4e17e6de
ms.openlocfilehash: a074f3361e6d5be497b1a3c25d56aaa7008b128e


---

# Azure의 하이브리드 ID 관리 보고서
Azure AD(Active Directory)를 사용하면 온-프레미스 또는 클라우드에서 발생하는 ID 관리 활동을 모니터링하는 단일 보고서를 만들 수 있습니다. 이 기능을 통해 모든 ID 및 액세스 데이터를 한 곳에서 관리할 수 있어 시간이 단축되고 전체 비용이 절감됩니다.

## Azure AD 하이브리드 보고란 무엇인가요?
하이브리드 보고를 통해 IT 전문가는 일반적인 ID 관리 보고 문제를 해결할 수 있습니다.

1. **서로 다른 시스템에서 ID 관리 작업을 수집합니다.** 하이브리드 보고서는 Azure AD 및 Identity Manager의 ID 관리 작업을 보여줍니다.

2. **보고 데이터를 내보내고 사용자 지정 보고서를 만듭니다.** Azure 포털에서 보고서를 보는 것 외에 사용자 지정 보기를 생성하기 위해 데이터를 내보낼 수도 있습니다.

3. **보고 시스템 인프라 비용을 절감합니다.** 클라우드의 하이브리드 보고 기능으로 온-프레미스 보고 데이터 웨어하우스 인프라를 사용하지 않아도 됩니다.

## 트래픽 관리자의 작동 방식

온-프레미스 데이터를 수집하려면 먼저 Identity Manager 서버에 보고 에이전트를 설치합니다. 보고 에이전트는 [Azure 클래식 포털](https://manage.windowsazure.com/)의 디렉토리 구성 페이지에서 다운로드할 수 있습니다.

하이브리드 보고 과정은 다음 단계를 따릅니다.
1. 보고 에이전트가 설치되면 Identity Manager의 활동 데이터가 Windows 이벤트 로그로 전송됩니다.
2. 보고 에이전트가 Windows 이벤트 로그에서 이벤트를 처리하여 Azure에 업로드합니다.
3. 활동 데이터는 한 달 동안 Azure에 저장됩니다.
4. 보고서를 요청할 때 필요한 보고서에 대해 작업 이벤트의 구문이 분석되고 필터링됩니다.
5. Azure 클래식 포털에서 보고 데이터를 검색하고 이 데이터를 작업 보고서로 렌더링합니다.

## 참고 항목
- [Identity Manager 하이브리드 보고 작업](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting)에 대한 자세한 내용 보기



<!--HONumber=Jul16_HO3-->


