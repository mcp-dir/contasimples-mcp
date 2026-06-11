# Instalação rápida

Conta Simples é um servidor MCP remoto hospedado em `https://api.mcp.ai/p_contasimples`. Você não baixa nem roda nada localmente — só aponta seu cliente pra essa URL.

A auth acontece em runtime: clientes com **OAuth 2.1** (Claude Desktop, Cursor, VS Code recentes) abrem o browser na 1ª chamada (magic-link). Clientes sem OAuth recebem a tool `authenticate` — abra `https://app.mcp.ai/agent-auth`, faça login, copie o JWT e cole no chat.

---

## Claude (Web e Desktop)

[➕ Abrir no Claude e conectar](https://claude.ai/customize/connectors?modal=add-custom-connector&mcpName=Conta%20Simples&mcpServerUrl=https%3A%2F%2Fapi.mcp.ai%2Fp_contasimples)

Manual: [claude.ai/customize/connectors](https://claude.ai/customize/connectors) → **+** → **Adicionar conector personalizado** → `Conta Simples` / `https://api.mcp.ai/p_contasimples`.

Config file (legado): `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS) ou `%APPDATA%\Claude\claude_desktop_config.json` (Windows):

```json
{ "mcpServers": { "contasimples": { "type": "http", "url": "https://api.mcp.ai/p_contasimples" } } }
```

## Cursor

[➕ Instalar no Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=contasimples&config=eyJ1cmwiOiJodHRwczovL2FwaS5tY3AuYWkvcF9jb250YXNpbXBsZXMifQ==)

`.cursor/mcp.json`:
```json
{ "mcpServers": { "contasimples": { "url": "https://api.mcp.ai/p_contasimples" } } }
```

## VS Code (Copilot Chat)

[➕ Instalar no VS Code](vscode:mcp/install?name=contasimples&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fapi.mcp.ai%2Fp_contasimples%22%7D)

`.vscode/mcp.json`:
```json
{ "servers": { "contasimples": { "type": "http", "url": "https://api.mcp.ai/p_contasimples" } } }
```

## Outros clientes MCP

Qualquer cliente com **MCP over HTTP**. URL fixa:

```
https://api.mcp.ai/p_contasimples
```

Dúvidas? [contasimples@mcp.ai](mailto:contasimples@mcp.ai)
