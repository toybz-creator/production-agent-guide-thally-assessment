# Build observations

These observations are based on the actual scaffold, content authoring, and disposable package check for this assessment.

1. **What worked well:** Thally’s supported CLI created a Git-ready docs project with clear content/configuration boundaries. Its starter renders Mermaid natively in MDX, and the content checker caught issues before publication.
2. **Where I encountered friction:** The Cloud-first quickstart recommends an all-repositories GitHub App grant, while this project needs a single-repository scope, so the CLI scaffold and existing-repository connection added a step. The first GitHub-to-Thally callback also failed to resolve `app.thally.io`; reopening setup and using **Check GitHub** with the already-authorized `toybz-creator` account recovered the connection. The scaffold installed 757 packages in about 57 seconds and printed two npm deprecation warnings, though installation succeeded.
3. **What I would improve:** `npx pag help` did not list the documented `pag task` command, although the command worked with a date-prefixed ID. The production build also logged a skipped embeddings step and a deprecated Next.js middleware convention; clearer release notes or fixes would help teams distinguish expected starter messages from project errors.
