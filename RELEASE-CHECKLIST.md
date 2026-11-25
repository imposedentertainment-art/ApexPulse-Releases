# Maintainer Release Checklist

This repository is the public APK distribution channel. Do not publish a release or update manifest using placeholder values.

## Before publishing

- [ ] Review the intended source commit in the private ApexPulse repository.
- [ ] Confirm the Android package ID is `com.apexpulse.app`.
- [ ] Confirm the version code is greater than the previously published version code.
- [ ] Build the release APK using the maintained Termux build process.
- [ ] Confirm the APK is signed with the established ApexPulse signing identity.
- [ ] Verify APK alignment and signature.
- [ ] Verify package ID, version code, and version name from the APK itself.
- [ ] Install the APK over the existing installation (do not uninstall or clear app data).
- [ ] Smoke-test launch, core simulator/session flow, and update screen on a real device.
- [ ] Calculate SHA-256 from the exact final APK.

## Publish

- [ ] Create a GitHub Release with a matching version tag (for example, `vX.Y.Z`).
- [ ] Attach the exact verified APK as `ApexPulse-X.Y.Z.apk`.
- [ ] Add concise user-facing release notes and identify any known limitations.
- [ ] Download the uploaded asset again.
- [ ] Confirm its size and SHA-256 match the locally verified APK.

## Publish the update manifest last

Only after the release asset has been independently checked, create or update root-level `latest.json` on `main`.

Required fields:

- `version`: semantic version string, without a leading `v`.
- `versionCode`: Android integer version code.
- `downloadUrl`: HTTPS URL of the exact APK attached to the GitHub Release.
- `sha256`: lowercase 64-character SHA-256 checksum of that exact APK.
- `releaseNotes`: concise user-facing release notes.

Optional field:

- `minimumVersion`: minimum supported app version, only when a forced compatibility floor is intentionally required.

Example shape only — **do not publish this example as a real manifest**:

```json
{
  "version": "X.Y.Z",
  "versionCode": 0,
  "downloadUrl": "https://github.com/OWNER/REPO/releases/download/TAG/APK.apk",
  "sha256": "REPLACE_WITH_REAL_64_CHARACTER_SHA256",
  "releaseNotes": "Summarise the actual changes in this release."
}
```

Never publish a guessed URL, placeholder checksum, unverified APK, unsigned APK, or APK signed with a different key. The manifest is the final publication step because the app uses it to discover updates.
