# Edward's AI Roadmap

A personal 80/20 learning roadmap — from LLM fundamentals to shipping agentic
systems into real work processes. Eleven modules, roughly 130 focused hours.

One self-contained `index.html`. No build step, no backend, no dependencies.
Open it in a browser and it works.

## Run it

```
open index.html          # macOS
xdg-open index.html      # Linux
```

## Published site

<https://littlewave.github.io/Edward-AI-learning-roadmap-/>

Served straight from `main` via **Settings → Pages → Source: Deploy from a
branch → `main` / `(root)`**. No build step and no Actions workflow — GitHub
serves `index.html` as-is, and any push to `main` republishes within a minute.

`.nojekyll` is present so GitHub skips Jekyll processing and copies the file
through untouched.

## How progress works

Each module scores on three things, weighted deliberately:

| Component | Weight | What it means |
|---|---|---|
| Concepts | 35% | Self-rated: Learning → Can explain → Can teach |
| Build | 40% | The hands-on task. Binary. The heaviest single lever. |
| Checkpoint | 25% | Three questions, graded PASS / PARTIAL / RETRY |

Overall progress is weighted by each module's estimated hours, so finishing a
6-hour module doesn't count the same as an 18-hour one.

**"Can explain"** means you could explain it to a sceptical colleague with no
notes and survive one follow-up question. Anything less is "Learning". The
number is only worth what your honesty is worth.

### Checkpoints

Write your answer cold, then hit **Copy grading prompt**. That builds a prompt
containing the question, the rubric, and your answer, instructed to grade
strictly — paste it into Claude or Copilot and record the verdict it gives you,
not the one you'd prefer. This is deliberately not multiple choice: recall tests
measure the wrong thing for applied work.

Nothing is ever locked. The order is a recommendation; modules 02 (Evaluation)
and 04 (Tool Calling) are the two that make everything after them cheaper.
The dashboard's module map shows the real dependency graph — after 04 the
path branches, and concepts link sideways via "connects to" chips.

## How reviews work (spaced repetition)

Reviews are the app's main event. Any concept you've started (level ≥
Learning) enters the review pool; the **Review** page serves whatever is due,
core concepts first, capped at 15 cards a session.

Each concept card is a Feynman rep: type the explanation from memory, flip to
compare against the essence, then grade yourself —

| Grade | Effect |
|---|---|
| Solid | Interval grows (1 → 3 → 7 days, then ×2.3, capped at 60). Two Solids in a row promote a Learning concept to "Can explain". |
| Shaky | Interval shrinks to ~60%; streak resets. |
| Forgot | Back tomorrow — and the concept's level honestly drops one notch. |

Passed checkpoints re-enter the queue a week later; "Need to redo" downgrades
the verdict to partial (your module % drops) until you re-pass it via the
grading flow. Progress is therefore earned by retrieval, not by clicking.

Checkpoints themselves come in three depths: **LV1 Recall** (retrieve it),
**LV2 Reasoning** (why it works), **LV3 Transfer** (apply it to a new
scenario) — and the copied grading prompt tells the LLM to grade at that
depth. Concepts marked **core** carry triple weight in module progress; the
per-concept tutor prompt quizzes along the same recall → reasoning → transfer
ladder.

## Your data

Progress lives in `localStorage`, in one browser, under the key
`edward-ai-roadmap-v1`. It is per-browser and per-URL — a local file and a
GitHub Pages URL are separate stores.

Use **Export** in the sidebar before switching machines or clearing site data,
and **Import** to restore. Export also gives you a plain JSON file you can
commit here if you want history.

## Cross-device sync

The sidebar's **Set up sync** connects the app to a **private GitHub Gist**
that holds the same JSON Export produces. After a one-time token paste per
device, the app pulls the latest progress on open and auto-pushes a few
seconds after every change; **Sync now** forces a round-trip.

- Token: create a personal access token with **only the gist scope**
  (github.com → Settings → Developer settings → Personal access tokens).
  It is stored in that browser's `localStorage` only — don't connect on a
  shared computer. Disconnect clears it.
- First device creates the gist; other devices paste the same token and the
  app finds the gist by its filename (`ai-roadmap-progress.json`).
- Conflicts are last-write-wins by timestamp — fine for one person, but
  finish on one device before picking up another.
- Local storage remains the source of truth: if GitHub is unreachable the
  app keeps working and shows "Offline — saved locally".
- The claude.ai artifact viewer blocks external requests, so sync only works
  on the GitHub Pages copy or a local file.

## Keeping the curriculum current

AI moves monthly; the content here was authored August 2026. Each module
tracks a **verified** date (the authoring date, or your last audit) with
three tiers: fresh (< 4 months), check soon (4–8 months), audit overdue
(> 8 months). Stale modules are listed on the dashboard's "Keeping current"
card.

To audit a module: open its **Freshness** card → **Copy audit prompt** →
paste into Claude with web search enabled. The prompt carries the module's
claims, resources and skip-list and demands tagged one-line findings
(`[RESOURCE]`, `[CLAIM]`, `[NEW]`, `[SKIP]`, `[OK]`). Paste the result back
into the card and **Mark audited** — the findings stay visible (dated) while
you study, and the module counts as verified from that day.

Saved findings are an overlay, not a content change: when an audit flags
something real, make the permanent fix in the `ROADMAP` array (below).

## Editing the curriculum

The content is a single `ROADMAP` array near the top of the `<script>` block.
Each module:

```js
{
  id:"04",                 // keep stable — progress is keyed off this
  title:"...",
  hours:14,
  tier:"Core Mechanics",
  keystone:true,           // optional flag
  why:"...",               // why this module earns its hours
  concepts:[               // the 20% worth learning
    {t:"the concept",      // shown in the list
     e:"the essence",      // 2–3 sentences: the core idea
     g:"you've got it when…", // the concrete bar for 'can explain'
     r:0}                  // index into resources[] — where to learn it
  ],
  skip:[ "..." ],          // the 80% you're consciously not learning yet
  build:{goal:"...", steps:["..."], done:"what finished looks like"},
  resources:[ {t:"docs", n:"name", u:"https://…", note:"…", star:true} ],
  checks:[ {q:"question", hint:"which concepts it draws on and the essence needed",
            rubric:["what a strong answer contains"]} ]
}
```

Each concept also generates a **tutor prompt** (copy button inside the
expanded row): a prompt that has Claude teach that single concept with a
work-grounded example, quiz you three questions deep, and grade you against
the concept's "you've got it when" bar.

Adding or removing a concept shifts that module's percentage, since concept
score is an average. Changing an `id` orphans its saved progress.

This roadmap should age. When a module's content stops matching reality, edit
it — that's the point of owning the file.
