<template>
  <Layout>
    <div class="max-w-6xl mx-auto">
      <div>
        Current local time
      </div>
      <div class="text-5xl">
        {{ currentTime }}
      </div>
      <div class="w-full mb-6 md:mb-0">
        <label class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2" for="grid-state">
          Select Timezone
        </label>
        <div class="relative">
          <select v-model="timezone" class="block appearance-none w-full bg-gray-200 border border-gray-200 text-gray-700 py-3 px-4 pr-8 rounded leading-tight outline-none focus:outline-none focus:bg-white focus:border-gray-500" id="grid-state">
            <option value="null" disabled selected>Select a timezone</option>
            <option v-for="timezone in timezones" :value="timezone">
              {{ timezone }} 
            </option>
          </select>
          <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700">
            <svg class="fill-current h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"/></svg>
          </div>
        </div>
        <div>
          To
        </div>
        <div>
          {{ to }}
        </div>
        <div class="border border-indigo-500 rounded-md p-2">
          <datetime type="datetime" class="w-full" input-class="w-full" v-model="date"></datetime>
        </div>
      </div>
    </div>
  </Layout>
</template>

<script>
import timezones from '@/data/timezones.json'
import { DateTime as DT } from 'luxon'

export default {
  metaInfo: {
    title: 'Hello, world!'
  },

  data: () => ({
    now: DT.local(),
    timezones: timezones.sort((a, b) => a.offset - b.offset),
    timezone: null,
    date: new Date().toLocaleTimeString(),
  }),

  mounted () {
    setInterval(() => {
      this.now = DT.local()
    }, 60000);
  },

  computed: {
    currentTime () {
      return this.now.toLocaleString(DT.DATETIME_HUGE)
    },
    to () {
      return this.now.setZone(this.timezone).toLocaleString(DT.DATETIME_HUGE)
      // if (this.offset === null) {
      //   return;
      // }
      // if (this.offset < 0) {
      //   return Number.isInteger(this.offset)
      //     ? this.now.minus({hours: this.offset}).toLocaleString(DateTime.DATETIME_HUGE)
      //     : this.now.minus({hours: parseInt(this.offset), minutes: 30}).toLocaleString(DateTime.DATETIME_HUGE)
      // } else {
      //   return Number.isInteger(this.offset)
      //     ? this.now.plus({hours: this.offset}).toLocaleString(DateTime.DATETIME_HUGE)
      //     : this.now.plus({hours: parseInt(this.offset), minutes: 30}).toLocaleString(DateTime.DATETIME_HUGE)
      // }
    }
  }
}
</script>
