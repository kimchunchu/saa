# 챕터 9. 디커플링 & 데이터 분석

## 9.1 메시징/통합

| 서비스 | 모델 | 핵심 |
|---|---|---|
| **SQS Standard** | 큐 (Pull) | 무제한 처리량, 최소 1회 전달, 순서 보장 안 됨 |
| **SQS FIFO** | 큐 | 순서 보장 + 중복 제거, 처리량 제한 |
| **SNS** | Pub/Sub (Push) | 다수 구독자에게 팬아웃 |
| **EventBridge** | 이벤트 버스 | 규칙 기반 라우팅, SaaS/AWS 이벤트, 스케줄 |
| **Kinesis Data Streams** | 스트림 | 실시간 대용량, 순서(샤드 단위), 재처리 가능 |
| **Kinesis Data Firehose** | 전송 | S3/Redshift/OpenSearch로 거의 실시간 적재 (관리형) |
| **Amazon MQ** | 브로커 | ActiveMQ/RabbitMQ 호환 (기존 앱 이전용) |
| **Step Functions** | 워크플로 | 상태 머신, 서버리스 오케스트레이션 |

### SQS 핵심 설정
- **Visibility Timeout**: 처리 중 메시지가 다른 소비자에게 보이지 않는 시간
- **Dead-Letter Queue (DLQ)**: 반복 실패 메시지 격리
- **Long Polling**: 빈 응답 감소, 비용 절감
- ASG와 연동: 큐 길이 기반 스케일링

### SNS + SQS 팬아웃
하나의 이벤트를 여러 큐/람다로 동시에 전달 (예: 주문 → 결제/배송/알림).

## 9.2 데이터 분석

| 서비스 | 용도 |
|---|---|
| **Athena** | S3 데이터를 SQL로 직접 쿼리 (서버리스) |
| **Glue** | 서버리스 ETL, 데이터 카탈로그, 크롤러 |
| **EMR** | Hadoop/Spark 빅데이터 처리 |
| **Redshift** | 데이터 웨어하우스 |
| **OpenSearch** | 검색, 로그 분석 |
| **QuickSight** | BI 시각화 |
| **Lake Formation** | 데이터 레이크 구축/권한 관리 |
| **MSK** | 관리형 Kafka |

> Athena 비용 절감: **Parquet/ORC 컬럼 포맷 + 압축 + 파티셔닝**

## 9.3 빈출 패턴

| 시나리오 | 답 |
|---|---|
| 트래픽 급증 시 백엔드 보호 | SQS 버퍼링 |
| 하나의 메시지를 여러 시스템에 전달 | SNS (+ SQS) |
| 순서 보장 필수 | SQS FIFO / Kinesis(샤드 내) |
| 실시간 스트림 → S3 적재(코딩 최소) | Firehose |
| S3 로그를 간단히 SQL 분석 | Athena |
| 온프레미스 JMS/AMQP 앱 이전 | Amazon MQ |
| 복잡한 멀티 스텝 워크플로 | Step Functions |

## 9.4 체크리스트

- [ ] SQS/SNS/EventBridge/Kinesis 차이를 설명한다
- [ ] SQS 가시성 제한 시간과 DLQ를 이해한다
