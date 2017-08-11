---
title: "스마트 카드 제안 PIN 가져오기 | Microsoft Docs"
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
ms.assetid: ced93932-9912-4b32-9586-ada69b38a796
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 08d28819402dd56f996d39aa03b8ac829bef0838
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-proposed-pin"></a>스마트 카드 제안 PIN 가져오기
서버에서 생성된 사용자 PIN을 가져옵니다.

**참고**: 프로필 템플릿 정책에서 지정한 경우에만 서버가 PIN을 설정합니다. 그렇지 않으면 사용자가 PIN을 제공해야 합니다.

**참고**: 이 항목에 표시된 URL은 API 배포 중 선택한 호스트 이름과 관련이 있습니다(예: `https://api.contoso.com`.
##<a name="request"></a>요청


메서드  |요청 URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards/{id}/serverproposedpHTTP 요청 및 응답 헤더

###<a name="url-parameters"></a>URL 매개 변수
매개 변수 | 설명
---------|------------
ID | 스마트 카드 식별자(MIM CM 관련)입니다. Microsft.Clm.Shared.Smartcard 개체에서 가져옵니다.
###<a name="query-parameters"></a>쿼리 매개 변수
매개 변수 | 설명
---------|------------
atr | 카드 atr입니다.
cardid | 카드 ID입니다.
challenge | 스마트 카드에서 발급한 질문을 나타내는 base-64 인코딩 문자열입니다.

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
성공하면 서버가 제안한 PIN을 나타내는 문자열을 반환합니다.

##<a name="example"></a>예

###<a name="request"></a>요청
```
GET GET /CertificateManagement/api/v1.0/smartcards/C6BAD97C-F97F-4920-8947-BE980C98C6B5/serverproposedpin HTTP/1.1
```
###<a name="response"></a>응답
```
HTTP/1.1 200 OK

... body coming soon
```       
