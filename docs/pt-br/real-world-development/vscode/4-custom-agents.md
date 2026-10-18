---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados, adaptados a tarefas ou domínios específicos do seu fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você pode fornecer instruções direcionadas, práticas recomendadas, padrões de codificação e conhecimentos específicos do domínio que orientam o Copilot a realizar determinados tipos de trabalho com mais eficiência. As equipes podem registrar seu conhecimento em agentes reutilizáveis — por exemplo, um agente de acessibilidade que aplica os requisitos de conformidade com [WCAG][wcag], um agente de segurança que segue práticas de codificação segura ou um agente de testes que mantém padrões de teste consistentes.

Os agentes personalizados são definidos por arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, o conhecimento especializado e as instruções do agente.

### Comparação entre agentes personalizados e agent skills

Há alguma sobreposição lógica entre agentes personalizados e [agent skills][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e orientam a IA sobre como realizar operações. A distinção mais simples é: um **agente personalizado** é quem executa o trabalho, e as **skills** são as ferramentas.

Os agentes personalizados têm sua própria janela de contexto e foram criados para coordenar skills (e até outros agentes) durante o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisa e atualiza o site de acordo com as diretrizes de acessibilidade; como parte desse trabalho, ele pode chamar skills, como uma skill de fluxo de pull request ou outra que execute e gerencie testes.

> [!NOTE]
> Não há uma única maneira correta de criar um agente personalizado. Como acontece com qualquer recurso de IA, teste e faça iterações para descobrir o que funciona melhor em seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer garantir que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente de suas capacidades ou preferências visuais. Comentários recentes de usuários indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual devido ao contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver essa questão de acessibilidade, a equipe de design solicitou a implementação de um modo de alto contraste que os usuários possam ativar e desativar.

Como a acessibilidade é fundamental, queremos implementar essa funcionalidade rapidamente. Para isso, usaremos um agente personalizado para gerar a funcionalidade.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar o modo de alto contraste.

## Revise o agente personalizado de acessibilidade

Um agente personalizado para acessibilidade já foi criado. Vamos revisar o conteúdo para entender como ele orientará o Copilot.

Volte ao seu codespace e abra um terminal. Crie uma nova branch baseada em `main` para o trabalho de acessibilidade (a PR de filtragem do Exercício 3 ficará separada):

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Examine as seções seguintes, que destacam:
   - principais responsabilidades ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código para HTML, CSS e JavaScript.
   - erros e problemas comuns.

## Use o agente personalizado no Copilot Chat

O VS Code mostra todos os agentes personalizados definidos em `.github/agents` no menu suspenso de agentes, na parte inferior da visualização Copilot Chat. Você pode selecionar um agente personalizado para limitar uma sessão de chat às instruções e ferramentas definidas para ele.

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção de modelo variam conforme o exercício; cada etapa indicará as opções necessárias.

1. Se **Agent** ainda não estiver selecionado, escolha-o no menu suspenso de agentes da visualização Chat.

   ![Captura de tela do seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione o menu suspenso de agentes na parte inferior da visualização Chat (ele mostra o agente ativo, que por padrão é **default**).
3. Selecione **Accessibility agent** na lista de agentes disponíveis.
4. Envie o seguinte prompt ao agente de acessibilidade:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. O Copilot Chat começará a trabalhar: explorará a base de código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá na conversa com o caminho do arquivo e um diff para você revisar.
6. À medida que as alterações forem aplicadas, o indicador **Files changed** será atualizado para mostrar os arquivos modificados pelo agente.

> [!NOTE]
> O processo provavelmente levará alguns minutos. O Copilot fará alterações reais no repositório — editará arquivos existentes, como componentes Astro, CSS e testes relacionados.

Você acompanhará e orientará o trabalho em andamento no próximo exercício.

## Resumo e próximos passos

Nesta lição, você explorou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes de IA especializados em tarefas e domínios específicos. Com agentes personalizados, você pode registrar a experiência e os padrões da equipe em agentes reutilizáveis, que orientarão o Copilot a realizar determinados tipos de trabalho com mais eficiência.

Você explorou estes conceitos:

- como os agentes personalizados são definidos.
- como usar um agente personalizado no modo agente do Copilot Chat.

Na próxima lição, você vai [acompanhar e orientar o trabalho do agente][next-lesson], revisar as alterações à medida que são feitas e adicionar um botão de modo claro à mesma sessão.

## Recursos

- [Sobre os agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar sua funcionalidade com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
