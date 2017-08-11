---
title: "인증서 요청 생성 옵션 가져오기 | Microsoft Docs"
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
ms.assetid: 36bd1fc9-3443-4028-90e7-a24fef0ec0ae
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5822da1b9cf8558becccff815fd0208923fcaaa0
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# <a name="get-certificate-request-generation-options"></a>인증서 요청 생성 옵션 가져오기

클라이언트 쪽 인증서 요청 생성을 위한 매개 변수를 가져옵니다.

**참고**: 이 항목에 표시된 URL은 API 배포 중 선택한 호스트 이름과 관련이 있습니다(예: `https://api.contoso.com`.
##<a name="request"></a>요청


메서드  |요청 URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{requestid}/certificaterequestgenerationoptions

###<a name="url-parameters"></a>URL 매개 변수
매개 변수 | 설명
--------|--------------
requestid| 필수 사항입니다. 인증서 요청 생성 매개 변수를 검색할 MIM CM 요청의 GUID 식별자입니다.

###<a name="request-headers"></a>요청 헤더
일반적인 요청 헤더에 대한 자세한 내용은 *CM REST API 서비스 세부 정보*에서 [HTTP 요청 및 응답 헤더](certificate-management-rest-api-service-details.md#http-request-and-response-headers)를 참조하세요.
###<a name="request-body"></a>요청 본문
없습니다.


##<a name="response"></a>응답
###<a name="response-codes"></a>응답 코드
코드  |설명  
---------|---------
200 | 확인
204 | 내용 없음
403 | 사용할 수 없음
500 | 내부 오류

###<a name="response-headers"></a>응답 헤더
일반적인 응답 헤더에 대한 자세한 내용은 *CM REST API 서비스 세부 정보*에서 [HTTP 요청 및 응답 헤더](certificate-management-rest-api-service-details.md#http-request-and-response-headers)를 참조하세요.
###<a name="response-body"></a>응답 본문
성공하면 CertificateRequestGenerationOptions 개체 목록을 반환합니다. 각 CertificateRequestGenerationOptions 개체는 클라이언트가 생성해야 하는 단일 인증서 요청에 해당하며 다음 속성이 있습니다.

속성| 설명
--------|-----------
Exportable | 요청에 대해 만든 개인 키를 내보낼 수 있는지 여부를 지정하는 값입니다.
FriendlyName | 등록된 인증서의 표시 이름입니다.
HashAlgorithmName | 인증서 요청 서명을 만들 때 사용되는 해시 알고리즘입니다.
KeyAlgorithmName | 공개 키 알고리즘입니다.
KeyProtectionLevel | 강력한 키 보호 수준입니다.
KeySize | 생성할 개인 키의 크기(비트)입니다.
KeyStorageProviderNames | 개인 키를 생성하는 데 사용할 수 있는 허용 가능한 KSP(키 저장소 공급자) 목록입니다. 인증서 요청을 생성하는 데 첫 번째 KSP를 사용할 수 없는 경우 성공할 때까지 지정된 KSP 중 어떤 것이라도 사용할 수 있습니다.
KeyUsages | 이 인증서 요청에 대해 만든 개인 키에서 수행할 수 있는 작업입니다. 기본값은 Signing입니다.
주체 | 주체 이름입니다.

**참고**: 이러한 속성에 대한 자세한 내용은 [Windows.Security.Cryptography.Certificates.CertificateRequestProperties 클래스](https://msdn.microsoft.com/library/windows/apps/br212079.aspx) 설명에 나와 있지만 이 클래스와 CertificateRequestGenerationOptions 개체 간에 일대일로 대응하지는 않습니다.

##<a name="example"></a>예

###<a name="request"></a>요청
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/certificaterequestgenerationoptions HTTP/1.1

```
###<a name="response"></a>응답
```
HTTP/1.1 200 OK

[
    {
        "Subject":"",
        "KeyAlgorithmName":"RSA",
        "KeySize":2048,
        "FriendlyName":"",
        "HashAlgorithmName":"SHA1",
        "KeyStorageProviderNames":[
            "Contoso Smart Card Key Storage Provider"
        ],
        "Exportable":0,
        "KeyProtectionLevel":0,
        "KeyUsages":3
    }
]
```       
