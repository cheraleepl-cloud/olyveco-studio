# Olyveco Product Studio — Standalone

Your brand-locked document builder, ready to run outside Claude.
Template, logo, document library, Word/PDF export, and the AI editor
(via your own Anthropic API key, kept private on the server).

## What's in here

- `src/App.jsx` — the entire studio (brand kit, logo, template, library, exports)
- `api/generate.js` — a small serverless function that forwards AI requests
  to Anthropic with your API key added server-side (never exposed in the browser)
- Standard Vite + React scaffolding

## Option A — Deploy to Vercel (recommended, ~10 minutes)

1. Create a free account at vercel.com (sign in with GitHub is easiest).
2. Put this folder in a GitHub repository:
   - Create a new repo at github.com, then upload these files
     (or use git: `git init`, `git add .`, `git commit -m "studio"`, push).
3. In Vercel: **Add New Project** → import the repo → Deploy.
   Vercel auto-detects Vite and the `api/` folder. No config needed.
4. Get an API key at **console.anthropic.com** (API Keys → Create Key).
5. In Vercel: your project → **Settings → Environment Variables** → add:
   - Name: `ANTHROPIC_API_KEY`
   - Value: your key
   Then **Redeploy** (Deployments → ⋯ → Redeploy).
6. Done. Your studio is live at `your-project.vercel.app` —
   library, exports, and AI generation all working.

Costs: Vercel free tier is plenty. AI generation bills your Anthropic
API account per use — roughly a cent or two per document at this size.

## Option B — Run locally

1. Install Node.js from nodejs.org (LTS version).
2. In this folder: `npm install`
3. `npm run dev` → opens at http://localhost:5173

Note: the AI generator needs the serverless function, which plain
`npm run dev` doesn't run. Two choices:
- Everything except generation works locally — fine for using the
  library and exports.
- Or install the Vercel CLI (`npm i -g vercel`), create a `.env` file
  containing `ANTHROPIC_API_KEY=your-key`, and run `vercel dev` —
  this runs the function locally too, so generation works.

## Keeping it in sync with Claude

The studio still lives as an artifact in Claude. When you improve it
there (new library documents, template tweaks), download the updated
.jsx file, replace `src/App.jsx` with it, change the fetch URL from
`https://api.anthropic.com/v1/messages` to `/api/generate`, and push.
Vercel redeploys automatically.

## Security notes

- Never put your API key in `src/App.jsx` or anywhere in the browser code.
- `.env` is git-ignored so a local key never lands in your repo.
- If you share the live URL publicly, anyone can generate documents on
  your API bill. For a public tool, consider adding rate limiting or a
  simple password — happy to help with that when you get there.
