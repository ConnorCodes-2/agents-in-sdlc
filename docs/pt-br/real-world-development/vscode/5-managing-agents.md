---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

Quando você coloca o GitHub Copilot no modo agente, ele trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece na sua máquina em tempo real, o Copilot Chat no VS Code mostra cada chamada de ferramenta e cada edição de arquivo à medida que ocorrem. Você pode revisar cada diff na conversa, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts complementares para refinar ou ampliar o trabalho sem sair do editor.

Neste exercício, você vai:

- acompanhar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições de arquivos propostas.
- orientar o agente durante a sessão para adicionar um botão de modo claro junto ao modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design faz uma solicitação adicional: também quer oferecer um botão de modo claro para usuários que consideram o tema padrão escuro demais. Em vez de começar uma conversa do zero, você orientará a sessão existente para ampliar o trabalho.

## Acompanhe o progresso do Copilot

A visualização Copilot Chat mostra cada etapa do agente: os arquivos que explora, os comandos que executa e as edições que propõe. Cada edição de arquivo aparece na conversa com um diff e controles **Keep**/**Undo**.

1. Volte à visualização Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra a conversa para acompanhar cada chamada de ferramenta. Você verá leituras e edições de arquivos e comandos do terminal que ele solicitará executar.
3. Quando o agente propuser uma edição de arquivo, o diff aparecerá na conversa:
    - Selecione **Keep** para aceitar a alteração.
    - Selecione **Undo** para desfazer a alteração e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Files changed** na parte superior do chat para ver todos os arquivos modificados pelo agente nesta sessão.

> [!NOTE]
> Se uma chamada de ferramenta solicitar permissão (por exemplo, para executar um comando do terminal), o agente fará uma pausa e exibirá uma solicitação **Allow**. Aprove somente os comandos com os quais se sentir confortável.

5. Aguarde o agente concluir a implementação do modo de alto contraste. Você verá uma mensagem final com o resumo das alterações.

## Oriente o agente para adicionar o modo claro

Agora que o modo de alto contraste está pronto, amplie a mesma conversa para adicionar um botão de modo claro. Como você está continuando a mesma sessão de chat, o agente mantém todo o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilo e o padrão de armazenamento local configurado.

1. No campo de entrada do Copilot Chat, envie o seguinte prompt complementar:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. O agente continuará de onde parou, explorando as alterações existentes e propondo adições para o modo claro.
3. Revise cada nova edição à medida que aparecer, como fez para o modo de alto contraste. Use **Keep** ou **Undo** em cada diff.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar a interface dos botões de um jeito que você não quer —, pare-o com o botão **Stop** ao lado do campo de entrada e envie uma mensagem complementar para esclarecer a solicitação.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts complementares para pedir ajustes ("simplifique o CSS — use variáveis CSS em vez de duplicar seletores"), corrigir o rumo ("não adicione um terceiro botão; inclua o modo claro no painel de configurações existente") ou ampliar o escopo ("adicione também atalhos de teclado para os dois botões").

## Revise o conjunto de alterações

Antes de fazer commit do trabalho, revise rapidamente tudo o que o agente alterou.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações nos componentes Astro, nos estilos e em testes relacionados.
3. Abra alguns dos arquivos alterados e examine os diffs. Confirme que os padrões de acessibilidade do agente personalizado estão presentes, incluindo atributos ARIA, navegação pelo teclado, HTML semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações no painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo agente do Copilot Chat para acompanhar e orientar uma sessão em andamento: revisou cada edição de arquivo à medida que era feita, aceitou ou rejeitou alterações e enviou prompts complementares para ampliar o trabalho sem perder o contexto.

Nesta lição, você explorou:

- como acompanhar o trabalho de um agente em tempo real.
- como aceitar ou rejeitar edições individuais de arquivos.
- como orientar uma sessão em andamento com prompts complementares.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade em uma pull request e revisará as alterações produzidas pelo agente personalizado local ao longo deste laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Fazer iterações no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
