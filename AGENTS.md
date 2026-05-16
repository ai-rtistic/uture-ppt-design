# AGENTS.md

## 0. 기본 작업 지침 (필수 선행)

**모든 작업을 시작하기 전에 반드시 [docs/system-prompt.md](docs/system-prompt.md)를 먼저 읽고, 그 내용에 따라 작업한다.**

- `docs/system-prompt.md`는 이 프로젝트의 역할 정의, 워크플로우, UTURE 프로젝트 컨벤션(하드 룰), 출력 형식, 디자인 시스템 경로 등 핵심 규칙을 담고 있다.
- `docs/system-prompt.md`의 내용은 본 AGENTS.md의 일반 지침보다 우선한다. 충돌 시 `docs/system-prompt.md`를 따른다.
- 디자인 시스템은 `./uture-design-system`을, 결과물은 `./results` 폴더를 사용한다.
- UTURE 하드 룰(분량 정보 표기 금지, 강사 표기 규칙, 워드마크 절제 사용 등)을 항상 준수한다.

## 0-1. 시각 검증 루프 (산출물 인계 전 필수)

`./results/`에 HTML 산출물을 작성한 뒤, 사용자에게 인계하기 전에 아래 루프를 수행한다.

1. **렌더 & 캡처** — 슬라이드를 PNG로 캡처한다. (webapp-testing 스킬 / Playwright / 헤드리스 브라우저 중 가용한 도구 사용)
2. **검증 호출** — `visual-verifier` 서브에이전트를 호출한다. 입력으로 HTML 경로, 캡처 PNG 경로, (선택) 집중 검토 포인트를 전달한다.
3. **판정 처리**
   - `PASS` → 사용자에게 인계.
   - `FAIL` → `Required Fixes`만 반영해 수정 → 1번부터 다시. **최대 3회**까지 반복.
   - 3회를 넘어가도 `FAIL`이면 자체 수정 중단 → 검증자 리포트를 사용자에게 그대로 보고하고 판단을 구한다.
4. **루브릭 출처** — 판정 기준은 [docs/visual-rubric.md](docs/visual-rubric.md). 루브릭 자체를 임의로 완화하지 않는다.

> 작은 단발 수정(텍스트 한 줄 변경 등) 또는 사용자가 명시적으로 "검증 생략"을 요청한 경우에는 이 루프를 건너뛸 수 있다.

## 0-1-2. 실습 자료 제작 루프 (교안 인계 후)

교안 본문(슬라이드 HTML) 작업이 끝난 뒤 사용자가 "이 주제로 실습 만들어줘" / "이 개념 실습 자료 추가해줘" / "실습 환경 준비해줘" 류의 요청을 하면 `exercise-builder` 서브에이전트를 호출한다.

1. **호출 시점** — 강의 본문이 완성되었거나 특정 섹션의 실습 키트가 필요할 때.
2. **전달할 입력** — (a) 강화할 강의 HTML 경로 또는 섹션 (b) 강화하려는 핵심 개념 한 줄 (c) 학습자 프로파일(예: "비개발자 직장인") (d) 기존 실습 폴더가 있다면 경로.
3. **산출물 위치** — `./results/<lecture-name>/exercises/<exercise-name>/` 아래. 기존 실습 트리에 자연스럽게 합쳐지는 폴더명을 사용한다.
4. **항상 2단계로 만든다** — 일반 실습 + 심화 실습 한 세트. 일반 다 한 사람이 심화로 넘어갈 수 있도록.
5. **데이터 품질 기준** — 실제 한국 직장에서 쓰는 데이터 톤(컬럼명·약어·결측·이형 표기·노이즈 포함). Lorem Ipsum/홍길동 반복 같은 비현실 데이터 금지. 30–200 행 볼륨이 기본.
6. **검증** — 스크립트를 번들한 경우 `exercise-builder`가 직접 실행해 동작 확인 후 인계한다.
7. **UTURE 하드 룰** — 분량 표기 금지, 명사형 종결, 이모지 금지. 본 AGENTS.md 0번 항목과 동일하게 적용.

> 작업 분량이 크면 `exercise-builder`가 STEP 3(폴더 구조 합의)에서 한 번 사용자 확인을 받는다. 데이터 생성 전에 방향이 맞는지 점검하기 위함.

## 0-2. 교안 HTML 작성 시 체크리스트 (필수)

교안 HTML 코드를 작성하는 작업에는 **사용자가 진행 상황을 실시간으로 확인**할 수 있도록 체크리스트 md를 운영한다.

1. **체크리스트 파일 생성** — 작업 시작 전 `./results/<산출물명>/CHECKLIST.md` (또는 그에 준하는 위치) 파일을 만든다.
   - 파일이 이미 있으면 기존 파일에 이어 쓰지 말고, 이번 작업 단위에 맞게 새 섹션을 추가하거나 덮어쓴다.
2. **잘게 쪼개기** — 한 항목 = 한 번에 끝낼 수 있는 단위로 잘게 나눈다.
   - 예: "슬라이드 1 레이아웃 마크업", "슬라이드 1 본문 카피 삽입", "슬라이드 1 이미지/아이콘", "슬라이드 1 반응형 점검", "슬라이드 1 시각 검증" …
   - "전체 PPT 작성" 같은 큰 덩어리는 금지. 사용자가 진행도를 못 알아본다.
3. **하나 끝낼 때마다 즉시 체크** — `- [ ]` → `- [x]`. 한 번에 여러 개를 몰아서 체크하지 않는다. 끝나는 즉시 체크해야 사용자가 어디까지 진행됐는지 본다.
4. **체크리스트 형식**
   ```markdown
   # <산출물명> 작업 체크리스트

   ## 슬라이드 1
   - [ ] 레이아웃 마크업
   - [ ] 본문 카피 삽입
   - [ ] 이미지/아이콘 배치
   - [ ] 시각 검증 통과

   ## 슬라이드 2
   - [ ] ...
   ```
5. **TodoWrite와의 관계** — 내부 작업 관리용 TodoWrite와 별개로, 이 체크리스트는 **사용자가 보는 진행판**이다. 둘 다 운영하되 동기화는 필수 아님.
6. **건너뛰지 말 것** — 단발 텍스트 한 줄 수정 등 미세 작업이 아니라면, 교안 HTML 작업에는 항상 체크리스트를 운영한다.

---

Behavioral guidelines to reduce common LLM coding mistakes. Merge with project-specific instructions as needed.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Building

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing artifacts:
- Don't "improve" adjacent code, markup, styles, or copy.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated issues, mention them - don't fix them silently.

When your changes create orphans:
- Remove imports/variables/styles/assets that YOUR changes made unused.
- Don't remove pre-existing unused code/assets unless asked.

The test: Every changed line should trace directly to the user's request.

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.