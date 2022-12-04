<template>
  <div class="formInput formDatePicker">
    <date-picker
      v-model="time"
      :clearable="false"
      :popup-class="'datepickerPopup'"
      :editable="false"
      v-bind="$attrs"
      format="YYYY/M/D"
      :value-type="dateFormat"
      :inline="true"
      :title-format="'YYYY/M/D'"
      @input="inputDate"
      @open="open"
      @calendar-change="changePanel"
    >
    <!-- <date-picker
      v-model="time"
      :clearable="false"
      :popup-class="'datepickerPopup'"
      :editable="false"
      v-bind="$attrs"
      format="YYYY/MM/DD"
      :value-type="dateFormat"
      :inline="true"
      @input="inputDate"
      @open="open"
      @calendar-change="changePanel"
    > -->
      <template v-slot:header>
        <label>月達標累計</label>
        <label style="color: blue">200</label>
        <br />
        <label>月累計任務值</label>
        <label style="color: blue">6000</label>
      </template>
      <template v-slot:footer>
        <input type="radio" id="no" name="radioGroup" value="no" />
        <label for="no">未達標</label>
        <input type="radio" id="yes" name="radioGroup" value="yes" />
        <label for="yes">達標</label>
        <div class="btns mt-0">
          <button class="btn btn-primary" type="button">查看數據統計</button>
        </div>
      </template>
    </date-picker>
  </div>
</template>
<script>
import DatePicker from 'vue2-datepicker'
import 'vue2-datepicker/locale/zh-tw'
import $ from 'jquery'
// import { vuexStore } from '@/mixins'
export default {
  components: {
    DatePicker
  },
  /** 使用(data-* attribute）將特定時間內容改為中文
   * 月份年份調整時先將更改過的文字內容替換成日期，再替換要顯示的文字
   * 日期下方顯示的內容請依照套版為主，此為示意內容，若一樣使用::after請注意選月份＆年時下方也會顯示
   * 元件相關資料請參考https://github.com/mengxiong10/vue2-datepicker/blob/master/README.zh-CN.md
   */
  // mixins: [vuexStore],
  inheritAttrs: false,
  props: {
    value: {
      type: [Array, String],
      default: () => []
    },
    dateFormat: {
      type: String,
      default: 'YYYY/M/D'
    },
  },
  data() {
    return {
      time: this.value,
      // 顯示中文的日期
      checkDate: ['2022/11/11', '2022/12/11', '2022/12/12'],
      
    }
  },
  mounted() {
    const date = this.time && this.time.length > 0 ? this.time : new Date()
    this.checkDate.forEach(item => {
      if (
        new Date(item).getMonth() === new Date(date).getMonth() &&
        new Date(item).getFullYear() === new Date(date).getFullYear()
      ) {
        // let tem = '[data-day=' + new Date(item).getDate() + ']'
        // $('.cell').filter(tem)[0].innerText = '逾期']
        $('.cell').each((k,v) => {
          if (v.title.indexOf(item) > -1) {
            v.innerText = '逾期';
          }
        })
      }
    })
  },
  computed: {
  },
  watch: {
    value(newValue) {
      this.time = newValue
    }
  },
  methods: {
    inputDate() {
      this.$emit('input', this.time)
    },
    open() {
      setTimeout(() => {
        const date = this.time && this.time.length > 0 ? this.time : new Date()
        this.checkDate.forEach(item => {
          if (
            new Date(item).getMonth() === new Date(date).getMonth() &&
            new Date(item).getFullYear() === new Date(date).getFullYear()
          ) {
            // let tem = '[data-day=' + new Date(item).getDate() + ']'
            // $('.cell').filter(tem)[0].innerText = '逾期'
            $('.cell').each((k,v) => {
              if (v.title.indexOf(item) > -1) {
                v.innerText = '逾期';
              }
            })
          }
        })
      }, 0)
    },
    changePanel(date) {
      setTimeout(() => {
        // const count = new Date(
        //   new Date(date).getFullYear(),
        //   new Date(date).getMonth() + 1,
        //   0
        // ).getDate()
        // for (let i = 1; i <= count; i++) {
          // console.log('i :>> ', i);
          // let tem = '[title=' + i + ']'
          // if ($('.cell').filter(tem)[0].innerText === '逾期') {
            //   $('.cell').filter(tem)[0].innerHTML = '<div>' + i + '</div>'
          // }
        // }
        $('.mx-table-date').find('.cell').not($(".not-current-month")).each(function(k,v) {
          // console.log(v.title.split('/')[2]);
          if ($(this).text() === '逾期') {
            v.innerHTML = '<div>' + v.title.split('/')[2] + '</div>';
          } else {
            v.innerHTML = '<div>' + v.title.split('/')[2] + '</div>';
          }
        })
        this.checkDate.forEach(item => {
          if (
            new Date(item).getMonth() === new Date(date).getMonth() &&
            new Date(item).getFullYear() === new Date(date).getFullYear()
          ) {
            // let tem = '[data-day=' + new Date(item).getDate() + ']'
            // $('.cell').filter(tem)[0].innerText = '逾期'
            $('.cell').each((k,v) => {
              if (v.title.indexOf(item) > -1) {
                v.innerText = '逾期';
              }
            })
          }
        })
      }, 0)
    }
  }
}
</script>
<style lang="scss">
@import '~vue2-datepicker/scss/index.scss';

.mx-icon-calendar {
  display: none;
}
.formDatePicker .mx-calendar-content .cell ::after {
  content: '\A 20';
  white-space: pre;
  color: blue;
}

.formDatePicker .mx-calendar-content .cell.not-current-month ::after {
  content: '';
}
.mx-calendar {
  height: 300px;
}
</style>
