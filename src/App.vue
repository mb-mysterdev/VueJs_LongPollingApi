<script setup>
import {onMounted, onUpdated, reactive, ref} from "vue";
import axios from "axios";

const apis = reactive([])
const lastUrl = ref(null);

onUpdated(() => {
  if(apis.length > 0){
    setInterval(function(){
      apis.map(api =>
      {
        console.log('hoho')
        axios.get(api?.url).then(res => {
          api.status = res.status
          if(api.status === 200){
            api.color = 'green'
          }else {
            api.color = 'red'
          }
        })
        .catch(res => {
          api.color = 'red'
        })
        return api
      });

    }, 2000)
  }
})


</script>

<template>
 <input type="url" v-model="lastUrl" />
  <button @click="apis.push({url: lastUrl})">Ajouter une api a surveiller</button>
  <div v-for="api in apis" :key="api" :style="'background:' + api.color ">
    {{ api.url }}
    {{ api.status }}
  </div>
</template>


