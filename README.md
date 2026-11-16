# ⚡ 우리 가족 포켓몬고 To Do

가족 셋(아빠 · 엄마 · 아들)이 각자 휴대폰에서 같은 페이지를 열어 **실시간으로 함께 쓰는 할 일 대시보드**입니다.
포켓몬고 분위기의 파스텔 카드와 몬스터볼 체크, 완료 축하 인터랙션이 들어 있습니다.

- 앱 주소: **https://joonstory.github.io/family-dashboard/**
- 데이터: Firebase Realtime Database `/family_board/v1` (기기 간 실시간 동기화)

## 기능

| 기능 | 설명 |
| --- | --- |
| 실시간 동기화 | 상단 배지로 연결 상태 표시(`🟢 실시간 동기화 중`). 한 사람이 체크하면 다른 휴대폰에 바로 반영 |
| 구성원 카드 | `아빠` · `엄마` · `아들` 3개 파스텔 카드 + 인라인 SVG 캐릭터(곰 / 여우 / 피카츄 느낌) |
| 진행률 | 카드마다 `X/Y개 완료` 와 진행 바, 상단에 오늘 전체 완료 현황 |
| 체크 | 몬스터볼 체크 + 취소선. 체크하는 순간 상단 축하 배너(`나이스! ✨ '…' 잡았다!`)와 중앙 `하이파이브! ⚡` 팝업(탭하면 닫힘) |
| 추가 · 수정 · 삭제 | 카드 하단 `+ 할 일 추가`, 항목 우측 ✏️(수정) · ✕(삭제, 한 번 더 확인) |
| 모바일 앱 느낌 | `apple-mobile-web-app-capable`, `theme-color`, 매니페스트, 홈 화면 아이콘, 세이프에어리어 대응 |

## 처음 열었을 때 (기본 데이터)

DB가 비어 있으면(`snapshot.val() === null`) 기본 할 일을 **딱 한 번** 저장합니다.
기본 데이터에는 `meta.initialized` 표시가 함께 저장되므로, 가족이 항목을 모두 지워도 기본 데이터가 다시 채워지지 않습니다.
새로 시작하고 싶을 때는 Firebase 콘솔에서 `/family_board/v1` 노드를 한 번 비우면 됩니다.

- **아들** — 기말고사 대비 국어·과학·사회·도덕 7개 항목(시험일 12/7·12/8·12/9)
- **아빠** — 응원 간식, 공부 분위기 (2개)
- **엄마** — 아침밥·비타민, 시험 일정 확인 (2개)

## 구조

```
index.html              앱 전체 (HTML + CSS + ES 모듈 JS, 빌드 없음)
icon.svg                파비콘
icon-192.png / icon-512.png / apple-touch-icon.png
manifest.webmanifest    홈 화면 추가용
```

Firebase v10 modular SDK를 CDN ESM으로 불러옵니다(`firebase-app.js`, `firebase-database.js` 10.14.1).
DB 구조:

```
family_board/v1
├─ meta/          { initialized, version, seededFor }
└─ members/
   ├─ dad/items/{key}: { text, done, order, updatedAt }
   ├─ mom/items/…
   └─ son/items/…
```

## 운영 메모

- 이 저장소와 앱은 **가족 전용**입니다. URL을 아는 사람은 누구나 열람·수정할 수 있으니 공개 게시판에 올리지 마세요.
- Firebase 보안 규칙은 현재 **테스트 모드(2026-12-16까지 읽기·쓰기 허용)** 입니다. 그 이후에는 콘솔에서 규칙을 갱신해야 데이터가 계속 저장됩니다.
- 실사용이 길어지면 Firebase Authentication(익명 로그인 등)으로 규칙을 좁히는 것을 권장합니다.
