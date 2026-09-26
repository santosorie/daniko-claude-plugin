---
name: generate_lesson
description: Draft a Daniko lesson plan for a teacher's class, grounded in their real grade/subject/topic and room data, then save it into Daniko once the teacher approves it. Use when a teacher asks to create, write, draft, or generate a lesson plan or lesson paper.
---

# Generate a Daniko lesson plan

## 1. Ground the request

Call the `get-class-context` tool first. It returns the teacher's rooms (classes) and the
grade level → subject → topic hierarchy. Use it to resolve the `grade_id`, `subject_id`, and
`topic_id` you'll need later — never guess these IDs, and never pick a subject/topic that
isn't nested under the grade the teacher actually means.

If the teacher's request doesn't already specify grade, subject, and topic, ask before
drafting rather than assuming.

## 2. Draft the lesson plan yourself

Do not call any tool to generate the content — write it yourself, using the teacher's stated
need (topic, class characteristics, duration, any special focus they mentioned). A Daniko
lesson plan (`content`) should have this structure, in this order:

1. **Learning Objectives** — what students should know/do by the end
2. **Materials** — what's needed to teach it
3. **Activities** — opening, main activity, closing, with rough timing that fits the
   requested duration
4. **Assessment** — how understanding will be checked

Write `content` as well-formed HTML — headings (`<h1>`/`<h2>`), paragraphs (`<p>`), lists
(`<ul>`/`<ol>`), and tables (`<table>`) where they fit. It's stored and rendered as raw HTML,
the same as Daniko's own web editor — do not send plain text or Markdown.

Keep the `title` short and specific (max 100 characters — it's a hard database limit), and
plain text (no HTML tags).

## 3. Get the teacher's approval

Show the draft in the conversation. Let the teacher ask for changes and iterate — do not
save anything until they've approved it or explicitly say to save it.

## 4. Save it

Call `save-lesson-plan` with the final `title`, `content`, and the `grade_id`/`subject_id`/
`topic_id` from step 1. Report back the `paper_id` it returns — the teacher may want a quiz
attached to this same lesson next (see the `generate_quiz` skill), and that requires this
`paper_id`.
