---
name: contasimples-mcp
description: Skill da REST API do Conta Simples na MCP.AI: 13 endpoints em /api/contasimples. Gestão financeira PJ no Conta Simples, extratos bancário e de cartão, cartões corporativos (listar/bloquear/desbloquear), categorias, usuários, papéis e convites. Gere suas credenciais em https://ib.contasimples.com/integracoes/api/credenciais Autentique com workspace API key (sk_live) gerada em app.mcp.ai/settings/api-keys. Use quando o usuário pedir algo coberto pelos endpoints.
---

# Conta Simples — REST API skill

Você tem acesso à **Conta Simples** REST API na MCP.AI.

> Gestão financeira PJ no Conta Simples, extratos bancário e de cartão, cartões corporativos (listar/bloquear/desbloquear), categorias, usuários, papéis e convites. Gere suas credenciais em https://ib.contasimples.com/integracoes/api/credenciais

## Base URL

```
https://api.mcp.ai/api/contasimples
```

Todo endpoint é um **POST** na Base URL + o path abaixo. Os parâmetros vão no corpo JSON.

## Autenticação

Inclua em toda request:

```
Authorization: Bearer sk_live_...
Content-Type: application/json
```

> Gere sua chave em **https://app.mcp.ai/settings/api-keys** (workspace API key `sk_live_…`, não expira, revogável). Uma única chave serve pra todos os seus MCPs.

## Formato de resposta

```json
{ "ok": true, "tool": "<tool_id>", "result": <payload> }
```

## Exemplo cURL

```bash
curl -X POST https://api.mcp.ai/api/contasimples/accounts \
  -H "Authorization: Bearer sk_live_..." \
  -H "Content-Type: application/json" \
  -d '{}'
```

## Reportar problemas

Se um endpoint retornar erro, vazio ou dado inesperado, reporte (não desista calado): **POST /api/contasimples/report** com `{ "message": "...", "context"?: "...", "conversation"?: [...] }`. Isso notifica o time da MCP.AI.

## Endpoints (13)

#### `contasimples_list_accounts`

Listar conexões (empresas) _(POST /api/contasimples/accounts)_

#### `contasimples_summary`

Resumo financeiro do período (bancário + cartão) _(POST /api/contasimples/summary)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `start_date` | string | Sim | Início YYYY-MM-DD |
| `end_date` | string | Sim | Fim YYYY-MM-DD |
| `top_n` | number | Não | Top N categorias/estabelecimentos (1-50, default 10) |
| `include_canceled` | boolean | Não | Incluir transações canceladas |
| `keywords` | string[] | Não | Busca textual client-side (OR, case-insensitive). Varre descrição/notes, contraparte do PIX/TED (sourceDestinationName), portador (bearerName), estabelecimento, categoria, tipo (transactionType.subType/description), operador/solicitante (user.*, requesterUser.*), centro de custo e, em cartão, o titular do cartão (card.responsibleName/responsibleEmail). Ver `_meta.keyword_filter.fields_searched` na resposta para a lista exata varrida. |
| `movement_type` | string | Não | Filtra transações bancárias por direção: `in` (entradas — PIX_IN, TED_IN, depósitos, rendimentos CDI) ou `out` (saídas — PIX_OUT, TED_OUT, PAYMENT, débitos). Aplicado client-side após o fetch (a API Conta Simples não suporta esse filtro nativamente). `_meta.movement_type_filter` reporta matched vs scanned. (in, out) |
| `accounts` | string[] | Não | Lista de ids/labels/api_key (parcial) das empresas vinculadas a este install. Em tools de leitura, a resposta vem agrupada em `accounts[]` + `errors[]`. Em tools de escrita e `contasimples_attachment`, exige exatamente 1 entrada. Max 20. Use `contasimples_list_accounts` para descobrir. |

#### `contasimples_card`

Listar cartões corporativos _(POST /api/contasimples/cards)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `status` | string | Não | Filtro por status (ACTIVATED, BLOCKED, CANCELLED, INACTIVATED) |
| `type` | string | Não | Tipo do cartão (PHYSICAL, VIRTUAL) |
| `email` | string | Não | Email do portador |
| `product_name` | string | Não | Nome do produto |
| `last4` | string | Não | Últimos 4 dígitos |
| `limit` | number | Não | Máximo de resultados (1-100) |
| `next_page_start_key` | string | Não | Cursor de paginação (account-específico — proibido com accounts > 1) |
| `accounts` | string[] | Não | Lista de ids/labels/api_key (parcial) das empresas vinculadas a este install. Em tools de leitura, a resposta vem agrupada em `accounts[]` + `errors[]`. Em tools de escrita e `contasimples_attachment`, exige exatamente 1 entrada. Max 20. Use `contasimples_list_accounts` para descobrir. |

#### `contasimples_card_write`

Bloquear ou desbloquear cartão _(POST /api/contasimples/cards/write)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `action` | string | Sim | Ação a executar (block, unblock) |
| `card_id` | string | Sim | ID do cartão |
| `reason` | string | Não | Motivo (somente block) |
| `accounts` | string[] | Não | Lista de ids/labels/api_key (parcial) das empresas vinculadas a este install. Em tools de leitura, a resposta vem agrupada em `accounts[]` + `errors[]`. Em tools de escrita e `contasimples_attachment`, exige exatamente 1 entrada. Max 20. Use `contasimples_list_accounts` para descobrir. |

#### `contasimples_category`

Listar categorias financeiras _(POST /api/contasimples/categories)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `accounts` | string[] | Não | Lista de ids/labels/api_key (parcial) das empresas vinculadas a este install. Em tools de leitura, a resposta vem agrupada em `accounts[]` + `errors[]`. Em tools de escrita e `contasimples_attachment`, exige exatamente 1 entrada. Max 20. Use `contasimples_list_accounts` para descobrir. |

#### `contasimples_statement_card`

Extrato de cartão de crédito _(POST /api/contasimples/statements/card)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `start_date` | string | Não | Início YYYY-MM-DD |
| `end_date` | string | Não | Fim YYYY-MM-DD |
| `limit` | number | Não | Máximo de resultados (5-100) |
| `types` | string[] | Não | Filtro por tipo (ex: PURCHASE) |
| `next_page_start_key` | string | Não | Cursor de paginação (account-específico — proibido com accounts > 1) |
| `keywords` | string[] | Não | Busca textual client-side (OR, case-insensitive). Varre descrição/notes, contraparte do PIX/TED (sourceDestinationName), portador (bearerName), estabelecimento, categoria, tipo (transactionType.subType/description), operador/solicitante (user.*, requesterUser.*), centro de custo e, em cartão, o titular do cartão (card.responsibleName/responsibleEmail). Ver `_meta.keyword_filter.fields_searched` na resposta para a lista exata varrida. |
| `accounts` | string[] | Não | Lista de ids/labels/api_key (parcial) das empresas vinculadas a este install. Em tools de leitura, a resposta vem agrupada em `accounts[]` + `errors[]`. Em tools de escrita e `contasimples_attachment`, exige exatamente 1 entrada. Max 20. Use `contasimples_list_accounts` para descobrir. |

#### `contasimples_statement_banking`

Extrato bancário (transações) _(POST /api/contasimples/statements/banking)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account_id` | string | Não | ID da conta |
| `start_date` | string | Não | Início YYYY-MM-DD |
| `end_date` | string | Não | Fim YYYY-MM-DD |
| `limit` | number | Não | Máximo de resultados (1-50) |
| `sorting` | string | Não | Ordenação |
| `has_attachments` | boolean | Não | Filtrar com anexo |
| `was_conciled` | boolean | Não | Filtrar conciliadas |
| `category_ids` | string[] | Não | IDs de categorias |
| `responsible_email` | string | Não | Email do responsável |
| `status` | number | Não | 1=cancelado 2=processado 3=pendente |
| `amount_eq` | string | Não | Valor exato |
| `amount_gt` | string | Não | Valor maior que |
| `amount_lt` | string | Não | Valor menor que |
| `next_page_start_key` | string | Não | Cursor de paginação (account-específico — proibido com accounts > 1) |
| `keywords` | string[] | Não | Busca textual client-side (OR, case-insensitive). Varre descrição/notes, contraparte do PIX/TED (sourceDestinationName), portador (bearerName), estabelecimento, categoria, tipo (transactionType.subType/description), operador/solicitante (user.*, requesterUser.*), centro de custo e, em cartão, o titular do cartão (card.responsibleName/responsibleEmail). Ver `_meta.keyword_filter.fields_searched` na resposta para a lista exata varrida. |
| `movement_type` | string | Não | Filtra transações bancárias por direção: `in` (entradas — PIX_IN, TED_IN, depósitos, rendimentos CDI) ou `out` (saídas — PIX_OUT, TED_OUT, PAYMENT, débitos). Aplicado client-side após o fetch (a API Conta Simples não suporta esse filtro nativamente). `_meta.movement_type_filter` reporta matched vs scanned. (in, out) |
| `accounts` | string[] | Não | Lista de ids/labels/api_key (parcial) das empresas vinculadas a este install. Em tools de leitura, a resposta vem agrupada em `accounts[]` + `errors[]`. Em tools de escrita e `contasimples_attachment`, exige exatamente 1 entrada. Max 20. Use `contasimples_list_accounts` para descobrir. |

#### `contasimples_attachment`

Baixar anexo de transação _(POST /api/contasimples/attachments)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `attachment_id` | string | Sim | ID do anexo |
| `accounts` | string[] | Não | Lista de ids/labels/api_key (parcial) das empresas vinculadas a este install. Em tools de leitura, a resposta vem agrupada em `accounts[]` + `errors[]`. Em tools de escrita e `contasimples_attachment`, exige exatamente 1 entrada. Max 20. Use `contasimples_list_accounts` para descobrir. |

#### `contasimples_user`

Listar usuários da empresa _(POST /api/contasimples/users)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `email` | string | Não | Filtrar por email |
| `limit` | number | Não | Máximo de resultados |
| `next_page_start_key` | string | Não | Cursor de paginação (account-específico — proibido com accounts > 1) |
| `accounts` | string[] | Não | Lista de ids/labels/api_key (parcial) das empresas vinculadas a este install. Em tools de leitura, a resposta vem agrupada em `accounts[]` + `errors[]`. Em tools de escrita e `contasimples_attachment`, exige exatamente 1 entrada. Max 20. Use `contasimples_list_accounts` para descobrir. |

#### `contasimples_user_write`

Remover usuário da empresa _(POST /api/contasimples/users/write)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `action` | string | Sim | Ação (delete) |
| `user_id` | string | Sim | ID do usuário |
| `accounts` | string[] | Não | Lista de ids/labels/api_key (parcial) das empresas vinculadas a este install. Em tools de leitura, a resposta vem agrupada em `accounts[]` + `errors[]`. Em tools de escrita e `contasimples_attachment`, exige exatamente 1 entrada. Max 20. Use `contasimples_list_accounts` para descobrir. |

#### `contasimples_role`

Listar papéis (roles) _(POST /api/contasimples/roles)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `accounts` | string[] | Não | Lista de ids/labels/api_key (parcial) das empresas vinculadas a este install. Em tools de leitura, a resposta vem agrupada em `accounts[]` + `errors[]`. Em tools de escrita e `contasimples_attachment`, exige exatamente 1 entrada. Max 20. Use `contasimples_list_accounts` para descobrir. |

#### `contasimples_invite`

Listar convites _(POST /api/contasimples/invites)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `status` | string | Não | Filtrar por status |
| `role_id` | string | Não | Filtrar por papel |
| `limit` | number | Não | Máximo de resultados |
| `next_page_start_key` | string | Não | Cursor de paginação (account-específico — proibido com accounts > 1) |
| `accounts` | string[] | Não | Lista de ids/labels/api_key (parcial) das empresas vinculadas a este install. Em tools de leitura, a resposta vem agrupada em `accounts[]` + `errors[]`. Em tools de escrita e `contasimples_attachment`, exige exatamente 1 entrada. Max 20. Use `contasimples_list_accounts` para descobrir. |

#### `contasimples_invite_write`

Criar convite de usuário _(POST /api/contasimples/invites/write)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `action` | string | Sim | Ação (create) |
| `role_id` | string | Sim | ID do papel |
| `email` | string | Sim | Email do convidado |
| `accounts` | string[] | Não | Lista de ids/labels/api_key (parcial) das empresas vinculadas a este install. Em tools de leitura, a resposta vem agrupada em `accounts[]` + `errors[]`. Em tools de escrita e `contasimples_attachment`, exige exatamente 1 entrada. Max 20. Use `contasimples_list_accounts` para descobrir. |

---

Este MCP também funciona via **conexão MCP** (Claude / Cursor) em `https://api.mcp.ai/p_contasimples` — veja o [README](../../README.md). A skill acima é pra consumir a **REST API** direto (agente próprio / código).
