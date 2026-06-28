<div align="center">

<img src="readme-hero.png" alt="Mind Reader — it writes the predictable part, so you don't have to." width="100%">

<br><br>

![macOS 14+](https://img.shields.io/badge/macOS-14%2B-0a0a0b?style=flat-square)
![Apple Silicon](https://img.shields.io/badge/Apple_Silicon-arm64-0a0a0b?style=flat-square)
![On-device](https://img.shields.io/badge/inference-100%25_on--device-0a0a0b?style=flat-square)
![Native Swift](https://img.shields.io/badge/native-Swift-0a0a0b?style=flat-square)
![Version](https://img.shields.io/badge/version-0.1.113-0a0a0b?style=flat-square)
![Price](https://img.shields.io/badge/price-free-0a0a0b?style=flat-square)

### [⬇&nbsp; Download for Mac](https://github.com/leonardoeloi/mind-reader/releases/download/appcast/MindReader-0.1.113.dmg) &nbsp;·&nbsp; [Website](https://leonardoeloi.github.io/mind-reader/) &nbsp;·&nbsp; [All releases](https://github.com/leonardoeloi/mind-reader/releases)

</div>

---

**Mind Reader** adds inline, ghost-text autocomplete to **every** text field on your Mac — Mail, Notes, Slack, your browser. The prediction runs **entirely on-device**: type, glance, press <kbd>Tab</kbd>. Nothing you write ever leaves the machine.

> **The fade *is* the confidence.** Confident predictions read sharp; uncertain ones fade toward grey — so you always know how much to trust before you accept.

It's a free, personal experiment in what a quiet, private autocomplete can feel like when it lives at the system layer instead of inside one app.

---

## Features

- **Inline ghost text, everywhere** — the same completion in Mail, Notes, Slack, a browser address bar or a search box. One behaviour, system-wide.
- **100% on-device** — runs on Apple's built-in Foundation Models (Apple Intelligence) **or** a bundled local Ollama model on `localhost`. No account, no server, no telemetry.
- **Word by word, or the whole line** — <kbd>Tab</kbd> takes the next word (chain it); a single key takes the entire suggestion; <kbd>Esc</kbd> waves it off.
- **It learns your voice** — few-shot learning from what you accept, an auto-built glossary of your own jargon/acronyms, and an optional style document. Review or forget individual examples, or switch learning off entirely, in **Privacy & Data**.
- **Per-app instructions** — different guidance for email, chat and notes, layered on a global style.
- **Context-aware, on your terms** — reads your clipboard, the text after the cursor, and (only if you allow it) what's on screen via on-device OCR. Each source is its own switch in **Privacy & Data** — turn any of them off whenever you like.
- **Never reads your secrets** — skips secure (password) fields entirely, and ignores clipboard items your password manager marks as concealed. What you type into a password box is never read, suggested, or stored.
- **VoiceOver-aware** — each suggestion is announced to VoiceOver, with the accept/dismiss keys, so the ghost text isn't invisible to screen-reader users.
- **You set how much it writes** — short (a nudge), medium, or most of a sentence.
- **A quiet, local record** — a dashboard tallies words completed, rough time saved and acceptance rate, with a per-day chart and a daily streak. Lives in a SQLite file you can open or export — nothing leaves the Mac.
- **Reads like your own text** — the ghost matches the field's real font **size and typeface** (in apps like Slack it even renders in the app's own font), lands exactly where your next character will, follows you across displays (multi-monitor honest), and never touches your field or undo history.
- **Streaming suggestions** *(local Ollama models)* — words appear as the model produces them, so it feels instant.
- **Inline autocorrect** *(experimental, off by default)* — quietly fixes a high-confidence typo in the word you just finished.

## Privacy

Everything is generated on your Mac. There is **no account, no sync, no server to opt out of — because there is no server.** What you type is never transmitted, logged, or used to train anything. The model and your text live in the same place: your machine.

And you stay in control of what feeds a suggestion. A **Privacy & Data** panel in Preferences lets you switch clipboard context and learning-from-what-you-accept on or off, review or forget individual learned examples, and clear the local history. Mind Reader also **never reads secure (password) fields**, or clipboard items your password manager marks as concealed — secrets are never read, suggested, or stored.

## The four keys — that's the entire interface

| Key | What it does |
|-----|--------------|
| <kbd>Tab</kbd> | Keep the next word (press again to keep going) |
| <kbd>&#96;</kbd> | Keep the whole suggestion at once *(configurable)* |
| <kbd>esc</kbd> | Dismiss, and stay quiet for a moment |
| <kbd>⌃⌥⌘L</kbd> | Toggle Mind Reader on/off, system-wide |

## Requirements

| | |
|---|---|
| **Mac** | Apple Silicon (M1 or newer) — ships as native `arm64`. Intel is not supported. |
| **macOS** | 14 (Sonoma) or later. On newer systems with Apple Intelligence it uses the built-in model with no download; on macOS 14–15 it uses a local Ollama model. |
| **Memory** | 8 GB RAM minimum, 16 GB comfortable (a larger model = sharper suggestions). |
| **Disk** | ~150 MB app + the model (first model download is ~1–5 GB, once, then cached). |
| **Permissions** | Accessibility & Input Monitoring (granted once). Screen Recording is **optional** (off by default). |
| **Price** | Free — no account, no plan, no limits. |

## Install

1. **[Download the DMG](https://github.com/leonardoeloi/mind-reader/releases/download/appcast/MindReader-0.1.113.dmg)** and drag **Mind Reader** into your Applications folder.
2. **First launch:** Mind Reader is signed by its author, not the App Store (the Accessibility & key-tap APIs it needs can't run in the sandbox). On the *"could not verify"* notice, click **Done**, then **System Settings → Privacy & Security → Open Anyway** — once per Mac. Comfortable in Terminal? `xattr -dr com.apple.quarantine "/Applications/Mind Reader.app"`
3. **Grant the two permissions** in Privacy & Security (Accessibility + Input Monitoring), pick a model in Preferences, and start typing.

It keeps itself updated: a gentle "update available" note appears in the menu bar; one click downloads and installs the EdDSA-signed update, keeping your permissions and settings intact.

## FAQ

<details>
<summary><strong>Is it really private?</strong></summary>

Yes. Suggestions are generated on your Mac — by Apple's built-in Foundation Models or a local Ollama model on `localhost`. Nothing you type is sent to any server. A small local history (SQLite) is kept on your Mac to learn your style and is never uploaded.

You're also in control of what informs a suggestion. In **Preferences → Privacy & Data** you can switch clipboard context and learning on or off, review or forget individual learned examples, and clear the history. And Mind Reader **never reads secure (password) fields** or clipboard items your password manager flags as concealed — what you type into a password box is never read, suggested, or stored.
</details>

<details>
<summary><strong>Which apps does it work in?</strong></summary>

Any standard text field — native apps (Notes, Mail, TextEdit) via macOS Accessibility, plus Electron and web apps (Slack, Chrome) via a keystroke fallback. You can mute it in any single app from the menu bar ("Pause in …") or the per-app blocklist. Code editors are best left disabled — they have their own completion.
</details>

<details>
<summary><strong>Does it work in Google Docs?</strong></summary>

No — and it's worth being honest about why. Google Docs draws your text onto a **canvas** and never exposes it to macOS as real text, so assistive apps (Mind Reader included) can't read what you're typing — even screen-reader mode only emits spoken announcements, not the editable text. It works everywhere that uses normal text fields: Mail, Notes, Slack, Gmail, Notion, Outlook on the web, and most other apps.
</details>

<details>
<summary><strong>Why does macOS warn me on first open?</strong></summary>

It's self-signed rather than distributed through the App Store (the Accessibility and event-tap APIs it relies on can't run in the App Store sandbox). Click **Done**, then **Open Anyway** in Privacy & Security — once per Mac. Because every update uses the same signing certificate, your permissions survive upgrades.
</details>

<details>
<summary><strong>Will it mess with my text or undo history?</strong></summary>

No. The suggestion is drawn in a separate floating overlay and never touches your field until you accept it. Ignore it and it simply fades — your document is untouched.
</details>

## Under the hood

- **Native Swift**, a lightweight menu-bar app (no Dock icon) with a custom monochrome icon.
- **On-device inference** via Apple Foundation Models or a bundled Ollama server (incl. MLX runners); reuses a system Ollama if one is running.
- **Field reading** through the macOS **Accessibility** APIs, with a keystroke/event-tap engine for Electron & web apps.
- **A floating overlay** that mirrors the field's font and caret — it draws the ghost text without ever modifying your document; only an accepted word is typed.
- **Auto-update** via [Sparkle](https://sparkle-project.org) with EdDSA signature verification; a stable signing certificate keeps your TCC permissions across upgrades.

## Design

Pure monochrome, *"the fade is the confidence."* The whole identity is a white→grey ramp — the logo's caret + fading "ghost word" pills, the ghost suggestion text, the confidence bars — form mirroring function. Type: Schibsted Grotesk · Hanken Grotesk · IBM Plex Mono.

## About this repository

This is the **public home** for Mind Reader — the app's source lives in a separate repo. Here you'll find:

- **Landing page** → [`index.html`](index.html), served at [leonardoeloi.github.io/mind-reader](https://leonardoeloi.github.io/mind-reader/) by GitHub Pages.
- **Release binaries** → the `MindReader-*.dmg` (human install) and `MindReader-*.zip` (Sparkle auto-update) assets on the [`appcast` release](https://github.com/leonardoeloi/mind-reader/releases/tag/appcast).
- **Auto-update feed** → [`appcast.xml`](appcast.xml) (read by Sparkle).
- **Adoption dashboard** → [`stats.html`](https://leonardoeloi.github.io/mind-reader/stats.html) (download counts, snapshotted daily).

---

<div align="center">
<sub>A free, personal experiment · native Swift, 100% on-device · <a href="https://leonardoeloi.github.io/mind-reader/">leonardoeloi.github.io/mind-reader</a></sub>
</div>
