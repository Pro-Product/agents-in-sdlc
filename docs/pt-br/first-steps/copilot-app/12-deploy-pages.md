---
title: "Lição 12 - Publique o site com o GitHub Pages"
description: "Use o aplicativo GitHub Copilot para configurar um fluxo de trabalho do Pages, revisar o pull request e verificar o Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Dê ao Space Quiz um endereço público publicando-o com o GitHub Pages. O Copilot pode preparar o fluxo de trabalho e orientar a publicação, mas você continua responsável por revisar as alterações e confirmar que o site está no ar.

Nesta lição, você vai:

- pedir ao Copilot para configurar a publicação no GitHub Pages.
- revisar o fluxo de trabalho e o pull request antes de fazer o merge.
- acompanhar a publicação e verificar o site publicado.

## Cenário

O Space Quiz já está em um repositório do GitHub, mas as pessoas ainda precisam de uma maneira simples de experimentá-lo. Publicar o site de arquivo único com o GitHub Pages o disponibiliza em uma URL pública, sem adicionar um servidor ou dependência de runtime.

## Peça ao Copilot para preparar a publicação

> [!WARNING]
> Criar um pull request pode fazer commit e enviar código. Revise as configurações de aprovação e o repositório de destino antes de enviar este prompt. Com **Aprovar tudo**, não espere uma solicitação de confirmação separada.

Na sessão do aplicativo Copilot para `space-quiz`, envie este prompt:

```plaintext
Prepare uma publicação no GitHub Pages para este repositório. Crie um fluxo de trabalho do GitHub Actions que compile o site, se necessário, e o publique no GitHub Pages depois que as alterações chegarem à branch padrão. Adicione um README que descreva o aplicativo e como executá-lo localmente, caso ainda não exista. Crie um pull request com essas alterações e informe o link e quaisquer configurações ou permissões necessárias do Pages. Não faça merge, não habilite o merge automático, não altere as configurações do Pages nem publique o site. Pare e aguarde minha revisão.
```

## Revise o fluxo de trabalho e o pull request

1. Confirme que o README foi adicionado somente se o repositório ainda não tinha um.
2. Revise as alterações do pull request, incluindo as etapas de compilação e publicação do fluxo de trabalho, os gatilhos e as permissões.
3. Confirme que o fluxo de trabalho publica a partir da branch padrão, e não de um pull request ainda não integrado.
4. Solicite as correções necessárias e revise o diff atualizado. Aguarde a aprovação nas verificações obrigatórias do pull request antes de autorizar o merge.

## Autorize o merge e a publicação

> [!WARNING]
> O prompt a seguir autoriza o agente a fazer merge do pull request revisado, alterar as configurações do Pages do repositório e publicar o site. Envie-o somente depois de concluir a revisão e confirmar o repositório e o pull request. As configurações de aprovação podem permitir essas ações sem outra solicitação de confirmação.

Substitua `<PR_NUMBER>` pelo número do pull request revisado e envie este prompt separado:

```plaintext
Revisei o pull request <PR_NUMBER> neste repositório e autorizo o merge e a publicação. Habilite o GitHub Pages usando o GitHub Actions, faça merge desse pull request somente depois que as verificações obrigatórias forem aprovadas e acompanhe a publicação até ela ser concluída com êxito. Informe a URL final do site no ar. Se você não tiver permissão para habilitar o Pages ou fazer o merge, diga exatamente o que preciso aprovar ou fazer; não informe sucesso até que o site esteja no ar.
```

![Diagrama em três etapas: um pull request de space-quiz integrado; uma execução bem-sucedida de Deploy website no GitHub Actions, com as etapas Build website, Upload artifact e Deploy to Pages; e o Space Quiz no ar em yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

## Verifique a publicação

1. Acompanhe a execução de publicação do GitHub Actions até que ela seja concluída com êxito.
2. Abra a URL informada e confirme que o Space Quiz carrega.

Se o agente não puder habilitar o Pages ou fazer o merge do pull request, siga as instruções específicas fornecidas. Não considere a tarefa concluída até que a publicação tenha êxito e o site esteja acessível.

## Resumo e próximas etapas

Agora o Space Quiz está disponível como um site no ar, e você verificou a publicação em vez de confiar apenas no relato do agente. Continue para [Lição 13: Revisão e próximas etapas][next-lesson].

[next-lesson]: ../13-review/
