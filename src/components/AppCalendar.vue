<script
  setup
  lang="ts"
>
import kanmusuList from '@/assets/kcbirthday.json'
import {onBeforeMount, type Ref, ref} from 'vue'
import {Dayjs} from 'dayjs'
import TheCalendar from "@/components/thecalendar/TheCalendar.vue";

export type CalendarEvent = {
  title: string,
  day: number | null,
  month: number | null,
  year: number | null,
  img: string
}

export type WeekItem = {
  day: Dayjs,
  events: Array<CalendarEvent>
}

export type MonthDays = {
  date: Date | string,
  isCurrentMonth: boolean,
}

export type MonthDaysWithEvents = {
  date: Date | string,
  isCurrentMonth: boolean,
  events: Array<CalendarEvent>
}

const date = ref([new Date()])
const events = ref<Array<Event>>([])
const calendarEvents: Ref<Array<CalendarEvent>> = ref([])
const imgUrls = import.meta.glob('@/assets/icons/calendar/*.png', {
  import: 'default',
  eager: true,
})

function createCalendarEvents() {
  calendarEvents.value = []
  kanmusuList.forEach(item => {
    const cEvent: CalendarEvent = {
      title: item.name,
      day: item.day,
      month: item.month,
      year: item.year,
      img: '',
    }
    if (imgUrls.hasOwnProperty(`/src/assets/icons/calendar/${item.name}.png`)) {
      cEvent.img = imgUrls[`/src/assets/icons/calendar/${item.name}.png`]+''
    } else {
      cEvent.img = imgUrls[`/src/assets/icons/calendar/_rensouhou.png`]+''
    }
    calendarEvents.value?.push(cEvent)
  })
}

onBeforeMount(() => {
  createCalendarEvents()
})
</script>
<template>
  <div class="pa-6">
    <h1>Kanmusu birthday</h1>
    <TheCalendar :calendar-events="calendarEvents" />
  </div>
</template>
<style scoped>
</style>
