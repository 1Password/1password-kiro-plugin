---
name: "1password"
displayName: "1Password Developer Environments"
description: "Manage 1Password Developer Environments directly from Kiro on macOS. Set up, mount, and inspect project secrets and .env files without exposing values. Only macOS is supported."
keywords: ["1password", "environment", "secret", "dotfile", "dev secrets", "op"]
author: "1Password"
---

# 1Password Environments

## Use When

- The user mentions 1Password Environments, 1Password Developer Environments, the 1Password MCP server, or local `.env` files from 1Password.
- The user asks to set up, mount, create, or sync a project `.env` file from a secret manager and 1Password is available.
- The user asks to configure repo environment variables, API keys, tokens, credentials, or secrets securely with 1Password.
- The user wants to list or compare Environment variable names without exposing secret values.

Do not use this power for unrelated password-manager tasks, arbitrary local `.env` file parsing, or non-1Password secret stores unless the user asks to migrate that configuration into 1Password.

## Onboarding

### Prerequisites

- macOS with the 1Password desktop app installed.
- 1Password Labs MCP server experiment enabled in the desktop app (`onepassword://settings/labs`).
- Access to a 1Password account with Developer Environments enabled.

The MCP server binary is at:

```text
/Applications/1Password.app/Contents/MacOS/onepassword-mcp
```

If the MCP server is unavailable, direct the user to enable the **1Password Labs MCP Server** experiment in the desktop app. If the Labs setting is missing, the account may not have the required `ai-local-mcp-server` feature flag.

### MCP Documentation Resources

The server exposes two built-in documentation resources that can be read for deeper context:

- `1password://docs/getting-started`
- `1password://docs/environments-guide`

## Tools

- `authenticate`: Authenticate with the 1Password desktop app and return the `accountId`. Always call this first in a turn unless you already have an `accountId`.
- `list_environments`: List Developer Environments for an account. Requires `accountId`.
- `create_environment`: Create a new Developer Environment. Requires `accountId` and a name.
- `rename_environment`: Rename an existing Developer Environment. Requires `accountId` and `environmentId`.
- `list_variables`: List variable _names_ in an Environment without returning values. Requires `accountId` and `environmentId`.
- `append_variables`: Add or update Environment variables. Requires `accountId` and `environmentId`. See variable schema note in the "Add or update variables" flow below.
- `create_local_env_file`: Mount an Environment as a local `.env` file. Requires `accountId`, `environmentId`, `environmentName`, and an absolute `mountPath`. macOS only.
- `list_local_env_files`: List existing local `.env` mounts for an Environment. Requires `accountId` and `environmentId`.

> **Parameter naming:** The server may return `account_id` and `environment_id` in responses, but MCP tool calls use camelCase — `accountId`, `environmentId`. Use camelCase in all tool calls.

## Common Flows

### Authenticate and resolve an Environment

Most operations start here. Run this sequence at the beginning of any turn unless you already hold both IDs.

1. Call `authenticate`. The 1Password desktop app will prompt the user for approval on first connection.
2. Store the returned `accountId`.
3. Call `list_environments` with `accountId`.
4. If the target Environment is unambiguous from the user's request, use it. Otherwise, ask the user to choose — do not guess.
5. Store the returned `environmentId`.

### Mount a 1Password Environment as this repo's `.env`

1. Authenticate and resolve the Environment (see above).
2. Call `list_local_env_files` with `accountId` and `environmentId` to check for an existing mount at the target path.
3. If the user says "here", "this repo", or "this project", derive the absolute path by appending `/.env` to the current workspace root.
4. If no duplicate exists, call `create_local_env_file` with `accountId`, `environmentId`, `environmentName`, and the absolute `mountPath`.
5. Report the mount path and Environment name. Do not read the mounted `.env` file to verify it — use `list_local_env_files` instead.

### Inspect variables

1. Authenticate and resolve the Environment (see above).
2. Call `list_variables`. Summarize the returned variable names only — do not request or display values.

### Create a new Environment

1. Call `authenticate` and store `accountId`.
2. Confirm the intended name with the user if it was not explicitly stated.
3. Call `create_environment` with `accountId` and the chosen name.
4. Store the `environmentId` from the response for any follow-on operations.
5. If the user wants to add variables immediately, proceed to the "Add or update variables" flow.

### Add or update variables

1. Confirm the user explicitly wants to create or update variables, and collect any missing names or values before proceeding.
2. Authenticate and resolve the Environment (see above).
3. Call `list_variables` first to identify whether the requested variable names already exist.
4. Call `append_variables` using the active MCP tool schema exactly as exposed in the current session.
   - If the schema accepts structured objects: use `{ "name": "API_KEY", "value": "...", "concealed": true }` for secrets; use `"concealed": false` only for non-sensitive values such as URLs or feature flags.
   - If the schema exposes `variables` as `string[]`: use the string format required by that schema. Ask for clarification if the format is ambiguous.

## Error Handling

- **Authentication or environment access fails:** Tell the user the 1Password desktop app may need approval, unlocking, or account access.
- **MCP server unavailable:** Direct the user to enable the 1Password Labs MCP Server experiment in the desktop app via `onepassword://settings/labs`. If the setting is missing, the account may lack the `ai-local-mcp-server` feature flag.
- **`create_local_env_file` fails:** Confirm the user is on macOS. This tool is not available on other platforms even though 1Password Developer Environments work on Linux.

## Safety

- Do not reveal, log, or echo secret values.
- Do not read a mounted `.env` file to verify it exists — use `list_local_env_files` instead.
- Ask before creating or modifying Environment variables unless the user's request is already explicit.
- Treat local `.env` mounts as sensitive even though 1Password does not persist plaintext secret contents to disk.
- If a user pastes a secret into the chat, do not repeat it back — refer to it by variable name only.
