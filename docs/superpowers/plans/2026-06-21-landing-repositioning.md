# 랜딩 리포지셔닝 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** `dottodash-web/index.html`의 정문 포지셔닝을 "ADHD를 위한 투두"에서 "결심만 하고 안 하는 사람을 위한, 안 하면 안 지워지는 실행 도구"로 바꾸고, ADHD는 SEO/세컨 섹션으로 강등한다.

**Architecture:** 단일 정적 HTML 파일의 카피·메타·섹션 순서 편집. 모든 가시 텍스트는 `data-en`/`data-ko`/`data-ja` 속성 + 자식 텍스트(현재 표시 로케일)로 구성된 트리링구얼 패턴. 편집은 **정확한 문자열 앵커 기반 Edit**으로 수행(라인 시프트 무관). 앱(Flutter)·기능·가격 변경 없음.

**Tech Stack:** 순수 HTML/CSS/JS(빌드 단계 없음). 검증 = 브라우저 수동 확인 + grep. **자동 테스트 하니스 없음** → 각 태스크의 "verify"는 (a) `open index.html`로 EN/KO/JP 토글 시 렌더 확인, (b) 편집 요소에 3개 로케일 속성이 모두 있는지 grep.

**커밋 정책:** 커밋 스텝은 표준 형식상 포함했으나, 실제 커밋/푸시는 사용자 승인 후 실행한다(이 레포 기본 규칙).

**스펙:** `docs/superpowers/specs/2026-06-21-landing-repositioning-design.md`

---

## File Structure

- Modify: `dottodash-web/index.html` (유일한 편집 대상)
  - `<head>` 메타(6,7,11,12) — title/description/OG
  - 히어로(275,276) — eyebrow/h1
  - `#surge`(285–321) — ADHD 급증 그래프 → 하단(가격 앞)으로 이동 + 리드 재명명
  - 신규 `#resolve` 섹션 — `#proof` 앞에 삽입(만인 공감 문제)
  - `#adhd`(354–365) — "fit your brain" → 행동 중심으로 탈-ADHD
  - `#record`(395–405) — Dot Matrix 헤드라인을 자기신뢰로

---

## Task 1: 히어로 eyebrow + H1 교체

**Files:**
- Modify: `index.html:275-276`

- [ ] **Step 1: eyebrow를 청중 라인으로 교체**

old_string:
```html
    <div class="eyebrow" data-en="Stop fake-checking." data-ko="가짜 체크는 그만." data-ja="偽のチェックは、もう。">Stop fake-checking.</div>
```
new_string:
```html
    <div class="eyebrow" data-en="For people who never follow through" data-ko="결심만 하고 안 하는 사람을 위한" data-ja="決めても動けない人のための">For people who never follow through</div>
```
> "Stop fake-checking"은 폐기가 아니라 강등 — 클로징(502 "No fake checks") + FAQ에 이미 메아리가 있으므로 별도 재배치 불필요.

- [ ] **Step 2: H1을 명사 없는 동사형으로 교체**

old_string:
```html
    <h1 data-en="A todo app for ADHD &amp; procrastinating brains" data-ko="ADHD·미루는 뇌를 위한<br>투두 앱" data-ja="ADHD・先延ばし脳のための<br>ToDoアプリ">A todo app for ADHD &amp; procrastinating brains</h1>
```
new_string:
```html
    <h1 data-en="Won't cross off until you actually do it." data-ko="안 하면, 안 지워져요." data-ja="やらなきゃ、消えない。">Won't cross off until you actually do it.</h1>
```
> 서브(277 "탭 한 번이면 완료 — … 흔들고·꾹 누르고·사진을 찍어야 지워져요")는 의도적으로 그대로 둠 — 이미 메커니즘을 설명하고 H1의 "무엇인지"를 받쳐준다.

- [ ] **Step 3: Verify**

Run: `open index.html` → 우상단 EN/KO/JP 토글하며 히어로 3개 로케일 렌더 확인. H1 줄바꿈 깨짐 없는지(이제 `<br>` 없음).
Also run: `grep -n 'data-en="Won' index.html` → 1줄 매치(h1).

- [ ] **Step 4: Commit**
```bash
git add index.html
git commit -m "feat(landing): hero — audience-first eyebrow + 'won't cross off until you do it' H1"
```

---

## Task 2: title / meta description / OG (탈-ADHD 정문, ADHD는 세컨 키워드)

**Files:**
- Modify: `index.html:6-7,11-12`

- [ ] **Step 1: `<title>` 교체**

old_string:
```html
<title>dottodash — a todo app for ADHD brains</title>
```
new_string:
```html
<title>dottodash — the to-do list you can't fake</title>
```

- [ ] **Step 2: meta description 교체(ADHD를 괄호 세컨 키워드로 보존)**

old_string:
```html
<meta name="description" content="A todo for ADHD &amp; procrastinating brains — shake, hold, or photograph to finish a task. A check you can't fake, so you actually do it. Free, with Pro. iPhone &amp; iPad.">
```
new_string:
```html
<meta name="description" content="A to-do list you can only check off by actually doing it — shake, hold, or photograph to finish a task. For anyone who plans but never follows through (ADHD included). Free, with Pro. iPhone &amp; iPad.">
```

- [ ] **Step 3: og:title 교체**

old_string:
```html
<meta property="og:title" content="dottodash — a todo app for ADHD brains">
```
new_string:
```html
<meta property="og:title" content="dottodash — the to-do list you can't fake">
```

- [ ] **Step 4: og:description 교체**

old_string:
```html
<meta property="og:description" content="A todo for ADHD &amp; procrastinating brains — shake, hold, or photograph to finish a task. A check you can't fake, so you actually do it. Free, with Pro. iPhone &amp; iPad.">
```
new_string:
```html
<meta property="og:description" content="A to-do list you can only check off by actually doing it — shake, hold, or photograph to finish a task. For anyone who plans but never follows through (ADHD included). Free, with Pro. iPhone &amp; iPad.">
```

- [ ] **Step 5: Verify**

Run: `grep -nE '<title>|name="description"|og:title|og:description' index.html`
Expected: 4줄 모두 "can't fake" 정문 + description에 "ADHD included" 잔류.

- [ ] **Step 6: Commit**
```bash
git add index.html
git commit -m "feat(landing): de-ADHD the title/meta/OG, keep ADHD as a secondary keyword"
```

---

## Task 3: `#surge`(ADHD 급증) 섹션을 가격 앞으로 이동 + 리드 재명명(강등)

**Files:**
- Modify: `index.html` — `#surge` 블록(현재 285–321)을 `#pro`(현재 467) 바로 앞으로 이동

> 이동은 SVG를 플랜에 복제하지 않기 위해 **런타임 캡처 방식**으로 한다: 파일에서 블록을 읽어 그 텍스트를 그대로 잘라 붙인다. 내용은 한 글자도 바꾸지 않는다(리드만 Step 4에서 재명명).

- [ ] **Step 1: 이동할 블록 캡처**

Run: `sed -n '285,321p' index.html` 로 `<section class="sect wrap" id="surge"> … </section>` 전체를 확인·복사한다. (라인 번호는 이전 태스크로 이동했을 수 있으니, 앵커는 `<section class="sect wrap" id="surge">`부터 그 다음 `</section>`까지.)

- [ ] **Step 2: 원위치에서 블록 제거**

Edit: old_string = Step 1에서 캡처한 `#surge` 섹션 전체(여는 `<section class="sect wrap" id="surge">`부터 닫는 `</section>`까지), new_string = `` (빈 문자열). 제거 후 남는 빈 줄은 정리.

- [ ] **Step 3: `#pro` 앞에 블록 삽입**

Edit: old_string =
```html
<section class="sect wrap" id="pro">
```
new_string = Step 1에서 캡처한 `#surge` 섹션 전체 + 빈 줄 1개 + `<section class="sect wrap" id="pro">`

- [ ] **Step 4: 리드 재명명("폭증 중" → "ADHD에게도 딱")**

old_string:
```html
    <div class="lead" data-en="Surging right now" data-ko="지금, 폭증 중" data-ja="いま、急増中">Surging right now</div>
```
new_string:
```html
    <div class="lead" data-en="Made for ADHD too" data-ko="ADHD에게도 딱" data-ja="ADHDにもぴったり">Made for ADHD too</div>
```
> h2(+385%)·도넛(85%)·HIRA 출처는 그대로 — 이제 강등된 ADHD 전용 섹션의 근거로 기능. SEO 키워드 보존.

- [ ] **Step 5: Verify**

Run: `grep -n 'id="surge"\|id="proof"\|id="record"\|id="surge"\|id="pro"' index.html`
Expected 순서: `#proof` < `#record` < … < `#surge` < `#pro` (surge가 record/themes 뒤, 가격 앞).
Also: `open index.html` → surge 그래프가 페이지 하단(가격 직전)에 렌더, 리드가 "ADHD에게도 딱".

- [ ] **Step 6: Commit**
```bash
git add index.html
git commit -m "refactor(landing): demote ADHD surge section above pricing, retitle as 'made for ADHD too'"
```

---

## Task 4: 신규 `#resolve`(만인 공감 문제) 섹션을 `#proof` 앞에 삽입

**Files:**
- Modify: `index.html` — `#proof` 섹션 바로 앞에 신규 섹션 삽입

> 정직성 유지를 위해 **하드 수치(출처 미검증) 없이** 관용적 카피로 작성. 검증 가능한 작심삼일 통계가 확보되면 ssub에 한 줄로 끼워 넣을 수 있음(스펙 §7.1). 기존 `.sect.wrap`/`.shead`/`.lead`/`.ssub` 클래스 패턴 재사용.

- [ ] **Step 1: 신규 섹션 삽입**

Edit: old_string =
```html
<section class="sect wrap" id="proof">
```
new_string =
```html
<section class="sect wrap" id="resolve">
  <div class="shead reveal">
    <div class="lead" data-en="The real problem" data-ko="진짜 문제" data-ja="本当の問題">The real problem</div>
    <h2 data-en="Deciding is easy. <span class='nw'>Following through isn't.</span>" data-ko="<span class='nw'>결심은 쉬워요.</span> <span class='nw'>끝까지 가는 게 어렵죠.</span>" data-ja="<span class='nw'>決めるのは簡単。</span><span class='nw'>やり遂げるのが難しい。</span>">Deciding is easy. <span class="nw">Following through isn't.</span></h2>
    <p class="ssub" data-en="You write the list and you mean it — then most of it quietly slides to tomorrow. The gap isn't motivation. It's the moment of actually moving." data-ko="목록은 적었고 마음도 먹었죠 — 근데 대부분은 슬그머니 내일로 밀려요. 문제는 의지가 아니라, '진짜 움직이는 순간'이에요." data-ja="リストは書いたし、やる気もあった — でも大半はそっと明日へ。問題はやる気じゃなく、『実際に動く瞬間』です。">You write the list and you mean it — then most of it quietly slides to tomorrow. The gap isn't motivation. It's the moment of actually moving.</p>
  </div>
</section>

<section class="sect wrap" id="proof">
```

- [ ] **Step 2: Verify**

Run: `grep -n 'id="resolve"\|id="proof"' index.html`
Expected: `#resolve`가 `#proof` 바로 앞(히어로 다음 첫 콘텐츠 섹션).
Also: `open index.html` → EN/KO/JP 토글 시 "결심은 쉬워요 / Deciding is easy" 정상 렌더, `.nw` 줄바꿈 자연스러움. 흐름: 문제(resolve) → 증명(proof, 1.8배)로 자연 연결.

- [ ] **Step 3: Commit**
```bash
git add index.html
git commit -m "feat(landing): add universal 'follow-through' problem section before the proof"
```

---

## Task 5: `#adhd` 디자인 섹션 탈-ADHD

**Files:**
- Modify: `index.html:357,360`

- [ ] **Step 1: 섹션 h2를 행동 중심으로**

old_string:
```html
    <h2 data-en="So we built it to fit your brain." data-ko="그래서, 뇌에 맞게 설계했어요." data-ja="だから、脳に合わせて設計しました。">So we built it to fit your brain.</h2>
```
new_string:
```html
    <h2 data-en="So we built it so the body moves first." data-ko="그래서, 행동이 먼저 나오게 설계했어요." data-ja="だから、体が先に動くよう設計しました。">So we built it so the body moves first.</h2>
```
> 리드(356 "It's not about willpower / 의지 문제가 아니에요")는 일반 카피라 유지. id="adhd"는 앵커/SEO상 그대로 둠(가시 텍스트 아님).

- [ ] **Step 2: "Just get started" 카드 KO에서 ADHD 제거**

old_string:
```html
data-ko="흔들고·누르고·찍는 작은 행동이 '생각'을 건너뛰고 몸을 먼저 움직여요. ADHD가 가장 막히는 '시작'을, 고민 없이."
```
new_string:
```html
data-ko="흔들고·누르고·찍는 작은 행동이 '생각'을 건너뛰고 몸을 먼저 움직여요. 누구나 가장 막히는 '시작'을, 고민 없이."
```
> EN/JA(360)에는 ADHD 언급이 없으므로 손대지 않는다.

- [ ] **Step 3: Verify**

Run: `grep -n "ADHD가 가장 막히는" index.html` → 0 매치(제거 확인).
Run: `grep -c 'fit your brain' index.html` → 0 매치.
Also: `open index.html` → KO에서 해당 카드가 "누구나 가장 막히는 '시작'"으로 렌더.

- [ ] **Step 4: Commit**
```bash
git add index.html
git commit -m "polish(landing): generalize the design section off the ADHD frame"
```

---

## Task 6: `#record` 헤드라인을 자기신뢰로

**Files:**
- Modify: `index.html:399`

- [ ] **Step 1: Dot Matrix h2를 "내 말을 지킨 증거"로**

old_string:
```html
    <h2 data-en="Only the days you showed up stay on the record." data-ko="해낸 날만, 기록에 남아요." data-ja="やり遂げた日だけが、記録に残ります。">Only the days you showed up stay on the record.</h2>
```
new_string:
```html
    <h2 data-en="Proof you actually kept your word." data-ko="내가 한 말을, 내가 지켰다는 증거." data-ja="自分との約束を、守った証。">Proof you actually kept your word.</h2>
```
> 리드(398 "정직한 기록")·ssub(400 "끝낸 일 하나가 점 하나…자동 채움도, 소급도 없이")는 그대로 — 이제 자기신뢰 헤드라인의 근거로 작동. (추후 ssub 끝에 "그렇게 조금씩, 나를 다시 믿게 돼요" 한 줄 추가 옵션.)

- [ ] **Step 2: Verify**

Run: `grep -n "지켰다는 증거" index.html` → 1 매치.
Also: `open index.html` → `#record` 섹션 헤드라인이 3개 로케일로 자기신뢰 카피 렌더.

- [ ] **Step 3: Commit**
```bash
git add index.html
git commit -m "feat(landing): reframe the Dot Matrix as proof you kept your word (self-trust)"
```

---

## Task 7: 최종 QA (로케일 완전성 + 3개 언어 통독)

**Files:**
- Verify only: `index.html`

- [ ] **Step 1: 섹션 순서 확인**

Run: `grep -nE 'id="(resolve|proof|adhd|modes|record|surge|pro|faq)"' index.html`
Expected 순서: `resolve` → `proof` → `adhd` → `modes` → `record` → (sync/themes) → `surge` → `pro` → … → `faq`.

- [ ] **Step 2: 트리링구얼 완전성 스폿체크**

Run: `grep -cE 'data-en=' index.html; grep -cE 'data-ko=' index.html; grep -cE 'data-ja=' index.html`
Expected: 세 카운트가 (거의) 동일 — 신규/편집 요소가 3개 로케일 모두 보유. 차이가 있으면 누락 요소를 찾아 보완.

- [ ] **Step 3: 정문에 ADHD 라벨 없음 확인**

Run: `grep -nE 'ADHD' index.html | head` → ADHD 언급은 (a) 강등 `#surge` 섹션, (b) meta description 괄호 세컨 키워드, (c) FAQ에만 존재. 히어로/title/og:title에는 없어야 함.

- [ ] **Step 4: 3개 언어 브라우저 통독**

`open index.html` → EN/KO/JP 각각 위→아래 1회 통독. 깨진 `<br>`/`.nw`/잘린 문장 없는지. 흐름: 히어로(안 하면 안 지워져요) → 문제(작심삼일) → 증명(1.8배) → 인사이트(체크는 쉬워서 속인다) → 설계(행동 먼저) → 9모드 → Dot Matrix(자기신뢰) → 동기화 → ADHD에게도 딱 → 가격 → 클로징.

- [ ] **Step 5: 최종 커밋(있다면)**
```bash
git add index.html
git commit -m "chore(landing): final QA pass — locale completeness + section order"
```

---

## Self-Review (작성자 체크 완료)

- **Spec coverage:** §3 히어로→T1 · §5 메타→T2 · §4②신규문제→T4 · §4⑧ADHD강등→T3 · §4⑤탈-ADHD설계→T5 · §4⑥자기신뢰→T6 · §4④(1.8배)·⑦동기화·⑨가격·⑩클로징·⑪FAQ는 변경 없음(유지). 모든 변경 요구가 태스크에 매핑됨.
- **Placeholder scan:** 모든 스텝에 실제 old/new 문자열·명령·기대결과 포함. "TBD/TODO" 없음. §7.1 미검증 통계는 T4에서 하드 수치 없는 카피로 회피(플레이스홀더 아님).
- **Consistency:** 클래스(`sect wrap`/`shead`/`lead`/`ssub`/`nw`)·id(`resolve` 신규, `surge`/`proof`/`pro`/`record`/`adhd` 기존) 일관. 신규 `#resolve`는 기존 섹션 패턴과 동일 구조.
