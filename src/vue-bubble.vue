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
export default {
  name: 'vue-bubble',
  props: {
    min: {type: Number},
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
      return (this.widthMonth / 12) * (12 * (this.start.getFullYear() - this.min) + this.start.getMonth())
    },
    getFullYears: function () {
      return ((this.end && this.end.getFullYear()) || this.start.getFullYear()) - this.start.getFullYear()
    },
    getMonths: function () {
      var fullYears = this.getFullYears()
      var months = 0

      if (!this.end) {
        months += !this.start.hasMonth ? 12 : 1
      } else {
        if (!this.end.hasMonth) {
          months += 12 - (this.start.hasMonth ? this.start.getMonth() : 0)
          months += 12 * (fullYears - 1 > 0 ? fullYears - 1 : 0)
        } else {
          months += this.end.getMonth() + 1
          months += 12 - (this.start.hasMonth ? this.start.getMonth() : 0)
          months += 12 * (fullYears - 1)
        }
      }

      return months
    },
    getWidth: function () {
      return (this.widthMonth / 12) * this.getMonths()
    },
    getDateLabel: function () {
      return [
        (this.start.hasMonth ? this.formatMonth(this.start.getMonth() + 1) + '/' : '') + this.start.getFullYear(),
        (this.end ? '-' + ((this.end.hasMonth ? this.formatMonth(this.end.getMonth() + 1) + '/' : '') + this.end.getFullYear()) : '')
      ].join('')
    },
    dataDuration: function (e, s) {
      return e ? Math.round((e - s) / 1000 / 60 / 60 / 24 / 39) : ''
    }
  }
}
</script>
<style>
</style>