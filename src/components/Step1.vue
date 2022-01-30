<template>
  <div class="w-full max-w-xs">
    <form class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4 justify-self-center	">
      <div class="mb-4">
        <label class="block text-gray-700 text-sm font-bold mb-2">
          Nom
        </label>
        <input class="shadow appearance-none border rounded w-full py-2 px-3
        text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
               type="text" v-model="urlName" placeholder="Nom de l'url">
      </div>
      <div class="mb-4">
        <label class="block text-gray-700 text-sm font-bold mb-2">
          URL
        </label>
        <input class="shadow appearance-none border rounded w-full py-2 px-3
        text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
               type="url" v-model="lastUrl" placeholder="l'url a surveiller">
      </div>
      <div class="mb-6">
        <label class="block text-gray-700 text-sm font-bold mb-2">
          Chaque combien de temps ?
        </label>
        <input class="shadow appearance-none rounded w-full py-2 px-3
        text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline"
               type="number" v-model="checkEveryHowManySeconds"
        >
      </div>
      <div class="flex items-center justify-between">
        <button
            @click="apis.push({url: lastUrl,name: urlName})"
            class="bg-blue-500 hover:bg-blue-700 text-white
            font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" type="button">
          Ajouter
        </button>
      </div>
    </form>
  </div>

  <div class="flex flex-col">
    <div class="overflow-x-auto sm:-mx-6 lg:-mx-8">
      <div class="inline-block py-2 min-w-full sm:px-6 lg:px-8">
        <div class="overflow-hidden shadow-md sm:rounded-lg">
          <table class="min-w-full">
            <thead class="bg-gray-100 dark:bg-gray-700">
            <tr>
              <th scope="col" class="py-3 px-6 text-xs font-medium tracking-wider text-left text-gray-700 uppercase dark:text-gray-400">
                Nom
              </th>
              <th scope="col" class="py-3 px-6 text-xs font-medium tracking-wider text-left text-gray-700 uppercase dark:text-gray-400">
                Method
              </th>
              <th scope="col" class="py-3 px-6 text-xs font-medium tracking-wider text-left text-gray-700 uppercase dark:text-gray-400">
                Status
              </th>
            </tr>
            </thead>
            <tbody>

            <tr
             v-for="api in apis" :key="api" :style="'background:' + api.color "
            class="border-b even:bg-gray-50 odd:dark:bg-gray-800
                 even:dark:bg-gray-700 dark:border-gray-600">
              <td class="py-4 px-6 text-sm font-medium text-gray-900 whitespace-nowrap dark:text-white">
                {{ api.name }}
              </td>
              <td class="py-4 px-6 text-sm text-gray-500 whitespace-nowrap dark:text-gray-400">
                GET
              </td>
              <td class="py-4 px-6 text-sm text-gray-500 whitespace-nowrap dark:text-gray-400">
                {{ api.status }}
              </td>
            </tr>

            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import {onUnmounted, reactive, ref} from "vue";
import axios from "axios";

export default {
  name: 'Step1',
  setup(){
    const apis = reactive([])
    const lastUrl = ref(null);
    const urlName = ref('')
    const checkEveryHowManySeconds = ref(2000);
    onUnmounted(()=> {
      clearInterval()
    })
    return {apis, lastUrl, checkEveryHowManySeconds,urlName}
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
              .catch(error => {
                console.log(error.response)
                api.status = error.status
                api.color = 'red'
              })

          return api
        });

      }, self.checkEveryHowManySeconds)
    }
  },

}


</script>
