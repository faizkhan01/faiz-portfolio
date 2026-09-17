# Md. Faizur Rahman Khan Portfolio

Professional AI-integrated portfolio website for Md. Faizur Rahman Khan, a senior software engineer with full-stack, backend, cloud, and AI-powered product experience.

## Features

- Responsive single-page portfolio with animated hero, experience, skills, AI chat, contact, and resume download.
- Recruiter-focused AI chat powered by Groq.
- Streaming chat responses with automatic retry for temporary provider errors.
- CV-aware answers from verified profile context embedded in `server.mjs`.
- Web-search mode for current-world questions using Tavily (real search results, real cited sources) when configured, with Groq Compound as fallback.
- FIFA World Cup 2026 match-update fallback using ESPN's public scoreboard endpoint to avoid invented sports results.
- Downloadable resume from `public/assets/Md. Faizur Rahman Khan Resume.pdf`.
- Static assets served from `public/`, with the Node server handling `/api/chat`.

## Tech Stack

- Node.js native HTTP server
- Vanilla HTML, CSS, and JavaScript
- Groq chat completions API
- Tavily search API for grounded web-search answers
- Groq Compound web-search model (fallback if Tavily is unconfigured)
- ESPN public scoreboard API for FIFA World Cup updates

## Project Structure

```txt
portfolio-website/
  public/
    assets/
    app.js
    index.html
    styles.css
  .env.example
  package.json
  server.mjs
```

## Environment Variables

Create a local `.env` file:

```bash
cp .env.example .env
```

Required:

```env
GROK_API_KEY=replace-with-your-groq-api-key
```

Optional (real web search for current-world questions):

```env
TAVILY_API_KEY=replace-with-your-tavily-api-key
```

Optional:

```env
GROK_CHAT_MODEL=openai/gpt-oss-20b
GROK_WEB_SEARCH_MODEL=groq/compound-mini
GROK_CHAT_COMPLETIONS_URL=https://api.groq.com/openai/v1/chat/completions
GROK_MAX_TOKENS=550
TAVILY_MAX_RESULTS=5
PORT=4173
```

`GROQ_API_KEY` and `GROQ_*` variable names are also supported by the server.

Do not commit `.env` or expose API keys in browser-side code.

## Run Locally

```bash
npm start
```

By default, the site runs at:

```txt
http://localhost:4173
```

To use a different port:

```bash
PORT=4174 npm start
```

## Chat Behavior

The chat endpoint is:

```txt
POST /api/chat
```

For Faizur/profile questions, the assistant answers from verified portfolio and CV context.

For current-world questions, the assistant searches with Tavily (when `TAVILY_API_KEY` is set) and includes verified source links when available; if Tavily is unconfigured or fails, it falls back to Groq's compound web-search model.

For FIFA World Cup 2026 match-update questions, the server uses ESPN scoreboard data directly instead of relying on LLM-generated sports summaries.

## Deployment

Recommended free deployment: Render Web Service.

Render settings:

```txt
Build Command: npm install
Start Command: npm start
Instance Type: Free
```

If this project is inside a larger repository, set:

```txt
Root Directory: portfolio-website
```

Add the same environment variables in the Render dashboard, especially:

```env
GROK_API_KEY=your-production-groq-key
TAVILY_API_KEY=your-production-tavily-key
```

Render provides `PORT` automatically, and `server.mjs` already reads it.

## Notes

- `.env`, `node_modules`, and `.DS_Store` are ignored by git.
- The resume button downloads `public/assets/Md. Faizur Rahman Khan Resume.pdf`.
- The FK browser tab icon is served from `public/assets/fk-favicon.svg`.
- Legacy OpenAI, DeepSeek, and OpenCode implementations are intentionally disabled in `server.mjs`.
