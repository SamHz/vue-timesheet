<template>
    <li>
        <span :style="{marginLeft: getStartOffset() + 'px', width: getWidth() + 'px'}"
              :class="[ 'bubble', 'bubble-' + (type || 'default') ]"
              :data-duration="dataDuration(end, start)"></span>
        <span class="date">{{ getDateLabel() }}</span>
        <span class="label">{{ label }}</span>
    </li>
</template>

<script>
  const moment = require('moment')
  export default {
    name: 'vue-bubble-week',
    props: {
      min: {type: Date},
      widthMonth: {type: Number},
      type: {type: String},
      start: {type: Date},
      end: {type: Date},
      label: {type: String}
    },
    methods: {
      formatMonth: function (num) {
        num = parseInt(num, 10)
        return num >= 10 ? num : '0' + num
      },
      getStartOffset: function () {
        return (this.widthMonth / 7) * (7 * parseInt(moment(this.start).diff(moment(this.min), 'week')) + parseInt(moment(this.start).format('d')) === 0 ? 7 : parseInt(moment(this.start).format('d')) - 1)
      },
      getDays: function () {
        return moment(this.end).diff(moment(this.start), 'days') + 1
      },
      getWidth: function () {
        return (this.widthMonth / 7) * this.getDays()
      },
      getDateLabel: function () {
        return moment(this.start).format('MM/DD') + '-' + moment(this.end).format('MM/DD')
      },
      dataDuration: function (e, s) {
        return e ? Math.round((e - s) / 1000 / 60 / 60 / 24 / 39) : ''
      }
    }
  }
</script>

<style>

</style>