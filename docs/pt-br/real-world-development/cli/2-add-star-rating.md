---
title: "Lição 2 - Adicione avaliações em estrelas: uma vitória rápida"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cartões dos jogos, revise-a em um navegador encaminhado e faça merge dela como seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e experimentou uma conversa, é hora de fazer sua primeira alteração no projeto. Ela será pequena: os dados dos jogos já incluem uma avaliação em estrelas, mas os cartões da página inicial ainda não a mostram. Você pedirá ao agente que a exiba, revisará a alteração e fará merge dela como seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma conversa focada em uma branch de funcionalidade.
- pedir ao agente para fazer uma pequena alteração no projeto.
- revisar a alteração com `/diff`.
- executar o aplicativo para confirmar a alteração em um navegador encaminhado.
- abrir e fazer merge do seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação em estrelas, que já aparece na página de detalhes do jogo. Mas os cartões da página inicial mostram apenas o título, a categoria, a editora e a descrição. Como aquecimento, você pedirá ao agente para exibir a avaliação existente em cada cartão: uma alteração pequena e independente, perfeita para sua primeira sessão.

## Anatomia de uma conversa

Uma **conversa** é onde você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal do CLI usa o repositório e a branch atualmente selecionados no terminal, em vez de criar um worktree dedicado. As conversas salvas permitem retomar a mesma discussão mais tarde, enquanto os arquivos e a branch continuam sendo o estado comum do Git no disco.

Em uma conversa, você verá três coisas: seus prompts e as respostas do agente, a atividade das ferramentas que o agente usa para explorar e editar arquivos e as alterações que você pode inspecionar com `/diff`.

## Inicie uma conversa e solicite a alteração

Vamos iniciar uma nova conversa para começar a implementar a funcionalidade.

1. Volte ao seu Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se ainda não estiver aberto, inicie o Copilot com o seguinte comando:

   ```bash
   copilot --yolo
   ```

4. Confirme que uma nova sessão foi iniciada usando o comando slash `/new` e pressionando <kbd>Enter</kbd>.
5. Use o seguinte prompt para solicitar a alteração:

   ```plaintext
   Mostre a avaliação starRating de cada jogo em uma escala de 5 nos cartões da página de listagem. Se a avaliação for null, mostre "No rating yet". Mantenha o layout dos cartões e adicione testes; depois, execute as verificações relevantes.
   ```

O Copilot explora o projeto, localiza os arquivos usados para exibir os detalhes dos jogos e cria o código necessário. Você acaba de adicionar uma funcionalidade com o Copilot CLI!

## Revise o diff

Todas as alterações geradas por IA devem ser revisadas antes do merge, mesmo as pequenas. Vamos explorar as alterações diretamente no Copilot CLI.

1. Digite `/diff` e inspecione cada arquivo alterado.
2. Confirme que o cartão do jogo mostra a avaliação numérica quando ela está disponível e `No rating yet` quando `starRating` é `null`.
3. Confirme que os testes cobrem os dois estados.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija as falhas.
5. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, não determinístico, então seu código exato pode variar. Revise o comportamento em vez de esperar uma implementação específica.

## Verifique as alterações

Não devemos apenas ler o código e presumir que funciona. Vamos pedir ao Copilot para iniciar o site para que possamos examinar a interface atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Inicie o aplicativo para que eu possa inspecionar a alteração das avaliações em estrelas no navegador. Informe a URL e mantenha o servidor em execução.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme que os cartões dos jogos mostram as avaliações em uma escala de cinco estrelas.
4. Volte ao Copilot e peça que pare o servidor iniciado:

   ```plaintext
   Pare o servidor de desenvolvimento que você iniciou.
   ```

## Abra e faça merge do seu primeiro pull request

Você criou a funcionalidade! Agora é hora de abrir um pull request (PR) para integrar o novo código ao projeto.

1. Peça ao agente padrão para fazer commit da alteração:

   ```plaintext
   Faça commit das alterações revisadas das avaliações em estrelas com uma mensagem de commit adequada.
   ```

2. Digite `/pr create`. O Copilot CLI envia o commit existente ao criar o PR e exibe a URL dele.
3. Abra o PR mantendo <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionada e selecionando a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando tudo estiver pronto, selecione **Merge pull request** e confirme o merge.
6. Volte ao seu Codespace e saia do Copilot CLI com `/exit`.
7. Atualize sua `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximas etapas

Parabéns! Você entregou sua primeira alteração usando o GitHub Copilot CLI! Especificamente, você:

- iniciou uma conversa focada do Copilot em uma branch de funcionalidade.
- orientou o agente para fazer uma pequena alteração nos cartões dos jogos.
- revisou a alteração com `/diff`.
- executou o aplicativo para confirmar a avaliação em estrelas em um navegador encaminhado.
- abriu e fez merge do seu primeiro pull request.

Em seguida, você [começará pela issue de filtragem e usará os modos Plan e Autopilot][next-lesson] para criar uma funcionalidade maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
