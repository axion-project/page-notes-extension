# Page Notes — OpenRouter AI

A Manifest V3 browser extension for Vivaldi (and any Chromium browser) that turns the page you're looking at into an editable Markdown note, using an OpenRouter model of your choice, then hands it straight to Obsidian.

Built for a mobile-first workflow: extract → review → send → save, with an explicit checkpoint before anything leaves your device.

## Features

- **Extract → preview → send.** Pulls the page's `article`/`main`/body text into an editable box first. Nothing is sent to any API until you confirm.
- **Blocked-sites list.** A configurable denylist (defaults include major sign-in and banking domains) plus a hard block on internal browser pages (`chrome:`, `file:`, etc.) that can't be overridden.
- **Any OpenRouter model.** Defaults to `nvidia/nemotron-3-super-120b-a12b:free`, but the model field is free text, use whatever you have access to.
- **Three note styles.** Quick summary, detailed structured notes, or a tight bullet list.
- **Obsidian handoff.** "Save to Obsidian" fires an `obsidian://new` URI directly into the app, no file manager required, which is what makes this usable on a phone. Copy-to-clipboard and direct `.md` download are also available.
- **Local history.** Keeps your last 20 notes in extension storage so you can revisit or re-copy one without regenerating it.
- **Minimal permissions.** `activeTab`, `scripting`, `storage`, `downloads`, and host access to `openrouter.ai` only. No `<all_urls>`, no background page scraping.

## Installation

### Desktop (for testing)

1. Download or clone this repo.
2. Go to `vivaldi://extensions` (or `chrome://extensions` in any Chromium browser).
3. Enable **Developer mode** (top right).
4. Click **Load unpacked** and select this folder.

### Android (Vivaldi 8.2+)

Vivaldi for Android only installs extensions from the Chrome Web Store, sideloading an unpacked extension is not supported on that platform. To get this onto your phone:

1. Test it thoroughly on desktop first (see above).
2. Package it as a `.zip` and submit it to the [Chrome Web Store](https://chrome.google.com/webstore/devconsole) as an **unlisted** item (free, no public listing, just needs a quick automated review).
3. On Vivaldi Android, tap the puzzle-piece icon in the address bar to reach the Web Store and install it from your unlisted link.

## Setup

1. Click the extension icon, then the ⚙ settings gear.
2. Paste an [OpenRouter](https://openrouter.ai/) API key. Consider creating a key scoped to this extension with a spend limit, rather than reusing one from another project.
3. Set your Obsidian vault name exactly as it appears in Obsidian (needed for the `obsidian://` handoff to land in the right vault).
4. Adjust the model, note style, or blocked-sites list as you like.
5. Save settings.

## Usage

1. Navigate to a page.
2. Click the extension icon → **Extract this page**.
3. Review the extracted text (title, source, character count, and which part of the page was used are shown above it). Trim or edit it if you want to exclude anything before it's sent anywhere.
4. Click **Send to AI**.
5. Edit the generated Markdown freely.
6. Choose **Save to Obsidian**, **Copy**, or **Download**.

## Privacy notes

- The API key is stored in `chrome.storage.local`, which is local to your device and not synced, but it is not a hardware-backed secret store. Treat it like any locally-saved credential: don't reuse a key you use elsewhere, and set a spend cap on it.
- Extraction only ever runs on the tab you're currently viewing when you click the button, nothing runs automatically or in the background.
- Notes and extracted text pass through OpenRouter and whichever model/provider you've selected; review OpenRouter's and that provider's data-handling terms if you're working with sensitive pages.

## File structure

```
manifest.json     Extension manifest (MV3)
popup.html/.css   Popup UI
popup.js          UI logic: extraction, preview, export, history
background.js     Service worker: calls the OpenRouter API
icons/            Extension icons
```

## License

MIT, or whatever you'd prefer, this is your project to license as you see fit.
