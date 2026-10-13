# Exemplos e perguntas frequentes

Cada exemplo mostra o pedido do vendedor e a sequência de tools que o agente chama. Os parâmetros estão em [tools.md](tools.md), e as regras que o agente segue estão no [guia para agentes](guia-para-agentes.md).

- [Exemplos de uso](#exemplos-de-uso)
  1. [Como está a Silva Embalagens?](#1-como-está-a-silva-embalagens)
  2. [Liguei para um telefone e ficou algo combinado](#2-liguei-agora-para-o-41-99876-5432-ficaram-de-mandar-o-pedido-semana-que-vem)
  3. [Marca uma visita](#3-marca-uma-visita-na-silva-embalagens-sexta-1610-às-14h)
  4. [Muda a visita de horário](#4-muda-a-visita-de-sexta-para-as-16h)
  5. [Manda a 72/26 para aprovação](#5-manda-a-7226-para-aprovação)
  6. [Propostas para vencer](#6-quais-propostas-minhas-estão-para-vencer)
  7. [Carteira sem visita há 90 dias](#7-quem-da-minha-carteira-em-curitiba-está-sem-visita-há-90-dias)
  8. [Semana da praça](#8-como-foi-a-semana-da-praça)
  9. [O que tenho amanhã?](#9-o-que-tenho-amanhã)
  10. [Registra a ligação para o comprador](#10-registra-que-liguei-para-o-comprador-da-padaria-silva)
- [Perguntas frequentes](#perguntas-frequentes)

## Exemplos de uso

### 1. "Como está a Silva Embalagens?"

1. `buscar_clientes` com `q` igual a `Silva Embalagens`.
2. Se vierem dois clientes ou mais, o agente mostra os nomes e as cidades e pede ao vendedor para escolher.
3. `ver_cliente` com o `cliente_id` escolhido.
4. Para cada proposta em `cliente_aceitou`, `ver_proposta` com o `proposta_id` dela, para ler `forma_escolhida.valor_final`.

O agente resume dono, contatos, último atendimento, propostas ativas e próximos compromissos. O vendedor também pode pedir isso pelo prompt `resumo_do_cliente`. No Claude Code, o prompt recebe uma palavra só, então ele usa o CNPJ ou uma palavra do nome, como em [Prompts](tools.md#prompts).

### 2. "Liguei agora para o 41 99876-5432, ficaram de mandar o pedido semana que vem"

1. `buscar_clientes` com `q` igual a `41998765432`.
2. O agente confirma o nome do cliente com o vendedor, porque o telefone pode estar em mais de um cliente.
3. `registrar_atendimento` com o `cliente_id` confirmado, `assunto` igual a `Ligação sobre o pedido da próxima semana` e `status` igual a `Pendente`.

O agente diz em qual cliente registrou.

### 3. "Marca uma visita na Silva Embalagens sexta, 16/10, às 14h"

1. `buscar_clientes` com `q` igual a `Silva Embalagens`.
2. `criar_compromisso` com `titulo` igual a `Visita Silva Embalagens`, `inicio` igual a `16/10/2026 14:00` e `cliente_id`.

Sem `fim`, o compromisso dura 1 hora, e o agente avisa o vendedor. Se o dia da semana e a data não batem, o agente pergunta ao vendedor antes de criar.

### 4. "Muda a visita de sexta para as 16h"

1. `minha_agenda` com `de` e `ate` iguais a `16/10/2026`.
2. `editar_compromisso` com o `compromisso_id` e `inicio` igual a `16/10/2026 16:00`.

Com só `inicio`, o CRM desloca o fim junto e mantém a duração.

### 5. "Manda a 72/26 para aprovação"

1. `ver_proposta` com `codigo` igual a `72/26`.
2. O agente confere que `ativa` é 1 e que o `estagio` é `rascunho`. Se não for, ele diz o estágio ao vendedor e para.
3. O agente mostra código, cliente e estágio e pergunta se pode enviar.
4. Com o "sim" do vendedor, `ver_proposta` de novo com o mesmo `codigo`.
5. `enviar_proposta` com o `id` dessa última leitura em `proposta_id` e `destino` igual a `aguardando_aprovacao`.

Se a resposta começar com "Nada foi gravado" ou com "Esta versão da proposta não é a ativa", o agente segue a linha da tabela [Como reagir a cada erro](guia-para-agentes.md#como-reagir-a-cada-erro).

### 6. "Quais propostas minhas estão para vencer?"

1. `listar_propostas` com `escopo` igual a `minhas`, `estagio` igual a `enviada` e `limite` igual a 50.
2. Enquanto `mais` vier `true`, a mesma chamada com `antes` igual ao `id` do último item.

O agente conta os dias desde `data_envio` e separa as vencendo (28 ou 29 dias) das vencidas (30 dias ou mais).

### 7. "Quem da minha carteira em Curitiba está sem visita há 90 dias?"

1. `minha_carteira` com `sem_atendimento_ha_dias` igual a 90 e `cidade` igual a `Curitiba`.
2. Enquanto `mais` vier `true`, a mesma chamada com `cursor` igual ao `proximo_cursor`.

O agente lista os clientes do mais antigo para o mais recente. Quem tem `ultimo_atendimento_aproximado: true` aparece com "em tal dia ou antes". O agente avisa que a lista mede atendimento registrado, não visita na agenda.

### 8. "Como foi a semana da praça?"

1. `ver_desempenho` com `modo` igual a `semana`.
2. Se o vendedor pediu uma praça que não vê, o agente chama de novo sem `escopo` e oferece um dos `escopos_disponiveis`.

O agente mostra as métricas com o período anterior, a conversão e os `avisos` da resposta.

### 9. "O que tenho amanhã?"

1. O agente calcula a data de amanhã no horário de Brasília. Se não sabe a data de hoje, pergunta ao vendedor.
2. `minha_agenda` com `de` e `ate` iguais a essa data, como `10/10/2026`. O `ate` é inclusive, então a resposta cobre o dia inteiro.

O agente lista os compromissos por hora. Se o vendedor também quer as pendências, o agente chama `meus_atendimentos` sem `status`.

### 10. "Registra que liguei para o comprador da Padaria Silva"

1. `buscar_clientes` com `q` igual a `Padaria Silva`. Com dois clientes ou mais, o agente pede ao vendedor para escolher.
2. `ver_cliente` com o `cliente_id`, para achar em `contatos` quem tem o `cargo` de comprador.
3. `registrar_atendimento` com `cliente_id`, `assunto` igual a `Ligação com o comprador`, `status` igual a `Concluído` e o `contato_id` achado.

Se o vendedor contou algo combinado para depois, o `status` é `Pendente`. Sem comprador em `contatos`, ou com dois, o agente pergunta ao vendedor. O agente diz em qual cliente registrou.

## Perguntas frequentes

### O agente vê tudo do CRM?

Não. Ele vê o que você vê na tela do CRM. Quem não tem papel comercial recebe lista vazia em `buscar_clientes` e `minha_carteira`.

### O agente manda a proposta para o cliente?

Não. `enviar_proposta` só muda o estágio e grava a data de envio. Você manda o PDF ao cliente por fora.

### O agente pode excluir um cliente ou uma proposta?

Não. Ele exclui só atendimentos e compromissos, e só depois da sua confirmação.

### Como sei o que o agente gravou?

O histórico do cliente mostra a origem `agente` e o selo do app, como "via Claude".

### Por que a busca não acha o cliente pelo final do telefone?

A busca por telefone precisa de 8 dígitos ou mais. Final ou pedaço de telefone não funciona.

### Por que o agente pediu o ano da proposta?

O mesmo número pode existir em mais de um ano. Com o ano, como `72/26`, o CRM acha a proposta certa.

### Por que o atendimento ficou com a data de hoje?

`registrar_atendimento` grava a data e a hora do momento do registro. A data não muda depois, nem pela edição.

### Quanto tempo a conexão dura?

O app renova o acesso sozinho por até 90 dias. Depois, conecte de novo. Os prazos estão em [Como o login funciona](instalacao.md#como-o-login-funciona).

### Funciona no ChatGPT?

O CRM aceita o login do Codex e do app do ChatGPT. Os passos estão em [ChatGPT](instalacao.md#chatgpt).
