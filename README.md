# Specsket plugin beta

Install the public Specsket marketplace in ChatGPT or Codex to add evidence-backed product-ingestion and supplier-onboarding workflows.

The package is public, but functionality remains private. Every connection uses Specsket OAuth, the integration-user allowlist, workspace permissions, subscription capabilities, and request-time authorization checks. The package contains no application source code, database credentials, OAuth secrets, or private repository history.

## ChatGPT installation

1. Open **Plugins** and choose **Add plugin marketplace**.
2. Use `https://github.com/pdbdnt/specsket-chatgpt-plugin.git` as the source.
3. Use `main` as the Git ref and leave sparse paths empty.
4. Install **Specsket Beta**, connect an approved Specsket account, and start a new chat.

## Codex installation

From a clone of this repository:

```bash
codex plugin marketplace add .
codex plugin add specsket@specsket-team
```

## Support and legal

- Product page: https://specsket.com/plugins/specsket
- Support: https://specsket.com/support
- Privacy: https://specsket.com/privacy
- Terms: https://specsket.com/terms

This package is source-available, not open source. The `LICENSE` file in the repository root permits installation and authorized use while reserving other rights. Contact `support@specsket.com` for help or additional permission.
