# Microsoft MCP certification package

This directory contains DrillerDB's package for the Microsoft **Apps and
Agents for M365 and Copilot** certification path.

## Package contents

- `package/manifest.json` - Microsoft 365 devPreview manifest with a remote MCP
  agent connector.
- `package/mcptools.json` - the exact 45 externally visible DrillerDB tools,
  generated from the production server registry.
- `package/intro.md` - setup, capabilities, authorization, security, support,
  and known limitations.
- `package/Color.png` - 192 x 192 full-bleed color icon.
- `package/Outline.png` - 32 x 32 transparent default icon.

The manifest references `https://drillerdb-mcp-cert.vault.azure.net/`. Before
submission, create that Key Vault, store the case-sensitive OAuth secrets
required by Microsoft, and grant the Microsoft certification service principal
`8e91e74f-afe9-41cd-8c3f-17a9562a74ea` the **Key Vault Secrets User** role.

The OAuth client must allow the Microsoft redirect endpoint:

`https://teams.microsoft.com/api/platform/v1.0/oAuthRedirect`

## Build the upload

Create the zip from inside `package/` so the five package files are at the zip
root. Do not include this README or credentials in the upload.
