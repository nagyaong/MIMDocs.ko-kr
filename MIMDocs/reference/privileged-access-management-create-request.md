---
title: "PAM 요청 만들기 | Microsoft Docs"
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
ms.assetid: fe8b3374-9d32-4cc3-9328-f1eafeadfe8e
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: de28af5c49eb98c5a1ccfbd8ed9353cf202e9e66
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# <a name="create-pam-request"></a>PAM 요청 만들기
PAM 역할로 상승하기 위해 권한 있는 계정에서 사용합니다.

**참고**: 이 항목에 표시된 URL은 API 배포 중 선택한 호스트 이름과 관련이 있습니다(예: `http://api.contoso.com`.
##<a name="request"></a>요청


메서드  |요청 URL  
---------|---------
POST     |/api/pamresources/pamrequests

###<a name="query-parameters"></a>쿼리 매개 변수
매개 변수 | 설명
--------|-------------
Justification | 선택 사항입니다. 권한 상승 요청에 대해 사용자가 제공한 이유입니다.
RoleId| 필수 사항입니다. 상승할 PAM 역할의 고유 식별자(GUID)입니다.
RequestedTTL| 필수 사항입니다. 요청된 만료 시간(초)입니다.
요청edTime | 선택 사항입니다. 권한을 상승하는 시간입니다.  
v | 선택 사항입니다. API 버전입니다. 포함되지 않은 경우 현재(최근에 발표된) 버전의 API가 사용됩니다. 자세한 내용은 [PAM REST API 서비스 세부 정보에서 버전 관리](privileged-access-management-rest-api-service-details.md#versioning)를 참조하세요.

**참고**: *Justification*, *RoleId*, *RequestedTTL*및 *RequestedTime* 매개 변수는 쿼리 매개 변수가 아닌 요청 본문의 속성으로 지정할 수 있습니다. *v* 매개 변수만을 쿼리 매개 변수로 지정할 수 있습니다.

###<a name="request-headers"></a>요청 헤더
일반적인 요청 헤더에 대한 자세한 내용은 *PAM REST API 서비스 세부 정보*에서 [HTTP 요청 및 응답 헤더](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers)를 참조하세요.
###<a name="request-body"></a>요청 본문
선택 사항입니다. 위에서 언급했듯이 *Justification*, *RoleId*, *RequestedTTL*및 *RequestedTime* 매개 변수는 URL 쿼리 문자열의 속성으로 지정하는 대신 요청 본문의 속성으로 지정할 수 있습니다.

##<a name="response"></a>응답
###<a name="response-codes"></a>응답 코드
코드  |설명  
---------|---------
200 | 확인
401 | 권한 없음
403 | 사용할 수 없음
408 | 요청 시간 초과   
500 | 내부 서버 오류
503 | 서비스를 사용할 수 없음

###<a name="response-headers"></a>응답 헤더
일반적인 응답 헤더에 대한 자세한 내용은 *PAM REST API 서비스 세부 정보*에서 [HTTP 요청 및 응답 헤더](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers)를 참조하세요.
###<a name="response-body"></a>응답 본문
성공적인 응답에는 다음 속성을 가진 PAM 요청 개체가 포함됩니다.

속성 | 설명
--------|-------------
요청ID | PAM 요청에 대한 고유 식별자(GUID)입니다.
CreatorID | 요청을 만든 계정에 대한 MIM 서비스의 고유 식별자(GUID)입니다.
Justification | 권한 상승에 대한 이유입니다.
CreationTime | 요청을 만든 시간입니다.
Creation메서드 | 요청을 만드는 데 사용되는 방법입니다.
ExpirationTime | 요청의 만료 시간입니다.
RoleID| PAM 역할의 고유 식별자(GUID)입니다.
요청edTTL | 요청된 만료 시간 제한(초)입니다.
RequestedTime | 권한 상승을 위해 요청된 시간입니다.
RequestStatus | 요청의 상태입니다. 사용 가능한 값은 "Processing", "Active", "Closed", "Closing", "Expired", "PendingApproval", "PendingMFA", "Rejected"입니다.

##<a name="example"></a>예

###<a name="request-1"></a>요청 1
```
POST /api/pamresources/pamrequests?Justification=Sample+Reason&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=7200&RequestedTime=2015%2F07%2F11+23%3A40 HTTP/1.1
```
###<a name="response-1"></a>응답 1
```
HTTP/1.1 201 Created

{  
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"c0112f13-b16b-40ad-b547-07f23a7fba52",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":"Sample Reason",
    "CreationTime":"2015-07-11T23:38:09.036164-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"7200",
    "RequestedTime":"2015-07-12T06:40:00Z",
    "RequestStatus":"PendingApproval"
}
```       

###<a name="request-2"></a>요청 2
```
POST /api/pamresources/pamrequests?Justification=&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=3600&RequestedTime= HTTP/1.1
```
###<a name="response-2"></a>응답 2
```
HTTP/1.1 201 Created

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"504f9c49-00db-42bd-a157-ee5664617189",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":null,
    "CreationTime":"2015-07-11T23:07:30.2200123-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"3600",
    "RequestedTime":"2015-07-12T06:07:27.7229894Z",
    "RequestStatus":"PendingApproval"
}
```       
