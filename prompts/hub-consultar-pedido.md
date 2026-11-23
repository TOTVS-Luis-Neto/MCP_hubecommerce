---
inclusion: manual
---

# Hub Ecommerce: consultar um pedido por id

Entrada: o **id do pedido** (por exemplo `1675980522101-01`). Se o usuario nao informar, pergunte. Tambem aceita CPF ou CNPJ. Use o servidor `playwright`. Somente leitura.

## Passos

1. Abra `https://hubecommerce.totvs.app/orders`. Se a pagina pedir login, pare, avise o usuario e aguarde.
2. Use `browser_wait_for` com `textGone: "Carregando"`.
3. Altere o filtro "Filtrar por" para **Todos os registros**. O dropdown e customizado: clique no texto do filtro atual e depois na opcao.
4. Digite o id no campo "cpf / cnpj / id pedido" e envie com Enter.
5. Aguarde 3 segundos e leia a tabela pelo arquivo de snapshot.
6. Responda com uma tabela: Id E-com, Cliente, Valor, Status, Criado Hub, Id ERP, Situacao da Integracao.
7. Se nao houver linha, diga que o pedido nao foi encontrado nesse filtro e sugira conferir o id.

## Regras

- Se a coluna Valor vier vazia, informe isso e nao invente o valor.
- Nao reprocese, edite nem exclua o pedido. Abrir o detalhe pela coluna de acoes e permitido, so para leitura.
- Nao leia nem copie cookies ou tokens.

## Salvar copia da consulta

Ao terminar, salve uma copia do resultado em Markdown em `g:\Drives compartilhados\Time TOTVS iPaaS\Suporte\Análises Clientes\Hub eCommerce\consultas\`, com o nome `AAAA-MM-DD_HHMM_<tipo>[_<id>].md` (crie a pasta se nao existir). O arquivo traz: data e hora, URL consultada, filtros usados e o resultado. Nao grave cookies, tokens nem o conteudo bruto do snapshot. Informe o caminho do arquivo na resposta.