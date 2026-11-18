---
description: Review every route in course-api in parallel, then write tests for the problems and gaps found
argument-hint: [optional route file to limit the run, e.g. routes/users.js]
allowed-tools: Task, Read, Grep, Glob
---

Run this workflow against the API in `course-api/`. If `$ARGUMENTS` names a route file, only work on that file; otherwise cover every file in `course-api/routes/`.

## Step 1 — Review (parallel)

List the route files. Then launch one `api-reviewer` subagent per route file, all in a single message so they run at the same time. They are independent of each other, so don't wait on one before starting the next. Tell each reviewer which one file to review and ask for findings ordered by severity, with `file:line`.

## Step 2 — Write tests (depends on Step 1)

Wait until every reviewer has finished. Merge their findings into one list, drop duplicates, and note which routes and cases (validation errors, 404s, wrong status codes) look untested or buggy.

Then launch a single `test-writer` subagent and give it that merged list as its brief. It must cover the cases the reviewers flagged for each route. It needs those results, so don't start it earlier.

## Step 3 — Report

Reply with:
- the merged review findings, most severe first
- what the test-writer added and the final `npm test` result
- any suspected application bugs the test-writer reported, left unfixed
