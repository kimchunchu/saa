# 챕터 11. 모니터링 & 거버넌스

## 11.1 모니터링

| 서비스 | 기능 |
|---|---|
| **CloudWatch Metrics** | 지표 수집 (EC2 기본 5분, 상세 1분). 메모리/디스크는 에이전트 필요 |
| **CloudWatch Alarms** | 임계값 알람 → SNS / ASG / EC2 액션 |
| **CloudWatch Logs** | 로그 수집/보관, Logs Insights 쿼리, 구독 필터 |
| **EventBridge** | 이벤트 기반 자동화 |
| **X-Ray** | 분산 추적 |
| **CloudTrail** | API 감사 로그 |
| **Config** | 구성 변경 이력 / 규정 준수 |
| **VPC Flow Logs** | 네트워크 트래픽 메타데이터 |

## 11.2 거버넌스 & 관리

| 서비스 | 기능 |
|---|---|
| **Organizations** | 다중 계정 관리, OU, SCP, 통합 청구 |
| **Control Tower** | 베스트 프랙티스 기반 멀티 계정 랜딩 존 구성 |
| **Service Catalog** | 승인된 제품 카탈로그 |
| **Systems Manager** | 패치, 런북, Session Manager, Parameter Store |
| **CloudFormation** | IaC(JSON/YAML), 스택, 변경 세트, 드리프트 감지 |
| **Resource Access Manager (RAM)** | 계정 간 리소스 공유 (서브넷, TGW 등) |
| **Trusted Advisor** | 모범 사례 점검 |
| **Health Dashboard** | AWS 서비스/계정 이벤트 |

## 11.3 Well-Architected Framework 6대 필라

1. 운영 우수성
2. 보안
3. 안정성
4. 성능 효율성
5. 비용 최적화
6. 지속 가능성

## 11.4 빈출 패턴

| 시나리오 | 답 |
|---|---|
| EC2 메모리 사용률 모니터링 | CloudWatch Agent |
| 인프라를 코드로 반복 배포 | CloudFormation |
| 다중 계정 환경 표준화 | Control Tower |
| 누가 보안 그룹을 변경했는지 | CloudTrail |
| 리소스가 규정에 맞는지 지속 평가 | Config Rules |
| 여러 계정이 서브넷 공유 | RAM |
| 마이크로서비스 지연 병목 추적 | X-Ray |

## 11.5 체크리스트

- [ ] CloudWatch / CloudTrail / Config의 차이를 안다
- [ ] Organizations와 SCP 개념을 이해한다
