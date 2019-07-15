---
title: PAM 활성화를 위해 또는 SSPR 시나리오에서 API를 통해 대체 Multi-Factor Authentication 공급자 사용 | Microsoft Docs
description: 사용자가 Privileged Access Management에서 역할을 활성화하고 셀프 서비스 암호 재설정을 사용할 때 사용자 지정 MFA API를 두 번째 보안 계층으로 설정합니다.
keywords: ''
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: mtillman
ms.date: 09/04/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 7fb111520f94541672fc56d0fd2ee95bfcd3a49e
ms.sourcegitcommit: f58926a9e681131596a25b66418af410a028ad2c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67690756"
---
# <a name="use-a-custom-multi-factor-authentication-provider-via-an-api-during-pam-role-activation-or-in-sspr"></a>PAM 역할 활성화 중에 또는 SSPR에서 API를 통해 사용자 지정 Multi-Factor Authentication 공급자 사용

Azure AD Premium 또는 Azure MFA의 고객은 Azure MFA를 PAM(Privileged Access Management) 역할 활성화 및 SSPR(셀프 서비스 암호 재설정)의 두 MIM 시나리오에 통합할 수 있습니다.

MIM 고객에게는 두 가지 추가 옵션이 제공됩니다.

 - MIM SSPR 시나리오에만 적용 가능하고 [OTP SMS 게이트로 셀프 서비스 암호 재설정 구성](https://docs.microsoft.com/en-us/previous-versions/mim/hh824692(v=ws.10)) 가이드에 문서화된 사용자 지정 일회용 암호 배달 공급자를 사용합니다.
 - 사용자 지정 다단계 인증 전화 통신 공급자를 사용합니다. MIM SSPR 및 PAM 시나리오 모두에 해당되며 이 문서에서 이에 대해 설명합니다.

이 문서에서는 고객이 개발한 API 및 통합 SDK를 통해 사용자 지정 다단계 인증 공급자와 MIM을 사용하는 방법을 설명합니다.  

## <a name="prerequisites"></a>전제 조건

사용자 지정 Multi-Factor Authentication 공급자 API와 MIM을 함께 사용하려면 다음이 필요합니다.

- 모든 후보 사용자의 전화 번호
- MIM 핫픽스 [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) 이상 - 공지 사항은 [버전 기록](reference/version-history.md) 참조
- SSPR 또는 PAM에 대해 구성된 MIM 서비스

## <a name="approach-using-custom-multi-factor-authentication-code"></a>사용자 지정 다단계 인증 코드를 사용하는 방법

### <a name="step-1-ensure-mim-service-is-at-version-452020-or-later"></a>1단계: MIM 서비스가 버전 4.5.202.0 이상인지 확인

MIM 핫픽스 [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) 이상의 버전을 다운로드하여 설치합니다.

### <a name="step-2-create-a-dll-which-implements-the-iphoneserviceprovider-interface"></a>2단계: IPhoneServiceProvider 인터페이스를 구현하는 DLL 만들기

DLL에는 세 가지 메서드를 구현하는 클래스가 포함되어야 합니다.

- `InitiateCall`: MIM 서비스가 이 메서드를 호출합니다. 이 서비스는 전화 번호 및 요청 ID를 매개 변수로 전달합니다.  메서드는 `Pending`, `Success` 또는 `Failed`의 `PhoneCallStatus` 값을 반환해야 합니다.
- `GetCallStatus`: `initiateCall`에 대한 이전 호출이 `Pending`을 반환한 경우 MIM 서비스는 이 메서드를 호출합니다. 이 메서드는 `Pending`, `Success` 또는 `Failed`의 `PhoneCallStatus` 값도 반환합니다.
- `GetFailureMessage`: `InitiateCall` 또는 `GetCallStatus`의 이전 호출이 `Failed`를 반환한 경우 MIM 서비스는 이 메서드를 호출합니다. 이 메서드는 진단 메시지를 반환합니다.

이러한 메서드의 구현은 스레드로부터 안전해야 하며, 또한 `GetCallStatus` 및 `GetFailureMessage`의 구현은 `InitiateCall`에 대한 이전 호출과 동일한 스레드에 의해 호출될 것이라고 가정해서는 안 됩니다.

DLL을 `C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\` 디렉터리에 저장합니다.

Visual Studio 2010 이상을 사용하여 컴파일할 수 있는 샘플 코드입니다.

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using Microsoft.IdentityManagement.PhoneServiceProvider;

namespace CustomPhoneGate
{
    public class CustomPhoneGate: IPhoneServiceProvider
    {
        string path = @"c:\Test\phone.txt";
        public PhoneCallStatus GetCallStatus(string callId)
        {
            int res = 2;
            foreach (string line in File.ReadAllLines(path))
            {
                var info = line.Split(new char[] { ';' });
                if (string.Compare(info[0], callId) == 0)
                {
                    if (info.Length > 2)
                    {
                        bool b = Int32.TryParse(info[2], out res);
                        if (!b)
                        {
                            res = 2;
                        }
                    }
                    break;
                }
            }
            switch(res)
            {
                case 0:
                    return PhoneCallStatus.Pending;
                case 1:
                    return PhoneCallStatus.Success;
                case 2:
                    return PhoneCallStatus.Failed;
                default:
                    return PhoneCallStatus.Failed;
            }       
        }
        public string GetFailureMessage(string callId)
        {
            string res = "Call ID is not found";
            foreach (string line in File.ReadAllLines(path))
            {
                var info = line.Split(new char[] { ';' });
                if (string.Compare(info[0], callId) == 0)
                {
                    if (info.Length > 2)
                    {
                        res = info[3];
                    }
                    else
                    {
                        res = "Description is not found";
                    }
                    break;
                }
            }
            return res;            
        }
        
        public PhoneCallStatus InitiateCall(string phoneNumber, Guid requestId, Dictionary<string,object> deliveryAttributes)
        {
            // Here should be some logic for performing voice call
            // For testing purposes we just write details in file             
            string info = string.Format("{0};{1};{2};{3}", requestId, phoneNumber, 0, string.Empty);
            using (StreamWriter sw = File.AppendText(path))
            {
                sw.WriteLine(info);                
            }
            return PhoneCallStatus.Pending;    
        }
    }
}
```
### <a name="step-3-backup-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>3단계: "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service"에 있는 MfaSettings.xml 백업

### <a name="step-4-edit-the-mfasettingsxml-file"></a>4단계: MfaSettings.xml 파일 편집

다음 줄을 업데이트하거나 지웁니다.

- 모든 구성 항목 줄 제거/지우기 

- 사용자 지정 전화 공급자를 사용하여 다음 줄을 업데이트하거나 MfaSettings.xml에 추가 <br>
`<CustomPhoneProvider>C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\CustomPhoneGate.dll</CustomPhoneProvider>`

### <a name="step-5-restart-mim-service"></a>5단계: MIM 서비스 다시 시작

서비스를 다시 시작한 후에는 SSPR 및/또는 PAM을 사용하여 사용자 지정 ID 공급자로 기능의 유효성을 검사합니다.

> [!NOTE] 
> 설정을 되돌리려면 MfaSettings.xml을 3단계의 백업 파일로 바꿉니다.


## <a name="next-steps"></a>다음 단계

- [Azure Multi-Factor Authentication 서버 시작](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)
- [Azure Multi-Factor Authentication이란](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [MIM 버전 릴리스 기록](./reference/version-history.md)
