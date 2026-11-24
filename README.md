# ApexPulse Releases

Official Android APK distribution for ApexPulse.

- APK binaries are attached to GitHub Releases.
- `latest.json` is the public update manifest read by the Android app.
- Application source code is kept in the separate [private ApexPulse source repository](https://github.com/imposedentertainment-art/ApexPulse).

## Manual release process

This project does **not** use GitHub Actions. Releases are built and verified locally, then uploaded manually.

1. Build the APK from a reviewed commit in the private source repository.
2. Sign it with the established ApexPulse signing identity.
3. Verify the APK signature and alignment.
4. Install it over the existing app to confirm update compatibility.
5. Calculate SHA-256 from the exact final APK.
6. Create a GitHub Release here and attach the APK.
7. Verify the uploaded asset URL and checksum.
8. Update `latest.json` only after all checks pass.

Do not publish placeholder manifests, fake checksums, unsigned APKs, or APKs signed with a different key.

## Update manifest

The root-level `latest.json` is intentionally absent until a verified release exists. When published, it uses:

- `version`: Semantic Versioning version string.
- `versionCode`: monotonically increasing Android version code.
- `minimumVersion`: optional minimum supported version.
- `downloadUrl`: HTTPS GitHub Release asset URL for the APK.
- `sha256`: 64-character SHA-256 checksum of that APK.
- `releaseNotes`: user-facing release notes.

Android requires user confirmation to install an update. The APK must retain the installed application's package ID and signing certificate.
