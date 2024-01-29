<script setup>
const emits = defineEmits(["emit"]);

emits("response", "hello wor");
</script>

<template>
  <h1 :style="$attrs.style">Hello World</h1>
</template>
