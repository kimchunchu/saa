# 챕터 3. 네트워킹 (VPC, Route 53, CloudFront)

## 3.1 VPC 구성 요소

| 구성 요소 | 설명 |
|---|---|
| VPC | 리전 단위 가상 네트워크 (CIDR 지정) |
| Subnet | AZ 단위. 퍼블릭/프라이빗은 라우팅으로 결정 |
| Route Table | 서브넷의 트래픽 경로 |
| Internet Gateway (IGW) | VPC ↔ 인터넷 (양방향) |
| NAT Gateway | 프라이빗 서브넷 → 인터넷 (아웃바운드 전용). **AZ별 배치**가 HA |
| Security Group | 인스턴스 단위, **Stateful**, Allow만 |
| Network ACL | 서브넷 단위, **Stateless**, Allow/Deny, 규칙 번호 순 |

## 3.2 VPC 연결

| 서비스 | 용도 |
|---|---|
| **VPC Peering** | 1:1 연결. 전이적 라우팅 불가, CIDR 중복 불가 |
| **Transit Gateway** | 다수 VPC/온프레미스를 허브-스포크로 연결 |
| **Gateway Endpoint** | S3, DynamoDB 전용 (무료, 라우팅 테이블 기반) |
| **Interface Endpoint (PrivateLink)** | 그 외 AWS 서비스/타사 서비스 (ENI 기반) |
| **Site-to-Site VPN** | 인터넷 위 IPsec 암호화 터널. 빠르게 구성 |
| **Direct Connect** | 전용선. 일관된 대역폭/저지연. 기본은 암호화 안 됨 |
| **Direct Connect + VPN** | 전용선 위 암호화 필요 시 |

## 3.3 DNS & 트래픽 분산 (Route 53)

| 라우팅 정책 | 용도 |
|---|---|
| Simple | 단일 리소스 |
| Weighted | 가중치 분산, 카나리 배포 |
| Latency | 지연시간이 가장 낮은 리전 |
| Failover | Active-Passive (헬스 체크 필요) |
| Geolocation | 사용자 위치 기반 (규제/현지화) |
| Geoproximity | 리소스 위치 + 바이어스 |
| Multi-value | 여러 정상 레코드 반환 |

- **Alias 레코드**: 루트 도메인(Zone Apex)에 ELB/CloudFront 연결 가능, 무료

## 3.4 엣지 서비스

| 서비스 | 설명 |
|---|---|
| **CloudFront** | CDN. 정적/동적 콘텐츠 캐싱, OAC로 S3 비공개 접근, 서명 URL/쿠키 |
| **Global Accelerator** | 애니캐스트 고정 IP, AWS 백본 사용. TCP/UDP, 빠른 장애 조치 |

> CloudFront = **캐싱**(HTTP 콘텐츠), Global Accelerator = **네트워크 경로 최적화/고정 IP**

## 3.5 빈출 패턴

| 시나리오 | 답 |
|---|---|
| 프라이빗 서브넷 인스턴스가 인터넷으로 패치 다운로드 | NAT Gateway |
| 인터넷 없이 S3 접근 | Gateway Endpoint |
| 수십 개 VPC + 온프레미스 연결 | Transit Gateway |
| 특정 IP 차단 | NACL (Deny) |
| 전 세계 사용자 정적 콘텐츠 가속 | CloudFront |
| 고정 IP 필요 + 글로벌 TCP/UDP 가속 | Global Accelerator |
| 온프레미스 빠른 암호화 연결 | Site-to-Site VPN |
| 안정적 대용량 하이브리드 연결 | Direct Connect |

## 3.6 체크리스트

- [ ] 퍼블릭/프라이빗 서브넷 3-tier 구조를 그릴 수 있다
- [ ] SG와 NACL 차이를 설명할 수 있다
- [ ] Gateway/Interface Endpoint를 구분한다
