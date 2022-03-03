<template>
  <div class="w-full flex justify-center">
    <input type="text" placeholder="Enter Pokemon here" class="mt-10 p-2 border-blue-500 border-2 mr-3" v-model="search" /> 
    <input type="text" placeholder="Enter some notes here" class="mt-10 p-2 border-blue-500 border-2" v-model="notes" /> 
  </div>
  <div class="mt-10 p-4 flex flex-wrap justify-center">
    <div class="text-2x ml-4 text-blue-500" v-for="(pokemon,idx) in filteredPokemons" :key="idx">
      <router-link :to="`/about/${urlIdLookup[pokemon.name]}`" v-if="idx > 0">
        {{pokemon.name}}
      </router-link>
    </div>
    <item-component title="Some title" @getName="getName" />
  </div>
</template>

<script>
import ItemComponent from "@/components/item-component.vue"
import {reactive, toRefs, computed, onMounted, ref, defineComponent, watch} from 'vue'

export default defineComponent ({
  name: 'PokeMons',
  components: {
    ItemComponent
  },
  setup() {
    const state = reactive({
      pokemons: [],
      urlIdLookup: {},
      search: "",
      filteredPokemons: computed(() => searchPokemons())
    })

    const notes = ref("")
    let  showNotesSavedNotification = ref(false);
    let  firstName = ref('John')
    let  lastName = ref('Doe')

    const fullName = computed({
      get() {
        return firstName.value + ' ' + lastName.value
      },
      set(newValue) {
        [firstName.value, lastName.value] = newValue.split(' ')
      }
    })
    
    watch(showNotesSavedNotification, (newValue, oldValue) => {
      console.log(newValue)
      console.log(oldValue)
    })

    watch(firstName, (val) => {
      console.log(val)
    })
    watch(fullName, (val) => {
      console.log(val)
    })
    // Deep watch
    watch(
      () => state.someObject,
      () => {
        // Note: `newValue` will be equal to `oldValue` here
        // *unless* state.someObject has been replaced
      },
      { deep: true }
    )

    function searchPokemons() {
      return state.pokemons.filter(pokemon => pokemon.name.toLowerCase().includes(state.search))
    }

    onMounted(() => {
      {
        showNotesSavedNotification.value = true
        firstName.value = 'Ebenezer'
        lastName.value  = "Igbinoba"
        fullName.value = "Joshua Igbinoba"
        fetch("https://pokeapi.co/api/v2/pokemon?offset=0")
        .then((res) => res.json())
          .then((data) => {
            state.pokemons = data.results
            state.urlIdLookup = data.results.reduce((acc, cur, idx) => acc = {...acc, [cur.name]: idx + 1})
          }, {})
      }
    }) 

    const getName = name => {
      console.log(name)
    }

    return {...toRefs(state), notes, getName}
  }
})
</script>
