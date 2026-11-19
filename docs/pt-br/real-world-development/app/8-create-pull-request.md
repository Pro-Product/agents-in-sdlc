---
title: "Lição 8 - Crie e faça merge do PR da funcionalidade"
description: "Revise filtragem, instruções, atualização da skill, perfil de QA e testes em conjunto; depois, crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

A implementação da filtragem, as atualizações das instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora é hora de revisar tudo em conjunto e abrir um pull request. Você fez merge do pull request (PR) das avaliações em estrelas manualmente; desta vez, permitirá que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos a funcionalidade, as atualizações das instruções, a atualização da skill e o agente de QA em alguns PRs separados. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma sessão e branch, e incluiremos todo esse trabalho neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionar todo o PR da funcionalidade e as evidências de verificação.
- autorizar o Agent Merge somente depois da revisão e confirmar que o PR foi integrado.

## Cenário

Durante o fluxo de trabalho da filtragem, você usou o Copilot para planejar, implementar e verificar uma funcionalidade. Agora a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização do merge sob o controle da pessoa desenvolvedora.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho restante para integrar um pull request no aplicativo GitHub Copilot. Quando habilitado, a sessão do aplicativo lê seu pull request e resolve os impedimentos — corrige verificações de integração contínua (CI) com falha, responde aos comentários de revisão e faz rebase quando necessário — e faz o merge assim que o GitHub permite. O Agent Merge funciona em segundo plano, continua após reiniciar o aplicativo e desativa-se depois que o pull request é integrado.

Até agora, você mesmo selecionou **Merge pull request**. O Agent Merge pode assumir essa responsabilidade, mas ainda precisa de autorização explícita para editar código e fazer merge. Revise as ações permitidas e o trabalho antes de conceder permissão para fazer merge.

## Use o Agent Merge para gerenciar o PR

Com todo o código criado e revisado, vamos permitir que o Agent Merge gerencie o processo do PR.

1. Use o seletor de agente para escolher **Default agent**.
2. Selecione o menu suspenso ao lado de **Create PR**.
3. Selecione **Agent Merge**. O botão mudará para **Agent Merge**.
4. Selecione o botão **Agent Merge** para iniciar o processo. Escolher a opção no menu suspenso, por si só, não inicia o processo.

O processo do Agent Merge será iniciado. Ele vai:

- criar o pull request com título e descrição.
- se você tiver iniciado a sessão a partir de uma issue, incluir uma referência a ela no corpo da descrição.
- fazer rebase ou resolver possíveis conflitos de merge com a branch de destino.
- acompanhar o processo de CI para garantir que todas as verificações sejam aprovadas.
- monitorar o PR em busca de comentários de outras pessoas desenvolvedoras ou da revisão de código do Copilot e fazer atualizações para resolvê-los.
- opcionalmente, fazer merge automático do PR assim que tudo for concluído com êxito.

Vamos permitir que o Agent Merge também faça merge do PR depois que todas as verificações forem aprovadas.

5. Revise o diff do PR e as evidências de verificação antes de conceder permissão para fazer merge.
6. Selecione o menu suspenso ao lado de **Agent Merge**.
7. Confirme que há uma marca de seleção ao lado de **Merge pull request**.
8. Peça explicitamente ao agente para fazer merge:

    ```plaintext
    Faça merge do PR depois que todas as verificações obrigatórias e aprovações forem concluídas. Não ignore as proteções do repositório.
    ```

9. Acompanhe o progresso e resolva os impedimentos informados. Confirme que o estado do PR é **Merged** antes de continuar; habilitar a permissão de merge ou ver um agente em execução não confirma que o merge foi concluído.

> [!IMPORTANT]
> O Agent Merge não ignora proteções do repositório nem permissões ausentes. Resolva esses impedimentos antes de continuar.

## Resumo e próximas etapas

Você automatizou várias partes do processo de desenvolvimento, incluindo gerar, testar e validar código e, agora, gerenciar o processo do pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionou todo o PR da funcionalidade e as evidências de verificação.
- autorizou o Agent Merge somente depois da revisão e confirmou que o PR foi integrado.

Em seguida, você vai [usar um canvas existente e criar um canvas de triagem][next-lesson] para explorar uma maneira mais rica de inspecionar, planejar e visualizar o trabalho com o agente.

## Recursos

- [Gerencie issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
