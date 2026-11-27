---
layout: post
title: "정보 탈취 목적의 멀웨어, 인포스틸러"
date: 2026-11-25
tags: [infostealer, stealer-logs, korea, account-takeover]
description: "인포스틸러는 사이트가 아니라 이용자 PC에서 계정 정보를 통째로 가져가는 악성코드다. 한국에서는 올해 들어 6천 대 가까운 감염 기기가 새로 확인됐고, 국내 주요 사이트 대부분의 계정이 그 기록에 나온다."
sources:
  - {name: "Hudson Rock — Infostealers Weekly Report 2026-07-20~07-27", url: "https://www.infostealers.com/report/infostealers-weekly-report-2026-07-20-2026-07-27/"}
  - {name: "AhnLab ASEC — 2026년 10월 인포스틸러 동향 보고서", url: "https://asec.ahnlab.com/ko/96697/"}
  - {name: "AhnLab ASEC — 2026년 8월 인포스틸러 동향 보고서", url: "https://asec.ahnlab.com/ko/95518/"}
  - {name: "Kaspersky — 35% of infostealer infections begin with users running files directly from temporary folders", url: "https://kaspersky.com/about/press-releases/kaspersky-35-of-infostealer-infections-begin-with-users-running-files-directly-from-temporary-folders"}
  - {name: "위키트리 — 앤트로픽, 클로드 이용자에 계정 침해 경고", url: "https://www.wikitree.co.kr/articles/1156318"}
kind: series
series: "인포스틸러 생태계"
part: 1
excerpt: "인포스틸러는 사이트가 아니라 이용자 PC에서 계정 정보를 통째로 가져가는 악성코드다. 한국에서는 올해 들어 6천 대 가까운 감염 기기가 새로 확인됐고, 국내 주요 사이트 대부분의 계정이 그 기록에 나온다."
updated: 2026-11-25 09:52:15 +0900
---

![밤의 책상 위 노트북에서 열쇠, 쿠키, 카드, 지갑이 빨간 선을 타고 빠져나가 봉인된 상자 하나로 모여 날아간다](https://jsik22.github.io/cybertissue-notes/assets/2026-11-25-infostealer-kr-1/1-hero.png)

## 인포스틸러는 무엇인가

인포스틸러(infostealer)는 정보 탈취를 목적으로 만든 악성코드다. PC에 들어와 몇 초 만에 계정 정보를 모아 공격자에게 보내고 사라진다. 사이트를 해킹하는 것이 아니라 그 사이트를 쓰는 사람의 PC를 노린다. 사이트 보안이 아무리 튼튼해도, 이용자 PC에 저장된 로그인 정보가 빠져나가면 공격자는 정상 이용자처럼 들어간다.

1편은 인포스틸러가 무엇이고 한국에서 얼마나 퍼져 있는지를 개괄한다.

## 무엇을 가져가나

브라우저에 저장된 비밀번호, 로그인 상태를 유지하는 쿠키, 자동 완성 정보와 카드, 암호화폐 지갑, 메신저 세션이 대상이다. 훔친 정보는 PC 한 대 단위로 묶이고, 이 묶음을 '로그'라고 부르며 따로 사고판다.

![칸이 나뉜 상자에 열쇠, 쿠키, 브라우저 창, 카드, 지갑, 말풍선이 담겨 있고 빨간 가격표가 걸려 있다. PC 한 대의 계정 정보가 '로그' 한 묶음으로 팔리는 모습](https://jsik22.github.io/cybertissue-notes/assets/2026-11-25-infostealer-kr-1/2-log-box.png)

어디에 저장된 무엇을 가져가는지는 3편, 어디로 팔려 가는지는 4편에서 다룬다.

## 한국의 감염 규모

인포스틸러 로그를 모아 색인하는 Hudson Rock은 매주 새로 색인한 감염 기기를 나라별로 공개한다. 2026년 45주 동안 한국에서 5,975대가 나왔다. 같은 기간 전 세계 약 92만 대의 0.6%다. 한 주 중앙값은 50대였지만, 7월 20~27일 주에는 2,071대로 전 세계 3위까지 올랐다. 하반기 중앙값은 105대로 상반기(36대)의 약 3배다.

![한국 주간 감염 기기 수 막대그래프(2026년 45주). 7월 20~27일 주가 2,071대로 가장 높고, 상반기 중앙값 36대, 하반기 중앙값 105대](https://jsik22.github.io/cybertissue-notes/assets/2026-11-25-infostealer-kr-1/3-weekly.svg)

- **불확실**<br>새로 '색인한' 수라서 감염된 날과 다르다. 큰 로그 묶음 하나가 한꺼번에 색인되면 그 주만 크게 튄다.

## 감염 PC에서 나온 국내 사이트 계정

PC 한 대가 감염되면 그 사람이 쓰던 사이트의 계정이 한꺼번에 나간다. 그래서 이용자가 많은 사이트일수록 감염 기록에 자주 나온다. 국내 사이트 116곳(Tranco .kr 상위 100곳과 국내 대형 서비스 16곳)을 Hudson Rock 무료 조회로 찾아보니, 조회가 끝난 113곳 중 97곳의 로그인 정보가 감염 PC의 기록에서 나왔다.

| 묶음 | 조회 | 감염 기록 | 이용자 감염 기기(중앙값) | 구성원 기록 |
|---|---|---|---|---|
| 대형 서비스 | 13 | 13 | 25,599대 | 10 |
| 기업 | 67 | 57 | 848대 | 15 |
| 교육 | 12 | 12 | 1,120대 | 12 |
| 공공·연구·기타 | 21 | 15 | 137대 | 4 |

- **관찰**<br>이용자 감염 기기는 그 사이트의 로그인 정보가 든 감염 PC 수이고, 중앙값은 기록이 있는 곳끼리 냈다. 구성원 기록은 그 기관 메일 주소로 로그인한 정보가 나온 곳의 수다.
- **관찰**<br>대형 서비스 13곳 중 10곳은 이용자 감염일이 11월로 찍혀 있다. 감염은 지난 일이 아니라 지금도 이어지고 있다.
- **추론**<br>사이트가 뚫린 것이 아니다. 기기 수는 그 사이트를 쓰는 사람이 얼마나 많은지를 따라간다.
- **추론**<br>대학 12곳 모두에 구성원 기록이 있다. 학교 메일 주소를 쓰는 학생과 교직원의 PC다.
- **불확실**<br>위의 한국 감염 수치와 기준이 다르다. 사이트별 수는 그 사이트를 쓰는 모든 이용자(해외 포함)의 누적 기록이고, PC 한 대가 여러 사이트에 걸치므로 더하면 사람 수가 되지 않는다. 무료 조회에 나오는 범위라 실제보다 적을 수 있다.

## 국내에서 퍼지는 방식

안랩 ASEC의 월간 동향을 보면 크랙·키젠 같은 불법 프로그램으로 위장해 검색 결과에 걸어 두는 방식이 매달 이어졌다. 주로 퍼진 계열도 바뀌었다. 4월에 처음 나온 Remus가 8월에는 전체 1위가 됐고, 9월에는 Lzveil이 새로 나왔다.

카스퍼스키가 2025년 다크웹 로그 500만 개를 분석한 결과, 감염의 35%는 내려받은 파일을 임시 폴더에서 바로 실행하며 시작됐다. 감염 경로는 2편에서 자세히 본다.

## 피해가 체감되는 순간

![자물쇠와 키패드가 잠긴 그대로인 문이 열려 있다. 옆에서 한 사람이 복사한 빨간 출입증을 카드 리더기에 대자 체크 표시가 켜지고, 화살표가 문으로 이어진다](https://jsik22.github.io/cybertissue-notes/assets/2026-11-25-infostealer-kr-1/4-session.png)

8월 말 앤트로픽은 일부 Claude 이용자에게 계정 침해를 알렸다. 인포스틸러가 PC에서 로그인 세션을 훔쳤고, 공격자는 그 세션으로 들어가 유료 사용량을 써 버렸다. 비밀번호를 몰라도, 2단계 인증 코드가 없어도 됐다. 로그인 상태 자체를 가져갔기 때문이다.

## 예방과 대응

크랙과 출처가 불분명한 설치 파일을 받지 않는다. 감염이 의심되면 PC부터 정리한다. 그다음 다른 기기에서 비밀번호를 바꾸고, 모든 기기에서 로그아웃해 훔쳐 간 로그인 상태를 끊는다.

## 출처

- Hudson Rock, Infostealers Weekly Report 2026-07-20~07-27: https://www.infostealers.com/report/infostealers-weekly-report-2026-07-20-2026-07-27/
- AhnLab ASEC, 2026년 10월 인포스틸러 동향 보고서: https://asec.ahnlab.com/ko/96697/
- AhnLab ASEC, 2026년 8월 인포스틸러 동향 보고서: https://asec.ahnlab.com/ko/95518/
- Kaspersky, 인포스틸러 로그 분석 보도자료(2026-06): https://kaspersky.com/about/press-releases/kaspersky-35-of-infostealer-infections-begin-with-users-running-files-directly-from-temporary-folders
- 위키트리, 앤트로픽의 Claude 계정 침해 경고 보도: https://www.wikitree.co.kr/articles/1156318
- 국내 사이트 표본 조회: Hudson Rock 무료 조회(2026-11-24), 순위 Tranco(2026-11-23)

---
이 글은 공개 자료와 CTiSSUE 조사를 사람이 정리한 연재입니다. 조사 방법과 도구: https://github.com/jsik22/cybertissue
