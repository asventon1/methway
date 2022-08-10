<template>
  <div class="about">
    <ExpressionCalc/>
  </div>
</template>

<script>
import { defineComponent } from 'vue'
import ExpressionCalc from '@/components/ExpressionCalc.vue'

export default defineComponent({
  name: 'Expression',
  components: {
    ExpressionCalc
  }
})
</script>
