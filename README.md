# fennec releases

Official binaries, GitHub Action and pre-commit hooks for **fennec**, the fast local security scanner by [Singularity RD](https://singularityrd.com).

fennec is commercial software. This repository contains **no source code**, only what you need to install and run fennec. Every scan requires a license key. Start a 14-day trial at **<https://fennec.singularityrd.com/pricing>**.

## Install

```sh
npx fennec .                                                 # zero install
pipx install fennec                                          # prebuilt wheels from PyPI
curl -fsSL https://fennec.singularityrd.com/install.sh | sh   # standalone binary (macOS/Linux)
```

Or download a binary for your platform from [Releases](https://github.com/singularityrd/fennec-releases/releases). Verify it against that release's `checksums.txt`:

```sh
sha256sum --check --ignore-missing checksums.txt
```

## Activate your license

```sh
fennec license activate FNC1....   # saves the key and activates this machine
fennec license status              # plan, seats, expiry, activation
fennec license deactivate          # free this machine's slot (e.g. moving to a new laptop)
```

Each seat covers 2 workstations and 5 concurrent CI jobs. In CI and containers, set `FENNEC_LICENSE_KEY` from a secret instead of running `activate`.

## GitHub Action

```yaml
- uses: singularityrd/fennec-releases/action@v0.14.1
  with:
    path: .
    severity: medium
    fail-on-findings: "true"
    upload-sarif: "true"
    license-key: ${{ secrets.FENNEC_LICENSE_KEY }}
```

## pre-commit

Install fennec first, then add to `.pre-commit-config.yaml`:

```yaml
repos:
  - repo: https://github.com/singularityrd/fennec-releases
    rev: v0.14.1
    hooks:
      - id: fennec
      - id: fennec-secrets
```

## Support

- Docs: <https://fennec.singularityrd.com/docs>
- Sales and support: <sales@singularityrd.com>

## License

Copyright (c) 2026 Singularity RD. All rights reserved. Use is governed by the [fennec Commercial License Agreement](LICENSE).
