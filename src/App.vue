<script setup lang="ts">
import { ref, watch, onMounted } from 'vue'
import ListaTarefas from './components/ListaTarefas.vue'

interface Tarefa {
  id: number
  texto: string
  concluida: boolean
}

interface Lista {
  id: number
  titulo: string
  tarefas: Tarefa[]
}

const listas = ref<Lista[]>([])
const tituloLista = ref("")

function criarLista() {
  if (tituloLista.value.trim() === "") return

  listas.value.push({
    id: Date.now(),
    titulo: tituloLista.value,
    tarefas: []
  })

  tituloLista.value = ""
}

function excluirLista(id: number) {
  listas.value = listas.value.filter(lista => lista.id !== id)
}

onMounted(() => {
  const dados = localStorage.getItem("listas")

  if (dados) {
    listas.value = JSON.parse(dados)
  }
})

watch(
  listas,
  () => {
    localStorage.setItem("listas", JSON.stringify(listas.value))
  },
  { deep: true }
)
</script>

<template>
  <div class="container">

    <h1>📝 Minhas Listas de Tarefas</h1>

    <div class="nova-lista">

      <input
        v-model="tituloLista"
        placeholder="Digite o título da lista"
      />

      <button @click="criarLista">
        Criar Lista
      </button>

    </div>

    <ListaTarefas
      v-for="lista in listas"
      :key="lista.id"
      :lista="lista"
      @excluir="excluirLista"
    />

  </div>
</template>

<style scoped>

.container{
  max-width:1000px;
  margin:auto;
  padding:30px;
}

h1{
  text-align:center;
  margin-bottom:30px;
  color:#2c3e50;
}

.nova-lista{
  display:flex;
  gap:10px;
  margin-bottom:30px;
}

input{
  flex:1;
  padding:12px;
  border:1px solid #ccc;
  border-radius:8px;
  font-size:16px;
}

button{
  padding:12px 20px;
  background:#42b883;
  color:white;
  border:none;
  border-radius:8px;
  cursor:pointer;
  font-size:16px;
  transition:.3s;
}

button:hover{
  background:#36966d;
}

body{
  background:#f5f7fa;
}

</style>
