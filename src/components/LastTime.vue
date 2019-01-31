<template>
  <div>{{ lastTime }}</div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator'

enum IntervalTime {
  SEC = 1000,
  MIN = 60 * IntervalTime.SEC,
  HOUR = 60 * IntervalTime.MIN,
  DAY = 24 * IntervalTime.HOUR,
  WEEK = 7 * IntervalTime.DAY,
  YEAR = 52 * IntervalTime.WEEK,
}

enum TimeUnit {
  SEC = 'sec',
  MIN = 'min',
  HOUR = 'hour',
  DAY = 'day',
  WEEK = 'week',
  YEAR = 'year',
}

@Component
export default class LastTime extends Vue {
  @Prop({ default: () => 0 }) baseTime!: number
  duration: number = 0
  intervalId: number = 0
  intervalTime: IntervalTime = IntervalTime.SEC
  timeUnit: TimeUnit = TimeUnit.SEC

  created() {
    const currentTime = new Date().getTime()
    this.duration = currentTime - this.baseTime
    this.intervalId = setInterval(() => {
      this.timer()
    }, this.intervalTime)
  }

  timer() {
    const currentTime = new Date().getTime()
    this.duration = currentTime - this.baseTime
    if (this.duration > IntervalTime.YEAR) {
      this.restartInterval(IntervalTime.YEAR, TimeUnit.YEAR)
    } else if (this.duration > IntervalTime.WEEK && this.intervalTime <= IntervalTime.DAY) {
      this.restartInterval(IntervalTime.WEEK, TimeUnit.WEEK)
    } else if (this.duration > IntervalTime.DAY && this.intervalTime <= IntervalTime.HOUR) {
      this.restartInterval(IntervalTime.DAY, TimeUnit.DAY)
    } else if (this.duration > IntervalTime.HOUR && this.intervalTime <= IntervalTime.MIN) {
      this.restartInterval(IntervalTime.HOUR, TimeUnit.HOUR)
    } else if (this.duration > IntervalTime.MIN && this.intervalTime <= IntervalTime.SEC) {
      this.restartInterval(IntervalTime.MIN, TimeUnit.MIN)
    }
  }

  restartInterval(intervalTime: IntervalTime, timeUnit: TimeUnit) {
    clearInterval(this.intervalId)
    this.intervalTime = intervalTime
    this.timeUnit = timeUnit
    this.intervalId = setInterval(() => {
      this.timer()
    }, this.intervalTime)
  }

  get lastTime(): string {
    let divide: IntervalTime = IntervalTime.SEC
    if (this.duration < IntervalTime.MIN) {
    } else if (this.duration < IntervalTime.HOUR) {
      divide = IntervalTime.MIN
    } else if (this.duration < IntervalTime.DAY) {
      divide = IntervalTime.HOUR
    } else if (this.duration < IntervalTime.WEEK) {
      divide = IntervalTime.DAY
    } else if (this.duration < IntervalTime.YEAR) {
      divide = IntervalTime.WEEK
    } else {
      divide = IntervalTime.YEAR
    }
    return Math.floor(this.duration / divide) + ' ' + this.timeUnit
  }
}
</script>

<style lang="scss" scoped></style>
