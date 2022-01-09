<template>
  <div class="home">
    <Hero />
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import Hero from '~/components/Hero/Hero.vue'

export default Vue.extend({
  name: "IndexPage",
  components: { Hero }
})
</script>
