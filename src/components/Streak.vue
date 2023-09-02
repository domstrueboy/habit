<script setup>
import { computed } from 'vue'

const props = defineProps({
  data: Array,
})

function stringToDate(str) {
  const date = new Date()
  const [day, month, year] = str.split('.').map(el => parseInt(el))

    date.setFullYear(2000 + year)
    date.setMonth(month - 1)
    date.setDate(day)
    date.setHours(0, 0, 0, 0)

    return date
}

const dates = computed(() => {
  return props.data.reduce((dates, dataItem) => {

    const isString = typeof dataItem === 'string'

    const streakStartDate = stringToDate(isString ? dataItem : dataItem[0])
    const lastDatesDate = dates.at(-1)?.date

    const differenceInDays = lastDatesDate ? (streakStartDate - lastDatesDate) / 1000 / 60 / 60 / 24 - 1 : 0
    const missedDates = []

    for (let i = 0; i < differenceInDays; i++) {
      const newDate = new Date(lastDatesDate.getTime())
      newDate.setDate(newDate.getDate() + i + 1)
      missedDates.push({ date: newDate, done: false })
    }

    const streakLength = isString ? 1 : dataItem[1]
    const streak = []

    for (let i = 0; i < streakLength; i++) {
      const newDate = new Date(streakStartDate.getTime())
      newDate.setDate(newDate.getDate() + i)
      streak.push({ date: newDate, done: true })
    }

    return [...dates, ...missedDates, ...streak]
  }, [])
})
</script>

<template>
  <div class="container">
    <div
      v-for="date in dates"
      :key="date.date"
      :class="['day', date.done && 'done']"
    />
  </div>
</template>

<style scoped>
.container {
  width: 480px;
  min-height: 200px;
  display: flex;
  gap: 3px;
  justify-content: start;
  align-content: start;
  flex-wrap: wrap;
  background-color: tomato;
}
.day {
  width: 20px;
  height: 20px;
  background-color: whitesmoke;
  border-radius: 2px;
}
.done {
  background-color: greenyellow;
}
</style>
