---
title: "스마트 카드 상태 업데이트 | Microsoft Docs"
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
ms.assetid: 598dace3-c6f2-447a-9301-c0b63ee38276
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: cb7b04539b8568a8b2ae83172b2aa8cddbf6174d
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# <a name="update-smartcard-status"></a>스마트 카드 상태 업데이트
스마트 카드의 상태를 업데이트합니다.

**참고**: 이 항목에 표시된 URL은 API 배포 중 선택한 호스트 이름과 관련이 있습니다(예: `https://api.contoso.com`.
## <a name="request"></a>요청


메서드  |요청 URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}

### <a name="url-parameters"></a>URL 매개 변수
매개 변수 | 설명
---------|------------
reqid | 필수 사항입니다. 요청 식별자(MIM CM 관련)입니다.
scid | 필수 사항입니다. 스마트 카드 식별자(MIM CM 관련)입니다. [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) 개체의 "uuid" 속성입니다.

### <a name="request-headers"></a>요청 헤더
일반적인 요청 헤더에 대한 자세한 내용은 *CM REST API 서비스 세부 정보*에서 [HTTP 요청 및 응답 헤더](certificate-management-rest-api-service-details.md#http-request-and-response-headers)를 참조하세요.
### <a name="request-body"></a>요청 본문
요청 본문에는 다음 속성이 포함됩니다.

속성 | 설명
---------|-----------
상태 | 요청을 설정하는 상태입니다. 예를 들어 "Retired"입니다.


## <a name="response"></a>응답
### <a name="response-codes"></a>응답 코드
코드  |설명  
---------|---------
200     | 확인
204 | 내용 없음
403 | 사용할 수 없음
500 | 내부 오류

### <a name="response-headers"></a>응답 헤더
일반적인 응답 헤더에 대한 자세한 내용은 *CM REST API 서비스 세부 정보*에서 [HTTP 요청 및 응답 헤더](certificate-management-rest-api-service-details.md#http-request-and-response-headers)를 참조하세요.
### <a name="response-body"></a>응답 본문
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

## <a name="example"></a>예

### <a name="request"></a>요청
```
PUT /certificatemanagement/api/v1.0/requests/b105403d-d021-41ea-9f11-be3d677d229e/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1

```
### <a name="response"></a>응답
```
HTTP/1.1 200 OK

{
    "Uuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Status":6,
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
## <a name="see-also"></a>참고 항목

- [Microsoft.Clm.Shared.Smartcards.Smartcard 클래스](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx))
