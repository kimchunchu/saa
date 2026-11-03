# 챕터 12. 문제 풀이 요령 & 8주 학습 플랜

## 12.1 문제 풀이 요령

1. **키워드 먼저**: 비용 / 운영 오버헤드 / 가용성 / 보안 / 성능 중 무엇이 핵심인지 표시
2. **제약 조건 확인**: "최소 변경", "코드 수정 없이", "가장 빠르게" 등
3. **소거법**: 명백히 틀린 보기 2개를 먼저 제거
4. **관리형 서비스 우선**: 운영 부담 적은 쪽이 대체로 정답
5. **다답형**: 요구하는 개수만큼 선택, 서로 충돌하는 보기는 피하기
6. **시간 관리**: 65문항 / 130분 → 문항당 약 2분. 모르는 문제는 플래그 후 넘어가기
7. 채점되지 않는 시험 문항이 섞여 있으나 구분할 수 없으므로 모두 정성껏 풀기

## 12.2 자주 헷갈리는 쌍

| 비교 | 핵심 차이 |
|---|---|
| SG vs NACL | Stateful/인스턴스 vs Stateless/서브넷 |
| SQS vs SNS | Pull 큐 vs Push 팬아웃 |
| Multi-AZ vs Read Replica | 가용성 vs 읽기 확장 |
| CloudFront vs Global Accelerator | 캐싱 vs 네트워크 가속/고정 IP |
| EFS vs EBS vs S3 | 공유 파일 vs 단일 블록 vs 객체 |
| Secrets Manager vs Parameter Store | 자동 교체 vs 단순 저장 |
| CloudTrail vs CloudWatch vs Config | API 기록 vs 지표/로그 vs 구성 상태 |
| Gateway vs Interface Endpoint | S3/DDB 무료 vs 그 외 PrivateLink |
| Kinesis Streams vs Firehose | 직접 소비/재처리 vs 관리형 적재 |

## 12.3 8주 학습 플랜

| 주차 | 학습 내용 | 실습 |
|---|---|---|
| 1 | 시험 개요, IAM, 글로벌 인프라 (1~2장) | IAM 사용자/역할, MFA, Budgets |
| 2 | VPC 네트워킹 (3장) | 3-tier VPC, NAT, Endpoint |
| 3 | 컴퓨팅 (4장) | EC2 + ALB + ASG, Lambda |
| 4 | 스토리지 (5장) | S3 수명 주기/복제, EBS 스냅샷, EFS |
| 5 | 데이터베이스 (6장) | RDS Multi-AZ, DynamoDB |
| 6 | 보안 + 고가용성/DR (7~8장) | KMS, WAF, Route 53 Failover |
| 7 | 디커플링/분석/비용/모니터링 (9~11장) | SQS+SNS, Athena, CloudWatch 알람 |
| 8 | 모의고사 3~5회 + 오답 노트 | 약점 챕터 재학습 |

## 12.4 시험 직전 체크리스트

- [ ] 모의고사 점수 80% 이상을 연속으로 달성
- [ ] 오답 노트의 반복 실수 패턴 점검
- [ ] 시험 환경(온라인/센터), 신분증 준비
- [ ] 전날 충분한 수면, 새로운 내용 학습 금지

## 12.5 합격 이후

- 자격증은 3년 유효, 상위 자격(Professional 등) 취득 시 갱신 가능
- 다음 단계: SAP, DevOps Engineer, Security Specialty 등
