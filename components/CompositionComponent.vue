<template>
  <div class="composition">
    <h2>Composition</h2>

    <div>
      <p>size: {{ size }}</p>
      <button @click="fetchMore">
        fetchMore
      </button>
      <p>length: {{ things.length }}</p>
      <ul>
        <li
          v-for="thing in things"
          :key="thing.id"
        >
          {{ thing.uid }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script lang="ts">
import { ref, useFetch, defineComponent, Ref } from '@nuxtjs/composition-api'
import axios from 'axios'

export default defineComponent({
  name: 'CompositionComponent',
  setup () {
    const things = ref([]) as Ref<any[]>
    const size = ref(3) as Ref<number>
    async function fetchThings (size = 1) {
      const { data } = await axios.get(`https://random-data-api.com/api/users/random_user?size=${size}`)
      return data
    }

    async function fetchMore () {
      const data = await fetchThings(size.value)
      things.value = [...things.value, ...data]
      size.value++
    }

    useFetch(async () => {
      try {
        const data = await fetchThings(size.value)
        things.value = data
        size.value++
      } catch (err) {
        console.error(err)
      }
    })

    return {
      things,
      size,
      fetchMore
    }
  }
})
</script>

<style lang="scss" scoped>
.composition {
  flex: 1;
}
</style>
