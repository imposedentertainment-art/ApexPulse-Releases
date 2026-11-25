# ApexPulse hosted build setup

This repository now contains a hosted Android build and release workflow at **Actions → Build and publish ApexPulse**.

It runs on GitHub's standard hosted Linux runner from this **public release repository**, so it does not consume the exhausted private-source-repository Actions allowance. GitHub documents standard hosted runner use in public repositories as free and unlimited:
https://docs.github.com/en/actions/concepts/billing-and-usage

## One-time setup

The workflow deliberately requires two repository secrets. It will not build or publish without them.

Open **Settings → Secrets and variables → Actions → New repository secret** in this repository.

### 1. A read-only token for the private source repository

Create a fine-grained personal access token in GitHub:
- Resource owner: `imposedentertainment-art`
- Repository access: only `ApexPulse`
- Repository permissions: **Contents: Read-only**
- No write, Actions, administration, or release permissions

Save it as:

`APEXPULSE_SOURCE_TOKEN`

The token is only used to check out the private source. It is not written into the repository or APK.

### 2. The existing signing keystore

Save a Base64-encoded copy of the existing ApexPulse debug keystore as:

`APEXPULSE_SIGNING_KEYSTORE_B64`

**Do not generate a new key. Do not upload the keystore to this public repository. Do not send the keystore or its encoded contents in chat.** The workflow checks the certificate SHA-1 against the currently installed app's established identity and refuses to build if it differs.

The current Termux signing backup is normally at:

`~/ApexPulse-signing-backup/apexpulse-release.keystore`

Because Android keeps Termux's private home separate from shared storage, exporting this key to a location that can be selected in a browser/file picker requires a one-time local export. After the secret is configured, routine builds and releases do not require Termux.

The expected keystore uses alias `androiddebugkey` and password `android`, matching the existing build configuration. If the backup has different credentials, the workflow must be adjusted before use.

## Running a build

1. Open **Actions** in this repository.
2. Select **Build and publish ApexPulse**.
3. Choose the private source ref. Use a feature branch for test builds; use `main` only after the source has been reviewed and promoted.
4. Leave **Publish this verified build** disabled for a test build.
5. Run the workflow and download the resulting APK from the workflow artifact.

## Publishing an in-app update

Only publish after the source has been promoted to the private repository's `main` branch and the version has been increased in `app/build.gradle.kts`.

1. Run the workflow with source ref `main`.
2. Enable **Publish this verified build**.
3. Enter release notes.
4. The workflow runs unit tests, builds the APK, verifies alignment, package/version metadata, SHA-256 and the established signing certificate.
5. It creates or updates the GitHub Release asset and updates `latest.json`.

The installed app can then use **You → Check for updates → Download & install**. The app verifies the download checksum, package identity, version and signing certificate before opening Android's installer.

## Safety gates

- No keystore or token is stored in Git.
- The source checkout token is read-only and limited to the private ApexPulse repository.
- The release workflow is manual; it does not publish every push.
- Publishing is refused unless the source ref is `main` and the version code is higher than the currently published version.
- The signing certificate must match SHA-1 `A70ADF61BB307148D1E064DEE91B7723D0654951`.
- The app remains Wi-Fi-only for telemetry and update downloads.
