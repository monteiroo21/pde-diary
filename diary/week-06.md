# Week 7 - 26 October to 1 November

## Literature review

- [x] Read "A Comprehensive Survey on Benchmarks and Solutions in Software Engineering of LLM-Empowered Agentic System"
- [x] Read "Automated Unit Test Improvement using Large Language Models at Meta"
- [x] Read "FlexFL: Flexible and Effective Fault Localization With Open-Source Large Language Models"
- [x] Read "Small Models, Big Tasks: An Exploratory Empirical Study on Small Language Models for Function Calling"
- [x] Read "An Empirical Evaluation of Using Large Language Models for Automated Unit Test Generation"
- [x] Read "MetaGPT: Meta Programming for a Multi-Agent Collaborative Framework"
- [x] Read "ChatDev: Communicative Agents for Software Development"
- [x] Read "IRIS: LLM-Assisted Static Analysis for Detecting Security Vulnerabilities"

## Sprint 2

- [x] Prepare the presentation for Sprint 2 of PD/E

## Orchestrator speed

- [x] Add timing to the reports, split into worker, orchestrator and fetching time
- [x] Batch worker questions across pages: 33 requests became 19, but the run was no faster
- [x] Find where the time goes: the workers writing their answers (about 7 tokens/s on CPU)
- [x] Try two worker requests in parallel on Ollama: no faster on CPU because then each worker had half of its capability (~3.5 tokens/s), so it was dropped 
- [x] Shorten the workers' reasons to the one fact that decides each verdict, and shorten the broken-links notes: about a third fewer tokens written
- [x] Ask the workers for compact JSON, one answer per line: another drop on tokens written
- [x] Result: the full link check went from about 27 to 18 minutes, still catching all 16 planted problems, with 1 false alarm

## Accessibility with local workers

- [x] Score the `qwen3:4b-instruct` accessibility workflow against the 11 defects planted on the home page: 8/11 with 1 false alarm, in about 20 minutes
- [x] Try the shorter reasons there too: twice as fast, but only 6/11, so they stay for links only
- [x] Keep compact JSON and shorter `axe-core` notes instead: still 8/11 (2 false alarms), in about 10 minutes
- [x] Two defects are missed in every qwen run: the "Find out more" link with no context, and the Portuguese review with no `lang`

## Orchestrator–worker pattern

- [x] Add the accessibility check to the orchestrator, so it checks both a site's links and its accessibility
- [x] Work in rounds: each round sends one batch of pages to the workers, and the orchestrator reviews every item from it before starting the next. Earlier rounds are cleared from its context to save space, and it can read them again with `get_evidence`
- [x] Send it the accessibility passes with the worker's reason, since most of the workers' mistakes are false positives
- [x] Add a `look_at` tool that shows the orchestrator a screenshot of an element on its page, since the workers and `axe-core` only read the code
- [x] First full run on the test site: 30 pages through both checks in 6 rounds, 54 items reviewed by the orchestrator (28 confirmed, 26 changed from fail to pass), in about 79 minutes on CPU
