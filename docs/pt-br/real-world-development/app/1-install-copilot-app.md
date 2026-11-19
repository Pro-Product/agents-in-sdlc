---
title: "Lição 1 - Instale o aplicativo GitHub Copilot"
description: "Instale o aplicativo GitHub Copilot, conecte o repositório criado a partir do modelo, conheça o workspace e experimente um chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O **[aplicativo GitHub Copilot][about-copilot-app]** é um aplicativo para desktop voltado ao desenvolvimento orientado por agentes. Ele é baseado no GitHub Copilot CLI e se integra nativamente ao GitHub, para que seus repositórios, branches e pipelines de CI funcionem imediatamente. O aplicativo foi criado para fluxos de trabalho em que você orienta vários agentes em paralelo — cada um em seu próprio workspace isolado — em vez de executar todo o trabalho por conta própria e automatizar tarefas repetitivas. Com o Node.js instalado e sua cópia do projeto pronta, o próximo passo é instalar o aplicativo e conectar esse repositório.

Nesta lição, você vai:

- instalar o aplicativo GitHub Copilot e entrar na sua conta.
- adicionar seu projeto ao aplicativo a partir do repositório do GitHub.
- conhecer o workspace, incluindo o backlog inicial criado pelo modelo.
- experimentar um chat rápido para saber mais sobre o aplicativo.

## Cenário

Sua equipe está adotando agentes de IA para avançar em um backlog crescente. O aplicativo Copilot oferece um único lugar para orientar esse trabalho: selecionar issues, executar agentes, revisar alterações e fazer merge de pull requests. Nesta lição, você instalará e conectará o aplicativo e aprenderá a iniciar uma conversa sobre o projeto.

> [!NOTE]
> É necessário ter um plano elegível do Copilot: Copilot Free, Copilot Student ou qualquer plano pago (Pro, Pro+, Business ou Enterprise). Se você usa o Copilot Business ou o Copilot Enterprise, o administrador precisa manter habilitada a política **GitHub Copilot app**.

## Instale e configure o aplicativo GitHub Copilot

Para usar o aplicativo GitHub Copilot, primeiro é necessário instalá-lo. Há versões para Windows, macOS e Linux. Vamos instalar o aplicativo, autenticar e adicionar o repositório Tailspin Toys.

1. Em um navegador, abra a [página inicial do aplicativo GitHub Copilot][download-app].
2. Baixe o aplicativo para sua plataforma e instale-o seguindo as instruções da página.
3. Abra o aplicativo depois da instalação.
4. Selecione **Sign in to GitHub** e siga as instruções para entrar com uma conta do GitHub.com ou do GHE.com. Se usar o GitHub Enterprise Cloud com residência de dados, selecione **GitHub Enterprise Cloud** e informe seu host `*.ghe.com` quando solicitado.

   Se usar o GitHub Enterprise Server, conclua primeiro a integração com sua conta na nuvem. Em seguida, abra **Settings**, selecione **Accounts**, selecione **Add account** e escolha **GitHub Enterprise Server**. Siga as instruções para autorizar o aplicativo no navegador, conforme descrito no [guia de entrada do Enterprise Server][ghes-sign-in].

5. Se a integração oferecer a opção de selecionar um repositório, selecione o repositório Tailspin Toys criado a partir do modelo. Você também pode pular essa etapa e adicionar o projeto depois.
6. Escolha um tema quando solicitado e conclua a integração.
7. Procure seu repositório na seção **Sessions** da barra lateral. Se ele já estiver listado, não é necessário adicioná-lo novamente.
8. Se o repositório ainda não estiver conectado, selecione **+** ao lado de **Sessions** para criar uma sessão e, em seguida, selecione **Add GitHub repository**. Use o proprietário e o nome do repositório anotados na lição de pré-requisitos.
9. Confirme que o repositório aparece na barra lateral. No layout usado aqui, as pastas de repositórios, como `tailspin-toys`, ficam agrupadas em **Sessions**, com sessões individuais abaixo de cada repositório.

## Conheça o workspace

Com o projeto conectado, conheça as áreas da barra lateral. O aplicativo organiza o trabalho em algumas seções:

- **New** — permite iniciar uma nova sessão de chat com o Copilot.
- **Pull requests** — seus pull requests, exibidos pela integração nativa com o GitHub do aplicativo. Nessa seção, você pode navegar e filtrar pull requests, conferir o estado da CI e revisá-los sem sair do aplicativo.
- **Issues** — suas issues, também exibidas pela integração. Você pode navegar e filtrar as issues e iniciar uma sessão a partir de uma delas.
- **Automations** — tarefas de agente salvas que são executadas conforme um agendamento ou sob demanda. Elas são úteis para gerenciar listas de tarefas, manutenção regular do projeto ou outras tarefas repetitivas. O encerramento apresenta essa seção como uma próxima etapa, não como outro exercício do workshop.
- **Customize** — adicione recursos ao aplicativo Copilot, como servidores MCP, plug-ins, skills e outros componentes. Você usará esta seção para configurar o Playwright MCP.
- **Chats** — conversas rápidas para perguntas e ideias que não precisam de uma branch nem de um workspace próprio. Essa seção aparece depois que você inicia o primeiro chat. Se não estiver visível, selecione **+** ao lado de **Sessions** para iniciar um chat.
- **Sessions** — agrupa as pastas de repositório e as sessões de agente. Selecione o nome de um repositório, como `tailspin-toys`, para abrir a visualização dele, ou selecione uma sessão abaixo do repositório para voltar à conversa. Cada sessão é executada em seu próprio workspace isolado, então você pode executar várias ao mesmo tempo sem que as alterações entrem em conflito. Você iniciará sua primeira sessão ao adicionar avaliações em estrelas.

Durante o workshop, você explorará o workspace.

> [!TIP]
> Em caso de dúvida, pergunte ao Copilot! Se não souber como fazer algo ou se não tiver certeza de que é possível, peça orientação ao Copilot.

### Encontre o backlog inicial

É provável que todo projeto tenha um backlog, e a Tailspin Toys não é exceção. Vamos explorar o backlog criado quando você configurou o projeto a partir do modelo.

1. Em **Sessions** na barra lateral, selecione a pasta do repositório Tailspin Toys, não uma sessão individual dentro dela. Depois, selecione **Issues** na navegação superior do repositório.
2. Localize as seguintes issues pelo título, sem presumir seus números:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecione uma issue para ler os detalhes. Cada issue também pode iniciar uma sessão de agente. Você começará pela issue de filtragem depois de fazer uma primeira alteração rápida.

> [!NOTE]
> As listas de **Issues** e **Pull requests** são filtradas automaticamente para exibir somente itens dos repositórios adicionados ao aplicativo Copilot. Para ver itens de outros repositórios, adicione-os ao aplicativo.

## Experimente um chat rápido

Uma ótima maneira de conhecer o aplicativo é usá-lo para aprender sobre o *próprio aplicativo* — e um **chat rápido** é perfeito para isso. Com chats rápidos, você pode fazer perguntas ou pensar em ideias sem criar uma branch nem um worktree. São ideais para perguntas rápidas e descartáveis, sem precisar de uma sessão.

1. Na barra lateral, selecione **+** ao lado de **Chats** para abrir um chat novo. Se **Chats** ainda não estiver visível, selecione **+** ao lado de **Sessions** para iniciar seu primeiro chat; **Chats** aparecerá depois.
2. Pergunte ao aplicativo como funcionam as sessões:

   ```plaintext
   Como o aplicativo GitHub Copilot usa worktrees?
   ```

3. Leia a resposta na visualização da conversa. Você verá que cada sessão é executada em seu próprio worktree Git isolado — esse detalhe permite executar vários agentes em paralelo sem conflito entre as alterações. Você pode continuar a conversa ou iniciar um novo chat quando quiser.

## Resumo e próximas etapas

Parabéns! Você instalou o aplicativo GitHub Copilot, conectou o projeto e explorou o workspace. Você aprendeu a:

- instalar o aplicativo e entrar no GitHub.
- adicionar um projeto a partir do repositório dele no GitHub.
- conhecer o workspace e localizar o backlog inicial em **Issues**.
- usar um chat rápido para fazer uma pergunta simples.

Em seguida, você vai [iniciar sua primeira sessão de agente][next-lesson] e usá-la para mostrar avaliações em estrelas nos cartões dos jogos.

## Recursos

- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Primeiros passos com o aplicativo GitHub Copilot][getting-started]
- [Trabalhe com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
