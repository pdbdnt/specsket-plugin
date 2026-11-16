# Staging recovery: when an outcome occurs

Part of [the workflow contract](workflow-contract.md); every rule there applies.

## When the 502 is the app, not the MCP

A `502 integration_upstream_transport_failed` from every tool while the MCP health check is fine means the Specsket app behind it is not reachable. Stop, do not retry writes, and tell the user the app needs to be started; ask whoever runs the local Specsket runtime to restart the app.
