<script setup>
const props = defineProps(['tarefas'])
</script>

<template>
    <ul v-if="props.tarefas.length > 0" class="list-group mt-4">
        <li class="list-group-item" v-for="tarefa in props.tarefas">
            <!-- Adicionando o evento @change para alterar a situação da tarefa -->
            <input @change="evento => tarefa.finalizada = evento.target.checked" :checked="tarefa.finalizada"
                :id="tarefa.titulo" type="checkbox">
            <label :for="tarefa.titulo" :class="{ done: tarefa.finalizada }" class="ms-3">
                {{ tarefa.titulo }}
            </label>
        </li>
    </ul>
    <p v-else class="mt-4">Você não tem nenhuma tarefa!</p>
</template>

<style scoped>
p {
    color: white;
    padding: 15px;
    border-radius: 15px;
    font-weight: bold;
    background-color: darkgray;
}
</style>

<style scoped>
.done {
    text-decoration: line-through
}
</style>
