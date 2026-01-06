# pybuild
Build your Python projects, and turn them into packaged executables for all platforms.

## Usage

Create a GitHub workflow at `.github/workflows/release.yml`:

```yaml
name: Release

on:
  release:
    types: [created]

jobs:
  build:
    uses: uukelele-scratch/pybuild/.github/workflows/build.yml@main
    with:
      app_name: "App"
      data_dir: "assets" 
      icon_ico: "assets/icon.ico"
      icon_png: "assets/icon.png"
      version: ${{ github.event.release.tag_name }}
      app_id: "AAAA-AAAA-AAAA-AAAA"

  publish:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/download-artifact@v4
      - uses: softprops/action-gh-release@v1
        with:
          files: |
            windows-installer/*.exe
            linux-appimage/*.AppImage
            macos-dmg/*.dmg
```

Then, publish a release.
