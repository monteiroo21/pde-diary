# Week 7 - 2 November to 8 November

## Planning

- [x] Meet with the supervisors to review the work done so far. Their feedback:
  - The current architecture isn't quite what they had in mind: the orchestrator, an LLM (Gemini), reviews the workers' verdicts and has the final say on them
  - It is still useful, as a baseline to compare the new architecture against
  - The thesis is about reducing the problems that come from relying on LLMs, so the responsibility must move from the LLM to the SLMs
- [x] Set the plan for the next weeks:
  - Analyse an existing framework, the **Agentic QE Framework**
  - Build a fleet of sub-agents, each an SLM specialised in one subtask, with its own knowledge, skills and prompts. Most existing solutions are built this way
  - The LLM only delegates the work to the sub-agents; the decisions come from the SLMs

## Sprint 2

- [x] Finish the slides and present Sprint 2 of PD/E: the review of the state of the art so far

## Report page

- [x] Build an HTML page from each JSON report, so the results can be read without going through the JSON
- [x] The page opens with a summary and totals, then lists the problems grouped by type. Each problem shows its impact, the WCAG criterion, where it is, why it fails, a suggested fix, and what found it (the link checker, `axe-core`, a worker or the orchestrator)
- [x] Further sections list what still needs review, the false alarms the orchestrator removed, the notices, and every page's status
- [x] Ask the orchestrator for an impact and a suggested fix on each problem it decides.

## Orchestrator speed

- [x] Look into context compaction to speed up the runs: it wouldn't help, since only the orchestrator keeps a conversation, finished rounds are already cleared from it, and the workers take 93% of the time
- [x] Ask the workers for each verdict as a list, `["h-1", reason, verdict]`, instead of a JSON object: 23% fewer tokens written (12,850 against 16,690)
- [x] Full run on the test site: about 69 minutes, against 79 last week. The fewer tokens account for about 9 minutes; the rest came from the laptop running faster that day
- [x] Without an example of the list in the prompt, a worker flagged 18 pages for having no `lang`, although all of them have `lang="en"`. Adding one example answer back to the prompt removed all of those false alarms

## Orchestrator–worker pattern

- [x] Show the orchestrator the facts of the accessibility passes too, so it can catch problems the workers missed
- [x] Draw a diagram of this architecture: Gemini crawls the site, sends the pages to the checks in rounds, reviews the workers' verdicts and submits the report. The crawler, link checker, `axe-core` and evidence tools are plain code, and the workers are `qwen3:4b-instruct` running locally
