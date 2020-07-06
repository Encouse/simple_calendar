<template>
  <div class = 'wrapper'>
    <div class="calendar">
      <div class = 'select'>
        <div class = 'button' @click = 'prevMonth'>prev</div>
        <div v-if = '!selectDateToggle' @click = 'setDate'>{{`${fulldate[1]}-${fulldate[0]}`}}</div>
        <div class = 'set-date' v-else>
          <input @keyup.enter = 'submitDate' v-model = 'dataDate'>
          <div @click = 'submitDate'>Set</div>
        </div>
        <div class = 'button' @click = 'nextMonth'>next</div>
      </div>
      <CalCell v-for = 'day in weekdaynames' :key = 'day' :data = 'day'/>
      <CalCell v-for = 'day in monthday' :key = 'day' :data = 'day' @on-cell-click = 'onClickDay'/>
    </div>
    <div class = 'set-event'>
      <input id = 'event' @keup.enter = 'setEvent' v-model = 'eventModel'>
      <div class = 'button' @click = 'setEvent'>Set</div>
    </div>
  </div>
</template>

<script>
import CalCell from '@/components/CalCell'
export default {
  name: 'calendar',
  components: {
    CalCell
  },
  mounted() {
    if (this.date) {
      let args = this.date.split('-').map(val => Number(val))
      this.fulldate = args
    }
    this.getDaysInMonth(...this.fulldate)
  },
  data() {
    return {
      monthdays: null,
      eventModel: null,
      eventObj: {},
      selectDateToggle: false,
      dataDate: this.fulldate,
      weekdaynames: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
      fulldate: [2013, 9, 8]
    }
  },
  props: {
    date: String,
  },
  methods: {
    getDaysInMonth: function(year, month) {
      let days = new Date(year, month, 0).getDate()
      this.monthdays = days
    },
    nextMonth: function() {
      if (this.month == 12) {
        this.fulldate[0] ++
        this.fulldate[1] = 1
      } else {
        this.fulldate[1] ++
      }
      this.getDaysInMonth(...this.fulldate)

    },
    prevMonth: function() {
      if (this.fulldate[1] == 1) {
        this.fulldate[0] --
        this.fulldate[1] = 12
      } else {
        this.fulldate[1] --
      }
      this.getDaysInMonth(...this.fulldate)
    },
    setDate: function() {
      this.selectDateToggle = true
    },
    submitDate: function() {
      this.selectDateToggle = false
      this.fulldate = this.dataDate.split('-')
      this.getDaysInMonth(...this.fulldate)
    },
    onClickDay: function(val) {
      this.fulldate[2] = val
      this.eventModel = this.eventObj[this.fulldate]
    },
    setEvent: function() {
      this.eventObj[this.fulldate] = this.eventModel
    }
  }
}
</script>

<style scoped>
.calendar {
  display: inline-block;
  max-width: 40%;
  max-height: 100%;
  border: 1px solid black;
}
.select {
  cursor: pointer;
  width: 100%;
  height: 40px;
  display: inline-flex;
  justify-content: space-around;
  line-height: 40px;
  vertical-align: middle;
}
.set-date {
  display: inline-flex;
}
.button {
  cursor: pointer;
}
.wrapper {
  display: inline-flex;
  width: inherit;
}
.set-event {
  margin-left: 10px;
}
</style>
