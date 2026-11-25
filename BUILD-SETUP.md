# ApexPulse hosted build setup

This repository now contains a hosted Android build and release workflow. After the one-time secrets are configured, it checks the private source repository's `main` branch every 15 minutes. When it detects a newer versionName and versionCode, it runs tests, builds and verifies the APK, publishes the release asset, and updates `latest.json`. No Termux or manual workflow run is needed for routine releases.

It runs on GitHub's standard hosted Linux runner from this **public release repository**, so it does not consume the exhausted private-source-repository Actions allowance. GitHub documents standard hosted runner use in public repositories as free and unlimited:
https://docs.github.com/en/actions/concepts/billing-and-usage

## One-time setup

The workflow uses two repository secrets for production builds and automatic releases. Feature-branch test builds need only the read-only source token and use a temporary signing identity; those test APKs cannot update the installed production app.

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

Because Android keeps Termux's private home separate from shared storage, exporting this key requires one final, one-time Termux operation. Run this in the ApexPulse Termux environment. It checks both known key locations and exports only the key whose SHA-1 matches the installed app:

```bash
EXPECTED="A70ADF61BB307148D1E064DEE91B7723D0654951"
mkdir -p "$HOME/storage/downloads"
for KEY in "$HOME/.android/debug.keystore" "$HOME/ApexPulse-signing-backup/apexpulse-release.keystore"; do
  [ -s "$KEY" ] || continue
  SHA1="$(keytool -list -v -keystore "$KEY" -storepass android -alias androiddebugkey 2>/dev/null | sed -n 's/.*SHA1: *//p' | head -n 1 | tr -d ':[:space:]' | tr '[:lower:]' '[:upper:]')"
  if [ "$SHA1" = "$EXPECTED" ]; then
    base64 "$KEY" | tr -d '\r\n' > "$HOME/storage/downloads/APEXPULSE_SIGNING_KEYSTORE_B64.txt"
    echo "Verified signing key exported to Downloads."
    exit 0
  fi
done
echo "ERROR: Neither known keystore matches the installed ApexPulse certificate. Nothing was exported."
exit 1
```

If `$HOME/storage/downloads` is not linked to Android's Downloads folder, run `termux-setup-storage` once and grant access, then rerun the command.

Open `APEXPULSE_SIGNING_KEYSTORE_B64.txt` from Downloads, copy its entire contents, and paste it into the GitHub secret value. Do not paste the value into ChatGPT, an issue, a commit, or a public file. Delete the temporary text file after the secret has been saved.

The expected keystore uses alias `androiddebugkey` and password `android`, matching the existing build configuration. After this one-time export and secret setup, routine builds, signing, publishing and in-app updates do not require Termux.

## Testing a feature branch

1. Configure `APEXPULSE_SOURCE_TOKEN` as described above.
2. Open **Actions** in this repository.
3. Select **Build and publish ApexPulse**.
4. Choose the feature branch as the source ref.
5. Leave **Publish this verified build** disabled.
6. Run the workflow and download the resulting APK from the workflow artifact. It is signed with a temporary key and is for build verification only; it cannot update the production installation.

## Publishing an in-app update

Routine releases are automatic after the source is reviewed and promoted to the private repository's `main` branch with a higher `versionName` and `versionCode` in `app/build.gradle.kts`. The scheduled workflow checks every 15 minutes, runs unit tests, builds the APK, verifies alignment, package/version metadata, SHA-256 and the established signing certificate, then creates the GitHub Release asset and updates `latest.json`.

For an immediate/manual release, run the workflow with source ref `main` and enable **Publish this verified build**.

The installed app can then use **You → Check for updates → Download & install**. The app verifies the download checksum, package identity, version and signing certificate before opening Android's installer.

## Safety gates

- No keystore or token is stored in Git.
- The source checkout token is read-only and limited to the private ApexPulse repository.
- Feature-branch builds never receive the production signing keystore; only the trusted `main` release path can access it.
- The scheduled workflow publishes only when the private source `main` branch has a higher versionName and versionCode than the currently published release; it does not publish ordinary commits.
- Publishing is refused unless the source ref is `main` and the version code is higher than the currently published version.
- The signing certificate must match SHA-1 `A70ADF61BB307148D1E064DEE91B7723D0654951`.
- The app remains Wi-Fi-only for telemetry and update downloads.
