<template>
  <div class="default">
    <h2>Default</h2>

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
import Vue from 'vue'
import axios from 'axios'

export default Vue.extend({
  name: 'DefaultComponent',
  data () {
    return {
      things: [] as any[],
      size: 3
    }
  },
  async fetch () {
    const posts = await this.fetchThings(this.size)
    this.things = posts
    this.size++
  },
  fetchKey () { return Date.now().toString() },
  methods: {
    async fetchMore () {
      const posts: any[] = await this.fetchThings(this.size)
      this.size++
      this.things = [...(this.things ?? []), ...(posts ?? [])]
    },
    async fetchThings (size = 1) {
      const { data } = await axios.get(`https://random-data-api.com/api/users/random_user?size=${size}`)
      return data
    }
  }
})
</script>

<style lang="scss" scoped>
.default {
  flex: 1;
}
</style>
