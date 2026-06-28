# Quiz Template

`quiz.html` is the canonical chrome for per-objective practice quizzes. New quizzes are derived by copy + edit:

1. `cp quiz.html ../<cert-slug>/practice-quiz-<D>-<S>.html`
2. Open the copy. Locate the CONFIG block at the top of the `<script>` tag.
3. Edit `QUIZ_TITLE`, `QUIZ_SUBTITLE`, `ACCENT_HEX`, and replace the entire `questions` array with the real question objects for this quiz.
4. Do not edit anything outside the CONFIG block. Style/UX fixes belong in `quiz.html`, then re-derive consuming quizzes if structural changes require it.

## CONFIG schema

| Field | Type | Notes |
|---|---|---|
| `QUIZ_TITLE` | string | Used for `<title>` and the on-page H1. Keep short. |
| `QUIZ_SUBTITLE` | string | One line under the H1. Convention: `C:\Shelnet\Quizzes> <CERT-CODE> Objective <D>.<S>` |
| `ACCENT_HEX` | string | Per-cert accent. Sec+ `'#3b82f6'`, CySA+ `'#06b6d4'`. Whole quiz UI tints from this. |
| `questions` | array | Each: `{ q, options: [a,b,c,d], correct: index, domain, explanation }`. 15 items per per-objective quiz; 85 for full practice exams. |

## What lives in the chrome (do not duplicate per quiz)

- All CSS (palette, terminal-bar, brutal header, layout, mobile responsive rules)
- The render/navigation/score JS engine
- 83% pass threshold (matches CompTIA's ~750/900)
- The retake-on-completion button

If you find yourself editing chrome inside a derived quiz, stop — that change should land in `quiz.html` and then propagate.
