# Instalação

Esta página é para o vendedor ou o gestor que quer usar o CRM da Profills pelo app de IA. O conector só serve a quem tem conta ativa no CRM. O endereço do conector é `https://mcp.profills.com/mcp`.

O CRM aceita o login do Claude e do ChatGPT, cada um no app ou no terminal. Vá direto ao seu caso:

- [Claude](#claude): [no claude.ai ou no Claude Desktop](#no-claudeai-ou-no-claude-desktop) e [no Claude Code](#no-claude-code).
- [ChatGPT](#chatgpt): [no Codex](#no-codex) e [no app do ChatGPT](#no-app-do-chatgpt).

Em todos os casos, duas etapas são só suas. Você entra no CRM com a sua conta e clica em permitir na tela de consentimento. A senha nunca passa pelo agente. Outro app recebe uma página do CRM com "Não foi possível conectar".

## Claude

### No claude.ai ou no Claude Desktop

O conector adicionado na conta aparece no claude.ai e no Claude Desktop. No plano Free, a conta tem no máximo 1 conector personalizado. No Pro e no Max, você mesmo adiciona. Os menus ficam em inglês.

1. Abra **Customize** e depois **Connectors**.
2. Clique em **+ Add** e depois em **Add custom connector**.
3. Dê um nome, como `CRM Profills`, preencha a URL `https://mcp.profills.com/mcp` e clique em **Continue**.
4. O Claude mostra a autenticação que detectou no servidor. Não mude nada e clique em **Continue**.
5. Em **Authentication**, escolha **Sign in now** ou **Sign in when needed**. Não escolha **No sign in**, porque sem login o servidor recusa toda chamada.
6. Em **OAuth client**, escolha **Use Claude's published identity (Recommended)**. As outras duas opções não funcionam.
7. Deixe **Request headers** vazio e clique em **Add**.
8. Entre com a sua conta do CRM e clique em permitir. Se o navegador não abrir sozinho, clique em **Connect** no conector, na lista de **Connectors**. Se o botão não aparecer, remova o conector e adicione de novo.

No Team, só o Owner e o Primary Owner adicionam o conector, em **Organization settings**, **Connectors**, **Add**, **Custom** e **Web**. No Enterprise, também adiciona quem tem o papel com "Manage access to Libraries". Quem adiciona escolhe as opções dos passos 5 a 7. Depois, cada membro abre **Customize** e **Connectors**, acha o conector com o rótulo **Custom** e clica em **Connect**.

### No Claude Code

1. Veja se o servidor já está configurado:

   ```bash
   claude mcp list
   ```

   Com `crm-profills` em `✔ Connected`, o conector já funciona. Pule para o passo 4. Com `! Needs authentication`, falta só o login. Pule para o passo 3, porque um segundo `add` falha com "already exists". Com outra URL, remova antes com `claude mcp remove crm-profills`.

2. Adicione o servidor para todos os projetos:

   ```bash
   claude mcp add --transport http --scope user crm-profills https://mcp.profills.com/mcp
   ```

   O escopo `user` deixa o CRM disponível em qualquer pasta. `claude mcp get crm-profills` mostra a URL gravada.

3. Faça o login. Dentro do Claude Code, digite `/mcp`, escolha `crm-profills` e siga o navegador. No terminal, o comando é `claude mcp login crm-profills`. A tela de consentimento mostra um aviso extra: "O app roda no seu computador (...). Só permita se foi você quem abriu esta conexão agora." Se foi você, clique em permitir.

   Sem navegador na máquina, como numa sessão SSH, rode `claude mcp login --no-browser crm-profills` num terminal interativo (`ssh -t`). Abra a URL impressa no seu computador, faça o login e cole no terminal a URL inteira da barra de endereço.

4. Confira com `claude mcp list`. O servidor aparece com `✔ Connected`. Se uma sessão aberta antes não mostrar os tools do CRM, abra uma sessão nova.

O Claude Code renova o login sozinho. Para entrar de novo, use **Re-authenticate** no `/mcp` ou `claude mcp login crm-profills`. Para desconectar, use **Clear authentication** ou `claude mcp logout crm-profills`. O modo `claude -p` não faz o login, então faça o login antes numa sessão interativa.

## ChatGPT

### No Codex

1. Veja se o servidor já está configurado:

   ```bash
   codex mcp list
   ```

   Com `crm-profills` na URL `https://mcp.profills.com/mcp` e sem `Not logged in`, o conector já funciona. Pule para o passo 4. Com `Not logged in`, pule para o passo 3. Com outra URL, remova antes com `codex mcp remove crm-profills`.

2. Adicione o servidor num terminal interativo:

   ```bash
   codex mcp add crm-profills --url https://mcp.profills.com/mcp
   ```

   O `add` grava o servidor em `~/.codex/config.toml` e já abre o login no navegador. Entre com a sua conta do CRM e clique em permitir. O terminal mostra `Successfully logged in.`. Pule para o passo 4.

   Sem navegador na máquina, o `add` imprime a URL de login e fica esperando. Interrompa com Ctrl+C, porque o servidor já ficou gravado, e siga no passo 3.

3. Faça o login com `codex mcp login crm-profills`. Sem navegador, use `codex mcp login --no-browser crm-profills`, abra a URL impressa no seu computador, faça o login e cole no terminal a URL inteira da barra de endereço, que começa com `http://127.0.0.1`.

4. Confira com `codex mcp list`. O servidor aparece sem `Not logged in`.

Para entrar de novo, rode `codex mcp login crm-profills`. Para desconectar, rode `codex mcp logout crm-profills`.

### No app do ChatGPT

1. Abra **Settings**, **Apps**, **Advanced settings** e ligue **Developer mode**.
2. Clique em **Create app**, ao lado de **Advanced settings**. Preencha a URL `https://mcp.profills.com/mcp` e escolha autenticação OAuth.
3. O app novo aparece em **Drafts**. Conecte, entre com a sua conta do CRM e clique em permitir.

O developer mode existe na web, nos planos Pro, Plus, Business, Enterprise e Education. No Business e no Enterprise, o admin liga o developer mode no painel de administração. Se o login falhar, avise o administrador do CRM e conte em que passo parou.

## Confira se o servidor responde

Este passo é para o agente, antes de configurar o conector. Os dois comandos respondem sem login.

```bash
curl -s https://mcp.profills.com/.well-known/oauth-protected-resource/mcp
```

A resposta é um JSON com `"resource": "https://mcp.profills.com/mcp"` e `"resource_name": "CRM Profills"`.

```bash
curl -s https://www.crmprofills.com.br/.well-known/oauth-authorization-server
```

A resposta é um JSON com `"client_id_metadata_document_supported": true`.

## O que a tela de consentimento mostra

A tela tem o título "Conectar" seguido do nome do app, e o texto "quer acessar o CRM em seu nome.". Ela lista o app, o endereço para onde o login volta e a sua conta. O bloco "O que o acesso permite" diz duas coisas:

- "Ler clientes, propostas, agenda e Desempenho que você já vê no CRM."
- "Registrar, editar e excluir os seus atendimentos e compromissos, com as regras da tela."

Com o mesmo acesso, o agente também cria e edita contatos e manda propostas para aprovação ou marca a proposta como enviada. Uma conta inativa ou em análise recebe uma página de erro no lugar da tela de consentimento.

## Como o login funciona

O servidor de login é o próprio CRM. O app abre a página de login do CRM, você entra e permite, e o CRM entrega ao app um acesso em seu nome. O conector confere esse acesso em cada pedido e nunca vê a sua senha.

| Item | Duração |
|---|---|
| Acesso | 1 hora, renovado sozinho pelo app |
| Renovação do acesso, contada do último uso | 30 dias |
| Conexão inteira, contada do primeiro login | 90 dias |
| Pedido de consentimento aberto | 10 minutos |

Depois de 90 dias, ou se a conexão cair antes disso, conecte de novo.
