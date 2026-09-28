# improved_token_effciency
Info from Eric Zakariasson (https://twitter.com/ericzakariasson)

here's a prompt to improve your agent harness based on what we've learned at cursor. enjoy

# Improve this agent harness's token efficiency

You're working on an LLM agent harness: the system prompt, tool definitions, request assembly, context caching, compaction, and retrieval, and how work is split across agents. Make the agent's runs cheaper without making it worse at its job.

- Objective: lower price-weighted token cost per completed task.
- Constraint: no measurable drop in task quality.

Measure per task, not per request. Every turn resends the prefix (tools, instructions, setup, and the conversation so far), so a change that shrinks each request but adds turns can cost more. Weight tokens by billing type: output, uncached input, and cached input are priced very differently.

Work in this order: map the harness and measure the baseline, rank the opportunities, make the changes that are safe to make directly, put the rest behind flags or in proposals, then report.

Figures below come from one team's production coding agent and its multi-agent experiments. Use them to gauge magnitude, not as targets. One round of these changes (prompt trimming, tool offloading, cache layout, sparse line numbers, subagent tuning) cut that team's overall token cost about 7% with no loss in quality. The larger percentages apply only to the part of the request each change touched.
