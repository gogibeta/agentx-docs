# Multi-Tab & Takeover

## Inline browser card

While a browser session is active, a compact card appears **inside the chat message flow** (not as a top overlay). It scrolls with the conversation — the chat stays fully usable while the browser works.

- **Tap the card** → fullscreen browser dialog.
- **Stop** → kills the session.
- **Take over** → you drive: your taps and typing go straight to the page.
- **Hide (eye icon)** → collapses the card; a small floating button brings it back.

## Multiple tabs

One chat can drive several browser tabs:

- Ask the agent, or use the `browser_tab` tool: `list`, `new`, `switch`, `close`.
- Tabs are independent sessions within the chat — separate pages, separate state.
- Tools always act on the **active** tab.

## Takeover mode

Tap **Take over** and the page is yours:

- Taps are forwarded to the page (mapped through the viewport).
- Typing goes to the focused field.
- Tap **Stop** or close the dialog to hand control back to the agent.

## The visible cursor

While the agent works, a ring-and-dot cursor shows exactly where it's acting — so you can follow what it's doing without guessing.
