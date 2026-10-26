# Local browser connection skill

This Codex skill discovers the currently exposed local Chrome or Edge connection, selects its actual returned ID, and verifies the page URL and document title. It requires a supported `cua_repl` tool and an already connected browser. Installing this file does not install an extension, grant permissions, or establish a browser connection.

## Install on another computer

Download this repository using GitHub **Code → Download ZIP**, extract it, and copy the folder `skills/local-browser-connection` to your Codex user skills folder:

- Windows: `%USERPROFILE%\.codex\skills\local-browser-connection`
- macOS/Linux: `~/.codex/skills/local-browser-connection`
- If `CODEX_HOME` is set, use its `skills` subfolder instead.

The resulting file must be `local-browser-connection/SKILL.md`, without an extra nested folder. If that skill already exists, compare it and preserve any custom changes before replacing it.

Start a new Codex task so it can discover the skill. Request `$local-browser-connection` and ask it to open a URL in the intended local Chrome or Edge and report the actual URL and title. If the tool or connection is absent, the skill reports that limitation; it does not change connection or security settings.

Only the reusable skill and this installation guide are included. Connection IDs and profiles must be discovered on each computer.
