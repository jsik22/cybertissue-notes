---
layout: post
title: "내 PC에서 무엇을 가져가나"
date: 2026-11-28
tags: [infostealer, session-hijacking, cookies, credentials, npki]
description: "인포스틸러는 브라우저의 비밀번호·쿠키·카드 파일과 지갑, 메신저 세션, 문서를 몇십 초 만에 한 묶음으로 가져간다. 쿠키는 2단계 인증을 건너뛰게 하고, 국내를 노린 공격은 공동인증서까지 가져간다."
sources:
  - {name: "SpyCloud — 2026 Identity Exposure Report 보도자료", url: "https://spycloud.com/newsroom/annual-identity-exposure-report-2026"}
  - {name: "AhnLab — Why Hackers Love Automatic Logins", url: "https://www.ahnlab.com/en/contents/content-center/35309"}
  - {name: "BleepingComputer — Google Chrome adds app-bound encryption to block infostealer malware", url: "https://www.bleepingcomputer.com/news/security/google-chrome-adds-app-bound-encryption-to-block-infostealer-malware/"}
  - {name: "SpyCloud — Infostealers bypass new Chrome security feature", url: "https://spycloud.com/blog/infostealers-bypass-new-chrome-security-feature/"}
  - {name: "Okta — Signing in without actually signing in", url: "https://www.okta.com/blog/threat-intelligence/signing_in_without_actually_signing_in/"}
  - {name: "CircleCI — January 4, 2023 security incident report", url: "https://circleci.com/blog/jan-4-2023-incident-report/"}
  - {name: "Google TAG — Phishing campaign targets YouTube creators with cookie theft malware", url: "https://blog.google/threat-analysis-group/phishing-campaign-targets-youtube-creators-cookie-theft-malware/"}
  - {name: "위키트리 — KISA 공공기관·언론사 유튜브 해킹 경고", url: "https://www.wikitree.co.kr/articles/1176145"}
  - {name: "Recorded Future — 2025 Identity Threat Landscape", url: "https://www.recordedfuture.com/blog/identity-trend-report-march-blog"}
  - {name: "Google — Protecting cookies with Device Bound Session Credentials", url: "https://blog.google/security/protecting-cookies-with-device-bound-session-credentials/"}
  - {name: "Google Workspace Updates — DBSC generally available in Chrome for Windows", url: "https://workspaceupdates.googleblog.com/2026/05/prevent-account-takeovers-with-DBSC-now-generally-available-in-the-Chrome-browser-for-Windows.html"}
  - {name: "Microsoft — Lumma Stealer: Breaking down the delivery techniques and capabilities", url: "https://www.microsoft.com/en-us/security/blog/2025/05/21/lumma-stealer-breaking-down-the-delivery-techniques-and-capabilities-of-a-prolific-infostealer/"}
  - {name: "Sophos — Why AMOS matters", url: "https://www.sophos.com/en-us/blog/why-amos-matters-the-macos-malware-stealing-data-at-scale"}
  - {name: "AhnLab ASEC — Malicious LNK Disguised as Notices", url: "https://asec.ahnlab.com/en/87620/"}
  - {name: "디지털데일리 — 카카오톡 PC 세션 악용 공격", url: "https://www.ddaily.co.kr/page/view/2025111009224254115"}
  - {name: "Flare — Infostealer malware", url: "https://flare.io/learn/resources/blog/infostealer-malware/"}
  - {name: "AhnLab ASEC — May 2026 Infostealer Trend Report", url: "https://asec.ahnlab.com/en/94172"}
  - {name: "BleepingComputer — Inside the Remus infostealer", url: "https://www.bleepingcomputer.com/news/security/inside-the-remus-infostealer-session-theft-maas-and-rapid-evolution/"}
  - {name: "Flashpoint — Midyear 2026 information-stealing malware threats", url: "https://flashpoint.io/resources/report/flashpoint-global-threat-intelligence-report-midyear-2026/information-stealing-malware-threats/"}
  - {name: "Zscaler — I StealC You: Tracking the rapid changes to StealC", url: "https://www.zscaler.com/blogs/security-research/i-stealc-you-tracking-rapid-changes-stealc"}
  - {name: "Malwarebytes — Info stealers can steal cookies for permanent access to your Google account", url: "https://www.malwarebytes.com/blog/news/2024/01/info-stealers-can-steal-cookies-for-permanent-access-to-your-google-account"}
  - {name: "AhnLab ASEC — RecordBreaker 유포와 ClipBanker 설치", url: "https://asec.ahnlab.com/en/52542"}
  - {name: "AhnLab ASEC — February 2026 Infostealer Trend Report", url: "https://asec.ahnlab.com/en/92902/"}
  - {name: "Moonlock — AMOS stealer adds backdoor", url: "https://moonlock.com/amos-stealer-backdoor"}
  - {name: "Microsoft — StealC and Amadey: Breaking down infostealers and the cybercrime services that deliver them", url: "https://www.microsoft.com/en-us/security/blog/2026/06/24/stealc-and-amadey-breaking-down-infostealers-and-the-cybercrime-services-that-deliver-them"}
  - {name: "SecurityWeek — Venom Stealer raises stakes with continuous credential harvesting", url: "https://www.securityweek.com/venom-stealer-raises-stakes-with-continuous-credential-harvesting/"}
kind: series
series: "인포스틸러 생태계"
part: 3
excerpt: "인포스틸러는 브라우저의 비밀번호·쿠키·카드 파일과 지갑, 메신저 세션, 문서를 몇십 초 만에 한 묶음으로 가져간다. 쿠키는 2단계 인증을 건너뛰게 하고, 국내를 노린 공격은 공동인증서까지 가져간다."
updated: 2026-11-28 11:25:58 +0900
---

![PC 본체의 서랍장 칸칸에 열쇠, 쿠키, 카드, 지갑, 말풍선, 문서, 인증서가 들어 있고, 빨간 장갑 손이 빠르게 훑으며 집어 낸다. 집어 낸 물건은 빨간 점선을 따라 가격표가 달린 봉인 상자로 모이고, 구석의 스톱워치 바늘은 거의 움직이지 않았다](https://jsik22.github.io/cybertissue-notes/assets/2026-11-28-infostealer-kr-3/1-hero.png)

## 24초 동안 일어난 일

2편의 포토샵 크랙 배포 페이지를 가상 PC에서 연 기록을 다시 보자. 내려받은 압축을 풀고 설치 파일을 실행하자 '실행 오류' 창이 떴다. 사용자 눈에는 크랙이 실패한 것처럼 보인다.

![ANY.RUN 분석 화면. 왼쪽 가상 PC에서는 압축을 푼 폴더의 Set-up.exe가 선택돼 있고, 그 위에 'Runtime Error — Failed to locate Framework DLL'(프레임워크 DLL을 찾지 못함) 오류 창이 떠 있다. 오른쪽에는 'Malicious activity'(악성 활동) 판정과 함께 이 PC에서 실행된 프로세스 목록이 보인다. ANY.RUN 공개 분석 기록 화면, 크랙 배포 사이트 주소는 가림](https://jsik22.github.io/cybertissue-notes/assets/2026-11-28-infostealer-kr-3/2-anyrun-vm.png)

그사이 뒤에서는 몇 단계를 거쳐 Lumma가 실행됐다. 이 악성 프로세스가 살아 있던 시간은 24초였다. 시작 19초쯤 뒤부터 크롬과 엣지의 비밀번호 파일과 방문 기록 파일 다섯 개를 열었다.

![악성코드 분석 기록의 행위 상세 창 네 장을 2×2로 놓았다. updater.exe가 연 파일로 크롬의 Login Data For Account와 History, 엣지의 Login Data와 History가 나온다. ANY.RUN 공개 분석 기록 화면](https://jsik22.github.io/cybertissue-notes/assets/2026-11-28-infostealer-kr-3/3-anyrun-files.png)

파일을 연 직후 이 프로세스는 5초 동안 외부 서버와 9번 연결해 약 420KB를 내보내고 63KB를 받았다. 보낸 내용은 암호화돼 있어 무엇이 몇 개 나갔는지는 알 수 없다.

![같은 프로세스의 통신 기록. 실행 19~25초 사이 외부 서버와 맺은 연결 9건이 시각순으로 나온다. 연결마다 국가(네덜란드 8건, 미국 1건), 소속망(클라우드플레어), 보낸 양과 받은 양이 적혀 있고, 한 번에 243KB를 보낸 연결도 있다. ANY.RUN 공개 분석 기록 화면, 서버 주소는 가림](https://jsik22.github.io/cybertissue-notes/assets/2026-11-28-infostealer-kr-3/4-anyrun-net.png)

SpyCloud가 2025년 인포스틸러 감염 1,320만 건을 분석해 보니, 계정 정보가 6억 4천만 건, 로그인 쿠키와 세션 정보가 86억 건 나왔다.

![SpyCloud 2026 신원 노출 보고서 보도자료 화면. 위쪽 수치 상자에 '8.6B stolen session cookies enabling MFA bypass'(2단계 인증을 건너뛰게 하는 탈취 세션 쿠키 86억 건), '13.2M infostealer malware logs recovered'(회수한 인포스틸러 로그 1,320만 건)가 있고, 아래 본문에 인포스틸러 감염 1,320만 건에서 계정 정보 6억 4,240만 건이 나왔다는 문장이 있다](https://jsik22.github.io/cybertissue-notes/assets/2026-11-28-infostealer-kr-3/5-spycloud.png)

## 브라우저 안의 파일 세 개

브라우저에 저장한 비밀번호, 로그인 상태를 담은 쿠키, 자동 완성과 카드 정보는 PC의 프로필 폴더에 평범한 파일로 놓인다. 윈도우의 크롬이라면 사용자 폴더 아래 AppData\Local\Google\Chrome\User Data\Default다. 아래는 계정을 하나도 넣지 않은 테스트용 크롬 프로필을 새로 만들어 폴더를 열어 본 화면이다. 빈 프로필에도 이 파일들이 먼저 생긴다.

![새로 만든 크롬 테스트 프로필 폴더의 파일 목록 일부. Cookies(로그인 상태), Login Data(저장된 비밀번호), Web Data(자동 완성·카드) 세 파일이 빨간 상자로 표시돼 있다. 맥에서 만든 빈 프로필이고 실제 계정은 없다](https://jsik22.github.io/cybertissue-notes/assets/2026-11-28-infostealer-kr-3/6-chrome-files.png)

파일 안의 비밀번호와 쿠키는 암호화돼 있다. 윈도우는 그 열쇠를 '지금 로그인한 사용자'에게 풀어 주는 방식(DPAPI)으로 지킨다. 사용자가 실행한 악성코드는 윈도우가 보기에 사용자 본인이라 열쇠를 그대로 받는다. 안랩은 이를 인포스틸러가 브라우저 저장 정보를 노리는 이유로 꼽았다.

구글은 2024년 7월 크롬에 앱 바인딩 암호화를 넣어 크롬이 아닌 프로그램에는 열쇠를 주지 않게 했다. 그러나 SpyCloud는 45일도 안 된 9월 12일에 이를 우회한 흔적을 처음 봤고, 이후 주요 계열들이 잇달아 우회 기능을 내세웠다. 최신 크롬을 쓴다고 저장 정보가 안전하지는 않다.

## 훔친 세션은 2단계 인증을 건너뛴다

로그인하면 사이트는 '이 사람은 확인이 끝났다'는 표시로 세션 ID(토큰)를 발급하고, 브라우저는 이를 쿠키에 담아 두었다가 접속할 때마다 함께 보낸다. 다음에 들어올 때 비밀번호를 다시 묻지 않는 이유다. 세션 ID는 비밀번호와 2단계 인증을 모두 통과한 뒤에 발급되므로, 쿠키째 복사해 다른 PC에서 내밀면 사이트는 검문을 다시 하지 않는다. Okta는 이를 '로그인하지 않고 로그인하는' 방식이라고 설명했다.

![두 줄로 그린 과정 비교. 윗줄의 주인은 열쇠 관문(비밀번호)과 휴대폰 관문(2단계 인증)을 차례로 지나 출입증을 받고 문으로 간다. 그 출입증에서 빨간 점선이 아랫줄로 내려와 공격자의 빨간 복사본이 되고, 공격자는 흐리게 그려진 두 관문을 빨간 화살표로 비켜 가 같은 문을 연다](https://jsik22.github.io/cybertissue-notes/assets/2026-11-28-infostealer-kr-3/7-cookie.png)

해외에서는 원인까지 확인된 사고가 있다. 2023년 1월 개발 플랫폼 CircleCI는 직원 노트북의 악성코드가 2단계 인증이 걸린 사내 로그인 쿠키를 훔쳤다고 밝혔다. 공격자는 그 직원 행세를 하며 운영 시스템 일부에 들어왔고, 악성코드는 백신에 걸리지 않았다. 구글은 2021년 유튜브 크리에이터를 노린 쿠키 탈취 캠페인을 공개하고 계정 약 4천 개를 복구했다.

국내에서도 공공기관과 연예인의 유튜브 채널이 넘어가 가상자산 광고 방송에 쓰이는 일이 이어졌다. 11월 KISA는 정부·공공기관·언론사 공식 채널 해킹을 경고했다. 다만 국내 사건은 계정을 잃은 경위가 공개되지 않아, 쿠키 탈취였는지는 알 수 없다.

- **관찰**<br>Recorded Future 집계로 2025년 악성코드가 훔친 계정 정보의 31%에는 아직 살아 있는 로그인 쿠키가 함께 있었다.

구글은 쿠키를 기기에 묶는 기술(DBSC)로 대응하고 있다. 기기 보안 칩 안의 열쇠가 있어야 쿠키를 새로 받을 수 있어, 훔친 쿠키는 금방 쓸모가 없어진다. 2026년 5월부터 윈도우 크롬의 구글 계정에 기본으로 적용됐고, 맥은 차례로 넓히는 중이다. 다른 사이트는 각자 이 기술을 도입해야 하고, 저장된 비밀번호나 지갑, 파일을 가져가는 것은 막지 못한다.

## 브라우저 밖에서 가져가는 것

- **암호화폐 지갑**<br>브라우저 확장 지갑과 PC용 지갑 프로그램의 파일을 가져간다. 맥용 AMOS는 가짜 하드웨어 지갑 프로그램으로 복구 문구까지 노린다.
- **메신저·게임 세션**<br>텔레그램 로그인 파일, 디스코드 토큰, 스팀 세션처럼 로그인 상태 파일을 가져간다. 쿠키처럼 비밀번호 없이 그 계정에 들어갈 수 있다.
- **업무 계정**<br>VPN·메일·FTP 설정과 개발 도구의 접속 키도 가져간다. SpyCloud는 2025년 로그에서 API 키 같은 계정 정보 1,810만 건을 찾았다.
- **문서 파일**<br>Lumma는 사용자 폴더의 PDF·워드 문서를 골라 간다. 메모장에 적어 둔 비밀번호도 함께 넘어갈 수 있다.
- **PC 정보와 화면**<br>컴퓨터 이름, 운영체제, 설치한 프로그램, 접속 국가 같은 정보와 감염 순간의 화면을 함께 담는다. 사는 쪽은 이것으로 회사 PC인지, 쓸 만한 로그인지 가늠한다.

## 국내를 노린 공격이 더 가져가는 것

국내를 겨냥한 공격은 공동인증서까지 노린다. ASEC은 2025년 4월 지방세 고지 같은 공지로 위장한 바로가기(LNK) 파일을 분석했다. 이 공격은 엣지·크롬·파이어폭스와 함께 네이버 웨일의 로그인 정보를 모았고, 공동인증서(NPKI)와 공무원용 행정전자서명(GPKI)도 가져갔다.

카카오톡 PC 버전도 표적이 됐다. 지니언스는 2025년 11월, 북한 연계로 추정되는 조직이 감염 PC에 로그인된 카카오톡으로 지인들에게 악성 파일을 보낸 사례를 공개했다. 공격자는 피해자의 휴대폰을 원격 초기화해 알림까지 막았다.

- **불확실**<br>두 사례 모두 국내를 노린 특정 공격이다. 시중에 팔리는 인포스틸러가 공동인증서나 카카오톡을 기본으로 가져간다는 공개 분석은 아직 없고, 카카오톡 사례의 악성코드도 원격 조종형(RAT)이다.

## 로그 한 묶음의 모습

훔친 정보는 PC 한 대마다 폴더 하나로 묶인다. 계열마다 파일 이름은 다르지만, 공개 분석에 나오는 구성은 대체로 이렇다.

| 구성 | 내용 |
|---|---|
| 비밀번호 목록 | 사이트 주소·아이디·비밀번호를 한 줄씩 적은 텍스트 파일 |
| 쿠키 폴더 | 브라우저 프로필별 로그인 쿠키 |
| 자동 완성 | 이름·주소·카드 정보 |
| 수집 파일 | 바탕화면과 문서 폴더에서 가져온 파일 사본 |
| 시스템 정보 | 국가, 운영체제, 하드웨어, 사용자 이름, 감염 날짜 |
| 화면 캡처 | 감염 순간의 화면 |

이런 폴더 수천 개가 한 묶음으로 돈다. 어디서 어떻게 팔리는지는 4편에서 다룬다.

## 계열마다 조금씩 다르다

| 계열 | 특징 |
|---|---|
| Lumma | 2022년부터 팔린 대표 계열. 2025년 5월 국제 단속 뒤에도 2026년 5월 국내 탐지 1위(ASEC) |
| Remus | 2026년에 나온 Lumma의 64비트판. 훔친 로그인 세션을 되살려 오래 쓰는 기능을 내세운다 |
| Vidar | 2018년부터 이어진 계열. 2025년 새 버전이 나온 뒤 2026년 초 감염의 70% 이상을 차지했다 |
| StealC | 브라우저 말고도 메신저·메일·VPN·게임까지 넓게 가져간다 |
| AMOS | 맥 전용. 가짜 암호 입력 창으로 맥 로그인 암호를 받아 내 키체인을 연다 |

가져가는 대상은 크게 같다. 차이는 얼마나 빨리, 넓게 가져가고 탐지를 얼마나 잘 피하느냐에 있다. 단속과 그 뒤의 재편은 5편에서 다룬다.

## 털고 사라지나, 남아 있나

앞의 기록에서 Lumma는 24초 만에 끝났고, 자동 실행 등록처럼 PC에 남는 흔적을 만들지 않았다. 끝나는 순간 PowerShell 스크립트를 하나 띄웠지만 오류로 실행되지 못해, 무엇을 하려 했는지는 알 수 없다.

인포스틸러 본체는 대개 이렇게 한 번 훔치고 빠진다. Zscaler는 StealC에 PC에 남는 기능이 없다고 밝혔다. Lumma와 StealC는 일을 마친 뒤 스스로 지우는 기능도 설정으로 켤 수 있다.

왜 남지 않을까. 표적 공격에서는 PC 한 대에 침투하는 일 자체가 어렵다. 그래서 한번 들어가면 오래 숨어 지내며 더 깊이 침투하고 확산시키는 것이 목표가 된다. 하지만 인포스틸러는 목표 자체가 다르다. 불특정 다수의 PC를 감염시켜 최대한 많은 사람의 정보를 모으는 것이 인포스틸러의 고유 미션이다. 한 대에 오래 머물수록 백신이나 사용자에게 들킬 위험만 커진다. 감염은 사용자가 크랙을 직접 실행해 주는 식으로 대량으로 일어난다. 마이크로소프트도 인포스틸러가 보통 조사를 어렵게 하려고 스스로 지운다고 설명한다.

- **추론**<br>쉽게 얻은 감염이니 PC 한 대를 지킬 이유도 적다. 들키기 전에 빨리 털고 빠지는 것이 인포스틸러의 가장 큰 임무다.

그렇다고 감염이 한 번으로 끝나지는 않는다. 공개 분석을 살펴본 대표 계열 9개 중 8개가 다른 악성코드를 더 내려받는 기능을 갖췄다. 안랩은 국내 소프트웨어로 위장한 스틸러가 정보를 뺀 뒤 가상자산 주소를 바꿔치기하는 악성코드를 깔고, 그 악성코드가 재부팅 뒤에도 남은 사례를 보고했다. 크랙 설치 파일 하나가 스틸러와 프록시웨어를 함께 깐 사례도 있다. 남아서 하는 일은 스틸러가 아니라 이렇게 따로 내려받은 악성코드가 맡는다. 반면 맥용 AMOS는 2025년 7월부터 백도어를 달아 스스로 PC에 남는다.

악성코드가 지워져도 훔친 로그인 상태는 남는다. Okta가 분석한 로그 묶음에서 구글 로그인 토큰의 94%가 아직 만료 전이었다. 비밀번호만 바꿔서는 이미 넘어간 로그인 상태가 끊기지 않는다.

- **불확실**<br>PC에 남는 기능이 스틸러 자체에 있는지, 함께 깔린 설치 프로그램에 있는지는 분석마다 구분이 다르다. 스스로 지우는 기능도 계열과 설정에 따라 켜기도 하고 끄기도 한다. 감염 뒤에도 남아 새로 저장된 비밀번호를 계속 보내는 상주형 스틸러도 나오고 있다.

## 예방과 대응

브라우저에 비밀번호와 카드를 저장하는 대신 비밀번호 관리자를 쓰고, 자동 로그인은 필요한 곳에만 켠다. 감염이 의심되면 PC부터 정리하고, 깨끗한 기기에서 비밀번호를 바꾼 뒤 '모든 기기에서 로그아웃'으로 서버 쪽 세션을 끊는다. 로그아웃하면 공격자가 가진 쿠키 속 세션 ID가 무효가 된다. 비밀번호만 바꾸면 훔쳐 간 로그인 상태는 그대로 남는다.

## 출처

- SpyCloud, 2026 신원 노출 보고서 보도자료(2026-03): https://spycloud.com/newsroom/annual-identity-exposure-report-2026
- AhnLab, 자동 로그인과 브라우저 저장 정보: https://www.ahnlab.com/en/contents/content-center/35309
- BleepingComputer, 크롬 앱 바인딩 암호화 도입(2024-07): https://www.bleepingcomputer.com/news/security/google-chrome-adds-app-bound-encryption-to-block-infostealer-malware/
- SpyCloud, 크롬 앱 바인딩 암호화 우회(2025-02): https://spycloud.com/blog/infostealers-bypass-new-chrome-security-feature/
- Okta, 세션 토큰 탈취 분석(2026-09): https://www.okta.com/blog/threat-intelligence/signing_in_without_actually_signing_in/
- CircleCI, 2023년 1월 사고 보고: https://circleci.com/blog/jan-4-2023-incident-report/
- Google TAG, 유튜브 크리에이터 대상 쿠키 탈취(2021-10): https://blog.google/threat-analysis-group/phishing-campaign-targets-youtube-creators-cookie-theft-malware/
- 위키트리, KISA 유튜브 계정 해킹 경고(2026-11): https://www.wikitree.co.kr/articles/1176145
- Recorded Future, 2025 신원 위협 보고서: https://www.recordedfuture.com/blog/identity-trend-report-march-blog
- Google, 기기 결합 세션(DBSC) 공개(2026-04): https://blog.google/security/protecting-cookies-with-device-bound-session-credentials/
- Google Workspace Updates, 윈도우 크롬 DBSC 일반 제공(2026-05): https://workspaceupdates.googleblog.com/2026/05/prevent-account-takeovers-with-DBSC-now-generally-available-in-the-Chrome-browser-for-Windows.html
- Microsoft, Lumma 스틸러 분석(2025-05): https://www.microsoft.com/en-us/security/blog/2025/05/21/lumma-stealer-breaking-down-the-delivery-techniques-and-capabilities-of-a-prolific-infostealer/
- Sophos, 맥 스틸러 AMOS: https://www.sophos.com/en-us/blog/why-amos-matters-the-macos-malware-stealing-data-at-scale
- AhnLab ASEC, 공지문 위장 LNK(2025-04): https://asec.ahnlab.com/en/87620/
- 디지털데일리, 카카오톡 PC 세션 악용 공격(2025-11): https://www.ddaily.co.kr/page/view/2025111009224254115
- Flare, 인포스틸러 로그의 구성: https://flare.io/learn/resources/blog/infostealer-malware/
- AhnLab ASEC, 2026년 5월 인포스틸러 동향: https://asec.ahnlab.com/en/94172
- BleepingComputer, Remus 스틸러(2026-05): https://www.bleepingcomputer.com/news/security/inside-the-remus-infostealer-session-theft-maas-and-rapid-evolution/
- Flashpoint, 2026 상반기 인포스틸러 위협: https://flashpoint.io/resources/report/flashpoint-global-threat-intelligence-report-midyear-2026/information-stealing-malware-threats/
- Zscaler, StealC 변화 추적(2025-05): https://www.zscaler.com/blogs/security-research/i-stealc-you-tracking-rapid-changes-stealc
- Malwarebytes, 쿠키 탈취와 구글 계정(2024-01): https://www.malwarebytes.com/blog/news/2024/01/info-stealers-can-steal-cookies-for-permanent-access-to-your-google-account
- AhnLab ASEC, RecordBreaker와 ClipBanker 설치(2023-05): https://asec.ahnlab.com/en/52542
- AhnLab ASEC, 2026년 2월 인포스틸러 동향: https://asec.ahnlab.com/en/92902/
- Moonlock, AMOS 백도어 추가(2025-07): https://moonlock.com/amos-stealer-backdoor
- Microsoft, StealC와 Amadey 분석(2026-06): https://www.microsoft.com/en-us/security/blog/2026/06/24/stealc-and-amadey-breaking-down-infostealers-and-the-cybercrime-services-that-deliver-them
- SecurityWeek, 상주형 Venom Stealer(2026-03): https://www.securityweek.com/venom-stealer-raises-stakes-with-continuous-credential-harvesting/

---
이 글은 공개 자료와 C.TiSSUE 조사를 사람이 정리한 연재입니다. 조사 방법과 도구: https://github.com/jsik22/ctissue
