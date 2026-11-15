# F1 Mobile Racing 5.4.11 - Offline Patcher

Turns **your own original 5.4.11 APK** (`com.codemasters.F1Mobile`, `versionCode 24001341`) into an offline-patched build, either on a PC via Python or directly on an Android phone via the Chaquopy Android app.

## Improvements over stock `v0.4`

* **Unified streaming engine**: `f1mobile_patcher.py` and `runner.py` share one streaming ZIP rebuild + in-place APK Signature Scheme v2 signer (constant memory, atomic `.tmp` writes, guaranteed cleanup on error/cancel).
* **Correct `.so` & `resources.arsc` alignment**: Uncompressed `.so` libraries are aligned to 4096 bytes and other stored entries (including `resources.arsc`) to 4 bytes on both PC and Android.
* **Complete v1 signature stripping & RSA self-verification**: Strips all JAR signature files (`MANIFEST.MF`, `*.SF`, `*.RSA`, `*.DSA`, `*.EC`), clamps pre-1980 DOS timestamps, detects already-patched APKs, and verifies the v2 RSA signature & entry alignment before committing output.
* **Standalone `.xapk` bundle support & diagnostics**: Directly extracts and patches `base.apk` / `com.codemasters.F1Mobile.apk` inside a standalone `.xapk` (or guides clearly on split APKS/APKM bundles), plus `--inspect`, `--verify`, and `--extract-obb`.
* **Improved Android app UX**: Retains state across rotation/dark-mode changes, checks installed game status & signing key (`Use Installed`, `Uninstall Store`, `Open Game`), validates ZIP magic bytes & free space (`StatFs`), supports non-destructive `Inspect APK` and `Cancel`, recovers unsaved patched APKs, and offers `4  Install APK`.

## How to Install F1 Mobile Racing 5.4.11 (Android)

1. **Get the files**: Install **ZArchiver**, download F1 Mobile Racing `5.4.11` (`.xapk`), and install the patcher APK (`f1-mobile-patcher-v0.4.apk`).
2. **Extract `.xapk` (recommended for low RAM/storage)**: In ZArchiver, extract the `.xapk` to get **`base.apk`** (~67 MB) and the **`.obb`** file. *(Do not install `base.apk` yet; if the Play Store version is installed, tap **Uninstall Store** in the patcher first.)*
3. **Patch & save**: Open **F1 APK Patcher**, tap **1  Pick APK** and select `base.apk` (or the `.xapk` directly), tap **2  Patch APK**, then **3  Save APK**.
4. **Install & initialize folders**: Tap **4  Install APK**, then **open the game once** until the loading logo appears and close it completely (this makes Android create `Android/obb/com.codemasters.F1Mobile/`).
5. **Copy the `.obb` file**: In ZArchiver, copy/move the `.obb` file into `Internal Storage/Android/obb/com.codemasters.F1Mobile/`. *(On Android 13/14+, grant access in the Files prompt or pair ZArchiver with Shizuku if `Android/obb` is restricted.)*
6. **Launch and play**: Open F1 Mobile Racing from your home screen or tap **Open Game** in the patcher.

## PC Usage

```bash
python3 f1mobile_patcher.py base.apk
# -> base-offline-patched.apk

# Patch a standalone .xapk directly and also extract its .obb file
python3 f1mobile_patcher.py F1_Mobile_5.4.11.xapk --extract-obb

# Inspect or verify an APK / .xapk without modifying anything
python3 f1mobile_patcher.py --inspect base.apk
python3 f1mobile_patcher.py --verify base-offline-patched.apk
```

## Running Automated Tests

```bash
python3 tests/test_patcher.py -v
```
