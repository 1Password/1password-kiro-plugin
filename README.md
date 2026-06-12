# 1Password Kiro Plugin

A Kiro plugin that connects Kiro to the 1Password desktop app's local MCP
server for [1Password Developer Environments](https://www.1password.dev/).

The plugin lets Kiro help with secure project environment setup — listing
Developer Environments, inspecting variable names, adding variables, and
creating local `.env` mounts. Secret values stay controlled by 1Password and
are never shared with Kiro.

> **Platform:** The MCP server is currently macOS only.

<!-- [![Add to Kiro](https://kiro.dev/images/add-to-kiro.svg)](https://kiro.dev/launch/mcp/add?name=1password&config=%7B%22command%22%3A%22%2FApplications%2F1Password.app%2FContents%2FMacOS%2Fonepassword-mcp%22%2C%22args%22%3A%5B%5D%2C%22disabled%22%3Afalse%2C%22autoApprove%22%3A%5B%22authenticate%22%2C%22list_environments%22%5D%7D) -->

## Documentation
### **Step 1: Turn on access to the MCP server**

To turn on the 1Password MCP server:

1. In 1Password, go to **Settings > Developer > MCP Server**, then enable “Integrate with MCP clients”

### Step 2: Install the Kiro Power

#### Option 1: Kiro Power Marketplace

- Search for “1Password” at https://kiro.dev/powers
- Click “Add to Kiro” to open the installation page in Kiro
- Click “+ Install” to install the Power
- Click “Try power” to validate your environment and get started.

#### Option 2: Install from GitHub

- In Kiro, click the Powers in the Kiro Activity Bar or choose “Powers: Focus on Installed view”
- Choose “Add Custom Power”
- From the menu, choose “Import Power from GitHub
- Paste in the repo URL (https://github.com/1Password/1password-kiro-plugin)
- Choose  `1password-kiro-power` in the list of installed Powers
- Click “Try power” to validate your environment and get started.


## Prerequisites

- macOS with the [1Password desktop]() app installed.
- The **1Password MCP Server** is enabled in the desktop app by enabling **Settings > Developer > Integrate with MCP Clients**
- A 1Password account with Developer Environments enabled.

The local MCP server is expected at:

```text
/Applications/1Password.app/Contents/MacOS/onepassword-mcp
```

## What's Included

| File                   | Purpose                                                                                                                           |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| [`mcp.json`](mcp.json) | MCP server configuration that points Kiro at the bundled `onepassword-mcp` binary.                                                |
| [`POWER.md`](POWER.md) | Steering doc describing when and how Kiro should use the 1Password MCP server, the exposed tools, common flows, and safety rules. |
| [`LICENSE`](LICENSE)   | MIT license.                                                                                                                      |

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
