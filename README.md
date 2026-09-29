# java-openjdk

OpenJDK 21 runtime + JDK tools layer for OpenCharly images.

The `java-openjdk` candy installs OpenJDK 21 (headless runtime + JDK tools)
cross-distro — Fedora ships `java-21-openjdk-headless`/`-devel`, Arch ships
`jdk21-openjdk`, Debian/Ubuntu ship `openjdk-21-jdk-headless`. A build step
symlinks a **distro-agnostic** `JAVA_HOME` (`/usr/lib/jvm/charly-jdk21`) onto
whichever JDK 21 root the package installed, so consumers (Android SDK, Appium,
Gradle, sbt) discover `java` and `javac` without hardcoding a distro path.

On Arch/CachyOS the step also points `archlinux-java` at the installed JDK 21 so
`/usr/bin/java` and `/usr/bin/javac` resolve (their `default-runtime` ships
unset until `archlinux-java set` runs).

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `java-openjdk` |
| Packages | `java-21-openjdk-headless` + `-devel` (fedora), `jdk21-openjdk` (arch), `openjdk-21-jdk-headless` (debian/ubuntu) |
| Environment | `JAVA_HOME=/usr/lib/jvm/charly-jdk21`, `PATH` += `$JAVA_HOME/bin` |
| Binaries | `/usr/bin/java`, `/usr/bin/javac` |
| Install files | `charly.yml` (`run:` step) |
| Service / port | none |

## How to use it

Compose the layer by pinning this repo in a box's nested `candy:` list. In the
real box schema an image is a single `candy:` node whose body carries `base:`
**and** a nested `candy:` list (there is no box-level `base:` sibling — see
`/charly-image:image` and a live example such as
`distro-cachyos/box/comfyui/charly.yml`):

```yaml
my-box:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-java-openjdk:v2026.239.1627'
```

After the image is built:

```bash
java -version      # openjdk version "21..."
javac -version
echo "$JAVA_HOME"  # /usr/lib/jvm/charly-jdk21
```

## Layout

- `charly.yml` — the `java-openjdk:` candy entity: the `env:`/`path_append:`
  wiring, the per-distro `distro:` package arms, the `run:` symlink step, and
  the `check:` assertions. No `skill:` entity yet.
- `CHANGELOG/` — per-CalVer release notes.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: none yet — this candy carries no `skill:` entity. Closest:
  `/charly-check:android` (the `kind: android` substrate this JDK backs) and
  `/charly-image:layer` (candy authoring). The gap is tracked in
  [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).
- Consumer: `/charly-check:android` — Android SDK / Appium / emulator
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
