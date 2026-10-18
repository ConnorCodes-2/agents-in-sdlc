---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao Copilot cloud agent que realizasse três tarefas diferentes, voltadas a melhorar a experiência do usuário e adicionar funcionalidades. Embora o cloud agent trabalhe de forma assíncrona e autônoma, ainda é importante acompanhar essas tarefas.

Há diversas ferramentas para gerenciar tarefas atribuídas ao cloud agent, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse centro de controle, você pode ver todas as tarefas de agentes com pull requests (PRs) abertas. Você pode explorar as operações realizadas e até orientar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para acompanhar as tarefas do cloud agent.
- orientar uma sessão em andamento para solicitar uma funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a criação do modo de alto contraste, a equipe percebeu que seria bom adicionar também um modo claro. Como o trabalho já envolvia atualizar o estilo do site e adicionar a funcionalidade de alternância, pareceu lógico incluir esse recurso. Você orientará o agente para que ele adicione o modo claro junto com o alto contraste.

## Revise as tarefas do Copilot cloud agent

Vamos conferir o status atual de todas as tarefas atribuídas ao Copilot cloud agent.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
    - atualizar a documentação da base de código.
    - gerar APIs para modificar jogos.
    - adicionar um modo de alto contraste ao site.
3. Selecione uma das tarefas em execução. Revise as operações realizadas pelo Copilot, que podem incluir:
    - obter o código do repositório.
    - criar o ambiente de trabalho do Copilot.
    - configurar servidores MCP.
    - realizar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas exatas exibidas variam de acordo com o estado do trabalho do Copilot e a abordagem adotada.

4. Observe também o painel da pull request (PR), que aparece à direita. Ele permite ver a PR e os arquivos alterados para acompanhar o trabalho.

## Oriente o cloud agent

Agora que você viu as tarefas em andamento, peça ao Copilot que inclua o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão referente à adição do modo de alto contraste. O título exato varia conforme o nome usado pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade no centro de controle](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que ela indique que a configuração terminou e o trabalho começou. Você saberá que isso aconteceu quando começar a ver mensagens semelhantes às abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o seguinte prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de tela da tarefa do cloud agent na página de agentes, com a caixa de diálogo para orientar uma sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe que o Copilot confirma o recebimento e inclui a solicitação no fluxo de trabalho.

## Deixe o Copilot trabalhar

Como antes, o Copilot começará a trabalhar na tarefa atualizada! Ele incorporará a nova solicitação ao fluxo de trabalho depois de concluir a etapa específica em que estava quando você enviou a mensagem.

Como antes, isso levará alguns minutos. Aproveite para pausar e refletir sobre tudo o que você aprendeu e explorou até agora.

## Resumo e próximos passos

Nesta lição, você explorou a página de agentes do Copilot, o centro para acompanhar e orientar as tarefas do GitHub Copilot cloud agent. Nesse centro de controle, você pode acompanhar todas as tarefas ativas e concluídas, revisar o trabalho em andamento e até redirecionar tarefas ativas para ajustar o escopo ou fornecer orientações adicionais.

Você explorou estes conceitos:

- usou o centro de controle e a página de agentes para acompanhar as tarefas do cloud agent.
- redirecionou uma sessão em andamento para solicitar funcionalidades adicionais.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, agora podemos nos concentrar em [fazer iterações nas pull requests criadas pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre os agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Fazer iterações no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
