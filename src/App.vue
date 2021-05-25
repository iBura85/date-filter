<template>
  <div style="padding: 30px">
    <date-filter @change="onDateFilterChange" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

import { DateFilter, DateFilterValue } from '@/components/datefilter'

export default defineComponent({
  name: 'APP',

  components: { DateFilter },

  methods: {
    onDateFilterChange(val: DateFilterValue) {
      console.log(val)
    },
  },
})
</script>
