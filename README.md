# Darkroom — AI Image Studio

A ready-to-deploy Next.js site that generates and edits images with AI. It uses
OpenAI's `gpt-image-1` model — the same image model family behind ChatGPT's
image generation and editing tools — so the results are the real thing, not a
clone or simulation of it.

## What it does

- **Generate** — type a prompt, get an image.
- **Edit photo** — upload an image and describe a change; the model edits it.
- **Regenerate** — re-run the same prompt for a new take.
- **Save** — downloads the current image as a PNG.
- **Share** — on phones, opens the native share sheet (WhatsApp, Telegram,
  Instagram, Messages, etc.) with the actual image attached. On desktop, opens
  a menu with WhatsApp/Telegram web links, "copy image," and download.
- **This session filmstrip** — every image you make in the current tab is kept
  as a thumbnail so you can flip back to earlier results. (It's in-memory only
  — closing the tab clears it, so save anything you want to keep.)

## 1. Get an OpenAI API key

You need your own key — this is what actually powers image generation, and it
is billed by OpenAI directly (image generation is pay-per-image, not free).

1. Go to https://platform.openai.com/api-keys and create a key.
2. Make sure your OpenAI account has billing set up — image generation
   requires a funded account, even a few dollars is enough to start.

Keep this key secret. It's only ever used on the server side in this project
(inside `app/api/*/route.js`) and is never sent to the browser.

## 2. Run it locally (optional, to test first)

```bash
npm install
cp .env.example .env.local
# open .env.local and paste your key after OPENAI_API_KEY=
npm run dev
```

Visit http://localhost:3000.

## 3. Deploy to Vercel

1. Push this folder to a GitHub repo (or use `vercel` CLI directly on the folder).
2. Go to https://vercel.com/new and import the repo.
3. Vercel auto-detects Next.js — no config needed.
4. Before the first deploy (or right after), go to **Project → Settings →
   Environment Variables** and add:
   - Key: `OPENAI_API_KEY`
   - Value: your key from step 1
   - Environments: Production, Preview, Development (select all)
5. Deploy. Done — the live URL works immediately.

If you deploy from the CLI instead:

```bash
npm i -g vercel
vercel
vercel env add OPENAI_API_KEY
vercel --prod
```

## Notes on other hosts (e.g. zone.id)

This is a standard Next.js 14 app (App Router), so any host that supports
Node.js Next.js apps works the same way: run `npm install && npm run build`,
then `npm start`, and set the `OPENAI_API_KEY` environment variable on that
host before starting it. If a host only supports static sites, it won't work
as-is, because the API routes need a Node server to keep your key private.

## Project structure

```
app/
  api/generate/route.js   → calls OpenAI images.generate
  api/edit/route.js       → calls OpenAI images.edit
  page.js                 → the UI
  layout.js               → fonts + page shell
  globals.css             → design system
```

## Costs & limits to know

- Every generate/edit/regenerate click is a real paid API call to OpenAI.
- There's no built-in rate limiting or auth in this starter — if you publish
  the URL publicly, anyone with the link can spend your API credits. Add a
  password gate or per-user limits before sharing it widely.
