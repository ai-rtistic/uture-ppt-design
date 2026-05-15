---
name: uture-design-system
description: Use this skill to generate well-branded interfaces and assets for UTURE (유쳐) AX Agency — a Korean AI/technology education studio. UTURE materials follow a strict minimal-editorial system: white canvas, Pretendard typography, a single warm orange accent (#FF4F00), and zero shadows/gradients/stock-imagery. Use for production design or throwaway prototypes, slides, workbooks, and learning artifacts.
user-invocable: true
---

# UTURE Design Skill

Read `README.md` in this folder first. It defines:

- **Brand DNA** — minimal editorial; "잘 정리된 교재"의 톤.
- **Content fundamentals** — Korean noun-form prose, no 1st/2nd person, no emoji, no marketing copy.
- **Visual foundations** — `colors_and_type.css` holds every color & type token. Import it.
- **Iconography** — Lucide is the temporary line-icon set (1.5px). No emoji, no unicode glyphs, no character art.

## Files in this skill

- `colors_and_type.css` — Drop-in stylesheet. All `--ut-*` tokens, type scale utility classes, component primitives (`.ut-eyebrow`, `.ut-pill`, `.ut-callout`, `.ut-card`).
- `assets/uture-logo.svg` / `uture_logo.png` — Graphic mark (asymmetric 4-point sparkle). Vector + raster pair.
- `assets/uture-text-logo.svg` / `uture_text_logo.png` — Wordmark "uture". Vector + raster pair.
- `slides/` — 8 sample slide archetypes (1920×1080) + a `<deck-stage>` `index.html` that plays them.
- `preview/` — Design-system preview cards (used in the parent project's Design System tab).

## When to use

- The user asks for a **UTURE lecture deck, workbook, handout, course landing, or learning artifact**.
- The user is building **internal training/onboarding** and wants this brand applied.
- The user pastes raw content (an article, paper summary, lesson outline) and wants it rendered in UTURE's editorial system.

## How to apply the system

1. **Start from a sample.** `slides/index.html` is the canonical deck. Copy a `<section>` whose archetype matches your content (cover / section / statement / concept / comparison / flow / step / closing).
2. **Use the tokens, never magic numbers.** Import `colors_and_type.css`. If a value isn't in the token set, you probably don't need it.
3. **Anchor every body slide.** Eyebrow (top-left) + page-marker (bottom-right) + bottom divider line. No exceptions except Cover and Section Divider.
4. **Every body slide carries one visual device.** Definition → metaphor sketch. Sequence → arrow flow. Comparison → card grid. Taxonomy → 2×2 matrix or tree. Numbers → mini chart or table. Text-only slides are reserved for Statement and Closing. See `README.md` § Visual Playbook for the full type → device mapping.
5. **Fill 40–70% of the body area.** Whitespace is a tool for hierarchy, not a target. If a slide feels empty, add the visual device or merge slides — do not stretch type to fill.
6. **Limit the orange.** Maximum one keyword OR one accent node per slide. Inside a diagram, only one node, one pin, or one card border gets the accent.
7. **No emoji, no gradients, no shadows, no stock photos, no character illustrations, no full-bleed imagery.** When in doubt, leave it blank — but first verify there is no diagram/table/metaphor that would actually serve the content.
8. **Cover slide carries the team name only.** 강사 표기가 필요하면 정확히 `uture | 유쳐 교육팀`으로 적는다 (`UTURE 교육팀` 등 다른 표기 금지). **분량 표시("30분", "60분 분량" 등)는 표지·본문 어디에도 넣지 않는다.** 날짜·청중·분량 같은 로지스틱스 정보는 스피커 노트나 별도 페이지로 분리한다.
9. **워드마크(`uture_text_logo`)는 데크 전체에서 표지 1회만 사용한다.** 섹션 디바이더·클로징·본문 어디에도 워드마크를 두지 않는다. 브랜드 위치가 필요한 슬라이드는 그래픽 마크 단독으로 처리한다.
10. **Korean prose:** noun-form endings (`~한다`, `~된다`), no 여러분/저는, no exclamation marks, no marketing adjectives. See `README.md` § Content Fundamentals.

## If the user asks for something visual without telling you what

Ask first — usually one round of these is enough:

- **Output kind:** PPT slides, workbook spread, course-page hero, handout PDF?
- **Lecture topic + audience level** (beginner / intermediate / executive)?
- **Density mode** (compressed / standard / detailed — see README § 10.1)?
- **How many slides** or how long the artifact should be?
- **Do they have icons / diagrams already**, or should we leave placeholders?

Then act as an expert UTURE designer. Output HTML files in the user's project (slides as `<deck-stage>` sections, handouts as A4 print-styled HTML) — and lift assets from this skill folder into theirs (the logo, the CSS, the deck-stage component).

Default to **HTML artifacts**, switch to **production code** only when the user is integrating into a real codebase.
