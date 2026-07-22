# DEUSPROOF Notarize — GitHub Action

Prove your AI-made work existed first. On every release, this action timestamps a
creative file and anchors it to **Bitcoin** on a public, append-only ledger — a
permanent, independently verifiable proof of priority. Free, no account, no key.

> The file's **content is recorded on a public permanent ledger**. Point it only at
> content you intend to make public (a generated post, a poem, a design brief, a spec).

## Usage

Add `.github/workflows/notarize.yml` to your repo:

```yaml
name: Notarize on release
on:
  release:
    types: [published]

jobs:
  notarize:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: Abracadabrastartup/deusproof-notarize@v1
        with:
          file: creations/latest.md      # the file to notarize
          handle: my-agent               # who to credit
          model: gpt-4                   # optional
```

The certificate URL appears in the job summary and as an output:

```yaml
      - uses: Abracadabrastartup/deusproof-notarize@v1
        id: proof
        with:
          file: creations/latest.md
      - run: echo "Proof: ${{ steps.proof.outputs.verify-url }}"
```

## Inputs

| Input | Required | Default | What it does |
|-------|----------|---------|--------------|
| `file` | yes | — | Path to the file to notarize (content goes on the public ledger). |
| `handle` | no | repo owner | Author/agent handle to credit. |
| `model` | no | — | The AI model that produced the work. |
| `fail-on-error` | no | `false` | If `false`, a notarization problem warns but never fails your build. |
| `api-base` | no | `https://deusproof.com` | Override only for testing. |

## Outputs

- `verify-url` — permanent public certificate URL.
- `certificate-id` — the certificate id.

## What it proves (honestly)

DEUSPROOF records **existence and time**: the exact bytes, scored (AAS 0–100),
timestamped (RFC 3161), sealed on a hash-chained ledger and anchored to Bitcoin
(OpenTimestamps — the anchor settles over hours). A certificate proves the content
existed at that moment; it does not, by itself, adjudicate authorship. Verify any
certificate independently — the `.ots` Bitcoin proof needs nothing from DEUSPROOF.

Learn more: https://deusproof.com · Agents: https://deusproof.com/skill.md
