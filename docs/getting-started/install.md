# Install the App

AgentX is an Android AI-agent app. Install it from GitHub Releases — always install **over** your existing copy, never uninstall first.

## Download

1. Go to [AgentX Releases](https://github.com/gogibeta/AgentX/releases).
2. Download the latest `AgentX-vX.X.X-fdroid-release.apk`.
3. Open the APK on your phone and tap **Install** / **Update**.

::: warning Never uninstall
Always choose **Update** / **Install over** the existing app. Uninstalling wipes your API keys, chat history, memory, and settings. Every release is signed with the same key, so updates install cleanly over your data.
:::

## Verify the install

After installing:

1. Open AgentX.
2. Go to **Settings → About**.
3. Confirm the version matches the release you downloaded (e.g. `2.4.0-beta15`).

::: tip Version stuck on an old number?
If About shows an older version string than the release you installed (e.g. the APK says beta15 but About shows an older number), that's usually just stale version metadata baked into that build — the new code is still installed and running. If you're unsure, reinstall from the release APK and confirm you see the **Update** prompt (not a fresh Install prompt), which proves the signature matches and your data is preserved.
:::

## What's next

- [First Launch](/getting-started/first-launch) — grant permissions and do the initial setup.
- [Free API Keys](/providers/nararouter) — get free model keys before anything else.
