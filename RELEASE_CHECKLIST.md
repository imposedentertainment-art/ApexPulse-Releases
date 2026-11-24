# Manual release checklist

- [ ] Source changes reviewed and merged to private `main`.
- [ ] Android `versionCode` incremented; `versionName` updated.
- [ ] Unit tests pass locally.
- [ ] APK builds successfully.
- [ ] APK passes `zipalign` and `apksigner verify`.
- [ ] Signing certificate matches the previously installed ApexPulse build.
- [ ] APK installs over the existing app without uninstalling it.
- [ ] Real simulator/network test completed, or explicitly marked unverified.
- [ ] SHA-256 calculated from the exact signed APK.
- [ ] GitHub Release created and APK asset uploaded.
- [ ] Uploaded asset URL and checksum independently verified.
- [ ] Only then publish/update root `latest.json`.
- [ ] Check update from an installed older build and verify Android's installer prompt.
