# 챕터 10. 비용 최적화

## 10.1 컴퓨팅 비용

| 방법 | 설명 |
|---|---|
| Right-sizing | Compute Optimizer 권장으로 인스턴스 크기 조정 |
| Savings Plans / RI | 안정적 사용량에 약정 할인 |
| Spot | 중단 허용 워크로드 |
| Auto Scaling | 필요한 만큼만 사용 |
| Graviton (Arm) | 가격 대비 성능 우수 |
| Lambda / Fargate | 유휴 비용 제거 |

## 10.2 스토리지 비용

- S3 **수명 주기 정책** + **Intelligent-Tiering**
- 불필요한 EBS 스냅샷/미사용 볼륨 정리
- EFS-IA, Glacier 계열 활용
- **Requester Pays** 버킷 (요청자가 전송 비용 부담)

## 10.3 네트워크 비용

- 동일 AZ 내 트래픽은 저렴/무료, **AZ 간·리전 간**은 과금
- **Gateway Endpoint**(S3/DynamoDB)는 무료 → NAT Gateway 처리 비용 절감
- 인바운드 전송은 일반적으로 무료, **아웃바운드 인터넷 전송**은 과금
- CloudFront 활용으로 오리진 전송량 감소

## 10.4 데이터베이스 비용

- RDS Reserved Instances
- Aurora Serverless (가변 워크로드)
- DynamoDB On-Demand vs Provisioned 선택
- ElastiCache로 DB 읽기 부하 감소

## 10.5 비용 관리 도구

| 서비스 | 기능 |
|---|---|
| **Cost Explorer** | 비용 분석/예측 |
| **Budgets** | 예산 초과 알림/자동 조치 |
| **Cost and Usage Report (CUR)** | 상세 사용 내역 |
| **Cost Allocation Tags** | 태그별 비용 추적 |
| **Trusted Advisor** | 비용/보안/성능 권고 |
| **Organizations** | 통합 청구, 볼륨 할인 공유 |

## 10.6 빈출 패턴

| 시나리오 | 답 |
|---|---|
| 상시 가동 + 1년 이상 사용 | Savings Plans / Reserved |
| 내결함성 배치 | Spot |
| 접근 빈도 불확실한 S3 | Intelligent-Tiering |
| 거의 접근 안 하는 장기 보관 | Glacier Deep Archive |
| 예산 초과 알림 | AWS Budgets |
| 프로젝트별 비용 분리 | 태그 + Cost Allocation |
| 사용하지 않는 시간대 EC2 중지 | Scheduled Scaling / Instance Scheduler |

## 10.7 체크리스트

- [ ] 요구사항에 맞는 구매 옵션과 스토리지 클래스를 고른다
- [ ] 데이터 전송 비용 구조를 이해한다
