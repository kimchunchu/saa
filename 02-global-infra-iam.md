# 챕터 2. 글로벌 인프라 & IAM 기초

## 2.1 글로벌 인프라

| 개념 | 설명 |
|---|---|
| **Region** | 지리적 독립 영역. 데이터 주권, 지연시간, 서비스 제공 여부, 가격으로 선택 |
| **Availability Zone (AZ)** | 리전 내 물리적으로 분리된 하나 이상의 데이터센터. 고가용성의 기본 단위 |
| **Edge Location** | CloudFront, Route 53, Global Accelerator가 사용하는 엣지 |
| **Local Zone / Wavelength / Outposts** | 초저지연 / 5G 엣지 / 온프레미스 AWS 하드웨어 |

## 2.2 공동 책임 모델

- **AWS**: 클라우드 *자체*의 보안 (물리 시설, 하이퍼바이저, 관리형 서비스 기반)
- **고객**: 클라우드 *내부*의 보안 (IAM, 데이터 암호화, OS 패치(EC2), 보안 그룹, 네트워크 설정)
- RDS 같은 관리형 서비스는 OS 패치를 AWS가 담당하지만, 데이터/접근 제어는 고객 책임

## 2.3 IAM 핵심

| 구성 요소 | 설명 |
|---|---|
| User | 사람/앱을 위한 장기 자격 증명 |
| Group | 사용자 묶음 (그룹 안에 그룹 불가) |
| Role | 임시 자격 증명. EC2, Lambda, 교차 계정 접근에 사용 |
| Policy | JSON 권한 문서 |

### 정책 평가 로직
1. 기본은 **암묵적 거부**
2. 명시적 **Allow**가 있으면 허용
3. 명시적 **Deny**는 항상 최우선

### 정책 유형
- **Identity-based**: 사용자/그룹/역할에 연결
- **Resource-based**: S3 버킷 정책, SQS 정책 등 (Principal 지정)
- **Permissions Boundary**: 개체가 가질 수 있는 최대 권한 한계
- **SCP (Organizations)**: 계정/OU의 최대 권한 한계 (권한을 부여하지는 않음)
- **Session Policy**: 임시 세션 권한 제한

## 2.4 모범 사례

- 루트 계정은 MFA 설정 후 일상 사용 금지
- **최소 권한 원칙**
- EC2에는 액세스 키 대신 **IAM Role (인스턴스 프로파일)** 사용
- 교차 계정은 **AssumeRole** 사용
- 외부 ID 제공자 연동: **IAM Identity Center(SSO)**, SAML 2.0, OIDC
- 모바일/웹 앱 사용자: **Amazon Cognito**

## 2.5 빈출 패턴

| 시나리오 | 답 |
|---|---|
| EC2 앱이 S3에 접근 | IAM Role |
| 다른 계정의 리소스 접근 | 교차 계정 Role + AssumeRole |
| 조직 전체에 특정 리전/서비스 사용 금지 | SCP |
| 여러 계정에 SSO | IAM Identity Center |
| 앱 사용자 로그인(소셜 포함) | Cognito User Pool |
| 앱 사용자에게 AWS 리소스 임시 권한 | Cognito Identity Pool |

## 2.6 체크리스트

- [ ] 정책 평가 순서를 설명할 수 있다
- [ ] SCP와 Permissions Boundary의 차이를 안다
- [ ] Role, STS의 동작 방식을 안다
