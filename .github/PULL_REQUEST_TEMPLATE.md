## What does this change?


## Checklist

- [ ] `action.yml` stays a `composite` action with no new runtime dependency (or I've explained why one is needed)
- [ ] Any input used inside `run:` is passed through `env:`, not interpolated directly via `${{ inputs.* }}`
- [ ] `.github/workflows/lint.yml` is green
