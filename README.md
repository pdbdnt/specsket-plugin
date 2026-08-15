# Specsket plugin beta

Install the public Specsket marketplace in ChatGPT or Codex to add evidence-backed product-ingestion and supplier-onboarding workflows.

The package is public, but functionality remains private. The plugin contains workflow instructions only; its repository does not register or embed the hosted MCP connection. Every live connection separately uses Specsket OAuth, the integration-user allowlist, workspace permissions, subscription capabilities, and request-time authorization checks. The package contains no application source code, MCP server implementation, database credentials, OAuth secrets, or private repository history.

## ChatGPT installation

1. Open **Plugins** and choose **Add plugin marketplace**.
2. Use `https://github.com/pdbdnt/specsket-plugin.git` as the source.
3. Use `main` as the Git ref and leave sparse paths empty.
4. Install **Specsket Beta**.
5. In your own ChatGPT account, enable Developer mode and add a custom MCP named **Specsket MCP Beta**.
6. Choose **Streamable HTTP**, enter `https://integrations.specsket.com/mcp`, save, and choose **Authenticate**.
7. Sign in with a Specsket account enabled for integrations, then start a new chat with the plugin and MCP connection enabled.

## Codex installation

From a clone of this repository:

```bash
codex plugin marketplace add .
codex plugin add specsket@specsket-team
codex mcp add specsket --url https://integrations.specsket.com/mcp
codex mcp login specsket
```

The plugin and MCP are separate: the plugin supplies reusable workflow instructions, while the MCP supplies authenticated tools and live Specsket access. If the MCP tools are unavailable, the skills stop instead of inventing permissions, schemas, validation, or staging results.

## Support and legal

- Product page: https://specsket.com/plugins/specsket
- Support: https://specsket.com/support
- Privacy: https://specsket.com/privacy
- Terms: https://specsket.com/terms

This package is source-available, not open source. The `LICENSE` file in the repository root permits installation and authorized use while reserving other rights. Contact `support@specsket.com` for help or additional permission.
