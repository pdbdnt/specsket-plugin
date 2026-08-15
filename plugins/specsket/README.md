# Specsket plugin beta

This public package contains Specsket workflow skills and a registered ChatGPT MCP connection. It does not contain the Specsket application, MCP server implementation, credentials, database access, or private repository history.

OAuth selects one eligible vendor workspace, one verified designer-private workspace, or a platform-administrator context. Product and supplier writes are staged into existing human-review queues; the plugin cannot publish or approve them.

## Install the beta marketplace

In ChatGPT, open **Plugins**, choose **Add plugin marketplace**, and enter:

- Source: `https://github.com/pdbdnt/specsket-chatgpt-plugin.git`
- Git ref: `main`
- Sparse paths: leave empty

In Codex, install from the cloned repository root:

```bash
codex plugin marketplace add .
codex plugin add specsket@specsket-team
```

After installation, connect with the Specsket account your administrator enabled for integrations and start a new chat. Each user receives an independent Specsket authorization and remains limited by their own workspace role and permissions.

## Safety boundaries

- Review the destination, warnings, and evidence summary before approving any write action.
- Successful writes return a review URL; they do not publish or approve records.
- Disabling a user in Specsket revokes the active Specsket authorization, although the connector may remain visible in ChatGPT until the user removes it.
- Never paste passwords, access tokens, private keys, or service-role credentials into a chat or support request.

Support: [support@specsket.com](mailto:support@specsket.com)
