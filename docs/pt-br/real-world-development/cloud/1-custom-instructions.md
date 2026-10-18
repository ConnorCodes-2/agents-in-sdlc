---
title: "Exercício 1 - Instruções personalizadas (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Copilot cloud agent →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica — ou há informações básicas que o Copilot precisa conhecer —, garanta que ele tenha acesso a esse contexto. Os [arquivos de instruções][instruction-files] fornecem essas orientações para que o Copilot entenda não apenas *o que* você quer que ele faça, mas também *como* deve fazê-lo.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de codificação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e arquivos de instruções com escopo de caminho.
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos ambientes do VS Code e do CLI, você não fará aqui um prompt *antes/depois*. O Copilot cloud agent trabalha de forma assíncrona em issues do GitHub, portanto é mais difícil demonstrar o impacto lado a lado em tempo real. Você verá a influência do arquivo de instruções mais adiante, ao revisar as pull requests criadas pelo cloud agent.

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

## Adicione um novo padrão para o repositório

A próxima etapa é a única edição que você fará aqui: adicionar uma regra geral para que a documentação fique no código na forma de comentários TSDoc e de um cabeçalho de comentário no arquivo. O cloud agent usará essa orientação ao trabalhar nas issues que você atribuir a ele mais adiante neste módulo.

Antes de editar, crie uma branch (o cloud agent lerá as instruções da branch de destino da issue, mas você fará o commit das alterações de forma organizada):

1. No terminal do codespace, crie uma branch e mude para ela:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## Faça commit, push e mescle a atualização das instruções

O cloud agent lê os arquivos de instruções da branch de destino da issue. Quando você atribuir uma issue ao Copilot no próximo exercício, ele criará uma branch baseada em `main`; por isso, as alterações nas instruções precisam estar em `main` para que o cloud agent as use.

1. Prepare e faça commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. Abra uma pull request de `custom-instructions-cloud` para `main` no github.com e mescle-a. Assim, o cloud agent lerá essas instruções quando trabalhar nas issues atribuídas a ele no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente em `main` neste workshop, pule as etapas da branch e faça commit diretamente em `main`. A etapa da branch está aqui para que o workshop reflita como você trabalharia em um projeto real.

## Resumo e próximos passos

Você explorou como o Copilot carrega o contexto dos arquivos de instruções deste projeto e adicionou um novo padrão para todo o repositório a `.github/copilot-instructions.md`. Você verá esse padrão nas pull requests produzidas pelo cloud agent ao longo do restante do módulo.

Na próxima lição, você vai [atribuir sua primeira issue ao Copilot cloud agent][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
