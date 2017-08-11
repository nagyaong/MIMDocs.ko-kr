---
title: "프로필 템플릿 가져오기 | Microsoft Docs"
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
ms.assetid: b7d8ed76-168b-4cb8-b87c-cdb0976c179a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8736b328926445f6434bd037a1ecf2e5de9ee466
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-templates"></a>프로필 템플릿 가져오기
지정된 사용자가 등록할 수 있는 프로필 템플릿 목록을 가져옵니다. 이 메서드는 프로필 템플릿의 제한된 보기를 반환합니다. 반환된 프로필 템플릿 데이터는 요청하는 사용자가 등록해야 하는 프로필 템플릿(있는 경우)을 결정할 수 있도록 충분한 목록이 반환되어야 합니다. 워크플로 및 권한이 지정되지 않은 경우 사용자에게 보이는 프로필 템플릿이 반환됩니다.

**참고**: 이 항목에 표시된 URL은 API 배포 중 선택한 호스트 이름과 관련이 있습니다(예: `https://api.contoso.com`.
##<a name="request"></a>요청


메서드  |요청 URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates?\[targetuser\] 

###<a name="url-parameters"></a>URL 매개 변수
매개 변수| 설명
--------|-------------
targetuser| 선택 사항입니다. 프로필 템플릿을 반환하는 대상 사용자를 지정합니다. 지정하지 않으면 현재 사용자의 ID가 사용됩니다. <br/>**참고**: 현재는 현재 사용자만 지원됩니다.

###<a name="request-headers"></a>요청 헤더
일반적인 요청 헤더에 대해서는 서비스 항목을 참조하세요.
###<a name="request-body"></a>요청 본문
없음

##<a name="response"></a>응답
###<a name="response-codes"></a>응답 코드
코드  |설명  
---------|---------
200     | 확인
204 | 내용 없음
500 | 내부 오류

###<a name="response-headers"></a>응답 헤더
일반적인 응답 헤더의 경우 서비스 항목을 참조하세요.
###<a name="response-body"></a>응답 본문
성공하면 다음과 같은 속성을 가진 ProfileTemplateLimitedView 개체의 목록을 반환합니다.

속성| 유형| 설명
--------|-----|--------
이름| 문자열| 프로필 템플릿의 표시 이름입니다.
설명| string| 프로필 템플릿에 대한 설명입니다.
Uuid| Guid| 프로필 템플릿에 대한 식별자입니다.
IsSmartcardProfileTemplate| 부울| 템플릿이 스마트 카드 프로필 템플릿인지 여부를 나타냅니다.
IsVirtualSmartcardProfileTemplate| 부울| 프로필 템플릿이 가상 스마트 카드를 필요로 하는지 여부를 나타냅니다.

##<a name="example"></a>예제

###<a name="request"></a>요청
```
GET /certificatemanagement/api/v1.0/profiletemplates HTTP/1.1
```
###<a name="response"></a>응답
```
HTTP/1.1 200 OK

[
    {
        "Name":"FIM CM Sample Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"12bd5120-86a2-4ee1-8d05-131066871578",
        "IsSmartcardProfileTemplate":false,
        "IsVirtualSmartcardProfileTemplate":false
    },
    {
        "Name":"FIM CM Sample Smart Card Logon Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"2b7044cf-aa96-4911-b886-177947e9271b",
        "IsSmartcardProfileTemplate":true,
        "IsVirtualSmartcardProfileTemplate":false
    }
]

```       
