---
title: Microsoft Identity Manager 동기화 서비스 설치 | Microsoft 문서
description: 동기화 서비스를 설치 및 구성하여 MIM 2016 구성 요소를 시작합니다.
keywords: ''
author: fimguy
ms.author: barclayn
manager: mbaldwin
ms.date: 05/01/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d2f8c000205aacafaeb4e159ef692e9666b4b965
ms.sourcegitcommit: a98a4c1aee12016d480c400f4ff2c6aadb6518ee
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/03/2018
---
# <a name="install-mim-2016-mim-synchronization-service"></a>MIM 2016 설치: MIM 동기화 서비스

>[!div class="step-by-step"]
[« Exchange Server](prepare-server-exchange.md)
[MIM 서비스 및 포털 »](install-mim-service-portal.md)

> [!NOTE]
> 이 연습에서는 Contoso라는 회사의 샘플 이름과 값을 사용합니다. 해당 항목을 사용자의 정보로 바꿉니다. 예를 들면 다음과 같습니다.
> - 도메인 컨트롤러 이름 - **corpdc**
> - 도메인 이름 - **contoso**
> - MIM 서비스 서버 이름 - **corpservice**
> - MIM 동기화 서버 이름 - **corpsync**
> - SQL Server 이름 - **corpsql**
> - 암호 - **Pass@word1**

Microsoft Identity Manager 2016 구성 요소를 설치하려면 먼저 설치 패키지를 설정합니다.

1. ID 관리 동기화 서버 **corpsync**에 사용할 서버에 *contoso\miminstall*로 로그인합니다.

2. MIM 설치 패키지의 압축을 풀거나 MIM 이미지 DVD를 마운트합니다.

## <a name="install-mim-2016-sp1-synchronization-service"></a>MIM 2016 SP1 동기화 서비스 설치

1. 압축을 푼 MIM 설치 폴더에서 **Synchronization Service** 폴더로 이동합니다.

2. **MIM 동기화 서비스 설치 관리자**를 실행합니다. 설치 관리자의 지침을 따라 설치를 완료합니다.

3. 시작 화면에서 **다음**을 클릭합니다.

    ![MIM 설치 관리자 마법사 시작 이미지](media/install-mim-sync/MIM_Install1.png)

4. 사용 조건을 검토하고 이에 동의하는 경우 **다음**을 클릭합니다.

5. **사용자 지정 설치** 화면에서 **다음**을 클릭합니다.

    ![사용자 지정 설치 이미지](media/install-mim-sync/MIM_Install2.png)

6.  동기화 서비스 데이터베이스 구성 화면에서 다음을 선택합니다.

    1.  SQL Server 위치: **corpsql.contoso.com**라고 하는 **원격 컴퓨터**.

    2.  SQL Server 인스턴스: **기본 인스턴스**

    ![데이터베이스 연결 이미지](media/install-mim-sync/MIM_Install3.png)

7.  이전에 만든 계정에 따라 동기화 서비스 계정을 구성합니다.

    1.  서비스 계정: *MIMSync*

    2.  암호: *Pass@word1*

    3.  서비스 계정 도메인 또는 로컬 컴퓨터 이름: *contoso*

    ![서비스 계정 이미지](media/install-mim-sync/MIM_Install4.png)

8.  MIM 동기화 서비스 설치 관리자에서 다음과 같은 관련 보안 그룹을 입력합니다.

    1. 관리자 = *contoso\MIMSyncAdmins*

    2. 운영자 = *contoso\MIMSyncOperators*

    3. 연결자 = *contoso\MIMSyncJoiners*

    4. 커넥터 찾아보기 = *contoso\MIMSyncBrowse*

    5. WMI 암호 관리 = *contoso\MIMSyncPasswordReset*

    ![보안 그룹 이미지](media/install-mim-sync/MIM_Install5.png)

9. 보안 설정 화면에서 **인바운드 RPC 통신에 방화벽 규칙 사용**을 선택하고 **다음**을 클릭합니다.

10. **설치**를 클릭하여 MIM 동기화 서비스 설치를 시작합니다.

    1. MIM 동기화 서비스 계정에 대한 경고가 나타날 수 있습니다. **확인**을 클릭합니다.

    2. MIM 동기화 서비스가 설치됩니다.

    3. 암호화 키에 대한 백업 만들기에 대한 알림이 나타납니다. **확인**을 클릭한 다음 암호화 키 백업을 저장할 폴더를 선택합니다.

        ![MIM 동기화 백업 암호화 키 알림 이미지](media/MIM-Install7.png)

    4. 설치 관리자가 설치를 성공적으로 완료하면 **마침**을 클릭합니다.

    5. 변경 내용을 적용하려면 로그아웃했다가 그룹 멤버 자격으로 로그인해야 합니다. 로그아웃하려면 **예**를 클릭합니다.

>[!div class="step-by-step"]  
[« Exchange Server](prepare-server-exchange.md)
[MIM 서비스 및 포털 »](install-mim-service-portal.md)
