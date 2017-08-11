---
title: "요청 취소, 중단 또는 완료 | Microsoft Docs"
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
ms.assetid: e29e0068-7602-455e-8a3a-690da9ca8eb5
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4f81c9d86006c477993b2ac8cc2e845de2c1d2f3
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# <a name="cancel-abandon-or-complete-a-request"></a>요청 취소, 중단 또는 완료
MIM CM 요청을 completed, canceled 또는 abandoned로 표시합니다.

**참고**: 이 항목에 표시된 URL은 API 배포 중 선택한 호스트 이름과 관련이 있습니다(예: `https://api.contoso.com`.
##<a name="request"></a>요청


메서드  |요청 URL  
---------|---------
PUT     |/CertificateManagement/api/v1.0/requests/{ID}

###<a name="url-parameters"></a>URL 매개 변수
속성| 설명
---------|--------
ID| 필수 사항입니다. 완료할 요청의 GUID입니다.


###<a name="request-headers"></a>요청 헤더
일반적인 요청 헤더에 대한 자세한 내용은 *CM REST API 서비스 세부 정보*에서 [HTTP 요청 및 응답 헤더](certificate-management-rest-api-service-details.md#http-request-and-response-headers)를 참조하세요.
###<a name="request-body"></a>요청 본문
요청 본문에는 다음 속성이 포함됩니다.

속성 | 설명
---------|-----------
상태 | 요청을 설정하는 상태입니다. 가능한 값은 다음과 같습니다. "Completed", "Canceled" 또는 "Abandoned"입니다.


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
성공하면 다음 속성을 가진 [Microsoft.Clm.Shared.Requests.Request](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx) 개체를 반환합니다. 이 개체는 완료됨으로 표시된 MIM CM 요청을 설명합니다.

Name | 설명
-----|------------
설명 | MIM CM 요청과 관련된 설명입니다.
완료 | MIM CM 요청이 완료된 시간입니다.
DataCollection | MIM CM 요청과 관련된 데이터 컬렉션 항목입니다.
DataCollectionFlags | MIM CM 요청에 대한 데이터 컬렉션 옵션입니다.
Flags | MIM CM 요청과 관련된 옵션입니다.
IsDataCollectionComplete | MIM CM 요청에 대해 데이터 컬렉션이 완료되었는지 여부를 나타내는 부울 값입니다.
IsEnrollmentAgent | MIM CM 요청을 실행하는 데 등록 에이전트가 필요한지 여부를 나타내는 부울 값입니다.
IsSmartcard | MIM CM 요청이 스마트 카드 요청 또는 소프트웨어 프로필 요청인지 여부를 나타내는 부울 값입니다.
NewProfileUuID | MIM CM 요청이 생성하는 새 소프트웨어 프로필의 ID입니다.
NewSmartcardUuID | MIM CM 요청이 할당한 새 스마트 카드의 ID입니다.
OldProfileUuID | MIM CM 요청을 만든 소프트웨어 프로필의 ID입니다.
OldSmartcardUuID | MIM CM 요청을 만든 스마트 카드의 ID입니다.
OrigHTTP 요청 및 응답 헤더atorUserUuID | MIM CM 요청을 시작한 사용자의 ID입니다.
우선 순위 | MIM CM 요청의 우선 순위입니다.
ProfileTemplateUuid | MIM CM 요청을 만든 프로필 템플릿의 ID입니다.
요청Type | MIM CM 요청의 형식입니다.
SecurityDescriptor | MIM CM 요청에 대한 보안 설명자입니다.
상태 | MIM CM 요청의 상태입니다.
Submitted | MIM CM 요청이 제출된 시간입니다.
TargetUserUuID | MIM CM 요청에 대한 대상 사용자의 ID입니다.
Uuid | MIM CM 요청에 대한 식별자입니다.

##<a name="example"></a>예제

###<a name="request"></a>요청
```
PUT /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1


{
    “status”:”Completed”
}
```
###<a name="response"></a>응답
```
HTTP/1.1 200 OK

{
    "Uuid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "RequestType":1,
    "Status":8,
    "Flags":2,
    "DataCollection":[

    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:36:21.93Z",
    "Completed":"2015-07-07T23:37:37.767Z",
    "NewProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "OldProfileUuid":"00000000-0000-0000-0000-000000000000",
    "NewSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "OldSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094534)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5219125)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}
```       
##<a name="see-also"></a>참고 항목

- [Microsoft.Clm.Provision.ExecuteOperations.Complete 메서드](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.complete.aspx)
- [Microsoft.Clm.Shared.Requests.Request](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx)
