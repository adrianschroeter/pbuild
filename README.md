# Project or Package Build Action

This GitHub Action utilizes the `pbuild` tool from the openSUSE project to build either individual packages or entire projects.

### Supported Formats

* `spec` / `rpm`
* `dsc` / `deb`
* `PKGBUILD` / Arch Linux
* `KIWI` / Various appliance images
* `Dockerfile` / Containers
* Various other formats

This action can be used to build against any Linux distribution for which a configuration file exists.

---

## How to Use It

Add a workflow file to your Git repository, for example, at `.github/workflows/pbuild.yml`:

```yaml
name: Single package build

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
        with:
          lfs: true

      # Call your custom action template
      - name: Run Pbuild Template
        uses: adrianschroeter/pbuild@main
        with:
          dist: 'tumbleweed'

      # Export build artifacts
      - uses: actions/upload-artifact@v7
        with:
          name: tumbleweed-x86_64-logfile
          path: _build.tumbleweed.x86_64/_log
          archive: false
          
      - uses: actions/upload-artifact@v7
        with:
          name: tumbleweed-x86_64-rpm
          path: _build.tumbleweed.x86_64/bc-1*.x86_64.rpm
          archive: false
          
      - uses: actions/upload-artifact@v7
        with:
          name: tumbleweed-x86_64-everything.zip
          path: _build.tumbleweed.x86_64

```

> 💡 **Build Type Selection:** > For project builds, the working directory must include a
  `_pbuild` or `_manifest` file. If neither file is present, the action will default to a
  single package build.

---

## Upstream Links

Code and further information can be found at the following links:

* [openSUSE/obs-build GitHub Repository](https://github.com/openSUSE/obs-build)
* [pbuild Documentation](https://opensuse.github.io/obs-build/pbuild.html)

