---
title: "PAM 요청 가져오기 | Microsoft Docs"
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
ms.assetid: 620eebd6-e4c3-473b-b824-ee6cfe83e509
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 00467b6443d3e6e0511ee1817a945ffe569b99b0
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# <a name="get-pam-requests"></a>PAM 요청 가져오기
이전에 게시된 PAM 요청 기록을 반환하기 위해 권한 있는 계정에서 사용됩니다.

**참고**: 이 항목에 표시된 URL은 API 배포 중 선택한 호스트 이름과 관련이 있습니다(예: `http://api.contoso.com`.
##<a name="request"></a>요청


메서드  |요청 URL  
---------|---------
GET     |/api/pamresources/pamrequests

###<a name="query-parameters"></a>쿼리 매개 변수
매개 변수 | 설명
----------|--------------
$filter | 선택 사항입니다. 필터 식에 PAM 요청 속성을 지정하여 필터링된 응답 목록을 반환합니다. 지원되는 연산자에 대한 자세한 내용은 [PAM REST API 서비스 세부 정보에서 필터링](privileged-access-management-rest-api-service-details.md#filtering)을 참조하세요.
v | 선택 사항입니다. API 버전입니다. 포함되지 않은 경우 현재(최근에 발표된) 버전의 API가 사용됩니다. 자세한 내용은 [PAM REST API 서비스 세부 정보에서 버전 관리](privileged-access-management-rest-api-service-details.md#versioning)를 참조하세요.

###<a name="request-headers"></a>요청 헤더
일반적인 요청 헤더에 대한 자세한 내용은 *PAM REST API 서비스 세부 정보*에서 [HTTP 요청 및 응답 헤더](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers)를 참조하세요.
###<a name="request-body"></a>요청 본문
없음

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
성공적인 응답에는 다음 속성을 가진 PAM 요청 개체의 목록이 포함됩니다.

속성 | 설명
--------|-------------
요청ID | PAM 요청에 대한 고유 식별자(GUID)입니다.
CreatorID | PAM 요청을 만든 Active Directory 계정에 대한 고유 식별자(GUID)입니다.
Justification | 권한 상승에 대한 이유입니다.
DisplayName | MIM에서 PAM 요청 표시 이름입니다.
CreationTime | 요청을 만든 시간입니다.
Creation메서드 | 요청을 만드는 데 사용되는 방법입니다.
ExpirationTime | 요청의 만료 시간입니다.
RoleID| PAM 역할의 고유 식별자(GUID)입니다.
요청edTTL | 요청된 만료 시간 제한(초)입니다.
RequestedTime | 권한 상승을 위해 요청된 시간입니다.
요청edStatus | 요청의 상태입니다. 가능한 값은 다음과 같습니다. "Active", "Closed", "Closing", "Expired", "Pending Approval" 및 "Rejected"

##<a name="example"></a>예

###<a name="request"></a>요청
```
GET /api/pamresources/pamrequests?$filter=CreationTime%20gt%20datetime'2015-06-12T04:49:32.431Z'%20and%20CreationTime%20lt%20datetime'2015-07-13T04:49:32.432Z' HTTP/1.1
```

###<a name="response"></a>응답
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests",
    "value":[
        {
            "RequestId":"b22e1343-9a2b-4e33-a70a-1bb7b2d405b9",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":null,
            "CreationTime":"2015-06-23T11:34:38.58Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-06-23T12:34:38.847Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-06-23T11:34:36.417Z",
            "RequestStatus":"Expired"
        },
        {
            "RequestId":"3a98d1c7-524d-4b72-9da7-bd9f907eab55",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"Reason for Request",
            "CreationTime":"2015-07-12T04:35:14.433Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-07-12T04:43:51.95Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"12960000",
            "RequestedTime":"2015-07-12T04:35:00Z",
            "RequestStatus":"Closed"
        },
        {
            "RequestId":"f5e80be1-e9a3-42c4-81f8-4be5a4a429f4",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":null,
            "CreationTime":"2015-07-12T04:48:17.46Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-07-12T05:48:17.853Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-12T04:48:14.057Z",
            "RequestStatus":"Active"
        },
        {
            "RequestId":"b0f0ddc0-c809-4770-9d39-0d706f97a2de",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"",
            "CreationTime":"2015-06-30T07:01:13.147Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"0001-01-01T00:00:00",
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-06-30T07:01:13.119Z",
            "RequestStatus":"Rejected"
        },
        {
            "RequestId":"5ec10e61-cdd1-404e-a18e-740467d87dbf",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"Example Reason",
            "CreationTime":"2015-07-12T04:49:09.963Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"0001-01-01T00:00:00",
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "RequestedTTL":"12960000",
            "RequestedTime":"2015-07-12T04:50:00Z",
            "RequestStatus":"PendingApproval"
        }
    ]
}
```       
