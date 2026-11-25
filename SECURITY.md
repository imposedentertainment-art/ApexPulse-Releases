# Security Policy

## Reporting a vulnerability

Please do not publish exploitable vulnerability details in a public issue.

Use GitHub's **Report a vulnerability** option on this repository if private vulnerability reporting is enabled. If it is not available, contact the repository owner privately through GitHub and request a secure channel.

Include:
- The affected ApexPulse version and Android version.
- A clear description of the issue and its potential impact.
- Reproduction steps or a proof of concept, where safe.
- Any suggested mitigation.

Do not include passwords, signing keys, keystores, access tokens, or other secrets in a report.

## APK authenticity

Only treat APKs attached to releases in this official repository as official distributions. Verify the release asset and published SHA-256 value. ApexPulse's updater also validates the downloaded APK before invoking Android's installer.

Security reports are reviewed as capacity allows. Please avoid publicly disclosing an unpatched issue before coordinating with the maintainer.
