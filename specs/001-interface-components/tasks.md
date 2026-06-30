---

description: "Task list for interface components single-file UI implementation"
---

# Tasks: Interface Components — Single-File UI

**Input**: Design documents from `specs/001-interface-components/`

**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, data-model.md, contracts/

**Tests**: The examples below include test tasks. Tests are OPTIONAL — only include them if explicitly requested in the feature specification.

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Path Conventions

- **Single project**: `src/`, `tests/` at repository root
- **Web app**: `backend/src/`, `frontend/src/`
- **Mobile**: `api/src/`, `ios/src/` or `android/src/`
- Paths shown below assume single project — adjust based on plan.md structure

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Create the single HTML file with base structure

- [x] T001 Create `index.html` with HTML5 doctype, `lang="pt-BR"`, viewport meta tag for responsiveness, and linked CSS/JS sections (`<style>` and `<script>`)

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Core infrastructure that MUST be complete before ANY user story can be implemented

**⚠️ CRITICAL**: No user story work can begin until this phase is complete

- [x] T002 [P] Define CSS custom properties in `index.html` `<style>` with the full color palette from constitution (vinho `#8B1A2B`, ouro `#D4A017`, areia `#F5E6D3`/`#E8D5C4`/`#C4A882`, texto `#2C1810`, sucesso `#2E7D32`, atencao `#F9A825`, inativo `#9E9E9E`) and base reset styles (box-sizing, margin/padding zero, font stack)
- [x] T003 [P] Define sticker data array in `index.html` `<script>` with at least 8-11 countries (FWC first, then BRA, ARG, GER, FRA, ENG, ESP, ITA, NED, POR, URU in alphabetical order), each with 6-12 stickers, and initialize the app state object mapping each code to `{ owned: false, repeats: 0 }`

**Checkpoint**: Foundation ready — user story implementation can now begin

---

## Phase 3: User Story 1 — Mark Stickers as Owned and Adjust Duplicates (Priority: P1) 🎯 MVP

**Goal**: User can click any sticker to toggle owned state and use +/- buttons to control duplicate count

**Independent Test**: Open `index.html` in browser, click a gray sticker — it turns green/owned, +/- buttons enable. Click + to increase repeat count, click - to decrease it. Click again to unmark.

### Implementation for User Story 1

- [x] T004 [P] [US1] Build the sticker grid HTML structure in `index.html`: a `<main id="grid">` container with dynamic country sections — each section has a country header (flag + code + name) and a sticker grid using CSS Grid
- [x] T005 [P] [US1] Implement CSS for sticker cards in `index.html` `<style>`: three state classes `.missing` (white/gray border, 0.7 opacity), `.owned` (green tint `#e8f5e9` background, `#2E7D32` border), `.duplicate` (yellow tint `#fff8e1` background, `#F9A825` border). Each card shows code, number, and a control row with − button, quantity display, + button
- [x] T006 [US1] Implement JavaScript state functions in `index.html` `<script>`: `toggleSticker(code)` flips owned (sets repeats to 0 if unmarking), `increment(code)` adds 1 to repeats only if owned, `decrement(code)` subtracts 1 only if owned and repeats > 0. Enforce invariant: repeats MUST be 0 when owned is false
- [x] T007 [US1] Implement `renderGrid()` function in `index.html` `<script>` that reads app state, filters by country sections, builds card HTML for each sticker applying the correct CSS class based on owned/repeats state, and injects into `#grid`
- [x] T008 [US1] Implement event delegation on `#grid` in `index.html` `<script>`: listen for clicks, detect if target is `.btn-inc` (call increment), `.btn-dec` (call decrement), or `.sticker` (call toggleSticker). Call `render()` after each mutation

**Checkpoint**: At this point, User Story 1 should be fully functional and testable independently

---

## Phase 4: User Story 2 — Filter and Navigate the Sticker Grid (Priority: P2)

**Goal**: User can switch between tab filters (Todas/Faltantes/Possuídas/Repetidas) to see only relevant stickers

**Independent Test**: Open `index.html`, mark some stickers. Click "Faltantes" — only unmarked stickers show. Click "Possuídas" — only owned with 0 repeats show. Click "Repetidas" — only owned with repeats >= 1 show. Click "Todas" — all return.

### Implementation for User Story 2

- [x] T009 [P] [US2] Build tab navigation HTML in `index.html`: a `<nav id="tabs">` with four `<button>` elements: Todas (default active), Faltantes, Possuídas, Repetidas — each with `data-filter` attribute
- [x] T010 [P] [US2] Implement CSS for tabs in `index.html` `<style>`: flex layout, equal width, `.active` tab gets vinho background/white text, inactive tabs get areia-medio background with areia-escuro border, min-height 44px for touch targets
- [x] T011 [US2] Implement `setFilter(filter)` in `index.html` `<script>`: update `activeFilter` variable, toggle `.active` class on tab buttons, call `render()`. Implement `passesFilter(code)` that returns true/false based on activeFilter value checking owned/repeats state
- [x] T012 [US2] Integrate filter with render: modify `renderGrid()` to skip stickers that don't pass the active filter. Add empty state: when no stickers match, show a `<div class="empty-state">` with "Nenhuma figurinha encontrada neste filtro."

**Checkpoint**: At this point, User Stories 1 AND 2 should both work independently

---

## Phase 5: User Story 3 — Generate and Copy Trading Report (Priority: P3)

**Goal**: User sees auto-generated report in the footer and copies it to clipboard as formatted WhatsApp text

**Independent Test**: Open `index.html`, mark some stickers with varied owned/repeats. Scroll to footer — report updates automatically. Click "Copiar" — text is copied. Paste into WhatsApp — formatting is correct with emoji headers, *bold* sections, grouped by country.

### Implementation for User Story 3

- [x] T013 [P] [US3] Build report footer HTML in `index.html`: a `<footer>` with heading "📋 Relatório de Troca", a `<pre id="report-text">` for the generated text, a `<button id="copy-btn">` with text "📋 Copiar para WhatsApp", and a `<span id="copy-feedback">` for status messages
- [x] T014 [P] [US3] Implement CSS for report footer in `index.html` `<style>`: vinho background, white text, ouro heading, dark code-block `<pre>` (`#1a0a0e` background, light text), monospace font, copy button styled with ouro background. Responsive max-width constraints for larger screens
- [x] T015 [US3] Implement `getReportText()` in `index.html` `<script>`: iterate all stickers, partition into three arrays (missing, owned-without-repeats, duplicated-with-repeats). Generate formatted WhatsApp text with `\n` line breaks, `*texto*` asterisk-bold for headers, 🏆 emoji prefix. Group codes by country within each section using `groupByCountry()` helper. Omit empty sections
- [x] T016 [US3] Implement clipboard copy in `index.html` `<script>`: `copyReport()` reads text from `#report-text`, calls `navigator.clipboard.writeText()`, shows success feedback ("✅ Copiado!"), resets after 2.5s. Fallback: if clipboard API fails or is unavailable, create a hidden `<textarea>`, select its contents, call `document.execCommand('copy')`, and show fallback instructions

**Checkpoint**: All user stories should now be independently functional

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: Responsive refinement, edge case handling, and validation

- [x] T017 [P] Add responsive CSS media queries in `index.html` `<style>` for 480px (smaller card sizes `--card-w: 58px`), 768px+ (max-width container 900px), and 1200px+ (max-width 1100px, larger cards `--card-w: 90px`). Ensure no horizontal scroll at any breakpoint
- [x] T018 Implement clipboard fallback in `index.html` `<script>`: when `navigator.clipboard` is unavailable or `.writeText()` rejects, display a read-only `<textarea>` with the report text and instructions to copy manually (Ctrl+C/Cmd+C). Add error feedback styling
- [x] T019 Implement empty state edge cases in `index.html` `<script>`: (a) when no stickers are marked, report shows "Nenhuma figurinha marcada ainda."; (b) when filter matches zero stickers, grid shows "Nenhuma figurinha encontrada neste filtro."; (c) copy button checks for empty report and shows "Nada para copiar — marque algumas figurinhas primeiro."
- [x] T020 [P] Run manual validation against `specs/001-interface-components/quickstart.md`: open `index.html` in Chrome, Firefox, and Safari; test all 4 acceptance scenarios per user story; verify WhatsApp copy-paste formatting

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies — can start immediately
- **Foundational (Phase 2)**: Depends on Setup completion — BLOCKS all user stories
- **User Stories (Phase 3-5)**: All depend on Foundational phase completion
  - User stories proceed sequentially in priority order (P1 → P2 → P3)
- **Polish (Phase 6)**: Depends on all desired user stories being complete

### User Story Dependencies

- **User Story 1 (P1)**: Can start after Foundational (Phase 2) — No dependencies on other stories
- **User Story 2 (P2)**: Can start after Foundational (Phase 2) — Depends on US1 grid rendering being in place (adds filter logic on top)
- **User Story 3 (P3)**: Can start after Foundational (Phase 2) — Depends on US1 state management (report reads from same state)

### Within Each User Story

- Data before rendering
- Rendering before interaction
- Core implementation before edge cases
- Story complete before moving to next priority

### Parallel Opportunities

- T002 and T003 (Foundational) can run in parallel (CSS variables vs sticker data)
- T004 and T005 (US1) can run in parallel (HTML grid vs CSS cards)
- T009 and T010 (US2) can run in parallel (tab HTML vs CSS)
- T013 and T014 (US3) can run in parallel (footer HTML vs CSS)
- T017 (responsive) can run in parallel with any remaining polish tasks
- Most implementation tasks are sequential within the same `index.html` file

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: Setup
2. Complete Phase 2: Foundational
3. Complete Phase 3: User Story 1 (grid + toggle + +/- controls)
4. **STOP and VALIDATE**: Open `index.html`, mark stickers, verify visual feedback and +/- controls
5. Deploy/demo if ready

### Incremental Delivery

1. Complete Setup + Foundational → Foundation ready
2. Add User Story 1 → Test independently → Deploy/Demo (MVP!)
3. Add User Story 2 → Test independently → Deploy/Demo
4. Add User Story 3 → Test independently → Deploy/Demo
5. Complete Polish → Run quickstart validation

### Parallel Team Strategy

With multiple developers:

1. Team completes Setup + Foundational together
2. Once Foundational is done:
   - Developer A: User Story 1 (grid + state + toggle)
   - Developer B: User Story 2 (tabs + filter) — can start after US1 render is stable
   - Developer C: User Story 3 (report + clipboard) — can start after US1 state is stable
3. Stories integrate into the same `index.html` file

---

## Notes

- [P] tasks = different concerns even if in the same file (CSS vs JS)
- [Story] label maps task to specific user story for traceability
- Each user story should be independently completable and testable
- All tasks operate on the single file `index.html` at repository root
- Commit after each logical group of tasks
- Stop at any checkpoint to validate story independently
- No test tasks included — spec does not request tests or TDD
