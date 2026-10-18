---
title: "Exercício 3 - Teste sua funcionalidade com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar a funcionalidade de filtragem com o modo agente. Antes de abrir uma pull request, confirme se ela realmente funciona no navegador. Em vez de percorrer o aplicativo por conta própria, conecte o **servidor MCP do Playwright** e deixe o Copilot controlar um navegador real para testar a funcionalidade — depois publique a branch e abra a PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot.
- adicionar o servidor MCP do Playwright ao workspace.
- pedir ao Copilot que o use para testar manualmente a funcionalidade de filtragem em um navegador.
- publicar a branch e abrir uma pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo agente fica muito mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) é como o Copilot faz isso: um padrão para o agente se comunicar com ferramentas e serviços externos.

![Diagrama mostrando o funcionamento interno do modo agente e sua interação com o contexto, o LLM e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) oferece aos agentes de IA uma maneira de se comunicar com ferramentas e serviços externos. Com o MCP, os agentes de IA podem se comunicar com ferramentas e serviços externos em tempo real. Assim, podem acessar informações atualizadas (usando recursos) e executar ações em seu nome (usando ferramentas).

Essas ferramentas e esses recursos são acessados por meio de um servidor MCP, que funciona como uma ponte entre o agente de IA e as ferramentas e serviços externos. O servidor MCP gerencia a comunicação entre o agente de IA e as ferramentas externas, como APIs existentes ou ferramentas locais, por exemplo, pacotes npm. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente de IA pode acessar.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: oferece acesso a APIs para gerenciar repositórios do GitHub. O agente de IA pode executar ações como criar repositórios, atualizar os existentes e gerenciar issues e pull requests.
- **[Servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp)**: oferece recursos de automação de navegador com o Playwright. O agente de IA pode executar ações como acessar páginas, preencher formulários e selecionar botões.

Há muitos outros servidores MCP disponíveis, com acesso a diferentes ferramentas e recursos. O GitHub hospeda um [registro do MCP](https://github.com/mcp) para facilitar a descoberta e as contribuições ao ecossistema.

> [!CAUTION]
> Em relação à segurança, trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor MCP, revise cuidadosamente o código-fonte, verifique o publicador e considere as implicações de segurança. Use somente servidores MCP confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revise a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. Você verá um servidor `github` já configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

A entrada `github` vem com o template do projeto. Por isso, o Copilot conseguiu ler o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], então não é necessário instalá-lo localmente: o VS Code se conecta a ele por HTTP, e você se autentica no GitHub na primeira vez que o Copilot usa uma de suas ferramentas.

## Adicione o servidor MCP do Playwright

Agora você adicionará um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador que ele pode controlar, exatamente o que você precisa para testar sua funcionalidade.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` junto com `github` para que o arquivo fique assim:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Salve o arquivo.

Diferentemente do servidor remoto `github`, o servidor `playwright` é **local**: o VS Code o inicia na sua máquina executando `npx @playwright/mcp@latest`. O sinalizador `--headless` permite executar o navegador sem uma janela visível, requisito para um codespace, que não tem uma área de trabalho para exibi-lo.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright nos testes de ponta a ponta, portanto o navegador necessário geralmente já está instalado. Se o Copilot informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Inicie e confie no servidor

O VS Code inicia servidores MCP **sob demanda**: na primeira vez que o Copilot precisa das ferramentas de um servidor, o VS Code o inicia e solicita a confirmação de que você confia nele.

1. Em `.vscode/mcp.json`, aparecerá uma ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor e confirmar que a configuração está correta.
2. Se o VS Code solicitar confirmação de que você confia no servidor, revise a configuração e escolha confiar nele para que possa ser iniciado.

Iniciar o servidor manualmente é opcional. Se você pular essa etapa, o VS Code iniciará o servidor (e solicitará que você confie nele) na primeira vez que o Copilot precisar das ferramentas, na próxima etapa.

## Teste a funcionalidade de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] oferece ao Copilot um navegador real para controlar. Em vez de percorrer o aplicativo para conferir seu trabalho, o agente pode abrir uma página, navegar, aplicar filtros e ler o resultado, além de apresentar um resumo do que observou. É uma forma rápida de confirmar que a funcionalidade se comporta como esperado, sem sair da conversa.

Nos bastidores, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página em vez de capturas de tela. Isso significa que o agente analisa elementos estruturados e rotulados (botões, links, itens de lista) da mesma forma que uma tecnologia assistiva. Assim, uma verificação funcional rápida também serve como uma verificação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste a filtragem:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot iniciará um navegador pelo servidor MCP do Playwright, executará cada etapa e informará o que observou. Compare o resumo com os critérios de aceitação da issue. Se algo parecer errado, faça perguntas complementares ou peça que o Copilot corrija o código antes de abrir uma pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321` para esse teste. Se você tiver parado o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor MCP do Playwright, talvez seja necessário baixar um navegador. Se ele informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publique a branch e crie uma pull request

Agora que você confirmou que a funcionalidade funciona, pode abrir uma pull request (PR) para que a equipe a revise. Primeiro, publique a branch `filtering-vscode`.

1. Acesse o painel **Source Control** no codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Captura de tela do painel Source Control mostrando as alterações feitas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Crie a pull request

Há várias maneiras de criar uma pull request, incluindo o github.com e a interface de linha de comando (CLI) do GitHub. Mas, como você já está usando o GitHub Copilot, vamos pedir a ele que crie a PR. O Copilot pode localizar a issue relacionada e associá-la à PR.

1. Acesse o painel Copilot Chat e selecione **New Chat** para iniciar uma nova sessão.
2. Confirme que **Agent** está selecionado no menu suspenso de agentes para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot que crie uma PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Quando necessário, selecione **Continue** para permitir que o Copilot colete informações e execute operações. Na primeira vez que ele usar uma ferramenta do GitHub, talvez seja necessário autenticar-se. Siga as instruções para permitir o acesso.
5. Observe o Copilot pesquisar as issues, encontrar a correta e criar a PR.
6. Selecione o link gerado pelo Copilot para revisar sua pull request, mas **não a mescle ainda**.

## Resumo e próximos passos

Parabéns! Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot.
- adicionou o servidor MCP do Playwright ao workspace.
- usou o servidor para testar manualmente a funcionalidade de filtragem em um navegador antes de publicá-la.
- publicou a branch e abriu uma pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* uma funcionalidade, mas o MCP é apenas uma maneira de ampliar o Copilot. Na próxima lição, vamos [criar um agente personalizado][next-lesson] para agilizar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro do MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
