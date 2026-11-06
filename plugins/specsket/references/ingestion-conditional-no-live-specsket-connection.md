# No live Specsket connection

Part of [the rules for specific situations](ingestion-conditional-rules.md).

If neither the authoring kit nor `specsket_get_capabilities` is available, or the **Specsket MCP Tools Beta** connection is unavailable, stop the live staging phase and explain that the public plugin supplies instructions but live Specsket access requires a separate Streamable HTTP MCP connection to `https://integrations.specsket.com/mcp`, followed by Specsket OAuth and a new chat. Do not infer the user's identity, permissions, destination, schemas, or live Specsket state, and do not claim that the current-schema preview was completed, anything was validated, or anything was staged.
