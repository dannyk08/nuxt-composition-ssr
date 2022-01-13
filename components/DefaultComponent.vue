<template>
  <div class="default">
    <h2>Default</h2>

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
import Vue from 'vue'
import axios from 'axios'

export default Vue.extend({
  name: 'DefaultComponent',
  data () {
    return {
      things: {
        type: Array,
        default: []
      }
    }
  },
  async fetch () {
    const posts = await this.fetchThings()
    this.things = posts
    console.log('posts >', posts)
  },
  fetchKey () { return Date.now().toString() },
  methods: {
    async fetchMore () {
      const posts = await this.fetchThings()
      this.things = [...(this.things ?? []), ...(posts ?? [])]
    },
    async fetchThings () {
      const { data } = await axios.get('https://random-data-api.com/api/users/random_user?size=3')
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
