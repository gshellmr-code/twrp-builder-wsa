<p align="center">
  <img src="https://raw.githubusercontent.com/gshellmr-code/twrp-builder-wsa/master/assets/twrp.png" alt="TWRP for WSA" width="200"/>
</p>

<h1 align="center">TWRP for WSA (x86_64)</h1>

<p align="center">
  Build TWRP recovery for Windows Subsystem for Android (WSA) using GitHub Actions.
</p>

## How it works

1. GitHub Actions syncs AOSP + TWRP source
2. Compiles TWRP for x86_64 architecture
3. Compiles custom dispatcher (`init.c`) — decides TWRP vs Android boot
4. Packages everything into `twrp.7z`
5. You download the artifact (~10 MB)

## Usage

1. Fork this repo
2. Go to **Actions** tab
3. Click **Build TWRP x86_64 for WSA**
4. Click **Run workflow**
5. Wait ~60 minutes
6. Download `twrp-x86_64-wsa` artifact

## Files

| File | Purpose |
|------|---------|
| `init.c` | Custom dispatcher — reads info.json, boots TWRP or Android |
| `info.json` | TWRP metadata with `recovery_flag` field |
| `patch.json` | Maps TWRP files into WSA initrd.img cpio |
| `.github/workflows/build-twrp.yml` | GitHub Actions build workflow |

## Boot Flow

```
WSA kernel loads initrd.img
  → /init (dispatcher) reads /info.json
    → recovery_flag: true  → exec /sbin/twrp (TWRP boots)
    → recovery_flag: false → exec /lspinit (Android boots)
```

## Injecting into WSA

```bash
# Using twrp.py from the main project:
twrp.py --inject twrp.7z
twrp.py --inject-file info.json into /
twrp.py --inject-folder twrp_files/ into /sbin/
```
