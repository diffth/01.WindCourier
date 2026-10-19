# 첫 비행 테스트 실행

Unreal Engine 5.8에서 `WindCourier.uproject`를 열고 C++를 빌드한 다음 `/Game/Maps/LV_FlightTest`에서 Play를 누른다. 기존 맵을 다시 생성할 필요는 없다. `FlightGameMode`가 테스트 세션을 만들며, 맵에 직접 배치한 기류·투하 액터가 없으면 기본 기류 8개와 투하 구역 B1·B2를 생성한다. 그냥 Play하면 하루 흐름(배달 3회 + 귀환)으로 시작한다. 아래 “조작과 1회 측정”은 그 첫 배달 B1의 절차이며, 하루 전체는 “하루 흐름” 절을 따른다.

## 홍보영상 촬영

`-FlightRouteCheck -FlightPromoFlight`는 시작 2초 뒤부터 좌우 S자 선회, 출력 보조 상승, 활공을 실제 조작 입력으로 수행한다. 배달 완료 검증이 아닌 촬영용이므로 `-benchmark -fps=30 -seconds=23`으로 종료 시간을 지정한다. `-FlightShotNoUI`로 HUD를 숨기고, `-DUMPMOVIE -ForceRes -ResX=1920 -ResY=1080`으로 원본 프레임을 저장한다. 앞 2초를 제외한 20초를 편집에 사용한다. 개발 빌드 전용 옵션이다.

## 조작과 1회 측정

| 입력 | 동작 |
|---|---|
| WASD / 왼쪽 스틱 | 선회·피치. W는 기수 내림 |
| Left Shift / RT 홀드 | 출력 보조. 놓으면 순항 |
| Left Ctrl / LT 누름 | 순항·활공 전환. 출력 보조 중에는 무시 |
| E / 게임패드 A | 조건 안에서 투하. 조건 밖에서는 편지 유지. C에서는 착륙 보조 시작·편지 전달 |
| F1 | 정확한 비행 수치 표시 켜기·끄기 |

1. B 방향으로 비행하며 임시 새떼 단서를 보고 기류에 진입한다. 기본 기류 5개는 출발점 A에서 B로 가는 직선 위 500/1100/1700/2300m 지점에 좌우 50m씩 번갈아 비켜 있고, 마지막 하나는 B를 250m 지난 곳에 있다. 하루 흐름에서는 같은 조건의 기류가 3개 더 생긴다(B1→B2에 2개, B2→C에 1개). 직선으로 날아도 기둥 가장자리를 스치므로 중심을 찾아 들어가야 오래 상승한다. 기류는 지면 Z=0 기준 200~1200m, 반경 120m, 상승력 6m/s다. 기류 안에서 방향 입력 없이 0.5초 머물면 최대 20% 선회 보조가 걸린다. 15% 이상 방향 입력으로 즉시 해제된다.
2. 상승 후 Ctrl로 활공하며 목적지 방향을 따라간다. 일반 기류의 수명은 90/135/180초이며, 소멸 20초 전 임시 새떼가 주황색으로 바뀐다.
3. B 지점은 출발 방향 왼쪽 약 32°, 3km 앞의 주황색 원판과 150m 기둥이다. 화면에 보이면 주황 마름모, 화면 밖이면 가장자리 화살표가 B 방향과 거리를 가리킨다. B의 수평 반경 80m 안, 지면 기준 고도 120m 이하, 상승 중이 아닐 때(수평 순항 포함, Shift 상승 중 제외) `DROP READY`를 확인하고 E를 누른다. 일부러 지나치거나 조건 밖에서 눌러도 편지가 유지되는지 확인한다.
4. 성공하면 Shift로 상승한다. 월드 고도 200m에 도달하면 회복 시간이 표시된다. 기류를 쓰지 않고도 회복 가능한지 확인한다.
5. 회복 뒤에는 목적지가 다음 배달(B2)로, 두 투하를 마치면 착륙 전달 C로 바뀐다. 이어서 착륙·전달·재이륙을 측정하려면 아래 “착륙 전달 C”를 따른다. 투하만 측정할 때는 여기서 멈춘다.
6. PIE를 정지한다. 다시 Play를 누르면 편지·기류·측정 시간이 초기화되고 새 CSV가 생성된다. 같은 조건에서 최소 3회 반복한다.

카메라는 기체 뒤 10m에서 따라가며, 순항보다 빨라지면 시야각이 넓어지고(1 m/s당 1°, 최대 100°) 출력 보조 중에는 시야각 +5°와 약한 흔들림이 더해진다. 속도감만 바꾸는 연출이며 순항 25 m/s는 그대로다. 활공 속도는 2026-09-19부터 30 m/s라서 활공 중에도 시야각이 약 5° 넓어진다. 값은 Jenny의 `Flight|Camera` 속성에서 조정한다.

기본 배치는 반복 비교용이다. B 기둥은 충돌이 없어 통과할 수 있다. 기류 시작 시점은 0/15/30/45/60초이며 소멸 20초 후 같은 위치에 다시 생성된다. 이 생성 간격과 6m/s 상승력은 구현용 가설이며 최종 기획 결정이 아니다. 재생성을 포함한 이벤트를 CSV에 남긴다. 맵에 `FlightThermal`을 직접 배치하면 속성 패널에서 수명·고도·반경·상승력을 조정할 수 있다. 배치 액터가 있으면 기본 기류 묶음은 생성하지 않는다.

흐린 날의 **수명만** 비교하려면 에디터 콘솔에서 `open /Game/Maps/LV_FlightTest?FlightCloudy`를 실행한다. 기본 기류의 수명이 60초가 된다. 흐림·안개 아트는 적용하지 않는다. 원래 조건은 `open /Game/Maps/LV_FlightTest`로 돌아간다. 직접 배치한 기류의 수명은 옵션으로 덮어쓰지 않는다.

## 수채화·소리 (D09 짧은 구간 검증)

2026-09-19부터 LV_FlightTest에 임시 수채화 표현과 합성 소리가 들어 있다. 최종 아트·사운드가 아니라 분위기와 단서 전달력을 판단하기 위한 시제품이다.

- **수채화**: 레벨 전체에 포스트 프로세스 `M_WatercolorPP`(색 번짐, 부드러운 명암 단계, 깊이 경계의 안료 뭉침, 약한 채도 감소)를 적용한다. 화면 공간 종이 질감은 GDD 9.3에 따라 넣지 않았다. 지면·벽·지형물은 격자선을 유지한 채 초록 들판·황토·크림·벽돌색으로 바꿨다. **F2**로 수채화 효과만 켜고 끄며 비교한다(색 변경은 유지).
- **소리**: 엔진 부조음, 낮은 바람, 높은 바람 세 개의 반복음을 합성했다. 순항은 엔진 중간, 출력 보조는 엔진이 크고 높아지며, 활공(Ctrl)에서는 엔진이 약 0.5초 만에 사라지고 바람만 남는다. 속도가 순항보다 빠르면 높은 바람이 커지고, 기류 안에서는 낮은 바람이 커지고 조금 높아진다.
- **투하 소포**(2026-10-09): 투하가 받아들여진 순간 기체 아래에서 작은 소포(`AFlightDroppedParcel`)가 기체 속도로 떨어져 나간다. 중력과 공기 저항(종단 속도 32 m/s)으로 떨어지며 구르고, 매 프레임 낙하선이 처음 닿는 지면에 그 경사대로 눕는다. 충돌은 없고 그날 내내 그 자리에 남는다. 판정·기록·소리는 그대로이며 그림만 더한 것이다(자동 테스트 월드에서는 생성하지 않는다). 메시는 `/Game/FlightTest/Environment/Parcel/SM_Parcel`(사용자의 Tripo 모델을 Blender로 거친 FBX, 같은 폴더의 `SM_Parcel.fbx`. `Scripts/import_parcel.py`가 0.4배로 들여와 약 39×12×24 cm, 지도 네 장은 `GDD/characters/Parcel/<tripo>.fbm`에서 `T_Parcel_C/N/R/M`·`M_Parcel`로; `DefaultGame.ini`에서 폴더째 쿡)이며 없으면 신발 상자 크기의 상자로 대신 보인다. 피벗 위치는 상관없다(바운드 중심을 기준으로 떨어지고, 착지하면 가장 얇은 면이 아래, 가장 긴 변이 땅을 따라 눕는다). 모델 확인용 `-FlightParcelPreview`: 시작 때 기체 왼쪽 3 m에 소포를 떨어뜨리고 카메라를 무릎 높이에서 그쪽으로 고정한다(`?Day=1 -FlightShotAt=5 -FlightShotNoUI`와 함께, `Saved/Screenshots/Review/shot_WxH.png`).
- 다시 만들기: 에디터를 닫고 `Scripts/build_flight_av.py`를 먼저 실행한 뒤 `Scripts/build_flight_test_level.py`를 실행한다(둘 다 절대 경로로 `-ExecutePythonScript`에 넘긴다). WAV 원본은 `Saved/GeneratedAudio/`에 생기며 Git에는 가져온 `.uasset`만 남는다.
- 수치 조정: 수채화는 `M_WatercolorPP`의 스칼라 파라미터(Bleed, Steps, Posterize, Soft, EdgeStrength 등), 소리는 Jenny의 `Flight|Audio` 속성에서 한다.

플레이에서 확인할 점은 [프로토타입 검증](../GDD/docs/P.프로토타입검증.md) 1.6절의 세 가지다: 수채화에서도 기체·지형·새떼·B를 구분하는가, 순항·출력 보조·활공과 기류 진입이 소리로 구분되는가, F1 진단을 끈 상태에서도 상태를 이해하는가.

## 착륙 전달 C (재이륙 검증)

2026-09-19 구현. 서사 편지를 착륙해서 전달하고 다시 이륙하는 흐름의 임시 시제품이다. 수치는 모두 첫 테스트용 초기값이다.

- **위치**: C는 A에서 약 1.7km, B에서 약 1.4km 떨어진 들판의 크림색 활주로(길이 400m, 폭 60m)다. 옆에 파란 기둥(100m, 충돌 없음)과 작은 벽돌 집이 있다. 활주로는 150°(B 쪽→A 쪽) 방향이지만 **양쪽 끝 어디서든** 착륙할 수 있다. 기체가 향한 쪽 끝이 착륙 방향이 된다. 화면의 `C` 마름모가 멈출 지점을 가리킨다.
- **시작**: 투하 후 고도를 회복하면 목적지가 C로 바뀐다(하루 흐름에서는 B1·B2를 모두 마친 뒤). 명령 없이도 C를 향해 800m 안에서 지면 고도 160m 아래로 내려오면 착륙 모드로 바뀌고, 그 뒤 선회해도 800m 안·160m 아래에 있는 동안 유지된다. A→B 경로는 C 위를 지나가므로 C를 향하지 않으면 바뀌지 않는다. B 없이 C만 반복하려면 에디터 콘솔에서 `open /Game/Maps/LV_FlightTest?FlightLandingOnly`를 실행한다. A에서 바로 C를 향한다.
- **접근**: 직접 조종한다. 착륙 모드에서는 화면 아래에 필요한 조작이 흰 글씨로 표시되고, 조건이 모두 맞으면 큰 노란 바탕의 `LANDING READY - PRESS [E] / [A]`로 바뀐다. 순서는 ① 활주로 끝을 지나치지 않았는지(너무 늦으면 돌아가서 다시 접근) ② 멈출 지점에서 350m 이내 ③ 중심선 좌우 120m 이내 ④ 활주로 방향(양쪽 중 가까운 쪽)과 오차 35° 이내 ⑤ 지면 고도 80m 이하 ⑥ 속도 33m/s 이하다. 350m 안이면 활주로 바로 위에서 눌러도 되며, 늦게 누를수록 활주로 앞쪽(최대 120m 더)에서 멈춘다. 이 가까이에서는 저고도 경고 대신 착륙 안내가 나온다.
- **착륙**: 초록색 `LANDING READY`에서 E/A를 누르면 착륙 보조가 시작된다. 기존 카메라를 유지한 채 곡선으로 활주로에 내려가 40m를 굴러 멈춘다. 중간에 취소되지 않으며 다른 입력은 무시된다. 조건 밖에서 누르면 이유만 표시하고 계속 비행한다.
- **전달**: 멈춘 뒤 `DELIVER THE LETTER`에서 E/A를 한 번 누르면 약 8초 장면이 끝까지 자동 재생된다. 임시 수취인(파란 몸통)이 앞쪽 오른쪽에 서 있고, 편지가 건너간 뒤 인사한다. 대사는 임시 문안이다.
- **재이륙**: 장면이 끝나면 3초 동안(플레이 2회차 요청으로 1.5초에서 늘림) 멈춘 곳 왼쪽 20m(수취인·집 반대편)의 이륙 지점으로 옮기면서 다음 목적지 A 방향으로 부드럽게 돈다. 이어서 약 2초간 자동으로 달려 이륙하고, 1초 동안 낮은 후방 카메라를 보여준 뒤 순항 상태로 조작을 돌려준다. 그동안 엔진 표시 아래에 `AUTOMATIC - controls return after takeoff`가 보인다. 이륙 직후는 지면 약 6m이므로 저고도 경고를 보고 Shift로 상승한다.
- **막힘**: 활주 경로(앞쪽 약 65m)에 장애물이 있으면 활주를 멈추고 3초간 `TAKEOFF PAUSED`를 표시한다. 시간 제한 없이 같은 자리에서 기다리다가 길이 비면 활주부터 다시 한다. 기본 맵에는 경로 위에 장애물이 없으므로 이 동작은 자동 테스트로 확인했다.
- **기록**: CSV에 `landing_approach_enter`, `can_land_enter/exit`, `landing_rejected`(이유 포함), `landing_assist_start`, `touchdown`, `landing_stopped`, `delivery_start`, `delivery_done`, `takeoff_roll_start`, `takeoff_blocked`, `takeoff_resumed`, `liftoff`, `control_returned`, `landing_input_ignored`가 남는다. 착륙 보조 시간은 `landing_assist_start`→`landing_stopped`, 재이륙 시간은 `delivery_done`→`control_returned`로 읽는다.
- **조정**: 맵에 `FlightLandingSite`를 직접 배치하면 속성 패널에서 접근 허용 범위(`Landing`), 장면 길이(`Delivery`), 이륙 지점·전환·활주·후방 카메라 시간(`Takeoff`)을 바꿀 수 있다. 이륙 지점 비교(바로 옆 / 20~40m 앞)는 `TakeoffOffset`(앞, 오른쪽 m)으로 한다. 배치 액터가 있으면 기본 C는 생성하지 않는다.

같은 조건으로 3회 반복하고, 매번 조건 밖 E 입력 횟수·재접근 여부와 체감(보조가 자동 조종처럼 느껴지는지, 전달 장면의 여운, 이륙 후 방향 판단, 조작 복귀 시점)을 기록한다.

## 하루 흐름 (배달 3회 + 귀환)

2026-09-20 구현. **Play 1회가 하루 1일**이다. 콘솔 옵션 없이 그냥 Play하면 하루 흐름으로 시작한다. 하루는 `A 출발 → 투하 B1 → 투하 B2 → 착륙 전달 C → A 귀환 → 정산` 순서다. 수치는 모두 첫 측정용 초기값이며, 배달 횟수와 하루 길이는 이 실측 뒤에 정한다([미결 사항](../GDD/docs/P.미결사항.md) 배달 횟수·하루 길이).

- **화면 표시**: 왼쪽 위에 `DELIVERY 1/3  B1`처럼 지금 몇 번째 배달인지와 목적지 이름이 나온다. 목적지 표식의 글자도 `B1`, `B2`, `C`, `A`로 바뀐다.
- **다음 목적지 미리 표시**(2026-09-20 1일차 평가로 추가): 투하에 성공하면 고도 회복을 마치기 전부터 왼쪽 위가 `NEXT  B2  1800 m  LEFT 30°`로 바뀌고 링과 표식도 다음 마을을 가리킨다. 올라가면서 방향을 미리 잡으라는 뜻이며, 배달 순번(`DELIVERY 1/3`) 자체는 회복을 마쳐야 넘어간다.
- **B1**: 기존 B와 같은 자리(A에서 약 3km, 남동쪽)다. 투하 후 Shift로 월드 고도 200m까지 회복하면 이 배달이 끝나고 목적지가 B2로 바뀐다. 회복 전에는 다음 목적지가 나오지 않는다.
- **B2**: 계곡 북쪽의 두 번째 마을(B1에서 약 2.2km)이다. 표식·조건·투하 방법은 B1과 같다. 2026-09-20 3일 측정에서 이 구간이 길게 느껴진다는 평가가 있어 거리는 그대로 두고 볼거리를 넣었다: 남쪽 절반에 바위 무리, 계곡 북쪽 벽을 넘는 지점에 400m 간격의 벽돌 탑 두 개(그 사이로 넘어가면 된다), 벽 너머에 낮은 언덕, 그리고 벽을 넘을 때 쓸 기류 하나다. 투하 후 다시 200m까지 회복하면 목적지가 C로 바뀐다.
- **C**: 위의 “착륙 전달 C”와 같다. 하루 흐름에서는 **두 투하를 모두 마쳐야** C가 목적지가 된다. 재이륙하면 목적지가 A로 바뀐다.
- **귀환 착륙**(2026-09-20 1일차 평가로 추가): C에서 이륙하면 목적지가 A가 되고, 우체국 옆 크림색 활주로에 **착륙해야** 하루가 끝난다. 활주로는 C와 같은 150° 방향이고 우체국 건물이 멈출 지점 오른쪽 45m에 있다. 접근·착륙 조건과 `LANDING READY` 표시는 C와 같다. 전달 장면과 자동 재이륙은 없다. 멈추면 그 자리에 그대로 주기하며, 그때 E를 눌러도 아무 일도 일어나지 않는다.
- **정산**: 멈추는 순간 화면 가운데에 배달별 시간·재접근 횟수와 하루 총 시간이 담긴 `DAY COMPLETE` 카드가 뜬다. 같은 내용이 로그에 `DAY_SUMMARY`로도 남는다.
- **정비**(2026-09-25부터 자동): 우체국에 멈추면 기체 점검을 자동으로 하고 탱크를 100%로 채운다. 카드에 점검 결과(가벼운 접촉 `brushes`·긁힘 `scrapes`·충돌 `crashes`, 바닥난 적이 있으면 `ran dry`)와 `fuel n% -> 100%`가 나온다. `M` 키는 없어졌다. 폭풍 날처럼 카드가 없는 날도 휴식하면 채운다. CSV `maintenance,fuel_n_pct`는 `day_done` 직후(폭풍 날은 휴식 직전)에 남는다.
- **다음 날**: 카드의 `[R]  REST - start day n`을 누르면 같은 맵이 다음 날 아침 카운터(`DAY n   TODAY'S ROUTE`)로 다시 열린다. 날짜, 탱크(휴식 때 자동 정비로 100%), 에다 편지 상태(싣고 있음 / 전달함)가 넘어간다. 휴식할 때 같은 세 값이 `Saved/SaveGames/FlightDay.sav`에 저장되어, Play를 멈췄다 다시 눌러도 그날 아침부터 시작한다. 노르바에서 마지막 편지를 전달해 엔딩이 나면 그 순간에도 다음 날 아침을 저장한다(연료 100%, 정산·정비를 거치지 않으므로). 엔딩 직후 Play를 멈춰도 다음 Play는 엔딩 뒤 아침이다. 2일차 이상이면 카운터 오른쪽 위에 `[N]  back to day 1`이 보이고, `N`을 **두 번**(3초 안에) 누르면 저장을 지우고 1일차로 다시 연다. 자동 테스트와 경로·착륙 검사는 저장을 읽지 않는다.
- **저녁 시간 기록(D01)**: 하루 흐름 CSV는 시작에 `day_number`(`n_actm`)를 남긴다. 저녁은 `day_done`에서 시작해 `maintenance`(정비) → `rest`(`to_day_n`, 휴식)로 잰다. 폭풍 날은 `storm_start` → `ledger_closed` → `rest`다. 저녁·폭풍의 머문 시간은 이 행들의 `time_s` 차이다.
- **서랍 편지 여러 통**: 폭풍 뒤 장거리가 열리면 서랍에 `Niko Fern - Orddel valley, Ardenne.`가, 3막부터 `Marta Bell - Serno riverside, Ardenne.`가 더해진다. 방향키 `↑` `↓`로 편지를 고르고(`>>` 표시) `L`로 싣는다. 실린 편지에서 `L`을 다시 누르면 서랍에 내려놓는다(CSV `letter_returned,KEY`). 전달한 편지는 서랍에서 사라지지 않고 맨 아래에 흐린 글씨와 `DELIVERED` / `전달함`으로 남는다(그날·이전 날 모두, 커서는 서지 않는다, 2026-10-05 사용자 요청). 그날 서랍 편지를 하나라도 전했으면 비행 중에 `POST OFFICE` / `우체국` 표식이 함께 떠서, 남은 노선을 두고 바로 돌아가 하루를 마칠 수 있다(우체국 착륙은 원래대로 `day_cut_short`, 마지막 편지는 제외). 서랍 제목 옆 안내가 `[L] take` / `[L] put back` 중 지금 할 동작을 알려 준다. 실은 편지는 노란색 `ABOARD`로 남고 각자 초록 표식(`ORDDEL  letter` 등)이 뜨며, 그 활주로에 착륙하면 전달된다. 두 활주로는 산맥 너머 평지에 서쪽을 향해 놓였다(오르델 (−9, −4) km, 세르노 (−12, +3) km). CSV는 편지마다 `letter_taken,KEY_거리`, `letter_delivered,KEY`, 하루 끝 `letter_done|letter_kept,KEY`.
- **편지 읽기**(2026-09-25, D01): 서랍 편지를 착륙해 전하면 엔딩과 같은 중앙 패널에 편지 본문과 수취인의 반응이 뜬다(`편지_초안` 문안, 제목 예: `니코 페른에게 온 편지`). 20초 동안 열려 있고, 3초 뒤부터 `[E]  close`로 닫고 바로 재이륙한다(CSV `letter_closed,표시_초`). 셀마의 메모는 브레나 정비소 보급 착륙에서 같은 패널로 나온다. 화면 배치는 `-FlightLetterPreview=KEY`(LUME·ORDDEL·SERNO·SCHOOL·WORKSHOP·NORVA)로 `Saved/Screenshots/EndingReview/letter_KEY_WxH.png`에 캡처한다(엔딩 미리보기와 같은 방식, `?Day=6?Act=3?Storm?Ended`로 실행). 패널 줄바꿈은 공백에서 끊는다.
- **아침·이륙·저녁 대사**(2026-09-25, [저녁_아침_대사_초안](../GDD/docs/P.저녁_아침_대사_초안.md)): 카운터 맨 위 오스카 한 줄이 상황(1일차·마지막 편지·폭풍 다음 날·막·엔딩 뒤)에 따라 바뀐다. 노선을 고르면 리브의 인사가 3초 자막으로 뜬다(1일차는 아버지의 격언, CSV `greeting`). 그날 서랍 편지를 전했거나·추락/연료 소진이 있었거나·엔딩을 봤거나·호수를 처음 다녀온 날에만, 우체국에 멈춘 뒤 정산 카드 앞에 `벨로아 우체국, 저녁` 패널(오스카·리브 한 줄씩)이 뜨고 3초 뒤 `E`로 닫는다(`evening_scene,KEY` → `evening_closed,표시_초`). 막이 바뀐 날(시험비행·장거리 첫 귀환)은 전환 대사가 있으므로 저녁 장면이 없다. 폭풍 장부에는 아버지의 격언(2회차)이 여백 글씨로 나오고, 폭풍 날은 비행 HUD를 숨긴다. 화면 확인용 `-FlightShotAt=N`(N초에 `Saved/Screenshots/Review/shot_WxH.png` 한 장).
- **호수 지대와 브레나**: 폭풍 뒤부터 카운터 `[4] LAKE RUN`(호숫가 마을 (−8, +7) km 투하, CSV `route_chosen,lake`)이 열린다. 서랍에 `Lukas Harn - Brena mountain school`(같은 날부터)과 `Selma Orr - Brena workshop`(3막부터)이 더해진다. 브레나 정비소(−13, −1.4 km)는 두 번째 보급 지점이라 장거리·호수 노선에서 `BRENA fuel` 표식이 뜨고, 착륙하면 탱크가 찬다(CSV `refuel,BRENA`). 셀마의 편지를 싣고 있으면 그 급유 장면에서 함께 전달된다.
- **새 지역의 지형(레벨 스크립트)**: 세션이 만드는 편지·보급 활주로에 C와 같은 크림색 활주로·파란 기둥·집이 그려진다(루메도 이제 보인다). 오르델은 남북 벽 사이 계곡(동쪽이 열림), 세르노는 활주로 남쪽 200 m의 강과 작은 배, 브레나는 학교와 정비소 격납고 뒤(서쪽 2.3 km)에 산, 호숫가 마을은 D와 같은 주황 투하 지점과 서쪽 호수, 노르드발은 우체국-등대 직선에서 3.5 km 이상 떨어진 바위 언덕, 노르바는 등대(충돌 없음)와 맵 북쪽 끝의 바다다.
- **엔딩 후 카드**: 엔딩을 본 뒤의 카운터는 시험비행 줄 없이 카드 3장(가까운 배달·장거리·호수)을 가까운 순으로 보여주고, 전날 고른 노선을 맨 뒤로 보낸다(`(flown yesterday)`). 키는 그대로(`1`·`2`·`4`)이고 순서만 바뀐다. 전날 노선은 휴식이 `Last=`로 넘기고 저장된다.
- **재방문 마을 변화**: 2막부터 우체국 앞에 새 노선 게시판(크림색 판)과 수리 자재(벽돌색 상자), 3막부터 D 마을에 안전한 착륙 표지(파란 기둥과 판)와 호수행 우편함(파란 상자), 엔딩 뒤 노르바 등대에 불빛과 우편 깃발(주황)이 선다. 그 마을 위 1 km 안을 300 m 아래로 처음 지날 때 아래쪽에 한 줄 문구가 6초 뜨고(CSV `village_change,BELLOIS2|ARDENNE3|NORDVALLEND`), 다시는 뜨지 않는다(휴식이 `Seen=`으로 넘기고 저장).
- **새 장면은 한 번씩**: 새와의 장면(첫 만남 → 동행 선회 → 날개 위 쉼)은 끝낸 만큼 휴식·엔딩 저장이 `Birds=n`(세이브 `BirdScenesDone`)으로 넘긴다. 첫 만남을 마친 다음 날부터는 다시 나오지 않는다(2026-09-25 전에는 매일 아침 다시 나왔다). 이 수정 전 세이브는 0으로 읽혀 첫 만남이 한 번 더 나온다.
- **첫 비행 이륙 오프닝**: 새를 아직 만나지 않은 1일차는 공중이 아니라 **우체국 활주로 위**에서 시작한다. 카운터가 떠 있는 동안 활주로에 서 있다가, 노선을 고르면 3초 뒤 활주 → 이륙 → 상승하고, 조작이 돌아오는 순간 새 첫 만남이 시작된다(CSV `opening_takeoff,A` → `takeoff_roll_start`·`liftoff`·`control_returned,A` → `bird_meeting_start`). 이륙 중 `E`는 받지 않는다. 자동 테스트·경로 검사는 기존 공중 시작 그대로다. 무인 실행으로 카드를 넘기려면 `-FlightAutoRoute`(가까운 배달 자동 선택).
- **꾸미기**: 노선 첫 배달(가까운 배달 = 강물빛 도장, 장거리 D = 능선 회색 스카프, 호수 = 호수 청색 도장), 처음 노르드발 안개 진입(안개 흰색 스카프), 마지막 편지(등대 주황 표식), 각 막의 끝(첫 우편·능선 통과·노르바 등대 표식)에 해금되어 **그때 탄 기체에 바로** 입혀진다. 화면 위쪽에 "지역·노선 — 꾸미기 해금"이 5초 뜨며, 지형 경고·저고도·긁힘이 뜨는 동안은 숨었다가 남은 시간만 이어서 보인다. 성능은 바뀌지 않는다. CSV `cosmetic_unlocked`. 해금 기록과 기체별 적용 상태는 저장·다음 날로 넘어간다(`Unlocked=`, `Worn=`).
- **노선 확장(2026-09-25, [이야기_확장안](../GDD/docs/P.이야기_확장안.md))**: 카운터는 노선 7개를 이야기 순서로 한 줄씩 보여주고 **키도 순서대로** 바뀌었다: `[1]` 가까운 배달 · `[2]` 해안(루메 마을·해안 끝 투하) · `[3]` 시험비행 · `[4]` 장거리 D · `[5]` 호수 · `[6]` 아르덴느 순회(오르델 계곡·세르노 강가 투하) · `[7]` 절벽 마을(착륙 전달, 편지 패널). **앞 노선을 한 번 마치면 다음 노선이 열린다**(가까운→해안→시험비행→폭풍→장거리→호수→아르덴느). **3막은 아르덴느 순회에서 돌아올 때**, 마지막 편지는 **절벽 마을을 마친 뒤** 서랍에 들어온다. 마친 노선은 휴식 URL `Done=`과 세이브 `RoutesDone`으로 이어지고, 이전 세이브는 막으로 추정한다. 엔딩 뒤에는 시험비행을 뺀 열린 노선 전부가 나오고 전날 노선이 맨 뒤로 간다(3장 제한 없음). 해안 노선은 2단계(2026-09-25)에서 지형을 넣었다: 루메 해안 북·서쪽 바다, 루메 마을 (−1.6, +2.4)·해안 끝 (−3.0, +1.0) km 투하 패드, 집 세 채·루메 등대·오두막. 해안 날 아침 오스카, 루메 마을 위 첫 통과 문구("등대 창문에…", CSV `village_change,LUMECOAST`), 첫 해안 노선 저녁(`evening_scene,COAST`)이 붙는다. 아르덴느 순회도 3단계(2026-09-25)에서 지형을 넣었다: 오르델 계곡 마을 (−8.5, −4.3) km(초안 좌표가 계곡 남쪽 벽 위라 동쪽 입구로), 세르노 강가 (−12.4, +3.5) km 투하 패드, 마을 집·강가 선착장, 계곡 남쪽 벽 동쪽 면의 작은 화살표. 순회 날 아침 오스카, 오르델 마을 위 첫 통과 문구("계곡 벽에 작은 화살표가…", `village_change,ORDDELARROW`). 절벽 마을도 4단계(2026-09-25)에서 지형을 넣었다: (−5, +11) km 활주로(114°)·집 세 채·활주로 너머 700 m의 바위 언덕. 호수·절벽 날 아침 오스카, 절벽 마을 첫 완주 저녁(`evening_scene,CLIFF`), 호수 첫 배달 저녁은 승인 문구("예전 소리하고 비슷하대")로 바뀌었다. 테스트 옵션: `?StartNear=CLIFF`(절벽 마을 앞에서 시작), `?StartNear=S`·`?StartNear=BRENA`(보급지 앞에서 시작, 2026-10-06), `?StartNear=C`(에밀의 과수원 앞에서 시작), `-FlightYardShot`(카메라를 우체국 앞마당·오스카에 고정 — `-FlightShotAt=8 -FlightShotNoUI`와 함께 풀 제거 확인용), `-FlightPilotShot`(카메라를 조종석 위 왼쪽 앞에 고정, `=low`는 바퀴 높이 옆 — 조종석 리브 확인용), `?StartNear=D`·`?StartNear=LAKE`(장거리 D 마을·호숫가 마을을 A에서 오는 방향으로 1.2 km 앞·120 m에서 시작, 2026-09-27 아트 확인용), `?TimeOfDay=Morning|Day|Evening`(시간대 고정, 2026-09-27 아트 확인용), `?StartNear=CLOUD`(첫 기류 위 구름 1 km 앞, 구름 중간 높이에서 구름 쪽으로 수평 시작 — 구름 통과 확인용, 2026-10-09), `?StartNear=PASS`(장거리 노선에서 고개 3 km 앞·고개 높이 +80 m에서 D 쪽으로 시작, 산맥 안개 확인용. 장거리가 열린 날이어야 한다: `?Day=6?Act=2?Storm -FlightAutoRoute=long`), `?PassWind=Max`(그날 고개 바람을 가장 센 날로 고정, 바람 단서 확인용), `-FlightLanguage=ko|en`(그 실행만 한국어/영어, 캡처용 — 평소 언어는 카운터에서 `Tab`, 기본 영어), `-FlightDiagnostics`(F1 진단 표시를 켠 채 시작 — 수취인 위치 표식도 보인다), `-FlightRecipientPreview`(C 활주로에 에밀 대신 리깅된 에다를 세운다, `=Lukas`·`=Marta`·`=Niko`·`=Otto`·`=Selma`처럼 리깅된 다른 인물도, `=OskarM`처럼 정적 모델도 — `-FlightRouteCheck -FlightLandingCheck -FlightVisualCheck`로 전달 장면 줌인 캡처), `-FlightGlideAfter=N`(노선을 고르고 N초 뒤 한 번 활공을 켠다 — 3막 새 쉼 장면 캡처용: `?Day=8?Act=3?Storm?Birds=2 -FlightAutoRoute -FlightGlideAfter=3`), `-FlightAutoRoute=cliff`처럼 노선 키를 주면 그 노선을 자동 선택. `StartNear`는 기체의 비행 방향까지 활주로 쪽으로 돌린다(이전에는 위치만 옮겨 방향이 어긋났다). 편지 키(`LUME`, `ORDDEL` 등)로 시작하면 그 편지도 자동으로 싣는다 — 편지 활주로는 그 편지를 싣고 있을 때만 착륙을 받는다(2026-09-27, 다른 편지를 실은 채 오르델에서 E가 투하 거절로 처리됐다). CSV `route_chosen`은 `coast_run`·`ardenne_round`·`cliff_village`. 이하 이전 항목의 키 번호(`[2]` 장거리, `[4]` 호수)와 해금 시점은 이 변경 전 기록이다.
- **남은 우편과 일찍 돌아오기**(2026-09-25): 투하에 성공하면 다음 목적지가 화면 가운데에 4초 뜬다(`NEXT  SERNO RIVERSIDE  8.7 km  -  turn LEFT`). 투하가 남아 있어도, 오늘 우체국에서 2 km 이상 나갔다가 우체국에 낮게 다가가면 착륙이 열리고(`Mail still aboard - landing here ends the day`) 착륙하면 하루가 끝난다(CSV `day_cut_short`·`mail_kept,마을`). 그 노선은 완료로 치지 않고, 전한 곳을 기억해 두었다가 다음에 그 노선을 고르면 **남은 곳만** 들른다(카드에 `(n left from before)`, CSV `route_resumed`, URL `Part=`, 세이브 `PartialRoutes`). 남은 곳을 다 전하면 노선 완료. 보급·서랍 편지 활주로도 이제 다음 들를 곳(남은 마을 → C → 우체국)을 향해 이륙한다.
- **언어**(2026-10-07): `Tab`은 영어 → 한국어 → `Content/FlightTest/Data/Translations/*.json` 표가 있는 언어 순으로 돈다(지금은 독일어·스페인어·프랑스어·이탈리아어 표까지 6개). 표 만들기·점검은 `Scripts/translations.py`, 구조는 [출시 준비 "다국어 구조"](../GDD/docs/ZR.릴리스타입_출시준비.md). `-FlightLanguage=<코드>`로 그 실행만 바꾼다.
- **인트로 카드**(2026-10-07, 사용자 A안): 1일차(새 게임)를 열면 조작 카드보다 먼저 세계관 세 문단 카드가 뜬다(전쟁 뒤 편지를 나르는 복엽기, 돌아오지 않은 아버지와 남은 제니·우편 가방, 벨로아 우체국 첫 출근). `E`/`A`로 넘기면 조작 카드(CSV `intro_card_closed`), `Tab` 언어. 테스트 실행과 `-FlightAutoRoute`에서는 뜨지 않는다. 확인용 실행: `?Day=1`(세이브를 읽지 않음). 문안은 [저녁·아침 대사 초안](../GDD/docs/P.저녁_아침_대사_초안.md).
- **조작 안내 카드**(2026-09-25, 외부 플레이테스트 준비): 1일차를 열면 카운터 전에 `조작` 카드가 먼저 뜬다(W/S 기수, A/D 선회, Shift 출력 보조, Ctrl 활공, E 투하·착륙·전달·닫기, 1~7 노선, Up/Down·L 서랍, R 휴식, N 두 번 처음부터). `E`로 닫으면 카운터(CSV `controls_card_closed`). 테스트 실행과 `-FlightAutoRoute`에서는 뜨지 않는다.
- **노선 잠금(막 진행)**: 1일차 카운터는 `[1]`만 열린다. 2일차부터 `[3]` 시험비행이 오스카의 제안과 함께 열리고, 시험비행에서 돌아온 다음 날은 **폭풍 휴식**(한 번)이다 — 비행 없이 오스카의 장부 대사가 뜨고, `E`로 장부를 덮은 뒤에만 `[R]` 휴식이 나온다. 그다음 날부터 `[2]` 장거리가 열리며 `[3]`은 `flown`으로 닫힌다. 2막부터는 카운터 제목 줄 오른쪽에 `[H]  aircraft: Jenny`가 보이고 `H`로 **DH-4**와 바꾼다(더 큰 임시 도형, 순항 28 m/s·활공 33 m/s·출력 33 m/s·상한 44 m/s·선회 50°/초·상승 8 m/s·활공 13:1·연료 0.8). 3막부터는 **Mailwing**(날개 긴 임시 도형, 순항 34 m/s·활공 39 m/s·출력 43 m/s·상한 52 m/s·선회 70°/초·상승 12 m/s·활공 23:1·연료 4/7 — 2026-10-09 사용자 요청으로 한 단계 더 올림, 그 전에는 31·36·38·48·60·10·18:1·2/3. 성능 서열은 Jenny < DH-4 < Mailwing, Jenny는 순항 25·활공 30·출력 28·상한 40·선회 40·상승 6·활공 9.7:1. 착륙 속도 한계는 기체의 활공 속도 + 3: 33 / 36 / 42 m/s). 기체는 노선을 고르기 전 카운터에서만 `H`로 바뀐다까지 `H`로 차례로 돈다. 선택은 저장되며 CSV에 `aircraft`(Jenny / DH-4 / Mailwing)로 남는다. 2막에서 장거리를 날아 우체국에 착륙하면 그 자리에서 **3막**이 된다 — 정산 카드 위에 오스카의 대사와 `LAST LETTER - Norva lighthouse, Nordvall`이 뜨고 CSV에 `act3_entered`가 남으며, 다음 날부터 서랍에 `The last letter - Norva lighthouse, Nordvall.`이 열리고 오스카의 첫 줄이 "마지막 편지도 우편이야. 네가 직접 전하면 돼."로 바뀐다. `L`로 싣고 북쪽 끝 노르바 등대(−8, +15 km, 북향 착륙)에 내리면 편지 전문 네 줄(18초)이 뜨는 **엔딩**이다(CSV `ending,last_letter`, 저장·URL `Ended`, 다시 나오지 않음). 마지막 편지를 실은 날은 기본 바람이 등대 쪽으로 분다(CSV `wind_answers`). 북쪽 10 km부터 12 km까지 안개가 기본의 6배까지 짙어진다. 닫힌 노선은 여는 조건과 함께 회색으로 남는다. 막도 저장되며 `N` 두 번은 1일차·1막으로 되돌린다. 건너뛰기 키는 없으므로 장거리를 보려면 1일차 배달 → 2일차 시험비행 → 3일차 순서를 한 번 거친다. `[L]` 서랍 편지는 1일차부터 열려 있다.
- **기록**: CSV에 `day_start`, `leg_start`, `leg_done`(이름과 구간 초), `day_done`(총 초)이 남는다. 귀환 착륙의 사건은 C와 같은 이름을 쓰되 `subject`가 `A`다(`landing_approach_enter,A`, `landing_assist_start,A`, `touchdown,A`, `landing_stopped,A`). 또한 `drop_success`·`approach_enter`·`can_drop_enter/exit`의 `subject`에 마을 이름이 들어간다. 고도 회복은 배달마다 따로 측정한다.
- **최소 3일**: 같은 조건으로 최소 3회(3일) 반복하고 매번 정산 카드의 숫자와 체감(반복 대기·재접근·긴 이동의 지루함, 하루가 길거나 짧게 느껴지는지)을 [측정 기록](../GDD/docs/P.프로토타입_측정기록.md)에 남긴다. 한 회차에 변수를 여러 개 바꾸지 않는다.
- **단일 배달로 돌아가기**: 자동 검사(`-FlightRouteCheck`, `-FlightLandingCheck`)와 `?FlightLandingOnly`는 예전처럼 투하 1회만 쓰며 하루 흐름을 켜지 않는다. 맵에 `FlightDropZone`을 직접 배치하면 배치한 개수·순서가 그대로 하루의 배달 순서가 되고(2개 이상이면 하루 흐름), 각 액터의 `Label`이 화면과 CSV에 쓰인다.

## 연료 (D03 1단계)

2026-09-20 구현. 소비 모델과 게이지·경고까지다. 장거리 노선과 보급 지점은 2단계에서 넣는다. 수치는 모두 하루 흐름 실측에서 뽑은 **초기값**이다.

- **소비**: 순항 0.15%/초, 출력 보조 0.4%/초, 활공 0%/초다. 활공이 공짜라서 장거리에서 바람을 쓸 이유가 생긴다. 착륙 보조·전달·자동 이륙 같은 자동 구간과 지상 정지 중에는 소비하지 않는다(간이 처리).
- **게이지**: 화면 오른쪽 위에 막대와 `FUEL  %`를 표시한다. 30% 이하에서 주황, 15% 이하에서 빨강으로 바뀐다. GDD의 최종 규칙은 “장거리에서만 게이지”이지만, 실측을 위해 지금은 항상 표시한다.
- **경고**: 30% 이하에서 `FUEL LOW`, 15% 이하에서 `FUEL CRITICAL`을 화면 아래에 3초 표시하고 10초마다 반복한다. F1 진단에도 정확한 연료가 나온다.
- **고갈**: 0%가 되면 엔진이 멈춰 활공 상태로 바뀌고 Shift가 듣지 않는다. 기체는 계속 날 수 있으므로 활공해서 착륙하면 된다. 연료 고갈 후 안전 지점 복귀(D04)는 아직 구현하지 않았다.
- **기록**: CSV에 `fuel_pct` 열이 추가됐다. 75/50/25% 시점에 `fuel_level`, 고갈 시 `fuel_empty`가 남는다.
- **참고 소비량**: 자동 검사(B 투하 + C 착륙, 210초)가 19.8%를 썼다. 실측 하루(배달 3회)에 대입하면 25~56%이며, 출력 보조를 많이 쓴 날일수록 많이 든다.
- **조정**: Jenny의 `Flight|Fuel` 속성에서 소비율을, HUD의 `FuelWarningPercent`·`FuelCriticalPercent`에서 경고 기준을 바꾼다.

## 장거리 노선과 보급 (D03 2단계)

2026-09-21 구현. 하루 흐름으로 Play하면 **출발할 때 노선 카드**가 뜬다(위쪽 숫자열 `1`·`2`·`3`·`4`). **플레이에서는 번호를 누를 때까지 기다린다**(2026-09-24 변경: 25초 자동 선택이 서랍 편지를 고르는 중에 두 번 하루를 시작시켰다). 카드가 떠 있는 동안 기체는 아침 자리에 멈춰 있고 연료도 쓰지 않으며, 노선을 고르면 순항 속도로 풀려 출발한다. 자동 테스트에서만 25초 뒤 기본 노선으로 시작하고 제목에 남은 초가 보인다.

- **[1] 오늘의 배달**: 지금까지와 같다(B1·B2 투하 + C 착륙 전달 + 우체국 귀환). 왕복 약 28%를 쓴다.
- **[2] 장거리 배달 D**: A에서 서쪽 10km의 마을 D에 투하하고 우체국으로 돌아온다. 착륙 전달 C는 이 노선에 없다. 중간에 **산맥을 넘어야 한다**.
- **예상 소모량**: 카드에 `왕복 최대 N% (활공 미반영)`으로 표시한다. GDD 3.4의 확정 사항대로 **기류·활공으로 아낄 연료를 미리 차감하지 않으므로 최악의 경우 값**이다. 90% 이상이면 `SUPPLY STOP NEEDED`와 보급 지점을 함께 표시한다. 실제로는 활공을 쓰면 훨씬 덜 든다 — 이 차이가 적절한지가 이번 검증의 핵심이다.
- **보급 지점 S**: 항로에서 800m 옆, A에서 약 5km 지점(**산맥 앞**)의 크림색 활주로다. 장거리 노선에서는 화면에 파란 `S fuel` 표식이 계속 떠 있다. 목적지가 아니라 **들르고 싶을 때 들르는 곳**이다. 낮게 S를 향해 접근하면 착륙 안내가 뜨고, C와 같은 방식으로 착륙한 뒤 E를 누르면 8초 장면과 함께 **연료가 가득 찬다**(무료). 이어서 자동으로 이륙한다.
- **산맥**(2026-09-21 플레이 평가로 추가): A에서 6.5km 지점에 남북으로 산맥이 가로놓여 있다. 항로 정면은 높이 520m이고, **남쪽으로 600m 내려가면 300m 높이의 안부(saddle)**가 있다. 넘어가려면 고도를 벌어야 하므로 출력 보조나 기류를 쓰게 된다. 산맥 앞에 기류가 하나 있다.
- **항로 지형지물**: A→D 사이에 3km마다 벽돌 탑, 1.5km마다 바위 무리, 8.6km 지점에 언덕, 산맥 앞에 구릉이 있다.
- **기록**: CSV에 `route_chosen`(`local_B1_B2_C` 또는 `long_D`), 보급 시 `refuel,S`가 남는다. 보급 착륙의 사건은 `subject`가 `S`다.
- **주의**: 장거리는 편도 약 6분, 왕복 12분 안팎이다. 하루 흐름(6분대)보다 한 회차가 훨씬 길다.

## CSV와 해석

기류 첫 안내: 출발 전 오스카가 새떼가 상승기류의 단서라고 알려준다. HUD의 `CIRCLING BIRDS = RISING AIR`는 “빙글 도는 새떼가 상승기류의 단서”라는 뜻이다. 새떼 쪽으로 접근하고 기류 안에서 선회하면 고도를 얻을 수 있다. 첫 활성 기류에 실제 진입하면 `RISING AIR FOUND ↑ 상승 중`과 선회 안내가 화면 아래 중앙에 3초 동안 뜬다. 기체가 출력 보조로 상승하는 것만으로는 진입 안내가 뜨지 않는다. 출발 안내는 첫 진입 전까지 유지하되 투하 관련 안내가 우선하며, 새 PIE 세션에서 초기화한다. F1 진단 표시를 꺼도 보인다. 다음 플레이에서는 안내를 이해하고 새떼를 의도적으로 찾아가는지 확인한다.

투하 접근 예고: 미배달 상태로 B 중심의 수평 거리 300m 이내에 들어오면 노란색 `DROP APPROACH` 안내가 뜬다. 300m 밖으로 벗어나면 안내를 즉시 숨기고, 다시 접근하면 현재 조건에 맞춰 새로 표시한다. 노란색·초록색 투하 안내가 표시되는 동안 월드의 B 목적지 표식은 잠시 숨기고, 화면의 방향·거리 정보는 유지한다. 고도가 높거나 상승 중이면 안내를 “고도를 낮추세요”·“상승을 멈추세요”로 즉시 갱신한다. 두 조건이 함께 부족하면 고도 안내를 먼저 표시하고, 고도 조건을 충족한 뒤에도 상승 중이면 상승 중단 안내로 갱신한다. 조건이 맞으면 초록색 안내 때 E/A를 누르도록 예고한다. 실제 투하 조건 충족 시 같은 위치에 초록색 `DROP READY`가 표시된다. 놓쳐도 우편은 유지되므로 다시 접근할 수 있다. 접근 예고 거리는 HUD의 `DropApproachDistanceMeters`에서 조정한다. 화면 캡처 검증은 기존 네 장에 `05_drop_approach.png`를 추가로 저장한다.

2026-09-19 고도 안내 추가: F1 진단 표시를 꺼도 엔진 상태 아래에 상승·유지·하강 화살표와 문구가 보인다. 활공 중에는 `GROUND CLEARANCE`로 기체 바로 아래 지면까지의 높이(m)를 함께 표시한다. 지면 50m 이하에서는 순항·출력 보조 상태에서도 숫자를 표시하고 `LOW ALTITUDE`와 Shift/RT 상승 안내를 띄운다. ±0.5 m/s 이내는 고도 유지로 표시한다. 경고 높이는 HUD의 `LowAltitudeWarningMeters`에서 조정한다. 다음 플레이에서는 안내를 보고 활공을 유지할 수 있는지, 지면 접근 시 경고와 투하 안내를 함께 읽을 수 있는지 확인한다.

저장 위치는 `Saved/FlightTelemetry/<UTC시각>_<고유ID>.csv`다. 원본 로그는 Git 추적 대상이 아니므로 필요한 실행 결과는 GDD 측정 기록에 요약한다.

`WindCourier.Flight.DeliverySession` 자동 테스트의 합성 기록은 `Saved/FlightTelemetry/Automation/`에 저장한다. 이 테스트는 별도 시험 월드에서 접근 위치를 설정하고 실제 입력 처리 함수·비행 이동·세션 이벤트·파일 저장을 실행한다. 키보드/패드 하드웨어 입력과 실제 맵 전체 노선을 조종하는 테스트는 아니다.

- 0.25초 간격 표본과 상태 전환, 기류 생성·진입·이탈·소멸, 투하 가능 진입·이탈, 시도 결과, 고도 회복, 세션 종료를 기록한다.
- `state`: 0 순항, 1 출력 보조, 2 활공. `cruise_s/power_s/glide_s`는 엔진 상태별 누적 시간이다.
- `search_s/climb_s/phase_glide_s/other_s`는 서로 겹치지 않는 관측 구간이다. 기류 안에서 상승 중이면 상승, 그 외 활공 상태이면 활공, 투하 전 기류 밖의 나머지는 탐색, 나머지는 기타로 분류한다. 탐색에는 목적지 이동·접근도 포함될 수 있으므로 사람의 의도나 140초 루프를 자동 판정하는 지표로 쓰지 않는다.
- `reapproaches`는 미배달 상태에서 수평 300m 접근권에 들어온 횟수에서 최초 1회를 뺀 값이다. 투하 가능 진입 횟수와는 다르다.
- `recovery_s=-1`은 미회복이다. 성공한 투하부터 월드 고도 200m까지 측정하며 `altitude_recovered`의 `subject`에 `without_thermal` 또는 `thermal_assisted`를 남긴다. 피치 조작은 허용되므로 `without_thermal`만으로 출력 보조 사용을 보장하지 않는다. 엔진 상태와 함께 확인한다.
- 기류 고도는 각 액터 기준 높이, 투하 고도는 기체 바닥부터 아래 지형까지의 AGL이다. 기본 맵의 기류 바닥은 모두 월드 Z=0이며 회복 목표는 월드 고도 200m로 고정되어 있다. 기류를 다른 높이에 배치하는 실험에서는 회복 기준도 수정해야 한다.

새떼는 디버그 선으로 그린 임시 단서다. F1로 수치를 꺼도 남는다. 구름·스카프·사운드와 완성 UI는 아직 구현하지 않았으므로 이 빌드에서 최종 분위기·단서 전달력·UI 사용성을 통과로 판정하지 않는다. 충돌 후 안전 지점 복귀와 장거리 연료도 후속 범위다.

## 외부 플레이테스트용 패키지 (2026-09-25)

에디터 없이 실행되는 Win64 Development 패키지. 에디터를 닫고 저장소 루트에서 만든다. 결과물은 저장소 밖에 둔다(약 1.1 GB).

```powershell
& 'C:/Program Files/Epic Games/UE_5.8/Engine/Build/BatchFiles/RunUAT.bat' BuildCookRun "-project=$PWD/WindCourier/WindCourier.uproject" -noP4 -platform=Win64 -clientconfig=Development -build -cook -stage -pak -archive '-archivedirectory=D:/AI/ozs/Game/WindCourier_0Proto' -unattended -utf8output
```

- 실행: `D:/AI/ozs/Game/WindCourier_0Proto/Windows/WindCourier.exe` (2026-09-27 `WindCourier_Build`에서 이름을 바꿈). 처음 켜면 1일차 조작 안내 카드부터 시작한다.
- 소리: C++에서 경로로 불러오는 `/Game/FlightTest/Audio`는 `Config/DefaultGame.ini`의 `DirectoriesToAlwaysCook`으로 쿡한다(2026-09-25 추가 — 그 전 패키지에는 엔진·바람·접촉 소리가 전부 빠져 있었다, 로그 `Failed to find object ... S_JennyEngine`). 패키지 로그에 `Failed to find object`가 0건인지 확인한다.
- 테스터 기록: 비행 CSV는 `Windows/WindCourier/Saved/FlightTelemetry/`, 세이브는 `Windows/WindCourier/Saved/SaveGames/FlightDay.sav`. 플레이 뒤 `Saved` 폴더째 받으면 된다.
- 처음부터 다시: 카운터에서 `N` 두 번, 또는 `Saved/SaveGames` 폴더를 지운다.
- 다른 사람에게 줄 때는 `Windows` 폴더 전체를 넘긴다. 확인 실행으로 생긴 `Saved` 폴더는 지우고 넘긴다.

## 알파 패키지 (2026-09-27)

프로토타입 패키지(`WindCourier_0Proto`, 옛 이름 `WindCourier_Build`)는 그대로 두고, 알파 아트가 들어간 빌드는 **별도 폴더**에 만든다(사용자 결정). 명령은 위와 같고 `-archivedirectory`만 다르다:

```powershell
& 'C:/Program Files/Epic Games/UE_5.8/Engine/Build/BatchFiles/RunUAT.bat' BuildCookRun "-project=$PWD/WindCourier/WindCourier.uproject" -noP4 -platform=Win64 -clientconfig=Development -build -cook -stage -pak -archive '-archivedirectory=D:/AI/ozs/Game/WindCourier_Alpha' -unattended -utf8output
```

- 실행: `D:/AI/ozs/Game/WindCourier_Alpha/Windows/WindCourier.exe` (약 1.3 GB). 기체·풍경 모델, 시간대, 음악, 종이·잉크 UI(고운바탕), 영문 대사, 고개 눈보라·붓 바람 줄기, 황조롱이가 들어 있다.
- 경로로 불러오는 에셋: `DirectoriesToAlwaysCook`에 `/Game/FlightTest/Audio`, `/Game/FlightTest/UI`, `/Game/FlightTest/Materials`(고개 안개·눈보라·붓 줄기 재질, 2026-09-27 추가). 패키지 로그의 `Failed to find object` 0건을 확인한다.
- 글꼴 라이선스: 빌드 뒤 `Windows/Licenses/GowunBatang-OFL.txt`로 복사한다(SIL OFL은 배포할 때 라이선스 전문을 함께 넣어야 한다).
- 2026-09-27 첫 빌드: BUILD SUCCESSFUL, `Failed to find object` 0건, 고개 앞 확인 실행에서 글꼴·종이 띠·눈보라·황조롱이 확인, 확인 실행의 `Saved`는 지웠다.

## 패키지 스크립트 (2026-09-29, ZR R7)

이제 패키지는 스크립트로 만든다. 아래 절들의 RunUAT 명령과 수동 복사를 한 번에 한다:

```powershell
powershell -ExecutionPolicy Bypass -File WindCourier/Scripts/package_build.ps1 -ArchiveDir D:/AI/ozs/Game/WindCourier_Beta
```

- `-Config Shipping`(R5용), `-SkipBuild`(이미 있는 패키지에 파일만 넣기).
- 빌드 로그 `WindCourier/Saved/Logs/package_build.log`, `Failed to find object` 건수를 출력한다.
- `Windows/Licenses/GowunBatang-OFL.txt`, `Windows/README_읽어주세요.txt`를 넣고 `Windows/WindCourier/Saved`를 지운다 — **테스터가 플레이한 폴더에 돌리면 그 기록이 지워진다.** 받을 기록은 먼저 옮긴다.

## 베타 테스트 빌드 (2026-09-27)

외부 피드백용. 명령은 알파와 같고 `-archivedirectory=D:/AI/ozs/Game/WindCourier_Beta`만 다르다. 빌드 뒤 두 파일을 `Windows/`에 넣는다:

- `Licenses/GowunBatang-OFL.txt` ← `WindCourier/SourceArt/Fonts/GowunBatang/OFL.txt`
- `README_읽어주세요.txt` ← `WindCourier/SourceArt/Playtest/README_읽어주세요.txt` (테스터 안내: 실행, 조작, Tab 언어, F2 수채화, 봐 줬으면 하는 점, `Saved` 폴더 보내기. 한국어·영어, 메모장용 UTF-8 BOM·CRLF)

- 2026-09-27 첫 빌드: BUILD SUCCESSFUL, `Failed to find object` 0건, 한국어 카운터 확인, 확인 실행의 `Saved` 삭제. 알파 빌드에 더해 언어 선택, 연료 게이지 위치, F1·F2 상태 표시가 들어 있다.

## 일시정지 메뉴 (2026-09-28, ZR R1)

- `Esc`(패드 `Start`): 게임을 멈추고 계속 / 조작 안내 / 게임 종료 메뉴. 1막 회항 선택 중의 `Esc`는 선택을 닫는다(메뉴 아님).
- 메뉴: 계속 / 조작 안내 / 타이틀로 / 게임 종료(타이틀로·종료는 한 번 더 묻는다).
- 타이틀(ZR R2): 새로 켤 때만 뜬다(이어하기 / 새로 시작 / 게임 종료, `Tab` 언어). URL에 `Day=`가 있거나 `?NoTitle`, `?StartNear=…`, `-FlightAutoRoute`, 테스트 실행이면 건너뛴다. 처음부터 다시(예전 카운터의 `N` 두 번)는 타이틀의 새로 시작으로 옮겼다. 확인용 `-FlightTitlePreview=confirm`.
- 설정(ZR R3): 타이틀과 `Esc` 메뉴의 설정 — 해상도·화면 모드(화면 적용에서 `Enter`)·수직 동기화, 전체·비행기·음악·환경음 음량, 언어, 수채화. `GameUserSettings.ini`에 남아 새로 시작해도 유지된다.
- 크레딧(ZR R7): 타이틀의 크레딧, 그리고 엔딩의 "끝" 카드 뒤(`E` → 크레딧). 엔딩 크레딧에서는 고른다(2026-10-05, 사용자 결정 — 곧장 다음 날로 넘어가는 게 어색했다): `E`(패드 A) **계속 날기** → "얼마 뒤, 벨로아의 아침", `T`(패드 Y) **타이틀로** → 타이틀의 이어하기가 같은 아침을 연다(엔딩이 이미 그 아침을 저장했다). CSV `ending_credits_closed,keep_flying|title`. 엔딩 뒤 첫 아침은 휴식 URL `AfterEnding`과 세이브 `bAfterEnding`으로 이어지고, 오스카가 시간이 흐른 걸 말하며("왔구나. 그새 바람이 조금 바뀌었어. 오늘도 우편은 있어."), 엔딩 날 노선을 "어제 노선"으로 뒤로 보내지 않는다. 다음 휴식부터는 평소의 엔딩 뒤 아침이다.
- 확인용 `-FlightMenuPreview=main|controls|settings|credits|title|quit`: 2초에 그 화면을 연다. 예: `-benchmark -fps=30 -seconds=8 -FlightShotAt=4 -FlightMenuPreview=quit -FlightLanguage=ko`.

## 자동 검증 재현 (PowerShell)

아래 엔진 경로는 이 작업 환경 기준이다. 에디터를 닫고 저장소 루트에서 실행한다.

```powershell
$project = Join-Path (Get-Location) 'WindCourier/WindCourier.uproject'
$engine = 'C:/Program Files/Epic Games/UE_5.8/Engine'
& "$engine/Build/BatchFiles/Build.bat" WindCourierEditor Win64 Development "-Project=$project" -WaitMutex -NoHotReloadFromIDE
& "$engine/Binaries/Win64/UnrealEditor-Cmd.exe" $project -unattended -nullrhi -nosound '-ExecCmds=Automation RunTests WindCourier.Flight' '-TestExit=Automation Test Queue Empty' "-ReportExportPath=$PWD/WindCourier/Saved/Automation/FirstLoop"
& "$engine/Binaries/Win64/UnrealEditor-Cmd.exe" $project '/Game/Maps/LV_FlightTest' -game -unattended -nullrhi -nosound -benchmark -fps=60 -seconds=30
```

테스트 결과는 로그가 아니라 `Saved/Automation/FirstLoop/index.json`의 `succeeded`·`succeededWithWarnings`·`failed`·`notRun`으로 센다. `-TestExit`는 프로세스를 즉시 끝내서, 0.2초마다 비워지는 로그의 마지막 줄들이 파일에 안 남을 때가 있다(12회 중 1회꼴). 그때 로그에는 마지막 테스트의 `Test Completed`가 없어 실행이 중간에 죽은 것처럼 보이지만, 종료 코드는 0이고 테스트는 모두 끝난 것이다(2026-10-05 확인).

자동 테스트는 투하 경계값·편지 보존·중복 완료 방지, 기류 범위·수명·재생성, 입력 우선순위, 선회 보조 해제, 30/60/120 FPS에서 출력 보조만의 고도 회복을 확인한다. 마지막 명령은 실제 맵을 입력 없이 구동해 CSV 생성과 첫 기류의 적용을 확인한다. 화면·키보드·패드 조작감 검증은 별도 플레이로 수행한다.

배달 통합 테스트 `WindCourier.Flight.DeliverySession`도 위 `WindCourier.Flight` 명령에 포함된다. 조건 밖 투하 → 편지 유지 → 접근권 이탈·재진입 → 활공 중 투하 성공 → 중복 입력 거부 → 출력 보조 고도 회복 → CSV 종료 기록을 확인한다. 회복 기록에는 기류 도움 여부와 재접근 횟수가 포함되며, 투하 성공과 고도 회복 이벤트가 각각 한 번만 저장되는지도 검사한다.

착륙 전달 테스트 `WindCourier.Flight.LandingDelivery`도 같은 명령에 포함된다. 조건 밖 E 거부(이유 표시) → 조건 안 E로 착륙 보조 시작 → 보조 중 E 무시 → 정지 지점 50cm 안 정지 → 지상 비행 입력 무시 → E 1회로 전달 → 장면 중 E 무시 → 이륙 방향 정렬 → 활주 경로 장애물로 대기(10초 동안 제자리) → 장애물 제거 후 재개 → 4초 안 이륙·조작 복귀 → 비행 입력 복귀 → CSV 사건 각 1회를 확인한다.

## 실제 맵 자동 경로 검사
### 새와의 비행 임시 장면 확인

1일차 첫 비행은 우체국 활주로에서 이륙하며 시작하고, 조작이 돌아오는 순간 오른쪽에서 밝은 날개가 있는 임시 새가 다가온다(첫 비행 이륙 오프닝). 방향을 크게 바꾸면 회피, 그대로 비행하면 가벼운 접촉 뒤 이탈로 기록된다. 그 뒤 활성 기류에 **들어가기만 하면**(0.5초) 같은 새와 두 마리가 약 7초간 동행한다(2026-09-25 변경: 이전에는 기류 안 3초 선회). 기류를 벗어나면 중단되고 다음 기류에서 다시 시작한다. 세 번째 장면은 **3막부터**, 동행을 본 뒤 안전 고도(120 m 이상)에서 방향을 크게 바꾸지 않고 활공으로 내려가면 새가 날개에 앉았다 떠난다. 기류 안에서는 시작하지 않는다(상승하면 새가 떠나므로). 마지막 편지를 싣고 있는 동안에는 나오지 않는다(2026-09-25 변경: 이전에는 `?FlightAct3` 테스트 옵션). 각 장면의 시작·완료·중단은 CSV의 `bird_` 사건으로 확인한다. 새는 충돌하지 않는 임시 도형이며, 장면 진행은 휴식·엔딩 저장으로 이어진다(`Birds=n`). **2026-09-25 임시 그림·소리**: 새는 기본 도형 모델(짙은 청회색, 오른쪽 날개의 밝은 무늬)로 보이고, 날갯짓·접촉 스침·짧은 울음이 들린다(`S_BirdWings`·`S_BirdBrush`·`S_BirdCall`, `Scripts/build_flight_av.py`에서 `WC_AV_ONLY=S_BirdWings,S_BirdBrush,S_BirdCall`로 다시 만들 수 있다). 접촉하면 카메라가 약하게 흔들린다. 날개 쉼에서는 위 날개 오른쪽에 다리를 내리고 앉는다. 장면이 끝나면 새가 앞쪽 바깥으로 날아간 뒤 사라진다.


개발 빌드에서 `-FlightRouteCheck`를 명시하면 테스트 조종자가 B 방향 직선으로 첫 기류를 통과한 뒤 B로 활공·투하하고 출력 보조로 200m까지 회복한다. 실제 맵의 지형과 충돌을 사용하며 기체 위치를 순간이동시키지 않는다. 기본 게임과 PIE에서는 자동 조종이 실행되지 않는다. 이 검사는 정해진 경로를 아는 자동 조종이므로 플레이어의 기류 발견 능력이나 조작감을 평가하지 않는다.

```powershell
$project = Join-Path (Get-Location) 'WindCourier/WindCourier.uproject'
$editor = 'C:/Program Files/Epic Games/UE_5.8/Engine/Binaries/Win64/UnrealEditor-Cmd.exe'
foreach ($frameRate in @(30, 60, 120)) {
    $routeLog = "$PWD/WindCourier/Saved/Logs/Route_${frameRate}.log"
    & $editor $project '/Game/Maps/LV_FlightTest' -game -unattended -nullrhi -nosound -benchmark "-fps=$frameRate" -seconds=240 -FlightRouteCheck "-abslog=$routeLog"
    if ($LASTEXITCODE -ne 0 -or !(Select-String -LiteralPath $routeLog -SimpleMatch 'ROUTE_CHECK_PASS' -Quiet)) {
        throw "Route check failed at $frameRate FPS. See $routeLog"
    }
}
```

`-FlightLandingCheck`를 함께 주면(`-seconds=480` 권장) 회복 뒤 C로 비행해 활주로에 맞춰 내려가고, 착륙 보조·전달·재이륙까지 마친 뒤 `LANDING_CHECK_PASS`를 남긴다. 제한 시간은 420초다. `-FlightVisualCheck`를 함께 주면 `06_landing_ready`부터 `10_control_returned`까지 다섯 장을 추가로 저장한다. **기준값(60 FPS, -nullrhi): 2026-10-01부터 223.356초** — C가 에밀의 과수원이 되어 전달 장면이 편지처럼 20초 읽히게 됐다(전에는 8초, 기준 211.343초).

성공하면 로그에 `ROUTE_CHECK_PASS`와 CSV 경로를 남기고 종료한다. 220초 안에 배달과 고도 회복을 마치지 못하면 `ROUTE_CHECK_FAIL` 및 종료 코드 1을 반환한다. 엔진 자체의 240초 종료와 구별하려면 위처럼 성공 표식까지 확인한다. CSV는 `Saved/FlightTelemetry/Automation/`에 저장한다. 화면 렌더링은 끈 검사이므로 HUD·임시 새떼의 시인성을 통과로 판정하지 않는다.

## 엔딩 편지 화면 확인

**2026-09-25 엔딩 순서(시놉시스 8, 사용자 승인)**: 노르바 편지 패널은 편지 초안 그대로 `리브에게.` → 본문 → `- 아빠` → "리브는 편지를 접어 주머니에 넣는다."로 읽힌다. 패널을 닫으면 자동 이륙 뒤 **조작을 돌려주지 않고** 약 8초간 하늘로 천천히 오르는 마지막 장면이 이어진다(비행 HUD 없음). 리브의 대사 "바람이랑 싸우지 마. 어디로 가고 싶은지 물어보고, 가는 길에 얹혀 가." → "……응, 이제 알 것 같아."(격언 3회차), 화면이 밝아지며 "끝" 카드(`바람 배달부` / "편지는 도착하는 데 시간이 걸린다." / `[E]  다음 날 아침으로`). `E`를 누르면 휴식과 같은 방식으로 다음 날 아침 우체국 카운터가 열린다. CSV `ending_finale` → `ending_card` → `ending_card_closed`. 화면 확인용 `-FlightFinalePreview`(노선 선택 직후 마지막 장면 시작, `-FlightShotAt`과 함께). 엔딩한 날의 저녁 장면(격언)은 마지막 장면으로 옮겨 없어졌다.

노르바에서 마지막 편지를 전달하면 18초 동안 중앙 전용 패널에 편지 네 줄을 함께 표시한다. 이때 비행 HUD·목적지 표식·해금 알림은 숨기고 배경을 어둡게 한다. 3초 뒤부터 패널 오른쪽 아래에 `[E]  close`가 뜨고, `E`로 바로 닫고 재이륙으로 넘어간다(CSV `ending_closed,표시_초`). 3초 안의 `E`는 무시된다(`landing_input_ignored`). 누르지 않으면 18초 뒤 자동 재이륙은 기존과 같다. 에디터 Play에서 `Esc`는 Play 종료이니 누르지 않는다. 미리보기는 닫기 안내를 항상 표시한다.

사람 플레이로 엔딩만 빨리 확인하려면 노르바 착륙 직전에서 시작한다. `StartNear=NORVA`는 노선을 고르는 순간 기체를 노르바 활주로 방향 1.2 km 앞, 120 m 위로 옮기는 테스트 전용 옵션이다(CSV `start_near,NORVA`). 실행한 그날에만 적용되고 휴식·엔딩 뒤 다음 날로는 넘어가지 않는다(2026-10-05: 엔딩 뒤 아침이 노르바 상공에서 다시 시작했다). 기존 해금이 엔딩 저장에서 빠지지 않도록 `Seen=`·`Unlocked=`·`Worn=`도 세이브 값으로 넘긴다.

```powershell
& $editor $project '/Game/Maps/LV_FlightTest?Day=5?Act=3?Storm?Letters=NORVA:aboard?StartNear=NORVA' -game -windowed -ResX=1600 -ResY=900
```

영상 클립은 `-FlightRecordAt=T -FlightRecordFrames=N`으로 찍는다: T초부터 N프레임을 `Saved/Screenshots/WindowsEditor/MovieFrame*.png`로 저장한다(엔진 `r.DumpingMovie`). `-benchmark -fps=30`과 함께 쓰면 매번 같은 비행이라 시각을 미리 정할 수 있다. 녹화·mp4 변환은 `Scripts/video_record_clip.ps1`, 묶음은 `video_rating_clips.ps1`(등급 신청 영상)·`video_trailer_clips.ps1` + `video_trailer.py`(트레일러), 2026-10-07.

1막 귀환 선택 창은 `-FlightAct1ChoicePreview`로 확인한다: 시험비행을 고른 뒤 2초에 선택 창을 연다(`?Day=3?Act=1?Done=local,coast -FlightAutoRoute=pass -FlightAct1ChoicePreview -FlightShotAt=6`). 저녁 장면은 `?Day=2?Act=1?Done=local -FlightAutoRoute=coast -FlightSettlePreview`(해안 첫 저녁), 마을 변화 문구는 `?Day=6?Act=2?Storm -FlightAutoRoute`(출발 직후 벨로아 문구)로 띄운다(2026-10-07).

투하 거부 배너 화면은 `-FlightRejectPreview`로 확인한다. 노선을 자동으로 고르고 우체국 북서쪽 1.2 km·150 m에서 `E`를 한 번 누른 뒤 `Saved/Screenshots/RejectReview/reject_WxH.png`로 찍는다. `Day=`를 주면 저장을 읽지 않는다.

```powershell
& $editor $project '/Game/Maps/LV_FlightTest?Day=6?Act=3?Storm?Ended' -game -unattended -RenderOffscreen -windowed -ResX=1280 -ResY=720 -nosound -benchmark -fps=30 -seconds=6 -FlightRejectPreview
```

비행 없이 화면 배치만 확인하려면 다음 명령을 실행한다. 저장을 읽지 않는 자동화 세션이며, 실제 전달·엔딩을 발생시키는 검사는 아니다. `-seconds=5`는 미리보기 실행 시간이고 실제 엔딩 표시 시간과 무관하다.

```powershell
& $editor $project '/Game/Maps/LV_FlightTest?FlightAutomation?Day=5?Act=3' -game -unattended -RenderOffscreen -windowed -ResX=1280 -ResY=720 -nosound -benchmark -fps=30 -seconds=5 -FlightEndingPreview
```

`$editor`·`$project`는 아래 자동 검증 명령과 같은 엔진 실행 파일·프로젝트 절대 경로다. 캡처는 `Saved/Screenshots/EndingReview/ending_1280x720.png`, CSV는 `Saved/FlightTelemetry/Automation/`에 남는다. `-ResX=960 -ResY=540`으로 작은 화면도 확인할 수 있다. 미리보기는 실제 전달을 거치지 않으므로 저장을 쓰지 않는다.

## 화면 캡처 검증

`-FlightRouteCheck -FlightVisualCheck`를 함께 주고 `-nullrhi` 대신 `-RenderOffscreen`으로 실행하면 GPU로 비행 화면을 렌더링해 저장한다. 새 창을 열지 않고 출발·기류 통과·투하 가능·투하 0.8초 뒤(소포, `03b_parcel`)·고도 회복 화면을 캡처한다.

```powershell
& $editor $project '/Game/Maps/LV_FlightTest' -game -unattended -RenderOffscreen -windowed -ResX=1280 -ResY=720 -nosound -benchmark -fps=30 -seconds=240 -FlightRouteCheck -FlightVisualCheck
```

이미지는 `Saved/Screenshots/FlightReview/01_departure.png`부터 `04_recovered.png`까지 저장된다. 다시 실행하면 같은 이름의 이미지를 갱신한다. `-FlightCleanHUD`를 추가하면 처음부터 정밀 진단 수치를 숨기며 이미지는 `FlightReviewClean/`에 저장한다. 실제 플레이에서는 F1로 같은 표시를 전환할 수 있다. 투하 가능 상태를 찍기 위해 시각 검사에서는 투하 입력을 한 프레임 늦춘다.

HUD는 밝은 배경에서도 읽을 수 있도록 반투명 바탕을 사용하고, 투하 가능 시 화면 아래 중앙에 별도 입력 안내를 표시한다. 완료 뒤에는 지난 목적지 방향 대신 완료 상태를 표시한다. 새떼는 어두운 임시 선 실루엣이며 소멸 예고 시 갈색·주황 계열로 바뀐다. 최종 아트·소리와 사람의 기류 발견 성공률은 별도로 검증한다.

## 구름 표현 수정 (2026-09-26)

`Clouds.txt`는 `x_m y_m radius_m height_m yaw_deg variant`의 6열이다. 화면 배치와 구름 진입 판정이 이 목록을 함께 읽는다. 밑면은 400 m(2026-10-09, 500 m에서 낮춤 — `FlightClouds::BaseCm`·`build_flight_test_level.py`의 `z`·기류 줄기 상단이 함께 바뀐다)이며 구름마다 높이·폭·방향이 다르다. 따뜻한 흰색 윗면과 청회색 아랫면, 부드러운 외곽 마스크를 사용한다. 이전의 원거리 소멸과 별도 볼류메트릭 구름층은 제거했다. 스텐실 17로 구름의 진한 수채화 외곽선을 제외한다. 진입 판정은 개별 돌출부까지 재현하지 않는 타원 돔 근사다.

기존 맵의 구름과 수채화 재질을 갱신하려면 에디터를 닫고 `Scripts/update_clouds.py`를 언리얼의 `-ExecutePythonScript`로 실행한다. 전체 맵 생성 스크립트에도 같은 설정이 반영되어 있다. 자동 검증: `WindCourier.Flight.Cloud`.

## 리브·오스카 모델 (2026-09-26)

`GDD/characters/Liv/Liv_H/Liv_H.blend`와 `GDD/characters/Oskar/Oskar_H/Oskar_H.blend`를 텍스처와 함께 적용했다. 오스카는 우체국 현관 앞(`Oskar_PostOffice`), 리브는 Jenny·DH-4·Mailwing의 조종석에 배치된다. 키는 적용 기준으로 각각 178 cm·165 cm이며, 기체별 좌석 위치에 맞췄다. 스카프 연결점은 리브의 목과 함께 기울어진다.

원본은 저장하거나 변경하지 않는다. 재적용은 Blender 백그라운드에서 `Scripts/export_h_characters.py` 실행 → 에디터를 닫고 언리얼에서 `Scripts/import_h_characters.py` 실행 순서다. 에셋은 `/Game/FlightTest/Characters/OskarH`·`LivH`에 저장한다. 두 원본은 뼈대 없는 메시이므로 현재는 정적 모델이며, 리브 착석 메시를 별도로 생성했다. 걷기·표정·조종 동작 애니메이션은 포함하지 않는다.
