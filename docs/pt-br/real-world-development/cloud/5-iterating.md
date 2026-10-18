---
title: "Exercício 5: Fazer iterações no trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revise o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que adicionasse documentação ao código, criasse uma funcionalidade de jogos relacionados para a equipe de design fazer iterações e implementasse recursos de acessibilidade, incluindo opções de alto contraste e modo claro. Vamos explorar as alterações no código e, se necessário, enviar comentários ao Copilot para melhorar o trabalho antes de habilitar as revisões para o restante da equipe.

### Cenário

Como já destacamos várias vezes, os fundamentos do design de software e do DevOps não mudam com a adoção da IA generativa. Sempre queremos revisar o código gerado e seguir nosso processo normal de DevOps. Com isso em mente, vamos revisar as sugestões do GitHub Copilot para criar a documentação, a funcionalidade de jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e GitHub Copilot cloud agent

Como o Copilot cloud agent realiza tarefas de forma assíncrona e sem supervisão, há algumas restrições de segurança para garantir que tudo continue seguro. Entre elas:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch usada para o código.
- O cloud agent é executado no GitHub Actions, onde cria um ambiente separado e efêmero para trabalhar.
- Qualquer workflow do GitHub Actions precisa da aprovação de uma pessoa antes de ser executado.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo o acesso a servidores MCP.

## Revise a documentação gerada

Vamos começar explorando a primeira pull request (PR) criada pelo GitHub Copilot cloud agent, que adiciona documentação ao código. Você fará isso pela interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao explorar a PR, talvez você veja um aviso de que o GitHub Copilot foi bloqueado por um firewall. **Isso é esperado**, pois o acesso do Copilot a recursos externos é limitado por padrão, incluindo chamadas a servidores MCP externos. Se quiser, você pode [personalizar ou desativar o firewall do Copilot cloud agent][agent-firewall].

1. Volte ao seu repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra a pull request intitulada algo como **Add missing documentation** ou um título mais descritivo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, a pull request terá a indicação **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos; aproveite para fazer uma pausa ou refletir sobre o que aprendeu até agora.

4. Quando a pull request estiver pronta, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui os novos comentários TSDoc e outros tipos de documentação. As alterações exatas variam.

    Ao examinar as alterações, procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentários. Eles vêm dos arquivos de instruções personalizadas que você revisou no início do Exercício 2.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role para baixo.
7. Você deverá ver um indicador de que há workflows aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprove e execute os workflows](../../../_images/shared-approve-workflows.png)
9. Você deverá ver os workflows na fila da seção de verificações da pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.

## Solicite alterações ao GitHub Copilot

Trabalhar com o Copilot em uma pull request não é uma via de mão única. Você também pode mencionar o Copilot em comentários, como faria com outros integrantes da equipe, seja na pull request ou em comentários embutidos no código. O Copilot verá os comentários e iniciará outra sessão para resolvê-los. Como os resultados não são determinísticos, não há um texto prescritivo para solicitar alterações. Algumas ideias:

- Adicionar cabeçalhos de comentários no início de cada arquivo de código, com uma breve descrição do que fazem.
- Adicionar comentários TSDoc a arquivos TypeScript e Astro.
- Criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário pedindo uma alteração na documentação gerada e mencione **@copilot**, como faria com qualquer usuário. Use uma das ideias acima ou outra sugestão de documentação que queira ver na base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Você pode selecionar **Back to pull request** para voltar à pull request.

    ![Voltar à pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, você verá um novo commit na pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue fazendo iterações até ficar satisfeito. Quando estiver tudo pronto, você poderá marcar a PR como pronta para revisão e mesclá-la à branch principal.

![Marcar a PR como pronta para revisão](../../../_images/ex7-ready-for-review.png)

## Revise a funcionalidade de jogos relacionados

Vamos voltar à PR criada pelo Copilot para resolver a issue sobre a exibição de jogos relacionados na página de detalhes.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione a PR cujo título é semelhante a **Show related games on the game details page** ou mais descritivo.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role para baixo.
6. Você deverá ver um indicador de que há workflows aguardando aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprove e execute os workflows](../../../_images/shared-approve-workflows.png)
8. Você deverá ver os workflows na fila da seção de verificações da pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** você pode mudar para essa branch no Codespace para testar manualmente a funcionalidade de jogos relacionados. Acesse seu Codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```
   
O Copilot criou a funcionalidade de jogos relacionados! Como antes, você pode fazer iterações no trabalho do Copilot cloud agent e solicitar atualizações. Por exemplo, você pode pedir ao Copilot que ajuste o número de jogos relacionados exibidos ou que adicione cabeçalhos de comentários e comentários TSDoc (lembre-se: esta tarefa foi atribuída **antes** da atualização das instruções personalizadas!). Como antes, envie essas solicitações em um novo comentário na guia **Conversation**. O Copilot verá o comentário e iniciará uma nova sessão.

## Revise os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados pelo agente personalizado de acessibilidade. Esta PR deve incluir tanto o modo de alto contraste atribuído no Exercício 3 quanto o modo claro solicitado no centro de controle no Exercício 4.

1. Volte ao seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione a PR cujo título é semelhante a **Add high contrast mode to website** ou mais descritivo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, a pull request terá a indicação **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, prestando atenção especial a:
   - componentes de interface para alternar entre os modos.
   - uso do armazenamento local para persistir as preferências do usuário.
   - alterações de CSS ou estilo para os modos de alto contraste e claro.
   - atributos de acessibilidade (rótulos ARIA, navegação pelo teclado etc.).
   - qualquer código JavaScript/TypeScript que gerencie a alternância de modos.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role para baixo.
7. Você deverá ver um indicador de que há workflows aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprove e execute os workflows](../../../_images/shared-approve-workflows.png)
9. Você deverá ver os workflows na fila da seção de verificações da pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** você pode mudar para essa branch no Codespace para testar manualmente os recursos de acessibilidade. Acesse seu Codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da branch criada pelo Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os botões dos modos de alto contraste e claro no navegador. Confirme se eles funcionam como esperado e se as preferências persistem ao recarregar a página.

Observe como o agente personalizado de acessibilidade ajudou a orientar o Copilot para implementar esses recursos seguindo as práticas recomendadas de acessibilidade. Se você identificar problemas ou melhorias de acessibilidade, mencione **@copilot** em um comentário para solicitar atualizações, como fez nas outras PRs.

## Exercício opcional — continue delegando

O cloud agent é mais útil quando você pode delegar itens reais do backlog e se concentrar em outras tarefas. Para criar esse hábito, registre mais algumas issues no seu repositório e atribua-as ao Copilot. Algumas ideias:

- Criar um formulário para demonstrar interesse dos apoiadores na página de detalhes do jogo.
- Implementar a paginação na página de lista de jogos.
- Adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o módulo do cloud agent. Ao longo destas lições, você:

- **Inspecionou os arquivos de instruções personalizadas incluídos neste repositório** para ver como afetam os resultados do cloud agent.
- **Atribuiu issues ao Copilot cloud agent** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** de acessibilidade, adicionando opções de alto contraste e modo claro.
- **Usou a página de agentes do Copilot como centro de controle** para acompanhar e orientar a sessão de acessibilidade enquanto estava em andamento.
- **Revisou e fez iterações nas pull requests do cloud agent**, mencionando **@copilot** para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o módulo do cloud agent. Se quiser continuar aprendendo, os outros módulos complementam o que você praticou:

- 🖥️ **[Módulo do VS Code](../../vscode/)** — explore o modo agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Módulo do CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal usando os modos Plan e Autopilot, agent skills, um agente de QA personalizado, MCP do Playwright, Agent Merge e uma integração opcional com `/delegate` para o cloud agent.

No seu próprio repositório, experimente estas ideias:

- Atribuir ao cloud agent uma issue de refatoração ou cobertura de testes.
- Criar um agente personalizado para outro domínio.
- Configurar `copilot-setup-steps.yml` para outra pilha de tecnologia.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do Copilot cloud agent][cloud-agent-best-practices]
- [Configurar o firewall do Copilot cloud agent][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
