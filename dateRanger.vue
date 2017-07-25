<template>
    <div class="dateRanger">
      <Select v-model="select.dimension" @on-change="dateChange" filterable>
        <Option :value="option.value" :key="option.id" v-for="option in select.options">{{option.name}}</Option>
      </Select>
      <Date-picker
        :value="[picker.beginTime, picker.endTime]"
        :options="picker.dateOptions"
        format="yyyy年MM月dd日"
        type="daterange"
        placement="bottom-end"
        placeholder="选择日期"
        :style="widthStyle"
        @on-change="timeChange"
        :clearable="clearable || false"
        :confirm="confirm || false"
      ></Date-picker>
    </div>
</template>
<script>
    export default {
      props: ['datePicker', 'width', 'confirm', 'clearable'],
      data () {
        return {
          select: {// 范围选择
            options: [// 时间段选项
              {id: 1, name: '今天', value: '今天'},
              {id: 2, name: '昨天', value: '昨天'},
              {id: 3, name: '最近7天', value: '最近7天'},
              {id: 4, name: '最近15天', value: '最近15天'},
              {id: 5, name: '本月', value: '本月'},
              {id: 6, name: '上月', value: '上月'},
              {id: 7, name: '本年', value: '本年'},
              {id: 8, name: '自定义', value: '自定义'}
            ],
            customersList: [],
            dimension: '今天'// 默认选中的第一项
          },
          picker: {// 时间段选择
            dateOptions: {// 非显示可选时间段选项
              disabledDate (date) {
                return date.valueOf() > Date.now();
              }
            },
            beginTime: '', // 起始时间
            endTime: '' // 结束时间
          },
          formItem: {
            customerId: '-1',
            distype1: '0',
            distype2: '2'
          },
          widthStyle: {// 日期选择器可控宽度
            width: this.width || '400px'
          },
          opToPi: [], // 默认映射
          piToOp: [] // 反向映射
        }
      },
      methods: {
        /**
         * 日期选择对应规则
         * @param value 选项
         * @return fn 执行后可得到对应日期范围
         */
        reducer (value) {
          let date = new Date()
          let y = date.getFullYear()
          let w = date.getDay() + 1
          let d = date.getDate()
          let currentTime = new Date().getTime()
          let start = this.picker.beginTime
          let end = this.picker.endTime
          let n = 1
          if (value.indexOf('天') > -1 || value.indexOf('日') > -1) { // 以日计
            if (value.indexOf('近') > -1 || value.indexOf('最近') > -1 || value.indexOf('前') > -1) {
              if (/(\d+)/.test(value)) {
                n = /(\d+)/.exec(value)[1] - 1
              }
            } else if (value.indexOf('今') > -1) {
              n = 0
            } else if (value.indexOf('前') > -1) {
              n = 2
            } else if (value.indexOf('昨') > -1) {
              n = 1
            }
            start = currentTime - 3600 * 1000 * 24 * n
            end = (value.indexOf('近') > -1 || value.indexOf('最近') > -1 || value.indexOf('前') > -1) ? currentTime : start
          } else if (value.indexOf('周') > -1 || value.indexOf('星期') > -1) { // 以周计
            if (value.indexOf('近') > -1 || value.indexOf('最近') > -1) { // || value.indexOf('前') > -1) {
              if (/(\d+)/.test(value)) {
                n = /(\d+)/.exec(value)[1] - 1
              }
            } else if (value.indexOf('本') > -1) {
              n = 0
            } else if (value.indexOf('上') > -1) {
              n = 1
            }
            start = currentTime - 3600 * 1000 * 24 * (7 * n + w)
            end = (value.indexOf('本') > -1) ? currentTime : currentTime - 3600 * 1000 * 24 * w
          } else if (value.indexOf('月') > -1) { // 以月计
            let t = 0
            if (value.indexOf('近') > -1 || value.indexOf('最近') > -1) { // || value.indexOf('前') > -1) {
              if (/(\d+)/.test(value)) {
                n = d
                t = /(\d+)/.exec(value)[1]
              }
            } else if (value.indexOf('本') > -1) {
              n = 0
            } else if (value.indexOf('上') > -1) {
              n = d
            }
            end = currentTime - 3600 * 1000 * 24 * n
            let endDate = new Date(end)
            let ey = endDate.getFullYear()
            let em = endDate.getMonth() - t + 1
            start = (new Date(ey + '/' + em + '/01')).getTime()
          } else if (value.indexOf('年') > -1) { // 以年计
            if (value.indexOf('近') > -1 || value.indexOf('最近') > -1) { // || value.indexOf('前') > -1) {
              if (/(\d+)/.test(value)) {
                n = /(\d+)/.exec(value)[1]
              }
            } else if (value.indexOf('本') > -1 || value.indexOf('今') > -1) {
              n = 0
            } else if (value.indexOf('上') > -1 || value.indexOf('去') > -1) {
              n = 1
            }
            start = (new Date((y - n) + '/01/01')).getTime()
            end = (value.indexOf('本') > -1 || value.indexOf('今') > -1) ? currentTime : (new Date((y - 1) + '/01/01')).getTime()
          }
          return {start, end}
        },
        /**
         * 日期格式化
         * @param start 起始日期
         * @param end 终止日期
         */
        changeDateFormat (start, end) {
          let startDate = new Date(start)
          let endDate = new Date(end)
          let year1 = startDate.getFullYear()
          let month1 = startDate.getMonth() + 1
          let day1 = startDate.getDate()
          let year2 = endDate.getFullYear()
          let month2 = endDate.getMonth() + 1
          let day2 = endDate.getDate()
          month1 < 10 ? month1 = '0' + month1 : month1
          month2 < 10 ? month2 = '0' + month2 : month2
          day1 < 10 ? day1 = '0' + day1 : day1
          day2 < 10 ? day2 = '0' + day2 : day2
//          this.initFunction()
          return {
            beginTime: year1 + '-' + month1 + '-' + day1,
            endTime: year2 + '-' + month2 + '-' + day2
          }
        },
        /**
         * 日期选择范围变更
         */
        dateChange (value) {
          if (value !== '自定义') {
            let index = this.piToOp.findIndex((item) => item === value)
            let start = this.opToPi[index]['beginTime']
            let end = this.opToPi[index]['endTime']
            let {beginTime, endTime} = this.changeDateFormat(start, end)
            this.picker.beginTime = beginTime
            this.picker.endTime = endTime
          }
        },
        /**
         * 日期范围变更
         */
        timeChange (value) {
          let ary = []
          value.map((item) => { // 并入数组
            item.replace(/(\d+)/g, ($1) => {
              ary.push($1)
            })
          })
          let _ranger = {// 合并需求
            beginTime: ary.splice(0, 3).join('-'),
            endTime: ary.join('-')
          }
          for (let i = 0; i < this.opToPi.length; i++) {
            let item = this.opToPi[i]
            console.log(item)
            if (item.beginTime === _ranger.beginTime && item.endTime === _ranger.endTime) {
              this.select.dimension = this.piToOp[i]
              return
            } else {
              this.select.dimension = '自定义'
            }
          }
          console.log(_ranger)
        }
      },
      created () {
        if (this.datePicker && this.datePicker.length > 0) { // 加载自定义日期选项
          this.select.options = []
          this.select.dimension = this.datePicker[0]// 默认选中第一项
          this.datePicker.forEach((item, index) => {
            let option = {'id': index + 1, 'name': item, 'value': item}
            this.select.options.push(option)
//            console.log(this.reducer(item))
            this.opToPi[index] = this.changeDateFormat(this.reducer(item).start, this.reducer(item).end);
            this.piToOp[index] = item;
          })
        } else {
          this.select.options.forEach((item, index) => {
            this.opToPi[index] = this.changeDateFormat(this.reducer(item.value).start, this.reducer(item.value).end);
            this.piToOp[index] = item.value;
          })
        }
        this.picker.beginTime = this.opToPi[0]['beginTime']
        this.picker.endTime = this.opToPi[0]['endTime']
      }
    }
</script>
<style scoped lang="scss">
  .dateRanger{
    margin-top:10px;
    .ivu-icon-ios-calendar-outline{
        top:2px;
    }
  }
</style>
