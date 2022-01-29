<script setup>
import {reactive, ref} from "vue";

const apis = reactive([])
const lastUrl = ref(null);
</script>

<template>
 <input type="url" v-model="lastUrl" />
  <button @click="apis.push(lastUrl)">Ajouter une api a surveiller</button>
  <div v-for="api in apis" :key="api">
    {{ api }}
  </div>
</template>


