<script setup>
import { ref, computed } from 'vue'

const tarefas = ref([
  { id: '1', titulo: 'Ler Vue', concluida: false },
  { id: '2', titulo: 'Praticar CSS', concluida: true },
  { id: '3', titulo: 'Revisar Vue', concluida: true },
])

const titulo = ref('')
const erro = ref('')
const busca = ref('')
const situacao = ref('todas') // 'todas' | 'pendentes' | 'concluidas'

// Desafio 1: contador derivado
const pendentes = computed(() =>
  tarefas.value.filter((t) => !t.concluida).length
)

// Desafios 2, 3, 4 e 5: uma única lista derivada, com busca E situação
const tarefasFiltradas = computed(() => {
  const termo = busca.value.trim().toLowerCase()

  return tarefas.value.filter((t) => {
    const passaBusca = t.titulo.toLowerCase().includes(termo)
    const passaSituacao =
      situacao.value === 'todas' ||
      (situacao.value === 'pendentes' && !t.concluida) ||
      (situacao.value === 'concluidas' && t.concluida)

    return passaBusca && passaSituacao
  })
})

function enviar() {
  const texto = titulo.value.trim()
  if (!texto) {
    erro.value = 'Informe um título.'
    return
  }
  tarefas.value.push({ id: crypto.randomUUID(), titulo: texto, concluida: false })
  titulo.value = ''
  erro.value = ''
}

function alternar(id) {
  const tarefa = tarefas.value.find((t) => t.id === id)
  if (tarefa) tarefa.concluida = !tarefa.concluida
}

function remover(id) {
  tarefas.value = tarefas.value.filter((t) => t.id !== id)
}

// Desafio 6: limpa só os filtros, não os dados
function limparFiltros() {
  busca.value = ''
  situacao.value = 'todas'
}
</script>

<template>
<main>
  <h1>TaskLab</h1>
  <p>Cadastro, busca e filtros</p>

  <form @submit.prevent="enviar" novalidate>
    <label for="nova-tarefa">Nova tarefa</label>
    <div class="linha">
      <input id="nova-tarefa" v-model.trim="titulo" aria-describedby="erro-form">
      <button type="submit">Adicionar</button>
    </div>
    <p id="erro-form" class="erro" role="alert">{{ erro }}</p>
  </form>

  <!-- Filtros: fora do form, para o Enter não enviar o cadastro -->
  <div class="filtros">
    <input v-model="busca" placeholder="Buscar tarefa" aria-label="Buscar tarefa">
    <select v-model="situacao" aria-label="Filtrar por situação">
      <option value="todas">Todas</option>
      <option value="pendentes">Pendentes</option>
      <option value="concluidas">Concluídas</option>
    </select>
    <button type="button" class="secundario" @click="limparFiltros">Limpar filtros</button>
  </div>

  <p role="status">{{ pendentes }} {{ pendentes === 1 ? 'pendente' : 'pendentes' }}</p>

  <!-- A tela mostra a lista filtrada, nunca a original -->
  <ul>
    <li v-for="tarefa in tarefasFiltradas" :key="tarefa.id" class="item" :class="{ concluida: tarefa.concluida }">
      <div>
        <strong>{{ tarefa.titulo }}</strong>
        <span class="situacao">{{ tarefa.concluida ? 'Concluída' : 'Pendente' }}</span>
      </div>
      <div class="acoes">
        <button type="button" @click="alternar(tarefa.id)">{{ tarefa.concluida ? 'Reabrir' : 'Concluir' }}</button>
        <button type="button" @click="remover(tarefa.id)">Remover</button>
      </div>
    </li>
  </ul>

  <!-- Desafio 7: cada mensagem observa uma lista; nunca aparecem juntas -->
  <p v-if="tarefas.length === 0">Nenhuma tarefa cadastrada.</p>
  <p v-else-if="tarefasFiltradas.length === 0">Nenhuma tarefa corresponde aos filtros.</p>
</main>
</template>

<style scoped>
main { width: min(720px, 92%); margin: 2rem auto; font-family: system-ui, sans-serif; color: #18342b; }
button, input, select { font: inherit; padding: .6rem; border: 1px solid #768b80; border-radius: .4rem; }
button { cursor: pointer; color: #fff; background: #176b50; }
button.secundario { color: #18342b; background: #fff; }
form { background: #fff; border: 1px solid #bdcfc5; border-radius: .6rem; padding: 1rem; margin: 1rem 0; }
.linha { display: flex; gap: .6rem; flex-wrap: wrap; }
.linha input { flex: 1; min-width: 150px; }
.filtros { display: flex; gap: .6rem; flex-wrap: wrap; align-items: center; margin-bottom: 1rem; }
.filtros input { flex: 1; min-width: 150px; }
.erro { color: #a12525; min-height: 1.2rem; }
ul { list-style: none; padding: 0; }
.item { background: #fff; padding: 1rem; margin: .7rem 0; border: 1px solid #bdcfc5; border-radius: .5rem; display: flex; flex-wrap: wrap; justify-content: space-between; gap: 1rem; }
.item.concluida strong { text-decoration: line-through; }
.situacao { display: block; font-size: .9rem; margin-top: .3rem; }
.acoes { display: flex; gap: .5rem; align-items: center; }
</style>