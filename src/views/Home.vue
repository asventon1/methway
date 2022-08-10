<template>
  <div class="home">
    <Welcome />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import Welcome from '@/components/Welcome.vue' // @ is an alias to /src

export default defineComponent({
  name: 'Home',
  components: {
    Welcome
  }
})
</script>
