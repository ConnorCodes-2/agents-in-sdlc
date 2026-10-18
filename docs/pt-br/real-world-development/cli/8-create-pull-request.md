---
title: "Lição 8 - Crie e mescle a PR da funcionalidade"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; em seguida, crie uma PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações das instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma branch. Agora é hora de revisá-los em conjunto e abrir uma pull request. Você mesmo mesclou a pull request (PR) de avaliação com estrelas; desta vez, permitirá que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos a funcionalidade, as atualizações das instruções, a atualização da skill e o agente de QA em algumas PRs separadas. Para agilizar o workshop, você manteve todo o fluxo de filtragem e qualidade em uma conversa e branch, e incluirá todo esse trabalho nesta PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionar toda a alteração da funcionalidade e as evidências de verificação.
- criar a PR da filtragem.
- habilitar o Agent Merge somente após a revisão e confirmar que a PR foi mesclada.

## Cenário

Ao longo do fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar uma funcionalidade. Agora a Tailspin Toys quer automatizar o trabalho restante da PR, mantendo a autorização da mesclagem sob o controle do desenvolvedor.

## Apresentação do Agent Merge

O **Agent Merge** automatiza o trabalho restante necessário para mesclar uma pull request. Quando você o habilita, o Copilot resolve os impedimentos da PR — corrige verificações de integração contínua (CI) com falha, responde a comentários de revisão e faz rebase quando necessário — e, em seguida, habilita a mesclagem automática do GitHub quando o repositório permite.

Até agora, você mesmo selecionou **Merge pull request**. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que está pronto antes de habilitar o Agent Merge.

## Use o Agent Merge para gerenciar a PR

Com todo o código criado, vamos revisá-lo em conjunto, criar a PR e deixar que o Agent Merge gerencie o restante do processo.

1. Volte ao seu Codespace.
2. Abra a caixa de diálogo do agente digitando `/agent`.
3. Selecione **Default** na lista de opções e pressione <kbd>Enter</kbd>.
4. Crie uma nova PR usando o comando `/pr create`.
5. Ative o agent merge digitando `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua da PR. Quando tudo for aprovado, ele fará a mesclagem.
7. Confirme que você recebeu uma mensagem do Copilot semelhante a "PR #14 was squash-merged successfully."

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de mesclagem, configurações do repositório nem permissões ausentes. Se houver um bloqueio, leia o motivo informado e conclua manualmente a mesclagem revisada quando o repositório permitir.

## Resumo e próximos passos

Você automatizou várias partes do processo de desenvolvimento, incluindo a geração, o teste e a validação de código e agora o processo de pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionou toda a alteração da funcionalidade e as evidências de verificação.
- criou a PR da filtragem.
- habilitou o Agent Merge somente após a revisão e confirmou que a PR foi mesclada.

Na próxima lição, você vai [explorar outros comandos de barra úteis do Copilot CLI][next-lesson] para gerenciar contexto, modelos e compartilhamento, além de delegar tarefas opcionais na nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
