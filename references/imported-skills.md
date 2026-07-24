# Imported optimization skills

Installed locally into `C:\Users\akuta\.hermes\skills\`:

## Installed and used as synthesis inputs

1. `token-budget-guard`
   - source: clawhub / community
   - focus: token/context budgeting, progressive disclosure, compression triggers

2. `cost-control`
   - source: clawhub / community
   - focus: budget guardrails, emergency stop, kill-switch ideas

3. `context-compression`
   - source: skills.sh / community
   - focus: compression quality, tokens-per-task framing

4. `prompt-caching`
   - source: skills.sh / community
   - focus: prompt/response caching and invalidation

5. `token-budget-advisor`
   - source: skills.sh / community
   - focus: explicit token-depth tradeoff framing

6. `cost-aware-llm-pipeline`
   - source: skills.sh / community
   - focus: lane-based routing by task complexity

7. `token-optimization`
   - source: skills.sh / community
   - focus: token-efficient tool/API interaction patterns

## Inspected but not imported automatically

8. `hermes-agent-health-check`
   - blocked by scanner with caution verdict

9. `context-budget`
   - blocked as dangerous by scanner

10. `context-degradation`
   - blocked by scanner with caution verdict

11. `token-efficiency`
   - blocked by scanner with caution verdict
   - also contains Claude-specific assumptions that should not become generic Hermes policy

## Policy
The local `hermes-optimizer` skill is a synthesis, not a verbatim clone.
Only safe and broadly reusable ideas should be absorbed into the universal skill body.
Provider-specific or tool-specific mappings belong in examples/references.
