---
inclusion: manual
---

# Hub Ecommerce: listar pedidos pendentes com erro

Use o servidor `playwright`. Somente leitura.

## Passos

1. Abra `https://hubecommerce.totvs.app/orders` (Gestao de integracao > Pedidos). Se a pagina pedir login, pare, avise o usuario e aguarde.
2. Use `browser_wait_for` com `textGone: "Carregando"` para esperar a tabela carregar.
3. Confirme que o filtro "Filtrar por" esta em **Pendentes com erro**. Se nao estiver, selecione essa opcao.
4. Leia a tabela pelo arquivo de snapshot. Colunas: Id E-com, Cliente, Valor, Status, Criado Hub, Id ERP, Situacao da Integracao.
5. Responda com a lista de pedidos, ou diga que nao ha pedidos pendentes com erro. Informe quantos registros a tela exibe. Se o botao "Carregar mais resultados" estiver habilitado, avise que ha mais paginas.

## Regras

- Nao reprocese, edite nem exclua pedidos.
- Nao leia nem copie cookies ou tokens.

## Salvar copia da consulta

Ao terminar, salve uma copia do resultado em Markdown em `g:\Drives compartilhados\Time TOTVS iPaaS\Suporte\Análises Clientes\Hub eCommerce\consultas\`, com o nome `AAAA-MM-DD_HHMM_<tipo>[_<id>].md` (crie a pasta se nao existir). O arquivo traz: data e hora, URL consultada, filtros usados e o resultado. Nao grave cookies, tokens nem o conteudo bruto do snapshot. Informe o caminho do arquivo na resposta.