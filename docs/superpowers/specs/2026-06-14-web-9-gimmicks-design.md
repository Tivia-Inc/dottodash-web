# dottodash.today — v2.1 (9-gimmick) 웹 업데이트 설계

작성 2026-06-14. 대상: [`index.html`](../../../index.html) 단일 파일 랜딩(EN/KO/JA 토글).
앱이 완수 방식 **5 → 9개**로 확장(v2.1.0)됨에 따라 웹을 갱신한다.

## 소스
- **Pencil 9기믹 시트** (`pencil-new.pen`): `EMh1v`(KO) · `rErw2`(EN) · `NRstI`(JA)
  = "어떻게 인증할까요? / How will you prove it? / どうやって証明する？" 바텀시트.
  아이콘 뱃지(`#EAF2FF`) + 이름 + 한 줄 설명. 9개 모드의 3개 언어 카피를 여기서 그대로 가져온다.
- **App Store v2.1 카피**: `dottodash/docs/APP_STORE_v2.1_COPY.md` (3카테고리 분류, ATT 제거).

## 결정 (사용자 승인)
1. modes 섹션 = **3카테고리 그룹** (Knock out / Lock in / Leave a mark), 신규 4개에 `NEW` 핀.
2. 모드 설명 = **인앱 펀치 카피** (Pencil 시트 문구 그대로).
3. 새 시트 이미지 = **Pencil export**로 기존 `v2-modes-*.png` 덮어쓰기.

---

## 9개 완수 방식 데이터 (구현 시 그대로 사용)

| 그룹 | 이모지 | EN | KO | JA | NEW |
|---|---|---|---|---|---|
| **Knock out** / 해치우기 / やっつける | 🔢 | Count | 횟수 | 回数 | |
| | ✊ | Hold | 길게 | 長押し | |
| | 📳 | Shake | 흔들기 | シェイク | ✅ |
| **Lock in** / 몰입하기 / 没頭する | ⏱️ | Timer | 타이머 | タイマー | |
| | 🎯 | Stay | 집중 | 集中 | |
| | 🔄 | Flip | 뒤집기 | 裏返し | ✅ |
| **Leave a mark** / 남기기 / 残す | 📷 | Photo | 사진 | 写真 | |
| | ✍️ | Sign | 서명 | サイン | ✅ |
| | ⌨️ | Type | 타이핑 | タイピング | ✅ |

설명 문구 (Pencil 그대로):

| 모드 | EN | KO | JA |
|---|---|---|---|
| Count | One tap at a time — count it out | 하나씩 세면서, 정한 만큼 채워요 | 数えながらタップして消そう |
| Hold | Press and hold — done in one go | 미루던 일도 꾹 누르면 한 방에 | ぐっと長押しで一気に片付け |
| Shake | Shake it off till it's gone | 흔들어야 후련하게 사라져요 | 振ってスッキリ消し去ろう |
| Timer | Brush for 2 min — just fill the time | 양치 3분, 시간을 채워야 끝! | 歯みがき2分、時間を満たすまで！ |
| Stay | Look away and it's back to square one | 한눈팔면 처음부터 다시 | よそ見したら最初からやり直し |
| Flip | Flip it face-down for a detox | 디지털 디톡스가 필요할 때 | デジタルデトックスしたいときに |
| Photo | Pics or it didn't happen | 촬영하고, 인증해야 진짜 끝나요 | 撮って記録、それで本当に完了 |
| Sign | Sign it, seal the deal | 사인 한 번으로 굳히는 다짐 | サイン一つで決意を固める |
| Type | Type it out, word for word | 또박또박 적어서 완료해요 | 一字ずつ打ち込んで完了 |

`NEW` 핀 텍스트는 3개 언어 공통 "NEW" (앱의 NEW 뱃지와 통일).

---

## 변경 ① `#modes` 섹션 (마크업 + CSS)

기존 2단 레이아웃(`.modes`: 왼쪽 `.modeshot`, 오른쪽 `.modelist`) 유지.
오른쪽을 **3개 카테고리 그룹 × 3행** = 9행으로 교체.

### 마크업
- `.modelist` 내부를 카테고리별 그룹으로 재구성. 각 그룹: 카테고리 라벨(`.modecat`) + `.mode` 행 3개.
- `.mode` 행 구조는 기존 그대로 (`.mi` 이모지 뱃지 + `h4` 이름 + `p` 설명). 위 표의 이모지/이름/설명을 `data-en/ko/ja`로.
- 신규 4개(Shake/Flip/Sign/Type)의 `h4`에 `<span class="newpill">NEW</span>` 추가.
- 카테고리 라벨도 `data-en/ko/ja` 다국어 (Knock out / 해치우기 / やっつける 등).

### CSS 추가
```css
.modecat{font-size:11px;font-weight:700;letter-spacing:.06em;text-transform:uppercase;
  color:var(--accent);padding:16px 6px 6px}
.modegroup:first-child .modecat{padding-top:0}
.newpill{display:inline-flex;align-items:center;background:var(--tint);color:var(--accent);
  font-size:10px;font-weight:700;letter-spacing:.04em;padding:2px 7px;border-radius:9999px;
  margin-left:8px;vertical-align:middle}
```
- `.modelist`는 `.modegroup`(vertical) 3개를 자식으로. 각 `.modegroup` 안에 `.modecat` + `.mode`×3.
- `.mode`의 기존 `border-top` 구분선은 유지(Pencil 시트의 행 구분선과 동일한 느낌).
  `.modegroup .mode:first-child` 가 라벨 다음에 와도 구분선이 어색하지 않으므로 별도 처리 불필요;
  필요 시 `.modegroup .mode:first-of-type{border-top:none}`로 그룹 첫 행 구분선 제거.

### shead 카피 (9 반영)
- `lead` 유지: Completion modes / 완료 방식 / 完了モード
- `h2` 유지: One tap won't finish it. / 한 번의 탭으로는 끝나지 않아요. / ワンタップでは終わりません。
- `ssub`에 "9가지" 뉘앙스 한 구절 추가:
  - EN: `Give every task a small mission — it's done only when you actually pull it off. Now in nine ways.`
  - KO: `할 일마다 작은 미션을 붙이세요. 그대로 해내야만 완료돼요. 이제 아홉 가지 방식으로.`
  - JA: `タスクごとに小さなミッションを。そのとおりにこなして、はじめて完了です。完了の方法は9つに。`

---

## 변경 ② 새 시트 이미지 (Pencil export)

- Pencil `export_nodes`로 `EMh1v`(KO)·`rErw2`(EN)·`NRstI`(JA)를 PNG(scale 4) export → 임시 폴더.
- 파일명을 노드ID에서 → `assets/v2-modes-ko.png` / `v2-modes-en.png` / `v2-modes-ja.png`로 이동·덮어쓰기.
- `index.html`의 `data-{lang}-src` 스왑 그대로라 코드 변경 없음. 캐시 무력화 위해 `?v=1` → `?v=2`.
- 프레임은 393×658(세로 시트). `.shot.sheet{width:300px}` 그대로 표시 OK.

---

## 변경 ③ "5/five" → "9" + ATT 문구 (3개 언어)

| 위치 | Before → After |
|---|---|
| `#pro` `ssub` | "Pro adds the **five** completion modes…" → "Pro adds all **nine** completion modes…" / KO "5가지 방식" → "9가지 방식" / JA "5つのモード" → "9つのモード" |
| Pro 플랜 모드 항목 | EN "All **5** completion modes (count · timer · hold · photo · stay)" → "**All 9 completion modes — Knock out, Lock in & Leave a mark**" / KO "완료 방식 **5종** (…)" → "완료 방식 **9종 — 해치우기 · 몰입하기 · 남기기**" / JA "完了モード**5種**（…）" → "完了モード**9種 — やっつける・没頭する・残す**" |
| FAQ "What are completion modes?" | EN "**Five** ways… count, timer, hold, photo, and stay." → "**Nine** ways to finish a task — across three families: Knock out (count, hold, shake), Lock in (timer, stay, flip), and Leave a mark (photo, sign, type). Each asks for a real action, so a check is hard to fake. They're part of Pro." / KO·JA 동일 구조로 |
| FAQ "Where is my data stored?" ⚠️ | "…Google AdMob, **and a tracking-permission prompt appears at first launch**; Pro removes the ads." → 추적 동의 문장 **삭제**: "…Google AdMob; Pro removes the ads." (3개 언어 모두. v2.1에서 ATT 제거 — 현재 사실 오류) |
| `band` `trynote` | EN "* And new ways to complete keep coming." → "* **Nine ways to complete now — and more coming.**" / KO "* 이제 완료 방식이 **9가지** — 앞으로도 계속 늘어나요." / JA "* 完了の方法はいま**9つ** — これからも増えていきます。" |

FAQ KO 신규: "할 일을 끝내는 **아홉 가지** 방식이에요 — 세 갈래로: 해치우기(횟수·길게·흔들기), 몰입하기(타이머·집중·뒤집기), 남기기(사진·서명·타이핑). 각 방식이 진짜 행동을 요구해서 체크를 가짜로 만들기 어려워요. Pro에 포함돼요."
FAQ JA 신규: "タスクを終える**9つ**の方法です — 3つのグループに：やっつける（回数・長押し・シェイク）、没頭する（タイマー・集中・裏返し）、残す（写真・サイン・タイピング）。どれも本物の行動を求めるので、チェックをごまかしにくくなります。Proに含まれます。"

---

## 변경 ④ 앱 아이콘 4종 (Pro 가치)

- Pro 플랜 테마 항목: EN "**6 accent themes**" → "**6 themes + 4 app icons**" / KO "6가지 테마" → "테마 6가지 + 앱 아이콘 4종" / JA "6つのテーマ" → "6つのテーマ + アプリアイコン4種"
- 테마 섹션(`#themes`) 본문은 유지(별도 섹션·이미지 추가 안 함).

---

## 범위 밖 (이번에 건드리지 않음)
- hero 헤드라인 / `<meta>` description — 현 문구 유효, 유지.
- 가격·구독·플랜 구조, 테마 스와치, 트리 다이어그램, 아카이브 섹션.
- "completion modes" → "finish modes" 명칭 통일은 보류(현 명칭 유지).

## 검증
- `flutter` 무관(순수 정적 HTML). 로컬에서 `index.html`을 브라우저로 열어 EN/KO/JA 토글 시:
  - modes 섹션 9개 + 카테고리 라벨 + NEW 핀 정상 노출, 모바일(<760px) 1단 폴백 정상.
  - 새 시트 이미지가 언어별로 스왑되는지.
  - "5/five"·ATT 잔존 문자열 0건 (`grep -i "five\|5 completion\|tracking-permission\|추적 동의\|5가지\|5つ\|5種"`).
- 작업은 `web-9-gimmicks` 브랜치에서. `main`(배포) 직접 변경 금지.

---

## 개정 (2026-06-14, 구현 중 사용자 피드백)
- **변경 ① 재설계**: 좌우 2단(시트 이미지 + 9행 리스트)이 같은 9개를 두 번 노출 → 중복.
  시트 이미지를 제거하고 **카테고리 3컬럼 그리드**(`.modecols` / `.modegroup`)로 전환:
  PC=가로 3열(해치우기·몰입하기·남기기), 모바일(<760px)=세로 1열로 3개씩 묶여 스택.
- **변경 ② 보류**: 9기믹 시트 이미지(`v2-modes-*.png`)는 더 이상 웹에서 사용하지 않아 제거.
  (Pencil `EMh1v/rErw2/NRstI` 원본은 그대로 보존되어 필요 시 재활용 가능.)
