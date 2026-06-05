# 1Password Kiro Plugin

A Kiro plugin that connects Kiro to the 1Password desktop app's local MCP
server for [1Password Developer Environments](https://www.1password.dev/).

The plugin lets Kiro help with secure project environment setup — listing
Developer Environments, inspecting variable names, adding variables, and
creating local `.env` mounts. Secret values stay controlled by 1Password and
are never shared with Kiro.

> **Platform:** The MCP server is currently macOS only.

## Documentation

> ⚠️ **Placeholder URL** — points to a not-yet-published page while
> development continues:

https://www.1password.dev/environments/mcp-kiro-server

## Prerequisites

- macOS with the [1Password desktop]() app installed.
- The **1Password Labs MCP Server** experiment enabled in the desktop app.
  Open the Labs settings with this link: `onepassword://settings/labs`
- A 1Password account with Developer Environments enabled.

The local MCP server is expected at:

```text
/Applications/1Password.app/Contents/MacOS/onepassword-mcp
```

## What's Included

| File                     | Purpose                                                                                                                           |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------- |
| [`mcp.json`](mcp.json)   | MCP server configuration that points Kiro at the bundled `onepassword-mcp` binary.                                                |
| [`POWER.md`](POWER.md)   | Steering doc describing when and how Kiro should use the 1Password MCP server, the exposed tools, common flows, and safety rules. |
| [`LICENSE`](LICENSE)     | MIT license.                                                                                                                      |

## Example Prompts

Once the plugin is enabled in Kiro, you can ask for tasks like:

- "List my 1Password Environments"
- "Create a local .env mount here"
- "Show me the variable names in my project environment"
- "Add a placeholder variable for my OpenAI API key"
- "Create a new Environment called my-project"

The 1Password desktop app may prompt for approval when Kiro connects to the
MCP server or accesses an Environment.

## License and Support

This power integrates with [1Password](https://1password.com) (Apache-2.0).

- [Privacy Policy](https://1password.com/legal/privacy)
- [Support](https://support.1password.com)
- [MIT LICENSE](LICENSE)
