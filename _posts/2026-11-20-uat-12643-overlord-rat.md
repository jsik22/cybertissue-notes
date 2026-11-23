---
layout: post
title: "UAT-12643 — 북한 연계 의심 그룹, Solana 블록체인에 C2를 숨기는 Overlord RAT와 정보 탈취"
date: 2026-11-20
tags: [uat-12643, overlord-rat, infostealer, north-korea, blockchain-c2, lolbin]
description: "Cisco Talos가 공개한 UAT-12643은 Overlord RAT, Good Game 스틸러, 암호화폐 클립보드 탈취기를 쓴다. RAT는 C2 주소를 Solana 블록체인 거래 메모에 숨긴다. 공개 자료와 점검 항목을 정리했다."
sources:
  - {name: "Cisco Talos, UAT-12643: Suspected North Korea-aligned actor blends blockchain resilience with targeted infostealing (2026-11-18)", url: "https://blog.talosintelligence.com/uat-12643-suspected-north-korea-aligned-actor-blends-blockchain-resilience-with-targeted-infostealing/"}
  - {name: "Cisco Talos IOCs, UAT-12643 (GitHub)", url: "https://github.com/Cisco-Talos/IOCs/blob/main/2026/11/uat-12643-suspected-north-korea-aligned-actor-blends-blockchain-resilience-with-targeted-infostealing.txt"}
  - {name: "AlienVault OTX, UAT-12643 펄스 (2026-11-20)", url: "https://otx.alienvault.com/pulse/6b0041b6f254e5321e03a189"}
excerpt: "Cisco Talos가 공개한 UAT-12643은 Overlord RAT, Good Game 스틸러, 암호화폐 클립보드 탈취기를 쓴다. RAT는 C2 주소를 Solana 블록체인 거래 메모에 숨긴다. 공개 자료와 점검 항목을 정리했다."
updated: 2026-11-20 23:25:40 +0900
---

> 최근 업데이트(11월 20일 23시): 첫 게시. 지난 기록은 글 끝 [업데이트 기록](#업데이트-기록)에 있다.

**목차**
* 목차
{:toc}

## 현재 상태

| 항목 | 상태 (11월 20일 23시 기준) |
|---|---|
| 행위자 | UAT-12643. 금전 목적. Talos는 북한 연계를 '낮은 신뢰도'로 평가 |
| 도구 | Overlord RAT(Go), Good Game 스틸러(파이썬, 공개 도구), 암호화폐 클립보드 탈취기(C++) |
| 표적 | 암호화폐 보유자, 게임 플랫폼 이용자, VoIP 소프트폰 운영자, 재무·회계 담당자. 유럽 추정 |
| 국내 피해 | 공개 보고 없음 |
| 탐지 | Talos ClamAV 시그니처 13종, Snort 룰(Snort 2 1:67490·67491, Snort 3 7:74·75) |
| 공개 IOC | Talos GitHub에 파일 해시와 배포 서버 IP 1개 |

## 무슨 일인가

Cisco Talos는 11월 18일 금전 목적의 위협 그룹 UAT-12643을 공개했다. 이 그룹은 원격 조종 악성코드 Overlord RAT, 정보 탈취 악성코드 Good Game, 암호화폐 지갑 주소를 바꿔치기하는 클립보드 탈취기를 함께 쓴다. RAT는 명령 서버(C2) 주소를 암호화해 Solana 블록체인 거래 메모에 올려 두고 읽어 간다. 서버 주소를 차단해도 블록체인에 새 주소를 올리면 감염 기기가 따라간다. Talos는 운영자의 배포 서버 루트 폴더가 인터넷에 열려 있던 덕분에 도구 묶음을 확인했다.

## 공개된 근거

### 초기 침투

| 방식 | 내용 |
|---|---|
| LNK 바로가기 | PDF 아이콘으로 위장. Defender 명령줄 도구 `MpCmdRun.exe -DownloadFile` 또는 `certutil -urlcache`로 내려받아 `RuntimeBroker.exe` 이름으로 실행 |
| HTA 파일 | `mshta.exe`로 실행, `MSXML2.ServerXMLHTTP`로 받아 `%TEMP%`에 쓰고 숨겨 실행 |
| MSBuild XML | `msbuild.exe`가 인라인 C# 작업을 메모리에서 컴파일·실행. 다운로드 요청은 Windows BITS User-Agent로 위장 |
| 유입 경로 | 이 파일들이 피해자에게 어떻게 전달됐는지는 확인되지 않음 |

- **미끼**<br>재무제표 PDF, 메신저 Viber 설치 파일 위장, VoIP 소프트폰 MicroSIP 설정 파일 이름을 썼다. 재무 담당자, 메신저 이용자, 콜센터·업무 전화 환경을 노린 것으로 Talos는 봤다.

### Overlord RAT

- **Solana 블록체인 C2**<br>운영자 지갑의 거래 기록을 Solana JSON-RPC(`getSignatures`·`getTransaction`)로 받아, 메모 데이터가 정확히 43자인 항목만 골라 복호화해 WebSocket C2 주소로 쓴다. 같은 기법은 2026년 3월 Lazarus 그룹 사례에서 먼저 보고됐다.

- **종료 방지**<br>관리자 권한과 SeDebugPrivilege를 얻은 뒤 자신을 시스템 핵심 프로세스로 지정한다. 보안 도구가 강제로 끝내면 `CRITICAL_PROCESS_DIED` 블루스크린이 난다.

- **초기화 뒤에도 살아남는 지속성**<br>사용자 레지스트리 자동 실행에 더해, Windows 복구 환경(WinRE)의 OEM 스크립트 경로(`Recovery\OEM`)와 `ResetConfig.xml`을 고쳐 '이 PC 초기화' 때 RAT가 먼저 실행되게 한다.

- **기능**<br>명령 35종. 키 입력·화면·웹캠·마이크 수집, 파일 조작, 명령 실행, 역방향 SOCKS 프록시(내부망 접근), 메모리 상 DLL 플러그인 적재.

### Good Game 스틸러·클립보드 탈취기

| 항목 | 내용 |
|---|---|
| Good Game 출처 | 공개된 파이썬 스틸러를 PyInstaller로 묶은 것. 이 그룹이 만든 것은 아님 |
| 수집 대상 | 브라우저 23종 비밀번호·쿠키, Discord 토큰, Roblox 세션, 암호화폐 지갑 21종, 게임 런처 4종, 화면·웹캠 한 장 |
| 파일 수집 | 바탕화면·문서 등에서 확장자 45종 중 영·불·서·포어 키워드(시드 문구, 비밀번호, 백업 코드 등)가 이름에 든 파일 |
| 반출 | 압축 파일을 gofile.io에 올리고 다운로드 링크를 Discord 웹후크로 전송 |
| 회피 | 가상머신·샌드박스 검사 5단계, 작업 관리자 일시 비활성화(`DisableTaskMgr`) |
| 클립보드 탈취기 | 0.5초마다 클립보드를 보고 지갑 주소 형식이면 운영자 주소로 바꿈. `%APPDATA%\MsgEdgeUpdate.exe`로 지속 |

### 북한 연계 판단

| 근거 | 내용 |
|---|---|
| Overlord RAT | 북한 연계로 추정되는 UNK_DeadDrop 클러스터가 2026년 6월 사용 |
| Solana 메모 C2 | 2026년 3월 Lazarus 사례와 같은 기법(구현 언어는 다름) |
| 신뢰도 | Talos 평가 '낮음'. 두 요소 모두 다른 행위자도 쓴다고 Talos가 함께 적음 |

## CyberTissue로 본 것

- **관찰**<br>Talos가 공개한 배포 서버 IP는 이번 보고 전인 2026년 6월 봇넷 목록, 9~10월 OverlordRAT IOC 묶음에 이미 올라 있었다. 같은 서버가 몇 달 동안 쓰였다는 뜻일 수 있다(추론). 호스팅 업체 서버라 다른 이용자가 있었을 가능성도 있다.

- **관찰**<br>공개된 파일 해시는 공개 악성코드 저장소에서 찾지 못했고, 공개 Telegram 채널에서 그룹명이나 도구명을 언급한 글도 찾지 못했다. 이 범위에서 찾지 못했다는 뜻이다.

- **관찰**<br>Talos가 밝힌 Snort 룰 번호는 무료 Snort Community 룰 묶음에는 없다. 구독 룰셋이나 Cisco 장비를 쓰지 않는 조직은 직접 탐지 조건을 만들어야 한다.

- **추론**<br>C2 주소를 블록체인에 두면 IP·도메인 차단이 오래 버티지 못한다. 대신 업무 PC가 Solana RPC 노드에 JSON-RPC를 보내는 것 자체가 드물어서 그 통신을 보는 편이 더 오래 쓰는 탐지 조건이 된다. Defender 명령줄 도구·certutil·mshta·msbuild를 이용한 다운로드도 업무 PC에서는 드물다.

- **추론**<br>국내 피해 보고는 없고 표적 언어도 유럽 쪽이다. 다만 미끼가 재무 문서·메신저·VoIP 설정 파일이라 업종을 가리지 않는다. 사용하는 도구도 공개 스틸러와 이미 알려진 RAT라 다른 행위자가 같은 묶음을 쓸 수 있다.

## 조직이 점검할 것

1. **블록체인 RPC 통신**<br>프록시·DNS 기록에서 업무 PC가 Solana RPC 엔드포인트(예: `api.mainnet-beta.solana.com`)로 보낸 요청을 찾는다. 업무상 필요가 없으면 차단을 검토한다.

2. **LOLBin 다운로드**<br>`MpCmdRun.exe -DownloadFile`, `certutil -urlcache`, `mshta.exe`의 외부 요청, `msbuild.exe`가 사용자 폴더의 XML을 실행한 기록을 EDR·프로세스 로그에서 찾는다. 사용자 폴더의 `RuntimeBroker.exe`처럼 시스템 파일 이름을 쓴 실행 파일도 본다.

3. **WinRE 변조**<br>`Recovery\OEM` 폴더의 낯선 배치 파일과 `ResetConfig.xml` 변경 여부를 확인한다. 감염이 의심되는 PC는 'PC 초기화' 대신 이미지로 다시 설치한다.

4. **반출 경로**<br>업무망에서 gofile.io 업로드와 Discord 웹후크 호출을 찾고, 필요 없으면 차단한다.

5. **IOC 대조**<br>Talos GitHub의 해시와 배포 서버 IP를 EDR·방화벽 기록과 맞춰 본다.

## IOC·TTP

- IOC는 Talos GitHub 목록을 그대로 쓴다(파일 해시, 배포 서버 IP 1개). 배포 서버의 서비스 포트는 8000(파일 배포), 5173(RAT WebSocket C2), 5656·5959(스틸러 TCP C2)였다.
- T1204.002 사용자 실행(악성 파일), T1218.005 Mshta, T1127.001 MSBuild, T1105 도구 전송(MpCmdRun·certutil)
- T1102.001 데드 드롭 리졸버(Solana 메모에 C2 주소), T1547.001 레지스트리 자동 실행, T1090 프록시(역방향 SOCKS)
- T1555.003 브라우저 자격 증명, T1115 클립보드 데이터, T1567 웹 서비스로 반출(gofile.io·Discord 웹후크)

## 업데이트 기록

| 시각(KST) | 내용 |
|---|---|
| 11월 20일 23시 | 첫 게시 |

## 출처

- Cisco Talos, UAT-12643: Suspected North Korea-aligned actor blends blockchain resilience with targeted infostealing (2026-11-18)
- Cisco Talos IOCs, UAT-12643 (GitHub)
- AlienVault OTX, UAT-12643 펄스 (2026-11-20)

---
이 글은 CyberTissue가 공개 정보만으로 자동 조사한 결과를 사람이 검토해 올린 것입니다. 추적기·게시글의 내용은 주장이며 확정된 사실이 아닙니다. 조사 방법과 도구: https://github.com/jsik22/cybertissue
