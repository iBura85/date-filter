<template>
  <div>
    <el-select
      v-model="customDateValue"
      value-key="label"
      @change="setCustomDateRange"
    >
      <el-option
        v-for="option in customDateOptions"
        :key="option.label"
        :label="option.label"
        :value="option"
      >
      </el-option>
    </el-select>

    <el-date-picker type="daterange" v-model="dateRange"></el-date-picker>
    <div>{{ dateFilter }}</div>
  </div>
</template>

<script lang="ts">
import { DateTime, DurationInput } from 'luxon'

interface CustomDateOption {
  id: string
  label: string
  range: {
    minus?: DurationInput
    plus?: DurationInput
  }
}

const customDateOptions: CustomDateOption[] = [
  {
    id: 'today',
    label: 'за сегодня',
    range: {
      minus: { day: 0 },
      plus: { day: 1 },
    },
  },
  {
    id: 'yesterday',
    label: 'за вчера',
    range: {
      minus: { day: 1 },
      plus: { day: 0 },
    },
  },
  {
    id: 'lastMonth',
    label: 'за последний месяц',
    range: {
      minus: { day: 30 },
      plus: { day: 1 },
    },
  },
]

const defaultCustomDateOption = customDateOptions.find(
  (x) => x.id === 'lastMonth',
)

import { defineComponent } from 'vue'

import { DateFilterValue } from './types'

export default defineComponent({
  name: 'DateFilter',

  data() {
    return {
      customDateValue: defaultCustomDateOption,
      customDateOptions: customDateOptions,
      dateFilter: {} as DateFilterValue,
    }
  },

  computed: {
    dateRange: {
      get(): Date[] {
        return [this.dateFilter.dateFrom, this.dateFilter.dateTo]
      },
      set(val: Date[]) {
        this.dateFilter.dateFrom = val[0]
        this.dateFilter.dateTo = val[1]
        this.$emit('change', this.dateFilter)
      },
    },
  },

  mounted() {
    this.setCustomDateRange()
  },

  methods: {
    setCustomDateRange() {
      const { range } = this.customDateValue

      const today = () => DateTime.local()
      const from = range.minus ? today().minus(range.minus) : today()
      const to = range.plus ? today().plus(range.plus) : today()

      this.dateFilter.dateFrom = from.startOf('day').toJSDate()
      this.dateFilter.dateTo = to.startOf('day').toJSDate()

      this.$emit('change', this.dateFilter)
    },
  },
})
</script>
