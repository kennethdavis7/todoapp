<script setup>
import { reactive, ref } from "vue";
import Hello from "../dumpster/Hello.vue";

const data = ref([1, 2]);
const color = ref("red");
const fontSize = ref(40);

const myObject = reactive({
  title: "How to do lists in Vue",
  author: "Jane Doe",
  publishedAt: "2016-04-10",
});

const items = ref([
  {
    msg: "ya",
  },
  {
    msg: "gak",
  },
]);

const count = ref(0);

const message = ref();

const selected = ref({ number: 12 });
const age = ref();

const helloMessage = ref();
</script>

<template>
  <Hello :style="{ color: color, fontSize: fontSize + 'px' }" />
  <p v-for="(item, index) in data" :key="index">{{ item }}</p>

  <ul>
    <li v-for="(value, key) in myObject" :key="key">{{ key }} : {{ value }}</li>
  </ul>

  <span v-for="n in 10" :key="n">{{ n }}</span>

  <!-- <ul>
    <template v-for="item in items">
      <li>{{ item.msg }}</li>
      <li class="divider" role="presentation">sfssa</li>
    </template>
  </ul> -->

  <button @click="count++">{{ count }}</button>

  <button @click.exact="onClick">A</button>

  <p>Message is: {{ message }}</p>
  <input v-model="message" placeholder="edit me" />

  <select v-model="selected">
    <!-- inline object literal -->
    <option :value="{ number: 123 }">123</option>
  </select>
  {{ selected.number }}

  <input v-model.number="age" />
  {{ age }}

  <Hello @response="(msg) => (helloMessage = msg)" />

  {{ helloMessage }}
</template>
