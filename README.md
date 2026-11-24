# ApexPulse Releases

This repository distributes official ApexPulse Android application updates.

- Android APK files are published as GitHub Release assets.
- `latest.json` is the update manifest consumed by the ApexPulse app.
- The application source code is not stored in this repository.

## Update manifest

The root-level `latest.json` will contain the current release metadata:

- `version`: Semantic Versioning string (`MAJOR.MINOR.PATCH`).
- `versionCode`: monotonically increasing Android version code.
- `minimumVersion`: oldest supported app version, when applicable.
- `downloadUrl`: direct URL to the APK attached to the matching GitHub Release.
- `sha256`: SHA-256 checksum of that exact APK.
- `releaseNotes`: concise user-facing release notes.

A manifest must only be published after the APK has been built, signed with the established ApexPulse signing key, uploaded, and verified. Never publish placeholder checksums or download URLs.

## Release compatibility

Updates must retain the Android application ID and signing certificate used by the installed app. Android will still ask the user to confirm installation.
