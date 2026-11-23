---
inclusion: manual
---

# Hub Ecommerce: ler uma tela de configuracao

Entrada: o **nome da tela** ou a URL (por exemplo "Clientes" ou `/customer-order-config`). Se nao informado, pergunte. Use o servidor `playwright`. Somente leitura.

## Telas conhecidas (URL)

- ERP: `/choose-erp`, `/question`, `/erp-data`, `/branches`, `/branch-configuration`
- Ecommerce: `/ecommerce-data`
- Estoque e Envios: `/stock`, `/order-carrier`
- Pagamentos: `/payment-method`, `/payment-type`
- Pedidos e Clientes: `/customer-order-config`, `/order-config`, `/order-status`, `/order-affiliate`
- SyncControl: `/sync-control`, `/sync-control-branch`
- Outras: `/dictionarys`, `/price-table`, `/seller`
- Catalogo: `/business-line`, `/product-brand`, `/product-category`, `/product-root`, `/product-sku`
- Gestao de integracao: `/order-stage`, `/customer-catalog`, `/orders`, `/documents`
- Outros: `/contact`, `/useful-links`

## Passos

1. Abra `https://hubecommerce.totvs.app` + a URL da tela. Se a pagina pedir login, pare, avise o usuario e aguarde.
2. Use `browser_wait_for` com `textGone: "Carregando"` antes de ler.
3. Leia o arquivo de snapshot e responda com: titulo da tela, botoes, filtros, colunas da tabela e os dados exibidos.
4. Se a tabela mostrar "Nenhum dado encontrado", diga que a lista esta vazia e informe se o indicador de carregamento ja tinha sumido.

## Regras

- Nao clique em "Cadastrar", "Salvar", "Excluir" ou "Avancar". Se o usuario pedir uma dessas acoes, confirme antes.
- Nao leia nem copie cookies ou tokens.

## Salvar copia da consulta

Ao terminar, salve uma copia do resultado em Markdown em `g:\Drives compartilhados\Time TOTVS iPaaS\Suporte\Análises Clientes\Hub eCommerce\consultas\`, com o nome `AAAA-MM-DD_HHMM_<tipo>[_<id>].md` (crie a pasta se nao existir). O arquivo traz: data e hora, URL consultada, filtros usados e o resultado. Nao grave cookies, tokens nem o conteudo bruto do snapshot. Informe o caminho do arquivo na resposta.