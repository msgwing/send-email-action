# ZeroSMTP — Send Email (GitHub Action)

Send an email from a GitHub Actions workflow through the free
**[ZeroSMTP](https://github.com/msgwing/ZeroSMTP)** relay (`mx.msgwing.com`) —
no paid transactional-email service, no SMTP server to run, and no
volume-based pricing tiers. A composite action wrapping `curl`'s built-in
SMTP support — no extra runtime or dependency installed on the runner.

## Usage

```yaml
- name: Notify on failure
  if: failure()
  uses: msgwing/send-email-action@v1
  with:
    username: ${{ secrets.ZEROSMTP_USERNAME }}
    password: ${{ secrets.ZEROSMTP_PASSWORD }}
    from: ${{ secrets.ZEROSMTP_USERNAME }}
    to: you@example.com
    subject: "CI failed on ${{ github.repository }}"
    body: "Workflow ${{ github.workflow }} failed: ${{ github.server_url }}/${{ github.repository }}/actions/runs/${{ github.run_id }}"
```

## Setup

1. Register a free account at [msgwing.com](https://msgwing.com) to get SMTP
   credentials (a random `@msgwing.com` address + password).
2. Add `ZEROSMTP_USERNAME` and `ZEROSMTP_PASSWORD` as
   [repository secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets)
   — never hardcode them in the workflow file.
3. Add the step above to any workflow (build failures, deploy notifications,
   scheduled reports, etc.).

## Inputs

| Input | Required | Default | Description |
| --- | --- | --- | --- |
| `username` | yes | | ZeroSMTP SMTP username |
| `password` | yes | | ZeroSMTP SMTP password |
| `from` | yes | | From address (usually the same as `username`) |
| `to` | yes | | Recipient address(es) — comma-separated for multiple |
| `subject` | yes | | Email subject |
| `body` | yes | | Plain-text email body |
| `port` | no | `465` | `465` (implicit SSL/TLS) or `587` (STARTTLS) |

## Good to know

- Mail is always sent **from a shared `@msgwing.com` address**, never from
  your own domain — fine for CI/CD notifications, not a fit if the "From"
  needs to show your own domain. See the
  [FAQ](https://github.com/msgwing/ZeroSMTP/blob/main/docs/FAQ.md).
- The relay is rate-limited (200 emails/day, 50/hour, 5/minute) to keep
  deliverability good for everyone — see
  [Sending limits](https://github.com/msgwing/ZeroSMTP/blob/main/docs/TROUBLESHOOTING.md#sending-limits-rate-limiting).
- Subject/From/To are stripped of embedded line breaks before use, to
  prevent header injection if any of those values come from
  less-trusted input (e.g. a PR title).

## License

MIT — see [LICENSE](LICENSE).
