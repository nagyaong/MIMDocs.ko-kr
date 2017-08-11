---
title: "요청에 스마트 카드 할당 | Microsoft Docs"
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
ms.assetid: 20f0bf6e-9ae0-4d21-8117-ed63e29315e6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2886186ade7058b034565501e200ab3773b0af31
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# <a name="assign-smart-card-to-a-request"></a>요청에 스마트 카드 할당
지정된 스마트 카드를 지정된 요청에 바인딩합니다. 바인딩되면 이 카드로만 요청을 실행할 수 있습니다.

**참고**: 이 항목에 표시된 URL은 API 배포 중 선택한 호스트 이름과 관련이 있습니다(예: `https://api.contoso.com`.
##<a name="request"></a>요청


메서드  |요청 URL  
---------|---------
POST     |/CertificateManagement/api/v1.0/smartcards

###<a name="url-parameters"></a>URL 매개 변수
없습니다.
###<a name="request-headers"></a>요청 헤더
일반적인 요청 헤더에 대한 자세한 내용은 *CM REST API 서비스 세부 정보*에서 [HTTP 요청 및 응답 헤더](certificate-management-rest-api-service-details.md#http-request-and-response-headers)를 참조하세요.
###<a name="request-body"></a>요청 본문
요청 본문에는 다음 속성이 포함됩니다.

속성 | 설명
---------|-----------
requestid | 스마트 카드가 바인딩되어야 하는 요청의 ID입니다.
cardid | 스마트 카드의 카드 ID입니다.
atr | 스마트 카드 ATR(재설정 응답) 문자열입니다.


##<a name="response"></a>응답
###<a name="response-codes"></a>응답 코드
코드  |설명  
---------|---------
201     | 만든 날짜
204 | 내용 없음
403 | 사용할 수 없음
500 | 내부 오류

###<a name="response-headers"></a>응답 헤더
일반적인 응답 헤더에 대한 자세한 내용은 *CM REST API 서비스 세부 정보*에서 [HTTP 요청 및 응답 헤더](certificate-management-rest-api-service-details.md#http-request-and-response-headers)를 참조하세요.
###<a name="response-body"></a>응답 본문
성공하면 새로 만든 스마트 카드 개체에 대한 URI를 반환합니다.
##<a name="example"></a>예

###<a name="request"></a>요청
```
POST /CertificateManagement/api/v1.0/smartcards HTTP/1.1

{
    "requestid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "cardid":"bc88f13f-83ba-4037-8262-46eba1291c6e",
    "atr":"3b8d0180fba000000397425446590301c8"
}

```
###<a name="response"></a>응답
```
HTTP/1.1 201 Created

"api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9"
```       
##<a name="see-also"></a>참고 항목

- [Microsoft.Clm.Provision.RequestOperations.CreateSmartcard 메서드(String, String, Request)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
