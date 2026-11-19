---
title: "Lição 2 - Adicione avaliações em estrelas: uma vitória rápida"
description: "Inicie sua primeira sessão de agente no aplicativo GitHub Copilot, faça uma pequena alteração nos cartões dos jogos e integre-a como seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Agora que você explorou o workspace e experimentou um chat rápido, é hora de iniciar uma **sessão de agente** e fazer sua primeira alteração no projeto. Ela será pequena: os dados dos jogos já incluem uma avaliação em estrelas, mas os cartões da página inicial ainda não a mostram. Você pedirá ao agente que a exiba, revisará a alteração e fará merge dela como seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma sessão de agente e aprender como ela é estruturada.
- pedir ao agente para fazer uma pequena alteração focada no projeto.
- revisar a alteração na visualização de diff do workspace.
- executar o aplicativo localmente para confirmar a alteração no navegador.
- abrir e fazer merge do seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação em estrelas, que já aparece na página de detalhes do jogo. Mas os cartões da página inicial mostram apenas o título, a categoria, a editora e a descrição. Como aquecimento, você pedirá ao agente para exibir a avaliação existente em cada cartão: uma alteração pequena e independente, perfeita para sua primeira sessão.

## Anatomia de uma sessão

Uma **sessão** é uma conversa com um agente executada em seu próprio workspace isolado. Cada sessão tem um **worktree Git e uma branch dedicados**, o que permite executar várias sessões ao mesmo tempo — uma adicionando uma funcionalidade, outra corrigindo um bug — sem que as alterações entrem em conflito. As sessões aparecem agrupadas por repositório na barra lateral; selecione uma para alternar para ela.

Em uma sessão, você verá três coisas: a **conversa** com o agente, a **atividade das ferramentas** usadas para explorar e editar arquivos e a lista de **arquivos alterados** com seus diffs.

## Inicie uma sessão e solicite a alteração

Vamos iniciar uma nova sessão para explorar o projeto e implementar a funcionalidade. Durante a [configuração do aplicativo][prior-lesson], você adicionou o projeto a partir do repositório do GitHub. Agora, criaremos uma nova sessão para esse repositório e solicitaremos a alteração.

1. Volte ao aplicativo GitHub Copilot ou abra-o.
2. Selecione **+** na parte superior da barra lateral para iniciar uma sessão.
3. Selecione `tailspin-toys` como repositório.
4. Escolha uma **new working tree** e o modo **Interactive** abaixo da caixa de prompt. Use o seguinte prompt para solicitar a alteração:

    ```plaintext
    Mostre a avaliação starRating de cada jogo em uma escala de 5 nos cartões da página de listagem. Se a avaliação for null, mostre "No rating yet". Mantenha o layout dos cartões, adicione testes e execute as verificações relevantes.
    ```

5. Pressione <kbd>Enter</kbd> para enviar o prompt ao Copilot.

O aplicativo Copilot começa criando um novo worktree, uma cópia isolada do projeto. Depois, explora o projeto, localiza os arquivos necessários para adicionar a funcionalidade e cria o código correspondente. Você acaba de adicionar uma funcionalidade com o aplicativo Copilot!

## Revise o diff

Todas as alterações geradas por IA devem ser revisadas antes do merge, mesmo as pequenas. Vamos explorar as alterações diretamente no aplicativo Copilot.

1. No canto superior direito do aplicativo, selecione **Toggle review panel**. No painel que abrir, selecione **Changes** para ver os arquivos alterados e seus diffs.

    ![Barra de ferramentas superior do aplicativo GitHub Copilot com uma seta indicando o botão Toggle review panel à direita de Create PR](../../../_images/app-2-review-panel.png)

2. Você deverá ver código adicionado a `GameCard.astro`, o arquivo principal usado para exibir os detalhes dos jogos. Ele deverá ser parecido com este: um pequeno bloco que mostra a avaliação quando disponível e usa `No rating yet` quando `starRating` é `null`:

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, não determinístico, então o código exato pode variar. Ainda assim, deve ser relativamente parecido com o exemplo.

## Verifique as alterações

Revise os resultados das verificações automatizadas do agente antes de abrir o navegador. Confirme que os testes cobrem uma avaliação numérica em `starRating` e o estado alternativo `null`. A ausência de um pré-requisito ou uma verificação ignorada não significa que ela foi aprovada; revise qualquer solicitação de instalação antes de aprová-la.

Não devemos apenas ler o código e presumir que funciona. Vamos pedir ao Copilot que abra o site para que possamos examinar a interface atualizada. Podemos pedir que ele inicie o site e o abra em um canvas de navegador.

> [!TIP]
> Um canvas é um widget interativo disponível dentro do aplicativo Copilot. Você explorará canvases personalizados e até criará um mais adiante; por enquanto, usaremos o canvas de navegador integrado.

1. Use o seguinte prompt para pedir ao Copilot que inicie o aplicativo e abra a página no canvas de navegador:

    ```plaintext
    Inicie o aplicativo e abra-o no canvas de navegador.
    ```

2. Em alguns instantes, o aplicativo será iniciado e uma janela do navegador será aberta dentro do aplicativo Copilot.
3. Confirme que os cartões dos jogos avaliados mostram a nota em uma escala de cinco estrelas.
4. Quando terminar, peça ao Copilot que pare o servidor de desenvolvimento iniciado para esta sessão e feche o canvas de navegador:

    ```plaintext
    Pare o servidor de desenvolvimento e feche o canvas de navegador.
    ```

## Abra e faça merge do seu primeiro pull request

Você criou a funcionalidade! Agora é hora de criar um pull request (PR) para integrar o novo código à base de código existente.

1. Selecione **Create PR** no canto superior direito.
2. Se solicitado, selecione **Sign in with your browser** e siga as instruções de autenticação.
3. Se o Copilot informar que a política do repositório exige uma issue correspondente antes da criação do PR, confirme que ele deve seguir esse processo.
4. O Copilot começará a criar o PR.
5. Selecione o balão **PR** logo acima do chat para abrir o PR no painel de revisão. Revise-o conforme necessário.
6. Quando estiver tudo pronto, selecione **Ready to merge**.
7. Na nova janela, selecione **Merge pull request** para fazer merge do seu pull request.

## Resumo e próximas etapas

Parabéns! Você entregou sua primeira alteração usando o aplicativo GitHub Copilot! Especificamente, você:

- iniciou uma sessão de agente e aprendeu como as sessões são estruturadas.
- orientou o agente para fazer uma pequena alteração focada nos cartões dos jogos.
- revisou a alteração na visualização de diff do workspace.
- executou o aplicativo localmente para confirmar a avaliação em estrelas no navegador.
- abriu o PR 1, revisou as verificações e fez merge dele explicitamente.

Em seguida, você [começará pela issue de filtragem e usará os modos Plan e Autopilot][next-lesson] para criar uma funcionalidade maior.

## Recursos

- [Trabalhe com sessões de agente no aplicativo GitHub Copilot][agent-sessions]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Gerencie issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#instale-e-configure-o-aplicativo-github-copilot
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
