# API Key Redactor

Paste logs, code, config, or chat transcripts and get back a redacted copy with likely secrets replaced by labeled placeholders like `[REDACTED:aws-access-key]`. Built for the moment before you paste something into a bug report, a support ticket, or a chat. Single HTML file, no server, no tracking, no external dependencies.

## Live demo

https://0xelitesystem.github.io/api-key-redactor/

## Features

Detected secret types:

- Vendor-prefixed keys (AWS, GitHub, OpenAI-style, Google, Slack, Stripe, and more)
- AWS access key IDs (`AKIA...`) and a 40-char AWS secret key heuristic
- GitHub tokens (`ghp_`, `gho_`, `ghu_`, `ghs_`, `ghr_`, `github_pat_`)
- Twilio account SIDs, SendGrid keys, npm tokens
- JWTs (three base64url segments, `eyJ` start)
- Private key blocks (`-----BEGIN ... PRIVATE KEY-----` through the END line)
- Connection string passwords (`protocol://user:password@host`, only the password is redacted)
- Bearer tokens in Authorization headers
- `.env` style lines where the key name contains SECRET, TOKEN, KEY, or PASSWORD (value redacted, key name kept)
- Generic quoted values assigned to suspicious variable names (password, secret, token, apikey, and friends)

Tool features:

- Findings table with type, masked preview (first 4 and last 4 chars), line number, and confidence tier (high for exact vendor prefixes, medium for contextual heuristics)
- Per-finding include/exclude checkboxes so you can drop false positives before copying
- Copy redacted text, download as `.txt`, load a fake-values example, clear everything
- Live scan toggle, keyboard friendly (Ctrl or Cmd plus Enter to scan), dark mode

## How it works

The whole tool is one HTML file with inline CSS and vanilla JavaScript. A list of well-commented regular expressions runs over your pasted text in the browser. Each match is recorded with its position, masked for the preview, and replaced in the output by a `[REDACTED:type]` placeholder. Overlapping matches are resolved by priority, so an exact vendor prefix wins over a generic heuristic covering the same span. Unchecking a finding rebuilds the output with that span left as it was.

## What it is not

- Not a guarantee. Pattern matching misses novel formats, split strings, encoded blobs, and homegrown credentials. Always review the output yourself before sharing.
- Not free of false positives. Hashes, test fixtures, and sample values can look like secrets. That is what the include checkboxes are for.
- Not a replacement for repo-side scanners. Pair it with pre-commit and CI secret scanning; this tool covers the ad hoc copy-paste moment those tools cannot see.

## Privacy

Everything runs in your browser. Nothing you paste is uploaded, stored, or sent anywhere. There are no analytics, no external requests, and no third-party scripts, so the page works offline after a single load. You can verify with the network tab in your browser's developer tools: no requests are made.

## More

Part of a catalog of single-file browser tools and plain-language references, all MIT licensed and dependency-free: [0xelitesystem.github.io](https://0xelitesystem.github.io/). Built by [elitesystem.ai](https://elitesystem.ai).

## License

MIT. Copyright 0xelitesystem 2026.
