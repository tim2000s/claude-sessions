# Review checklist

Each item is a way this tool has gone wrong, or could, stated as something a diff can be
checked against. Label findings important, nit or pre-existing.

- A snapshot write never shrinks the saved set unless `--replace` was given. A half-failed
  restore followed by an automatic save is the failure that loses windows.
- A window is never matched to a transcript that existed before its process started, except
  by an exact `--resume` id. Headless `claude -p` runs leave transcripts in the same directory.
- Only `entrypoint: cli` transcripts count as conversations. `sdk-cli` and `sdk-py` are hooks
  and scripts.
- Anything written into a transcript is appended, one complete JSON record per line, and the
  file is left parseable even when it did not end in a newline. Nothing in a transcript is
  ever rewritten or truncated.
- A transcript belonging to a window that is open is not written to without `--force`, since
  the window appends to the same file and holds its own copy of the state.
- Every failure to identify, position or reopen a window is reported, with the command to
  retry it.
- New behaviour comes with a test that fails against a broken implementation (`python3 -m
  pytest tests`).
