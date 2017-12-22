# vue-timesheet

> This Vue component is derived from [timesheet.js]<https://github.com/sbstjn/timesheet.js> 

> and add weeks table 

![show](https://raw.githubusercontent.com/sbstjn/timesheet.js/master/screen.png)

## Build Setup

``` bash
# install dependencies
npm install vue-timesheet -s
# Javascript Code:
// please make sure install moment.js this weeks table driver it, thanks.
import 'vue-timesheet/dist/vue-timesheet.min.css'
import vueTimeSheet from 'vue-timesheet';
export default {
    components: {
        vueTimeSheet
    },
    data() {
        return {
            // years table
            tsData: [
                      ['2002', '09/2002', 'A freaking awesome time', 'lorem'],
                      ['06/2002', '09/2003', 'Some great memories', 'ipsum'],
                      ['2003', 'Had very bad luck'],
                      ['10/2003', '2006', 'At least had fun', 'dolor'],
                      ['02/2005', '05/2006', 'Enjoyed those times as well', 'ipsum'],
                      ['07/2005', '09/2005', 'Bad luck again', 'default'],
                      ['10/2005', '2008', 'For a long time nothing happened', 'dolor'],
                      ['01/2008', '05/2009', 'LOST Season #4', 'lorem'],
                      ['01/2009', '05/2009', 'LOST Season #4', 'lorem'],
                      ['02/2010', '05/2010', 'LOST Season #5', 'lorem'],
                      ['09/2008', '06/2010', 'FRINGE #1 & #2', 'ipsum']
                    ],
            startYear: '2000',
            endYear: '2018',
            // weeks table, for the moment, the weeks data need to complete, it's not like years table
            tsData1: [
                       ['2017-11-01', '2017-11-11', 'A freaking awesome time', 'lorem'],
                       ['2017-11-02', '2017-11-22', 'Some great memories', 'ipsum'],
                       ['2017-11-08', '2017-11-22', 'Had very bad luck', 'default'],
                       ['2017-11-01', '2017-11-01', 'At least had fun', 'dolor'],
                       ['2017-11-20', '2017-11-22', 'Enjoyed those times as well', 'ipsum'],
                       ['2017-11-09', '2017-11-09', 'Bad luck again', 'default'],
                       ['2017-11-05', '2017-11-08', 'For a long time nothing happened', 'dolor'],
                       ['2017-11-18', '2017-11-28', 'LOST Season #4', 'lorem'],
                       ['2017-11-25', '2017-11-30', 'LOST Season #4', 'lorem'],
                       ['2017-11-11', '2017-11-12', 'LOST Season #5', 'lorem'],
                       ['2017-11-15', '2017-11-30', 'FRINGE #1 & #2', 'ipsum']
            ]
            startDate: new Date('2017-11-01'),
            endDate: new Date('2017-11-30')
        }
    }
}

# Template
<vue-time-sheet :startDate="startYear" :endDate="endYear" mode="year" :model="tsData"></vue-time-sheet>
<vue-time-sheet :startDate="startDate" :endDate="endDate" mode="week" :model="tsData1"></vue-time-sheet>

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
