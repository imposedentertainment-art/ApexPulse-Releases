# ApexPulse — Official Android Releases

This is the official public distribution repository for **ApexPulse**, the native Android sim-racing application from Imposed Entertainment.

> **Release status:** The repository is prepared for distribution. No official APK release is published until a signed build has passed the release checks below.

## Download ApexPulse

- **[All releases](https://github.com/imposedentertainment-art/ApexPulse-Releases/releases)**
- **[Latest stable release](https://github.com/imposedentertainment-art/ApexPulse-Releases/releases/latest)**

When a release is available, download the APK asset attached to the latest stable GitHub Release. Android will ask you to confirm installation.

## In-app updates

ApexPulse can check this repository for optional updates using the public manifest:

`https://raw.githubusercontent.com/imposedentertainment-art/ApexPulse-Releases/main/latest.json`

The manifest is intentionally not present until the first verified APK has been published. It will contain the release version, Android version code, minimum supported version (when applicable), exact APK download URL, SHA-256 checksum, and release notes.

The app verifies the downloaded file before handing it to Android's package installer. Android still requires the user to approve installation. Updates must retain the ApexPulse package ID and signing identity.

## Privacy and connectivity

ApexPulse is designed to keep simulator and session functionality on the device. This repository provides the optional update channel; it is not an application backend or account service. Checking for updates and downloading a release require an internet connection.

## Release integrity

Official APKs are distributed as GitHub Release assets rather than committed to Git history. Each stable release must be:

1. Built from a reviewed source commit in the private [ApexPulse source repository](https://github.com/imposedentertainment-art/ApexPulse).
2. Signed with the established ApexPulse signing key.
3. Checked for package ID, version code/name, APK alignment, and signature validity.
4. Installed over an existing build to verify upgrade compatibility and basic device operation.
5. Hashed with SHA-256.
6. Uploaded as a GitHub Release asset and downloaded again for independent checksum verification.
7. Reflected in `latest.json` only after the uploaded asset has passed verification.

See [RELEASE-CHECKLIST.md](RELEASE-CHECKLIST.md) for the publication procedure. GitHub Actions are not required; releases are published manually.

## Repository contents

- GitHub Releases: signed APK distribution.
- `latest.json`: public in-app update manifest, added only after a real release is verified.
- `RELEASE-CHECKLIST.md`: maintainer release procedure.
- `SECURITY.md`: vulnerability reporting guidance.
- `SUPPORT.md`: user support guidance.

## Source code

The application source is maintained separately in the private [ApexPulse repository](https://github.com/imposedentertainment-art/ApexPulse). This repository contains distribution materials, not the application source.

## Important

Only install ApexPulse APKs published by the official Imposed Entertainment account in this repository. Do not install unofficial APKs or share signing keys, keystores, passwords, or release credentials.
