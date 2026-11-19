---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Explore os modos de agente: use Plan para definir uma abordagem, Autopilot para implementar a filtragem a partir de uma issue e Interactive para revisar e verificar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Começamos adicionando uma pequena funcionalidade ao projeto. Mas alterações maiores exigem um processo mais robusto. Felizmente, o aplicativo GitHub Copilot foi criado para trabalhar com o fluxo de trabalho existente da organização e ajudar a desenvolver as coisas certas, da maneira certa. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento orientado por agentes, começando com uma issue para gerar uma nova funcionalidade, garantindo que o código seja válido, que ela se comporte como esperado e que, por fim, seja integrada ao projeto.

> [!NOTE]
> Você usará a mesma sessão ao longo do fluxo de trabalho da funcionalidade. Normalmente, você usaria sessões ou PRs diferentes para vários tipos de arquivo, mas vamos simplificar para focar nos conceitos principais.

Nesta lição, você vai:

- iniciar uma nova sessão de agente a partir de uma issue do GitHub.
- definir os requisitos no modo **Plan**.
- implementar a nova funcionalidade usando o modo **Autopilot**.
- revisar o código.
- validar a funcionalidade manualmente em um canvas de navegador.

Ao continuar o trabalho nessa funcionalidade, você atualizará as instruções do repositório, personalizará a skill `quality-checks` existente, adicionará validação com MCP, criará um agente de QA e abrirá o PR da funcionalidade.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve a funcionalidade, mas detalhes como combinar categorias precisam ser acordados antes de escrever o código. Você usará o modo Plan para resolver essas decisões e, depois, autorizará uma implementação com escopo definido no Autopilot.

## Contexto

Adicionar agentes de programação de IA ao fluxo de desenvolvimento não muda os fundamentos. Na verdade, eles se tornam ainda mais importantes! A maioria das pessoas desenvolvedoras segue um fluxo parecido:

1. Abrir uma issue registrada com os detalhes do trabalho.
2. Criar um plano do que será desenvolvido.
3. Criar e revisar o código.
4. Executar os testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois que o código for revisado e o processo de integração contínua for concluído com êxito, fazer merge do código.

> [!NOTE]
> Os detalhes exatos variam de acordo com sua equipe e organização, mas a maioria dos fluxos segue alguma variação do processo acima.

Ao seguir essa abordagem padrão, você garante que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de validação que o código escrito manualmente.

## Modos de sessão

O **modo da sessão** controla o nível de autonomia do agente. Você pode defini-lo no menu suspenso abaixo do campo de prompt e alterá-lo quando quiser:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua resposta antes de continuar.
- **Plan**: o agente cria primeiro um plano. Você o revisa e aprova antes de o agente executá-lo.
- **Autopilot**: o agente trabalha de forma totalmente autônoma: escreve código, executa testes e itera sem aguardar respostas.

Comece no modo Plan, revise o plano e depois use o Autopilot para implementá-lo.

## Inicie uma sessão a partir da issue

Confirme que o PR das avaliações em estrelas foi integrado e que sua `main` local está atualizada antes de começar.

1. Em **Sessions** na barra lateral, selecione a pasta do repositório Tailspin Toys e, na navegação superior do repositório, selecione **Issues**. Abra **Allow users to filter games by category and publisher**. Essa issue já foi criada pelo modelo; não é necessário criar outra.
2. Selecione **New session** e escolha uma **new working tree** baseada em `main` atualizada.

    ![Visualização de uma issue no aplicativo GitHub Copilot com uma seta apontando para o botão New session](../../../_images/app-new-session-from-issue.png)

3. Confirme que a issue foi anexada à sessão e selecione **Plan** no seletor de modo.

> [!NOTE]
> Se o Node.js ou o npm não forem encontrados, peça ao agente para executar `node --version` e `npm --version`. Se os comandos funcionarem no terminal, mas não na sessão, verifique a configuração do PATH ou do gerenciador de versões do aplicativo e feche-o completamente antes de reabri-lo. Tente novamente a operação e as verificações que falharam; não reinstale o npm apenas porque o agente não o encontrou.

## Planeje a funcionalidade de filtragem

O planejamento permite revisar a abordagem antes de o Copilot escrever código. Como você começou pela issue, o Copilot já tem a solicitação da funcionalidade no contexto. Envie:

```plaintext
Implemente esta funcionalidade.
```

Responda às perguntas do Copilot e compare o plano com os critérios de aceitação da issue. Confirme que ele abrange a filtragem por categoria e editora, controles acessíveis, alterações de acesso a dados e testes. Converse sobre qualquer comportamento pouco claro, como combinar várias categorias ou o que acontece quando nenhum jogo corresponde.

O plano deve incluir lint, testes unitários, testes de ponta a ponta e verificação de tipos com as ferramentas existentes no projeto. Mantenha o escopo em implementar e testar a filtragem; você criará o PR depois de concluir o fluxo de qualidade. Peça alterações ao plano antes de aprová-lo e guarde a URL da issue e quaisquer esclarecimentos acordados para validá-los mais tarde.

## Aprove o Autopilot explicitamente

Quando estiver satisfeito com o plano, selecione **Approve and implement this plan** ou a opção equivalente na sua versão. Confira o modo ativo em vez de presumir que a aprovação o altera. Confirme que o indicador de modo mostra **Autopilot**.

O Copilot começará a implementar a funcionalidade! Você verá que ele segue o plano estabelecido, gera código e executa testes, iterando durante o processo.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano antes. Se o Copilot informar que faltam dependências ou que há conflito de portas, resolva o problema de configuração antes de considerar as verificações concluídas. Pare somente os servidores que você iniciou.

## Revise e verifique a implementação

Depois que o código for gerado, ele precisa ser revisado antes do merge, como qualquer outro código. Vamos revisar o código e executar o site para confirmar que tudo funciona.

1. Abra **Changes** e inspecione a implementação da filtragem e os testes.
2. Compare o resultado com a issue e os esclarecimentos aprovados, incluindo combinações de categorias e editoras. Confirme que as alterações seguem as instruções existentes do repositório.
3. Inspecione os resultados de lint, testes unitários, testes de ponta a ponta e verificação de tipos. Uma verificação ignorada não conta como aprovada.
4. Corrija falhas e execute novamente as verificações afetadas antes de aceitar a implementação. A configuração de testes de ponta a ponta do Playwright compila e serve uma visualização e pode reutilizar um servidor local; confirme que o servidor testado pertence a este worktree, e não a uma lição anterior.

## Explore a nova funcionalidade

Tudo certo com o código — mas será que ele funciona? Vamos iniciar o aplicativo como antes e abrir o site em um canvas de navegador.

1. Use o seguinte prompt para pedir ao Copilot que inicie o aplicativo e abra a página no canvas de navegador:

    ```plaintext
    Inicie o aplicativo e abra-o no canvas de navegador.
    ```

2. Em alguns instantes, o aplicativo será iniciado e uma janela do navegador será aberta dentro do aplicativo Copilot.
3. Selecione uma categoria e confirme que os resultados correspondem a ela. Limpe o filtro e selecione uma editora; confirme que os resultados correspondem a essa editora.
4. Combine filtros de categoria e editora e compare os resultados com a issue e o plano aprovado. Se houver suporte a várias categorias, verifique o comportamento combinado acordado.
5. Teste uma combinação sem jogos correspondentes e confira o estado vazio. Limpe ou redefina os filtros e confirme que a lista completa volta a aparecer.
6. Quando terminar, peça ao Copilot que pare o servidor de desenvolvimento iniciado para esta sessão:

    ```plaintext
    Pare o servidor de desenvolvimento e feche o canvas de navegador.
    ```

## Mantenha esta sessão para as próximas lições

Ainda não crie nem faça merge do PR da filtragem. Mantenha esta sessão, o worktree e a branch para as Lições 4 a 8. Em seguida, você aplicará instruções personalizadas ao código de filtragem e adicionará melhorias à skill e ao agente de QA antes de criar e fazer merge do PR combinado na Lição 8. Você pode deixar a sessão ociosa depois de parar o servidor de desenvolvimento; retome a mesma sessão na próxima lição.

## Resumo e próximas etapas

Você usou diferentes modos de agente para desenvolver e revisar uma funcionalidade. Nesta lição, você:

- iniciou uma nova sessão de agente a partir de uma issue do GitHub.
- definiu os requisitos no modo **Plan**.
- implementou a nova funcionalidade usando o modo **Autopilot**.
- revisou o código.
- validou a funcionalidade manualmente em um canvas de navegador.

Em seguida, vamos explorar melhor como o código é gerado e garantir que siga as práticas documentadas usando [instruções personalizadas][next-lesson].

## Recursos

- [Trabalhe com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
