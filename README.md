# MCP_hubecommerce

Prompts e manual para consultar o **Hub eCommerce** da TOTVS (`https://hubecommerce.totvs.app`) a partir do **Kiro**, usando o servidor MCP do **Playwright** (`@playwright/mcp`).

O Hub não tem um servidor MCP próprio. O Playwright abre um navegador real controlado pelo Kiro, e os prompts deste repositório guardam o passo a passo de cada consulta. O login é manual (SSO/MFA): você loga na janela aberta e o Kiro continua a partir da página logada.

As consultas são **somente leitura**. Cadastrar, salvar, excluir, avançar ou reprocessar exige confirmação explícita. Tokens e cookies nunca são lidos nem gravados.

## Pré-requisitos

- Node.js instalado
- Chromium do Playwright: `npx playwright install chromium`
- Acesso ao Hub eCommerce com o seu usuário

## Instalação

**1. Registre o servidor Playwright** em `~/.kiro/settings/mcp.json` (usuário) ou `.kiro/settings/mcp.json` (workspace):

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest"]
    }
  }
}
```

Depois reconecte os servidores MCP no Kiro. O servidor sobe sozinho; não há comando de login.

**2. Copie os prompts** da pasta `prompts/` para `~/.kiro/steering/`.

**3. Ajuste os caminhos.** Os prompts `hub-*` salvam uma cópia de cada consulta numa pasta do Drive do time. Troque esse caminho por uma pasta sua, se necessário.

## Prompts disponíveis

| Prompt | Descrição | Entrada |
|---|---|---|
| `playwright-sites` | Carregado automaticamente. Define como abrir o Hub, aguardar o login e as regras de segurança. | — |
| `hub-ler-menu` | Lista o menu completo do Hub com a URL de cada tela. | — |
| `hub-pedidos-com-erro` | Lista os pedidos em "Pendentes com erro" (Gestão de integração > Pedidos). | — |
| `hub-consultar-pedido` | Busca um pedido em "Todos os registros" e retorna cliente, status, Id ERP e situação da integração. | id do pedido, CPF ou CNPJ |
| `hub-ler-tela-config` | Lê uma tela de configuração (filtros, colunas e dados). | nome ou URL da tela |

## Uso

No chat do Kiro, digite `#` e o nome do prompt:

```
#hub-ler-menu
#hub-pedidos-com-erro
#hub-consultar-pedido 1675980522101-01
#hub-ler-tela-config Clientes
```

Fluxo:

1. O Kiro abre o Hub na janela do Playwright.
2. Se pedir login, faça o login na janela e avise o Kiro.
3. O Kiro confirma a página, executa os passos e responde com o resultado.

## Dicas

- A tela `/orders` abre em "Pendentes com erro". Pedidos já integrados só aparecem em "Todos os registros".
- As tabelas carregam depois da página. Os prompts esperam o texto "Carregando" sumir antes de ler.
- Para manter o login entre sessões, adicione `--user-data-dir <pasta>` em `args`. Confirme os argumentos com `npx @playwright/mcp@latest --help`.
- Para evitar mudanças inesperadas, troque `@latest` por uma versão fixa.

## Segurança

- Somente leitura por padrão; ações que alteram dados pedem confirmação.
- Tokens e cookies nunca são copiados para arquivos ou respostas.
- Os resultados das consultas podem conter dados de clientes. Guarde-os em pasta com acesso restrito.

## Documentação

O passo a passo completo, com o mapa de telas do Hub e a solução de problemas, está em [Manual - Hub eCommerce com Playwright MCP.md](Manual%20-%20Hub%20eCommerce%20com%20Playwright%20MCP.md) (também em [PDF](Manual%20-%20Hub%20eCommerce%20com%20Playwright%20MCP.pdf)).
