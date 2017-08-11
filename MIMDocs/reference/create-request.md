---
title: "요청 만들기 | Microsoft Docs"
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
ms.assetid: 80fe0656-6fb2-400c-9ef8-5f62b61b2a1b
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d5af8767583cb6999de399de58b321147846291a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# <a name="create-request"></a>요청 만들기
MIM CM 요청을 만듭니다.

**참고**: 이 항목에 표시된 URL은 API 배포 중 선택한 호스트 이름과 관련이 있습니다(예: `https://api.contoso.com`.
##<a name="request"></a>요청


메서드  |요청 URL  
---------|---------
POST     |/CertificateManagement/api/v1.0/requests

###<a name="url-parameters"></a>URL 매개 변수
없음

###<a name="request-headers"></a>요청 헤더
일반적인 요청 헤더에 대한 자세한 내용은 *CM REST API 서비스 세부 정보*에서 [HTTP 요청 및 응답 헤더](certificate-management-rest-api-service-details.md#http-request-and-response-headers)를 참조하세요.
###<a name="request-body"></a>요청 본문
요청 본문에는 다음 속성이 포함됩니다.

속성 | 설명
---------|-----------
profiletemplateuuid | 필수 사항입니다. 사용자가 요청을 만드는 데 사용하는 프로필 템플릿의 GUID입니다.
datacollection | 필수 사항입니다. 등록자가 제공해야 하는 데이터를 나타내는 이름-값 쌍 컬렉션입니다. 제공되어야 하는 필요한 데이터 컬렉션은 프로필 템플릿의 워크플로 정책에서 검색할 수 있습니다. 빈 컬렉션을 지정할 수 있습니다.
target | 선택 사항입니다. 요청이 만들어질 대상 사용자의 GUID입니다. 지정하지 않으면 현재 사용자로 기본 설정됩니다.
형식 | 필수 사항입니다. 만들어지는 요청의 형식입니다. 사용 가능한 요청 형식은 "Enroll", "Duplicate", "OfflineUnblock", "OnlineUpdate", "Renew", "Recover", "RecoverOnBehalf", "Reinstate", "Retire", "Revoke", "TemporaryCards" 및 "Unblock"입니다.<br/>**참고**: 모든 요청 형식이 모든 프로필 템플릿에서 지원되는 것은 아닙니다. 예를 들어 소프트웨어 프로필 템플릿에서는 차단 해제 작업을 지정할 수 없습니다.
설명 | 필수 사항입니다. 사용자가 입력할 수 있는 설명입니다. 워크플로 정책에서 이 속성이 필요한지 여부를 정의합니다. 빈 문자열을 지정할 수 있습니다.
priority | 선택 사항입니다. 요청의 우선 순위입니다. 지정하지 않으면 프로필 템플릿 설정에서 결정한 대로 기본 요청 우선 순위가 사용됩니다.


##<a name="response"></a>응답
###<a name="response-codes"></a>응답 코드
코드  |설명  
---------|---------
201     | 만든 날짜
403 | 사용할 수 없음
500 | 내부 오류

###<a name="response-headers"></a>응답 헤더
일반적인 응답 헤더에 대한 자세한 내용은 *CM REST API 서비스 세부 정보*에서 [HTTP 요청 및 응답 헤더](certificate-management-rest-api-service-details.md#http-request-and-response-headers)를 참조하세요.
###<a name="response-body"></a>응답 본문
성공한 경우 새로 만든 요청에 대한 URI를 반환합니다.
##<a name="example"></a>예

###<a name="request-1"></a>요청 1
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "datacollection":"[]",
    "type":"Enroll",
    "profiletemplateuuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "comment":""
}
```
###<a name="response-1"></a>응답 1
```
HTTP/1.1 201 Created

"api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099"
```
###<a name="request-2"></a>요청 2
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{  
    "datacollection":"[]",
    "type":"Unblock",
    "smartcard":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "comment":""
}
```
###<a name="response-2"></a>응답 2
```
HTTP/1.1 201 Created

"api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc"
```       

###<a name="request-3"></a>요청 3
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "profiletemplateuuid" : "97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
    "datacollection":
    [
        {"name" : "pavle"},
        {"city" : "seattle"}
    ],
    "target" : "97CC3493-F556-4C9B-9D8B-982434201527",
    "type" : "Enroll",
    "comment" : "LALALALA",
    "priority" :  "4"
}
```
##<a name="see-also"></a>참고 항목

- [Microsoft.Clm.Provision.RequestOperations.InitiateEnroll 메서드](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateenroll.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateOfflineUnblock 메서드](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateofflineunblock.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateRecover 메서드](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiaterecover.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateRetire 메서드](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateretire.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateUnblock 메서드](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateunblock.aspx)
