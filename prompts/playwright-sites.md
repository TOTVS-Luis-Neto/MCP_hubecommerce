---
inclusion: auto
name: playwright-sites
description: Como abrir e usar o Hub Ecommerce (hubecommerce.totvs.app) e o TDN do iPaaS (tdn.totvs.com) com o servidor MCP playwright. Use quando o usuario pedir para abrir, ler ou navegar nesses sites.
---

# Playwright: Hub Ecommerce e TDN

## Sites conhecidos

| Apelido | URL inicial | Login |
|---|---|---|
| Hub Ecommerce | https://hubecommerce.totvs.app/ | Manual pelo usuario (SSO/MFA) |
| TDN iPaaS | https://tdn.totvs.com/display/public/IPAAS/Gateways | Nao exige para paginas publicas |

## Fluxo de inicio

1. Use as ferramentas do servidor `playwright` (`browser_navigate`, `browser_snapshot`). Nao use `web_fetch` nesses dominios: o TDN responde 403 e o Hub monta o conteudo no navegador.
2. Abra a URL inicial do site pedido.
3. Se a pagina pedir login, pare e avise o usuario para logar na janela aberta. Aguarde ele dizer que terminou.
4. Depois do aviso, rode `browser_snapshot` para confirmar que a pagina esta logada antes de continuar.

## Regras

- Leitura e navegacao podem seguir sem confirmacao.
- Peca confirmacao antes de qualquer acao que altere dados: clicar em salvar, excluir, enviar ou reprocessar, preencher formularios, ou usar `browser_evaluate`.
- Nao use `iniciar_login_ipaas` nem `confirmar_empresa` aqui. Eles servem so para o login do iPaaS e sao independentes do Playwright.
- Os snapshots sao gravados em `.playwright-mcp\` no workspace. Leia o arquivo indicado na resposta para ver o conteudo da pagina.
- Nao copie tokens, cookies ou dados de clientes para arquivos ou respostas.
- Toda consulta feita no Hub Ecommerce deve ter uma copia salva em Markdown em `g:\Drives compartilhados\Time TOTVS iPaaS\Suporte\Análises Clientes\Hub eCommerce\consultas\` (veja a secao 'Salvar copia da consulta' nos prompts hub-*).
