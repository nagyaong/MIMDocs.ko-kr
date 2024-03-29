---
title: FIM 2010 R2에서 Microsoft Identity Manager 2016으로 업그레이드 | Microsoft 문서
description: FIM 2010 R2 구성 요소를 업그레이드한 다음 MIM 2016의 새로운 구성 요소를 설치하는 방법을 알아봅니다.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 9471ccc1-bafe-46ee-b169-1464262380e1
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 36b5e97675d5900bf3b5348ad4857827c426e60e
ms.sourcegitcommit: 4f0b2883922bcb8fbef6b4284c35c6ca62c11565
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56952165"
---
# <a name="upgrade-from-forefront-identity-manager-2010-r2"></a>Forefront Identity Manager 2010 R2에서 업그레이드

환경이 FIM(Forefront Identity Manager) 2010 R2이며 MIM(Microsoft Identity Manager) 2016을 시도하려는 경우 이 문서를 가이드로 사용합니다. 이 업그레이드에는 세 가지 단계가 있습니다.

1.  AD(Active Directory) 도메인에 도메인 가입된 서버에 MIM 동기화 서비스(동기화)를 설치합니다. 그러면 동기화의 FIM 2010 R2 인스턴스가 대체됩니다.

2.  MIM 서비스 및 포털을 설치합니다. 이 시점에서 SSPR(셀프 서비스 암호 재설정) 등록 포털 및 서비스 포털을 설치하도록 선택할 수도 있습니다. 그러면 Privileged Access Management 기능 집합을 제외하고 설치됩니다.

3.  MIM 추가 기능 및 확장을 별도의 클라이언트 컴퓨터에 배포합니다. 여기에는 SSPR Windows 로그인 통합 클라이언트가 포함됩니다.


이 가이드에서는 다음이 이미 설정되어 있다고 가정합니다.
- 테스트 환경에 배포된 FIM 2010 R2
- Windows Server 2012, Windows Server 2012 R2 또는 Windows Server 2008 R2에서 실행 중인 서버
- FIM 2010 R2용으로 구성된 로컬 및 환경 필수 구성 요소(SQL Server, Exchange Server, SharePoint Services 등)


## <a name="preparation"></a>준비

1.  FIM 서비스 데이터베이스, FIM 동기화 데이터베이스, FIM 동기화 및 서비스 구성 및 소프트웨어를 백업합니다.

2.  FIM 2010 R2 구성 요소가 설치된 각 서버(예: *CORPIDM*)에서 Contoso\Administrator로 로그인합니다. 이 배포 예제에서는 FIM 2010 R2를 **MIM**으로 업그레이드하기 위해 관리자 권한이 필요합니다.

3.  MIM 소프트웨어를 다운로드하거나 압축을 풉니다.  이 소프트웨어가 없는 경우 [Microsoft Identity Manager 라이선스 및 다운로드](microsoft-identity-manager-licensing.md)를 참조하세요.

## <a name="upgrade-the-synchronization-service"></a>동기화 서비스 업그레이드

1.  FIM 2010 R2 동기화 서비스("동기화")가 배포되는 서버에 관리자로 로그인합니다.

2.  이 절차를 시작하기 전에 데이터베이스를 백업해야 합니다.

3.  **서비스** 콘솔을 열고 **Forefront Identity Manager 동기화 서비스**를 찾아서 중지합니다.

    ![서비스 콘솔 이미지](media/MIM-UpgFIM1.PNG)

4.  **MIM 동기화 서비스 설치 관리자**를 실행합니다. 설치 관리자가 기존 동기화 버전을 검색하고 업그레이드를 제안합니다. **업데이트** 단추를 클릭하여 계속 진행합니다.

5.  사용 조건에 동의하면 **다음** 을 클릭하여 계속 진행합니다.

6.  동기화가 사용하고 있는 서비스 계정에 대한 암호를 입력하고 **다음**을 클릭합니다.

    ![MIM 동기화 서비스 계정 구성 이미지](media/MIM-UpgFIM3.png)

7.  보안 그룹 이름이 올바른지 확인하고 **다음**을 클릭합니다.

    ![MIM 동기화 보안 그룹 구성 이미지](media/MIM-UpgFIM4.png)

8.  인바운드 RPC 통신에 대한 방화벽 규칙 확인란을 변경하지 않은 상태로 둡니다.

9. 설치 관리자가 FIM 2010 R2에서 MIM으로 동기화를 업그레이드할 준비가 되었습니다. **업그레이드** 를 클릭하여 업그레이드 프로세스를 시작합니다.

10. 이제 업그레이드가 진행됩니다. 업그레이드가 진행 중인 동안 설치 관리자를 종료하거나 컴퓨터를 다시 시작하지 마세요.

    ![MIM 동기화 설치 상태 이미지](media/MIM-UpgFIM7.png)

11. 업그레이드하는 동안 동기화 데이터베이스의 업그레이드와 관련된 경고가 표시됩니다. 이 절차를 시작하기 전에 DB를 백업하는 것이 좋습니다.

12. 업그레이드가 성공적으로 완료되면 **마침**을 클릭합니다.

    ![MIM 동기화 설치 성공 이미지](media/MIM-UpgSP1.png)

13. **동기화 서비스** 가 다시 시작되었습니다.

## <a name="upgrade-the-service-and-portal"></a>서비스 및 포털 업그레이드

1.  FIM 2010 R2 서비스 및 포털이 배포되는 서버에 관리자로 로그인합니다.

2.  **서비스** 콘솔을 열고 **Forefront Identity Manager 서비스**를 찾아서 중지합니다.

    ![서비스 콘솔 이미지](media/MIM-UpgFIM9.PNG)

3.  MIM 서비스 및 포털 설치 관리자를 실행합니다. **설치** 단추를 클릭하여 계속 진행합니다.

    ![MIM 서비스 및 포털 설치 이미지](media/MIM-UpgSP2.png)

4.  사용 조건에 동의하면 **다음** 을 클릭하여 계속 진행합니다.

5.  MIM 사용자 환경 개선 프로그램 화면에서 **다음** 을 클릭하여 계속 진행합니다.

6.  설치하려는 MIM 기능 및 구성 요소를 선택하고 완료되면 **다음**을 클릭합니다.

    ![사용자 지정 설치 이미지](media/MIM-UpgSP4.png)

    1.  **MIM 서비스:** 이 기능은 하나 이상의 서버에서 필수이며 같은 서버나 다른 서버에 SQL Server 데이터베이스 서버가 있어야 합니다.

    2.  **MIM 포털:** 이 기능은 하나 이상의 서버에서 필수이며 SharePoint 2013 Foundation이 필요합니다.

    3.  **MIM 암호 등록 포털:** 이 기능은 셀프 서비스 암호 재설정을 위해 필요합니다.

    4.  **MIM 암호 재설정 포털:** 이 기능은 암호 재설정을 위해 필요합니다.

7.  사용 중인 SQL Server 세부 정보를 FIM 서비스 데이터베이스에 입력합니다. 기존 데이터베이스를 다시 사용하고 해당 데이터를 유지하는 옵션을 선택합니다. **다음** 을 클릭하여 계속합니다.

8. 기존 데이터베이스를 다시 사용하는 옵션을 선택한 경우 데이터베이스를 백업하라는 알림이 표시됩니다.

9. 메일 서버의 세부 정보를 입력합니다. 메일 서버가 현재 서버에 있는 경우 메일 서버 위치로 "localhost"를 입력합니다. **다음** 을 클릭하여 계속합니다.

    ![메일 서버 연결 구성 이미지](media/MIM-UpgSP6.png)

10. 클라이언트의 유효성을 검사하기 위해 사용할 서비스에 대한 인증서를 선택합니다. FIM 서비스에서 이전에 사용한 로컬 인증서 저장소의 기존 인증서를 사용해야 합니다.

    ![서비스 인증서 구성 이미지](media/MIM-UpgSP7.png)

    1.  로컬 인증서 저장소 옵션을 선택한 경우 **인증서 선택** 단추를 클릭하고 팝업 창의 목록에서 인증서를 선택합니다. **확인**을 클릭한 후 **다음**을 클릭합니다.

        ![인증서 선택 팝업 창 이미지](media/MIM-UpgSP8.PNG)

11. MIM 서비스에 대한 서비스 계정 자격 증명을 구성합니다. 서비스 계정은 동기화 서비스에서 사용하는 서비스 계정과 동일한 계정을 사용할 수 없습니다. 이 계정은 FIM 서비스에서 사용한 계정과 동일해야 합니다.

    ![서비스 계정 구성 이미지](media/MIM-UpgSP9.png)

12. 이전 단계에서 구성한 MIM 서비스의 배포에 따라 MIM 동기화 서버의 세부 정보를 구성합니다.

    ![MIM 서비스 및 포털 동기화 구성 이미지](media/MIM-UpgSP10.png)

13. MIM 포털을 설치할 때 MIM 서비스 서버의 주소를 입력합니다. **다음**을 클릭합니다.

14. MIM 포털을 설치할 때 FIM 포털이 현재 호스트된 SharePoint 사이트 컬렉션의 URL을 입력합니다. **다음**을 클릭합니다.

## <a name="install-the-mim-password-registration-portal"></a>MIM 암호 등록 포털 설치

1. MIM 암호 등록 포털을 설치하는 경우 요청된 URL을 암호 등록 포털에 입력합니다. **다음**을 클릭합니다.

2. 서비스 및 포털을 사용하도록 클라이언트 및 최종 사용자에 대한 기능을 구성합니다.

    1.  **방화벽에서 포트 5725 및 5726 열기**여부를 선택합니다.

    2.  **MIM 포털 사이트에 대해 인증된 사용자 액세스 권한 부여** 여부를 선택합니다.

    3.  **다음**을 클릭합니다.

3. MIM 암호 등록을 위한 액세스 정보 및 자격 증명을 제공합니다.

    ![MIM 암호 등록 포털 구성 이미지](media/MIM-UpgSP15.png)

    1.  서비스 계정 이름(도메인 포함) 및 암호를 MIM 암호 등록에 입력합니다.

    2.  암호 등록 포털의 호스트에 대한 세부 정보(이름 및 포트(예: 8080))를 입력합니다.

    3.  **open port in firewall** (방화벽에서 포트 열기) 옵션을 선택합니다.

    4.  **다음**을 클릭합니다.

4. 다음 MIM 암호 등록 구성 화면에서 다음을 수행합니다.

    1.  MIM 서비스를 설치한 위치(일반적으로 동일한 시스템)를 MIM 암호 등록에 알립니다.

    2.  이전에 FIM 암호 재설정에 대해 구성된 대로, 이 포털에 엑스트라넷 및 인트라넷 사용자가 액세스할 수 있는지, 아니면 인트라넷 사용자만 액세스할 수 있는지 결정합니다.

## <a name="install-the-mim-password-reset-portal"></a>MIM 암호 재설정 포털 설치

1. MIM 암호 재설정 포털을 설치하는 경우 액세스 세부 정보 및 자격 증명을 MIM 암호 재설정에 입력합니다.

    ![FIM 암호 재설정 포털 구성 이미지](media/MIM-UpgSP17.png)

    1.  서비스 계정 이름(도메인 포함) 및 암호를 MIM 암호 재설정에 입력합니다.

    2.  암호 재설정 포털의 호스트에 대한 세부 정보(이름 및 포트(예: 8088))를 입력합니다.

    3.  **open port in firewall** (방화벽에서 포트 열기) 옵션을 선택합니다.

    4.  **다음**을 클릭합니다.

2. 다음 MIM 암호 재설정 구성 화면에서 다음을 수행합니다.

    1.  MIM 서비스를 설치한 위치를 MIM 암호 재설정에 알립니다.

    2.  이 포털에 엑스트라넷 및 인트라넷 사용자가 액세스할 수 있는지, 아니면 인트라넷 사용자만 액세스할 수 있는지 지정합니다.

## <a name="finish-installation-and-upgrade"></a>설치 및 업그레이드 완료

1. 모든 구성 정의가 성공적으로 완료되면 설치 페이지가 나타납니다. **설치** 를 클릭하여 MIM 서비스 및 포털의 설치 및 업그레이드를 시작합니다.

2. 이제 MIM 서비스 및 포털 설치 및 업그레이드가 진행됩니다. 설치하는 동안 설치 관리자를 취소하거나 컴퓨터를 다시 시작하지 마세요.

3. MIM 서비스 및 포털 설치(업그레이드)가 성공적으로 완료되면 확인 화면이 나타납니다. **마침** 을 클릭하여 설치를 완료하고 설치 관리자를 종료합니다.

4. **Forefront Identity Manager 서비스** 가 다시 시작되었습니다.

참고: SSPR을 위해 현재 FIM 추가 기능 및 확장이 사용자의 컴퓨터에 배포된 경우 모든 FIM 추가 기능 및 확장이 MIM 2016으로 업그레이드될 때까지 암호 재설정을 위해 새 MFA 전화 게이트를 구성하지 마세요.  FIM 2010 및 FIM 2010 R2 추가 기능 및 확장은 새 게이트를 인식하지 않으므로 오류가 발생하며 사용자가 암호 재설정을 완료할 수 없습니다.

Microsoft Identity Manager 2016 SP1 업그레이드 지침은 다음 [Microsoft Identity Manager 2016 Service Pack 1 update package](https://blogs.technet.microsoft.com/iamsupport/2016/11/08/microsoft-identity-manager-2016-service-pack-1-update-package/)(Microsoft Identity Manager 2016 서비스 팩 1 업데이트 패키지)를 참조하세요.
