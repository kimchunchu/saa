# 챕터 4. 컴퓨팅 (EC2, ELB, ASG, Lambda, 컨테이너)

## 4.1 EC2

### 인스턴스 패밀리
| 계열 | 용도 |
|---|---|
| M, T | 범용 (T는 버스터블) |
| C | 컴퓨팅 최적화 |
| R, X | 메모리 최적화 (인메모리 DB) |
| I, D | 스토리지 최적화 (고 IOPS, 순차 처리량) |
| P, G | GPU (ML, 그래픽) |

### 구매 옵션
| 옵션 | 특징 | 적합한 경우 |
|---|---|---|
| On-Demand | 유연, 가장 비쌈 | 단기/예측 불가 |
| Reserved / Savings Plans | 1·3년 약정, 큰 폭 할인 | 안정적 상시 워크로드 |
| Spot | 대폭 할인, 중단 가능 | 배치, 내결함성 작업 |
| Dedicated Host/Instance | 전용 하드웨어 | BYOL, 규정 준수 |
| Capacity Reservation | 특정 AZ 용량 보장 | 용량 확보 |

### Placement Group
- **Cluster**: 저지연/고처리량 (단일 AZ)
- **Spread**: 장애 격리 (최대 AZ당 7개)
- **Partition**: 대규모 분산 시스템 (Hadoop, Cassandra 등)

## 4.2 Elastic Load Balancing

| 종류 | 계층 | 특징 |
|---|---|---|
| **ALB** | L7 | 경로/호스트 기반 라우팅, 컨테이너/Lambda 대상, WebSocket |
| **NLB** | L4 | 초저지연, 고정 IP(EIP), 수백만 RPS, PrivateLink 연동 |
| **GWLB** | L3 | 방화벽/IDS 등 가상 어플라이언스 배치 |
| CLB | L4/L7 | 구세대 (시험에서는 보통 오답) |

- **Sticky Session**, **Cross-Zone LB**, **Connection Draining(Deregistration Delay)**

## 4.3 Auto Scaling

- 구성: Launch Template + ASG (min/desired/max)
- 정책: Target Tracking(가장 간단), Step, Scheduled, Predictive
- 헬스 체크: EC2 / ELB. ELB 헬스 체크 사용 시 비정상 인스턴스 교체
- 다중 AZ에 걸쳐 구성 → 고가용성

## 4.4 서버리스 & 컨테이너

| 서비스 | 설명 |
|---|---|
| **Lambda** | 이벤트 기반, 최대 15분, 자동 확장. 동시성 제한/Provisioned Concurrency |
| **API Gateway** | REST/HTTP/WebSocket API 관리, 스로틀링, 캐싱 |
| **ECS** | AWS 컨테이너 오케스트레이션 |
| **EKS** | 관리형 Kubernetes |
| **Fargate** | 서버 관리 없는 컨테이너 실행 (ECS/EKS 모두) |
| **App Runner / Elastic Beanstalk** | 코드/컨테이너를 간편 배포 |
| **Batch** | 대규모 배치 작업 (Spot 활용) |

## 4.5 빈출 패턴

| 시나리오 | 답 |
|---|---|
| 서버 관리 없이 컨테이너 | Fargate |
| 경로 기반 라우팅(/api, /img) | ALB |
| 고정 IP + 극한 성능 TCP | NLB |
| 중단 가능한 배치 처리 비용 절감 | Spot |
| 예측 가능한 트래픽 증가에 대비 | Scheduled Scaling |
| 이벤트 발생 시 짧은 코드 실행 | Lambda |
| 서드파티 방화벽 어플라이언스 | GWLB |

## 4.6 체크리스트

- [ ] ALB/NLB/GWLB를 구분한다
- [ ] EC2 구매 옵션별 적합 시나리오를 안다
- [ ] Lambda 한계(시간, 메모리, 동시성)를 안다
