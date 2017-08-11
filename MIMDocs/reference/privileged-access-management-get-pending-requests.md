---
title: "보류 중인 PAM 요청 가져오기 | Microsoft Docs"
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
ms.assetid: 005dc8fd-d73e-4557-b485-5566f16537eb
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: aa1e8cd48b1bcca6e856bd553b6b92a062a08ff4
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# <a name="get-pending-pam-requests"></a>보류 중인 PAM 요청 가져오기
승인이 필요한 보류 중인 요청 목록을 반환하기 위해 권한 있는 계정에서 사용합니다.

**참고**: 이 항목에 표시된 URL은 API 배포 중 선택한 호스트 이름과 관련이 있습니다(예: `http://api.contoso.com`.
##<a name="request"></a>요청

메서드  |요청 URL  
---------|---------
GET     |/api/pamresources/pamrequeststoapprove

###<a name="query-parameters"></a>쿼리 매개 변수
매개 변수 | 설명
----------|--------------
$filter | 선택 사항입니다. 필터 식에 보류 중인 PAM 요청 속성을 지정하여 필터링된 응답 목록을 반환합니다. 지원되는 연산자에 대한 자세한 내용은 [PAM REST API 서비스 세부 정보에서 필터링](privileged-access-management-rest-api-service-details.md#filtering)을 참조하세요.
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
성공적인 응답에는 다음 속성을 가진 PAM 요청 승인 개체 목록이 포함됩니다.

속성 | 설명
---------|-------------
RoleName | 승인이 필요한 역할의 표시 이름입니다.
요청or | 승인될 요청자의 사용자 이름입니다.
Justification | 사용자가 제공한 타당한 이유입니다.
RequestedTTL | 요청된 만료 시간(초)입니다.
요청edTime | 권한 상승을 위해 요청된 시간입니다.
CreationTime | 요청을 만든 시간입니다.
FIM요청ID | 단일 요소 “Value”와 PAM 요청의 고유 식별자(GUID)를 포함합니다.
요청orID | 단일 요소인 “Value”와 PAM 요청을 만든 Active Directory 계정에 대한 고유 식별자(GUID)를 포함합니다.
ApprovalObjectID | 단일 요소 “Value”와 승인 개체에 대한 고유 식별자(GUID)를 포함합니다.

##<a name="example"></a>예제

###<a name="request"></a>요청
```
GET /api/pamresources/pamrequeststoapprove HTTP/1.1
```
###<a name="response"></a>응답
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequeststoapprove",
    "value":[
        {
            "RoleName":"ApprovalRole",
            "Requestor":"PRIV.Jen",
            "Justification":"Justification Reason",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-11T22:25:00Z",
            "CreationTime":"2015-07-11T22:24:52.51Z",
            "FIMRequestID":{
                "Value":"9802d7b7-b4e9-4fe4-8f5c-649cda127e49"
            },
            "RequestorID":{
                "Value":"73257e5e-00b3-4309-a330-f1e607ff113a"
            },
            "ApprovalObjectID":{
                "Value":"5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143"
            }
        }
    ]
}
```       
