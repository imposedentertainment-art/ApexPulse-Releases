# ApexPulse — Official Android Releases

This is the official public distribution repository for **ApexPulse**, the native Android sim-racing application from Imposed Entertainment.

> **Current stable release:** **ApexPulse 1.6.0** (versionCode 10). The signed APK and verified in-app update manifest are published.

## Download ApexPulse

- **[All releases](https://github.com/imposedentertainment-art/ApexPulse-Releases/releases)**
- **[Latest stable release](https://github.com/imposedentertainment-art/ApexPulse-Releases/releases/latest)**

Download the **[ApexPulse 1.6.0 APK](https://github.com/imposedentertainment-art/ApexPulse-Releases/releases/download/v1.6.0/ApexPulse-1.6.0.apk)** from the official release. Android will ask you to confirm installation.

## In-app updates

ApexPulse can check this repository for optional updates using the public manifest:

`https://raw.githubusercontent.com/imposedentertainment-art/ApexPulse-Releases/main/latest.json`

The manifest currently identifies version 1.6.0 (versionCode 10), minimum supported version 1.2.1, the exact APK URL, SHA-256 checksum and release notes. A new manifest is committed only after the hosted workflow independently downloads the release asset and verifies its checksum.

The app verifies the downloaded file before handing it to Android's package installer. Android still requires the user to approve installation. Updates must retain the ApexPulse package ID and signing identity.

## Privacy and connectivity

ApexPulse is designed to keep simulator and session functionality on the device. This repository provides the optional update channel; it is not an application backend or account service. **Simulator discovery and UDP telemetry require local Wi-Fi.** Update checks and APK downloads use Android's default internet network and can use Wi-Fi or mobile data. ApexPulse does not deliberately block cellular data for update traffic.

## Release integrity

Official APKs are distributed as GitHub Release assets rather than committed to Git history. Each stable release must be:

1. Built from a reviewed source commit in the private [ApexPulse source repository](https://github.com/imposedentertainment-art/ApexPulse).
2. Signed with the established ApexPulse signing key.
3. Checked for package ID, version code/name, APK alignment, and signature validity.
4. Checked for in-place upgrade compatibility on a physical Android device before the release is treated as device-accepted. The hosted workflow does not currently automate this physical-device check.
5. Hashed with SHA-256.
6. Uploaded as a GitHub Release asset and downloaded again for independent checksum verification.
7. Reflected in `latest.json` only after the uploaded asset has passed verification.

See [RELEASE-CHECKLIST.md](RELEASE-CHECKLIST.md) for the publication gates and [BUILD-SETUP.md](BUILD-SETUP.md) for the one-time setup of the hosted build and in-app publishing workflow. The workflow runs on this public distribution repository so standard hosted runners do not consume the private source repository's Actions allowance.

## Repository contents

- GitHub Releases: signed APK distribution.
- `latest.json`: live public in-app update manifest for ApexPulse 1.6.0 and later verified releases.
- `RELEASE-CHECKLIST.md`: maintainer release procedure.
- `SECURITY.md`: vulnerability reporting guidance.
- `SUPPORT.md`: user support guidance.
- `.github/workflows/build-apexpulse.yml`: manual hosted build, verification and release publication workflow.
- `BUILD-SETUP.md`: required one-time secret setup.

## Source code

The application source is maintained separately in the private [ApexPulse repository](https://github.com/imposedentertainment-art/ApexPulse). This repository contains distribution materials, not the application source.

## Current release details

- Version: `1.6.0`
- Android version code: `10`
- APK: `ApexPulse-1.6.0.apk`
- SHA-256: `05fb9c757abbeba41706229c87d4c0ce5fb0dbe42fab04e0452291e39b716022`
- Minimum supported version in the update manifest: `1.2.1`

## Important

Only install ApexPulse APKs published by the official Imposed Entertainment account in this repository. Do not install unofficial APKs or share signing keys, keystores, passwords, or release credentials.
