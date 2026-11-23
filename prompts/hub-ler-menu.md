---
inclusion: manual
---

# Hub Ecommerce: ler o menu

Use o servidor `playwright`. Somente leitura.

## Passos

1. Abra `https://hubecommerce.totvs.app/`. Se a pagina pedir login, pare, avise o usuario e aguarde ele logar na janela.
2. Rode `browser_snapshot`. O menu lateral aparece recolhido em parte. Se faltarem itens, leia o arquivo de snapshot indicado na resposta, que traz o menu completo com as URLs.
3. Responda com a arvore do menu: grupo, item e URL de cada tela.

## Regras

- Nao clique em itens que alterem dados. Expandir grupos do menu e permitido.
- Nao leia nem copie cookies ou tokens.

## Salvar copia da consulta

Ao terminar, salve uma copia do resultado em Markdown em `g:\Drives compartilhados\Time TOTVS iPaaS\Suporte\Análises Clientes\Hub eCommerce\consultas\`, com o nome `AAAA-MM-DD_HHMM_<tipo>[_<id>].md` (crie a pasta se nao existir). O arquivo traz: data e hora, URL consultada, filtros usados e o resultado. Nao grave cookies, tokens nem o conteudo bruto do snapshot. Informe o caminho do arquivo na resposta.