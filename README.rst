# FOSSASIA Eventyay — My Open Source Contributions

> Fork of [fossasia/eventyay](https://github.com/fossasia/eventyay) — an open source event management platform with 1.6k+ stars, used globally for ticketing, talks, and video management. Built with Python, Django, Vue.js, and PostgreSQL.

---

## About the Project

**Eventyay** is FOSSASIA's flagship open source platform for managing events end-to-end — from ticket sales and attendee management to talk scheduling and video streaming. It is one of the primary projects under FOSSASIA, a globally recognized GSoC (Google Summer of Code) organization.

Contributing to eventyay means working on a production-grade Django/Python codebase reviewed by international maintainers, with automated review from tools like Sourcery AI, GitHub Copilot, and ChatGPT Codex.

---

## My Pull Requests

### ✅ [#3438 — Fix product meta typeahead filtering for limited team members](https://github.com/fossasia/eventyay/pull/3438)
**Status: Merged** | **Approved by 3 reviewers** | May 4, 2026

**Problem:**
Team members with limited event access were seeing incorrect product meta value suggestions in the typeahead search. The queryset used for default product meta values was incorrectly pulling from the broader `matches` queryset instead of the scoped `defaults` queryset, causing limited-access users to see metadata from events they shouldn't have access to.

**What I did:**
- Fixed the queryset assignment in `app/eventyay/control/views/typeahead.py` to correctly filter default product meta values based on the events the limited team member has permission to access
- Added organizer constraint to team filtering to prevent cross-organizer data leakage
- Wrote a regression test in `app/tests/tickets/control/test_typeahead.py` to lock in the correct behavior and prevent future regressions

**Files Changed:**
- `app/eventyay/control/views/typeahead.py`
- `app/tests/tickets/control/test_typeahead.py`

**Review process:** Went through automated Sourcery AI review, addressed feedback, received LGTM from 3 maintainers (ArnavBallinCode, Rachit7168, Saksham-Sirohi), tested locally before merge.

---

### 🔄 [#3446 — fix: improve webhook reliability with timeout, break-to-continue, and explicit error status](https://github.com/fossasia/eventyay/pull/3446)
**Status: Closed** (closed in favour of existing PRs #3394 and #3396) | April 30, 2026

**Problem:**
The webhook delivery system had three critical reliability issues:
1. A `break` statement in `notify_webhooks` was silently dropping all remaining webhook notifications in a batch whenever a single log entry lacked an organizer or webhook type
2. The `send_webhook` function had no HTTP timeout, meaning a hanging remote endpoint could block a Celery worker indefinitely — and enough such endpoints could exhaust the entire worker pool
3. The `RequestException` handler created `WebHookCall` records without explicitly setting `success=False`, relying on model defaults and causing inconsistency

**What I did:**
- Replaced `break` with `continue` in `notify_webhooks` so that only the problematic log entry is skipped while remaining entries continue processing (fixes issue #3400)
- Added `WEBHOOK_TIMEOUT = 30` seconds constant and applied it to the `requests.post()` call in `send_webhook` to prevent Celery worker pool exhaustion (fixes issue #3398)
- Made the timeout configurable via Django settings after Sourcery AI review feedback
- Added explicit `success=False` in the `RequestException` error path for consistent `WebHookCall` records
- Added `logger.debug()` calls for skipped entries to improve observability
- Added response body size limiting to prevent OOM from oversized webhook responses

**Files Changed:**
- `app/eventyay/api/webhooks.py` — 22+ insertions, 3 deletions
- `app/eventyay/config/settings.py` — added configurable webhook timeout

**Review process:** Reviewed by Sourcery AI, GitHub Copilot, and ChatGPT Codex. Addressed all feedback across 4 commits. PR was eventually closed as another contributor had raised PRs for the same issues earlier.

---

### 🔄 [#3138 — Fix: Standardize login button text for consistency](https://github.com/fossasia/eventyay/pull/3138)
**Status: Closed** (closed in favour of #3177) | April 2, 2026

**Problem:**
The login page had inconsistent UI text — the email login button read "Login with Email" while the rest of the application used the standardized "Log in" phrasing, creating an inconsistent user experience.

**What I did:**
- Updated the button text in `app/eventyay/eventyay_common/templates/eventyay_common/auth/_login_options.html`
- Maintained the `{% translate %}` tag for i18n consistency so the fix works across all supported languages
- Provided before/after screenshots in the PR description

**Files Changed:**
- `app/eventyay/eventyay_common/templates/eventyay_common/auth/_login_options.html`

**Review process:** Reviewed by Sourcery AI and GitHub Copilot. Closed in favour of a superseding PR that addressed the same issue.

---

### 🔄 [#3108 — Fix: update login button text to 'Log in with email' for consistency](https://github.com/fossasia/eventyay/pull/3108)
**Status: Closed** | April 1, 2026

**Problem:**
Initial PR addressing the same login button text inconsistency (issue #3099) — the button read "Login with Email" instead of the consistent "Log in with email" wording used throughout the app.

**What I did:**
- Updated login button template text
- Maintained i18n tag structure

This was the first PR I raised on the eventyay codebase — the starting point of my open source contribution journey here.

---

## Key Learnings from Contributing

**Technical:**
- Working with large Django/Python codebases with 20,000+ commits
- Understanding Celery task queues and webhook delivery patterns
- Writing regression tests with pytest and Django test client
- Handling i18n/translation string updates correctly
- Configuring Django settings for runtime-tunable constants

**Process:**
- Writing clear, structured PR descriptions with Summary, Changes, Testing sections
- Responding to automated AI code reviews (Sourcery, Copilot, Codex)
- Iterating on feedback across multiple commits
- Understanding open source contribution etiquette — picking unassigned issues, respecting prior work

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| Backend | Python, Django |
| Frontend | HTML, JavaScript, Vue.js, CSS/SCSS |
| Database | PostgreSQL |
| Task Queue | Celery |
| Testing | pytest, Django Test Client |
| DevOps | Docker, GitHub Actions |

---

## About Me

**Harshit Singh** | Final Year B.Tech @ NIT Calicut (Batch 2027)

- 🏆 LeetCode Guardian | Rating 2270 | Top 0.63% globally
- 🌍 Open Source Contributor — FOSSASIA & GSSoC'26
- 💼 Intern @ BHEL | Senior Tech Lead @ Tathva
- 📫 [LinkedIn](https://www.linkedin.com/in/harshitsinghnitc/) | [GitHub](https://github.com/Harshit-0018)
