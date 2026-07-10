# Publishing this listing to MCP registries

Maintainer notes — the steps below are tied to the org's GitHub / npm
identity, so a human runs them. Everything they need is already in this
repo.

## 0. Preconditions (one-time)

1. **npm package carries `mcpName`.** The official registry verifies
   npm package ownership by reading `mcpName` from the published
   package's `package.json`. The monorepo's
   `packages/mcp-server/package.json` must contain:

   ```json
   "mcpName": "io.github.neuroverseos/bevia"
   ```

   and a version matching `server.json` (`0.1.0`) must be published:

   ```bash
   cd packages/mcp-server
   npm publish --access public
   ```

2. **GitHub identity.** `mcp-publisher login github` must authenticate
   as an account that can claim the `io.github.neuroverseos/*`
   namespace (owner / public member of the NeuroverseOS org).

## 1. Official MCP Registry (registry.modelcontextprotocol.io)

```bash
# Install the publisher CLI
brew install mcp-publisher
# (or: curl -L "https://github.com/modelcontextprotocol/registry/releases/latest/download/mcp-publisher_$(uname -s | tr '[:upper:]' '[:lower:]')_$(uname -m | sed 's/x86_64/amd64/').tar.gz" | tar xz mcp-publisher)

# From this repo's root (where server.json lives)
mcp-publisher login github     # device-code flow in the browser
mcp-publisher publish
```

Verify:

```bash
curl "https://registry.modelcontextprotocol.io/v0.1/servers?search=io.github.neuroverseos/bevia"
```

**On every release:** bump `version` in `server.json` (and the npm
package version if the proxy changed), re-run `mcp-publisher publish`.

## 2. Community directories (once the official listing is live)

- **PulseMCP** — crawls the official registry automatically; check
  pulsemcp.com after a day, or submit manually via their site.
- **mcp.so** — submit via the "Submit" affordance on mcp.so (points at
  this repo).
- **Smithery** — add via smithery.ai → publish server → point at this
  repo / the remote URL.

Each expects a public GitHub home with a README and manifest — this
repo is that home. Point every listing's "source" link here and the
"website" link at <https://bevia.co>.
