# Mind Reader — website & release feed

Public home for **Mind Reader**, the local autocomplete app for macOS.

- **Landing page** → https://leonardoeloi.github.io/mindreader-releases/ (served by GitHub Pages from this repo's root `index.html`)
- **Download** → the `MindReader-*.zip` assets on the [`appcast` release](https://github.com/leonardoeloi/mindreader-releases/releases/tag/appcast)
- **Auto-update feed** → `appcast.xml` (Sparkle reads this; published by `scripts/release.sh` in the app repo)
- **Adoption dashboard** → `/stats.html` (download counts; `stats/history.json` snapshotted daily by `.github/workflows/stats-snapshot.yml`)

> The app's source code lives in a separate private repo. This repo only hosts the public site,
> the release binaries, and the update feed.
