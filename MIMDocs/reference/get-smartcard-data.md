---
title: "스마트 카드 데이터 가져오기 | Microsoft Docs"
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
ms.assetid: 81f4b7cd-e4d9-4b11-b125-78cc9f183cf0
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9a84982971d169b5c9e7bd758cc74d795416975b
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-data"></a>스마트 카드 데이터 가져오기
현재 사용자가 수행할 수 있는 가능한 작업 목록과 함께 사용자의 스마트 카드 프로필 목록을 가져옵니다. 그런 다음 지정된 작업에 대한 요청을 시작할 수 있습니다.

**참고**: 이 항목에 표시된 URL은 API 배포 중 선택한 호스트 이름과 관련이 있습니다(예: `https://api.contoso.com`.
##<a name="request"></a>요청


메서드  |요청 URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards <br/> /CertificateManagement/api/v1.0/smartcards/{smartcarduuid}


###<a name="url-parameters"></a>URL 매개 변수
속성| 설명
---------|--------
smartcarduuid | 선택 사항입니다. MIM CM에 의해 표시되는 스마트 카드 UUID입니다. [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) 개체의 "uuid" 필드입니다.

###<a name="query-parameters"></a>쿼리 매개 변수
속성| 설명
---------|--------
cardid | 선택 사항입니다. MIM CM에 의해 표시되는 스마트 카드 UUID입니다. [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) 개체의 "uuid" 필드입니다.


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
성공한 경우 다음 속성을 가진 JSON 직렬화된 [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) 개체를 반환합니다.

Name | 설명
-----|-----------
AssignedUserUuid | 스마트 카드가 할당된 사용자의 식별자입니다.
Atr | 현재 초기화되는 카드에 대한 스마트 카드 ATR(재설정 응답) 문자열입니다.
설명 | 스마트 카드에 대한 설명입니다.
Flags | 스마트 카드를 설명하는 플래그입니다.
미들웨어 | 스마트 카드에 대한 미들웨어입니다.
ParentSmartcardUuid | 스마트 카드가 대체한 이전 스마트 카드의 식별자입니다.
PermanentSmartcardUuid | 스마트 카드와 연결된 영구적인 스마트 카드의 식별자입니다.
PrimarySmartcardUuid | 기본 스마트 카드의 식별자입니다.
ProfileTemplateUuid | 스마트 카드를 제어하는 정책 및 설정이 포함된 프로필 템플릿의 식별자입니다.
ProfileTemplateVersion | 스마트 카드 프로필을 만든 시점의 프로필 템플릿 버전입니다.
SerialNumber | 스마트 카드의 일련 번호입니다.
상태 | 스마트 카드의 상태입니다.
Uuid | 스마트 카드 프로필의 식별자입니다.

##<a name="example"></a>예

###<a name="request-1"></a>요청 1
```
GET /certificatemanagement/api/v1.0/smartcards?cardid=d1ef6869-5517-42a0-8f05-16ca1a0b834d HTTP/1.1

```
###<a name="response-1"></a>응답 1
```
HTTP/1.1 200 OK

{
    "Uuid":"438d1b30-f3b4-4bed-85fa-285e08605ba7",
    "Status":3,
    "Flags":1,
    "ParentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PrimarySmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PermanentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "AssignedUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "ProfileTemplateVersion":46,
    "Comment":"",
    "SerialNumber":"{d1ef6869-5517-42a0-8f05-16ca1a0b834d}",
    "Middleware":"MSBaseCSP",
    "Atr":"3b8d0180fba000000397425446590301c8"
}
```       
###<a name="request-2"></a>요청 2
```
GET /certificatemanagement/api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1
```
###<a name="response-2"></a>응답 2
```
HTTP/1.1 200 OK

{
    "Uuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Status":2,
    "Flags":1,
    "ParentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PrimarySmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PermanentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "AssignedUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "ProfileTemplateVersion":46,
    "Comment":"",
    "SerialNumber":"{bc88f13f-83ba-4037-8262-46eba1291c6e}",
    "Middleware":"MSBaseCSP",
    "Atr":"3b8d0180fba000000397425446590301c8"
}
```       
##<a name="see-also"></a>참고 항목

-[Microsoft.Clm.Shared.Smartcards.Smartcard 클래스](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx)
