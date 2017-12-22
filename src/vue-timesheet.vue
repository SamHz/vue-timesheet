<template>
    <div class="bg">
        <div v-if="loading">loading...</div>
        <div v-if="!loading" ref="timesheet" class="timesheet color-scheme-default">
            <div class="scale" ref="scale">
                <section v-if="mode == 'year'" v-for="(c, index) in sections" ref="section">{{c}}</section>
                <section v-if="mode == 'week'" v-for="(w, index) in sectionWeeks" ref="section">{{w}}</section>
            </div>
            <ul class="data" v-if="hasWidth" ref="ulBubble">
                <vue-bubble v-if="mode == 'year'" v-for="d in tsData" :key="d.label" :label="d.label" :start="d.start" :end="d.end" :type="d.type" :min="yearMin" :widthMonth="widthMonth"></vue-bubble>
                <vue-bubble-week v-if="mode == 'week'" v-for="d in tsData" :key="d.label" :label="d.label" :start="d.start" :end="d.end" :type="d.type" :min="minWeek" :widthMonth="widthMonth"></vue-bubble-week>
            </ul>
        </div>
    </div>
</template>

<script>
    import vueBubble from './vue-bubble.vue'
    import vueBubbleWeek from './vue-bubble-week.vue'
    const moment = require('moment')
    moment.lang('zh-cn', {
      week: {
        dow: 1 // Monday is the first day of the week
      }
    })
    export default {
      name: 'vue-timesheet',
      components: {
        vueBubble,
        vueBubbleWeek
      },
      data () {
        return {
          sections: [],
          sectionWeeks: [],
          widthMonth: 0,
          loading: true,
          tsData: [],
          hasWidth: false,
          minWeek: 0
        }
      },
      props: {
        yearMin: { type: Number },
        yearMax: { type: Number },
        model: { type: Array },
        startDate: {type: Date},
        endDate: {type: Date},
        mode: {type: String}
      },
      methods: {
        // 按年计算
        drawSections: function () {
          if (this.yearMin > this.yearMax) {
            this.sections = []
          } else {
            for (var c = this.yearMin; c <= this.yearMax; c++) {
              this.sections.push(c)
            }
          }
        },
        // 按周计算
        drawSectionWeeks: function () {
          let sDate = this.startDate
          let eDate = this.endDate
          if (sDate.getTime() > eDate.getTime()) {
            sDate = this.endDate
            eDate = this.startDate
          }
          let sDate1 = moment(sDate)
          let eDate1 = moment(eDate)
          let weekNum = eDate1.diff(sDate1, 'week') + 1
          weekNum = weekNum < 4 ? 4 : weekNum
          for (let w = 0; w < weekNum; w++) {
            this.sectionWeeks.push(moment(sDate).utc().startOf('week').add(w, 'w').format('MM/DD'))
          }
          this.minWeek = new Date(moment(sDate).utc().startOf('week').format('YYYY-MM-DD'))
        },
        parseDate: function (date) {
          if (this.mode === 'week') {
            return new Date(date)
          }
          if (date.indexOf('/') === -1) {
            date = new Date(parseInt(date, 10), 0, 1)
            date.hasMonth = false
          } else {
            date = date.split('/')
            date = new Date(parseInt(date[1], 10), parseInt(date[0], 10) - 1, 1)
            date.hasMonth = true
          }
          return date
        },
        parseData: function (data) {
          if (this.mode === 'week') {
            for (let n = 0, m = data.length; n < m; n++) {
              let beg = this.parseDate(data[n][0])
              let end = this.parseDate(data[n][1])
              let lbl = data[n][2]
              let cat = data[n][3]
              this.tsData.push({start: beg, end: end, label: lbl, type: cat})
            }
          } else {
            for (let n = 0, m = data.length; n < m; n++) {
              let beg = this.parseDate(data[n][0])
              let end = data[n].length === 4 ? this.parseDate(data[n][1]) : null
              let lbl = data[n].length === 4 ? data[n][2] : data[n][1]
              let cat = data[n].length === 4 ? data[n][3] : data[n].length === 3 ? data[n][2] : 'default'
              if (beg.getFullYear() < this.yearMin) {
                this.yearMin = beg.getFullYear()
              }
              if (end && end.getFullYear() > this.yearMax) {
                this.yearMax = end.getFullYear()
              } else if (beg.getFullYear() > this.yearMax) {
                this.yearMax = beg.getFullYear()
              }
              this.tsData.push({start: beg, end: end, label: lbl, type: cat})
            }
          }
          this.loading = false
        }
      },
      created () {
        this.drawSections()
        this.drawSectionWeeks()
        console.log(this.model)
        this.parseData(this.model || [])
      },
      mounted () {
        this.$nextTick(function () {
          this.widthMonth = this.$refs.section ? this.$refs.section[0].offsetWidth : 0
        })
      },
      watch: {
        widthMonth: function (newWidthMonth) {
          if (newWidthMonth > 0) {
            this.hasWidth = true
          }
        }
      }
    }
</script>
<style>
    @import "./timesheet.min.css";
    .bg {
        padding-top: 26px;
        background: #fff;
    }
</style>