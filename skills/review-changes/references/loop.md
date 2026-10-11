# Loop

| Setting | Default |
|---------|---------|
| Max loops | 4 |

"max N loops" → max_loops = N.

1. Run the [Workflow](../SKILL.md#workflow) through validation.
2. Report to the user:
   ```
   Loop {N}/{max}:
   - {summary of key feedback points}
   ```
3. Check stop conditions, in this order:
   1. If the loop ended with no validated **Must fix** or **Should fix** finding → **stop, go to the report**
   2. If loops completed >= max_loops → **stop, go to the report**
   3. Otherwise → **revise and loop**
4. Revise: fix the validated **Must fix** and **Should fix** findings. **Nice to have** findings are optional. Each revision should be targeted. Don't rewrite everything — fix what the reviewer flagged. Maintain a list of ALL prior feedback to avoid regressions.
5. Go back to step 1.

If the same finding survives 2 consecutive loops, stop and ask the user.

**Never** continue past max_loops without user consent.
