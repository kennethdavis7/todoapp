<script setup>
import { computed, onMounted, ref, watch } from "vue";
import Card from "./components/Todos.vue";
import { initFlowbite } from "flowbite";

const data = ref([
  {
    id: 1,
    todo: "Coding",
  },
  {
    id: 2,
    todo: "Olahraga",
  },
]);

const searchValue = ref("");

const filteredData = computed(() => {
  const query = searchValue.value.trim().toLowerCase();

  return data.value.filter((todo) => {
    return todo.todo.toLowerCase().includes(query);
  });
});

// debounce

const deleteTodo = (todo) => {
  data.value = data.value.filter((dataTodo) => dataTodo.id !== todo.id);
};

const updateTodo = (todoValue, todoId) => {
  console.log(todoValue, todoId);
  data.value = data.value.map((dataTodo) => {
    if (todoId == dataTodo.id) {
      return { ...dataTodo, todo: todoValue };
    } else {
      return dataTodo;
    }
  });
};

const addTodo = (todoValue) => {
  data.value.push({
    id: data.value.length + 1,
    todo: todoValue,
  });
};

onMounted(() => {
  initFlowbite();
});
</script>

<template>
  <div class="container mx-auto flex justify-center mt-20">
    <Card
      :data="filteredData"
      @deleteTodo="deleteTodo"
      @updateTodo="updateTodo"
      @addTodo="addTodo"
      v-model:search="searchValue"
    />
  </div>
</template>
