---
title: "샘플 등록 연습 | Microsoft Docs"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 92b97803-9475-4b90-9a6c-430f107a167d
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 574a4d6fc2d5d4a51483a371cf96c2d48c582a8b
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# <a name="sample-enrollment-walkthrough"></a>샘플 등록 연습
이 항목에서는 가상 스마트 카드 셀프 서비스 등록을 수행하는 데 필요한 단계를 보여 줍니다. 사용자가 설정한 PIN을 사용하는 자동 승인 요청을 보여 줍니다.
1.  클라이언트가 사용자를 인증한 다음 인증된 사용자가 등록할 수 있는 프로필 템플릿의 목록을 요청합니다.

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates.
    ```
    <br/>
2.  클라이언트가 결과 목록을 사용자에게 표시합니다. 사용자가 이름이 "가상 스마트 카드 VPN"인 vSC(가상 스마트 카드) 프로필 템플릿과 UUID *97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1*을 선택합니다.

3.  이전 단계에서 반환된 UUID를 사용하여 클라이언트가 선택한 프로필 템플릿의 등록 정책을 검색합니다.

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll
    ```
 <br/>   
4.  클라이언트가 반환된 정책에서 "DataCollection" 필드를 분석합니다. 이때 클라이언트가 "요청 이유"라는 제목의 단일 데이터 컬렉션 항목이 나타난 것을 확인합니다. 또한 클라이언트가 "collectComments" 플래그가 *false*로 설정되어 있는지 확인합니다. 이 플래그는 사용자에게 입력하라는 메시지를 표시하지 않도록 합니다.

5.  사용자가 인증서를 요구하는 이유를 입력한 후 클라이언트가 요청을 만듭니다.

    ```
    POST /CertificateManagement/api/v1.0/requests

    {
        "datacollection":"[{“Request Reason”:”Need VPN Certs”}]",
        "type":"Enroll",
        "profiletemplateuuid":"97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
        "comment":""
    }
    ```
<br/>
6.  서버가 성공적으로 요청을 만들고 클라이언트에게 요청 URI를 반환합니다. /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5.

7.  반환된 URI를 호출하여 클라이언트가 요청 개체를 검색합니다.

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5
    ```
<br/>
8.  클라이언트가 요청의 "state” 속성이 "approved"로 설정되어 있는지 확인합니다. 요청 실행을 시작할 수 있습니다.

9.  클라이언트가 요청을 검사하여 해당 요청과 이미 연결된 스마트 카드가 있는지 확인합니다. 이 작업은 "newsmartcarduuid" 매개 변수의 콘텐츠를 분석하여 수행됩니다.

10. 빈 GUID만 포함되어 있으므로 클라이언트가 MIM CM에서 아직 사용하지 않은 기존 카드를 사용하거나 프로필 템플릿이 가상 스마트 카드를 위해 구성되는 경우에는 카드를 하나 만들어야 합니다.

11. 후자는 등록 가능한 프로필 템플릿의 초기 쿼리를 통해 클라이언트에게 표시되므로(1단계) 클라이언트가 이제 가상 스마트카드 장치를 만들어야 합니다.

12. 클라이언트가 3단계에서 선택한 템플릿의 UUID를 사용하여 프로필 템플릿에서 스마트 카드 정책을 검색합니다.

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/configuration/smartcards
    ```
13. 스마트 카드 정책은 "DefaultAdminKeyHex" 속성에 카드에 대한 기본 관리자 키를 포함합니다. 스마트 카드를 만드는 경우 클라이언트는 스마트 카드의 초기 관리자 키를 이 키로 설정해야 합니다.  

14. 스마트 카드 장치를 만들면 클라이언트가 해당 장치를 요청에 할당해야 합니다.

    ```
    POST /CertificateManagement/api/v1.0/smartcards

    {
        "requestid":" C6BAD97C-F97F-4920-8947-BE980C98C6B5",
        "cardid":"23CADD5F-020D-4C3B-A5CA-307B7A06F9C9",
    }
    ```
<br/>
15. 서버가 새로 만든 스마트 카드 개체에 대한 URI로 클라이언트에 응답합니다. api/v1.0/smartcards/D700D97C-F91F-4930-8947-BE980C98A644.

16. 클라이언트가 스마트 카드 개체를 검색합니다.

    ```
    GET /CertificateManagement/api/v1.0/smartcards/ D700D97C-F91F-4930-8947-BE980C98A644
    ```
<br/>
17. 12단계에서 가져온 스마트 카드 정책에서 "diversifyadminkey" 플래그 값을 확인했기 때문에 클라이언트는 관리자 키를 다양화해야 한다는 것을 알고 있습니다.

18. 클라이언트가 제안된 관리자 키를 검색합니다.

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/diversifiedkey?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9
    ```
<br/>
19. 관리자 키를 설정하려면 클라이언트가 관리자로 카드에 인증해야 합니다. 이를 위해 클라이언트가 스마트 카드에서 인증 질문을 요청하고 서버에 제출합니다.

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=false
    ```

    서버는 HTTP 응답 본문에 질문 응답을 보냅니다. 16진수 챌린지 문자열을 찾고, base64로 변환한 후 URL에 매개 변수로 전달합니다. URL은 16진수 형식으로 다시 변환해야 하는 다른 응답을 반환합니다.
<br/>
20. 서버가 HTTP 응답 본문에 질문 응답을 보내면 클라이언트가 이를 사용하여 관리자 키를 다양화합니다.

21. 스마트 카드 정책에서 "UserPinOption" 필드를 검사하여 사용자가 자신들이 원하는 PIN을 입력해야 한다는 것을 클라이언트가 확인합니다.

22. 사용자가 대화 상자에 자신들이 원하는 PIN을 입력하면 클라이언트가 19단계에 나와 있는 대로 질문-응답 인증을 수행합니다. 여기서 유일한 차이점은 다양화된 플래그가 이제 "true"로 설정되어야 한다는 것입니다.

    ```
    GET /CertificateManagement/api/v1.0/ requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=true
    ```
<br/>
23. 클라이언트가 서버로부터 받은 응답을 사용하여 원하는 사용자 PIN을 설정합니다.

24. 이제 클라이언트가 인증서 요청을 생성할 준비가 되었습니다. 프로필 템플릿 인증서 생성 매개 변수를 쿼리하여 키/요청이 생성되는 방식을 확인합니다.

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificaterequestgenerationoptions.
    ```
<br/>
25. 서버가 키, 인증서 이름, 해시 알고리즘, 키 알고리즘, 키 크기 등의 내보내기에 대해 자세히 설명하는 단일 JSON 직렬화된 CertificateRequestGenerationOptions 개체로 응답합니다. 클라이언트가 이러한 매개 변수를 사용하여 인증서 요청을 생성합니다.

26. 단일 인증서 요청이 서버에 제출됩니다. 인증서 템플릿이 CA에 대한 인증서 보관을 지정하는 경우 클라이언트가 PFX blob의 암호를 해독하는 데 사용되는 PFX 암호를 추가로 지정할 수 있습니다. 즉, CA가 키 쌍을 생성한 다음 클라이언트에 보냅니다. 클라이언트가 몇 가지 설명을 추가할 수도 있습니다.

    ```
    POST /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificatedata

    {
       "pfxpassword":"",
       "certificaterequests":[
           "MIIDZDC…”
        ]
    }   
    ```
<br/>
27. 몇 초 후 서버가 단일 JSON 직렬화된 Microsoft.Clm.Shared.Certificate 개체로 응답합니다. "isPkcs7" 플래그를 확인하여 클라이언트가 이 응답이 PFX blob이 아니라는 것을 확인합니다. 클라이언트가 "pkcs7" 문자열에서 base64 디코딩으로 blob을 추출하고 설치합니다.

28. 클라이언트가 요청을 완료로 표시합니다.

    ```
    PUT /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5

    {
        "status":"Completed"
    }
    ```
