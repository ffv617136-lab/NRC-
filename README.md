# NRC Social — OpenRouter Nemotron Edition

This version uses **`nvidia/nemotron-3-ultra-550b-a55b:free`** through OpenRouter for NPC replies in the Feed, Group chats, and DMs. A private Vercel serverless function handles the API key; the browser never receives the key. If the AI fails or runs out of free requests, NPCs reply using built-in scripts.

## Set it up from your phone (iPhone)

1. Download and unzip `NRC-Social-OpenRouter.zip` in the iPhone **Files** app.
2. Open https://github.com/new and make a repository, e.g. `nrc-social`. Upload `index.html`, `package.json`, and `api/reply.js` **in the `api` folder**. You may need Safari's **Request Desktop Website** feature to upload folders. **Never upload any API key or .env file.**
3. Open https://openrouter.ai/settings/keys and generate an **OpenRouter API key**. It is a secret: don't paste it in your website or send it in chat.
4. Open https://vercel.com/new and import your GitHub repo. Set the framework preset to **Other** and leave the root directory at the repo root. Deploy.
5. Go to Vercel project **Settings → Environment Variables**. Add these two **server-only** secrets, applying to Production:
   - Name: `OPENROUTER_API_KEY` ; Value: the key you created on OpenRouter.
   - Name: `NRC_ACCESS_CODE` ; Value: a long secret phrase **different** from your OpenRouter API key. This is for your own use, not a public sign-up password.
   - Optional: `NRC_SITE_URL` ; Value: `https://your-project.vercel.app` (your real Vercel URL).
6. **Redeploy** after adding variables (Vercel project → Deployments → latest → Redeploy).
7. Open your Vercel website URL. Go to **Customize → OpenRouter · Nemotron 3 Ultra Replies**. Enter **your NRC access code**, then tap **Connect Nemotron**. Never enter the actual OpenRouter API key in the browser.
8. Open a character DM and send a message. NPC replies are now AI-generated. The feed and group reply systems also call AI while connected.

## Important limitations

- This specific Nemotron model handles **text only**. The app still supports photo posts/DMs/group messages visually, but Nemotron **cannot see images**. Add a text caption to help it react. Photos stay in the browser and are not sent to OpenRouter by this app.
- Proactive NPC DMs and unscheduled NPC posts remain **scripted**, so the website doesn't constantly consume your quota.
- The model is currently listed as free but free-tier **rate limits and availability** apply. OpenRouter advertises a limit of **50 free-model requests per day** for accounts without credits, which may change. Each NPC response consumes one request. Errors use scripted fallback.
- Text from your conversation (up to 12 recent messages plus the current prompt) is shared with OpenRouter and model providers. **Do not send personal, confidential, or sensitive information.** The free NVIDIA endpoint has specific logging/data-usage disclosures.
- This is a personal-use prototype, not full account authentication. A shared access code is unsuitable for a public multi-user service. Anyone with that code could spend your API quota. Browser data does not sync between devices.
- If you use GitHub Pages, AI replies will not function on that domain: the `/api/reply` server function only runs where the backend is deployed (e.g., Vercel).
- The `:free` model remains hard-coded; there is no automatic paid model fallback.
- Flirting and romantic themes are possible; explicit sexual descriptions involving school-age characters are not included.

## API endpoint

`POST /api/reply` with `X-NRC-Passcode: <your NRC access code>` and JSON fields `character`, `context` (dm, group, feed, comment, reply), `message`, `history` (array of `{speaker, text}`), `imageAttached` (boolean). The Vercel function uses OpenRouter's standard `POST https://openrouter.ai/api/v1/chat/completions` endpoint.

References:
- https://openrouter.ai/nvidia/nemotron-3-ultra-550b-a55b:free
- https://openrouter.ai/docs/api/api-reference/overview
- https://openrouter.ai/pricing
- https://vercel.com/docs/environment-variables
