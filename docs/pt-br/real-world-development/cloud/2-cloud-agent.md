---
title: "Exercício 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Provavelmente são poucas, se houver, as organizações que não enfrentam dificuldades com dívida técnica. Ela pode incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de funcionalidades que ficaram no backlog por falta de tempo para implementá-las. O cloud agent do GitHub Copilot foi criado para realizar tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina o trabalho, o agente gera uma PR de rascunho, pronta para revisão por uma pessoa. Isso permite delegar tarefas tediosas, acelerar o desenvolvimento e liberar os desenvolvedores para se concentrarem em questões mais importantes.

Você vai explorar os seguintes aspectos do Copilot cloud agent:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas de forma segura.
- entender a importância de issues com escopo claro.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas para resolver. Os prestadores de serviços inicialmente contratados para criar a primeira versão do site deixaram a documentação em um estado insatisfatório: ela está completamente ausente. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas no aplicativo.

Além disso, a equipe de design está pronta para melhorar a descoberta de jogos. Ela quer que a página de detalhes de cada jogo mostre títulos relacionados — outros jogos da mesma categoria — para que as pessoas possam continuar explorando. Por enquanto, não é necessária uma implementação refinada; a equipe só quer algo que possa usar para testes de aceitação da experiência. Essa tarefa está bloqueada, mas há outras issues de prioridade maior.

Esses são exemplos de tarefas que acabam sendo adiadas com facilidade e que são ótimas candidatas para o Copilot cloud agent. Ele pode trabalhar nelas de forma assíncrona enquanto o desenvolvedor se concentra em outras tarefas e, depois, pode revisar o trabalho do Copilot e confirmar que está tudo certo.

## Apresentação do GitHub Copilot cloud agent

O [GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode realizar tarefas em segundo plano, de maneira semelhante a um desenvolvedor. E, assim como no trabalho com um desenvolvedor, há várias formas de fazer isso, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria uma pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar na tarefa. Você pode acompanhar a sessão do Copilot enquanto o trabalho está em andamento ou depois que ele termina. Quando a solução proposta estiver pronta para revisão, o Copilot marcará você na pull request!

## A importância de instruções com escopo bem definido

O GitHub Copilot não faz mágica — não é possível pular a etapa de raciocínio. Até tarefas aparentemente simples ficam complexas quando analisadas em detalhes; por isso, [defina cuidadosamente o escopo das tarefas para o Copilot cloud agent][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a adoção da IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram as instruções personalizadas e como elas orientam o Copilot. Se você já fez o exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para dar uma olhada rápida. Antes de atribuir trabalho ao Copilot cloud agent, examine os arquivos de instruções incluídos neste repositório, sem alterá-los, para depois identificar o impacto que eles tiveram.

Abra os arquivos a seguir na interface web do GitHub ou em um codespace, caso já tenha iniciado um:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, especialmente as expectativas para convenções do TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que limita essas instruções aos arquivos `**/*.test.ts`.

Quando você atribuir a issue `Code lacks documentation` ao cloud agent na próxima seção, observe na pull request resultante os comentários TSDoc, as convenções de TypeScript e os cabeçalhos dos arquivos — eles vêm desses arquivos de instruções. Voltaremos a isso ao revisar PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento do Copilot cloud agent

Desenvolver código, independentemente de quem esteja envolvido, geralmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto. O mesmo vale para tarefas atribuídas ao Copilot, que trabalha de maneira semelhante a um engenheiro de software.

O cloud agent usa o [GitHub Actions][github-actions] para configurar o ambiente de trabalho. Você pode personalizar esse ambiente por meio de um [workflow de configuração especial][setup-workflow], definido no arquivo `.github/workflows/copilot-setup-steps.yml` e executado antes de o agente começar. Isso permite disponibilizar as ferramentas de desenvolvimento e dependências necessárias. Esse workflow foi configurado previamente para facilitar o laboratório e permitir que você aprenda usando-o. Ele garante que o Copilot tenha acesso ao Node.js, às dependências do projeto, aos binários do navegador para testes de ponta a ponta e ao banco de dados SQLite migrado e preenchido do aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo é parecido com qualquer outro arquivo de workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar na pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para confirmar que o workflow funciona sem precisar esperar o Copilot executá-lo.

## Adicione documentação

Todos reconhecem a importância da documentação, mas a maioria dos projetos tem informações desatualizadas ou não tem documentação alguma. Esse tipo de dívida técnica costuma ficar sem solução, reduzindo a produtividade e dificultando a manutenção da base de código e a integração de novos desenvolvedores. Felizmente, o Copilot é ótimo para criar documentação, e esta é uma issue perfeita para atribuir ao Copilot cloud agent. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o resultado.

1. Em uma nova guia do navegador, acesse o repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de criação de issue.
4. Selecione **Blank issue** para criar a issue.
5. Defina **Title** como `Code lacks documentation`.
6. Defina **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra a pull request (PR) recém-criada, cujo título será parecido com `[WIP]: Code lacks documentation`. Se ela ainda não aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar alguns minutos para aparecer na PR. O Copilot está configurando o ambiente (executando o workflow mencionado acima), analisando o projeto e determinando a melhor abordagem para a tarefa.

13. Revise a lista e as tarefas que o Copilot executará.
14. Role a linha do tempo da pull request para baixo; você deverá ver uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

  ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão em tempo real e acompanhar como o Copilot resolve o problema. Isso inclui explorar o código e entender seu estado, fazer uma pausa para pensar e decidir o plano adequado e também criar código.

O processo provavelmente levará alguns minutos. Um dos principais objetivos do Copilot cloud agent é executar tarefas de forma assíncrona, liberando você para se concentrar em outras atividades. Vamos aproveitar exatamente esse recurso, atribuindo outra tarefa ao Copilot cloud agent e, em seguida, voltando a atenção para o desenvolvimento de uma funcionalidade no aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes dos jogos

Como vimos, uma das grandes vantagens do GitHub Copilot cloud agent é a possibilidade de dividir o trabalho: enquanto você se concentra em um conjunto de tarefas, ele cuida de outro. Talvez adicionar uma seção de jogos relacionados para a equipe de design não leve muito tempo, mas você ainda poderia usar esse tempo para outras tarefas. Vamos atribuí-la ao Copilot cloud agent!

1. Volte ao seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de criação de issue.
4. Selecione **Blank issue** para usar o template em branco.
5. Defina **Title** como `Show related games on the game details page`.
6. Defina **Description** como:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você deverá ver uma sequência de 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar!

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando diligentemente na segunda solicitação! Como o cloud agent atua de maneira semelhante a um engenheiro de software, não é necessário acompanhá-lo o tempo todo. Você pode revisar o trabalho quando ele estiver concluído. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Nesta lição, você explorou o [GitHub Copilot cloud agent][copilot-agents]. Com o cloud agent, você pode atribuir issues ao Copilot para que sejam executadas de forma assíncrona. Você pode usá-lo para resolver dívidas técnicas, criar funcionalidades ou ajudar a migrar código de uma estrutura para outra.

Você explorou estes conceitos:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas de forma segura.
- entender a importância de issues com escopo claro.
- atribuir issues ao Copilot.

Enquanto o cloud agent trabalha em segundo plano, agora podemos nos concentrar na criação e no uso de agentes personalizados. O [Copilot cloud agent também pode usar servidores MCP][cloud-agent-mcp] e tem acesso às instruções personalizadas, que exploramos em módulos anteriores.

## Recursos

- [Sobre o Copilot cloud agent][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do Copilot cloud agent][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
