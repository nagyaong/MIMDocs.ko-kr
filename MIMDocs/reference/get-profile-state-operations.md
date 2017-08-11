---
title: "프로필 상태 작업 가져오기 | Microsoft Docs"
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
ms.assetid: f62c827b-5229-4b13-ad37-4f62ad231d30
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8e8428984404b2aebda8f53e4f7841b699a5d23a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-state-operations"></a>프로필 상태 작업 가져오기
지정된 프로필에서 현재 사용자가 수행할 수 있는 가능한 작업 목록을 가져옵니다. 그런 다음 지정된 작업에 대한 요청을 시작할 수 있습니다.

**참고**: 이 항목에 표시된 URL은 API 배포 중 선택한 호스트 이름과 관련이 있습니다(예: `https://api.contoso.com`.
##<a name="request"></a>요청


메서드  |요청 URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles/{id}/operations <br/>/CertificateManagement/api/v1.0/smartcards/{id}/operations

###<a name="url-parameters"></a>URL 매개 변수
매개 변수 | 설명
---------|------------
id | 프로필 또는 스마트 카드의 식별자(GUID)입니다.

###<a name="request-headers"></a>요청 헤더
일반적인 요청 헤더에 대한 자세한 내용은 *CM REST API 서비스 세부 정보*에서 [HTTP 요청 및 응답 헤더](certificate-management-rest-api-service-details.md#http-request-and-response-headers)를 참조하세요.
###<a name="request-body"></a>요청 본문
없음

##<a name="response"></a>응답
###<a name="response-codes"></a>응답 코드
코드  |설명  
---------|---------
200     | 확인
204 | 내용 없음
403 | 사용할 수 없음
500 | 내부 오류

###<a name="response-headers"></a>응답 헤더
일반적인 응답 헤더에 대한 자세한 내용은 *CM REST API 서비스 세부 정보*에서 [HTTP 요청 및 응답 헤더](certificate-management-rest-api-service-details.md#http-request-and-response-headers)를 참조하세요.
###<a name="response-body"></a>응답 본문
성공하면 스마트 카드에서 사용자가 수행할 수 있는 가능한 작업 목록을 반환합니다. 이 목록에는 개수에 관계 없이 다음 항목이 포함될 수 있습니다. *OnlineUpdate*, *Renew*, *Recover*, *RecoverOnBehalf*, *Retire*, *Revoke*및 *Unblock*

##<a name="example"></a>예

###<a name="request"></a>요청
```
GET /certificatemanagement/api/v1.0/smartcards/438d1b30-f3b4-4bed-85fa-285e08605ba7/operations HTTP/1.1
```
###<a name="response"></a>응답
```
HTTP/1.1 200 OK

[
    "renew",
    "unblock",
    "retire"
]
```       
