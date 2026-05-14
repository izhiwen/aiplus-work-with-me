# Changelog

## 0.4.0 — Public template

- First public release of `aiplus-work-with-you` as a fork-and-personalize
  template for AiPlus user profile bundles.
- Generalized identity, project map, and secret alias namespace into
  `[Your Name]`, `~/your-workspace`, and `[your-namespace]` placeholders.
- Documented forking workflow for users who want to convert their personalized
  fork into a private repo.
- Apache-2.0 LICENSE added to match the rest of the public AiPlus module family.
- Added `aiplus-module.json` manifest declaring the public-template boundary.
- No `compatibility_aliases` carried forward — this is a fresh public
  template, not a renamed private profile.
- Profile structure preserved: USER.md / MEMORY.md / preferences/ /
  identities/ / sync/ / docs/, all with the v0.3.1 schema contract.
- No secrets, no provider payloads, no real project paths committed in this
  template; users must fill in their own values after forking.
