---
layout: post
title: "FrostyGoop — Modbus로 난방을 끊은 ICS 악성코드, 공개 정보로 다시 보기"
date: 2026-11-15
tags: [ics, ot, malware, modbus, frostygoop]
description: "2024년 우크라이나 지역난방 중단에 쓰인 FrostyGoop(BUSTLEBERM)을 공개 보고서와 CyberTissue 조회 결과로 정리하고, OT 운영 조직이 점검할 항목을 뽑았다."
sources:
  - {name: "Dragos, FrostyGoop ICS Malware Intel Brief (2024-07)", url: "https://hub.dragos.com/hubfs/Reports/Dragos-FrostyGoop-ICS-Malware-Intel-Brief-0724_.pdf"}
  - {name: "Unit 42, FrostyGoop's Zoom-In (2024-11)", url: "https://unit42.paloaltonetworks.com/frostygoop-malware-analysis/"}
  - {name: "Forescout Vedere Labs, Insights Others Missed (2024-08)", url: "https://www.forescout.com/blog/ics-malware-frostygoopbustleberm-insights-others-missed/"}
  - {name: "MITRE ATT&CK S1165 FrostyGoop", url: "https://attack.mitre.org/software/S1165/"}
  - {name: "MITRE ATT&CK C0041 FrostyGoop Incident", url: "https://attack.mitre.org/campaigns/C0041/"}
excerpt: "2024년 우크라이나 지역난방 중단에 쓰인 FrostyGoop(BUSTLEBERM)을 공개 보고서와 CyberTissue 조회 결과로 정리하고, OT 운영 조직이 점검할 항목을 뽑았다."
---

> 최근 업데이트(11월 19일): 노트 양식을 정리했다(내용 변화 없음). 지난 기록은 글 끝 [업데이트 기록](#업데이트-기록)에 있다.

**목차**
* 목차
{:toc}

## 현재 상태

| 항목 | 상태 (11월 15일 기준) |
|---|---|
| 정체 | Go로 작성된 Windows용 ICS 악성코드(다른 이름 BUSTLEBERM). Modbus TCP(포트 502)로 제어기 레지스터를 직접 읽고 씀 |
| 피해 | 2024년 1월 우크라이나 지역난방 회사, 600여 동 아파트 약 이틀 난방 중단 |
| 공격 주체 | Dragos는 TAT2024-24로 부르며 특정 국가 조직에 귀속하지 않음 |
| 새 공격 | 2024년 이후 이 악성코드를 쓴 새 공격 보도 찾지 못함 |
| 공개 IOC | 공개 보고서 SHA-256 8건, 이 가운데 3건 MalwareBazaar 등재(아래 IOC·TTP) |
| ATT&CK | 소프트웨어 S1165, 캠페인 C0041 |

## 무슨 일인가

2024년 1월, 우크라이나의 한 지자체 지역난방 회사가 공격을 받아 600여 동의 아파트에 약 이틀 동안 난방이 끊겼다. 영하의 날씨였다. Dragos는 2024년 7월 이 사건에 FrostyGoop(다른 이름 BUSTLEBERM)이 쓰였다고 평가했다. FrostyGoop은 Go로 작성된 Windows용 프로그램으로, 산업 제어 프로토콜인 Modbus TCP(포트 502)로 제어기의 레지스터를 직접 읽고 쓴다. Modbus TCP로 OT에 실제 영향을 낸 첫 ICS 특화 악성코드로 꼽힌다.

## 공개된 근거

- **Dragos(2024-07)**<br>공격자는 외부에 노출된 라우터의 확인되지 않은 취약점으로 몇 달 전 들어왔을 가능성이 있다. 이후 ReGeorg와 비슷한 터널 웹셸을 주로 Tor를 거쳐 썼고, SAM 레지스트리에서 자격 증명을 빼냈다. 2024년 1월에는 모스크바 소재 주소로 L2TP 연결을 열었다. ENCO 제어기의 펌웨어를 모니터링 기능이 없는 버전으로 내리고 잘못된 계측값을 보고하게 만들었다. 라우터·관리 서버·제어기가 충분히 분리되지 않았다는 점도 지적했다.

| 출처 | 핵심 |
|---|---|
| Unit 42(2024-11) | 샘플의 동작(명령줄 인자, JSON 설정 파일, 읽기 3·쓰기 6/16 기능 코드)을 자세히 분석했고, 인터넷에 노출된 Modbus 기기를 대규모로 관측했다. |
| Forescout(2024-08) | 펌웨어 다운그레이드와 라우터 악용은 악성코드 자체 기능이 아니므로, 공개 샘플과 사건의 연결은 생각보다 약할 수 있다고 봤다. 실제 피해 규모가 보도보다 작았을 가능성도 언급했다. |
| MITRE ATT&CK | 소프트웨어 S1165, 캠페인 C0041로 정리돼 있다. Dragos는 공격 주체를 TAT2024-24로 부르며 특정 국가 조직에 귀속하지 않았다. |

## CyberTissue로 본 것

- **관찰**<br>공개 보고서의 SHA-256 8건 가운데 3건이 MalwareBazaar에 FrostyGoop 태그로 등재돼 있다(2024-07-23~24). 8건 모두 OTX 펄스 3건에 들어 있고, ThreatFox에는 없다.

- **관찰**<br>다크웹 onion 검색엔진, 공개 Telegram 채널, 랜섬웨어 유출 사이트 추적기에서 FrostyGoop·BUSTLEBERM 언급을 찾지 못했다. 이 범위에서 찾지 못했다는 뜻이지 유통이 없다는 뜻은 아니다.

- **관찰**<br>GitHub 코드 검색에서는 1,290건이 나왔다. 대부분 탐지 규칙(YARA), OT 공격 모의 실습 환경, 보고서 미러다.

- **추론**<br>2024년 이후 이 악성코드를 쓴 새 공격 보도는 찾지 못했다. 다만 기능이 범용적이라 같은 방식은 Modbus 기기가 인터넷에 노출된 어디에나 적용될 수 있다.

- **불확실**<br>일부 공개 저장소가 이 악성코드를 특정 국가 배후 조직으로 분류하지만, 원 보고서의 귀속과 다르다.

## 조직이 점검할 것

1. **Modbus 502 노출 확인**<br>자사와 설비 유지보수 협력사 자산 가운데 인터넷에 열린 Modbus TCP 502 포트가 있는지 확인한다.

2. **OT 경계 라우터 점검**<br>OT 경계 라우터의 모델·펌웨어·패치 상태와 외부 관리 인터페이스 노출 여부를 확인한다.

3. **터널·Tor 흔적 찾기**<br>L2TP 같은 터널 연결의 해외 목적지와 Tor 출구에서 들어온 웹 요청 흔적을 로그에서 찾는다.

4. **펌웨어 다운그레이드 이력**<br>제어기 펌웨어의 버전 이력에서 기능이 빠진 구버전으로 내려간 기록이 있는지 본다.

5. **비정상 Modbus 쓰기 탐지**<br>엔지니어링 스테이션이 아닌 호스트에서 나간 Modbus 쓰기(기능 코드 6·16)를 탐지 규칙으로 잡는다.

6. **로컬 계정·SAM 덤프 점검**<br>관리 서버의 로컬 계정 재사용과 SAM 덤프 흔적을 점검한다.

## IOC·TTP

공개 보고서(Unit 42)와 MalwareBazaar에 등재된 샘플 해시 일부다.

- `5d2e4fd08f81e3b2eb2f3eaae16eb32ae02e760afc36fa17f4649322f6da53fb` (mb.exe)
- `a63ba88ad869085f1625729708ba65e87f5b37d7be9153b3db1a1b0e3fed309c` (read-all-mb.exe)
- `2fd9cb69ef30c0d00a61851b2d96350a9be68c7f1f25a31f896082cfbf39559a` (modbus.exe)

주요 기법(MITRE C0041·S1165): 공개 애플리케이션 악용(T1190), 웹셸(T1505.003), SAM 덤프(T1003.002), 펌웨어 다운그레이드(T1689), 파라미터 변조(T0836), 모니터링 상실(T0829), 가용성 상실(T0826).

## 업데이트 기록

| 시각(KST) | 내용 |
|---|---|
| 11월 15일 | 첫 게시 |
| 11월 19일 | 노트 양식 정리(현재 상태 표·목차·업데이트 기록 추가, 항목 형식 통일). 내용 변화 없음 |

## 출처

- Dragos, FrostyGoop ICS Malware Intel Brief (2024-07)
- Unit 42, FrostyGoop's Zoom-In (2024-11)
- Forescout Vedere Labs, Insights Others Missed (2024-08)
- MITRE ATT&CK S1165, C0041

---
이 글은 CyberTissue가 공개 정보만으로 자동 조사한 결과를 사람이 검토해 올린 것입니다. 추적기·게시글의 내용은 주장이며 확정된 사실이 아닙니다. 조사 방법과 도구: https://github.com/jsik22/cybertissue
