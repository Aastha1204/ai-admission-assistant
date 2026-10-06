# AI Admission Assistant (sales demo)

Three pages: `/demo` (send this to prospects), `/student` (WhatsApp-style chat), `/owner` (institute dashboard).

## Run locally

```bash
npm install
cp .env.example .env.local   # then fill LLM_API_KEY (optional, see below)
npm run dev                  # http://localhost:3100
```

## Rebrand for a new institute

Edit **only** `config/client.json` (name, logo text, primary color, area, owner, counsellor, WhatsApp number, `demoVideoUrl`).
Courses, FAQs, slots and the scripted demo live in `data/*.json`. FAQ text can use `{{area}}`, `{{businessName}}`, `{{counsellorName}}`.

`demoVideoUrl`: a YouTube link or `/demo.mp4` (put the file in `public/`). Leave empty to hide the player.

## Environment variables

| Variable | Required | Purpose |
|---|---|---|
| `LLM_PROVIDER` | no | `openai` (default in `.env.example`) or `anthropic` |
| `LLM_API_KEY` | no | Provider key. Server-side only. Without it the assistant runs on built-in rules, so the demo never breaks. |
| `LLM_MODEL` | no | Default `gpt-4o-mini` (OpenAI) / `claude-haiku-4-5-20251001` (Anthropic) |
| `NEXT_PUBLIC_SUPABASE_URL` | no | Enables cross-device sync |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | no | Enables cross-device sync |

Without the Supabase variables (or if the connection fails) the app falls back to localStorage + BroadcastChannel, which syncs tabs of the **same browser** only. For "owner on laptop, student on phone" you need Supabase.

## Supabase setup (5 minutes)

1. Create a project at supabase.com.
2. SQL editor: paste and run `supabase/schema.sql` (creates the `leads` table, demo policies and Realtime).
3. Project settings > API: copy the Project URL and the `anon` key into the two variables above.

The policies are open for demo use. Do not store real student data in this project.

## Deploy on Vercel

1. Push this folder to a GitHub repo (`.env.local` is git-ignored).
2. Vercel > Add New > Project > import the repo. Framework is auto-detected as Next.js.
3. Add the environment variables from the table above (Settings > Environment Variables), for Production and Preview.
4. Deploy. Open `https://<project>.vercel.app/demo`.

The QR code on `/demo` uses the current page origin, so on the deployed URL it points to the live `/student`.

## Demo-day checklist

- Open `/owner` on the laptop and `/student` on your phone (scan the QR on `/demo`).
- Press **Reset demo** on `/owner` before each prospect.
- `/demo` Play works with no internet and no API key.
- If anything is slow, the student chat falls back to rule-based replies after 12 seconds.
