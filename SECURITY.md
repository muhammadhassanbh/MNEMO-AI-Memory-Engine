# Security & Privacy

MNEMO is designed as a **local-first developer tool**. Read this before deploying or storing sensitive data.

## What stays on your device

- **Memory rows** and **chat history** — stored in browser **IndexedDB** (`mnemo` database).
- **Feature toggles** and **settings** — same IndexedDB store.
- **LLM API keys** (default mode) — stored **encrypted on disk** by the local MNEMO API server (`MNEMO_DATA_DIR`, AES-256-GCM). Keys are **not** kept in the browser when `VITE_MNEMO_BACKEND=true`.
# Google Meet Background Design

Professional custom backgrounds for Google Meet, optimized for virtual meetings, remote work, and polished presentation.

---

## Quick Start

1. Open [Google Meet](https://meet.google.com).
2. Start a new meeting or join an existing one.
3. Click your camera preview or open the meeting controls and choose **Apply visual effects**.
4. Under **Backgrounds**, click **+ Add**.
5. Upload a PNG from the `images/` folder in this repository.
6. Select the new background thumbnail to apply it.

> Tip: Upload one background first, confirm it renders well, then add the rest.

---

## Why this repo exists

This repository is designed for users who want:

- High-quality, polished backgrounds that work with Google Meet.
- A simple, repeatable upload process for teams and individuals.
- Clean designs that keep the focus on you, not your room.

---

## Best Practices

- Use a high-resolution PNG (1920x1080 or larger) for crisp output.
- Choose an image with minimal text or busy patterns.
- Prefer subtle gradients, abstract shapes, or branded office scenes.
- Ensure your face is well-lit and in front of a plain background for better chroma estimation.
- Avoid highly reflective surfaces or extreme backlighting.

---

## Supported platforms

Google Meet custom backgrounds work in:

- Chrome (desktop)
- Microsoft Edge (desktop)
- Google Meet mobile app (Android / iOS)
- Google Meet desktop app where supported

If you do not see **Apply visual effects**, update your browser or switch to a supported device.

---

## Video walkthroughs

- Watch a quick setup tutorial: https://www.youtube.com/watch?v=dQw4w9WgXcQ
- Google Meet background guide: https://support.google.com/meet/answer/10062182

> Note: The above video link is a placeholder example. Replace it with your own team tutorial or training video if available.

---

## Recommended workflow

1. Open this repository and inspect the `images/` folder.
2. Choose the background file that fits your meeting style.
3. Upload the PNG via Meet’s **+ Add** button.
4. Preview the effect and adjust lighting if needed.
5. Save the background so it is available for future meetings.

---

## Frequently asked questions

### Can I use JPG instead of PNG?
Google Meet accepts JPG, PNG, and GIF, but PNG is preferred for transparency and better image fidelity.

### Do backgrounds stay saved forever?
Custom backgrounds are stored with your Google account. They remain available the next time you sign in.

### Why does my background look blurry?
Use a larger image file (1080p or higher) and avoid small, low-resolution assets.

---

## Share this with your team
Send colleagues a direct link to this repo so they can upload the same branded backgrounds. This helps keep remote meetings consistent and professional.

---

## License
See the `LICENSE` file for details.
Legacy browser-only mode (`npm run dev:browser`) stores keys in **localStorage** (`mnemo_api_keys`) — not recommended for production.

Nothing is sent to MNEMO servers (there are none). Export/copy actions use your clipboard locally.

## What leaves your device

When you send a chat message:

1. The React UI sends the request to the **local MNEMO API** (`127.0.0.1:47831` by default) with a session token.
2. The API loads the **provider API key from encrypted storage** and forwards the request to that provider (Anthropic, OpenAI, Google, xAI, Groq, Cerebras, or OpenRouter).
3. The assembled **system prompt includes your memory table** (filtered by pins, staleness, and smart retrieval settings).

The API server must bind to **localhost only** — do not expose port 47831 to the public internet without TLS and proper authentication.

## Local use only

The default setup is for **`npm run dev` or the Tauri desktop app on your machine**. Do **not** deploy this stack to a public URL without:

- Per-user authentication
- Server-side key vault (KMS)
- HTTPS and rate limiting

Hosting the stock build publicly exposes users to **API key theft** or **open proxy abuse**.

## API keys

- Use **restricted** or scoped keys where the provider allows it.
- Do not commit keys to git or share exported JSON that contains secrets.
- Back up `MNEMO_DATA_DIR` or set `MNEMO_ENCRYPTION_KEY` before reinstalling (see [DEPLOYMENT.md](DEPLOYMENT.md)).
- Clear app data when using a shared computer.

## Session token

The local API issues a Bearer token (localhost-only `/api/v1/session` endpoint). The desktop/web UI stores it in **sessionStorage** for API calls. This is acceptable for a single-user local app; it is **not** multi-tenant security.

## Opt-in environment context (Séance mode)

When **enabled in Settings**, MNEMO may append battery level, time of day, and network quality hints to the **system prompt** sent to your LLM provider. Default is **off**.

## Experimental tools

`experimental/mnemos-sidecar.js` reads Claude Desktop local storage. It is **not** part of the main app, is **unsupported**, and may violate third-party terms of use. Do not run on work machines without explicit approval.

## Reporting issues

Open a GitHub issue for security concerns. Do not paste API keys or private memory exports in public tickets.
