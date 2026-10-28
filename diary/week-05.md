# Week 5 - 19 October to 25 October

## Literature review

- [x] Collect 19 papers on multi-agent systems for software engineering, SLMs and agentic CI/CD
- [x] Rank them in a reading list by how much they matter for each thesis objective (O1, O2, O3)

## Planning

- [x] Meet with the supervisors to review the work done so far
- [x] Set the next objectives from that meeting:
  - Build a crawler, so the analysis covers a whole website and not just one page
  - Introduce an orchestrator–worker pattern
  - Work out how to give the agent the context of each use case, since the same problem can be more or less severe depending on the use case


## Test site

- [x] Plant accessibility defects on the test site's home page (vague alt text, "Click here" links, a skipped heading level, a misleading `aria-label`, a Portuguese quote with no `lang`), so the analyzer's findings can be checked against known problems
- [x] Update the Gemini model to `gemini-3.5-flash-lite`

## Agent Skills

- [x] Switch skills to discovery: the agent only sees each skill's name and description, and loads the full instructions with `load_skill` when it needs them
- [x] Add worked examples to the skills as reference files, which the agent can read with `read_skill_file` for hard cases
- [x] Record in each report which skills and files the agent loaded

## Browser interaction

- [x] Give the accessibility agent a Playwright browser, through Pydantic AI, so it can use the page like a visitor (click, hover, type, open menus, dialogs and tabs)
- [x] Re-run `axe-core` and the page content check after each interaction, to catch problems that only show up then, like the errors a form shows
- [x] Use screenshots to decide colour-contrast cases `axe-core` can't decide, like text over an image
- [x] Guard the browser so the agent can't change anything: only GET requests go through and form submissions are blocked

## Site crawler

- [x] Build a static crawler that follows a site's links from its start page and lists every page it finds (status, title, depth, redirects, forms)
- [x] Respect `robots.txt` and `nofollow`, and skip links that look like actions (logout, delete, add to cart)
- [x] Save each crawl and reuse a recent one instead of crawling again

## Orchestrator–worker pattern

- [x] Add an orchestrator that checks every link on a website: a Gemini model crawls the site and sends its pages to `qwen3:4b-instruct` workers running locally on Ollama
- [x] The workers judge each page's links with the workflow approach; the orchestrator reviews the verdicts they failed or were unsure about, reading the evidence and the relevant skill before confirming or changing them
- [x] Avoid repeated work: each URL is requested once, and a link that looks the same on several pages is judged once
- [x] First run on the test site: 30 pages analyzed, 108 items, 20 reviewed by the orchestrator (17 confirmed, 3 changed from fail to pass), in about 27 minutes on CPU
