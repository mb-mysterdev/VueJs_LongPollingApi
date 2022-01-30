<template>
  <input class="shadow appearance-none border rounded w-full py-2 px-3
   text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
         type="url" v-model="lastUrl" placeholder="l'url a surveiller">

  <label>chaque combien de temps ?</label>
  <input class="shadow appearance-none border rounded w-full py-2 px-3
   text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
         type="number" v-model="checkEveryHowManySeconds" placeholder="l'url a surveiller">
  <button @click="apis.push({url: lastUrl})" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
    Ajouter
  </button>
  <div v-for="api in apis" :key="api" :style="'background:' + api.color ">
    {{ api.url }}
    {{ api.status }}
  </div>
</template>

<script>
import {reactive, ref} from "vue";
import axios from "axios";

export default {
  name: 'Step1',
  setup(){
    const apis = reactive([])
    const lastUrl = ref(null);
    const checkEveryHowManySeconds = ref(2000);
    return {apis, lastUrl, checkEveryHowManySeconds}
  },
  updated() {
    if(this.apis.length > 0){
      const self = this;

      setInterval(function(){
        self.apis.map(api =>
        {
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

      }, self.checkEveryHowManySeconds)
    }
  }
}


</script>

<style scoped>

</style>
