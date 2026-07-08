---
status: active
failure: public-package-hygiene
---

# Failed Case: Public Package Hygiene Drift

## Wrong

The generated page or skill content includes local paths, screenshots, test project names,
private repository names, credentials, evaluation prompts, or notes about how the skill was
developed.

## Why It Fails

This skill is intended for public reuse. Private or process-specific context makes the
package confusing, non-portable, and unsafe to publish.

## Correct

- Keep only public, portable UI rules and public package guidance.
- Refer to this skill folder, `@kubed/components`, `@kubed/icons`, and public KubeSphere
  terminology.
- Do not mention local machines, private source trees, live environments, tests, or
  development history.

## Checklist

- No local filesystem paths.
- No private repository names.
- No account, password, IP, or session identifiers.
- No screenshot/test/evaluation prompts.
- No commentary about how the rules were derived.
