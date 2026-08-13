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

## Your data

Progress lives in `localStorage`, in one browser, under the key
`edward-ai-roadmap-v1`. It is per-browser and per-URL — a local file and a
GitHub Pages URL are separate stores.

Use **Export** in the sidebar before switching machines or clearing site data,
and **Import** to restore. Export also gives you a plain JSON file you can
commit here if you want history.

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
  concepts:[ "..." ],      // the 20% worth learning
  skip:[ "..." ],          // the 80% you're consciously not learning yet
  build:"...",             // the hands-on task
  resources:[ {t:"docs", n:"name", u:"https://…", note:"…", star:true} ],
  checks:[ {q:"question", rubric:["what a strong answer contains"]} ]
}
```

Adding or removing a concept shifts that module's percentage, since concept
score is an average. Changing an `id` orphans its saved progress.

This roadmap should age. When a module's content stops matching reality, edit
it — that's the point of owning the file.
