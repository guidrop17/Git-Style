# Git Style Guide

## Branching

- Escolha nomes curtos e descritivos:

```
# GOOD
$ git checkout -b feature/oauth-migtration

# BAD
$git checkout -b login_fix
```

- Identificadores de tickets correspondentes em um serviço externo (por exemplo, um problema do GitHub) também são bons candidatos para uso em nomes de ramificações. Por exemplo:

```
# GitHub issue #23

$ git checkout -b fix/issue-15
```

- Use barras para separar palavras
- Quando várias pessoas estão trabalhando no mesmo recurso, pode ser conveniente ter ramificações de recursos pessoais e uma ramificação de recursos para toda a equipe. Use a seguinte convenção de nome:

```
$ git checkout -b feature-a/master # team-wide branch

$ git checkout -b feature-a/joao # Joao's personal branch

$ git checkout -b feature-a/maria # Maria's personal branch
```

- Merger à vontade as ramificações pessoais para a ramificação de toda a equipe. Eventualmente, a ramificação de toda a equipe será mesclada em `master`.
- Exclua sua ramificação do repositório upstream após a mesclagem, a menos que haja um motivo específico para não fazê-lo.
- _Dica_: Use o seguinte comando enquanto estiver em `master`, para listar ramificações mescladas:
```
$ git branch --merged | grep -v `\\*`
```

## Commits

- **Cada commit deve ser uma única mudança lógica.** Não faça várias mudanças lógicas em um commit. Por exemplo, se um patch corrigir um bug e otimizar o desempenho de um recurso, divida-o em dois commits separados.

- _Dica_: Use `git add -p` para preparar interativamente partes específicas dos arquivos modificados.

- **Não divida uma única alteração lógica em vários commits.** Por exemplo, a implementação de um recurso e os testes correspondentes devem estar no mesmo commit.

- **Commit _early_ e _often_.** Commits pequenos e autocontidos são mais fáceis de entender e reverter quando algo dá errado.

- **Os commits devem ser ordenados _logicamente_ .** Por exemplo, se o commit X depende das mudanças feitas no commit Y, então o commit Y deve vir antes do commit X.

### Mensagens de confirmação

A mensagem de confirmação consiste em três partes distintas separadas por uma linha em branco: o título, o corpo (opcional) e o rodapé (opcional). O layout fica assim:
```
feat: ensinar como escrever mensagem de commit

Uma explicação adicional é uma boa prática que recomendamos que você siga.

Resolva: #1234
```
O **título** é dividido em duas partes: o **tipo** e o **assunto**.

### Model

O tipo está contido no título e pode ser um dos seguintes:

- **feat:** um novo recurso
- **fix:** uma correção de bug
- **docs:** alterações na documentação
- **style:** formatação, falta de ponto e vírgula, etc.; sem alteração de código
- **refactor:** refatorar o código de produção
- **test:** adição de testes, teste de refatoração; sem alteração de código de produção
- **chore:** atualizar tarefas de compilação, configurações do gerenciador de pacotes, etc.; sem alteração de código de produção

### Subject

Os assuntos não devem ter mais de 50 caracteres, devem começar com letra maiúscula e não terminar com ponto. Use o modo imperativo na linha de assunto. Separe o assunto do corpo (quando houver) com uma linha em branco.

Exemplo:

```
# GOOD
refactor: Refactor subsystem X for readability
fix: Remove deprecated methods

# BAD
Fixed bug with Y
More fixes from broken stuff
```

### Body

Como nem todos os commits são complexos o suficiente para garantir um corpo, ele só deve estar presente na mensagem ao fornecer o contexto e economizar tempo dos colegas e futuros contribuidores. Normalmente você vai querer fornecer mais contexto quando seu commit tiver um desses tipos: `feat`, `refactor` e `fix`.

Use o corpo para explicar o que e o porquê de um commit, não o como – o código deve fazer isso.

Ao escrever uma mensagem de commit, pense no que você precisaria saber se você executasse o commit daqui a um ano.

Enrole o corpo em 72 caracteres por linha.

### Footer

O rodapé também é opcional e é usado para rastreamento/referência a problemas:

Exemplo:

```

Resolves: #123

See also: #456, #789

```

## Merging

- **Não reescreva o histórico publicado.** O histórico do repositório é valioso por si só e é muito importante poder contar o que realmente aconteceu. Alterar o histórico publicado é uma fonte comum de problemas para quem trabalha no projeto.
- No entanto, há casos em que reescrever a história é legítimo. São quando:
  - Você é o único trabalhando na filial e ela não está sendo revisada.
  - Você quer arrumar seu branch (ex. squash commits) e/ou rebase ele no `master` para fazer o merge mais tarde.

- Dito isso, nunca reescreva o histórico do branch `master` ou qualquer outro branch especial (ou seja, usado por servidores de produção ou CI).

- Mantenha a história limpa e simples. Pouco antes de você mesclar sua ramificação:
  - Certifique-se de que está em conformidade com o guia de estilo e execute as ações necessárias, caso não esteja (squash/reordenar commits, reformular mensagens etc.)
  - Rebase para o branch que será mesclado

- Isso resulta em um branch que pode ser aplicado diretamente no final do branch `master` e resulta em um histórico muito simples.

- Esta estratégia é mais adequada para projetos com filiais de curta duração. Caso contrário, pode ser melhor ocasionalmente mesclar o branch `master` em vez de fazer o rebase nele.

- Se sua ramificação incluir mais de um commit, não faça merge com fast-forward:

```
# GOOD - ensures that a merge commit is created

$ git merge --no-ff my-branch

# BAD
  
$ git merge my-branch
```

- **NUNCA** faça commit de algo como `Fix linter` ou `Fix tests`. Essas `correções` devem ser compactadas nos commits que originaram a mudança.

- **NUNCA** esmague completamente uma ramificação antes de mesclá-la, a menos que toda a ramificação (todos os commits) esteja relacionada a uma única alteração lógica.

- **NUNCA** use git merge master em um branch. **SEMPRE** use o git rebase master, depois force-push, espere o CI limpar e só então mescle no master.

- **CONVENÇÃO** é sempre usar a interface web do Github para mesclar no master e **NUNCA** usar o Git na linha de comando (ou seja, git checkout master; git merge --no-ff branch; git push). Isso evita confusão e esquecimento de algumas coisas realmente importantes, como a mesclagem sem avanço rápido.

## Outros

- Existem vários fluxos de trabalho e cada um tem seus pontos fortes e fracos. Se um fluxo de trabalho se adapta ao seu caso, depende da equipe, do projeto e dos seus procedimentos de desenvolvimento.

Dito isso, o mais importante é realmente escolher um fluxo de trabalho e mantê-lo.

- _Ser consistente_. Isso está relacionado ao fluxo de trabalho, mas também se expande para coisas como mensagens de confirmação, nomes de ramificações e tags. Ter um estilo consistente em todo o repositório torna mais fácil para todos os contribuidores entender o que está acontecendo olhando para o log, uma mensagem de commit, etc.
- _Teste antes de dar "push"_. Não empurre o trabalho pela metade.
