# Contributing to send-email-action

Thanks for considering a contribution to this action.

## Bug reports

[Open an issue](https://github.com/msgwing/send-email-action/issues/new) with
the workflow snippet you used (with secrets redacted), the exact error, and
whether it happens on every run or intermittently.

## Feature requests

Open an issue describing the use case. This action intentionally stays a
thin wrapper around `curl`'s SMTP support — no extra runtime dependency — so
requests that would require adding a language runtime or library will be
weighed against that tradeoff.

## Pull requests

```bash
git clone https://github.com/msgwing/send-email-action.git
cd send-email-action
git checkout -b my-fix
```

- Keep `action.yml` a `composite` action with no extra runtime dependencies
  if at all possible — that's what makes this action fast and simple to
  trust.
- Never interpolate `${{ inputs.* }}` directly into a `run:` script body —
  pass inputs through `env:` and reference them as shell variables (see the
  existing steps), to avoid script/command injection from untrusted input.
- The `.github/workflows/lint.yml` CI validates `action.yml` and runs a
  self-test against the real `mx.msgwing.com` (expecting an auth failure
  with a placeholder credential) — make sure it stays green.

## Related project

This action is the CI/CD companion to
[msgwing/ZeroSMTP](https://github.com/msgwing/ZeroSMTP), which has the full
docs, FAQ, and per-language code examples for the underlying relay.
