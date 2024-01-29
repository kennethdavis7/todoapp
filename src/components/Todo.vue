<script setup>
import { ref } from "vue";

const props = defineProps(["data"]);
const emits = defineEmits(["delete", "update"]);

const editFormTodoName = ref(props.data.todo);

const onUpdate = () => {
  emits("update", editFormTodoName.value);
};
</script>

<template>
  <div class="w-full p-2 mt-3 flex justify-between rounded-md bg-slate-100">
    <span class="text-sm">{{ props.data.todo }}</span>

    <div class="actions flex w-2/12 justify-between align-middle">
      <button type="button" @click="onUpdate">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 24 24"
          fill="currentColor"
          class="w-5 h-5"
        >
          <path
            d="M21.731 2.269a2.625 2.625 0 0 0-3.712 0l-1.157 1.157 3.712 3.712 1.157-1.157a2.625 2.625 0 0 0 0-3.712ZM19.513 8.199l-3.712-3.712-12.15 12.15a5.25 5.25 0 0 0-1.32 2.214l-.8 2.685a.75.75 0 0 0 .933.933l2.685-.8a5.25 5.25 0 0 0 2.214-1.32L19.513 8.2Z"
          />
        </svg>
      </button>

      <button type="button" @click="emits('delete', props.data)">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke-width="1.5"
          stroke="currentColor"
          class="w-5 h-5"
        >
          <path stroke-linecap="round" stroke-linejoin="round" d="M6 18 18 6M6 6l12 12" />
        </svg>
      </button>
    </div>
  </div>
</template>
