# PromptFrenzy Starters

Pinned starter scaffolds used as **locked repo inputs** for PromptFrenzy code-render prompt templates (`text-to-html` and friends). A starter gives every model the same working boilerplate so generation effort goes into the creative part of the prompt, not into reinventing setup — and so outputs are reproducible and compatible with our offline rendering/recording tooling.

## How templates reference a starter

A template stores a locked input variable:

```
(repo, commit SHA, path)
```

e.g. `(Prompt-Frenzy/starters, <sha>, threejs)`. Always pin a commit SHA, never a branch — a starter folder at a SHA is immutable, so a template's behaviour can't change underneath it.

## Layout

```
registry.json          machine-readable index of starters
<starter>/
  starter.json         manifest: entry file, engine versions, prompt-insertion instructions
  index.html           the scaffold, with a marked "YOUR SCENE HERE" region
  vendor/              pinned dependencies, vendored — no CDN at runtime
```

Adding a new project type = a new folder + a `registry.json` entry. No tooling changes: consumers read `starter.json` to find the entry file and the instructions to inline into the prompt.

## Rules for scaffolds

- **Vendor everything.** No CDN imports, no runtime network access — outputs must render in a sandboxed iframe and under the offline deterministic recorder.
- **Drive animation from `requestAnimationFrame` timestamps** (not `setInterval`, not wall-clock reads inside the loop) so the recorder's virtual clock captures smooth video.
- **One marked region** for model output; everything outside it is contract. Consumers validate the import map and scaffold structure are untouched before rendering.
- Bumping a vendored dependency is a normal commit; existing templates keep their pinned SHA and migrate deliberately. Record the starter repo+SHA in every generation/benchmark run's metadata — runs against different scaffold versions are different protocols.

## Licenses

This repo is MIT licensed (see `LICENSE`). Vendored dependencies keep their own licenses (e.g. `threejs/vendor/` is [three.js](https://github.com/mrdoob/three.js), MIT — see `threejs/vendor/LICENSE`).

## Community starters (future)

External submissions stay in their authors' repos. They enter the same registry with an explicit `repo` field and an approved, pinned SHA (so a starter can't change after review). This monorepo is just the first entry in that registry.
