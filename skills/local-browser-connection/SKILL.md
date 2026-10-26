---
name: local-browser-connection
description: Select and verify the user's connected local Chrome or Edge through cua_repl for browser connection tests and page reads, especially when a fixed browser alias reports unavailable. Complements the bsk browser skill; does not configure connections or permissions.
---

# Local browser connection

Use this workflow when the task targets a user's local browser and `mcp__cua_repl.js` is available. Read the current tool instructions and returned browser documentation before acting. The separate `browser-skill` uses bsk; its CLI availability is independent of cua_repl browser availability.

## Discover before selecting

1. Call `await cua.listBrowsers()` to inspect the current connections. On a fresh cua_repl runtime, obey its first-invocation rule: execute only the single supported entry-point call.
2. Match the requested browser using the returned `id`, `name`, `family`, `type`, and `profileName` when present. Record the selected identity. If several relevant profiles remain ambiguous, resolve the intended profile before opening or claiming a tab.
3. Select with `let browser = await cua.getBrowser({ id: selectedId })`, using an ID returned by the current inventory, and read its returned documentation. Do not assume the fixed alias `chrome` always resolves.
4. Distinguish a local Chrome/Edge extension connection from ChatGPT in-app browser (IAB), MCP Apps, and a cloud browser. A connected PC does not imply that its Chrome is exposed to this task. Do not substitute a different browser or infer that the user's PC or all of Chrome is broken because this task lacks a connection.

## Open and verify

Follow the selected browser's current documentation. The following API order was successfully used with a discovered Chrome extension connection:

```javascript
// selectedId is the actual Chrome ID from this run's inventory.
let browser = await cua.getBrowser({ id: selectedId });
// Read the documentation returned by getBrowser before continuing.
await browser.nameSession("🔎 Page title check");
let tab = await cua.createBrowserTab(selectedId, requestedUrl, {
  sessionName: "🔎 Page title check"
});
// createBrowserTab returns page metadata and accessibility state.
let domTab = await browser.tabs.get(tab.id);
nodeRepl.write(await domTab.playwright.evaluate(() => ({
  title: document.title,
  url: location.href
})));
```

Use these methods only when the current returned documentation supports them. Preserve the selected browser binding across later turns; a stale tab does not by itself justify changing browser selection. Use a new tab when necessary to preserve existing edits. Read the returned accessibility state for requested visible text, or use a supported screenshot when visual verification is needed. Stop once the requested evidence is present. Follow the current tab cleanup/marking instructions.

Report the actual browser identity, current URL, and title or observed screen evidence. Never replace an unread title with memory or search results. Navigation is not proof of reading: obtain fresh returned page state and actual URL/title after opening.

## Failures and limits

- Do not repeat the same unavailable operation without materially relevant new evidence. A later user-requested check or changed connection inventory permits a fresh discovery and, if the required connection is present, one supported attempt.
- Distinguish unavailable, denied, pending, occupied by another extension/task, and uncertain outcomes. For an uncertain result, inspect supported read-only state before retrying a mutation. Stop the affected operation on denial or occupancy; do not switch backends, install software, change security/connection/permission settings, restart daemons, or bypass warnings to recover.
- Report the failed action, target, observed reason, completed work, and outcome status. Do not infer why another Codex task succeeded without evidence from that task.
- A successful browser read proves only that operation. It grants no authorization for and proves no success of file transfers, submissions, reports, or publication.
