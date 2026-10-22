# 바람 배달부 — 프레스 킷

> 정리일: 2026-10-22 · 상태: 초안 — 둘 곳 확정(itch.io), 로고는 사용자 제작 중
> 기자·유튜버·스트리머가 메일 한 통 없이 기사를 쓰거나 영상을 만들 수 있게, 사실·글·이미지·영상을 한 묶음으로 둔다. [프로젝트 점검](ZR.프로젝트_점검.md) 5장 "프레스 킷·커뮤니티 채널"의 앞쪽 절반. 글은 [스토어 페이지 초안](ZR.스토어_페이지_초안.md)을 그대로 쓰고, 여기서는 고르고 묶는 것만 한다.

[문서 목차](../../README.md) · [Steam 출시 준비](ZR.스팀_출시_준비.md) · [Epic 홍보 전략](ZR.에픽_홍보_전략.md) · [유튜브 홍보 운영안](유튜브_홍보_운영안.md)

## 1. 어디에 둘까

**결정(사용자 2026-10-22): A — itch.io 데모 페이지에 zip으로 올린다.**


| 안 | 방법 | 장점 | 단점 |
|---|---|---|---|
| **A. itch.io 데모 페이지 아래 링크 + zip (추천)** | 이미 있는 데모 페이지 설명 끝에 "Press kit" 링크. zip은 같은 페이지의 다운로드 파일(무료, 이름 `WindCourier_PressKit.zip`)로 올린다 | 새 사이트가 필요 없다. 데모와 한 곳에 있다 | 페이지 꾸밈이 itch.io 틀 안이다 |
| B. 공개 클라우드 폴더(Google Drive 등) | 폴더 하나에 같은 파일, 보기 링크 공개 | 가장 빠르다 | 오래가면 링크가 깨지기 쉽다. 폴더 소유 계정이 드러난다 |
| C. presskit() 형식의 정적 페이지 | GitHub Pages 등 공개 페이지 | 업계에서 익숙한 형식 | 만들 것이 가장 많다. 이 저장소는 비공개 유지라 별도 저장소가 필요하다(점검 7장) |

Steam 페이지를 열면 긴 소개 맨 아래와 토론 게시판 고정 글(스토어 초안 12-5)에 같은 링크를 건다.

## 2. 한눈에 보기 (Fact sheet)

| 항목 | 한국어 | English |
|---|---|---|
| 제목 | 바람 배달부 | Wind Courier |
| 개발·배급 | ozs | ozs |
| 개발 지역 | 대한민국 | South Korea |
| 출시일 | 미정(Coming Soon) | TBA |
| 플랫폼 | Windows (Steam, Epic Games Store) | Windows (Steam, Epic Games Store) |
| 가격 | $7.99 | $7.99 |
| 무료 데모 | itch.io, 2일차까지 — 세이브가 정식판으로 이어짐 | itch.io, days 1–2 — save carries over |
| 장르 | 비행 어드벤처 | Flying adventure |
| 플레이 시간 | 엔딩까지 약 1시간 30분 | About 1.5 hours to the ending |
| 언어 | 한국어·영어, 독·프·이·스(AI 번역) | English, Korean; DE/FR/IT/ES (AI translated) |
| 조작 | 키보드, 게임패드 전체 지원 | Keyboard, full gamepad support |
| 엔진 | Unreal Engine 5 | Unreal Engine 5 |
| 연락처 | info@ozs.co.kr | info@ozs.co.kr |
| 링크 | Steam(공개 뒤), itch.io 데모 | Steam (once live), itch.io demo |

- **채울 것(사용자)**: 개발자 소개 두세 줄 — 몇 명이 만들었는지, 회사·개인 여부. 문서에 근거가 없어 비워 둔다. AI 사용은 소개에 따로 쓰지 않고 4장의 고지 문안을 그대로 붙인다(스토어·itch.io와 같은 문장이어야 한다).

## 3. 소개 글

- 한 줄: 스토어 초안 1장(한·영)의 짧은 소개.
- 긴 소개·기능 목록: 스토어 초안 2장(한·영). 기사에서 그대로 인용해도 된다고 적어 둔다.
- 다른 언어: 2-1장(원어민 검수 전이라 프레스 킷에는 넣지 않는다 — 한·영만).

## 4. 고지·크레딧 (그대로 붙인다)

- AI 사용 고지: [출시 준비 R9](ZR.릴리스타입_출시준비.md) 문안(한·영). itch.io 데모 페이지(스토어 초안 11장)의 문장과 같다.
- Meshy 표기: "Character 3D models generated with Meshy (meshy.ai), used under CC BY 4.0. Modified: refined in Blender and rigged for animation."
- 엔진·글꼴·소리: 게임 크레딧과 같은 줄(출시 준비 R10).
- 이용 안내(영상 제작자용): "You are free to stream and make videos of Wind Courier, including monetized ones." — **사용자 확인 필요**. 음악이 모두 CC0라 저작권 신고 걱정은 없다(`output/video/music/SOURCES.md`, `SourceArt/Audio/SOURCES.md`).

## 5. 이미지·영상

파일은 `output/` 아래에 이미 있다. zip에는 아래만 골라 넣고 이름을 알기 쉽게 바꾼다.

| zip 안 이름 | 원본 | 크기 | 비고 |
|---|---|---|---|
| `logo/WindCourier_logo.png` | **사용자가 새로 만드는 통일 로고 한 장**(2026-10-22) | 투명 배경 | 지금 로고 세 파일(`library_logo.png` 546×260, `epic_logo_960x540.png`, `library_logo_1280x720.png`, 모두 RGBA)은 같은 그림이지만 조금씩 달라 통일한다. 새 로고가 나오면 스토어 이미지 쪽 세 파일도 이것으로 다시 만들지 정한다 |
| `keyart/keyart_landscape.png` | `store_images/epic_landscape_2560x1440.png` | 2560×1440 | 로고 있음 |
| `keyart/keyart_landscape_nologo.png` | `store_images/epic_landscape_nologo_2560x1440.png` | 2560×1440 | 기사 머리 이미지용 |
| `keyart/keyart_portrait.png` | `store_images/epic_portrait_1200x1600.png` | 1200×1600 | 로고 없는 판도 있음 |
| `screenshots/01_coast.png` … | `store_screenshots/release_1.0.0/en/` 12장 | 1920×1080 | **02·04는 다시 찍은 뒤 넣는다**(개발 빌드 글자, 점검 7장 1번). UI가 없는 장면이 대부분이라 영어판 하나로 충분하다. 한국 매체용으로 `ko/` 02·04·06·07을 더한다 |
| `video/trailer.mp4` | `video/trailer_steam.mp4` | 1920×1080, 49초 | `trailer_steam_goldtape.mp4`, `trailer_steam_oldsmoke.mp4`는 넣지 않는다(옛 소리·연기) |

- 없는 것: 움직이는 GIF(소셜·기사용 짧은 반복 영상)와 개발 중 사진. GIF는 트레일러 클립(`output/video/trailer_clips/`)에서 3~5초 잘라 만들 수 있다 — 필요하면 요청.
- zip 맨 위에 `README.txt`(2장 표 + 3장 한 줄 소개 + 4장 고지·이용 안내 + 연락처)를 둔다.

## 6. 남은 일

- [x] 1장 둘 곳 정하기 — itch.io(2026-10-22)
- [ ] 통일 로고(사용자 제작) 받기
- [ ] 2장 개발자 소개, 4장 영상 이용 안내 문구 확인
- [ ] 스크린샷 02·04 다시 찍기(Steam 출시 준비 2장 ②)를 마친 뒤 zip 묶기
- [ ] Steam 페이지 공개 뒤 링크 채우기
