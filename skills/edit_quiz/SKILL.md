---
name: edit_quiz
description: Review and change the quiz questions already saved on a teacher's Daniko lesson plan — fix a wrong answer key, reword a question, add a missing explanation, or change its points. Use when a teacher asks to edit, fix, correct, review, or change an existing quiz or quiz question.
---

# Edit an existing Daniko quiz

## 1. Find the lesson plan, then read its quiz

You need the `paper_id`. If the teacher just worked on that lesson in this conversation, reuse
it. Otherwise call `get-lesson-plans` to find it — and ask which one if more than one matches.

Then call `get-quiz` with that `paper_id`. It returns every saved question with its
`question_id`, choices, which choice is correct, points, and explanation.

Show the teacher the questions relevant to what they asked about. If they described a problem
vaguely ("the third one is wrong"), confirm which `question_id` you mean before changing it.

## 2. Draft the correction yourself

Work out the fix yourself — no tool generates content. Common cases:

- **Wrong answer key** — the right choice is marked incorrect. Send the full `choices` array
  with the correct one flagged.
- **Missing explanation** — quizzes saved before this feature may have none. Write one.
- **Reworded question** — send `question` only; everything else stays.
- **Wrong points** — send `points` (10, 20 or 30 only).

## 3. Get the teacher's approval

Show the change — ideally before and after — and let them iterate. Don't save until they
approve.

## 4. Save it

Call `edit-quiz` with the `question_id` and only the fields that changed. Anything you omit
keeps its current value.

Two things to be careful about:

- **`choices` replaces the whole set.** There's no way to edit one choice in place. If you
  send `choices`, send every choice for that question, with exactly one marked correct —
  otherwise the ones you left out are gone.
- **One question per call.** To fix several, call `edit-quiz` once per `question_id`.

## Adding questions instead of editing

`edit-quiz` only changes questions that already exist. To add new ones, use the
`generate_quiz` skill — but read the warning there first: `save-quiz` appends, so saving a
second set leaves the lesson holding both.
