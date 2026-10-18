---
title: "Exercício 2 - Adicione funcionalidades com o modo agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até as atualizações mais simples de um aplicativo costumam exigir alterações em vários arquivos e operações como a execução de testes. O fluxo típico de um desenvolvedor inclui localizar todos os arquivos necessários, fazer as alterações, executar os testes, depurar, descobrir qual arquivo ficou de fora, fazer outra atualização... E assim por diante.

É aí que entra o modo agente do Copilot.

O modo agente do Copilot foi criado para agir com mais autonomia no IDE. Ele trabalha de maneira semelhante a um desenvolvedor: começa explorando a estrutura existente do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como usar o modo agente para adicionar funcionalidades ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo agente e o cloud agent foram criados para experiências diferentes. O modo agente trabalha no IDE, permitindo ciclos rápidos de feedback e interação. O cloud agent funciona como um colega de programação que trabalha de forma assíncrona, como integrante da equipe, e interage com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- o modo agente do GitHub Copilot pode implementar funcionalidades na interface e na camada de dados.
- revisar as alterações e os testes gerados pelo modo agente do Copilot antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que os usuários filtrem por categoria e editora. Você já adicionou um helper de editoras no exercício anterior e, agora, concluirá o trabalho restante na camada de dados, na interface e nos testes com o modo agente do Copilot.

## Execute o site Tailspin Toys

Antes de fazer qualquer alteração, vamos explorar o site Tailspin Toys para entender suas funcionalidades atuais.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com um tema para desenvolvedores. Ele permite listar jogos e exibir detalhes sobre eles. O site é um único aplicativo Astro que gera páginas como HTML estático na compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando o Drizzle ORM — não há uma API de back-end separada nem uma estrutura de interface no lado do cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é implementada com um pequeno `<script>` Astro com escopo restrito, usando as APIs DOM padrão.

### Inicie o site

Para facilitar a execução do site, há uma tarefa `npm run dev` que inicia o único servidor de desenvolvimento Astro. Execute-a no GitHub Codespace seguindo estas etapas:

1. Volte ao codespace. Você deve estar na branch `main` após o exercício anterior.
2. Abra um novo terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma branch para o trabalho de filtragem e mude para ela:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o seguinte comando para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, você verá uma mensagem com a URL, semelhante a esta:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo solicitar que você abra uma janela do navegador para `http://localhost:4321`, feche-a selecionando o **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, selecionar um link para a URL `localhost` no terminal do Codespace redirecionará automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o codespace, que agora hospeda seu servidor web!

### Explore o site

Com o site em execução, você pode explorar suas funcionalidades. Entre os principais recursos estão:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá uma página de detalhes com mais informações, incluindo título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção de modelo variam conforme o exercício; cada etapa indicará as opções necessárias.

A implementação inicial do site funciona, mas queremos aprimorá-la com novos recursos. Vamos começar revisando o backlog. Quando você criou o projeto a partir do template, um backlog de issues foi criado automaticamente. Peça ao GitHub Copilot que mostre esses itens.

1. Selecione **Agent** no menu suspenso de agentes da visualização Chat. O agente **Agent** planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos do terminal e invoca ferramentas.

   ![Captura de tela do seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo em detrimento de outro. Ao desenvolver este workshop, usamos o Claude Sonnet 4.5 e o incluímos nas instruções para que as sugestões sejam relativamente consistentes e a experiência seja adequada. No entanto, como os LLMs são probabilísticos, as sugestões que você receber poderão ser diferentes das descritas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot poderá usar outro comando MCP, mas ainda deverá concluir a tarefa.

3. Peça ao Copilot informações sobre o backlog enviando o seguinte prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```
4. Selecione **Continue** para executar o comando e listar todas as issues.
5. Revise a lista de issues gerada.

Observe que o Copilot também priorizou os itens com base na utilidade provável para os usuários.

## Revise os arquivos de instruções

Antes de iniciar a geração do código pelo agente, reserve um momento para revisar o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de instruções da [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para arquivos de instruções específicos para vários tipos de arquivo, incluindo:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao local do arquivo de instruções. Isso facilita a reutilização e a divisão de conjuntos grandes de instruções em arquivos menores e mais fáceis de gerenciar, além de permitir apontar para exemplos e modelos.

## Implemente a filtragem

Para concluir a filtragem, serão necessárias pelo menos três alterações diferentes no aplicativo:

- adicionar ou refinar a lógica de filtragem restante na camada de dados (`src/lib/`).
- adicionar ou atualizar testes para o comportamento da filtragem.
- atualizar a página de lista de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes de mesclar as alterações à base de código. O modo agente do Copilot pode realizar essas tarefas. Vamos adicionar a funcionalidade.

1. Continue na conversa atual com o Copilot ou inicie outra selecionando **New Chat**.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Captura de tela mostrando um exemplo de seleção do arquivo de instruções da interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado no menu suspenso de agentes da visualização Chat.

   ![Captura de tela do seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade com base na issue relacionada ao backlog usando este prompt:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorar o projeto e localizar os arquivos relacionados à funcionalidade desejada, incluindo os helpers da camada de dados, a interface e os testes. Em seguida, ele começará a modificar os arquivos e executar os testes.

   ![Captura de tela mostrando o Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Você verá que o Copilot executa várias tarefas, como explorar o projeto, modificar arquivos e executar testes. Dependendo da complexidade da tarefa e da base de código, isso pode levar alguns minutos. Durante o processo, poderão aparecer os botões **Keep** e **Undo** no editor. Quando o Copilot terminar, você verá um botão **Keep** ou **Undo** para cada alteração; não é necessário selecioná-los enquanto o agente ainda está trabalhando.

7. Quando o Copilot solicitar, selecione **Continue** para executar os testes.

   ![Captura de tela de uma caixa de diálogo no painel Copilot Chat solicitando confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Talvez haja pausas e alguns testes falhem durante o processo. Tudo bem! O Copilot alterna entre a geração do código e a execução dos testes até concluir a tarefa e não encontrar mais erros.

   ![Captura de tela de uma sessão completa do modo agente do Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado e procure possíveis problemas.

> [!CAUTION]
> Lembre-se: é sempre importante revisar o código gerado pelo Copilot ou por qualquer ferramenta de IA.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade!
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, exploramos como usar o modo agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Aprendemos como:

- o modo agente do GitHub Copilot pode implementar funcionalidades na interface e na camada de dados.
- o modo agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- revisar as alterações e os testes gerados pelo modo agente do Copilot antes de mesclá-los à base de código.

Agora vamos [testar sua funcionalidade com o servidor MCP do Playwright][next-lesson] e abrir uma pull request.

### Exercício de exploração bônus – Implemente a paginação

À medida que a lista de jogos cresce, será necessário adicionar paginação. Usando o que aprendeu neste exercício, peça ao Copilot que implemente a paginação no site. Algumas considerações:

- siga as práticas recomendadas existentes, incluindo o uso dos arquivos de instruções existentes.
- decida como a paginação funcionará: se o usuário poderá selecionar o tamanho da página ou se ele será fixo.
- ao criar o prompt, forneça ao Copilot as orientações necessárias para implementar o comportamento desejado.
- talvez seja necessário fazer iterações com o GitHub Copilot, pedindo alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot!

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
