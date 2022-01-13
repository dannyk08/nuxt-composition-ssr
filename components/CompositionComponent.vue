<template>
  <div class="composition">
    <h2>Composition</h2>

    <div>
      <button @click="fetchMore">
        fetchMore
      </button>
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
import { ref, useFetch, defineComponent } from '@nuxtjs/composition-api'
import axios from 'axios'

export default defineComponent({
  name: 'CompositionComponent',
  setup () {
    const things = ref([])
    async function fetchThings () {
      const { data } = await axios.get('https://random-data-api.com/api/users/random_user?size=3')
      return data
    }

    async function fetchMore () {
      const data = await fetchThings()
      things.value = [...things.value, ...data]
    }

    useFetch(async () => {
      try {
        const data = await fetchThings()
        things.value = data
      } catch (err) {
        console.error(err)
      }
    })

    return {
      things,
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
