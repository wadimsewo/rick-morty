<template>
  <div class="p-5">
    <div class="text-4xl font-bold text-center p-4">H&U: Test</div>
    <div class="flex justify-center">
      <input type="text" placeholder="Nach Character suchen..." class="border-2 border-gray-400 rounded p-4 lg:w-1/2 w-10/12"
             v-model="searchInput" @input="getItems(null)">
    </div>
    <div v-if="items" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 2xl:grid-cols-6">
      <div class="py-10 px-4" v-for="result in items.results" :key="result.id">
        <div class="max-w-sm rounded overflow-hidden shadow-lg">
          <img class="w-full" :src="result.image" alt="Mountain" loading="lazy">
          <div class="px-6 py-4">
            <div class="font-bold text-xl mb-2">{{ result.name }}</div>
            <p class="text-base font-bold"
               :class="{'text-green-700': result.status === 'Alive', 'text-red-700' : result.status === 'Dead', 'text-gray-400': result.status === 'unknown' }">
              <span class="text-black">Status:</span> {{ result.status }}
            </p>
            <p class="text-gray-700 text-base">Species: {{ result.species }}</p>
            <p class="text-gray-700 text-base">Gender: {{ result.gender }}</p>
            <p class="text-gray-700 text-base">Location: {{ result.location.name }}</p>
          </div>
        </div>
      </div>
    </div>
    <div v-else>
      Kein Content vorhanden
    </div>
    <div class="flex justify-center">
      <button class="bg-blue-500 px-4 py-2 mx-4 text-lg font-semibold tracking-wider text-white rounded hover:bg-blue-600 disabled:opacity-50" @click="getItems(items.info.prev)" :disabled="!items?.info?.prev">&lt; Prev</button>
      <button class="bg-blue-500 px-4 py-2 text-lg font-semibold tracking-wider text-white rounded hover:bg-blue-600 disabled:opacity-50" @click="getItems(items.info?.next)" :disabled="!items?.info?.next">Next ></button>
    </div>
  </div>
</template>

<script>
import {onMounted, ref} from "vue"
import axios from "axios";

export default {
  name: 'App',
  components: {},
  setup() {
    const searchInput = ref('')
    const items = ref(null);
    const getItems = (url) => {
      const req = url ?? `https://rickandmortyapi.com/api/character/?name=${searchInput.value}`;
      axios.get(req).then(res => {
        items.value = res.data;
      })
    }

    onMounted(() => {
      getItems();
    })

    return {searchInput, getItems, items}
  }
}
</script>
