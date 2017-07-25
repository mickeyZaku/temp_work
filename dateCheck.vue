<template>
   <!--使用日期 start-->
        <div class="">
            <Row>
                <Col span="16">
                <Select v-model="yearSelect" style="width:88px;" @on-change="yearChange()">
                    <Option v-for="item in yearList" :value="item.value" :key="item">{{ item.label }}</Option>
                </Select>
                </Col>
                <Col span="8">
                  <Row >
                      <Col span="8"><span class="legend"><i class="blankColor"></i>非排期</span></Col>
                      <Col span="8"><span class="legend"><i class="unCheckColor"></i>未选排期</span></Col>
                      <Col span="8"><span class="legend"><i class="checkedColor"></i>已选排期</span></Col>
                  </Row>
                </Col>
            </Row>
            <br>
            <Tabs type="card" :animated="false" @on-click="tabsChange" v-model="monthList.name">
              <Tab-pane  v-for="(item, index) in monthList" :label="item.label" :key="monthList.length" :name="item.name">
                <table class="dateList">
                  <tr class="checkNum">
                    <td v-for="(num,index) in dayList"><span>{{index+1}}</span></td>
                  </tr>
                  <tr class="checkedCell">
                    <td v-for="(day, index) in dayList" :data-id="day" class="dateCheckBox blankColor" :class="{unCheckColor:day==1,checkedColor:day==2}" v-on:click="selectDays(index,day)"><span class="placeholder">{{day}}</span></td>
                  </tr>
                </table>
              </Tab-pane>
            </Tabs>
           <div class="choose-func">
             <span v-on:click="checkAllDays()">全选</span>
             <span v-on:click="resetAll()">重置</span>
           </div>
        </div>
        <!--使用日期 end -->
</template>
<script>
// var dateData = [
        //   '2016-12-01', '2016-12-02', '2016-12-03', '2016-12-11', '2016-12-12', '2016-12-13', '2016-12-14', '2017-01-07', '2017-01-08', '2017-01-09', '2017-01-21', '2017-01-22', '2017-01-23', '2017-01-24', '2017-01-25', '2017-02-11', '2017-02-12', '2017-02-13', '2017-02-23', '2017-02-24', '2017-02-25'
        // ]
export default {
  props: ['dateArr', 'dateSelect'],
  data () {
    return {
      yearSelect: '2017',
      result: [],
      yearList: [],
      monthList: [],
      dayList: [],
      dateObj: {},
      tabs: 6
//      dateArr_temp: '',
//      dateSelect_temp: ''
    }
  },
  methods: {
    setDate: function (data, selectDate) {
      this.monthList = [];
      this.dayList = [];
      this.dateObj = {};
//      未选排期
      if (selectDate != null && selectDate.length > 0) {
        var lent = selectDate.length;
        var thatnew = this;
        for (var ij = 0; ij < lent; ij++) {
          var itemnew = selectDate[ij].split('-');
//          console.log(itemnew)
          if (!thatnew.dateObj[itemnew[0]]) {
            thatnew.dateObj[itemnew[0]] = {};
          }
          var monthn = thatnew.dateObj[itemnew[0]][itemnew[1]];
          if (!monthn) {
            monthn = thatnew.dateObj[itemnew[0]][itemnew[1]] = [];
            var totaln = 31
            if (itemnew[1] === '04' || itemnew[1] === '06' || itemnew[1] === '09' || itemnew[1] === '11') {
              totaln = 30;
            } else if (itemnew[1] === '02') {
              if (this.isLeapYear(itemnew[0])) {
                totaln = 29
              } else {
                totaln = 28;
              }
            }
            for (var ih = 0; ih < totaln; ih++) {
              monthn.push(0);// 数据的状态 不可用 0 可用 1 选中是 2
            }
          }
          monthn[Number(itemnew[2]) - 1] = 1;// 不可用 0 可用 1 选中是 2
        }
      }

//    已选排期
      if (data != null && data.length > 0) {
        var len = data.length;
        var that = this;
        for (var i = 0; i < len; i++) {
          var item = data[i].split('-');
          if (!that.dateObj[item[0]]) {
            that.dateObj[item[0]] = {};
          }
          var month = that.dateObj[item[0]][item[1]];
          if (!month) { // 判断月份
            month = that.dateObj[item[0]][item[1]] = [];
            var total = 31;
            if (item[1] === '04' || item[1] === '06' || item[1] === '09' || item[1] === '11') {
              total = 30;
            } else if (item[1] === '02') {
              if (this.isLeapYear(item[0])) {
                total = 29
              } else {
                total = 28;
              }
            }
            for (var j = 0; j < total; j++) {
              month.push(0);// 数据的状态 不可用 0 可用 1 选中是 2
            }
          }
          month[Number(item[2]) - 1] = 2;// 不可用 0 可用 1 选中是 2
          // console.log(that.dateObj);
          if (i === len - 1) {
            this.initYear(that.dateObj);
          }
        }
      }
    },
    initYear: function (obj) {
      var yearArr = Object.keys(obj);
      for (var i = 0; i < yearArr.length; i++) {
        this.yearList[i] = {
          value: yearArr[i],
          label: yearArr[i]
        }
      }
      this.initMonth();
    },
    yearChange: function () {
      this.initMonth();
    },
    initMonth: function () {
      var monthArr = Object.keys(this.dateObj[this.yearSelect]).sort();
      var that = this;
      that.monthList = [];
      for (var i = 0; i < monthArr.length; i++) {
        if (monthArr[i].substring(0, 1) === '0') {
          that.monthList.push(
            {
              name: monthArr[i],
              label: monthArr[i].charAt(1) + '月'
            }
            );
//          that.$set(that.monthList, i, {
//            name: monthArr[i],
//            label: monthArr[i].charAt(1) + '月'
//          })
        } else {
          that.monthList.push(
            {
              name: monthArr[i],
              label: monthArr[i] + '月'
            }
          );
//          that.$set(that.monthList, i, {
//            name: monthArr[i],
//            label: monthArr[i] + '月'
//          })
        }
      }
      this.monthList.name = that.monthList[0].name;// 初始化Tabs的第一项的值
      this.initDay();
    },
    isLeapYear: function (Year) {
      if (((Year % 4) === 0) && ((Year % 100) !== 0) || ((Year % 400) === 0)) {
        return (true);
      } else {
        return (false);
      }
    },
    initDay: function () {
      this.dayList = this.dateObj[this.yearSelect][this.monthList.name];
      // 最重抛出的方法
      // this.getResult();
    },
    tabsChange: function () {
      // console.log(this.monthList.name);// 到这里啦tabschange的值时什么呀
      this.initDay();
    },
    selectDays: function (index, status) {
      // var st = status === 1 ? 2 : 1;
      if ((status !== 1) && (status !== 2)) { return };
      this.$set(this.dayList, index, (status === 1 ? 2 : 1));
      // this.getResult();
      // console.log(this.dateObj);
    },
    checkAllDays: function () { // 全选
      console.log('checkAll')
      for (var l = 0; l < this.dayList.length; l++) {
        if (this.dayList[l] === 1) {
          this.$set(this.dayList, l, 2);
        };
      }
      for (var i in this.dateObj) {
        var tmpYear = this.dateObj[i];
        for (var j in tmpYear) {
          var MonthListTmp = tmpYear[j];
          for (var k = 0; k < MonthListTmp.length; k++) {
            if (this.dateObj[i][j][k] === 1) {
              this.dateObj[i][j][k] = 2;
            }
          }
        }
      }
    },
    resetAll: function () { // 重置
      console.log('resetAll');
      for (var l = 0; l < this.dayList.length; l++) {
        if (this.dayList[l] === 2) {
          this.$set(this.dayList, l, 1);
        };
      }
      for (var i in this.dateObj) {
        var tmpYear = this.dateObj[i];
        for (var j in tmpYear) {
          var MonthListTmp = tmpYear[j];
          for (var k = 0; k < MonthListTmp.length; k++) {
            if (this.dateObj[i][j][k] === 2) {
              this.dateObj[i][j][k] = 1;
            }
          }
        }
      }
    },
    getResult: function () { // 获取选中的数据调用这个方法
      var temp = this.dateObj;
      var result = [];
      // console.log(temp);
      for (var i in temp) {
        for (var j in temp[i]) {
          for (var k = 0; k < temp[i][j].length; k++) {
            var dateItem = '';
            if (temp[i][j][k] === 2) {
              var tmpK = k + 1;
              if (tmpK < 10) {
                tmpK = '0' + tmpK;
              }
              dateItem = i + '-' + j + '-' + (tmpK);
              result.push(dateItem);
            }
          }
        }
      }
      console.log(result);
      return result;
    }
  },
  created: function () {
    this.setDate(this.dateArr, this.dateSelect);
  },
  watch: {
    dateArr: {
      handler: function () {
        this.monthList = []
        this.dayList = []
        this.setDate(this.dateArr, this.dateSelect);
        console.log(this.monthList, this.dayList)
      },
      deep: true
    },
    dateSelect: {
      handler: function () {
        this.setDate(this.dateArr, this.dateSelect);
        console.log(this.monthList, this.dayList)
      },
      deep: true
    }
  }
}
</script>

<style lang="scss">
    $blankColor:#edeef1;
    $unCheckColor:#fff;
    $checkedColor:#6A7088;
    .blankColor{
      background: $blankColor;
      /*<!--border: 1px solid $blankColor;-->*/
    }
    .unCheckColor{
      /*border: 1px solid #a8a8a8;*/
      background: $unCheckColor;
    }
    .checkedColor{
      background:$checkedColor;
      /*<!--border:1px solid $checkedColor;-->*/

    }
  .legend{
    position: absolute;
    left:-10px;
    color: #666;
    font-size: 12px;
    .unCheckColor{
      border: 1px solid #e5e5e5;
    }
    i{
      display: inline-block;
      width: 12px;
      height: 12px;
      margin-right:6px;
    }
  }

  .dateList{
    border-spacing: 0;
    tr{
       &.checkNum{
         td{
           border-top: 1px solid #fff;
           border-left: 1px solid #fff;
           border-bottom: 1px solid #fff;
           &:last-child{
             border-right: 1px solid #fff;
           }
         }
       }
       height:27px;
       td{
         border-top: 1px solid #dee1e5;
         border-left: 1px solid #dee1e5;
         border-bottom: 1px solid #dee1e5;
         padding: 0;
         text-align: center;
         background: #fff;
         width: 27px;
         height: 26px;
         &:last-child{
           border-right: 1px solid #dee1e5;
         }
         span{
           display: block;
           min-width: 18px;
           width: 27px;
           height: 26px;
           &.placeholder{
             text-indent: -9999px;
           }
         }
         &.blankColor{
           background: $blankColor;
         }

         &.unCheckColor{
           background: $unCheckColor;
         }
         &.checkedColor{
           background: $checkedColor;
         }
       }
     }
  }
  .choose-func{
    width: 60px;
    height: 16px;
    line-height: 16px;
    margin-top:7px;
    text-align: center;
    span{
      cursor: pointer;
      color:#3d70fb;
      margin-right:7px;
    }
  }
</style>

