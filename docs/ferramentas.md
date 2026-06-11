# Ferramentas

Conta Simples expõe 14 ferramentas.

### 1. `contasimples_list_accounts`
**Input**: `accounts` (opcional)

Lista conexões Conta Simples (empresas) vinculadas a este install — id, label e apelido de exibição.

### 2. `contasimples_summary`
**Input**: `start_date`, `end_date`, `top_n` (opcional), `include_canceled` (opcional), `keywords` (opcional), `movement_type` (opcional), `accounts` (opcional)

Resumo agregado do período (bancário + cartão) — uma chamada em vez de paginar centenas de transações. Retorna totais (entradas/saídas, PIX/TED, cartão compras/IOF/estorno), top categorias e top estabelecimentos, cash_flow e flags _meta (truncated se passar do limite de páginas). Período máximo 62 dias (API). Por padrão só transações bancárias processadas (status=2); include_canceled=true busca todas e exclui canceladas (status=1) dos totais, contando-as em banking.canceled_excluded. Opcional: accounts — agrega N empresas em paralelo, resposta vem em `accounts[]` (uma entrada por conta) + `errors[]` se alguma falhar. Opcional: keywords — restrinja o agregado às transações cuja descrição/contraparte/categoria/tipo contenham qualquer uma das palavras (_meta.keyword_filter mostra matched vs scanned).

### 3. `contasimples_card`
**Input**: `status` (opcional), `type` (opcional), `email` (opcional), `product_name` (opcional), `last4` (opcional), `limit` (opcional), `next_page_start_key` (opcional), `accounts` (opcional)

Lista cartões corporativos (crédito). Filtros opcionais: status (ACTIVATED|BLOCKED|CANCELLED|INACTIVATED), type (PHYSICAL|VIRTUAL), email, product_name, last4, limit, next_page_start_key. Opcional: accounts — lista cartões de N empresas em paralelo (`accounts[]` + `errors[]`). `next_page_start_key` proibido quando accounts > 1; pagine por conta isoladamente.

### 4. `contasimples_card_write_block`
**Input**: `card_id`, `reason` (opcional), `accounts` (opcional), `card_ids` (opcional)

Mutações em cartão: block (body reason opcional), unblock. Aceita `accounts` com 1 entrada (ou omitido em install single-account) — o card_id existe em apenas uma empresa. [Flattened action: block] Bulk support: accepts card_ids for batched execution.

### 5. `contasimples_card_write_unblock`
**Input**: `card_id`, `reason` (opcional), `accounts` (opcional), `card_ids` (opcional)

Mutações em cartão: block (body reason opcional), unblock. Aceita `accounts` com 1 entrada (ou omitido em install single-account) — o card_id existe em apenas uma empresa. [Flattened action: unblock] Bulk support: accepts card_ids for batched execution.

### 6. `contasimples_category`
**Input**: `accounts` (opcional)

Lista categorias financeiras. Aceita `accounts` (agrupa por conta em `accounts[]`).

### 7. `contasimples_statement_card`
**Input**: `start_date` (opcional), `end_date` (opcional), `limit` (opcional), `types` (opcional), `next_page_start_key` (opcional), `keywords` (opcional), `accounts` (opcional)

Extrato de cartão de crédito. start_date e end_date (YYYY-MM-DD) emparelhados; janela ≤ 62 dias. types opcional (ex. PURCHASE). limit 5–100. Opcional: keywords — filtro textual client-side nesta página (_meta.keyword_filter). Opcional: accounts — agrega N empresas em paralelo (`accounts[]` + `errors[]`). `next_page_start_key` proibido quando accounts > 1; pagine por conta isoladamente.

### 8. `contasimples_statement_banking`
**Input**: `account_id` (opcional), `start_date` (opcional), `end_date` (opcional), `limit` (opcional), `sorting` (opcional), `has_attachments` (opcional), `was_conciled` (opcional), `category_ids` (opcional), `cost_center_ids` (opcional), `responsible_email` (opcional), `status` (opcional), `amount_eq` (opcional), `amount_gt` (opcional), `amount_lt` (opcional), `next_page_start_key` (opcional), `keywords` (opcional), `movement_type` (opcional), `accounts` (opcional)

Extrato bancário (transações). start_date e end_date devem vir juntos ou omitidos. No máximo um entre amount_eq, amount_gt, amount_lt. limit 1–50. Opcional: keywords — filtro textual client-side nesta página (_meta.keyword_filter). Opcional: accounts — agrega N empresas em paralelo (`accounts[]` + `errors[]`). `next_page_start_key` proibido quando accounts > 1; pagine por conta isoladamente.

### 9. `contasimples_attachment`
**Input**: `attachment_id`, `accounts` (opcional), `attachment_ids` (opcional)

Baixa anexo por ID (PNG/JPEG/PDF) — retorna content_base64, content_type, size_bytes. Aceita `accounts` com 1 entrada (ou omitido em install single-account). Bulk support: accepts attachment_ids for batched execution.

### 10. `contasimples_user`
**Input**: `email` (opcional), `limit` (opcional), `next_page_start_key` (opcional), `accounts` (opcional)

Lista usuários da empresa (email, limit, next_page_start_key opcionais). Aceita `accounts` (agrupa por conta).

### 11. `contasimples_user_write_delete`
**Input**: `user_id`, `accounts` (opcional), `user_ids` (opcional)

Remove usuário da empresa (DELETE na API). action: delete. Aceita `accounts` com 1 entrada. [Flattened action: delete] Bulk support: accepts user_ids for batched execution.

### 12. `contasimples_role`
**Input**: `accounts` (opcional)

Lista papéis (roles) para convites de usuário. Aceita `accounts` (agrupa por conta).

### 13. `contasimples_invite`
**Input**: `status` (opcional), `role_id` (opcional), `limit` (opcional), `next_page_start_key` (opcional), `accounts` (opcional), `role_ids` (opcional)

Lista convites pendentes ou histórico (status, role_id, limit, next_page_start_key). Aceita `accounts` (agrupa por conta). Bulk support: accepts role_ids for batched execution.

### 14. `contasimples_invite_write_create`
**Input**: `role_id`, `email`, `accounts` (opcional), `role_ids` (opcional)

Cria convite: action create com role_id e email. Aceita `accounts` com 1 entrada. [Flattened action: create] Bulk support: accepts role_ids for batched execution.

## Prompts de exemplo

```
Liste meus cartões corporativos ativos
Mostre o extrato bancário deste mês
Liste os convites pendentes da empresa
```
