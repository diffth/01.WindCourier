# 바람 배달부 — Steam 출시 준비

> 정리일: 2026-10-07 · 2장 순서표 수정 2026-10-22(2026-10-11 결정 반영) · 상태: 진행 중
> itch.io에는 2026-10-11 알파를 내리고 2일차까지의 무료 데모를 올렸다(사용자, [출시 준비](ZR.릴리스타입_출시준비.md) "itch.io 데모"). 데모 세이브는 정식판으로 이어진다. 이 문서는 **Steam을 먼저** 준비하는 순서와 할 일이다. 글·이미지·사양은 [스토어 페이지 초안](ZR.스토어_페이지_초안.md), 빌드 품질은 [출시 준비(ZR)](ZR.릴리스타입_출시준비.md)에 있다.

## 1. 지금 상태 (2026-10-07)

- Steamworks 파트너 가입 끝. 이 게임의 Steam Direct 신청(등록비 $100)을 냈고 **Valve 심사 중**(사용자). 심사가 끝나면 AppID와 기본 Depot이 생긴다.
- 빌드: `D:/AI/ozs/Game/WindCourier_1.0.0_Shipping/Windows`(**1.0.0 후보**, 2026-10-08 — 연기 모양·아침/귀환/엔딩 음악 포함, 1.9 GB, 6개 언어, 누락 에셋 0, 기호 파일 246 MB는 `Symbols/`). 이전 0.9.3은 `WindCourier_0.9.3_Shipping`.
- 스토어 준비물: 소개 글 6개 언어, 이미지 7종, 스크린샷 6개 언어, 사양 추정안(사용자 승인), 연령 설문 답 초안, AI 고지 문안 — 모두 [스토어 페이지 초안](ZR.스토어_페이지_초안.md).

## 2. 순서와 기간

Steam 규칙(아래 출처): 처음 내는 개발자는 **등록비를 낸 뒤 30일**이 지나야 출시할 수 있고, 새 게임은 **"출시 예정(Coming Soon)" 페이지를 2주 이상** 공개해야 한다. 스토어 페이지 심사는 보통 영업일 3~5일, 빌드 심사도 따로 있다.

**2026-10-11 결정(사용자, [프로젝트 점검](ZR.프로젝트_점검.md) 3장)으로 순서를 바꿨다.** 당장 출시하지 않고 Coming Soon 페이지를 먼저 열어 찜을 모은다. 페이지에 출시일을 못 박지 않고, 출시 시점은 목표 찜 수와 출시 판단 날짜(아직 안 정함)로 정한다. 2주는 Steam 최소 요건일 뿐 목표 기간이 아니다. 사람 검증과 외부 피드백은 Coming Soon 기간에 하고, 반영한 빌드로 출시한다.

| 단계 | 누가 | 할 일 | 기간 |
|---|---|---|---|
| ① 앱 생성 | Valve | 신청 심사 → AppID 발급 | 진행 중 |
| ② 스크린샷 다시 찍기 | 사용자 | 6개 언어의 02(상승기류)·04(투하 가능)를 다시 찍는다. 지금 `release_1.0.0` 사진에는 개발 빌드 전용 "F1: diagnostics OFF"가 보인다. `store_screenshots.ps1`이 `-FlightReleaseHUD`를 붙이므로 빌드 뒤 다시 돌리면 된다(프로젝트 점검 7장 1번) | ③ 전 |
| ③ 스토어 페이지 작성 | 사용자(문안은 준비됨) | 3장 표대로 Steamworks에 입력. 출시일은 날짜 없이 "Coming soon"으로 둔다 | 하루 |
| ④ 스토어 페이지 심사 | Valve | "검토 준비 완료" 표시 뒤 심사 | 영업일 3~5일, 원하는 공개일 7일 전에 제출 |
| ⑤ Coming Soon 공개 | 사용자 | 심사 통과 뒤 공개 → 찜(위시리스트) 모으기. 다음 Next Fest 등록 일정을 확인한다 | 출시 판단 날짜까지(최소 2주) |
| ⑥ **한국 등급분류 신청** | 사용자 | GCRB(gcrb.or.kr)에 1.0.0으로 **지금 신청**(사용자 결정 2026-10-11, itch.io 데모는 공개 유지). 신청할 때 데모를 본편 등급으로 함께 다룰 수 있는지 묻는다. 자료는 [등급분류 신청 자료](ZR.등급분류_신청자료.md)(내용 설명서·동영상 구성안). 간소화 심의(전체·12·15세)는 11월부터 2일 이내 | 진행 중(2026-10-14 게임제작업 등록 단계) |
| ⑦ 사람 확인 | 사용자 | 키보드로 되는 [사람 확인 목록](ZR.사람확인_목록.md) B항(설정 한 바퀴)과 소리 23개 듣기를 먼저 한다. 게임패드가 오면 Shipping 1.0.0으로 한국어·패드 완주(A·C항) | ⑪ 전 |
| ⑧ 외부 피드백 | 사용자 | 보류를 풀고 3~5명에게 빌드를 건넨다. [프로토타입 검증](P.프로토타입검증.md) 3장 질문으로 받는다 | Coming Soon 기간 중 |
| ⑨ Steam 데모(선택) | 사용자 | Next Fest에 나가면 itch.io 데모(2일차까지)를 Steam 별도 데모 앱으로 올린다 | Next Fest 등록 마감 전 |
| ⑩ 출시 판단 | 사용자 | 판단 날짜에 찜 수를 목표와 비교해 출시일을 정한다 | 판단 날짜 |
| ⑪ 출시 빌드 확정·업로드 | 사용자(스크립트 준비됨) | ⑦·⑧을 반영한 1.0.x Shipping으로 내용 동결 → 4장대로 beta 브랜치에 올림. 화면이 바뀌었으면 6장 표(스크린샷·트레일러·등급 영상)를 다시 만든다 | 출시 5주 전 |
| ⑫ 빌드 심사 | Valve | 실행 확인 | 며칠 |
| ⑬ 출시 | 사용자 | 등록비 낸 날부터 30일 이후, **한국 등급이 나온 뒤** 출시 버튼. 출시 주 10 % 할인은 그 전에 등록 | ⑩에서 정한 날 |

- ②~④는 AppID가 나오면 바로 한다. ⑥~⑧은 AppID와 상관없이 지금 할 수 있다.
- 출시일을 정할 때 기준: 판단 날짜 + 5주(⑪~⑫ 여유)와 등록비 30일 조건 중 늦은 쪽. Next Fest에 나가면 그 행사가 끝난 뒤로 잡는다.
- 등급은 1.0.0으로 받는다. ⑦·⑧로 고친 1.0.x가 등급 신청 내용(설명서·영상)과 달라지면 변경 신고가 필요한지 GCRB에 확인한다. 등급에 닿는 변경(폭력·언어 등)은 하지 않는다.
- 등급은 판매(배포) 시작 전에만 있으면 된다 — Coming Soon 공개에는 필요 없다. 무료 데모·플레이테스트도 한국에 배포하면 등급 대상이라(수수료 면제는 비영리 게임만, 등급 자체는 면제 아님) Steam 데모·외부 피드백 빌드도 등급 결정 뒤에 공개 배포한다. 비공개로 몇 명에게 건네는 것은 배포로 보는지 신청 때 함께 묻는다.

## 3. Steamworks에 넣을 것

| Steamworks 항목 | 값 / 출처 |
|---|---|
| 게임 이름 | Wind Courier (한국어 이름 바람 배달부는 스토어 한국어 설명에) |
| 개발자 / 퍼블리셔 | ozs / ozs |
| 짧은 설명·긴 설명 | 스토어 초안 1·2장(한·영), 2-1장(독·프·이·스) |
| 지원 언어 | 인터페이스·자막: 영어, 한국어, 독일어, 프랑스어, 이탈리아어, 스페인어(스페인). 음성 없음 |
| 장르·태그 | 스토어 초안 3장 |
| 시스템 사양 | 스토어 초안 4장(Windows만) |
| 캡슐·배경 이미지 | `output/store_images/`(스토어 초안 6장) |
| 스크린샷 | `output/store_screenshots/beta_2026-10-07*/`(언어별, 스토어 초안 7장) |
| 트레일러 | **D판으로 결정(사용자 2026-10-07)** → `output/video/trailer_steam.mp4`(49초, 1920×1080 H.264 + AAC). 음악 "Minimalistic Flute & Strings Tune"(Spring Spring, CC0)을 북이 들어오는 40초 지점부터 1.1배 빠르게, 장면 13컷 + 제목 키아트, 게임 소리(엔진·바람·들판)를 낮게 깖. 다른 판 A~C와 클립은 `output/video/`, 음악 출처 `output/video/music/SOURCES.md`. 2026-10-07 고침: 연기 모양을 바꾼 뒤 다시 녹화, 3막 날개 위 황조롱이 장면을 조종석 카메라(`-FlightPilotShot`)로 더함(그 비행 37~47초). 1.0 빌드로 다시 만들기: `WindCourier/Scripts/video_trailer_clips.ps1`(장면 녹화, 약 40분) → `python WindCourier/Scripts/video_trailer.py`(기본값이 D판) |
| AI 생성 콘텐츠 공개 | [출시 준비 R9](ZR.릴리스타입_출시준비.md) 문안(키아트 포함) |
| 콘텐츠 설문 | 스토어 초안 9장 |
| 컨트롤러 | 게임패드 완전 지원(Xbox 배치) |
| 가격 | **$7.99**, 지역 가격은 Steam 권장값, 출시 주 10 % 할인(사용자 결정 2026-10-07, [스토어 초안 8-1장](ZR.스토어_페이지_초안.md)) |
| 클라우드 | Auto-Cloud: 루트 `WinAppDataLocal`, 하위 경로 `WindCourier/Saved/SaveGames`, 파일 `FlightDay.sav`·`FlightDay.sav.bak`(스토어 초안 10장, 결정 A) |
| 실행 옵션 | 실행 파일 `WindCourier.exe`, 운영체제 Windows, 64비트 |
| 재배포 패키지 | **Visual C++ 2015-2022 x64** 체크 — 패키지에 UE 사전 요구 설치 파일이 들어 있지 않아(`-prereqs` 없이 빌드), 깨끗한 PC에서는 이것이 없으면 실행되지 않는다 |

## 4. 빌드 올리기 (SteamPipe)

스크립트: `WindCourier/Scripts/steam_upload.ps1`(2026-10-07). Steamworks SDK의 `steamcmd.exe`가 필요하다(SDK를 받아 `C:/SteamworksSDK`에 풀거나 `-SteamCmd`로 경로를 준다).

```powershell
# 미리보기: 올릴 파일만 세고 아무것도 보내지 않는다
powershell -ExecutionPolicy Bypass -File WindCourier/Scripts/steam_upload.ps1 `
    -AppId <AppID> -DepotId <DepotID> -SteamUser <빌드용 계정> `
    -BuildDir D:/AI/ozs/Game/WindCourier_1.0.0_Shipping/Windows -Preview
# 실제 업로드(+ 비공개 beta 브랜치에 바로 걸기)
... -SetLive beta
```

- Shipping 패키지가 아니면 거부한다(Development는 콘솔·확인용 옵션이 살아 있다). `.pdb`는 올리지 않는다.
- `default` 브랜치는 스크립트로 걸지 않는다 — 출시는 Steamworks에서 직접.
- VDF와 steamcmd 기록은 `WindCourier/Saved/SteamPipe/`(git 제외 폴더).
- 확인(2026-10-07): 미리보기로 0.9.3 Shipping 34개 파일·1,883 MB, Development 패키지는 거부됨. 실제 업로드는 AppID가 없어 아직.

## 5. 출시 전에 할 일

- [ ] **한국 등급분류** — 한국에서 파는 게임은 플랫폼과 상관없이 등급이 있어야 하고, Steam에 올렸다고 면제되지 않는다(등급 없는 Steam 게임이 한국에서 막힌 일이 있다). Steam 콘텐츠 설문은 한국 등급을 대신하지 않는다. 조사(2026-10-07, 기사·기관 안내 기준 — **신청 전 공식 사이트에서 다시 확인**):
  - **어디서**: PC 게임 등급분류는 단계적으로 게임물관리위원회(GRAC)에서 민간 기구 **게임콘텐츠등급분류위원회(GCRB, gcrb.or.kr)**로 넘어가는 중이다. 2025년 11월 1단계로 개인사업자까지 GCRB가 받고, **2026년 11월 2단계**(10월 예정에서 한 달 미뤄짐)부터 국내외 개인까지 넓히고 간소화 심의를 들인다. 사업자 등록이 없는 개인이면 11월 이후 GCRB, 그 전이면 GRAC에 문의.
  - **기간**: 위원회 심의 약 7일, 간소화(전체·12·15세만) 2일 이내.
  - **비용**: PC 게임 기초가액 36만 원 × 이용형태·장르·한글화 계수. 개인·인디 50 % 할인 안내가 있다. 수수료 면제는 사업자 등록 없는 개인의 **비영리** 게임만이라 유료 판매에는 해당하지 않는다. GCRB 사이트에 수수료 계산기가 있다.
  - **예상 등급**: 전체이용가(스토어 초안 9장) → 간소화 대상.
  - **사업자 등록 있음(사용자, 2026-10-07)** → 2단계를 기다릴 필요 없이 GCRB에 신청할 수 있다. 시점은 2장 표 ⑥(2026-10-11 결정으로 지금, 1.0.0으로), 자료는 [등급분류 신청 자료](ZR.등급분류_신청자료.md). 신청 전에 공식 사이트에서 수수료·제출 자료를 다시 확인.
- [x] 가격: $7.99, 출시 주 10 % 할인(2026-10-07). 출시 할인은 Steamworks에 출시 전에 등록.
- [x] 버전: 출시 빌드는 1.0.0으로(출시 준비 R4 결정: 베타 0.9.x, 출시 1.0.0) — 2026-10-08 올리고 Shipping 다시 빌드(`WindCourier_1.0.0_Shipping`). 이 뒤로 게임을 고치면 1.0.x로 올린다.
- [x] README: Release README(`SourceArt/Release/README_읽어주세요.txt`, Shipping 패키지에 들어감)의 의견 창구를 itch.io 댓글에서 **Steam 토론 게시판**으로 바꿨다(2026-10-07, 한·영, 0.9.3 Shipping 패키지에도 복사). 2026-10-08 1.0.0으로 올리며 "베타 버전입니다" 문단을 "의견과 오류"로 바꾸고(문구가 바뀔 수 있다는 줄은 뺌), 실행 안내를 "압축을 풀고"에서 "Steam 라이브러리의 '플레이' 또는 WindCourier.exe"로 고쳤다. itch.io 데모(2026-10-11)는 Shipping이고 자기 안내문(`SourceArt/Demo`, 의견은 itch.io 댓글로)을 쓴다.
- [ ] Shipping으로 처음부터 엔딩까지 한 판(사람 확인 목록 R5) — 출시 빌드로.
- [ ] Steam 클라이언트로 설치·실행 한 번: 비공개 브랜치에서 받아 실행, Auto-Cloud가 세이브를 올리고 다른 PC(또는 지운 뒤)에서 내려받는지(스토어 초안 10장 확인 항목).
- [ ] 저작권 표기·크레딧: Steam 페이지 법적 고지란에 Unreal Engine 문구(크레딧과 같은 문장), 글꼴 OFL은 패키지 `Licenses/`에 이미 있음.

## 6. 1.0 빌드 때 다시 만들 것 (2026-10-07)

1.0 후보 빌드(2026-10-08)가 나오면 한 번에 다시 만든다. 출시 빌드 확정(2장 ⑪) 때 화면이 바뀌었으면 다시 한 번 만든다. 지금 것들은 0.9.3 기준이고, 일부는 연기 모양을 바꾸기 전(2026-10-07) 화면이다.

| 무엇 | 어떻게 | 지금 상태 |
|---|---|---|
| Steam 패키지 | `package_build.ps1 -Config Shipping` (버전 1.0.0) | **1.0.0 끝(2026-10-08)** — `WindCourier_1.0.0_Shipping` |
| 등급 신청 영상 | `video_rating_clips.ps1` | **1.0.0 끝(2026-10-08)** — `output/video/rating_ko/rating_ko_full.mp4` 3분 31초 |
| 스토어 스크린샷 6개 언어 | `store_screenshots.ps1`(언어별 4장 + 영어 12장) | **1.0.0 끝(2026-10-08)** — `output/store_screenshots/release_1.0.0/` |
| 트레일러 | `video_trailer_clips.ps1` → `video_trailer.py` | 연기 바꾼 뒤(2026-10-07). **2026-10-08 새 엔진 소리로 다시 합침**(`video_trailer.py`만 — 엔진·바람·들판 루프를 `SourceArt/Audio`에서 섞으므로 영상은 그대로, 49.4초). 예전 GOLD TAPE 엔진판은 `trailer_steam_goldtape.mp4` — 올리지 말 것. 같은 날 해안 나무·고개 눈보라를 바꾼 뒤 `t03_coast`·`t07_pass_eve` 두 클립만 다시 녹화해 다시 합쳤다(이전 클립·트레일러는 `output/video/trailer_clips_before_1008/`) |

## 7. Steam Deck (2026-10-22 초안)

게임패드를 완전히 지원하므로 Deck 사용자가 시도할 가능성이 높다. Deck 호환성 등급(검증됨·플레이 가능·지원 안 됨)은 개발자가 스스로 매기지 않고 Valve가 직접 검사해 매긴다(Steamworks의 Steam Deck 호환성 검토 — 신청 시점·기준 원문은 포털 확인). 아래는 코드만 읽고 내린 판단이고, Deck 실기나 1280×800 캡처로 확인한 것은 없다.

### 코드로 확인한 것

| Valve가 보는 것 | 이 게임 | 판단 |
|---|---|---|
| 입력: 모든 동작을 패드로 | 비행·카운터·서랍·메뉴·편지·크레딧 모두 패드 버튼이 있다. 키보드에만 있는 조작은 개발용 F1·F2뿐이고 Shipping에서 빠진다. `N`·마우스 왼쪽은 입력 장치 감지에만 쓴다 | 문제 없음 |
| 입력: 화면의 버튼 표기 | Xbox 이름(A/B/X/Y, LB/RB, LT/RT, View/Menu). Deck은 A/B/X/Y가 같고 L1/R1, L2/R2, ⧉/☰로 적혀 있다 | Xbox 표기를 허용하는지 포털 확인. 안 되면 "플레이 가능" 쪽 |
| 입력: 글자 입력 | 이름 입력 등 글자를 치는 곳이 없다 | 화상 키보드 필요 없음 |
| 화면: 1280×800 | 해상도 목록은 1280×720 이상을 모두 넣으므로 1280×800이 들어간다. UI 배율 `max(1, min(1280/1600, 800/900))` = 1 → 1280×720과 같은 배치에 세로 80px 여유 | 캡처 확인 필요(Q1의 5개 해상도에 없음) |
| 화면: 글자 크기 | 기본 글꼴 13px, 가장 작게 그리는 글자는 0.85배(약 11px — 버전 표기 등). 글자 크기 130%는 1280×800에서 111%까지만 커진다(세로 720 제한) | Valve 최소 기준(1280×800에서 9px로 알려짐, 포털 확인)보다 크다 |
| 실행: 런처·안티치트·계정 | 없음. Steam·Epic SDK도 없다(스토어 초안 10장 결정 A) | 문제 없음 |
| 실행: Proton | Windows용 UE 5.8 패키지. Proton에서 돌아가는지는 모른다 | 실기 확인 필요 |
| 성능 | 기본은 Lumen을 쓰는 중간 이상일 수 있다. 첫 실행의 성능 측정이 Deck에서 무엇을 고를지 모른다. 낮음은 Lumen과 가상 그림자를 끈다(Q2) | **가장 큰 위험.** 30fps가 나오는지 모른다 |

### 할 수 있는 것

- [ ] **1280×800 캡처**(Development 빌드, Deck 없이 됨) — Q1과 같은 방식으로 `-RenderOffscreen -windowed -ForceRes -ResX=1280 -ResY=800`. 타이틀·설정·카운터·편지·비행·정산·크레딧. 한국어·영어, 글자 크기 100%·130%.
- [ ] **낮은 사양 흉내** — Deck이 없으면 정확히 재기 어렵다. 그래픽 품질 낮음으로 1280×800 프레임을 재 두면 Valve 검사 결과를 읽을 때 참고가 된다.
- [ ] Deck을 가진 사람이 있으면 외부 피드백(2장 ⑧) 때 Deck 한 판을 부탁한다 — 실행 여부, 첫 실행에 고른 품질, 프레임, 글자가 읽히는지.
- 스토어 페이지에는 Deck 지원을 따로 적지 않는다. 검사 결과가 나오면 Steam이 페이지에 표시한다.

## 출처

- [게임콘텐츠등급분류위원회 등급분류 신청 및 절차](https://www.gcrb.or.kr/Mobile_NEW/sub/classification%20Application.aspx) · [수수료 계산](https://www.gcrb.or.kr/Mobile_NEW/sub/fee%20calculation.aspx)
- [11월 민간분류 2단계 시행 (2026-09-23)](https://m.news.nate.com/view/20260923n21650) · [민간이양 본격화 (2026-05-28)](https://news.nate.com/view/20260528n32188)
- [게임 출시 전, 꼭 알아야 하는 게임 등급분류 (토스)](https://toss.im/apps-in-toss/blog/game_rating_classification)
- [게임위, 스팀 유통 해외 PC게임에 등급분류 안내 (이데일리)](https://edaily.co.kr/news/read?mediaCodeNo=257&newsId=04414886622386256)

- [Release Process (Steamworks)](https://partner.steamgames.com/doc/store/releasing) — 체크리스트, Coming Soon 2주, 심사 순서
- [Coming Soon (Steamworks)](https://partner.steamgames.com/doc/store/coming_soon)
- [Steam Direct](https://partner.steamgames.com/steamdirect) — 등록비 $100, 매출 $1,000 뒤 환급, 첫 출시 30일 대기
- [Content Survey (Steamworks)](https://partner.steamgames.com/doc/gettingstarted/contentsurvey)
- [GRAC Seeks To Punish People Who Distribute Games On Steam Without a Rating](https://blog.musaistudio.com/korean-game-market-grac-seeks-to-punish-people-who-distribute-games-on-steam-without-a-rating)
- [Korean game-related laws and regulations](https://gamebacknd.ghost.io/a-guide-to-entering-the-south-korean-game-market-for-global-developers-series-3-korean-game-related-laws-and-regulations/)
