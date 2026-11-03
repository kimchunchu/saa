# 챕터 5. 스토리지 (S3, EBS, EFS, FSx)

## 5.1 스토리지 유형 비교

| 유형 | 서비스 | 특징 |
|---|---|---|
| 객체 | S3 | 무제한 용량, 11 nines 내구성 |
| 블록 | EBS, Instance Store | 인스턴스 부착 디스크 |
| 파일 | EFS, FSx | 공유 파일 시스템 |

## 5.2 Amazon S3

### 스토리지 클래스
| 클래스 | 용도 |
|---|---|
| Standard | 자주 접근 |
| Intelligent-Tiering | 접근 패턴 불명확, 자동 계층화 |
| Standard-IA | 가끔 접근, 다중 AZ |
| One Zone-IA | 가끔 접근, 재생성 가능 데이터, 단일 AZ |
| Glacier Instant Retrieval | 아카이브, 밀리초 조회 |
| Glacier Flexible Retrieval | 아카이브, 분~시간 단위 조회 |
| Glacier Deep Archive | 최저가, 12시간 이상 조회 |

### 핵심 기능
- **수명 주기 정책(Lifecycle)**: 클래스 전환/만료 자동화
- **버저닝**, **MFA Delete**
- **복제**: CRR(교차 리전), SRR(동일 리전) — 버저닝 필요
- **암호화**: SSE-S3, SSE-KMS, SSE-C, 클라이언트 측
- **접근 제어**: 버킷 정책, ACL, Block Public Access, **Access Points**
- **Object Lock**: WORM (Governance / Compliance 모드)
- **S3 Transfer Acceleration**, **Multipart Upload**(100MB 이상 권장)
- **Pre-signed URL**: 임시 접근
- **S3 Event Notification** → SNS/SQS/Lambda/EventBridge

## 5.3 EBS

| 볼륨 | 특징 |
|---|---|
| gp3 / gp2 | 범용 SSD (gp3는 IOPS/처리량 독립 조절) |
| io2 / io2 Block Express | 고성능, 높은 IOPS, **Multi-Attach** 가능 |
| st1 | 처리량 최적화 HDD (빅데이터, 로그) |
| sc1 | 콜드 HDD (최저가) |

- AZ에 종속, **스냅샷**(S3에 저장)으로 AZ/리전 간 이동
- 암호화 시 스냅샷/볼륨 모두 암호화
- **Instance Store**: 초고성능 임시 스토리지, 중지/종료 시 데이터 소실

## 5.4 파일 시스템

| 서비스 | 용도 |
|---|---|
| **EFS** | Linux용 NFS, 다중 AZ 공유, 자동 확장. Standard/IA 클래스 |
| **FSx for Windows** | SMB, Active Directory 통합 |
| **FSx for Lustre** | HPC, ML (S3 연동) |
| **FSx for NetApp ONTAP / OpenZFS** | 멀티 프로토콜, 마이그레이션 |

## 5.5 하이브리드 & 데이터 이전

| 서비스 | 용도 |
|---|---|
| **Storage Gateway** | 온프레미스 ↔ AWS (File, Volume, Tape) |
| **DataSync** | 온라인 데이터 전송 자동화 (NFS/SMB → S3/EFS) |
| **Snow Family** | 대용량 오프라인 이전 (Snowcone, Snowball Edge) |
| **Transfer Family** | SFTP/FTPS로 S3/EFS 접근 |
| **AWS Backup** | 중앙 집중 백업 정책 |

## 5.6 빈출 패턴

| 시나리오 | 답 |
|---|---|
| 접근 패턴 알 수 없음 + 비용 절감 | S3 Intelligent-Tiering |
| 규정상 삭제/수정 불가 보관 | S3 Object Lock (Compliance) |
| 여러 Linux EC2가 공유 파일 시스템 | EFS |
| Windows 파일 서버 + AD | FSx for Windows |
| 데이터베이스용 고 IOPS | io2 |
| 페타바이트급 오프라인 이전 | Snowball Edge |
| 온프레미스 NFS를 S3로 지속 동기화 | DataSync / File Gateway |

## 5.7 체크리스트

- [ ] S3 클래스와 최소 보관 기간/조회 시간을 구분한다
- [ ] EBS 볼륨 유형을 용도별로 고를 수 있다
- [ ] EFS vs FSx를 구분한다
