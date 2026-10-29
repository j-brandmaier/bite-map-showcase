# Bite Map

An Android app that helps two working parents keep track of what their toddler eats, so they can see at a glance what they've served lately and stop cooking the same few meals.

> **Demo:** `[placeholder]`

<!-- Screenshot strip: Log a meal | History | Photo viewer | Account -->

## The problem

My husband and I want to raise an adventurous eater, not what we jokingly call a "chicken nugget kid." But as two working parents, we kept defaulting to the same meals because neither of us could remember what we'd made recently.

The problem wasn't "track nutrition." It was **"we can't keep track of what we've been serving."**

## Who it's for

Two tech-savvy but busy working parents sharing responsibility for feeding one toddler. Logging has to take seconds or it won't happen, and it can't depend on a network connection.

## My role

I'm a product manager, and I built this using Claude Code for the coding and Google Stitch for design drafts. I:

- wrote the requirements and kept them updated as the scope evolved
- set the product principles the build had to follow
- made the scope and trade-off calls of what to prioritize and what to defer
- directed the design work and judged what the AI tools produced
- tested the app on my own phone

## What v1 does

| Need | Feature |
|---|---|
| Logging has to be fast | Meal type pre-selects from the time of day. The design target is 3 taps or fewer. |
| We don't always know the details yet | A packed lunch can be logged before it's eaten and rated later. A photo can stand in for typing food names. |
| We want to see how it went | A 7-day history shows foods, meal type, photos and a 1–5 "how much was eaten" rating. |
| Variety matters | Each meal can carry up to 3 cuisines and any of six nutrition groups (protein, fruit/veg, grain, dairy, treat, other). |
| We're two people | Either parent can log or edit, and the other sees it within about 30 seconds. Works offline and syncs when it can. |
| Privacy matters | No analytics or trackers. The data lives in a private backend that only the household can read. |

## What I chose not to build yet

The original idea was bigger: a "rut" flag when one cuisine dominated the week, "haven't had in a while" suggestions, a recipe library, and weekly summaries with a star for a varied week. Those features only work once there’s a meal history to analyze, so I shipped the logging first to get something usable into our hands sooner.

→ **[Scope decisions: what I shipped, what I cut, and why](docs/scope-decisions.md)**

## How I worked with AI

I treated Claude Code like a capable engineer who needs clear direction. The setup was a short constitution, an EARS-style spec, a running decisions log, and an instructions file that tells every session to read them first. I used Google Stitch for visual design exploration.

→ **[How I worked with AI tools, including what went wrong](docs/working-with-ai.md)**

→ **[Product principles, and where each one shows up in the app](docs/product-principles.md)**

## Under the hood

- **App:** Expo (SDK 57), React Native, TypeScript, Android
- **Local data:** on-device SQLite, so logging never waits on a connection
- **Shared data:** Supabase (Postgres) with row-level security so each household only sees its own rows
- **Sign-in:** passwordless, using a 6-digit code emailed to the parent
- **Sync:** push on every save, a refresh every 30 seconds while the app is open, last-write-wins on conflicts
- **Running cost:** $0 on free tiers

→ **[Architecture overview](docs/architecture.md)**

## Status

v1 is feature-complete for the logging and history scope and in final testing before my husband and I start using it every day. I'll add what we learn here once we have it. The deferred features (rut flag, suggestions, recipes, weekly summaries) are the roadmap.

## About the code

The code repository is private. This repo holds the case study: the write-up, the video, screenshots, and sanitized copies of the project's planning documents. Happy to walk through the app live upon request.
