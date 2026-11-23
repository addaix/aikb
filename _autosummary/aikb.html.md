# aikb

Programmatic CRUD for AI project knowledge bases (Claude Projects, and a local provider; Gemini Gems are planned, not implemented).

### Modules

| [`base`](aikb.base.html.md#module-aikb.base)                                    | Core module: protocols, stores, providers, and factory functions for aikb.   |
|-----------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------|
| [`mcp_server`](aikb.mcp_server.html.md#module-aikb.mcp_server)                        | MCP server exposing knowledge base CRUD as tools.                            |
| [`sync`](aikb.sync.html.md#aikb.sync)(a, b, \*[, manifest, manifest_path, ...]) | Bidirectional sync with three-way reconciliation.                            |
