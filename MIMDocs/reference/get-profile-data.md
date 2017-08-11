---
title: "프로필 데이터 가져오기 | Microsoft Docs"
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
ms.assetid: 3eba062b-7adf-4766-9b94-cba1c7be2fd3
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 068497509b07b879fcadde6b60a9530d3d8db093
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-data"></a>프로필 데이터 가져오기
현재 사용자가 수행할 수 있는 가능한 작업 목록과 함께 사용자의 소프트웨어 인증서 프로필 목록을 가져옵니다. 그런 다음 지정된 작업에 대한 요청을 시작할 수 있습니다.

**참고**: 프로필 템플릿 정책에서 지정한 경우에만 서버가 PIN을 설정합니다. 그렇지 않으면 사용자가 PIN을 제공해야 합니다.

**참고**: 이 항목에 표시된 URL은 API 배포 중 선택한 호스트 이름과 관련이 있습니다(예: `https://api.contoso.com`.
##<a name="request"></a>요청


메서드  |요청 URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles<br/>/CertificateManagement/api/v1.0/profiles/{id} <br/>/CertificateManagement/api/v1.0/requests/{requestid}/profiles

###<a name="url-parameters"></a>URL 매개 변수
매개 변수 | 설명
---------|------------
ID | 반환할 프로필의 식별자(GUID)입니다.
requestId | 프로필을 반환할 요청의 식별자입니다.

###<a name="query-parameters"></a>쿼리 매개 변수
매개 변수 | 설명
---------|------------
상태 | 선택 사항입니다. 데이터를 검색할 프로필의 상태를 나타냅니다. 가능한 상태 형식은 다음과 같습니다. "Active", "Approved", "Canceled", "Completed", "Denied", "Executing", "Failed", "None" 및 "Pending" <br/>상태가 지정되지 않은 경우 상태에 관계없이 모든 프로필이 반환됩니다.
###<a name="request-headers"></a>요청 헤더
일반적인 요청 헤더에 대한 자세한 내용은 *CM REST API 서비스 세부 정보*에서 [HTTP 요청 및 응답 헤더](certificate-management-rest-api-service-details.md#http-request-and-response-headers)를 참조하세요.
###<a name="request-body"></a>요청 본문
없음

##<a name="response"></a>응답
###<a name="response-codes"></a>응답 코드
코드  |설명  
---------|---------
200 | 확인
204 | 내용 없음
403 | 사용할 수 없음
500 | 내부 오류

###<a name="response-headers"></a>응답 헤더
일반적인 응답 헤더에 대한 자세한 내용은 *CM REST API 서비스 세부 정보*에서 [HTTP 요청 및 응답 헤더](certificate-management-rest-api-service-details.md#http-request-and-response-headers)를 참조하세요.
###<a name="response-body"></a>응답 본문
성공한 경우 다음 속성을 가진 JSON 직렬화된 [Microsoft.Clm.Shared.Profiles.Profile](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx) 개체 목록을 반환합니다.

속성 | 설명
---------|------------
AssignedUserUuID | 프로필이 할당된 사용자의 식별자입니다.
설명 | 프로필에 대한 설명입니다.
Flags | 프로필을 설명하는 플래그입니다.
ParentProfileUuID | 프로필이 대체한 이전 프로필의 식별자입니다.
PrimaryProfileUuID | 기본 프로필의 식별자입니다.
ProfileOperations | 프로필의 현재 사용자가 수행할 수 있는 가능한 작업 목록입니다.
ProfileTemplateUuID | 프로필을 제어하는 정책 및 설정을 포함한 프로필 템플릿의 식별자입니다.
ProfileTemplateVersion | 프로필을 만든 시점의 프로필 템플릿 버전입니다.
상태 | 프로필의 상태입니다.
UuID | 프로필의 식별자입니다.


##<a name="example"></a>예

###<a name="request"></a>요청
```
GET /certificatemanagement/api/v1.0/profiles?status=Active HTTP/1.1
```
###<a name="response"></a>응답
```
HTTP/1.1 200 OK

[
    {
        "Uuid":"c0dd5c7d-ec35-4346-baca-3ad711e9722f",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"1c9e2606-fea2-4048-a6ac-b014e54c22df",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"5ad77b40-aa42-4533-9396-c9c59fd021a8",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"ff342953-c444-4dc7-b144-f5515d6460c6",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"1e3a31fe-699b-4a6b-945c-18b83c985bc1",
        "ProfileTemplateVersion":9,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable"
        ]
    }
]
```       
##<a name="see-also"></a>참고 항목

- [Microsoft.Clm.Shared.Profiles.Profile 클래스](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx)
