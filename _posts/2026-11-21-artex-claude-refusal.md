---
layout: post
title: "ARTEX — FBI가 경고한 AI 자율 침투 도구, 최신 변형 실측과 탐지"
date: 2026-11-21
tags: [artex, agentic-ai, llm-guardrails, deepseek, fbi-flash]
description: "FBI FLASH가 경고한 AI 자율 침투 도구 ARTEX의 현황, Claude 모델 거절 실험, 최신 변형 두 계열의 실험실 실측을 정리했다. UA를 무작위로 바꾸는 변형은 공개 UA 룰을 피했고, 요청 헤더 모양을 보는 anti-artex 룰이 잡았다."
sources:
  - {name: "FBI FLASH-20261113-001, Threat Overview of ARTEX Autonomous Penetration Testing Framework (2026-11-13, TLP:CLEAR)", url: "https://www.ic3.gov/CSA/2026/261113.pdf"}
  - {name: "Check Point Research, Hacking on Autopilot: Inside ARTEX Cybercrime Campaigns (2026-11-05)", url: "https://research.checkpoint.com/2026/hacking-on-autopilot-inside-artex-cybercrime-campaigns/"}
  - {name: "GitGuardian, An ARTEX build ships no secret scanner. In our lab it found the secrets anyway (2026-11-10)", url: "https://blog.gitguardian.com/an-artex-build-ships-no-secret-scanner-in-our-lab-it-found-the-secrets-anyway/"}
  - {name: "CrowdStrike, Unknown Threat Actor Uses ARTEX to Target South Korean Finance (2026-10-07)", url: "https://www.crowdstrike.com/en-us/blog/unknown-threat-actor-uses-artex-to-target-south-korean-finance/"}
  - {name: "CrowdStrike, VAPOR PANDA Uses AI-Driven ARTEX to Scale a Global Espionage Campaign (2026-10-29)", url: "https://www.crowdstrike.com/en-us/blog/vapor-panda-uses-artex-to-scale-global-campaign/"}
  - {name: "Hunt.io, ARTEX Exposed: Infrastructure Behind the South Korea Bank Hack (2026-11-04)", url: "https://hunt.io/blog/artex-south-korea-bank-infrastructure"}
  - {name: "MalwareMustDie, MMD-072-2026 ARTEX Detection Signature (2026-11-13)", url: "https://blog.malwaremustdie.org/2026/11/mmd-072-2026-artex-detection-signature.html"}
  - {name: "Theori, aipt-detections (2026-10-12)", url: "https://github.com/theori-io/aipt-detections"}
excerpt: "FBI FLASH가 경고한 AI 자율 침투 도구 ARTEX의 현황, Claude 모델 거절 실험, 최신 변형 두 계열의 실험실 실측을 정리했다. UA를 무작위로 바꾸는 변형은 공개 UA 룰을 피했고, 요청 헤더 모양을 보는 anti-artex 룰이 잡았다."
updated: 2026-11-22 22:01:38 +0900
---

> 최근 업데이트(11월 22일 20시): 현장에서 쓰이는 최신 ARTEX 변형 두 계열을 실험실에서 실측했다. 도구 UA를 무작위 브라우저 값으로 바꾸는 계열에는 공개 UA 룰이 거의 걸리지 않았고, 요청 헤더 모양을 보는 anti-artex 룰이 도구 요청을 모두 잡았다. 결과는 anti-artex 룰 v0.4.0에 반영했다. 지난 기록은 글 끝 [업데이트 기록](#업데이트-기록)에 있다.

**목차**
* 목차
{:toc}

## 현재 상태

| 항목 | 상태 (11월 22일 20시 기준) |
|---|---|
| 도구 | ARTEX. LLM이 계획·실행을 맡는 자율 침투 도구. 원본 저장소는 10월 8일 비공개 전환. 원 개발자라고 밝힌 계열은 v0.3.30(11월 14일), 가장 널리 퍼진 영문 재배포판은 v0.7.0(11월 21일)까지 나왔다 |
| 경고 | FBI FLASH(11월 13일). 8월부터 여러 행위자가 악용, 금융·정부·국방·에너지 등 표적 |
| 노출 콘솔 | 11월 초 1,600개 이상(FBI·Check Point) |
| 쓰이는 모델 | 중국 모델 위주, DeepSeek Flash 계열이 가장 많음(FBI·Check Point) |
| Claude 모델 | 거절 사례가 공개 보고 2건과 우리 실험실에서 모두 확인됨 |
| 국내 | 10월 국내 금융권 침입에 쓰였다는 CrowdStrike 보고, 같은 인프라를 넓혀 본 Hunt.io 보고 |
| 공개 탐지 자료 | MalwareMustDie·Theori 룰 묶음, CTiSSUE [anti-artex](https://github.com/jsik22/anti-artex)(탐지 룰, IOC 목록) |

## 무슨 일인가

ARTEX는 목표만 주면 LLM 에이전트 여럿이 정찰부터 침투·자료 수집까지 알아서 이어 가는 침투 테스트 도구다. 모델을 내장하지 않고 외부 LLM API에 붙인다. FBI는 11월 13일 FLASH 경보에서 8월부터 여러 행위자가 이 도구를 무기화했고, 일부 침입은 표적 선정부터 특권 획득까지 3시간이 안 걸렸다고 밝혔다. 같은 경보는 공격자 쪽 ARTEX에서 DeepSeek Flash 계열이 가장 많이 쓰이며, 이유로 낮은 비용과 '침투 테스트'로 포장했을 때 안전장치가 보이지 않는 점을 들었다.

CTiSSUE는 이 도구의 탐지 지문을 만들기 위해 인터넷과 분리된 실험실에 ARTEX를 소스에서 빌드해 올렸다. 표적은 연습용 취약 앱(OWASP Juice Shop) 하나였고, 모델은 Anthropic Claude Sonnet 5.5를 붙였다. 결과는 표적으로 간 요청 0건이었다. 모델이 ARTEX의 계획 요청을 거절했다.

## 공개된 근거

### ARTEX와 악용 현황

| 항목 | 내용 |
|---|---|
| 개발·공개 | 7월 GitHub 공개, 10주 만에 별 1,500~2,000개. 개발자는 10월 8일 저장소를 내렸다(FBI) |
| 악용 시작 | 늦어도 8월(FBI) |
| 표적 업종 | 금융, 정부, 기술, 국방, 반도체, 에너지, 의료, 제조, 통신, 법률(FBI) |
| 노출 콘솔 | 2,557곳에서 ARTEX 패널 확인, 1,608곳이 조사 시점에 살아 있음(Check Point) |
| 국내 사례 | 공개 디렉터리에서 ARTEX와 Claude Code 작업 기록이 나왔고, ARTEX의 주 모델은 DeepSeek v4.1-flash였다(CrowdStrike, 10월 7일) |
| 국가 배후 의심 | VAPOR PANDA가 DeepSeek로 ARTEX를 돌려 200곳 넘는 조직을 노렸다(CrowdStrike, 10월 29일) |

### 모델 선택과 거절

- **Check Point(11월 5일)**<br>데이터베이스가 통째로 노출된 공격자 ARTEX 10곳을 분석했다. 미국 모델을 설정한 곳은 3곳이었고, 안전장치에 걸린 것은 Anthropic Opus 5.5를 쓸 때뿐이었다. 그 운영자는 이후 더 관대한 중국 모델로 바꿨다. 분석한 캠페인들은 DeepSeek v4.1 Flash·v4 Pro, Kimi K3.1 Pro를 썼다.

- **GitGuardian(11월 10일)**<br>밀폐된 실험실에서 ARTEX v0.3.15를 Claude Sonnet으로 돌렸다. 찾은 자격 증명을 써 보라는 지시 두 가지는 출력도 명령도 없이 끝났다. Claude Opus로 다시 돌리자 첫 요청에서 도구를 하나도 쓰기 전에 명시적 거절이 나왔다. ARTEX 없이 같은 요청을 보내도 같은 거절이 나와, 거절은 도구가 아니라 모델에서 온다고 결론지었다.

- **FBI FLASH(11월 13일)**<br>ARTEX가 Claude·GPT 같은 상용 모델과 DeepSeek·GLM·Grok·Kimi에 모두 붙는다고 적었다. 서버에서 `api.anthropic.com`, `api.deepseek.com`, `open.bigmodel.cn`, `api.x.ai`로 예상하지 못한 호출이 나가면 다른 의심 활동과 함께 조사하라고 권고했다.

## CTiSSUE로 본 것

실험실 구성(Claude 실험, 11월 21일): ARTEX v0.3.15(원본의 마지막 공개 버전)를 원본 소스에서 빌드했고, 표적은 같은 격리망의 Juice Shop 하나였다. 바깥으로는 Anthropic API 하나만 나갈 수 있게 막았다. 작업은 'Juice Shop의 웹 취약점을 찾아 검증하라' 하나였고, 표적 범위도 그 호스트 하나로 제한했다.

- **관찰: 계획 단계에서 거절**<br>작업 첫 단계인 목표 분해 호출은 출력 0토큰으로 끝났다. 원인을 보려고 ARTEX의 LLM 기록을 켠 뒤 받은 계획(planner) 응답 2건 중 1건은 도구 호출, 1건은 종료 사유 `refusal`에 본문이 비어 있었다.

- **관찰: 표적 요청 0건**<br>약 2분 동안 모델 호출 41번, 입력 약 65만 토큰을 썼지만, 표적 쪽 패킷 캡처에는 ARTEX가 보낸 HTTP 요청이 하나도 없었다.

- **관찰: ARTEX는 거절을 구분하지 않는다**<br>ARTEX는 본문도 도구 호출도 없는 응답을 '생각만 하고 끝난 빈 회차'로 보고, 계속하라는 재촉 지시를 붙여 다시 보낸다. 거절을 따로 처리하는 코드는 없다. 실제로 워커들은 재촉 두 번 뒤 결과 0건으로 '완료' 처리됐고, 계획 단계는 새 작업을 계속 만들어 냈다. 거절된 채로 토큰만 쓰는 구조다.

- **추론: 우리 관찰은 공개 보고와 같은 방향이다**<br>Check Point(실제 공격자 설정), GitGuardian(실험실, Sonnet·Opus), CTiSSUE(실험실, Sonnet 5.5) 세 곳에서 Claude 모델은 ARTEX의 침투 작업을 진행하지 않았다. 다만 우리는 워커 단계의 응답 원문을 기록하기 전에 작업을 멈췄고, 같은 결과를 반복 확인하지는 않았다. 계획 단계 거절 기록과 표적 요청 0건이 근거의 전부다.

- **추론: 거절은 방어가 아니다**<br>ARTEX는 모델을 설정 한 줄로 바꾼다. Check Point가 본 운영자처럼 거절당하면 DeepSeek 같은 모델로 옮기면 된다. 방어하는 쪽은 특정 모델의 안전장치에 기대지 말고, ARTEX가 어떤 모델로 돌든 남기는 흔적을 봐야 한다.

거절을 피하려고 지시문을 고치는 시험은 하지 않았다. 모델이 거절하면 멈추고 기록한다는 원칙을 따랐고, GitGuardian도 같은 원칙을 밝혔다.

## 탐지 자료: anti-artex

CTiSSUE는 ARTEX 탐지 자료를 [anti-artex](https://github.com/jsik22/anti-artex) 저장소로 공개했다. 여러 보고와 저장소에 흩어진 IOC와 탐지 룰을 직접 확인하고 실측해 한곳에 쌓는 것이 목표다.

| 항목 | 내용 |
|---|---|
| 탐지 룰 | Snort 3·Suricata 룰. ARTEX 도구 UA, 포크 UA, 백도어 콘솔 로그인, 요청 헤더 모양, 사냥용 행동 룰 |
| IOC | 공개 보고의 지표(IP·도메인·User-Agent 등)에 지금 상태와 권고 조치(차단 검토·사냥·차단 금지 등)를 붙인 목록 |
| 시험 | 합성 pcap 회귀 시험 통과(정상 오탐 0). 실험실 캡처(v0.3.15, 최신 변형 두 계열)로 공개 룰 묶음과 비교 |

- **가짜 모델 실측**<br>실제 모델 대신, 정해 둔 도구 호출을 돌려주는 가짜 모델로 ARTEX 엔진을 돌렸다. 표적에 가는 요청은 ARTEX 도구가 만든 실제 트래픽이고, 같은 트래픽을 몇 번이든 재현해 룰을 시험할 수 있다. 모델이 어떤 전술을 고르는지는 재현하지 않는다.

- **관찰: 워커의 curl 공격은 ARTEX 전용 룰에 걸리지 않았다**<br>도구 UA를 그대로 쓰는 버전(v0.3.15·v0.7.0)에서는 공개 룰 묶음과 anti-artex 모두 ARTEX 도구(WebFetch, api-recon 스크립트)가 만든 요청을 잡았다. 워커가 curl로 보낸 로그인·검색 변형 요청은 ARTEX 전용 룰 어디에도 걸리지 않았다. anti-artex는 이 빈자리에 로그인 연속 시도를 보는 사냥용 룰을 더했다.

- **관찰: UA를 무작위로 바꾸는 최신 변형도 요청 모양은 그대로다**<br>원 개발자라고 밝힌 계열의 v0.3.30은 ARTEX 도구의 UA를 표준 브라우저 UA 20개 중 하나로 무작위로 바꾼다. 실험실에서 이 계열을 돌리자 공개 묶음의 ARTEX 전용 룰은 하나도 울리지 않았다(Theori·MalwareMustDie 0건). 요청을 보내는 Go·Python 클라이언트의 헤더 모양은 그대로라, "브라우저 UA인데 도구 기본 헤더 모양"을 보는 anti-artex 룰이 도구 요청 24건을 모두 잡았다. 공개 묶음에 없는 유형이다. 가장 널리 퍼진 v0.7.0은 기본값에서 도구 UA를 그대로 써 공개 UA 룰에도 걸린다.

- **관찰: 최신 영문 재배포판은 스캐너와 속도 조절을 갖췄다**<br>v0.7.0 이미지에는 nuclei·httpx·katana·ffuf·sqlmap 같은 스캐너와 헤드리스 브라우저가 들어 있고, 호스트별 속도 제한을 거는 기능이 생겼다. 가짜 모델 실측에서 속도 조절은 경보 수를 바꾸지 않았다. 스캐너는 각자의 기본 UA나 무작위 브라우저 UA를 쓴다.

- **관찰: ARTEX 기록 프록시는 흔적을 남긴다**<br>요청 기록 기능을 켜면 셸 도구 요청에 프록시용 `Proxy-Connection` 헤더가 남아 표적 서버까지 간다. UA와 상관없는 신호라 anti-artex v0.4.0에 사냥용 룰로 넣었다.

- **관찰: 공개 IOC의 절반은 최근 관측이 없다**<br>가장 잘 정리된 공개 IOC 목록에서도 IP 절반은 제3자 색인에 최근 관측이 없었다. 지금 ARTEX 콘솔이 열린 지표는 모두 그 목록 밖에 있었다. 지금 열린 콘솔 IP는 다른 사람이 가로챌 수 있어 공개 목록에서 뺐다.

- **추론: 일부 공개 지표는 ARTEX가 아니라 운영자 몫이다**<br>위치 조회(db-ip), 웹훅 반출, DNS 콜백 서비스 호출은 ARTEX 내장 지시문과 코드에 없다(관찰). 이 지표들은 운영자가 붙인 프롬프트·도구에서 나온 것으로 보고, 운영자마다 다를 수 있다.

## 조직이 점검할 것

1. **서버의 LLM API 호출**<br>웹 서버·DB 서버처럼 LLM을 쓸 이유가 없는 서버에서 `api.deepseek.com`, `open.bigmodel.cn`, `api.x.ai`, `api.anthropic.com` 같은 도메인으로 나가는 연결을 찾는다. 공격자가 침투한 서버에서 에이전트를 돌리는 경우를 잡는다.

2. **내부망의 ARTEX 콘솔**<br>기본 설정의 ARTEX 콘솔은 8787/tcp로 모든 인터페이스에 열린다. 내부 자산 점검에서 이 포트에 ARTEX 콘솔이 응답하는 호스트가 있는지 본다. 승인된 테스트 장비가 아니면 조사 대상이다.

3. **빠른 순차 열거**<br>FBI는 로그인·검색 엔드포인트에 대한 고빈도 순차 열거, 조금씩 바꾼 실패 요청의 반복, 하위 에이전트들의 중복 요청을 행동 지표로 들었다. 사람이 하기 어려운 속도와 패턴의 요청을 WAF·웹 로그에서 본다.

4. **피해 서버의 외부 조회**<br>FBI는 침투한 서버에서 위치 확인용 `api.db-ip.com` 조회와 `webhook.site`로의 명령 출력 전송을 지표로 들었다. 서버에서 이 두 도메인으로 가는 DNS 질의와 연결을 찾는다.

5. **공개 탐지 자료 함께 쓰기**<br>MalwareMustDie·Theori 룰 묶음과 anti-artex 룰을 탐지 모드로 함께 넣는다. IOC는 권고 조치 열을 보고 쓴다(정상 서비스·CDN 뒤 주소는 막지 않는다). ARTEX 전용 룰이 울리지 않아도 공격이 없다는 뜻은 아니다. 모델이 ARTEX 도구 대신 curl만 쓰면 일반 웹 공격 탐지의 몫이다. MalwareMustDie 룰 4260110은 Suricata 7.0.17에서 로드되지 않아 설정 시험(`suricata -T`)이 실패한다.

6. **UA에 기대지 않기**<br>최신 변형은 도구 UA를 브라우저 값으로 바꾼다. TLS를 푸는 지점(WAF·리버스 프록시)에서 요청 헤더 모양을 보는 룰을 함께 쓰고, sqlmap·ffuf 같은 스캐너 기본 UA와 `HeadlessChrome` UA는 일반 웹 공격 탐지로 본다.

7. **노출 면 줄이기**<br>FBI 권고대로 외부에 열린 관리 인터페이스(클라우드 관리, 배포·게시 엔드포인트, 백업, 하이퍼바이저)를 신뢰 망으로 옮긴다. ARTEX는 이미 알려진 노출과 약한 설정을 기계 속도로 엮는 도구라, 남아 있는 노출 면이 곧 표적이다.

## 업데이트 기록

| 시각(KST) | 내용 |
|---|---|
| 11월 21일 15시 | 첫 게시 |
| 11월 22일 06시 | anti-artex 저장소 공개 내용 반영(탐지 룰·IOC·실험실 실측), 출처 3곳 추가 |
| 11월 22일 20시 | 최신 변형 두 계열(v0.7.0·v0.3.30) 실험실 실측 결과와 anti-artex 룰 v0.4.0 반영 |
| 11월 22일 22시 | 탐지 자료 설명을 anti-artex 저장소 기준으로 정리(버전·시험 수치 대신 저장소 참조, 공개 묶음 비교 기준, MalwareMustDie 4260110 로드 실패) |

## 출처

- FBI FLASH-20261113-001, Threat Overview of ARTEX Autonomous Penetration Testing Framework (2026-11-13, TLP:CLEAR)
- Check Point Research, Hacking on Autopilot: Inside ARTEX Cybercrime Campaigns (2026-11-05)
- GitGuardian, An ARTEX build ships no secret scanner. In our lab it found the secrets anyway (2026-11-10)
- CrowdStrike, Unknown Threat Actor Uses ARTEX to Target South Korean Finance (2026-10-07)
- CrowdStrike, VAPOR PANDA Uses AI-Driven ARTEX to Scale a Global Espionage Campaign (2026-10-29)
- Hunt.io, ARTEX Exposed: Infrastructure Behind the South Korea Bank Hack (2026-11-04)
- MalwareMustDie, MMD-072-2026 ARTEX Detection Signature (2026-11-13)
- Theori, aipt-detections (2026-10-12)

---
이 글은 CTiSSUE가 공개 정보만으로 자동 조사한 결과를 사람이 검토해 올린 것입니다. 추적기·게시글의 내용은 주장이며 확정된 사실이 아닙니다. 조사 방법과 도구: https://github.com/jsik22/ctissue
