<template>
  <div class="about">
    <div v-if="pokemon" class="w-3/12 bg-purple-100 m-auto shadow-2xl flex justify-center flex-col items-center mt-4">
      <h3 class="text-2xl text-green-900 uppercase">
        {{pokemon.name}}
      </h3>
      <div class="flex justify-center">
        <img :src="pokemon.sprites.front_shiny" alt="" />
        <img :src="pokemon.sprites.back_shiny" alt="" />
      </div>
      <h3 class="text-yellow-400">Types</h3>
      <div class="" v-for="(type, idx) in pokemon.types" :key="idx">
        <h3 class="text-blue-900">
          {{type.type.name}}
        </h3>
      </div>
    </div>
  </div>
</template>
<script>
import {useRoute} from 'vue-router'
import {reactive, toRefs} from 'vue'


export default {
  setup() {
    const route = useRoute()

    const state = reactive({
      pokemon: null
    })

    function getPokemon() {
      fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}`)
      .then((res) => res.json())
        .then((data) => {
          state.pokemon = data
        }, {})
    }
    getPokemon()

    return {...toRefs(state)}
  }
}
</script>
