# 챕터 6. 데이터베이스

## 6.1 서비스 선택 가이드

| 요구사항 | 서비스 |
|---|---|
| 관계형(SQL), 관리형 | **RDS** (MySQL, PostgreSQL, MariaDB, Oracle, SQL Server) |
| 클라우드 네이티브 고성능 관계형 | **Aurora** |
| 키-값/문서, 밀리초 지연, 서버리스 | **DynamoDB** |
| 인메모리 캐시 | **ElastiCache** (Redis / Memcached) |
| 데이터 웨어하우스(OLAP) | **Redshift** |
| 그래프 | **Neptune** |
| MongoDB 호환 | **DocumentDB** |
| 시계열 | **Timestream** |
| Cassandra 호환 | **Keyspaces** |

## 6.2 RDS & Aurora

### 고가용성 vs 확장성
| 기능 | 목적 | 특징 |
|---|---|---|
| **Multi-AZ** | 고가용성/장애 조치 | 동기 복제, 자동 페일오버, 읽기에는 사용 불가(인스턴스 배포 기준) |
| **Read Replica** | 읽기 확장 | 비동기 복제, 최대 5개(RDS), 교차 리전 가능 |

### Aurora 특징
- 스토리지가 3개 AZ에 걸쳐 **6개 복사본**, 자동 확장
- Reader 최대 15개, **Reader Endpoint** 로드 분산
- **Aurora Serverless v2**: 예측 불가/가변 워크로드
- **Aurora Global Database**: 교차 리전 저지연 복제, 빠른 DR
- **RDS Proxy**: 연결 풀링 (Lambda 등에서 연결 폭주 방지, 페일오버 시간 단축)
- 백업: 자동 백업(PITR) + 수동 스냅샷, 암호화는 생성 시 설정

## 6.3 DynamoDB

- **용량 모드**: Provisioned(Auto Scaling) / On-Demand
- **DAX**: DynamoDB 전용 인메모리 캐시 (마이크로초)
- **Global Tables**: 다중 리전 Active-Active
- **PITR**, **On-demand Backup**, **TTL**, **Streams**(변경 이벤트 → Lambda)
- 인덱스: LSI(생성 시에만), GSI(언제든)
- 파티션 키 설계: 핫 파티션 방지

## 6.4 ElastiCache

| | Redis | Memcached |
|---|---|---|
| 복제/Multi-AZ | 지원 | 미지원 |
| 영속성/백업 | 지원 | 미지원 |
| 데이터 구조 | 다양 | 단순 |
| 멀티스레드 | 제한적 | 지원 |
| 용도 | 세션, 리더보드, Pub/Sub | 단순 캐싱 |

## 6.5 Redshift

- 컬럼 기반 OLAP, 페타바이트 규모 분석
- **Redshift Spectrum**: S3 데이터 직접 쿼리
- Multi-AZ/스냅샷 교차 리전 복사 지원

## 6.6 마이그레이션

- **DMS**: DB 마이그레이션 (지속 복제 CDC 지원)
- **SCT (Schema Conversion Tool)**: 이기종 변환 (예: Oracle → Aurora PostgreSQL)

## 6.7 빈출 패턴

| 시나리오 | 답 |
|---|---|
| DB 장애 시 자동 복구(고가용성) | RDS Multi-AZ |
| 읽기 부하 분산 | Read Replica / Aurora Reader |
| DynamoDB 읽기 지연 최소화 | DAX |
| 글로벌 다중 리전 쓰기 | DynamoDB Global Tables |
| Lambda → RDS 연결 문제 | RDS Proxy |
| 복잡한 분석 쿼리 | Redshift |
| 이기종 DB 마이그레이션 | SCT + DMS |
| 변동이 큰 DB 워크로드 비용 절감 | Aurora Serverless v2 |

## 6.8 체크리스트

- [ ] Multi-AZ와 Read Replica의 목적 차이를 설명한다
- [ ] Aurora의 아키텍처 특징을 안다
- [ ] 캐시 계층 선택(DAX vs ElastiCache)을 한다
