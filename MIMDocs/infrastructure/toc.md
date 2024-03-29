# [이해 및 탐색](../microsoft-identity-manager-2016.md)
## [MIM 2016이란?](../microsoft-identity-manager-2016.md)
### [MIM2016 SP1 PAM 배포 스크립트](../sp1-deployment-scripts.md)
## [PAM에 대해 알아보기](../pam/privileged-identity-management-for-active-directory-domain-services.md)
## [Azure의 하이브리드 보고](../identity-manager-hybrid-reporting-azure.md)
# [계획 및 디자인](../microsoft-identity-manager-2016-supported-platforms.md)
## [지원되는 플랫폼](../microsoft-identity-manager-2016-supported-platforms.md)
## [디렉터리에 연결](../supported-management-agents.md)
## [용량 계획](../capacity-planning-guide.md)
## [배포 토폴로지](../topology-considerations.md)
## [PAM 배포 계획](../pam/environment-overview.md)
# [배포 및 사용](../microsoft-identity-manager-deploy.md)
## [처음 배포](../microsoft-identity-manager-deploy.md)
### [도메인 설정](../preparing-domain.md)
### [서버 설치: Windows Server](../prepare-server-ws2016.md)
### [서버 설치: SQL](../prepare-server-sql2016.md)
### [서버 설치: SharePoint](../prepare-server-sharepoint.md)
### [서버 설치: Exchange](../prepare-server-exchange.md)
### [MIM 설치: 동기화](../install-mim-sync.md)
### [MIM 설치: 서비스 및 포털](../install-mim-service-portal.md)
### [MIM 설치: 데이터베이스 동기화](../install-mim-sync-ad-service.md)
## [Forefront Identity Manager 2010 R2에서 업그레이드](../microsoft-identity-manager-2016-upgrade-from-fim-2010-R2.md)
## [MIM 인증서 관리 설치](../mim-cm-deploy.md)
## [BHOLD 설치 항목](../bhold/bhold-installation-guide.md)
### [BHOLD core 설치](../bhold/bhold-core-installation.md)
### [BHOLD 통합 설치](../bhold/bhold-integration-installation.md)
### [BHOLD 증명 설치](../bhold/bhold-attestation-installation.md)
### [BHOLD 모델 생성기 설치](../bhold/bhold-model-generator-installation.md)
### [BHOLD 보고 설치](../bhold/bhold-reporting-installation.md)
### [BHOLD 분석 설치](../bhold/bhold-analytics-installation.md)
### [BHOLD 액세스 관리 커넥터 설치](../bhold/bhold-access-management-connector-install.md)
## [Password Change Notification Service (암호 변경 알림 서비스)](../deploying-mim-password-change-notification-service-on-domain-controller.md)
## [Identity Manager 하이브리드 보고](../working-with-identity-manager-hybrid-reporting.md)
## [셀프 서비스 암호 재설정](../working-with-self-service-password-reset.md)
## [MIM 인증서 관리자](../working-with-mim-certificate-manager.md)
### [스마트 카드 등록](../certificate-manager-for-non-administrators.md)
### [소프트웨어 인증서 만들기](../certificate-manager-for-software-certificates.md)
# [Privileged Access Management 사용](../pam/privileged-identity-management-for-active-directory-domain-services.md)
## [Privileged Access Management를 위한 MIM 구성](../pam/configuring-mim-environment-for-pam.md)
## [구성 요소 이해](../pam/principles-of-operation.md)
### [환경 개요](../pam/environment-overview.md)
### [계층 모델](../pam/tier-model-for-partitioning-administrative-privileges.md)
### [배스천 환경 계획](../pam/planning-bastion-environment.md)
### [역할 정의](../pam/defining-roles-for-pam.md)
### [고가용성 및 재해 복구](../pam/high-availability-disaster-recovery-considerations-bastion-environment.md)
### [하드웨어 및 소프트웨어 요구 사항](../pam/hardware-software-requirements.md)
### [1단계 - CORP 도메인](../pam/step-1-prepare-corp-domain.md)
### [2단계 - PRIV 도메인 컨트롤러](../pam/step-2-prepare-priv-domain-controller.md)
### [3단계 - PAM 서버](../pam/step-3-prepare-pam-server.md)
### [4단계 - PAM 서버에 MIM 설치](../pam/step-4-install-mim-components-on-pam-server.md)
### [5단계 - PRIV 및 CORP 간에 트러스트 설정](../pam/step-5-establish-trust-between-priv-corp-forests.md)
### [6단계 - 권한 있는 계정 만들기](../pam/step-6-transition-group-to-pam.md)
### [7단계 - 사용자의 액세스 권한 상승](../pam/step-7-elevate-user-access.md)
### [Windows Server 2016을 사용하여 MIM PAM 배포](../pam/deploy-pam-with-windows-server-2016.md)
### [Azure MFA 설정](../pam/use-azure-mfa-for-activation.md)
## [스크립트를 사용하여 PAM 구성](../pam/sp1-pam-configure-using-scripts.md)
### [1단계 PRIV 도메인 구성](../pam/sp1-step1-configuring-priv-domain.md)
### [2단계 CORP 도메인 구성](../pam/sp1-step2-configuring-corp-domain.md)
### [3단계 SQL 구성](../pam/sp1-step3-installing-configuring-sql.md)
### [4단계 SharePoint 구성](../pam/sp1-step4-configuring-sharepoint.md)
### [5단계 PAM 설치/구성](../pam/sp1-step5-configuring-pam.md)
### [6단계 PAM 트러스트 설정](../pam/sp1-step6-setup-pam-trust.md)
### [7단계 SID 기록/SID 필터링 설정](../pam/sp1-step7-setup-sidhistory-sidfiltering.md)
### [8단계 PAM 배포 확인](../pam/sp1-step8-pam-deployment-verification.md)
### [부록](../pam/sp1-pam-deployment-addendum.md)
# 인프라 관리
## [Best Practice Analyzer for Identity Manager (Identity Manager에 대한 모범 사례 분석기)](https://technet.microsoft.com/library/jj203402)
## [Password Change Notification Service](https://technet.microsoft.com/library/e27c0bc6-c808-4fdb-9e59-58feeb419308)(암호 변경 알림 서비스)
## 인증서 관리
### [CLMUtil Command-Line Tool](https://technet.microsoft.com/library/cc720647)(CLMUtil 명령줄 도구)
### [Configuration Profile Templates (구성 프로필 템플릿)](https://technet.microsoft.com/library/cc708656)
### [Using the certificate management website (인증서 관리 웹 사이트 사용)](https://technet.microsoft.com/library/cc720560)
### [Managing Smart Card Applications (스마트 카드 응용 프로그램 관리)](https://technet.microsoft.com/library/cc708681)
### [Backup and Restore](https://technet.microsoft.com/library/dd883245)(백업 및 복원)
## 셀프 서비스 암호 재설정
### [Programmatic User Registration (프로그래밍 방식으로 사용자 등록)](https://technet.microsoft.com/library/jj134294)
### [포털 사용자 지정](../reference/mim-portal-customizations.md)
## 서비스 및 포털
### [Kerberos](https://technet.microsoft.com/library/jj134299)
### [동적 로깅](mim-service-dynamic-logging.md)
### [Export Performance Guide](https://technet.microsoft.com/library/hh322883)(내보내기 성능 가이드)
## 보고
### [Reporting Custom Reports and Extensibility](https://technet.microsoft.com/library/jj133861)(사용자 지정 보고서 및 확장성 보고)
## [Microsoft identity software: Public release build versions (Microsoft Identity 소프트웨어: 공용 릴리스 빌드 버전)](https://blogs.technet.microsoft.com/iamsupport/idmbuildversions/)
# [참조](../reference/microsoft-identity-manager-2016-developer-reference.md)
## 개발자 참조
### [MIM 2016 개발자 참조](../reference/microsoft-identity-manager-2016-developer-reference.md)
### BHOLD
#### [BHOLD 개발자 참조](../reference/mim2016-bhold-developer-reference.md) 
### [인증서 관리 REST API 참조](../reference/certificate-management-rest-api-reference.md)
#### [CM REST API 서비스 세부 정보](../reference/certificate-management-rest-api-service-details.md)
#### [샘플 등록 연습](../reference/sample-enrollment-walkthrough.md)
#### [프로필 템플릿 가져오기](../reference/get-profile-templates.md)
#### [정책 작업](../reference/policy-operations.md)
#### [워크플로 정책 가져오기](../reference/get-workflow-policy.md)
#### [스마트 카드 정책 가져오기](../reference/get-smartcard-policy.md)
#### [요청 작업](../reference/request-operations.md)
##### [요청 만들기](../reference/create-request.md)
##### [요청 가져오기](../reference/get-request.md)
##### [요청 취소, 중단 또는 완료](../reference/cancel-abandon-complete-request.md)
#### [인증서 요청 작업](../reference/certificate-request-operations.md)
##### [인증서 요청 생성 옵션 가져오기](../reference/get-certificate-request-generation-options.md)
##### [인증서 응답 가져오기](../reference/get-certificate-responses.md)
#### [스마트 카드 작업](../reference/smartcard-operations.md)
##### [요청에 스마트 카드 할당](../reference/assign-smartcard-to-request.md)
##### [스마트 카드 데이터 가져오기](../reference/get-smartcard-data.md)
##### [스마트 카드 인증 응답 가져오기](../reference/get-smartcard-authentication-response.md)
##### [스마트 카드의 다양한 관리자 키 가져오기](../reference/get-smartcard-diversified-admin-key.md)
##### [스마트 카드 제안 PIN 가져오기](../reference/get-smartcard-proposed-pin.md)
##### [스마트 카드 상태 업데이트](../reference/update-smartcard-status.md)
#### [프로필 작업](../reference/profile-operations.md)
##### [프로필 데이터 가져오기](../reference/get-profile-data.md)
##### [프로필 상태 작업 가져오기](../reference/get-profile-state-operations.md)
#### [인증서 작업](../reference/certificate-operations.md)
##### [스마트 카드 또는 프로필 인증서 가져오기](../reference/get-smartcard-profile-certificates.md)
##### [사용자 인증서 가져오기](../reference/get-user-certificates.md)
### [권한 있는 액세스 관리 REST API 참조](../reference/privileged-access-management-rest-api-reference.md)
#### [PAM REST API 서비스 세부 정보](../reference/privileged-access-management-rest-api-service-details.md)
#### [PAM 역할 가져오기](../reference/privileged-access-management-get-roles.md)
#### [PAM 요청 만들기](../reference/privileged-access-management-create-request.md)
#### [PAM 요청 가져오기](../reference/privileged-access-management-get-requests.md)
#### [PAM 요청 닫기](../reference/privileged-access-management-close-request.md)
#### [보류 중인 PAM 요청 가져오기](../reference/privileged-access-management-get-pending-requests.md)
#### [보류 중인 PAM 요청 승인 또는 거부](../reference/privileged-access-management-approve-reject-pending-request.md)
#### [PAM 세션 정보 가져오기](../reference/privileged-access-management-get-session-info.md)
## 기술 참조
### [Microsoft Identity Manager 2016 SP1 용어](../reference/mim-2016-sp1-terms.md)
### [Resource Control Display Configuration XML Reference (리소스 컨트롤 표시 구성 XML 참조)](../reference/rcd-configuration-xml-reference.md)
### [관리 에이전트 실행 오류 코드](../reference/maerrorcodes.md)
### [Microsoft Identity Manager 2016에 대한 함수 참조](../reference/mim2016-functions-reference.md)
### [Microsoft Identity Manager 2016 암호 관리 참조](mim2016-password-management.md)
### BHOLD
#### [BHOLD 개념 가이드](../bhold/bhold-concepts-guide.md)
## 버전 기록
### [MIM 버전 기록](../reference/version-history.md)
### [BHOLD 버전 기록](../reference/version-bhold-history.md)
