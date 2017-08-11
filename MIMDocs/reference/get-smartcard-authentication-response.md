---
title: "스마트 카드 인증 응답 가져오기 | Microsoft Docs"
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
ms.assetid: e05ec898-06cd-4c17-a4f4-8f3545af0f14
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1c7a6f5e7ebd1e6e4cfc0992607adb91743f343e
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-authentication-response"></a>스마트 카드 인증 응답 가져오기
기본 CSP 인증 질문에 대한 응답을 가져옵니다.

**참고**: 이 항목에 표시된 URL은 API 배포 중 선택한 호스트 이름과 관련이 있습니다(예: `https://api.contoso.com`.
##<a name="request"></a>요청


메서드  |요청 URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}/authenticationresponse

###<a name="url-parameters"></a>URL 매개 변수
매개 변수 | 설명
---------|------------
reqid | 필수 사항입니다. 요청 식별자(MIM CM 관련)입니다.
scid | 필수 사항입니다. 스마트 카드 식별자(MIM CM 관련)입니다. [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) 개체에서 가져옵니다.
###<a name="query-parameters"></a>쿼리 매개 변수
매개 변수 | 설명
---------|------------
atr | 선택 사항입니다. 스마트 카드 ATR(재설정 응답) 문자열입니다.
cardid | 필수 사항입니다. 카드 ID입니다.
challenge | 필수 사항입니다. 스마트 카드에서 발급한 질문을 나타내는 base-64 인코딩 문자열입니다.
diversified | 필수 사항입니다. 스마트 카드 관리자 키가 다양화되었는지 여부를 나타내는 부울 플래그입니다.


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
성공하면 질문 응답을 나타내는 바이트 BLOB을 반환합니다.

##<a name="example"></a>예

###<a name="request"></a>요청
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/authenticationresponse?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e&challenge=1hFD%2Bcz%2F0so%3D&diversified=False HTTP/1.1

```
###<a name="response"></a>응답
```
HTTP/1.1 200 OK

"F0Zudm4wPLY="
```       
##<a name="see-also"></a>참고 항목

- [Microsoft.Clm.Provision.ExecuteOperations.GetBaseCspResponse 메서드](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.getbasecspresponse.aspx)
