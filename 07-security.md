# 챕터 7. 보안 서비스

## 7.1 암호화

| 서비스 | 설명 |
|---|---|
| **KMS** | 관리형 키 서비스. AWS 관리형 / 고객 관리형 키(CMK), 키 정책, 자동 교체 |
| **CloudHSM** | 전용 하드웨어, 고객이 키 완전 통제 (FIPS 140-2 Level 3) |
| **ACM** | TLS 인증서 발급/갱신 (ELB, CloudFront, API Gateway 연동) |
| **Secrets Manager** | 비밀 저장 + **자동 교체**(RDS 등) |
| **SSM Parameter Store** | 설정/비밀 저장 (교체 기능 없음, 저렴) |

- **전송 중 암호화**: TLS/HTTPS
- **저장 시 암호화**: EBS, S3, RDS 등 KMS 연동
- **Envelope Encryption**: 데이터 키로 데이터 암호화, KMS 키로 데이터 키 암호화
- KMS 키는 **리전 종속** (멀티 리전 키 별도 지원)

## 7.2 네트워크/애플리케이션 보호

| 서비스 | 보호 대상 |
|---|---|
| **AWS WAF** | L7 웹 공격(SQLi, XSS), IP/지역 차단, 속도 제한 (ALB, CloudFront, API Gateway) |
| **Shield Standard** | 모든 고객 무료, L3/L4 DDoS 방어 |
| **Shield Advanced** | 고급 DDoS 방어, 대응팀, 비용 보호 |
| **Network Firewall** | VPC 단위 상태 기반 방화벽 |
| **Firewall Manager** | 조직 전체 WAF/SG/방화벽 정책 중앙 관리 |

## 7.3 탐지 & 감사

| 서비스 | 기능 |
|---|---|
| **GuardDuty** | 위협 탐지 (CloudTrail, VPC Flow Logs, DNS 로그 분석) |
| **Inspector** | EC2/ECR/Lambda 취약점 스캔 |
| **Macie** | S3의 민감 데이터(PII) 탐지 |
| **Security Hub** | 보안 점검 결과 통합 대시보드 |
| **Detective** | 보안 이벤트 근본 원인 조사 |
| **CloudTrail** | API 호출 기록 (누가/언제/무엇을) |
| **Config** | 리소스 구성 변경 이력 및 규정 준수 평가 |
| **Artifact** | 규정 준수 보고서 다운로드 |

## 7.4 접근 방식 보안

- **Systems Manager Session Manager**: SSH/키 없이 인스턴스 접속 (포트 개방 불필요)
- **VPC Endpoint**: 인터넷 우회 접근
- **S3 Block Public Access**, **버킷 정책**으로 조건부 접근 (`aws:SourceVpce`, `aws:SecureTransport`)
- **Cognito**, **IAM Identity Center**, **Directory Service**(Managed AD, AD Connector)

## 7.5 빈출 패턴

| 시나리오 | 답 |
|---|---|
| SQL 인젝션/XSS 방어 | WAF |
| DDoS 방어 고도화 | Shield Advanced |
| S3 내 개인정보 탐지 | Macie |
| 의심스러운 활동/탈취 계정 탐지 | GuardDuty |
| API 호출 감사 | CloudTrail |
| 규정 준수 설정 평가 | Config |
| DB 비밀번호 자동 교체 | Secrets Manager |
| 키를 직접 완전히 통제 | CloudHSM |
| SSH 포트 없이 인스턴스 관리 | Session Manager |

## 7.6 체크리스트

- [ ] CloudTrail vs Config vs CloudWatch 역할을 구분한다
- [ ] Secrets Manager vs Parameter Store를 구분한다
- [ ] 암호화 대상별 KMS 적용 방법을 안다
