# Findings

What we learned the expensive way ([SE1/W10](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/08-work.md#w10)). Blameless: say what would have found it, not who
should have asked.

## 2026-11-19 — A blanket rename of the checker changed a word it should not have

- **What happened:** the checker was ported from DM1 by replacing `DM` with `DO` throughout. That
  also turned every `README` into `READOE`, so the checker would have looked for a file that does
  not exist.
- **Kind:** K2 (surprise)
- **Cost:** minutes; it was seen in the first listing of the result, before the checker was run.
- **Findable by:** reading the replacement's output, which is how it was found; or by running the
  checker, which would have failed on the missing file.
- **Changed:** nothing in the repository. The next port should replace `DM1` and `DM[0-9]`, not
  two letters.
