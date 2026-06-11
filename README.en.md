# Conta Simples

### Conta Simples for Claude, Cursor and AI agents

Business finance on Conta Simples, bank and card statements, corporate cards (list/block/unblock), categories, users, roles and invites. Generate your credentials at https://ib.contasimples.com/integracoes/api/credenciais

- 📊 **14 tools**
- ✏️ **Read and write**
- 💬 **Works with any MCP client**: Claude Desktop, Cursor, VS Code, Cline, Continue
- 🔑 **Magic-link login (no password)**

[Portuguese version](README.md) · [Full docs (PT-BR)](docs/)

---

## One-click install

### Claude (Web and Desktop)

[➕ Open in Claude and connect](https://claude.ai/customize/connectors?modal=add-custom-connector&mcpName=Conta%20Simples&mcpServerUrl=https%3A%2F%2Fapi.mcp.ai%2Fp_contasimples)

Manual: [claude.ai/customize/connectors](https://claude.ai/customize/connectors) → **+** → **Add custom connector** → name `Conta Simples`, URL `https://api.mcp.ai/p_contasimples`.

### Cursor

[➕ Install in Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=contasimples&config=eyJ1cmwiOiJodHRwczovL2FwaS5tY3AuYWkvcF9jb250YXNpbXBsZXMifQ==)

### VS Code (Copilot Chat)

[➕ Install in VS Code](vscode:mcp/install?name=contasimples&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fapi.mcp.ai%2Fp_contasimples%22%7D)

### Any other MCP-over-HTTP client

```
https://api.mcp.ai/p_contasimples
```

---

## 14 tools

| Tool | Description |
|---|---|
| `contasimples_list_accounts` | Lista conexões Conta Simples (empresas) vinculadas a este install — id, label e apelido de exibição. |
| `contasimples_summary` | Resumo agregado do período (bancário + cartão) — uma chamada em vez de paginar centenas de transações. Retorna totais (entradas/saídas, PIX/TED, cartão compras/IOF/estorno), top categorias e top estabelecimentos, cash_flow e flags _meta (truncated se passar do limite de páginas). Período máximo 62 dias (API). Por padrão só transações bancárias processadas (status=2); include_canceled=true busca todas e exclui canceladas (status=1) dos totais, contando-as em banking.canceled_excluded. Opcional: accounts — agrega N empresas em paralelo, resposta vem em `accounts[]` (uma entrada por conta) + `errors[]` se alguma falhar. Opcional: keywords — restrinja o agregado às transações cuja descrição/contraparte/categoria/tipo contenham qualquer uma das palavras (_meta.keyword_filter mostra matched vs scanned). |
| `contasimples_card` | Lista cartões corporativos (crédito). Filtros opcionais: status (ACTIVATED|BLOCKED|CANCELLED|INACTIVATED), type (PHYSICAL|VIRTUAL), email, product_name, last4, limit, next_page_start_key. Opcional: accounts — lista cartões de N empresas em paralelo (`accounts[]` + `errors[]`). `next_page_start_key` proibido quando accounts > 1; pagine por conta isoladamente. |
| `contasimples_card_write_block` | Mutações em cartão: block (body reason opcional), unblock. Aceita `accounts` com 1 entrada (ou omitido em install single-account) — o card_id existe em apenas uma empresa. [Flattened action: block] Bulk support: accepts card_ids for batched execution. |
| `contasimples_card_write_unblock` | Mutações em cartão: block (body reason opcional), unblock. Aceita `accounts` com 1 entrada (ou omitido em install single-account) — o card_id existe em apenas uma empresa. [Flattened action: unblock] Bulk support: accepts card_ids for batched execution. |
| `contasimples_category` | Lista categorias financeiras. Aceita `accounts` (agrupa por conta em `accounts[]`). |
| `contasimples_statement_card` | Extrato de cartão de crédito. start_date e end_date (YYYY-MM-DD) emparelhados; janela ≤ 62 dias. types opcional (ex. PURCHASE). limit 5–100. Opcional: keywords — filtro textual client-side nesta página (_meta.keyword_filter). Opcional: accounts — agrega N empresas em paralelo (`accounts[]` + `errors[]`). `next_page_start_key` proibido quando accounts > 1; pagine por conta isoladamente. |
| `contasimples_statement_banking` | Extrato bancário (transações). start_date e end_date devem vir juntos ou omitidos. No máximo um entre amount_eq, amount_gt, amount_lt. limit 1–50. Opcional: keywords — filtro textual client-side nesta página (_meta.keyword_filter). Opcional: accounts — agrega N empresas em paralelo (`accounts[]` + `errors[]`). `next_page_start_key` proibido quando accounts > 1; pagine por conta isoladamente. |
| `contasimples_attachment` | Baixa anexo por ID (PNG/JPEG/PDF) — retorna content_base64, content_type, size_bytes. Aceita `accounts` com 1 entrada (ou omitido em install single-account). Bulk support: accepts attachment_ids for batched execution. |
| `contasimples_user` | Lista usuários da empresa (email, limit, next_page_start_key opcionais). Aceita `accounts` (agrupa por conta). |
| `contasimples_user_write_delete` | Remove usuário da empresa (DELETE na API). action: delete. Aceita `accounts` com 1 entrada. [Flattened action: delete] Bulk support: accepts user_ids for batched execution. |
| `contasimples_role` | Lista papéis (roles) para convites de usuário. Aceita `accounts` (agrupa por conta). |
| `contasimples_invite` | Lista convites pendentes ou histórico (status, role_id, limit, next_page_start_key). Aceita `accounts` (agrupa por conta). Bulk support: accepts role_ids for batched execution. |
| `contasimples_invite_write_create` | Cria convite: action create com role_id e email. Aceita `accounts` com 1 entrada. [Flattened action: create] Bulk support: accepts role_ids for batched execution. |

---

## Pricing

Free.

---

## License

MIT — see [LICENSE](LICENSE). The MCP server at `api.mcp.ai/p_contasimples` is proprietary (hosted); this repo (manifests, docs, skills) is MIT.
