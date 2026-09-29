# LoomSeal Verify

Verify a [LoomSeal](https://loomseal.com) evidence bundle in CI. The check runs offline on
the runner with the open Apache-2.0 verifier: signature, chain continuity, anchors, and span
coverage, and the job fails if the bundle does not verify.

## Usage

```yaml
- name: Verify the audit bundle
  uses: kordloom/loomseal-verify-action@v1
  with:
    bundle: audit.loomseal.json
```

With evidence artifacts and a pinned producer key:

```yaml
- name: Verify the audit bundle
  uses: kordloom/loomseal-verify-action@v1
  with:
    bundle: audit.loomseal.json
    evidence: ./evidence
    fingerprint: sha256:2c26b46b68ffc68ff99b453c1d30413413422d706483bfa0f98a5e886266e7ae
```

## Inputs

| Input         | Required | Default  | Purpose                                              |
|---------------|----------|----------|------------------------------------------------------|
| `bundle`      | yes      |          | Path to the `.loomseal.json` bundle to verify        |
| `evidence`    | no       |          | Directory of artifacts checked against the digests   |
| `fingerprint` | no       |          | Required producer key fingerprint, `sha256:<hex>`    |
| `version`     | no       | `latest` | loomseal release to install, such as `v0.5.0`        |

## What the check proves

A verified bundle proves the producer holding the signing key assembled these exact claims,
that the claims sit in an append-only order not rewritten since its heads were anchored, and,
when span claims are present, that the population arithmetic holds, with coverage reported as
a measurement. What it deliberately does not prove is documented in
[the spec](https://loomseal.com/spec). The action downloads the release binary, checks it
against the published SHA256SUMS, and runs entirely on the runner: no account, no service,
and no network access during verification itself.

## License

Apache-2.0, the same as the verifier it runs.
