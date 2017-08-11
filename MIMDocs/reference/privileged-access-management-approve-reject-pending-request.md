---
title: "보류 중인 PAM 요청 승인 또는 거부 | Microsoft Docs"
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
ms.assetid: 0632656f-ecf4-4090-85a8-216d5638140a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3e5506f96a22d1918cff7d0c9b822babb0f6cfa9
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# <a name="approve-or-reject-a-pending-pam-request"></a>보류 중인 PAM 요청 승인 또는 거부
PAM 역할로 상승시키기 위한 요청을 승인하거나 닫거나 거부하기 위해 권한 있는 계정에서 사용합니다.

**참고**: 이 항목에 표시된 URL은 API 배포 중 선택한 호스트 이름과 관련이 있습니다(예: `http://api.contoso.com`.
##<a name="request"></a>요청


메서드  |요청 URL  
---------|---------
POST     |/api/pamresources/pamrequeststoapprove({approvalId)/Approve <br/>/api/pamresources/pamrequeststoapprove({approvalId)/Reject

###<a name="url-parameters"></a>URL 매개 변수
매개 변수 | 설명
----------|-----------
approvalId | PAM의 승인 개체 식별자(GUID)입니다. `guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'`

###<a name="query-parameters"></a>쿼리 매개 변수
매개 변수 | 설명
----------|--------------
v | 선택 사항입니다. API 버전입니다. 포함되지 않은 경우 현재(최근에 발표된) 버전의 API가 사용됩니다. 자세한 내용은 [PAM REST API 서비스 세부 정보에서 버전 관리](privileged-access-management-rest-api-service-details.md#versioning)를 참조하세요.


###<a name="request-headers"></a>요청 헤더
일반적인 요청 헤더에 대한 자세한 내용은 *PAM REST API 서비스 세부 정보*에서 [HTTP 요청 및 응답 헤더](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers)를 참조하세요.
###<a name="request-body"></a>요청 본문
없습니다.

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
없음
##<a name="example"></a>예

###<a name="request"></a>요청
```
POST /api/pamresources/pamrequeststoapprove(guid'5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143')/Approve HTTP/1.1


```
###<a name="response"></a>응답
```
HTTP/1.1 200 OK

```       
