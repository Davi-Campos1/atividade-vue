<script setup lang="ts">
interface Tarefa {
  id: number
  texto: string
  concluida: boolean
}

defineProps<{
  tarefa: Tarefa
}>()

const emit = defineEmits<{
  (e: 'remover', id: number): void
}>()
</script>

<template>
  <li class="item">

    <label class="texto">

      <input
        type="checkbox"
        v-model="tarefa.concluida"
      />

      <span :class="{ concluida: tarefa.concluida }">
        {{ tarefa.texto }}
      </span>

    </label>

    <button
      class="btnExcluir"
      @click="emit('remover', tarefa.id)"
    >
      🗑
    </button>

  </li>
</template>

<style scoped>

.item{
  display:flex;
  justify-content:space-between;
  align-items:center;
  padding:10px;
  border-bottom:1px solid #ddd;
}

.texto{
  display:flex;
  align-items:center;
  gap:10px;
  font-size:16px;
}

.concluida{
  text-decoration:line-through;
  color:#888;
}

.btnExcluir{
  background:#e74c3c;
  color:white;
  border:none;
  border-radius:6px;
  padding:6px 10px;
  cursor:pointer;
}

.btnExcluir:hover{
  background:#c0392b;
}

</style>
