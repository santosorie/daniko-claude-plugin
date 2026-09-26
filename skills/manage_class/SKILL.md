---
name: manage_class
description: Create or rename a teacher's Daniko classes (rooms), see which lesson plans are assigned to a class, see which classes a lesson plan is published to, and publish a lesson to a class immediately or on a chosen date. Use when a teacher asks about their classes, wants to create or rename one, or wants to assign, publish, schedule, or share a lesson plan with a class.
---

# Manage Daniko classes

## Seeing classes and what's in them

Call `get-rooms` with no `room_id` to list the teacher's classes. Call it with a `room_id` to
see that class in detail — its join code, student count, and every lesson plan assigned to it
with when each becomes available.

For the other direction — which classes a given lesson is published to — call
`get-lesson-plans` with that `paper_id`; its response lists the rooms it's assigned to.

## Creating a class

Call `create-room` with the name the teacher confirms. Report back the join code it returns —
that's what students use to join, so the teacher will want it.

A teacher may have at most 10 classes. If they're at the limit the tool says so; don't try to
work around it, just tell them.

## Renaming a class

Find the `room_id` with `get-rooms`, confirm the new name with the teacher, then call
`rename-room`. If more than one class could match what they described, ask which one rather
than guessing.

## Publishing a lesson to a class

Call `assign-lesson-to-room` with the `paper_id` and `room_id`.

**Always ask the teacher whether the lesson should go live now or on a specific date** before
calling it — don't assume:

- **Go live now** — omit `available_from`. Students can open it immediately.
- **Scheduled** — pass `available_from` as `"YYYY-MM-DD HH:MM"`. Students can't open it until
  then.

Assigning a lesson that's already assigned just updates its availability. It never removes the
lesson from any other class, so it's safe to publish the same lesson to several classes one at
a time.
