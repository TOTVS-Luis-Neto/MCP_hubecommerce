# Manual: consultar o Hub eCommerce pelo Kiro com Playwright MCP

Este manual descreve o que foi feito para o Kiro abrir e consultar o Hub eCommerce (`https://hubecommerce.totvs.app/`). Cobre apenas o Hub. O TDN ficou fora do escopo por enquanto.

## 1. Visão geral

Não existe um servidor MCP próprio do Hub. A solução combina duas peças:

| Peça | Função |
|---|---|
| Servidor MCP **Playwright** (`@playwright/mcp`) | Abre um navegador real controlado pelo Kiro. Navega, clica, digita e lê a página. |
| **Prompts** (arquivos de steering) | Roteiros prontos das consultas, para o Kiro repetir o mesmo passo a passo. |

O Playwright foi usado porque o Hub monta o conteúdo no navegador e exige login. A ferramenta de leitura simples de URLs (`web_fetch`) não extraiu nenhum conteúdo da página.

## 2. Pré-requisitos

- Node.js instalado.
- Chromium do Playwright instalado. Se faltar: `npx playwright install chromium`.
- Acesso ao Hub com o seu usuário (login manual, com SSO/MFA quando houver).

## 3. Passo a passo da configuração

### 3.1 Registrar o servidor Playwright

Edite `C:\Users\seu.usuario\.kiro\settings\mcp.json` e inclua o bloco abaixo dentro de `mcpServers`:

```json
"playwright": {
  "command": "npx",
  "args": ["@playwright/mcp@latest"]
}
```

Depois reconecte os servidores MCP no Kiro, pelo painel de MCP ou reiniciando o IDE. O servidor sobe sozinho. Não há comando de login como o `iniciar_login_ipaas` do iPaaS.

Para validar o arquivo, confira se o JSON abre sem erro e se `mcpServers` lista `playwright`.

### 3.2 Criar o steering do Playwright

Arquivo: `C:\Users\seu.usuario\.kiro\steering\playwright-sites.md`.

Ele é carregado automaticamente quando você pede para abrir, ler ou navegar no Hub. Define:
- usar as ferramentas do Playwright, e não o `web_fetch`;
- abrir a URL inicial, e se pedir login, parar e esperar você logar;
- confirmar com `browser_snapshot` que a página está logada;
- ler e navegar sem confirmação, mas pedir confirmação antes de qualquer ação que altere dados;
- nunca copiar tokens ou cookies.

### 3.3 Criar os prompts de consulta

Arquivos em `C:\Users\seu.usuario\.kiro\steering\`, todos de inclusão manual (só rodam quando chamados):

| Prompt | O que faz | Entrada |
|---|---|---|
| `hub-ler-menu` | Lista o menu completo com as URLs | nenhuma |
| `hub-pedidos-com-erro` | Lista os pedidos em "Pendentes com erro" | nenhuma |
| `hub-consultar-pedido` | Busca um pedido em "Todos os registros" | id do pedido, CPF ou CNPJ |
| `hub-ler-tela-config` | Lê uma tela de configuração | nome ou URL da tela |

### 3.4 Definir onde salvar as cópias

Pasta: `g:\Drives compartilhados\Time TOTVS iPaaS\Suporte\Análises Clientes\Hub eCommerce\`

```
Hub eCommerce\
├── prompts\     cópias de backup dos prompts e do steering
└── consultas\   resultado de cada consulta feita no Hub
```

Os prompts `hub-*` têm uma seção "Salvar cópia da consulta". Ao terminar, o Kiro grava um arquivo Markdown em `consultas\`, com o nome `AAAA-MM-DD_HHMM_<tipo>[_<id>].md`. O arquivo traz data e hora, URL, filtros e resultado. Não grava cookies, tokens nem o snapshot bruto.

As cópias em `prompts\` não se atualizam sozinhas. Se você editar um prompt em `.kiro\steering\`, copie o arquivo de novo.

## 4. Como usar

1. Em uma sessão do Kiro, digite `#` e o nome do prompt, com a entrada. Exemplos:
   - `#hub-ler-menu`
   - `#hub-pedidos-com-erro`
   - `#hub-consultar-pedido 1675980522101-01`
   - `#hub-ler-tela-config Clientes`
2. O Kiro abre a janela do Playwright. Se o Hub pedir login, faça o login nela e avise o Kiro.
3. O Kiro confirma o estado da página, executa os passos e responde com o resultado.
4. Confira o arquivo salvo em `Hub eCommerce\consultas\`.

## 5. Mapa do Hub

Menu lido em 17/11/2026, com o caminho de cada tela.

**Início**: `/home`

**Configurações**
- ERP: `/choose-erp`, `/question`, `/erp-data`, `/branches`, `/branch-configuration`
- Ecommerce: `/ecommerce-data`
- Estoque e Envios: `/stock`, `/order-carrier`
- Pagamentos: `/payment-method`, `/payment-type`
- Pedidos e Clientes: `/customer-order-config`, `/order-config`, `/order-status`, `/order-affiliate`
- SyncControl: `/sync-control`, `/sync-control-branch`
- Outras: `/dictionarys`, `/price-table`, `/seller`

**Cadastros (Catálogo)**: `/business-line`, `/product-brand`, `/product-category`, `/product-root`, `/product-sku`

**Gestão de integração**: `/order-stage` (Pedidos a Processar), `/customer-catalog` (Clientes), `/orders` (Pedidos), `/documents` (Faturamento)

**Outros**: `/contact` (Dados de Contato), `/useful-links` (Ajuda)

## 6. Detalhes aprendidos na consulta de pedidos

- A tela `/orders` abre com o filtro "Pendentes com erro". Pedidos já integrados só aparecem em "Todos os registros".
- O dropdown "Filtrar por" é customizado: clique no texto do filtro atual e depois na opção.
- A busca aceita CPF, CNPJ ou id do pedido, e é enviada com Enter.
- As tabelas carregam depois da página. Espere o texto "Carregando" sumir antes de ler. Sem isso, a tela mostra "Nenhum dado encontrado" mesmo com dados.
- Colunas da tela de pedidos: Id E-com, Cliente, Valor, Status, Criado Hub, Id ERP, Situação da Integração.
- Exemplo validado: o pedido `1675980522101-01` apareceu com status `invoiced` e situação "Integrado".

## 7. Segurança e boas práticas

- As consultas são somente leitura. Cadastrar, salvar, excluir, avançar e reprocessar exigem confirmação explícita.
- Tokens e cookies nunca são lidos nem gravados.
- A pasta de consultas fica em Drive compartilhado e pode conter nomes de clientes. Confirme quem tem acesso.
- Para fixar a versão do Playwright, troque `@latest` por uma versão específica em `args`.
- O login não fica salvo entre sessões. Para mantê-lo, adicione `--user-data-dir <pasta>` em `args`. Confirme os argumentos com `npx @playwright/mcp@latest --help`.

## 8. Solução de problemas

| Sintoma | Causa provável | O que fazer |
|---|---|---|
| Ferramentas `playwright` não aparecem | Servidor não conectado | Reconecte no painel de MCP ou reinicie o IDE. Rode `npx @playwright/mcp@latest --help` para testar o pacote. |
| Erro de navegador não encontrado | Chromium ausente | Rode `npx playwright install chromium`. |
| Hub pede login | Sessão nova ou expirada | Faça login na janela do Playwright e avise o Kiro. |
| Tabela vazia com "Carregando" | Leitura antes do fim do carregamento | Peça para esperar o carregamento e ler de novo. |
| Pedido não encontrado | Filtro em "Pendentes com erro" | Troque para "Todos os registros" e confira o id. |
| O `#prompt` não aparece na lista | Nome não reconhecido | Cite o nome do arquivo na mensagem. |

## 9. Arquivos envolvidos

| Arquivo | Local |
|---|---|
| Configuração dos servidores MCP | `C:\Users\seu.usuario\.kiro\settings\mcp.json` |
| Steering e prompts (originais) | `C:\Users\seu.usuario\.kiro\steering\` |
| Cópias dos prompts | `...\Análises Clientes\Hub eCommerce\prompts\` |
| Resultados das consultas | `...\Análises Clientes\Hub eCommerce\consultas\` |
| Este manual | `...\Análises Clientes\Hub eCommerce\` |

Observação: o `playwright-sites.md` também cita o TDN, que ficou fora deste manual. Esse trecho pode ser removido do steering se o TDN não for usado.
