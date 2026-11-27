---
layout: post
title: "Warden Stealer — Claude Code·Codex 토큰을 노리는 인포스틸러, 공개 IOC로 다시 보기"
date: 2026-11-17
tags: [wardenstealer, infostealer, ai-agent, claude-code, maas, ioc]
description: "Warden Stealer는 v1.9부터 AI 코딩 에이전트의 OAuth·API 토큰을 표준 수집한다. 공개 IOC를 abuse.ch 피드와 대조했더니 겹침이 작았고, C2 일부는 11월 중순까지 관측됐다."
sources:
  - {name: "Gen Threat Labs, Warden Stealer: The Rapid Rise of an Infostealer with an Appetite for AI Agent Data (2026-10-08)", url: "https://www.gendigital.com/blog/insights/research/warden-stealer-rust-infostealer-analysis"}
  - {name: "Gen Threat Labs, Infostealers Have Found a New Target: Your AI Agent (2026-09-08)", url: "https://www.gendigital.com/blog/insights/research/infostealers-your-ai-agent"}
  - {name: "SpyCloud Labs, IntelStealers: the infostealers going after AI tokens and AI configuration data (2026-10-30)", url: "https://spycloud.com/blog/infostealers-now-target-ai-tokens-and-api-keys/"}
  - {name: "SOC Prime, Warden Stealer: The Rapid Rise of an Infostealer with an Appetite for AI Agent Data (2026-10-12)", url: "https://socprime.com/active-threats/warden-stealer-the-rapid-rise-of-an-infostealer-with-an-appetite-for-ai-agent-data/"}
  - {name: "Cyber Security News, Hackers Use New Infostealer Malware to Steal ChatGPT, Claude, and Codex Tokens (2026-11-04)", url: "https://cybersecuritynews.com/hackers-use-new-infostealer-malware/"}
  - {name: "Gen Threat Labs IOC 저장소 (WardenStealer)", url: "https://github.com/gendigitalinc/ioc/tree/master/WardenStealer"}
excerpt: "Warden Stealer는 v1.9부터 AI 코딩 에이전트의 OAuth·API 토큰을 표준 수집한다. 공개 IOC를 abuse.ch 피드와 대조했더니 겹침이 작았고, C2 일부는 11월 중순까지 관측됐다."
updated: 2026-11-19 10:11:25 +0900
---

> 최근 업데이트(11월 19일): 노트 양식을 정리했다(내용 변화 없음). 지난 기록은 글 끝 [업데이트 기록](#업데이트-기록)에 있다.

**목차**
* 목차
{:toc}

## 현재 상태

| 항목 | 상태 (11월 18일 기준) |
|---|---|
| 정체 | Rust로 만든 윈도 인포스틸러, 구독형 MaaS(v1.9 이후 월 450~1,500달러) |
| 노리는 데이터 | v1.9(9월 29일 공지)부터 Claude Code, Codex CLI, Cursor, Grok 등 AI 코딩 에이전트 자격 증명 파일 표준 수집 |
| 규모 | SpyCloud: 8월 3일 이후 이 계열 고유 감염 13만 건 넘게 재수집 |
| 유포 경로 | 크랙 소프트웨어, 게임 치트, 악성 광고, ClickFix |
| 공개 IOC | Gen IOC 저장소. abuse.ch 피드와 겹침 작음(해시 21건 중 MalwareBazaar 2건, C2 도메인 50개 중 ThreatFox 21개) |
| C2 활동 | 표본 C2 10개 중 3개가 11월 16일까지 수동 DNS에 관측 |

## 무슨 일인가

Warden Stealer는 Rust로 만든 윈도 인포스틸러로, 구독형 MaaS로 팔린다. Gen Threat Labs는 자사가 CallbackBeaver라고 부르던 악성코드가 이것이라고 밝혔다. 2026년 9월 29일 공지된 v1.9부터 Claude Code, Codex CLI, Cursor, Grok 같은 AI 코딩 에이전트의 자격 증명 파일을 표준으로 수집한다. SpyCloud는 8월 3일 이후 이 계열의 고유 감염을 13만 건 넘게 재수집했다고 밝혔다. 유포 경로는 크랙 소프트웨어, 게임 치트, 악성 광고, ClickFix(웹 페이지가 시키는 명령 붙여넣기)다.

## 공개된 근거

- **Gen(10-08)**<br>자체 로더가 스틸러를 explorer.exe에 주입한다. 빌드마다 난독화를 바꾸고, 크롬 계열의 App-Bound Encryption을 우회해 쿠키를 복호화한다. 암호화폐 클리퍼도 들어 있다. 추가 페이로드는 certutil로 받아 msiexec로 조용히 설치한다. 가격은 v1.9 이후 월 450~1,500달러다.

- **SpyCloud(10-30)**<br>재수집한 로그에 Claude Code의 OAuth 자격 증명 파일(access·refresh 토큰, MCP 서버 토큰), CLAUDE.md, Codex CLI 인증 파일(OpenAI API 키 포함)이 있었다.

| 출처 | 핵심 |
|---|---|
| SOC Prime(10-12) | explorer.exe 주입과 certutil 다운로드를 탐지 규칙으로 제시했다. |
| 출처마다 다른 점 | 첫 관측을 Gen은 5월 초(빌드), SpyCloud는 8월 3일로 적었다. |

## CTiSSUE로 본 것

- **관찰**<br>Gen이 블로그에 공개한 해시 21건 중 MalwareBazaar에 등록된 것은 2건이었다. 그중 한 건은 MalwareBazaar가 WARDENStealer로, ThreatFox가 Raven Stealer로 분류했다. Gen의 C2 도메인 50개 중 ThreatFox의 WARDENStealer 목록(IOC 598건)에 있는 것은 21개였다.

- **관찰**<br>표본 C2 10개 중 8개는 Cloudflare 뒤에 있었고, 3개는 11월 16일까지 수동 DNS에 관측됐다. 하나는 도메인 500개 이상이 모인 공유 호스팅으로 옮겨 운영자 인프라로 볼 근거가 없다.

- **관찰**<br>랜섬웨어 추적기, 공개 텔레그램 채널, 다크웹 검색에서 Warden 언급은 찾지 못했다.

- **추론**<br>피드 하나로 만든 차단 목록은 상당수를 놓친다. webex, azureupdate처럼 정상 서비스를 사칭한 C2 이름은 프록시 로그에서 정상 트래픽처럼 보이기 쉽다.

- **다루지 않은 것**<br>판매처로 보도된 Exploit·XSS 포럼의 게시글은 이 글에서 다루지 않았다. 찾지 못했다는 것이 유통이 없다는 뜻은 아니다.

## 조직이 점검할 것

1. **C2 조회 이력**<br>개발자 PC의 프록시·EDR 로그에서 Gen과 ThreatFox의 C2 도메인 조회 이력을 찾는다(8월 이후).

2. **주입·설치 흔적 확인**<br>explorer.exe를 대상으로 한 원격 스레드 생성, certutil 다운로드 뒤 msiexec 무인 설치, Defender의 WardenStealer 탐지 이력을 확인한다.

3. **세션·토큰 재발급**<br>감염이 의심되면 비밀번호 변경으로 끝내지 않는다. AI 서비스 세션을 폐기하고 API 키와 refresh 토큰을 재발급한다. MCP 설정에 넣어 둔 사내 시스템 토큰도 교체한다. 제공자 콘솔에서 사용량과 과금 이상을 확인한다.

4. **설정 파일 비밀 점검**<br>CLAUDE.md와 프로젝트 설정 파일에 비밀번호, 토큰, 내부 주소가 있는지 점검한다. 이 파일은 통째로 수집된다.

5. **외주 개발사 점검**<br>외주 개발사가 사내 저장소나 AI 도구 토큰을 갖고 있다면 같은 점검을 요청한다.

## IOC·TTP

- C2 도메인 일부(Gen 2026-10-08): hroffice[.]work, macfilecloud8[.]com, plainhorizon[.]org, recap-check[.]org, webex-028ue2[.]com, web03-azureupdate[.]com. 전체 목록은 Gen IOC 저장소에 있다.
- 초기 빌드 SHA-256(Gen): 241df5a4ee38658329025152807fcd69b7e40361428000bb56d14cadeb48b437
- T1055 Process Injection: explorer.exe 주입(Gen)
- T1539 Steal Web Session Cookie, T1552.001 Credentials In Files(SpyCloud)
- T1105 Ingress Tool Transfer: certutil 다운로드(Gen)

## 업데이트 기록

| 시각(KST) | 내용 |
|---|---|
| 11월 18일 00시 | 첫 게시 |
| 11월 19일 | 노트 양식 정리(현재 상태 표·목차·업데이트 기록 추가, 항목 형식 통일). 내용 변화 없음 |

## 출처

- Gen Threat Labs, Warden Stealer: The Rapid Rise of an Infostealer with an Appetite for AI Agent Data (2026-10-08)
- Gen Threat Labs, Infostealers Have Found a New Target: Your AI Agent (2026-09-08)
- SpyCloud Labs, IntelStealers: the infostealers going after AI tokens and AI configuration data (2026-10-30)
- SOC Prime, Warden Stealer: The Rapid Rise of an Infostealer with an Appetite for AI Agent Data (2026-10-12)
- Cyber Security News, Hackers Use New Infostealer Malware to Steal ChatGPT, Claude, and Codex Tokens (2026-11-04)
- Gen Threat Labs IOC 저장소 (WardenStealer)

---
이 글은 CTiSSUE가 공개 정보만으로 자동 조사한 결과를 사람이 검토해 올린 것입니다. 추적기·게시글의 내용은 주장이며 확정된 사실이 아닙니다. 조사 방법과 도구: https://github.com/jsik22/cybertissue
