# 챕터 8. 고가용성 & 재해 복구

## 8.1 설계 원칙

- **단일 장애점(SPOF) 제거**: 다중 AZ, 로드 밸런서, 오토 스케일링
- **느슨한 결합**: 큐/이벤트로 컴포넌트 분리
- **장애를 전제로 설계**: 자동 복구, 헬스 체크
- **무상태(Stateless)**: 세션은 ElastiCache/DynamoDB에 저장

## 8.2 RTO / RPO

- **RTO**: 복구 목표 시간 (얼마나 빨리 복구해야 하나)
- **RPO**: 복구 목표 시점 (얼마만큼의 데이터 손실까지 허용하나)

## 8.3 DR 전략 (비용 ↑ / 복구시간 ↓)

| 전략 | 설명 | RTO/RPO |
|---|---|---|
| **Backup & Restore** | 백업을 다른 리전에 저장, 필요 시 복원 | 시간 단위 |
| **Pilot Light** | 핵심(DB)만 상시 가동, 장애 시 나머지 기동 | 수십 분~시간 |
| **Warm Standby** | 축소된 전체 환경을 상시 가동 | 분 단위 |
| **Multi-Site Active-Active** | 두 리전 모두 트래픽 처리 | 거의 0 |

## 8.4 서비스별 HA/DR 기능

| 서비스 | 기능 |
|---|---|
| S3 | CRR, 버저닝 |
| EBS | 스냅샷 교차 리전 복사 |
| RDS | Multi-AZ, 교차 리전 Read Replica / 스냅샷 |
| Aurora | Global Database |
| DynamoDB | Global Tables, PITR |
| Route 53 | Failover 라우팅 + 헬스 체크 |
| **Elastic Disaster Recovery (DRS)** | 온프레미스/타 클라우드 → AWS 지속 복제 |
| **AWS Backup** | 백업 중앙화, 교차 리전/계정 복사 |

## 8.5 빈출 패턴

| 시나리오 | 답 |
|---|---|
| 리전 장애 시 최저 비용으로 DR | Backup & Restore / Pilot Light |
| 리전 장애에도 거의 무중단 | Multi-Site Active-Active |
| 웹 계층 AZ 장애 대비 | ASG + ALB 다중 AZ |
| DB 자동 장애 조치 | RDS Multi-AZ / Aurora |
| 온프레미스 서버 DR | Elastic Disaster Recovery |

## 8.6 체크리스트

- [ ] 4가지 DR 전략을 RTO/RPO/비용으로 비교한다
- [ ] 각 서비스의 교차 리전 복제 수단을 안다
