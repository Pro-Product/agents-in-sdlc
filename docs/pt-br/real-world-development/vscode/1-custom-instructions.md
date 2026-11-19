---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica — ou se há informações de contexto que o Copilot deve conhecer — é importante garantir que ele possa acessá-las. Há várias maneiras de compartilhar contexto específico com o Copilot. Uma das principais são os [arquivos de instruções][instruction-files], que fornecem orientação sobre a geração de código.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instruções associados a caminhos.
- gerar a primeira fatia de dados para a filtragem (um helper de editoras) usando as instruções atuais.
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`.
- reenviar o mesmo prompt e observar o código gerado adotar o novo padrão.
- fazer commit das instruções atualizadas e da fatia de filtragem em `main` para que o Copilot use as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir alguns dos padrões definidos. O Copilot não é determinístico. O objetivo deste exercício é observar a *tendência* de mudança do comportamento depois de atualizar as instruções, não obter exatamente o mesmo texto em cada execução.

## Arquivos de instruções

### Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa de testes unitários.
- A interface do usuário deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir documentação na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar as tarefas de acordo com as práticas destacadas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo e seus requisitos de programação. Esse recurso permite orientar as sugestões e os trechos de código, especificando convenções, bibliotecas e até os tipos de comentários que você prefere. Você pode criar instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo de instruções enviado ao Copilot em **todas** as solicitações feitas para o repositório. Ele deve conter informações sobre o projeto relevantes para a maioria das solicitações de chat ou CLI feitas ao Copilot, como a pilha tecnológica, uma visão geral do que está sendo desenvolvido, práticas recomendadas e outras orientações globais.
- `.github/instructions/*.instructions.md`: arquivos que podem ser criados para tarefas ou tipos de arquivo específicos. Você pode fornecer diretrizes para linguagens como TypeScript ou Astro ou para tarefas como criar um componente de interface ou um novo conjunto de testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, mas não para conclusões de código nem sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam as instruções do repositório e os arquivos `*.instructions.md` (com frontmatter `applyTo`) ao gerar código.
>
> Por fim, o Copilot [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], incluindo arquivos AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma conversa completa sobre a criação de arquivos de instruções está fora do escopo deste workshop. No entanto, os exemplos do projeto mostram uma abordagem representativa. Em linhas gerais:

- Mantenha as instruções de `copilot-instructions.md` focadas em orientações para todo o projeto, como uma descrição do que está sendo desenvolvido, a estrutura do projeto e os padrões globais de código.
- Use arquivos `*.instructions.md` para instruções específicas de tipos de arquivo (testes unitários, componentes Astro, camada de dados) ou de tarefas específicas.
- Use linguagem natural e mantenha as orientações claras. Inclua exemplos de como o código deve (e não deve) ser escrito.

Não há uma única maneira certa de criar arquivos de instruções, assim como não há uma única maneira de usar IA. Experimente para descobrir o que funciona melhor para seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter uma coleção robusta de arquivos de instruções. Ao explorar os arquivos deste projeto, talvez você note instruções para vários tipos de tarefas, incluindo [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa opção de maneira diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI). A lição para a interface que você está usando explicará o recurso quando for relevante.
>
> Procura modelos ou um ponto de partida? Explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções personalizadas deste projeto

Leia os arquivos de instruções incluídos neste repositório: há um arquivo principal `copilot-instructions.md` e uma coleção de arquivos `*.instructions.md` para diversas tarefas. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Elas se aplicam a qualquer interação com o Copilot neste repositório.
3. Abra a pasta `.github/instructions` e confira seu conteúdo. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo: ele define um padrão glob (relativo à raiz do repositório) que determina a quais arquivos as instruções se aplicam. Neste caso, ele corresponde a arquivos de teste TypeScript, como os que seguem `**/*.test.ts`.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções (como `unit-tests.instructions.md`) e arquivos existentes do projeto. Assim, você pode dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e indicar exemplos para o Copilot seguir ao gerar código. (Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.)

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário nos arquivos.

## Crie uma branch para as alterações

Vamos seguir algumas práticas recomendadas e criar uma branch para as alterações.

1. Volte ao Codespace usado no exercício anterior.
2. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma branch e mude para ela:

   ```bash
   git checkout -b custom-instructions
   ```

## Use o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao Codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. A seleção de modo e modelo varia conforme o exercício — cada etapa indicará o que é relevante.

Para observar o impacto das instruções personalizadas, comece enviando um prompt com o arquivo de instruções atual. Mais tarde, você o atualizará e enviará o mesmo prompt novamente para comparar o resultado.

1. Feche as abas do editor abertas nos exercícios anteriores para que o Copilot use somente o contexto desejado.
2. Abra `src/lib/publishers.ts` para indicar onde o helper deve ficar.
3. Na visualização Chat, selecione **Agent** no menu suspenso de agentes para que o Copilot possa aplicar alterações aos arquivos.

   ![Captura de tela do seletor de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie o seguinte prompt:

   ```plaintext
   Crie ou atualize src/lib/publishers.ts com um helper de acesso a dados que retorne uma lista de todas as editoras, com nome e id de cada uma. Aplique as alterações ao arquivo.
   ```

5. O Copilot explora o projeto e aplica atualizações ao código, geralmente abrangendo o arquivo do helper e seus testes.
6. Observe que o helper proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras — isso segue as convenções da camada de dados em `.github/instructions/drizzle.instructions.md` (aplicável a `src/lib/*.ts`).
7. Observe que o código proposto **não tem** comentários TSDoc nem cabeçalho de comentário no arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele adicione comentários mesmo sem essa orientação. Tudo bem — o objetivo é observar a *consistência* depois da atualização das instruções.

## Adicione um padrão para todo o repositório

Como já destacamos, `.github/copilot-instructions.md` fornece informações do projeto ao Copilot. Vamos documentar os padrões de código do repositório para melhorar as sugestões.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe que ela documenta os padrões de código do projeto, mas ainda não há uma regra para documentação no código, por isso o helper gerado não tinha comentários de documentação.
3. Adicione as linhas Markdown a seguir logo abaixo dos padrões existentes, instruindo o Copilot a incluir cabeçalhos de comentário e comentários TSDoc:

   ```markdown
   - Toda função exportada deve ter um comentário TSDoc descrevendo sua finalidade, seus parâmetros e seu valor de retorno.
   - Antes dos imports ou de qualquer código, adicione ao arquivo um bloco de comentários que explique sua finalidade.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ser criados no nível do repositório (`.github/copilot-instructions.md`) para fornecer orientações globais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o lugar certo para padrões de todo o projeto, como a regra de comentários de documentação que você acabou de adicionar.

## Reenvie o prompt e observe a diferença

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o histórico.
2. Clique novamente em `src/lib/publishers.ts` para direcionar o Copilot ao arquivo correto.
3. Envie o **mesmo prompt** de antes:

   ```plaintext
   Crie ou atualize src/lib/publishers.ts com um helper de acesso a dados que retorne uma lista de todas as editoras, com nome e id de cada uma. Aplique as alterações ao arquivo.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários parecido com este:

   ```typescript
   /**
    * Helpers de acesso a dados de editoras da plataforma de financiamento coletivo Tailspin Toys.
    * Fornece funções para recuperar informações de editoras do banco de dados.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc parecido com este:

   ```typescript
   /**
    * Retorna uma lista de todas as editoras com seus identificadores e nomes.
    *
    * @param db - O cliente de banco de dados do Drizzle.
    * @returns Uma promessa que é resolvida com uma matriz de objetos de editoras.
    */
   ```

Você acabou de orientar o Copilot a seguir um novo padrão do projeto e aplicá-lo ao código real que será usado no próximo exercício.

## Faça commit das instruções e envie a branch

Os arquivos de instruções são recursos do repositório e devem ser gerenciados usando o mesmo processo de controle do código-fonte. Vamos fazer commit das alterações e enviar a branch.

1. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. No terminal, confirme que as instruções e as alterações do helper estão presentes:

   ```bash
   git status
   ```

3. No terminal, prepare e faça commit das instruções e de todos os arquivos alterados pelo Copilot para a base do helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras atualizações relacionadas (por exemplo, testes), adicione também esses arquivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Crie e faça merge de um pull request

Com a branch enviada, vamos abrir um pull request associado à issue sobre o padrão de documentação no backlog. Poderíamos fazer isso manualmente, mas o Copilot já tem acesso às ferramentas do GitHub conectadas ao projeto. Vamos pedir que localize a issue, crie um PR para encerrá-la e, depois, faça o merge.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executará ações no GitHub por você. O modelo do projeto já conecta as ferramentas do GitHub ao ambiente, então não é preciso configurar nada. Na primeira vez que o Copilot usar uma delas, o VS Code talvez peça que você entre no GitHub. Siga as instruções para autorizar. Em um exercício posterior, você aprenderá como essa conexão funciona (Model Context Protocol).

1. Abra o Copilot Chat no Codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização do Copilot Chat e confirme que **Agent** está selecionado no seletor de agentes.
3. Peça ao Copilot para localizar a issue relacionada à atualização dos arquivos de instruções e criar um PR da branch atual, descrevendo tanto as atualizações das instruções quanto a base do novo helper de editoras:

   ```
   Encontre a issue relacionada à atualização do arquivo de instruções. Crie um novo PR a partir da branch atual, indique que o PR encerra essa issue e mencione também que adicionamos a base do helper de editoras para o próximo trabalho de filtragem.
   ```

4. O Copilot começará a localizar a issue e criar o PR.
5. Quando solicitado a permitir que o Copilot execute ações no GitHub em seu nome, revise o comando e selecione **Allow** se for apropriado.
6. Depois que o PR for criado, peça ao Copilot que faça merge do PR e volte à branch `main`:

   ```
   Faça merge do PR em main. Em seguida, volte localmente para main e baixe o código mais recente para que tudo esteja atualizado.
   ```

7. Quando solicitado a permitir que o Copilot execute ações no GitHub e comandos do shell em seu nome, revise o comando e selecione **Allow** se for apropriado.

Você acabou de criar e fazer merge de um pull request com a ajuda do GitHub Copilot!

## Resumo e próximas etapas

Você explorou como o Copilot obtém contexto dos arquivos de instruções do projeto e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o que o Copilot produz com as instruções *atuais*.
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`.
- reenviar o mesmo prompt e observar o código proposto adotar o novo padrão.
- fazer commit das instruções atualizadas e da base do helper em `main` para que o próximo exercício possa usá-los.

Em seguida, você colocará as instruções em prática no [modo de agente][next-lesson], enquanto o Copilot adiciona uma nova funcionalidade à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções personalizadas pessoais para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
