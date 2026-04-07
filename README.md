# ACSLab project
Monitor illegal filming blocking program using Bluetooth and ARP protocols
# 🛡️ BlueShield Monitor — BLE & ARP 기반 모니터 불법 촬영 차단 시스템

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Raspberry%20Pi%205-c51a4a?style=for-the-badge&logo=raspberrypi&logoColor=white"/>
  <img src="https://img.shields.io/badge/Protocol-Bluetooth%20LE-0082FC?style=for-the-badge&logo=bluetooth&logoColor=white"/>
  <img src="https://img.shields.io/badge/Protocol-ARP-00B388?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Language-Python-3776AB?style=for-the-badge&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge"/>
</p>

> **사무실·회의실·기밀 공간에서 비인가 디바이스를 실시간 탐지하고,  
> 화면을 즉시 차단하여 모니터 불법 촬영을 원천 차단하는 보안 시스템**

---

## 📌 프로젝트 개요

모니터를 향한 불법 촬영은 기업 기밀 유출, 개인정보 침해의 심각한 경로입니다.  
**BlueShield Monitor**는 **Bluetooth LE 스캔**과 **ARP 스캔**을 이중으로 활용하여  
주변의 비인가 디바이스를 탐지하고, 위협이 감지되는 순간 **모니터 화면을 즉시 암전**시킵니다.  
동시에 **관리자 서버에 실시간 알림**을 전송하여 보안 담당자가 즉각 대응할 수 있도록 합니다.

---

## 🏗️ 시스템 아키텍처

```
<img width="300" height="200" alt="Image" src="https://github.com/user-attachments/assets/96b687c6-d20d-4428-ae4b-b9fe0d805642" />
```

---

## ✨ 주요 기능

| 기능 | 설명 |
|------|------|
| 🔵 **BLE 스캔** | 주변 블루투스 디바이스를 실시간 탐지하여 비인가 기기 식별 |
| 🌐 **ARP 스캔** | 동일 네트워크 내 연결된 디바이스 MAC 주소 기반 탐지 |
| 🚫 **화면 즉시 차단** | 위협 감지 시 모니터 화면을 즉각 블랙아웃 처리 |
| 🔔 **관리자 알림** | 비인가 디바이스 탐지 시 관리자 서버에 실시간 알림 전송 |
| ✅ **화이트리스트 관리** | 인가된 디바이스 목록을 등록·관리하여 오탐 최소화 |
| 📋 **로그 기록** | 탐지 이벤트, 차단 이력을 타임스탬프와 함께 저장 |

---

## 🔍 탐지 방식 상세

### 1. Bluetooth LE (BLE) 스캔
- `bleak` 또는 `bluetoothctl` 기반으로 주변 BLE 디바이스 스캔
- 탐지된 디바이스의 **MAC 주소 / 디바이스명**을 화이트리스트와 비교
- 미등록 디바이스 감지 시 → 위협 판정

### 2. ARP 스캔
- `scapy` 또는 `arp-scan` 기반으로 로컬 네트워크 스캔
- 연결된 모든 디바이스의 **MAC 주소**를 수집
- 화이트리스트에 없는 MAC 감지 시 → 위협 판정

### 3. 이중 판정 로직
```
BLE 비인가 탐지  OR  ARP 비인가 탐지
          │
          ▼
     위협 레벨 산정
          │
    ┌─────┴──────┐
    ▼            ▼
 화면 차단    관리자 알림
```

---

## 🛠️ 기술 스택

| 항목 | 사용 기술 |
|------|-----------|
| **엣지 서버** | Raspberry Pi 5 |
| **OS** | Raspberry Pi OS (64-bit) |
| **언어** | Python 3.11+ |
| **BLE 스캔** | `bleak` / `bluetoothctl` |
| **ARP 스캔** | `scapy` / `arp-scan` |
| **화면 차단** | `vcgencmd` / HDMI CEC / xrandr |
| **알림 전송** | HTTP REST API / MQTT / Email (SMTP) |
| **화이트리스트 DB** | SQLite / JSON |

---

## 📁 프로젝트 구조

```
BlueShield-Monitor/
│
├── scanner/
│   ├── ble_scanner.py        # BLE 디바이스 스캔 모듈
│   └── arp_scanner.py        # ARP 네트워크 스캔 모듈
│
├── core/
│   ├── threat_engine.py      # 위협 판정 엔진 (화이트리스트 비교)
│   ├── screen_blocker.py     # 화면 차단 제어
│   └── notifier.py           # 관리자 서버 알림 전송
│
├── db/
│   ├── whitelist.json        # 인가 디바이스 화이트리스트
│   └── event_log.db          # 탐지 이벤트 로그 (SQLite)
│
├── admin/
│   └── dashboard.py          # 관리자 모니터링 대시보드 (선택)
│
├── config/
│   └── settings.yaml         # 스캔 주기, 알림 설정 등
│
├── main.py                   # 진입점 — 전체 시스템 실행
├── requirements.txt
└── README.md
```

---

## ⚙️ 설치 및 실행

### 사전 요구사항
- Raspberry Pi 5 (Raspberry Pi OS 64-bit)
- Python 3.11 이상
- 블루투스 모듈 활성화
- 네트워크 인터페이스 (Wi-Fi / Ethernet)

### 1. 저장소 클론
```bash
git clone https://github.com/your-username/blueshield-monitor.git
cd blueshield-monitor
```

### 2. 의존성 설치
```bash
pip install -r requirements.txt
sudo apt-get install arp-scan bluetooth bluez -y
```

### 3. 화이트리스트 설정
`db/whitelist.json`에 인가된 디바이스를 등록합니다:
```json
{
  "ble_whitelist": [
    {"mac": "AA:BB:CC:DD:EE:FF", "name": "관리자 노트북", "owner": "홍길동"}
  ],
  "arp_whitelist": [
    {"mac": "11:22:33:44:55:66", "name": "업무용 PC", "ip": "192.168.1.10"}
  ]
}
```

### 4. 설정 파일 수정
`config/settings.yaml`:
```yaml
scan:
  ble_interval: 10        # BLE 스캔 주기 (초)
  arp_interval: 15        # ARP 스캔 주기 (초)

alert:
  admin_server_url: "http://your-admin-server/api/alert"
  method: "POST"          # HTTP / MQTT / EMAIL

screen:
  block_method: "hdmi"    # hdmi / xrandr / dpms
```

### 5. 실행
```bash
# 일반 실행
sudo python3 main.py

# 백그라운드 서비스로 등록 (systemd)
sudo systemctl enable blueshield.service
sudo systemctl start blueshield.service
```

---

## 🔔 알림 예시

비인가 디바이스 탐지 시 관리자 서버로 전송되는 JSON 페이로드:

```json
{
  "event": "UNAUTHORIZED_DEVICE_DETECTED",
  "timestamp": "2025-04-02T13:45:22+09:00",
  "device": {
    "mac": "DE:AD:BE:EF:CA:FE",
    "name": "Unknown Device",
    "detection_method": "BLE",
    "rssi": -62
  },
  "action_taken": "SCREEN_BLOCKED",
  "location": "3층 회의실 A"
}
```

---

## 🔒 보안 고려사항

- **MAC 스푸핑 대응** : MAC 주소 단독 의존을 줄이고 BLE + ARP 이중 탐지로 신뢰도 향상
- **오탐 방지** : 스캔 주기 조정 및 연속 N회 탐지 시에만 차단 동작 (Debounce 로직)
- **화이트리스트 암호화** : 민감한 디바이스 정보는 암호화 저장 권장
- **알림 채널 보안** : HTTPS / MQTT TLS 사용 권장

---

## 📊 동작 흐름

```
시작
 │
 ├─▶ BLE 스캔 (주기적)
 │       │
 │       └─▶ 화이트리스트 비교 ──▶ 비인가 탐지?
 │                                      │ YES
 ├─▶ ARP 스캔 (주기적)                  ▼
 │       │                      ┌──────────────┐
 │       └─▶ 화이트리스트 비교  │  화면 차단    │
 │               │ 비인가 탐지? │  + 알림 전송  │
 │               └─────────────▶│  + 로그 기록  │
 │                              └──────────────┘
 │
 └─▶ 인가 디바이스만 존재 → 화면 정상 유지
```

---

## 🚀 향후 개선 계획

- [ ] 웹 기반 관리자 대시보드 (React)
- [ ] 카메라 모듈 연동 — 비인가자 얼굴 캡처 및 저장
- [ ] Wi-Fi Probe Request 스캔 추가
- [ ] 다중 라즈베리파이 클러스터 지원 (대형 공간 커버리지)
- [ ] AI 기반 디바이스 행동 패턴 분석 (이상 탐지)
- [ ] 모바일 앱 알림 (푸시 알림)

---

## 🤝 기여 방법

1. 이 저장소를 Fork 합니다
2. 새 브랜치를 생성합니다 (`git checkout -b feature/새기능`)
3. 변경사항을 커밋합니다 (`git commit -m 'Add: 새기능 추가'`)
4. 브랜치에 Push 합니다 (`git push origin feature/새기능`)
5. Pull Request를 생성합니다

---

## 📄 라이선스

This project is licensed under the MIT License.  
자세한 내용은 [LICENSE](./LICENSE) 파일을 참고하세요.

---

## ⚠️ 법적 고지

> 본 시스템은 **합법적인 보안 목적**으로만 사용해야 합니다.  
> 타인의 디바이스를 무단으로 스캔하거나 차단하는 행위는 관련 법령에 위반될 수 있습니다.  
> 반드시 **해당 공간의 운영자 또는 관리자 권한** 하에 운영하십시오.

---

<p align="center">
  Made with ❤️ on Raspberry Pi 5
</p>
