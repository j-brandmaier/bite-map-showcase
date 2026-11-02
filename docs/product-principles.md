# Product & Engineering Principles

These principles are documented in the project's constitution, which every development session starts by referencing. They served as my explicit tiebreakers whenever two valid product ideas conflicted.

---

## 1. Speed over completeness at the point of logging

*Target velocity: No more than 3 taps from app launch to a saved entry for repeat meals.*

- **Smart Sorting:** Recently used foods appear at the top of the item picker.
- **Auto-Positioning:** Meal type chips auto-scroll so the current context is immediately visible.
- **Frictionless Entry:** Photo-only entries are supported, letting a rapid photo stand in for manual food selection.
- **Optional Metadata:** Mandatory cuisine and nutrition group tagging was removed after initial testing showed it added friction.

---

## 2. Infer, don't ask, when confidence is high

*The parent confirms or overrides—they should never start from a blank form if data can be reasonably inferred.*

- **Time-Based Defaults:** Meal type automatically pre-selects based on local time (covering all 24 hours without gaps), requiring only a single tap to override.
- **Predictive Ranking:** Search results prioritize items frequently served in recent days.
- **Contextual Date Selection:** Date defaults automatically to the current calendar day.

---

## 3. Editable after the fact, with zero penalty

*Logging must never be blocked by information that isn't known at the moment of entry.*

- **Deferred Ratings:** Meals can be logged without a consumption score and updated later. Unrated meals display a clear, amber "No rating" status indicator.
- **Advance Entry:** Packed daycare lunches can be logged at 8:00 AM before the meal occurs.
- **Non-Destructive Editing:** Meals can be edited or deleted at any time without resetting the original entry timestamp, preserving chronological feed ordering.

---

## 4. Accurate about what's tracked, not what's estimated

*History, ratings, and category tags are strictly accurate. No guessed numbers or false precision.*

- **Qualitative Taxonomy:** v1 tracks broad nutrition groups (protein, fruit/veg, grain, dairy, treat, other) rather than caloric or macro estimates. Caloric calculations require external database lookups and introduce estimation errors that erode user trust.
- **Human-Centric Metrics:** Consumption ratings represent human observation ("how much was eaten"), explicitly avoiding automated estimation models.

---

## Engineering Ground Rules as Product Choices

- **Zero Ongoing Cost:** Operates exclusively within free-tier resource limits. Any paid dependency requires an explicit architectural RFC and logged trade-off decision.
- **Maintainable & Standardized Stack:** Selected widely adopted, standard tooling (Expo, React Native, SQLite, Supabase) to ensure long-term platform maintainability.
- **Privacy by Design:** Built for a toddler's daily data. Zero third-party analytics, behavioral trackers, or advertising SDKs. Data is strictly isolated via backend Row-Level Security (RLS).
