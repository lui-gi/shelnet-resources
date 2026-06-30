# Quiz Template

`quiz.html` is the canonical chrome for per-objective practice quizzes and full practice exams. New quizzes are derived by copy + edit:

1. `cp quiz.html ../<cert-slug>/exams/practice-quiz-<N>.html`
2. Open the copy. Locate the CONFIG block at the top of the `<script>` tag.
3. Edit `QUIZ_TITLE`, `QUIZ_SUBTITLE`, `ACCENT_HEX`, and replace the entire `questions` array with the real question objects for this quiz.
4. Do not edit anything outside the CONFIG block. Style/UX fixes belong in `quiz.html`, then re-derive consuming quizzes if structural changes require it.

## CONFIG schema

| Field | Type | Notes |
|---|---|---|
| `QUIZ_TITLE` | string | Used for `<title>` and the on-page title row (left). Lowercase short label, e.g. `'practice quiz 1'`. |
| `QUIZ_SUBTITLE` | string | One line, shown right in the title row. Convention: `'<cert-slug> · obj <D>.<S>'` (e.g. `'sec-plus · obj 1.1'`). The question count is appended automatically — do **not** include it. |
| `ACCENT_HEX` | string | Per-cert accent. Match the manifest cert accent so the inner accent matches the cert dashboard's outer Frame. Sec+ `'#38bdf8'`, CySA+ `'#22d3ee'`, A+ `'#fb7185'`. Whole quiz UI tints from this. |
| `questions` | array | Each: `{ q, options: [a,b,c,d], correct: index, domain, explanation }`. 15 items per per-objective quiz; 85 for full practice exams. |

## Keyboard shortcuts (chrome, not per-quiz)

| Key | Action |
|---|---|
| `1`–`9` | Select option |
| `←` | Previous question |
| `→` or `Enter` | Next question / finish |
| `Enter` (on results) | Retake (reload) |

## What lives in the chrome (do not duplicate per quiz)

- All CSS (TUI vocabulary, mono, alpha tiers, edge-to-edge layout, mobile rules).
- The render / navigation / score / keyboard JS engine.
- 83% pass threshold (matches CompTIA's ~750/900).
- The retake-on-completion action link.

If you find yourself editing chrome inside a derived quiz, stop — that change should land in `quiz.html` and then propagate.
