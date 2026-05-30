# Copilot instructions for `uping`

- Keep changes in portable C and follow the existing style in `uping.c`.
- Prefer the smallest correct change; avoid unrelated refactors.
- Do not introduce new dependencies unless the repository already needs them.
- Keep command-line flags, output formatting, and timing behaviour stable unless the task explicitly changes them.
- Update `README.md` when user-facing behaviour or build/install steps change.
- Use ASCII by default and only add comments where the code is not self-explanatory.
- Validate with `make` and the relevant runtime example when behaviour changes.
