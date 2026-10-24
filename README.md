# TaskLab — Motor de Busca Combinada (Sábado 1)

Aluno: Luann

**Teste online:** [link do Playground]

## Como rodar localmente
npm install
npm run dev

## Critérios atendidos
- **Contador:** `computed` de pendentes, atualiza sozinho ao criar, concluir, reabrir ou remover.
- **Busca:** ignora maiúsculas; campo vazio mostra todas as tarefas.
- **Situação:** `<select>` com Todas, Pendentes e Concluídas.
- **Combinação:** um único `computed` (`tarefasFiltradas`) aplica busca E situação.
- **Integridade:** os filtros nunca alteram `tarefas`, só a lista exibida.
- **Limpar filtros:** um clique reseta busca e seletor, sem afetar os dados.
- **Mensagens de vazio:** uma observa `tarefas`, a outra `tarefasFiltradas`; nunca aparecem juntas.

O código do exercício está em `src/App.vue`.
