# How I worked with AI tools

As a product manager, I leveraged Claude Code as my engineering execution partner; my job was to make sure it built the right thing and to catch it when it didn't. This page covers how I set that up, what I checked, and what went wrong.

## The setup: give the AI a source of truth

Left alone, an AI assistant fills gaps with plausible guesses. So instead of prompting feature by feature, I gave every session the same four documents:

| Document | Purpose |
|---|---|
| **Constitution** | Product principles, engineering rules and privacy rules that stay fixed for the whole build. |
| **Spec** | Numbered requirements in EARS syntax ("WHEN a meal is logged for a time at or before now, THE system SHALL prompt for the rating"). The spec says that code disagreeing with it is a bug in either the code or the spec, never something to ignore. |
| **Decisions log** | Every non-obvious choice, with the alternative I rejected and why. |
| **Progress log** | What's done, what's built but not yet checked on a phone, and what's next. |

An instructions file tells every new session to read these before changing anything. That's the "context engineering" part: the AI never starts from a blank page, and I never have to re-explain the product.

## Deciding before building

For the shared-data design I had the AI lay out six decisions with an alternative for each, before any code was written. For example, I chose passwordless sign-in over a shared secret, a refresh-on-open sync over a background sync service, and hard deletes over a tombstone table. Each was weighed against how small this app is (two users, a few meals a day). Most of the "obvious, bigger" options lost.

## Reviewing what the AI produced

I read the plans, not just the results, and I tested on a real phone. A few catches from the decisions log:

- **A pairing flow nobody could finish.** The function that creates a household returned an internal ID, which no screen ever showed. The first parent would have had no way to see the invite code to share. It was caught before on-device testing and fixed.
- **A privacy hole in a draft.** An early draft of the database rules let any signed-in user read every household's row, which would have let a stranger list all invite codes and join anyone's household. It was caught while drafting and replaced with rules that only show a member their own household.
- **A confident claim that was wrong.** An early note said the email service would send to any recipient. The first real test showed it only delivered to the account owner, and the note was corrected in the log. I record corrections rather than overwrite them.
- **Honest status.** The progress log distinguishes "confirmed on-device" from "built, not yet seen on a device," so I always know what's actually been verified.

## Where design tooling failed, and how I diagnosed it

I used Google Stitch to generate screen designs. It kept inventing content that isn't in the app: allergen and weaning notes, a Notes field, a "Mindful eating moment" card, invite-code expiry timers, a Share button.

Rather than keep re-prompting, I looked for the cause. The Stitch project still carried a legacy design-system file from an earlier attempt, which was dragging in its old color palette and content. I took these steps:

1. Removed that file so the new "Soft Sage" design system was the only one.
2. Replaced the auto-generated product description with a brief derived only from the spec and constitution, including an explicit **"not in the app"** list.
3. Wrote prompts with the exact on-screen text for every frame.
4. Reviewed each export against that text, and rejected frames that drifted.

For the Log a meal screen, Stitch still kept drifting, so I stopped using it there and built the remaining mock-ups using Claude Code with HTML. Then I built the screens from those.

**What I took from this:** AI design and code tools are strongest when constrained by a written source of truth and weakest when asked to be creative within a product they don't understand. And when output keeps going wrong, look for a standing cause before re-prompting.

## Working the way I'd work with a team

- **Small, reviewable steps.** Each change comes with a note on what to check on a phone.
- **Spec and code stay in sync.** When the build taught me something (cuisine belongs to the meal, not the food), I updated the spec and logged why.
- **Product owns the "what," engineering owns the "how."** The constitution says so, and it applies to the AI as much as to a person: I made the product calls, and I let it propose how to build them.

## What I'd do differently

- **Set stronger limits on command retries.** Stitch and Claude each stumbled with certain tasks. In hindsight, I spent too many cycles getting subpar output before realizing that I needed to diagnose the root cause (outdated reference files). 
- **Keep the context files lean and current.** My Progress.md file grew with each decision, leading to a bloated context that sometimes got out of sync with decisions. The next time I go through an activity like this, I will archive finished history, keep a shorter current snapshot, and use AI to become disciplined about updating progress.
