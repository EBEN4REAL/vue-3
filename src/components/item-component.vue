<template>
  <div class="about">
    <input v-model.lazy="nameStr" />
  </div>
</template>
<script>
import {onMounted, reactive, toRefs, watch} from 'vue'


export default {
  props: {
    title: String,
  },  
  emits: ['getName', 'submit'],
  setup(props, ctx) {
    const state = reactive({
      nameStr: "Eben"
    })
    watch(() => state.nameStr, (val) => {
      ctx.emit('getName', val)
    })
    onMounted(() => {
      console.log(props.title)
    })

    return {...toRefs(state)}

  }
}
</script>
