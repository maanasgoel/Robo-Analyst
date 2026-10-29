# Robo-Analyst

Can an AI replicate a fundamental analyst's calls from the documents the analyst actually read, without cheating?

This proof of concept runs end to end on 1,700+ public SEC earnings filings (634 earnings events, 40 stocks). The LLM only reads: nine narrow agents extract quote-verified changes from each masked filing, and a separate blinded model, trained on analyst rating history, makes every call without seeing any text or company names.

## Design

- **As-of gate:** only documents available before each decision, timed to when EDGAR actually releases a filing
- **Masking:** company names, tickers, people, and products removed; dollar levels turned into relative changes
- **Extraction agents:** nine narrow "what changed" agents with strict schemas, where code checks every quote against the source and drops claims that don't match
- **Master agent:** asks follow-up questions until new answers stop moving the call
- **Blinded decision model:** never sees text or identity, so LLM recall can't leak into the call
- **Leakage audit:** a 2×2 design (masked vs. named, before vs. after the model's training cutoff), validated in a 500-company simulation where the true leakage is known

## Key finding

Masking barely hides identity. On real filings, the LLM still recognized **97% of megacaps** and **77% of small and mid caps**, so masking alone cannot make a pre-cutoff backtest truly out of sample.

## Status

The pipeline runs end to end. With only 19 post-release events, the sample is too small to show predictive edge, and the results are reported with confidence intervals that include zero. Code is private.

## Links

- [Interactive demo](https://claude.ai/artifact/MeeFTthABfohXsVkUv4EYG)
- [Research, design, and build plan](https://claude.ai/artifact/THB6YD6zgaQf5VeKz32QeJ)
