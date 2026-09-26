---
name: generate_quiz
description: Draft multiple-choice quiz questions for a teacher's existing Daniko lesson plan, then save them once the teacher approves. Use when a teacher asks to create, write, draft, or generate a quiz, test, or exam questions for a lesson.
---

# Generate a Daniko quiz

## 1. Find the lesson plan it attaches to

Every quiz in Daniko belongs to an existing lesson plan (a "paper"). You need its `paper_id`:

- If you just created the lesson plan this conversation (via the `generate_lesson` skill),
  reuse the `paper_id` it returned — don't ask the teacher to repeat it.
- Otherwise, ask the teacher which lesson plan the quiz is for.

## 2. Confirm the quiz shape

If the teacher didn't already say, ask: how many questions, and what difficulty/point value
per question. Daniko only accepts **10, 20, or 30** points per question (a fixed enum, not a
free choice) — if the teacher gives another number, round to the nearest of the three and
tell them you did.

## 3. Draft the questions yourself

Write the questions and choices yourself — no tool generates content for you. For each
question:

- A clear question text
- 2–4 multiple-choice options, exactly one marked correct
- An `explanation` — always write one. Say *why* the correct answer is correct, and where it
  helps, why the tempting wrong option isn't. Students see this after answering, so it's the
  part that teaches; a question without it only tests. Keep it to a sentence or two.

Base the questions on the lesson plan's actual content so they test what was taught, not
generic trivia about the topic.

## 4. Get approval, then save

Show the drafted questions in the conversation and let the teacher edit before saving —
don't save on the first draft automatically. Once approved, call `save-quiz` with the
`paper_id` and the full `questions` array. Report back how many questions were saved.
