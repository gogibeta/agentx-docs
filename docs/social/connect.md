# Connect to AgentX

## Paste your worker URL

1. In AgentX, open the **social / web-search settings** — the page that asks for your own social worker URL. (The app never uses the developer's test setup.)
2. Paste your deployed worker base URL, e.g. `https://fixtweet.<your-account>.workers.dev`.
3. **No trailing path needed** — the app appends `/llms.txt` itself to learn the API.
4. **No API key** — the worker needs none.

## Behaviors to know

- API calls require a `User-Agent` header — the app already sends one. Missing UA returns HTTP 401.
- A small set of routes (X followers/following/media tabs, some IG/Threads proxy routes) need an optional credential pool and answer 404/500/501 without one. Normal search, posts, and profiles work without any credentials.

## Test it

Ask the agent something like *"search X for posts about …"* — results should come back as clean text, not raw HTML.
