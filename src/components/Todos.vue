<script setup>
import Todo from "./Todo.vue";
import { ref } from "vue";
import { TransitionRoot, TransitionChild, Dialog, DialogPanel } from "@headlessui/vue";

const props = defineProps(["data"]);
const emits = defineEmits(["searchTodo", "deleteTodo", "updateTodo", "addTodo"]);
const searchModel = defineModel("search");

const editFormTodo = ref([]);

const isOpen = ref(false);
const todoInput = ref(null);

const setIsOpen = (value) => {
  isOpen.value = value;
};

const deleteTodo = (todo) => {
  emits("deleteTodo", todo);
};

const updateTodo = (todoValue, todoId) => {
  isOpen.value = true;
  editFormTodo.value = [todoValue, todoId];
};

const onUpdate = () => {
  emits("updateTodo", editFormTodo.value[0], editFormTodo.value[1]);
  setIsOpen(false);
};

const addTodo = (event) => {
  if (!event.target.elements.addTodo.value) return;
  emits("addTodo", event.target.elements.addTodo.value);
};
</script>

<template>
  <div class="card w-96 bg-base-100 shadow-xl">
    <div class="card-body">
      <div class="flex w-full justify-between header">
        <h1 class="card-title text-3xl mb-4">To Do List</h1>
        <form class="w-4/12" @submit.prevent>
          <input
            type="text"
            name="searchTodo"
            id="searchTodo"
            placeholder="Seach here"
            v-model="searchModel"
            class="input input-bordered w-full mr-2 input-sm max-w-xs"
          />
        </form>
      </div>

      <form class="flex justify-between w-full" @submit.prevent="addTodo">
        <input
          type="text"
          name="addTodo"
          id="addTodo"
          placeholder="Type here"
          class="input input-bordered w-10/12 mr-2 input-sm max-w-xs"
        />
        <button type="submit" class="btn btn-sm btn-outline w-2/12">Add</button>
      </form>

      <Todo
        v-for="data in props.data"
        :key="data.id"
        :data="data"
        @delete="deleteTodo"
        @update="(value) => updateTodo(value, data.id)"
      />
    </div>
  </div>

  <TransitionRoot
    :show="isOpen"
    as="template"
    enter="duration-300 ease-out"
    enter-from="opacity-0"
    enter-to="opacity-100"
    leave="duration-200 ease-in"
    leave-from="opacity-100"
    leave-to="opacity-0"
  >
    <Dialog :initialFocus="todoInput" :open="isOpen" @close="setIsOpen">
      <TransitionChild
        enter="duration-300 ease-out"
        enter-from="opacity-0"
        enter-to="opacity-100"
        leave="duration-200 ease-in"
        leave-from="opacity-100"
        leave-to="opacity-0"
      >
        <div class="fixed inset-0 bg-black/30" aria-hidden="true" />
      </TransitionChild>
      <div class="fixed inset-0 w-full overflow-y-auto">
        <div class="flex w-full min-h-full items-center justify-center p-4">
          <div class="w-3/12">
            <TransitionChild
              enter="duration-300 ease-out"
              enter-from="opacity-0 scale-95"
              enter-to="opacity-100 scale-100"
              leave="duration-200 ease-in"
              leave-from="opacity-100 scale-100"
              leave-to="opacity-0 scale-95"
            >
              <DialogPanel class="w-full rounded bg-white">
                <div class="flex items-center justify-between pt-5 px-5 pb-0 rounded-t">
                  <button
                    type="button"
                    class="text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm w-5 h-5 ms-auto inline-flex justify-center items-center dark:hover:bg-gray-600 dark:hover:text-white"
                    @click="setIsOpen(false)"
                  >
                    <svg
                      class="w-3 h-3"
                      aria-hidden="true"
                      xmlns="http://www.w3.org/2000/svg"
                      fill="none"
                      viewBox="0 0 14 14"
                    >
                      <path
                        stroke="currentColor"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="m1 1 6 6m0 0 6 6M7 7l6-6M7 7l-6 6"
                      />
                    </svg>
                    <span class="sr-only">Close modal</span>
                  </button>
                </div>
                <form @submit.prevent="onUpdate" class="px-4 md:px-5 pb-4 md:pb-4">
                  <div class="grid gap-4 mb-4 grid-cols-2">
                    <div class="col-span-2">
                      <label
                        for="name"
                        class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
                        >Todo</label
                      >
                      <input
                        type="text"
                        name="todo"
                        id="todo"
                        class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white dark:focus:ring-primary-500 dark:focus:border-primary-500"
                        placeholder="Type product todo"
                        required=""
                        v-model="editFormTodo[0]"
                        ref="todoInput"
                      />
                      <input
                        type="hidden"
                        name="id"
                        id="id"
                        class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white dark:focus:ring-primary-500 dark:focus:border-primary-500"
                        placeholder="Type product todo"
                        required=""
                        :value="editFormTodo[1]"
                      />
                    </div>
                  </div>
                  <button
                    type="submit"
                    class="text-white inline-flex items-center bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
                    @click="onUpdate"
                  >
                    Update
                  </button>
                </form>
              </DialogPanel>
            </TransitionChild>
          </div>
        </div>
      </div>
    </Dialog>
  </TransitionRoot>
</template>
