---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica — ou há informações básicas que o Copilot precisa conhecer —, garanta que ele tenha acesso a esse contexto. Há várias maneiras de compartilhar contextos específicos com o Copilot. Uma das principais são os [arquivos de instruções][instruction-files], que fornecem orientações sobre a geração de código.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de codificação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e arquivos de instruções com escopo de caminho.
- gerar a primeira parte da camada de dados para a filtragem (um helper de editoras) usando as instruções atuais.
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`.
- executar novamente o mesmo prompt e observar o código gerado seguir o novo padrão.
- fazer commit da atualização das instruções e da parte da filtragem em `main` para que o Copilot use as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir alguns dos padrões definidos. O Copilot é não determinístico. O objetivo deste exercício é observar a *tendência* do comportamento após a atualização das instruções, não obter uma saída idêntica caractere por caractere.

## Arquivos de instruções

### Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa de testes unitários.
- A interface do usuário deve usar o modo escuro e ter um visual moderno.
- O código deve incluir documentação na forma de comentários TSDoc.
- Cada arquivo deve começar com um bloco de comentários descrevendo sua finalidade.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações certas para realizar as tarefas de acordo com as práticas destacadas.

### Instruções personalizadas

As instruções personalizadas permitem fornecer contexto e preferências ao Copilot para que ele entenda melhor seu estilo de codificação e seus requisitos. Esse recurso ajuda você a orientar o Copilot para obter sugestões de código mais relevantes. Você pode especificar convenções de codificação, bibliotecas e até os tipos de comentários que prefere incluir no código. É possível criar instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo enviado ao Copilot em **todas** as solicitações feitas no repositório. Ele deve conter informações do projeto relevantes para a maioria das solicitações de chat ou CLI enviadas ao Copilot, como a pilha de tecnologia, uma visão geral do projeto, práticas recomendadas e outras orientações gerais.
- Arquivos `.github/instructions/*.instructions.md`, que podem ser criados para tarefas ou tipos de arquivo específicos. Você pode usá-los para fornecer orientações para linguagens específicas, como TypeScript ou Astro, ou para tarefas como criar um componente de interface ou testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados apenas para gerar código no Copilot Chat — não para conclusões de código ou sugestões da próxima edição.
>
> O Copilot Chat, o Copilot CLI e o Copilot cloud agent usam as instruções do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo`.
>
> O Copilot [também oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], incluindo AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma conversa completa sobre como criar arquivos de instruções está além do escopo deste workshop. No entanto, os exemplos incluídos no projeto mostram uma abordagem representativa. Em linhas gerais:

- Mantenha `copilot-instructions.md` focado em orientações gerais do projeto, como uma descrição do que está sendo desenvolvido, a estrutura do projeto e os padrões globais de codificação.
- Use arquivos `*.instructions.md` para fornecer instruções específicas para tipos de arquivo, como testes unitários, componentes Astro e a camada de dados, ou para tarefas específicas.
- Use linguagem natural. Escreva orientações claras e dê exemplos de como o código deve (e não deve) ser.

Não há uma única maneira correta de criar arquivos de instruções, assim como não há uma única maneira de usar IA. A experimentação ajudará você a descobrir o que funciona melhor para o projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto robusto de arquivos de instruções. Ao explorar os arquivos deste projeto, você verá instruções para vários tipos de tarefas, como [atualizações da interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece esse recurso de um jeito (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI); a lição da interface que você estiver usando indicará quando for relevante.
>
> Quer modelos ou um ponto de partida? Explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções personalizadas deste projeto

Leia os arquivos de instruções incluídos neste repositório: há um `copilot-instructions.md` principal e uma coleção de arquivos `*.instructions.md` para várias tarefas. Abra-os no editor ou na interface web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas de **GitHub Actions Workflows**. Elas se aplicam a todas as interações com o Copilot neste repositório.
3. Abra a pasta `.github/instructions` e explore os arquivos. Observe que há instruções para arquivos Astro, para a camada de dados Drizzle, para testes e outros.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo: ele define um glob (relativo à raiz do repositório) que determina a quais arquivos as instruções se aplicam. Nesse caso, arquivos de teste TypeScript, como os que correspondem a `**/*.test.ts`, serão selecionados.
5. Observe as instruções específicas para a criação de testes unitários neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e vá até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Isso permite dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e apontar o Copilot para exemplos. (Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.)

> [!NOTE]
> A seção **Code formatting requirements** de `copilot-instructions.md` documenta os padrões de codificação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Crie uma branch para as alterações

Vamos seguir algumas práticas recomendadas e criar uma branch para as alterações.

1. Volte ao codespace usado no exercício anterior.
2. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma branch e mude para ela:

   ```bash
   git checkout -b custom-instructions
   ```

## Use o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção de modelo variam conforme o exercício; cada etapa indicará as opções necessárias.

Para observar o impacto das instruções personalizadas, comece enviando um prompt com o arquivo de instruções atual. Mais tarde, você atualizará o arquivo e enviará o mesmo prompt novamente para comparar os resultados.

1. Feche as guias do editor abertas nos exercícios anteriores para que o Copilot receba apenas o contexto desejado.
2. Abra `src/lib/publishers.ts` para indicar onde o helper deve ficar.
3. Selecione **Agent** no menu suspenso de agentes da visualização Chat para que o Copilot possa aplicar alterações nos arquivos.

   ![Captura de tela do seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie o seguinte prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará atualizações de código, muitas vezes incluindo o arquivo do helper e os testes.
6. Observe que o helper proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna um array de editoras tipado — isso vem das convenções da camada de dados em `.github/instructions/drizzle.instructions.md` (aplicadas a `src/lib/*.ts`).
7. Observe que o código proposto **não inclui** comentários TSDoc nem um cabeçalho de comentário para o arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, existe a possibilidade de ele adicionar comentários de documentação mesmo sem essa orientação. Se isso acontecer, tudo bem — o objetivo continua sendo observar a melhoria de *consistência* após a atualização das instruções.

## Adicione um novo padrão para o repositório

Como vimos, `.github/copilot-instructions.md` fornece informações gerais do projeto ao Copilot. Vamos registrar os padrões de codificação para melhorar as sugestões.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe como ela documenta os padrões do projeto, mas ainda não contém uma regra para documentação no código. Por isso, o helper gerado não tinha comentários de documentação.
3. Adicione as linhas Markdown abaixo, logo após os padrões existentes, para instruir o Copilot a incluir cabeçalhos de comentários nos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ser criados no nível do repositório (`.github/copilot-instructions.md`) para orientações gerais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o lugar certo para padrões gerais, como a regra de comentários de documentação que você acabou de adicionar.

## Execute novamente o prompt e observe a alteração

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o buffer.
2. Volte a `src/lib/publishers.ts` para que o Copilot se concentre no arquivo correto.
3. Envie o **mesmo prompt** usado antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você acabou de orientar o Copilot a seguir um novo padrão e aplicá-lo ao código real que servirá de base para o próximo exercício.

## Faça commit das instruções e envie a branch

Os arquivos de instruções são como qualquer outro recurso do repositório e são gerenciados com o mesmo controle de versão. Vamos fazer commit e enviar a branch ao repositório.

1. Abra uma nova janela de terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. No terminal, confirme que as alterações nas instruções e no helper estão presentes:

   ```bash
   git status
   ```

3. No terminal, prepare e faça commit das alterações nas instruções e de todos os arquivos alterados pelo Copilot para criar o helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Crie e mescle uma pull request

Com a branch enviada, devemos criar uma pull request e associá-la à issue do backlog sobre os padrões de documentação. Poderíamos fazer isso manualmente, mas o Copilot pode ajudar usando as ferramentas do GitHub já conectadas ao projeto. Vamos pedir ao Copilot que encontre a issue, crie uma PR para encerrá-la e depois faça a mesclagem.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executará ações no GitHub em seu nome. O template do projeto já conecta as ferramentas do GitHub ao workspace, portanto não é necessário configurá-las. Na primeira vez que o Copilot usar uma delas, o VS Code poderá solicitar que você entre no GitHub. Siga as instruções para permitir o acesso. Você aprenderá como funciona essa conexão (Model Context Protocol) em um exercício futuro.

1. Abra o Copilot Chat no codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização Copilot Chat e confirme que **Agent** está selecionado no seletor de agentes.
3. Peça ao Copilot que encontre a issue relacionada à atualização dos arquivos de instruções e crie uma PR a partir da branch atual, descrevendo as atualizações nas instruções e a base do helper de editoras:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. O Copilot começará a procurar a issue e criar a PR.
5. Quando solicitado a **Allow** que o Copilot execute ações do GitHub em seu nome, revise o comando e selecione **Allow** quando apropriado.
6. Depois que a PR for criada, peça ao Copilot que a mescle e volte para `main` com o seguinte prompt:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Quando solicitado a **Allow** que o Copilot execute ações do GitHub e comandos do shell em seu nome, revise o comando e selecione **Allow** quando apropriado.

Você acabou de criar e mesclar uma pull request com a ajuda do GitHub Copilot!

## Resumo e próximos passos

Você explorou como o Copilot carrega o contexto dos arquivos de instruções do projeto e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções *existentes*.
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`.
- executar novamente o mesmo prompt e observar o código proposto seguir o novo padrão.
- fazer commit da atualização das instruções e do helper de base em `main` para que o próximo exercício possa usá-los.

Na próxima lição, você colocará essas instruções em prática no [modo agente][next-lesson], enquanto o Copilot adiciona uma nova funcionalidade à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções personalizadas pessoais para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
