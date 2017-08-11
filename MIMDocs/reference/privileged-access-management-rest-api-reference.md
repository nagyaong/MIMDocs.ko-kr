---
title: "권한 있는 액세스 관리 REST API 참조 | Microsoft Docs"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: reference
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 541854b7-f285-4e8b-bbaf-3f15da69467f
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 02de09a7de843cb42080daa4ce43a5a0c74f2c18
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# <a name="privileged-access-management-rest-api-reference"></a>권한 있는 액세스 관리 REST API 참조
MIM(Microsoft Identity Manager) 2016은 PAM(권한 있는 액세스 관리)이라는 새 시나리오를 추가합니다. 조직에서는 PAM을 사용하여 중요한 리소스에 대해 시스템 또는 서비스 관리자 같은 높은 권한 있는 사용자 계정의 액세스 권한을 보다 잘 제어할 수 있습니다. PAM은 제한된 시간 액세스 권한을 JIT(Just-In-Time)에, 즉 액세스 권한이 필요할 때 제공하여 높은 권한 있는 계정 액세스를 제어합니다.

사용자는 다음 두 가지 방법 중 하나를 사용하여 MIM 서비스에 권한 있는 액세스 권한(권한 상승)을 요청할 수 있습니다.

- PAM REST API 사용
- PAM PowerShell 새 PAMRequest cmdlet 사용

이 가이드의 항목에서는 PAM REST API를 설명합니다. PowerShell cmdlet 사용에 대한 자세한 내용은 Connect 사이트에 나와 있는 테스트 랩 가이드의 Microsoft Identity Manager를 사용하여 권한 있는 액세스 관리 설명을 참조하세요.

##<a name="pam-rest-api-resources-and-operations"></a>PAM REST API 리소스 및 작업
PAM REST API는 다음 리소스에서 작동합니다.
- **PAM 역할**: PAM 역할은 사용자 컬렉션을 액세스 권한 컬렉션과 연결합니다. 액세스 권한은 보안 그룹에 대한 참조로 정의됩니다.  모든 PAM 역할에는 PAM 역할로 권한이 상승될 수 있는 대상인, 후보자라는 사용자 계정 목록이 있습니다. PAM 역할에 대해 다음 작업을 수행할 수 있습니다.

    - [PAM 역할 가져오기](privileged-access-management-get-roles.md)

- **PAM 요청**: PAM 역할 액세스 권한으로 권한을 상승하려는 사용자는 PAM 요청을 제출하고 권한 상승을 위해 요청에 대한 승인을 받아야 합니다. PAM 요청 개체는 MIM 서비스에서 이 요청의 수명 주기를 추적합니다. PAM 요청에 대해 다음 작업을 수행할 수 있습니다.

    - [PAM 요청 만들기](privileged-access-management-create-request.md)
    - [PAM 요청 가져오기](privileged-access-management-get-requests.md)
    - [PAM 요청 닫기](privileged-access-management-close-request.md)

- **PAM 요청 보류 중**: 사용자가 제출한 PAM 요청을 승인하거나 거부하는 데 사용됩니다. 보류 중인 PAM 요청에 대해 다음 작업을 수행할 수 있습니다.

    - [보류 중인 PAM 요청 가져오기](privileged-access-management-get-pending-requests.md)
    - [보류 중인 PAM 요청 승인 또는 거부](privileged-access-management-approve-reject-pending-request.md)

- **PAM 세션**: PAM REST API를 사용하는 경우 클라이언트(예: 웹 브라우저)에 PAM REST API 끝점이 포함된 세션이 있습니다. 이 세션에서 클라이언트는 REST API 끝점으로 인증됩니다. PAM 세션에 대해 다음 작업을 수행할 수 있습니다.

     - [PAM 세션 정보 가져오기](privileged-access-management-get-session-info.md)

서비스에 대한 자세한 내용은 [PAM REST API 서비스 세부 정보](privileged-access-management-rest-api-service-details.md)를 참조하세요.

##<a name="pam-sample-portal-on-github"></a>GitHub의 PAM 샘플 포털
PAM REST API 사용법을 배울 수 있는 한 가지 방법은 API를 사용하는 예제 웹 응용 프로그램인 PAM 샘플 포털을 사용하는 것입니다. [GitHub의 PAM 샘플 리포지토리](http://go.microsoft.com/fwlink/?LinkID=618550&clcid=0x409)에서 PAM 샘플 포털에 대한 코드를 찾을 수 있습니다. PAM 테스트 랩 가이드에서 샘플 포털을 배포하는 방법을 배울 수 있습니다.
