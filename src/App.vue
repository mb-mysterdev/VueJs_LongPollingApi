<script>
import {onMounted, onUpdated, reactive, ref} from "vue";
import axios from "axios";

export default {
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

<template>
  <input type="url" v-model="lastUrl" /><br>
  chaque combien de temps ?<br>
  <input type="number" v-model="checkEveryHowManySeconds" />
  <button @click="apis.push({url: lastUrl})">Ajouter une api a surveiller</button>
  <div v-for="api in apis" :key="api" :style="'background:' + api.color ">
    {{ api.url }}
    {{ api.status }}
  </div>
</template>


