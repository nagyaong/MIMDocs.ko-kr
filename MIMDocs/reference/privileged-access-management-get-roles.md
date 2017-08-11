---
title: "PAM 역할 가져오기 | Microsoft Docs"
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
ms.assetid: d3c4f528-c3c8-41c1-905e-7eb84f074ce4
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4cb4bbc6c7696f354e5759a677ece88a4e606544
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# <a name="get-pam-roles"></a>PAM 역할 가져오기
계정이 후보가 되는 PAM 역할을 나열하기 위해 권한 있는 계정에서 사용합니다.

**참고**: 이 항목에 표시된 URL은 API 배포 중 선택한 호스트 이름과 관련이 있습니다(예: `http://api.contoso.com`.
##<a name="request"></a>요청


메서드  |요청 URL  
---------|---------
GET     |/api/pamresources/pamroles

###<a name="query-parameters"></a>쿼리 매개 변수
매개 변수 | 설명
----------|--------------
$filter | 선택 사항입니다. 필터링된 응답 목록을 반환하는 필터 식에 PAM 역할 속성을 지정합니다. 지원되는 연산자에 대한 자세한 내용은 [PAM REST API 서비스 세부 정보에서 필터링](privileged-access-management-rest-api-service-details.md#filtering)을 참조하세요.
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
성공적인 응답에는 다음 속성을 가진 하나 이상의 PAM 역할 컬렉션이 포함됩니다.

속성 | 설명
--------|-------------
RoleID | PAM 역할의 고유 식별자(GUID)입니다.
DisplayName | MIM 서비스에서 PAM 역할의 표시 이름입니다.
설명 | MIM 서비스에서 PAM 역할의 설명입니다.
TTL | 역할의 액세스 권한 최대 만료 시간(초)입니다.
AvailableFrom | 요청이 활성화되는 가장 빠른 시간입니다.
AvailableTo | 요청이 활성화되는 가장 늦은 시간입니다.
MFAEnabled | 이 역할에 대한 활성화 요청에 MFA 질문이 필요한지 여부를 나타내는 부울 값입니다.
ApprovalEnabled | 이 역할에 대한 활성화 요청에 역할 소유자의 승인이 필요한지 여부를 나타내는 부울 값입니다.
AvailibitlyWHTTP 요청 및 응답 헤더dowEnabled | 역할을 지정된 시간 간격 동안만 활성화할 것인지 여부를 나타내는 부울 값입니다.

##<a name="example"></a>예

###<a name="request"></a>요청
```
GET /api/pamresources/pamroles HTTP/1.1
```
###<a name="response"></a>응답
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamroles",
    "value":[
        {
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "DisplayName":"Allow AD Access ",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":false,
            "AvailabilityWindowEnabled":false
        },
        {
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "DisplayName":"ApprovalRole",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":true,
            "AvailabilityWindowEnabled":false
        }
    ]
}
```       
