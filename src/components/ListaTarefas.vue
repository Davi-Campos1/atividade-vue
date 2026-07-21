<script setup lang="ts">
import { ref } from 'vue'

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

const props = defineProps<{
  lista: Lista
}>()

const emit = defineEmits<{
  (e: 'excluir', id: number): void
}>()

const novaTarefa = ref("")

function adicionarTarefa() {
  if (novaTarefa.value.trim() === "") return

  props.lista.tarefas.push({
    id: Date.now(),
    texto: novaTarefa.value,
    concluida: false
  })

  novaTarefa.value = ""
}

function removerTarefa(id: number) {
  props.lista.tarefas = props.lista.tarefas.filter(
    tarefa => tarefa.id !== id
  )
}

function tarefasConcluidas() {
  return props.lista.tarefas.filter(t => t.concluida).length
}
</script>

<template>

<div class="card">

    <div class="topo">

        <h2>
            {{ lista.titulo }}
        </h2>

        <button
            class="btnExcluirLista"
            @click="emit('excluir', lista.id)"
        >
            🗑
        </button>

    </div>

    <div class="novaTarefa">

        <input
            v-model="novaTarefa"
            placeholder="Digite uma tarefa"
            @keyup.enter="adicionarTarefa"
        >

        <button @click="adicionarTarefa">
            Adicionar
        </button>

    </div>

    <p class="contador">
        {{ tarefasConcluidas() }} de {{ lista.tarefas.length }} concluídas
    </p>

    <ul>

        <li
            v-for="tarefa in lista.tarefas"
            :key="tarefa.id"
        >

            <label>

                <input
                    type="checkbox"
                    v-model="tarefa.concluida"
                >

                <span
                    :class="{feito:tarefa.concluida}"
                >
                    {{ tarefa.texto }}
                </span>

            </label>

            <button
                class="btnRemover"
                @click="removerTarefa(tarefa.id)"
            >
                ❌
            </button>

        </li>

    </ul>

</div>

</template>

<style scoped>

.card{

    background:white;

    border-radius:15px;

    padding:20px;

    margin-bottom:25px;

    box-shadow:0 4px 12px rgba(0,0,0,.1);

}

.topo{

    display:flex;

    justify-content:space-between;

    align-items:center;

    margin-bottom:20px;

}

.topo h2{

    color:#2c3e50;

}

.novaTarefa{

    display:flex;

    gap:10px;

    margin-bottom:15px;

}

.novaTarefa input{

    flex:1;

    padding:10px;

    border-radius:8px;

    border:1px solid #ccc;

}

.novaTarefa button{

    background:#42b883;

    color:white;

    border:none;

    padding:10px 18px;

    border-radius:8px;

    cursor:pointer;

}

.novaTarefa button:hover{

    background:#36966d;

}

ul{

    list-style:none;

    padding:0;

}

li{

    display:flex;

    justify-content:space-between;

    align-items:center;

    padding:10px 0;

    border-bottom:1px solid #eee;

}

label{

    display:flex;

    gap:10px;

    align-items:center;

}

.feito{

    text-decoration:line-through;

    color:gray;

}

.btnExcluirLista{

    background:#e74c3c;

    color:white;

    border:none;

    padding:8px 12px;

    border-radius:8px;

    cursor:pointer;

}

.btnExcluirLista:hover{

    background:#c0392b;

}

.btnRemover{

    background:transparent;

    border:none;

    cursor:pointer;

    font-size:18px;

}

.contador{

    margin-bottom:10px;

    color:#666;

    font-size:14px;

}

</style>
