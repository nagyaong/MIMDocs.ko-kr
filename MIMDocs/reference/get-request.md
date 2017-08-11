---
title: "요청 가져오기 | Microsoft Docs"
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
ms.assetid: dcacf36c-0670-44d7-9f40-388667235271
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 56bb25f7282879943dc624f3c24b836c7ae4a58c
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# <a name="get-request"></a>요청 가져오기
하나 이상의 지정된 MIM CM 요청을 가져옵니다.

**참고**: 이 항목에 표시된 URL은 API 배포 중 선택한 호스트 이름과 관련이 있습니다(예: `https://api.contoso.com`.
##<a name="request"></a>요청


메서드  |요청 URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{id}

###<a name="url-parameters"></a>URL 매개 변수
속성| 설명
---------|--------
id| 선택 사항입니다. 검색할 요청의 GUID입니다.
###<a name="query-parameters"></a>쿼리 매개 변수
속성| 설명
---------|--------
targetuser| 선택 사항입니다. 요청의 대상 사용자입니다. 대상이 지정되지 않은 경우 대상 사용자에 관계 없이 모든 요청이 반환됩니다. <br/> **참고**: 현재는 "현재 사용자"만 지원됩니다.
상태| 선택 사항입니다. 검색할 요청의 상태를 나타냅니다. 가능한 상태 형식은 다음과 같습니다. *Approved*, *Canceled*, *Completed*, *Denied*, *Executing*, *Failed*, *None*및 *Pending* <br/>상태가 지정되지 않은 경우 상태에 관계없이 모든 요청이 반환됩니다.

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
성공할 경우 다음 속성을 갖는 하나 이상의 [Request](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requests.request.aspx) 개체를 반환합니다.

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

##<a name="example"></a>예

###<a name="request-1"></a>요청 1
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1

```
###<a name="response-1"></a>응답 1
```
HTTP/1.1 200 OK

{
    "Uuid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "RequestType":1,
    "Status":3,
    "Flags":2,
    "DataCollection":[

    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:36:21.93Z",
    "Completed":"0001-01-01T00:00:00",
    "NewProfileUuid":"00000000-0000-0000-0000-000000000000",
    "OldProfileUuid":"00000000-0000-0000-0000-000000000000",
    "NewSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "OldSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094534)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5219125)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}```       
###Request 2
```
GET /certificatemanagement/api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc HTTP/1.1
```
###Response 2
```
HTTP/1.1 200 OK

{ "Uuid":"0c96d73f-967b-420e-854a-43ad2a1504bc", "RequestType":5, "Status":3, "Flags":2, "DataCollection":[ { "Name":"Sample Data Item", "Value":null } ], "DataCollectionFlags":32, "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780", "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780", "Submitted":"2015-07-07T23:40:33.133Z", "Completed":"0001-01-01T00:00:00", "NewProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc", "OldProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc", "NewSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9", "OldSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9", "Priority":0, "Comment":"", "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3", "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000", "IsSmartcard":true, "IsEnrollmentAgent":false, "IsDataCollectionComplete":false }
```       

##See Also

- [Microsoft.Clm.Provision.FindOperations.FindRequest Method](http://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.findoperations.findrequests.aspx)
- [Microsoft.Clm.Shared.RequestPermission Enumeration](http://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requestpermission.aspx)
- [Microsoft.Clm.Shared.Requests.Request Class](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requests.request.aspx)
