# fennec releases

Official binaries, GitHub Action and pre-commit hooks for **fennec**, the fast local security scanner by [Singularity RD](https://singularityrd.com).

fennec is commercial software. This repository contains **no source code**, only what you need to install and run fennec. Every scan requires a license key. Start a 14-day trial at **<https://fennec.singularityrd.com/pricing>**.

## Install

| Platform | How |
| --- | --- |
| Linux x86_64 / ARM64 | `curl -fsSL https://fennec.singularityrd.com/install.sh \| sh` |
| Windows x86_64 | Download `fennec-windows-x86_64.exe` from [Releases](https://github.com/singularityrd/fennec-releases/releases) and put it on your `PATH` as `fennec.exe` |
| macOS | Coming in the next release |
| npm (`@singularityrd/fennec`), PyPI (`fennec-scanner`), VS Code | Coming soon |

The install script verifies the binary against the release's `checksums.txt`.

Do not install the unscoped `fennec` packages from npm or PyPI: they belong to unrelated projects.

## Verify a download

Every release ships `checksums.txt`, a CycloneDX SBOM (`fennec-<version>.cdx.json`), full third-party license texts, and a [minisign](https://jedisct1.github.io/minisign/) signature (`.minisig`) for each file. Release signing key:

```
RWQRdwdoY/8UKPJ+jJpn1qDxJKmsP/RtfAY/g+YNfIscvjXOOIy9feQn
```

```sh
minisign -Vm checksums.txt -P RWQRdwdoY/8UKPJ+jJpn1qDxJKmsP/RtfAY/g+YNfIscvjXOOIy9feQn
sha256sum --check --ignore-missing checksums.txt
```

The same key is published at <https://fennec.singularityrd.com/release-minisign.pub>.

## Activate your license

```sh
fennec license activate FNC1....   # saves the key and activates this machine
fennec license status              # plan, seats, expiry, activation
fennec license deactivate          # free this machine's slot (e.g. moving to a new laptop)
fennec doctor                      # check license, network, Git and config
```

Each seat covers 2 workstations and 5 concurrent CI jobs. In CI and containers, set `FENNEC_LICENSE_KEY` from a secret instead of running `activate`. Behind a proxy, set `HTTPS_PROXY`. `fennec license status --show-telemetry` shows exactly what activation sends.

## GitHub Action

```yaml
- uses: singularityrd/fennec-releases/action@v0.15.1
  with:
    path: .
    severity: medium
    fail-on-findings: "true"
    upload-sarif: "true"
    license-key: ${{ secrets.FENNEC_LICENSE_KEY }}
```

The Action runs on Linux and Windows runners.

## pre-commit

Install fennec first, then add to `.pre-commit-config.yaml`:

```yaml
repos:
  - repo: https://github.com/singularityrd/fennec-releases
    rev: v0.15.1
    hooks:
      - id: fennec
      - id: fennec-secrets
```

## Support

- Docs: <https://fennec.singularityrd.com/docs>
- Security reports: <https://fennec.singularityrd.com/security>
- Sales and support: <anil@singularityrd.com>

## License

Copyright (c) 2026 Singularity RD. All rights reserved. Use is governed by the [fennec Commercial License Agreement](LICENSE).
