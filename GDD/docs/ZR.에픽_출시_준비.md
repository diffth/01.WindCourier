# 바람 배달부 — Epic Games Store 출시 준비

> 정리일: 2026-10-14 · 상태: 포털 가입 전 준비 끝 (이미지·영상·업로드 스크립트)
> 출시 로드맵은 [개발파이프라인](00.개발파이프라인.md)의 **Steam·Epic 동시 출시**를 따른다. Steam 쪽은 [Steam 출시 준비](ZR.스팀_출시_준비.md), 글·이미지·사양은 [스토어 페이지 초안](ZR.스토어_페이지_초안.md), 홍보는 [Epic 홍보 전략](ZR.에픽_홍보_전략.md)에 있다. 이 문서는 Epic에서만 따로 해야 하는 일을 모았다.
> 이미지 규격·업로드 도구·한국 등급은 Epic 개발자 문서(dev.epicgames.com, 2026-10-14 확인)를 따랐다. 제출비·수익 배분은 배포 안내 페이지가 자동 조회를 막아(403) 기사와 검색 요약으로 확인했다. **"포털 확인"이라고 적은 항목은 Epic Developer Portal에서 직접 보고 고친다.**

## 1. 지금 상태 (2026-10-14)

- Epic Developer Portal 계정·조직: 없음.
- 빌드: Steam과 같은 `D:/AI/ozs/Game/WindCourier_1.0.0_Shipping/Windows`를 그대로 쓴다. SDK 없이 올리기로 한 결정(스토어 초안 10장, 결정 A) 덕분에 스토어별 코드 경로가 없다.
- 이미 Steam용으로 끝난 것 중 Epic에 그대로 쓸 것: 소개 글 6개 언어, 사양, AI 고지 문안, IARC 설문 답안 초안, 크레딧·라이선스 표기.
- **Epic 규격으로 새로 만든 것(2026-10-14)**: 가로·세로 키아트, 제품 로고, 소셜 미리보기, 스크린샷 jpg 6개 언어, 트레일러(스테레오 음성), 업로드 스크립트 `epic_upload.ps1` — 4·5장.

## 2. Epic 조건 요약

| 항목 | 내용 | 이 게임에 미치는 영향 |
|---|---|---|
| 제출비 | 게임당 **$100**, 출시 전에 낸다. 매출로 돌려받는 조건이 있다(포털 확인) | Steam Direct와 같은 금액 |
| 수익 배분 | 기본 88 / 12. **타이틀당 연 매출 $1M까지는 개발자 100 %**(매년 1월 1일 초기화, 자동 적용) | $7.99 게임이면 사실상 전액이 개발자 몫이다 |
| Unreal 로열티 | Epic Games Store에서 난 매출에는 언리얼 5 % 로열티가 붙지 않는다 | 게다가 언리얼 로열티는 제품당 총매출 $1M부터라 지금 규모에서는 Steam 쪽도 부담이 없다 |
| 업적 | 2023-03-09 뒤에 들어온 게임은 **다른 PC 스토어에 업적이 있으면** Epic 업적도 켜야 한다 | 1.0은 Steam 업적이 없으므로(결정 A) **해당 없음**. 나중에 Steam 업적을 넣으면 Epic 업적도 함께 넣어야 한다 |
| 크로스플레이 | 멀티플레이 게임만 해당 | 싱글플레이라 해당 없음 |
| 연령 등급 | 포털에서 IARC 설문으로 무료 발급(5~15분). 대부분 지역은 선택이지만 등급이 없으면 가장 높은 연령으로 취급된다. **한국은 GRAC 등급 필수 — Epic에서는 IARC 설문으로 GRAC 등급이 함께 나온다**(19세 이상만 GRAC에 직접 신청) | 스토어 초안 9장 답안을 그대로 쓴다. 6장 첫 항목 참고 |
| 스토어 이미지 수위 | 메인 스토어 노출 이미지·글은 IARC 12(ESRB Teen) 이하, 그리고 게임 등급 이하 | 전체이용가 예상이라 해당 없음 |
| 콘텐츠 | 실행되고, 스토어 페이지 설명과 내용이 맞아야 한다 | AI 고지와 설명이 실제 게임과 맞는지 한 번 더 본다 |

## 3. 순서

| 단계 | 누가 | 할 일 | 기간 |
|---|---|---|---|
| ① 포털 가입 | 사용자 | Epic Developer Portal 가입 → 조직(Organization) 생성. 조직 이름 **ozs** | 하루 |
| ② 본인·사업자 인증, 세금·정산 | 사용자 | 사업자 정보, 세금 양식(한국 사업자 → W-8BEN-E 계열, 포털 안내대로), 정산 계좌 | 며칠(검토 대기) |
| ③ 제품 생성, 제출비 | 사용자 | 제품(Product) 만들기, $100 결제 | 하루 |
| ④ 스토어 페이지 입력 | 사용자(문안·이미지는 준비) | 4장 표대로 입력 | 하루 |
| ⑤ IARC 등급 | 사용자 | 포털에서 설문 제출 → 즉시 발급. 배포 지역에 **South Korea**를 넣고 설문을 내야 GRAC 등급이 붙는다(Store Settings > Regions and ratings) | 한 시간 |
| ⑥ 빌드 업로드 | 사용자 | 5장(BuildPatchTool) | 한 시간 |
| ⑦ 검수 | Epic | 스토어 페이지·빌드 검수 | 며칠~2주(포털 확인) |
| ⑧ Coming Soon 공개 | 사용자 | Steam Coming Soon과 **같은 주**에 연다 → Epic 찜 모으기 | Steam과 같은 기간 |
| ⑨ 출시 | 사용자 | Steam과 같은 날, 같은 시각. Steam 출시가 **한국 등급(GCRB)이 나온 뒤**라 Epic도 그 날짜를 따른다 | — |

- ①~③은 Steam AppID를 기다리는 동안 지금 바로 해도 된다. 인증·세금 검토가 가장 오래 걸릴 수 있다.
- 출시일은 Steam 쪽 결정(찜 목표와 출시 판단 날짜, [프로젝트 점검](ZR.프로젝트_점검.md) 3장)에 맞춘다. Epic만 따로 날짜를 잡지 않는다.

## 4. 포털에 넣을 것

| 항목 | 값 / 출처 |
|---|---|
| 제품 이름 | Wind Courier (한국어 설명에 "바람 배달부") |
| 개발사 / 배급사 | ozs / ozs |
| 짧은·긴 설명 | 스토어 초안 1·2장(한·영), 2-1장(독·프·이·스) — Steam의 BBCode 꾸밈은 Epic 편집기에 맞게 다시 다듬는다 |
| 지원 언어 | 인터페이스·자막 6개 언어, 음성 없음 |
| 장르·기능 태그 | 스토어 초안 3장에서 Epic 태그 목록에 있는 것만 고른다 |
| 시스템 사양 | 스토어 초안 4장(Windows만) |
| 제품 로고 (필수) | `output/store_images/epic_logo_960x540.png` — 규격 960×540 PNG, 1 MB 이하. Steam 로고(투명)를 가운데에 맞춤, 0.4 MB |
| 오퍼 이미지 가로 (필수) | `output/store_images/epic_landscape_2560x1440.png`(제목 있음) — 규격 2560×1440, jpg/png. 16:9 키아트(1672×941)를 1.53배 키우고 선명도 보정. 제목 없는 판 `epic_landscape_nologo_2560x1440.png`도 있음 |
| 오퍼 이미지 세로 (필수) | `output/store_images/epic_portrait_1200x1600.png`(로고 있음) — 규격 1200×1600, jpg/png. 제목 없는 키아트에서 3:4 띠(프로펠러~조종석)를 잘라 1.7배 키우고, 아래를 갈색으로 어둡게 한 뒤 로고를 얹음(Steam 세로 캡슐과 같은 방식). 로고 없는 판 `epic_portrait_nologo_1200x1600.png`도 있음 |
| 라이브러리 이미지 (필수) | 오퍼 이미지와 같은 규격. 포털에서 "오퍼 이미지를 라이브러리 이미지로 복사" 옵션을 켠다 |
| 오퍼 이미지 로고 위치 | Epic 예시는 로고를 그림 **가운데**에 둔다(권장, 필수 아님). 우리 것은 비행기를 가리지 않게 가로는 왼쪽 위, 세로는 아래에 두었다. 검수에서 지적되면 바꾼다 |
| 미디어 캐러셀 (필수, 1개 이상) | 이미지 1920×1080, 4 MB 이하 / 영상 MP4 1920×1080, 30·60 fps, AAC 스테레오 48 kHz 320 kbps, 300 MB 이하. Epic 권장: **5개 이상, 영상 하나 이상, 영상을 맨 앞에**, 스크린샷 포함, 글자 적게 |
| 트레일러 | `output/video/trailer_epic.mp4`(37.9 MB) — Steam D판의 영상은 그대로 두고 소리만 모노 → 스테레오 48 kHz AAC로 다시 만듦. 인코더 지정값은 320 kbps이고 실제 측정값은 228 kbps다(소리가 단순해 덜 씀). 업로드에서 막히면 그때 고친다 |
| 스크린샷 (캐러셀·소개란) | `output/store_images/epic_screens/<언어>/*.jpg` — 6개 언어(영어 12장, 나머지 4장씩), 모두 1 MB 이하(280~580 KB, 품질 95). 소개란 추가 이미지 규격(1920×1080 jpg, 1 MB, 최대 20장)과 캐러셀 규격을 함께 맞췄다. Epic은 언어별 이미지를 지원하고 영어만 필수 |
| 소개란 (필수) | 글 + 이미지 1장 이상(가로 700 px 이상, 4 MB 이하). 이미지는 위 스크린샷을 쓴다 |
| 소셜 미리보기 (선택) | `output/store_images/epic_social_1200x1200.jpg` — 1200×1200 jpg, 4 MB 이하. 프로펠러~조종석 정사각형 + 로고, 0.6 MB |
| 만드는 법 | 이미지: `python WindCourier/Scripts/make_epic_keyart.py` / 스크린샷·트레일러: `python WindCourier/Scripts/make_epic_media.py`. 스크린샷이나 트레일러를 다시 만들면 뒤의 것을 다시 돌린다 |
| AI 사용 고지 | [출시 준비 R9](ZR.릴리스타입_출시준비.md) 문안. Epic 포털에 AI 공개 항목이 있는지 확인하고, 없으면 긴 설명 끝에 넣는다 |
| 법적 고지 | 크레딧과 같은 Unreal Engine 문장 |
| 가격 | **$7.99**(Steam과 같게), 지역 가격은 Epic 권장값. 출시 주 10 % 할인도 Steam과 같게 |
| 실행 설정 | 실행 파일 `WindCourier.exe`, Windows 64비트 |
| 사전 요구 설치 | Visual C++ 2015-2022 x64 — Steam과 같은 이유(`-prereqs` 없이 빌드). 포털 설정이 아니라 **업로드 명령의 인자**로 넣는다(5장, `epic_upload.ps1`이 처리) |
| 클라우드 세이브 | Epic 클라우드 세이브 경로 설정이 SDK 없이 되는지 확인(포털 확인). 안 되면 1.0은 클라우드 없이 내고 페이지에 적지 않는다 |
| 연령 등급 | 3장 ⑤ |

## 5. 빌드 올리기 (BuildPatchTool)

스크립트: `WindCourier/Scripts/epic_upload.ps1`(2026-10-14, BuildPatchTool 1.8.8 문서 기준). BuildPatchTool은 포털 **Artifacts and Binaries > Download build patch tool**에서 받아 `C:/BuildPatchTool`에 풀거나 `-BuildPatchTool`로 경로를 준다.

```powershell
# 비밀값은 환경 변수로만 넘긴다(명령 기록·로그에 남지 않게)
$env:WC_EPIC_BPT_SECRET = '<BPT Client Secret>'
# 미리보기: 올릴 파일을 세고 실제 명령만 보여 준다
powershell -ExecutionPolicy Bypass -File WindCourier/Scripts/epic_upload.ps1 `
    -OrganizationId <조직 ID> -ProductId <제품 ID> -ArtifactId <아티팩트 ID> -ClientId <BPT Client ID> `
    -BuildDir D:/AI/ozs/Game/WindCourier_1.0.0_Shipping/Windows -Preview
# 실제 업로드(+ 비공개 샌드박스에 라벨)
... -Label Live -SandboxId <스테이지 샌드박스 ID>
```

- ID 위치: 클라이언트 ID·비밀값은 **Product Settings > BPT credentials**, 아티팩트 ID는 **Epic Games Store > Artifacts and Binaries**.
- Shipping 패키지가 아니면 거부한다. 공개 샌드박스 라벨(= 출시)은 스크립트로 하지 않는다 — `-Label`은 `-SandboxId`가 있어야 받는다. 출시는 포털에서 직접.
- 빌드 버전은 `1.0.0-Windows`(`DefaultGame.ini`의 버전). 같은 아티팩트에 같은 버전을 두 번 올릴 수 없으니 다시 올리면 `-BuildVersion 1.0.0-Windows-2`처럼 준다.
- VC++ 런타임: 엔진의 `vc_redist.x64.exe`를 패키지의 `Engine/Extras/Redist/en-us/`에 복사하고 `-PrereqPath`·`-PrereqArgs "/install /quiet /norestart"`·`-PrereqIds`로 건다. 런처가 처음 설치할 때 한 번 실행한다. 이 파일은 Shipping 폴더에 남으므로 Steam 업로드에도 같이 들어간다(25 MB, 해롭지 않음). Epic 문서는 DLL을 실행 파일 옆에 두는 방식(app-local)을 더 권하지만, 빌드를 바꾸지 않는 쪽을 골랐다.
- `.pdb`는 올리지 않는다(무시 목록을 `Saved/EpicBPT/ignore.txt`에 만든다. 지금 패키지에는 `.pdb`가 없다).
- 확인(2026-10-14): 미리보기로 1.0.0 Shipping 파일 34개·1,892 MB, Development 패키지(`WindCourier_Beta`) 거부, `-SandboxId` 없는 라벨 거부. 실제 업로드는 포털 계정이 없어 아직.
- 업로드 뒤 빌드를 비공개 샌드박스에 걸고 Epic 런처로 설치·실행 한 번.

## 6. 출시 전에 할 일

- [ ] **한국 판매와 등급** — Epic 문서(Game Ratings in Korea, 2026-10-14 확인): 한국 배포에는 GRAC 등급이 법으로 필요하고, **Epic에서 파는 게임은 GRAC가 IARC 등급을 인정한다.** 그래서 Epic판은 포털에서 IARC 설문을 내면(배포 지역에 South Korea 포함) 한국 등급까지 끝난다. 19세 이상이나 등급 거부(RC)일 때만 GRAC에 직접 신청한다. 주의:
  - 공개된 **플레이 가능한 빌드는 모두** 등급이 필요하다(데모·베타·테스트 포함). 플레이할 빌드 없이 미리 구매만 받는 기간은 등급이 필요 없다.
  - 지금 신청 중인 GCRB 등급은 Steam판(IARC 미참여)을 위한 것이다. Epic판은 IARC 쪽 등급을 따로 받는다. 두 등급이 다르게 나오면(예: 전체이용가와 12세) 페이지마다 다른 표시가 되므로 결과를 맞춰 본다.
  - 한국 법의 GaaS 요건(실명 확인, 과몰입 문구 등)은 인터넷으로 콘텐츠를 내려받는 서비스형 게임 대상이라 싱글 오프라인인 이 게임은 해당하지 않는다고 본다. 확률형 아이템도 없다.
- [ ] Epic 런처로 설치·실행 한 번(비공개 채널).
- [ ] 세이브 경로 확인: Epic 런처로 실행해도 `%LOCALAPPDATA%/WindCourier/Saved/SaveGames`에 생기는지. 같은 PC에서 Steam판과 Epic판이 세이브를 같이 쓰게 되는데, 같은 빌드라 문제는 없다.
- [ ] 출시 할인: 포털의 Discount Management Tool에 출시 전에 등록.
- [ ] Mac: 1.0은 Windows만. Mac을 낸다면 코드 서명·공증이 먼저(출시 준비 R5 Mac 기록).
- [ ] README: Release README의 의견 창구가 "Steam 토론 게시판"이다. Epic 구매자를 위해 한 줄 추가할지 결정(예: 이메일이나 itch.io 댓글).

## 출처

- [Epic will now let developers self-publish to the Epic Games Store (APH Networks)](https://aphnetworks.com/news/26880-epic-will-now-let-developers-self-publish-epic-games-store) · [Epic Games has released self-publishing tools (MCV/DEVELOP)](https://www.mcvuk.com/business-news/epic-games-has-released-self-publishing-tools-for-game-developers-and-publishers/) — 제출비 $100, 88 %, Unreal 로열티 면제, 업적·크로스플레이·IARC 조건 (2023 기사)
- [Epic Games Store Distribution](https://store.epicgames.com/en-US/distribution) · [Revenue share](https://store.epicgames.com/distribution/revenue-programs/revenue-share) — $1M까지 100 % (페이지 자동 조회 불가, 검색 요약 기준)
- Epic 개발자 문서(2026-10-14 확인): [Storefront Media Guide](https://dev.epicgames.com/docs/epic-games-store/sales-and-marketing/marketing/storefront-media-guide) — 이미지·영상 규격 · [BuildPatch Tool Instructions (1.8.8)](https://dev.epicgames.com/docs/epic-games-store/publishing-tools/uploading-binaries/bpt-instructions-170) · [Game Prerequisites Installers](https://dev.epicgames.com/docs/epic-games-store/services/game-prereqs-installer) · [IARC Ratings](https://dev.epicgames.com/docs/epic-games-store/requirements-guidelines/content-ratings/iarc-ratings) · [Game Ratings in Korea](https://dev.epicgames.com/docs/epic-games-store/requirements-guidelines/content-ratings/game-rating-korea) · [Content Guidelines](https://dev.epicgames.com/docs/epic-games-store/requirements-guidelines/content-ratings/content-guidelines)
- [IARC Milestones](https://www.globalratings.com/milestones) — Epic 2023년 IARC 도입 · [GRAC joins IARC (ESRB)](https://www.esrb.org/blog/republic-of-koreas-game-rating-and-administration-committee-grac-becomes-newest-participant-in-iarc-rating-system/)
- [EGS Storefront Merchandising Carousel Guidelines (PDF)](https://static-assets-prod.epicgames.com/eos-docs/files/marketing-overview-files/EGS_Storefront_Merchandising_Carousel-Guidelines.pdf)
