---
layout: post
title: "어떻게 감염되나"
date: 2026-11-25
tags: [infostealer, clickfix, seo-poisoning, crack, phishing]
description: "인포스틸러는 대부분 사용자가 직접 실행해서 들어온다. 공짜 프로그램, 검색 상단의 가짜 다운로드 사이트, 명령어를 붙여넣게 하는 화면, 게임 모드와 런처, 기관을 사칭한 메일이 국내에서 확인된 주요 경로다."
sources:
  - {name: "Kaspersky — 35% of infostealer infections begin with users running files directly from temporary folders", url: "https://kaspersky.com/about/press-releases/kaspersky-35-of-infostealer-infections-begin-with-users-running-files-directly-from-temporary-folders"}
  - {name: "AhnLab ASEC — December 2025 Infostealer Trend Report", url: "https://asec.ahnlab.com/en/92142/"}
  - {name: "AhnLab ASEC — 2026년 5월 인포스틸러 동향 보고서", url: "https://asec.ahnlab.com/ko/94171/"}
  - {name: "AhnLab — Fake Claude Download Sites Spreading Malware", url: "https://www.ahnlab.com/en/contents/content-center/36141"}
  - {name: "AhnLab ASEC — 카카오톡 PC버전 사칭 사이트 유포", url: "https://asec.ahnlab.com/ko/92971/"}
  - {name: "AhnLab — The Hidden Threat Behind Fake CAPTCHAs and Installation Guides", url: "https://www.ahnlab.com/en/contents/content-center/36184"}
  - {name: "BleepingComputer — Over 116,000 Minecraft systems infected in WeedHack malware campaign", url: "https://bleepingcomputer.com/news/security/over-116-000-minecraft-systems-infected-in-weedhack-malware-campaign"}
  - {name: "게임메카 — PC 런처 침해 보도", url: "https://www.gamemeca.com/view.php?gid=1782993"}
  - {name: "데일리시큐 — 개인정보위 직원 사칭 악성메일", url: "https://www.dailysecu.com/news/articleView.html?idxno=209719"}
  - {name: "AhnLab ASEC — Vidar Infostealer Being Spread through Phishing Emails", url: "https://asec.ahnlab.com/ko/94363/"}
kind: series
series: "인포스틸러 생태계"
part: 2
excerpt: "인포스틸러는 대부분 사용자가 직접 실행해서 들어온다. 공짜 프로그램, 검색 상단의 가짜 다운로드 사이트, 명령어를 붙여넣게 하는 화면, 게임 모드와 런처, 기관을 사칭한 메일이 국내에서 확인된 주요 경로다."
updated: 2026-11-25 18:23:10 +0900
---

![위에서 공격자가 낚싯대를 쥐고 있고, 빨간 낚싯줄 세 가닥 끝의 바늘이 세 미끼에 꽂혀 있다. 깨진 자물쇠와 열쇠(크랙·키젠), '무료 압축 프로그램' 검색 결과(맨 위 광고 표시), 금화·검·조준선이 붙은 PC 게임 화면. 줄을 타고 열쇠·쿠키·카드·지갑이 공격자 쪽으로 올라간다](https://jsik22.github.io/cybertissue-notes/assets/2026-11-25-infostealer-kr-2/1-hero.png)

## 해킹당한 것이 아니라 내가 실행한 것

인포스틸러 감염은 대부분 사용자가 파일을 직접 실행하면서 시작된다. 카스퍼스키가 2025년 다크웹 로그 500만 개를 분석해 보니, 감염의 약 35%가 브라우저로 받은 파일이 놓이는 임시 폴더에서 바로 실행된 경우였다. 보안 프로그램을 꺼 달라는 안내를 따른 경우도 있었다.

공격자는 사용자가 의심 없이 누를 만한 곳에 파일을 둔다. 국내에서 확인된 경로를 다섯 가지로 나눠 본다.

## 공짜 프로그램과 검색 결과

가장 오래되고 꾸준한 경로는 크랙·키젠 같은 불법 프로그램이다. 안랩 ASEC은 매달 이 방식의 인포스틸러를 집계하는데, 2026년 1~10월 보고서에 한 달도 빠지지 않고 나온다. 5월에는 신종 Remus가 이 경로 유포의 36%를 차지했다.

공격자는 자기 블로그가 검색에서 막히자 남의 사이트를 빌렸다. 유명 포럼, 기업 홈페이지의 질문 게시판, 관리가 안 된 블로그에 내려받기 글을 심는다. ASEC은 국내 지역 신문사 홈페이지에 올라온 유포 글을 예시로 실었다. 파일은 대용량 파일 공유 서비스나 클라우드 저장소에 둬서 겉보기에는 평범한 내려받기 링크로 보인다.

- **관찰**<br>유포 글은 프로그램 이름과 "무료", "정품 인증" 같은 말로 검색 결과 위쪽을 노린다.
- **추론**<br>정상 사이트에 걸린 글이라 사용자는 그 사이트를 믿고 누른다. 사이트 운영자도 모르는 사이에 유포지가 된다.

## 검색 상단의 가짜 다운로드 사이트

정품 프로그램을 받으려는 사람도 걸린다. 2026년 4월 안랩은 구글에서 "claude desktop"을 검색하면 위쪽에 뜨는 가짜 Claude 다운로드 사이트를 찾았다. 내려받기 버튼을 누르면 명령어를 복사해 실행하라는 창이 뜨고, 따라 하면 파일과 브라우저 저장 정보, 암호화폐 지갑을 빼 간다. 안랩은 검색 광고로 위쪽에 노출됐을 것으로 봤다.

같은 수법은 국내 메신저에도 쓰였다. 올봄 "카카오톡 PC버전"을 검색하면 공식 사이트처럼 꾸민 가짜 페이지가 위쪽에 떴다. 정상 설치 파일을 함께 깔아 의심을 피했고, ASEC은 5,000대 넘는 감염을 확인했다. 이 건의 악성코드는 원격 조종형(RAT)이라 인포스틸러와는 종류가 다르지만, 사용자를 끌어들인 경로는 똑같다.

## 명령어를 붙여넣게 하는 화면

최근 가장 빠르게 늘어난 방식은 사용자에게 명령어를 직접 붙여넣게 하는 것이다. "로봇이 아님을 확인하세요" 같은 가짜 인증 화면이나 오류 창을 띄우고, 해결하려면 단축키를 눌러 붙여넣고 실행하라고 안내한다. 이 방식을 ClickFix라고 부른다.

안랩은 침해된 국내 온라인 매거진 사이트에서 이 화면으로 LummaC2를 퍼뜨린 사례를 공개했다. 사용자가 직접 명령을 실행하므로 내려받은 파일이 없고, 브라우저의 다운로드 경고도 뜨지 않는다.

- **관찰**<br>정상 웹사이트는 사용자에게 명령어를 붙여넣어 실행하라고 하지 않는다.

## 게임 모드와 런처

게임 이용자는 핵·치트·모드를 찾아 낯선 파일을 받는 일이 잦아 표적이 되기 쉽다. 2026년 6월 맥아피는 마인크래프트 무료 모드와 클라이언트로 위장한 인포스틸러가 11만 6천여 대를 감염시켰다고 밝혔다. 유포 링크는 유튜브 시연 영상의 설명란과 댓글에 걸려 있었다.

공식 프로그램이 통로가 된 경우도 있다. 10월 말부터 11월 초까지 국내 개발사가 만든 한 인기 게임의 PC 런처가 악성 원격 에이전트를 내려받았다. 런처가 불러오는 공식 스크립트가 바뀌었고, 에이전트는 브라우저에 저장된 비밀번호·쿠키, 암호화폐 지갑, 개발자 자격 증명을 노렸다. 퍼블리셔와 개발사가 사과했지만 실제 피해 규모는 아직 공개되지 않았다.

- **추론**<br>공식 런처가 바뀌면 사용자는 피할 방법이 거의 없다. 이런 공급망 침해는 드물지만 한 번에 많은 사람이 노출된다.

## 기관·회사를 사칭한 메일

메일 첨부도 여전히 쓰인다. ASEC은 2026년 7월 이력서나 저작권 침해 안내로 꾸민 메일로 한국을 겨냥해 Vidar를 퍼뜨린 사례를 보고했다. 실행 파일에 워드 문서 아이콘을 붙여 문서처럼 보이게 했다.

11월 24일 개인정보보호위원회는 자기 직원을 사칭한 "개인정보 유출 통지" 메일을 조심하라고 알렸다. 최근 대형 유출 사고로 불안한 사람을 노려 '안내서', '보상신청서' 같은 첨부와 링크로 계정을 빼 가고 악성코드를 깐다. 개인정보위는 이런 메일을 보낸 적이 없다고 밝혔다.

## 경로를 한눈에

| 경로 | 미끼 | 국내 사례 |
|---|---|---|
| 불법 프로그램 | 크랙·키젠·무료 정품 인증 | 지역 신문사 홈페이지의 유포 글 |
| 가짜 다운로드 사이트 | 검색 상단·광고의 공식 사이트 흉내 | 가짜 Claude 사이트, 메신저 사칭 사이트 |
| 명령어 붙여넣기 | 가짜 인증 화면·오류 창 | 침해된 온라인 매거진 사이트 |
| 게임 | 모드·치트·공식 런처 침해 | 국내 개발사 게임 PC 런처 |
| 메일 | 이력서·저작권·기관 통지 | 개인정보위 사칭 유출 통지 메일 |

다섯 경로 모두 사용자의 클릭이나 실행이 마지막 단계다. 공격자는 보안 장비를 뚫기보다 사람이 스스로 문을 열게 만드는 데 공을 들인다. 왜 백신이 이를 놓치는지는 5편에서 다룬다.

## 편법을 찾는 사람이 먼저 걸린다

사례를 모아 보면 미끼의 상당수가 불법 프로그램, 정품 인증 우회, 게임 핵·치트처럼 규칙을 피해 가려는 수요를 노린다. 카스퍼스키도 감염이 출처를 알 수 없는 곳에서 프로그램을 받거나 정품 인증을 불법으로 시도한 경우와 이어진다고 분석했다. 남의 프로그램을 공짜로 쓰거나 게임에서 남보다 앞서려던 사람이 자기 계정을 통째로 내주게 되는 셈이다.

공격자에게 이 수요는 가장 쉬운 통로다. 이런 파일은 원래 출처를 확인할 수 없고, 백신이 막으면 잠시 꺼 두라는 안내가 붙어 있어도 이상하게 여기지 않는다. 경고를 넘기고 실행하는 일을 사용자가 대신 해 준다.

## 예방과 대응

프로그램은 공식 사이트 주소를 직접 입력해 받고, 검색 결과 맨 위의 광고 링크는 한 번 더 확인한다. 웹페이지가 명령어를 붙여넣어 실행하라고 하면 그 자체가 공격 신호다. 기관이 보낸 메일이라도 첨부를 열기 전에 해당 기관 홈페이지에서 공지를 확인한다.

## 출처

- Kaspersky, 인포스틸러 로그 분석 보도자료(2026-06): https://kaspersky.com/about/press-releases/kaspersky-35-of-infostealer-infections-begin-with-users-running-files-directly-from-temporary-folders
- AhnLab ASEC, 2025년 12월 인포스틸러 동향: https://asec.ahnlab.com/en/92142/
- AhnLab ASEC, 2026년 5월 인포스틸러 동향: https://asec.ahnlab.com/ko/94171/
- AhnLab, 가짜 Claude 다운로드 사이트: https://www.ahnlab.com/en/contents/content-center/36141
- AhnLab ASEC, 카카오톡 PC버전 사칭 사이트: https://asec.ahnlab.com/ko/92971/
- AhnLab, 가짜 CAPTCHA와 ClickFix: https://www.ahnlab.com/en/contents/content-center/36184
- BleepingComputer, 마인크래프트 가짜 모드 감염: https://bleepingcomputer.com/news/security/over-116-000-minecraft-systems-infected-in-weedhack-malware-campaign
- 게임메카, PC 런처 침해: https://www.gamemeca.com/view.php?gid=1782993
- 데일리시큐, 개인정보위 직원 사칭 메일: https://www.dailysecu.com/news/articleView.html?idxno=209719
- AhnLab ASEC, 피싱 메일로 퍼지는 Vidar: https://asec.ahnlab.com/ko/94363/

---
이 글은 공개 자료와 CyberTissue 조사를 사람이 정리한 연재입니다. 조사 방법과 도구: https://github.com/jsik22/cybertissue
