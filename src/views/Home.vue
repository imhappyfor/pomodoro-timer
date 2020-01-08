<template>
  <div class="home">
    <timer/>
  </div>
</template>

<script>
// @ is an alias to /src
import timer from '@/components/timer.vue'

export default {
  name: 'home',
  components: {
    timer
  }
}
</script>
