---
name: edit_lesson
description: Find and update an existing Daniko lesson plan (paper) a teacher already created — title, content, or grade/subject/topic — rather than creating a new one. Use when a teacher asks to edit, update, revise, fix, or change an existing lesson plan or lesson paper.
---

# Edit an existing Daniko lesson plan

## 1. Find the lesson plan

Call `get-lesson-plans` with no `paper_id` to list the teacher's own lesson plans (title, grade,
subject, topic). Match it to what the teacher described. If more than one could match, ask
which one before continuing — never guess and edit the wrong paper.

Once you know the `paper_id`, call `get-lesson-plans` again with that `paper_id` to fetch its
current title and full HTML `content`.

## 2. Draft the change yourself

Apply the teacher's requested change to the content or title yourself — do not call any tool
to generate it. Keep the parts the teacher didn't ask to change as they are. `content` is
well-formed HTML (e.g. `<h1>`, `<h2>`, `<p>`, `<table>`, `<ul>`), stored and rendered as-is,
same as the web editor — do not send plain text or Markdown.

## 3. Get the teacher's approval

Show the revised title/content (or just the parts that changed) in the conversation. Let the
teacher ask for further changes and iterate — do not save anything until they've approved it
or explicitly say to save it.

## 4. Save it

Call `edit-lesson-plan` with the `paper_id` and only the fields that changed (`title`,
`content`, `grade_id`, `subject_id`, `topic_id`) — fields you omit keep their current value.
Report back once saved.
