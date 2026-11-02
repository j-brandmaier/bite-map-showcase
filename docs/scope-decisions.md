# Scope decisions: what I shipped, what I cut, and why

The first version of the spec described a bigger product than I could get into daily use quickly. As I worked through the initial concept, I cut the scope down to a focused v1 that solves our core problem: **we can't keep track of what we've been serving.** 

Everything I cut remains tracked in the backlog, so even though some ideas have been deprioritized from the initial release, I can evaluate them for later release.  

---

## What I cut

| ID | Requirement | Decision | Date |
|---|---|---|---|
| REQ-10 | Flags a cuisine on the log screen if served for lunch/dinner >4 times in 7 days | Deferred | 2026-10-15 (spec updated 10-19) |
| REQ-11 | Weekly summary: cuisines served, nutrition pie chart, weekly variety star, rating trends | Deferred | 2026-10-15 (spec updated 10-19) |
| REQ-05 | "Haven't had in a while" suggestions, ranked by longest absence | Deferred | 2026-10-19 |
| REQ-06 | Filter recipes that are safe for a cold daycare lunch | Deferred | 2026-10-19 |
| Recipe library | Full recipe creation and editing | Deferred | 2026-10-19 |
| Real-time sync | Changes appear instantly on the other phone | Replaced with 30-second refresh | 2026-10-19 |
| Delete account | In-app account removal | Deferred | 2026-10-26 |

### Why these features?
The rut flag and recommendations are the "smart" layer of the app. But intelligence features only work if there is a consistent, trustworthy meal history to analyze—and building that habit is what v1 is for. 

A shared 7-day History already answers *"what have we served lately?"* The deferred features simply automate the *noticing*. I wanted to validate that we'd actually log consistently before spending time automating insights over data that might not exist yet.

### What each cut saved
* **Weekly summaries & Recipes:** Saved building dedicated creation workflows, scheduled background jobs, and multi-step data pipelines.
* **Realtime sync:** WebSockets required complex merge logic across two code paths. Replacing it with a simple 30-second background refresh easily satisfied our <60s sync target with a fraction of the architectural complexity.

---

## What I kept (and the trade-offs involved)

* **Photo-only logging over itemized details (2026-09-16):** Snapping a photo of a packed lunch is much faster than tagging individual foods. The trade-off is real: photo-only meals don't populate downstream nutritional variety features. I intentionally chose logging velocity over data completeness.
* **Offline-first architecture:** Sometimes we only have a few seconds to log a meal, and it can't wait on a spinner. The app saves locally to SQLite immediately and syncs to Supabase when connected.
* **Asynchronous meal logging:** A packed lunch is decided in the morning, but how much was actually eaten isn't known until evening. Decoupling logging from rating matches real household workflows.

---

## Decisions I reversed after using the app on my phone

Writing a spec and living with it are two very different things. Testing on physical hardware forced three key course corrections:

* **Tagging foods became optional (2026-09-26):** I originally required a cuisine and at least one nutrition group to save a meal. In daily use, that extra tap killed momentum. I made metadata optional because speed beat completeness.
* **Cuisine moved from the food to the whole meal (2026-09-29):** Cuisine describes the overall meal ("Mexican night"), not an individual ingredient like rice. Moving this to the meal level simplified both the user experience and the underlying database query for future rut detection.
* **Dropdown focus behavior (2026-09-26 to 09-27):** The food search dropdown wasn't dismissing cleanly during single-handed use. It took three iterations to get the state right: dismissing the keyboard shouldn't close the suggestions, but tapping outside must.

---

## Explicitly out of scope for v1

Calorie and macro tracking, multi-child profiles, grocery list generation, and complex nutrition dashboards.
