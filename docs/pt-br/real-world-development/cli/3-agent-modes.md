---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use Plan para acordar uma abordagem, Autopilot para criar a filtragem a partir de uma issue e o modo Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando uma pequena funcionalidade ao projeto. Mas alterações maiores exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi criado para trabalhar com o fluxo existente de uma organização, garantindo que criemos as coisas certas da maneira certa. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento orientado por agentes: começará usando uma issue para gerar uma nova funcionalidade e verificará se o código é válido, se a funcionalidade se comporta como esperado e se, por fim, ela é mesclada com sucesso ao projeto.

> [!NOTE]
> Você usará a mesma conversa e branch ao longo do fluxo de trabalho da funcionalidade. Em geral, você teria branches ou PRs diferentes para os vários tipos de arquivos, mas vamos simplificar para nos concentrarmos nos conceitos principais.

Nesta lição, você vai:

- iniciar uma nova conversa do Copilot a partir de uma issue do GitHub.
- definir requisitos no modo Plan.
- implementar a nova funcionalidade usando o modo Autopilot.
- revisar o código.
- validar a funcionalidade manualmente em um navegador encaminhado.

Ao longo do trabalho nessa funcionalidade, você atualizará as instruções do repositório, personalizará a skill existente `quality-checks`, adicionará validação por MCP, criará um agente de QA e abrirá a PR da funcionalidade.

## Cenário

O catálogo do Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve a funcionalidade, mas detalhes como combinar categorias precisam ser acordados antes de escrever o código. Você usará o modo Plan para resolver essas decisões e, em seguida, autorizará uma implementação com escopo definido usando o Autopilot.

## Contexto

Introduzir agentes de codificação com IA no fluxo de desenvolvimento não muda os fundamentos. Na verdade, eles se tornam ainda mais importantes! O fluxo da maioria dos desenvolvedores é semelhante a:

1. Começar por uma issue registrada que descreve o que precisa ser feito.
2. Criar um plano do que precisa ser desenvolvido.
3. Desenvolver e revisar o código.
4. Executar os testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar uma pull request (PR).
7. Depois da revisão do código e do sucesso do processo de integração contínua, mesclar o código.

> [!NOTE]
> Os detalhes exatos variam conforme a equipe e a organização, mas a maioria segue uma variação do tema acima.

Ao seguir essa abordagem padrão, você garante que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de avaliação que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham juntos. O agente sugere alterações e aguarda sua resposta antes de continuar.
- **Plan**: o agente cria um plano primeiro e não pode editar os arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma — escreve código, executa testes e faz iterações até concluir a tarefa.

Comece no modo Plan, revise o plano e, em seguida, use o Autopilot para implementá-lo.

## Comece pela issue

Antes de começar a filtragem, volte ao seu Codespace e verifique se o repositório e o terminal estão prontos.

1. Volte ao seu Codespace. Se ele estiver parado, reinicie-o antes de continuar.
2. Confirme que a PR de avaliação com estrelas foi mesclada.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as teclas de seta para destacar a issue intitulada **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que o prompt agora começa com `#7` (ou um número parecido). O caractere `#` permite incluir no contexto uma issue ou pull request (PR) do GitHub.

## Planeje a funcionalidade de filtragem

O planejamento permite definir a abordagem de implementação de uma funcionalidade ou execução de uma tarefa antes de delegá-la ao Copilot. É sempre uma boa ideia dedicar algum tempo ao planejamento de tarefas complexas. Vamos mudar para o modo Plan e pedir ao Copilot que crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme que o indicador abaixo do prompt exibe **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, digite o seguinte prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot começará a criar o plano! Primeiro, ele explorará o projeto e, em seguida, determinará a melhor abordagem.

3. Durante o processo, o Copilot poderá fazer perguntas sobre o funcionamento dos recursos de filtragem. Responda conforme suas preferências. Não há respostas certas ou erradas!
4. Quando o plano estiver concluído, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Role para cima e para baixo para revisar o plano.
6. Peça ao Copilot que revise qualquer parte do plano que não corresponda às suas decisões.

## Aprove o Autopilot

Com o plano escrito e revisado, é hora de implementá-lo! Vamos deixar o Copilot trabalhar usando o Autopilot.

O Autopilot permite que o Copilot faça iterações sobre o problema até considerar a tarefa concluída.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção com nome semelhante na versão instalada.
2. Confirme que o indicador abaixo do prompt exibe **Autopilot**.
3. Acompanhe o Copilot enquanto ele segue o plano definido, gera código e executa testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, então revise o plano primeiro. Se o Copilot informar que há dependências ausentes ou conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revise e verifique a implementação

Depois que o código for gerado, ele precisa ser revisado antes da mesclagem, como qualquer outro código. Vamos revisar o código e executar o site para garantir que tudo esteja funcionando.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador de modo não exibe mais **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com a issue e as decisões tomadas durante o planejamento.
4. Depois de revisar o código, pressione <kbd>Esc</kbd> para sair da visualização de diff.
5. Revise a saída das verificações do projeto e peça ao Copilot que resolva eventuais falhas.

## Explore a nova funcionalidade

Tudo certo com o código — mas será que ele funciona? Vamos iniciar o aplicativo como antes e abrir o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste a filtragem por categoria, por editora e as combinações acordadas no plano.
4. Confirme que o comportamento de redefinição e de resultados vazios corresponde à issue e às suas decisões.
5. Volte ao seu Codespace e peça ao Copilot que pare o servidor de desenvolvimento iniciado.

## Resumo e próximos passos

Você usou diferentes modos de conversa para desenvolver e revisar uma funcionalidade. Nesta lição, você:

- iniciou uma nova conversa do Copilot a partir de uma issue do GitHub.
- definiu requisitos no modo Plan.
- implementou a nova funcionalidade usando o modo Autopilot.
- revisou o código.
- validou manualmente a funcionalidade em um navegador encaminhado.

Agora vamos nos aprofundar na geração de código e garantir que ela siga as práticas documentadas, [usando instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
