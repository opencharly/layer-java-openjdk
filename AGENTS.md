# AGENTS.md — layer-java-openjdk

Standalone candy repo for the `java-openjdk` layer — OpenJDK 21 runtime + JDK
tools with a distro-agnostic `JAVA_HOME` symlink. The candy lives in `charly.yml`
at the repo root: the `env:`/`path_append:` wiring, the per-distro `distro:`
package arms, the `run:` symlink step, and the `check:` assertions. This candy
carries no `skill:` entity, so no owning `/charly-*` skill is projected for it.

Canonical files:

- `charly.yml` — the `java-openjdk:` candy entity.
- `CHANGELOG/` — per-CalVer release history; read it before changing baked
  checks.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-check:android` — the closest owning skill: the `kind: android` device
  substrate whose SDK/Appium/emulator consumers this JDK backs. Load before
  editing or troubleshooting the candy.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`/`run:`, per-distro `distro:` arms, package
  sections, `env:`/`path_append:`). Load before editing any entity field or plan
  step.

There is no dedicated `/charly-*:java-openjdk` owning skill yet — this repo's
candy carries no `skill:` entity. The gap is tracked in
[opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291);
when one is authored, add it here.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence; they must stay
  valid on every distro arm they run on. Scope a distro-specific check in the
  command itself — the check runner does not honour runner-level
  `exclude-distro` fields.

## Modify this repo

- Edit the `java-openjdk:` candy entity in `charly.yml`. When a `skill:` entity
  is added, mirror behaviour changes into it too — the skill is the projected
  usage source.
- The `run:` symlink step resolves the distro JDK root into the canonical
  `/usr/lib/jvm/charly-jdk21`; keep that path and the `check:` assertions in
  sync across every distro arm.
- New behaviour claims belong in the `plan:` as an observable `check:` step.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
