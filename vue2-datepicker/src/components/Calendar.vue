<template>
  <div class="health-datepicker">
    <!-- 底線 -->
    <div class="health-line"></div>
    <div class="formInput formDatePicker">
      <date-picker
        :lang="lang"
        v-model="time"
        :clearable="false"
        :popup-class="'datepickerPopup'"
        :editable="false"
        format="YYYY/M/D"
        :value-type="dateFormat"
        :inline="true"
        :title-format="'YYYY/M/D'"
        @panel-change="openPanel"
        @input="changePanel"
        @calendar-change="changePanel"
      >
        <template v-slot:header>
          <div class="mx-slot-header">
            <div class="mx-slot-header-list">
              <label class="mx-slot-header-title">月達標累計</label>
              <label class="cl-flex cl-align-center mx-slot-header-num">
                <span 
                 :style="{color: options.monthlyTargetValue === '-' ? options.content.minusColor : options.content.mainColor, paddingBottom: options.monthlyTargetValue === '-' ? '8px' : ''}" 
                 class="mx-slot-header-num1">
                  {{ options.monthlyTargetValue }}
                </span>
                <span>次</span>
              </label>
            </div>
            <div class="mx-slot-header-list">
              <label class="mx-slot-header-title">月累計任務值</label>
              <label class="mx-slot-header-num">
                <span class="mx-slot-header-num2">{{ options.monthlyCumulativeValue }}</span>
              </label>
            </div>
          </div>
        </template>
        <template v-slot:footer>
          <div class="mx-slot-footer">
            <img
                :src="
                  `assets/health/images/information.png` | bindCDNPath
                "
                alt="information"
                class="mx-slot-footer-info"
                @click="checkBeacon()"
              />
            <div class="mx-slot-list" style="margin-bottom: 11px;" v-if="isWalk">
              <span class="mx-slot-footer-num mx-slot-footer-num-step">
                5,000
              </span>
              <label class="mx-slot-footer-txt">達5,000步</label>
              <span class="mx-slot-footer-num mx-slot-footer-num-step">7,500</span>
              <label class="mx-slot-footer-txt">達7,500步</label>
            </div>
            <div class="mx-slot-list" style="margin-bottom: 9px;" v-if="isHeart">
              <div>
                <p class="cl-flex cl-align-center" style="margin-bottom: 10px;">
                  <span class="mx-slot-footer-num mx-slot-footer-num-heart-rate">
                    20
                  </span>
                  <label class="mx-slot-footer-txt">達運動心率維持20分鐘</label>
                </p>
                <p class="cl-flex cl-align-center">
                  <span class="mx-slot-footer-num mx-slot-footer-num-heart-rate">30</span>
                  <label class="mx-slot-footer-txt">達運動心率維持30分鐘</label>
                </p>
              </div>
            </div>
            <div class="mx-slot-list" style="margin-bottom: 9px;" v-if="isSleep">
              <img
                :src="
                  `assets/health/images/Ellipse_184.png` | bindCDNPath
                "
                alt="達標"
                class="mx-slot-footer-img"
              />
              <label class="mx-slot-footer-txt">達標</label>
              <img
                :src="
                  `assets/health/images/Ellipse_199.png` | bindCDNPath
                "
                alt="未達標"
                class="mx-slot-footer-img"
              />
              <label class="mx-slot-footer-txt">未達標</label>
            </div>
            <div class="mx-slot-list" style="margin-bottom: 9px;" v-if="isGYM">
              <img
                :src="
                  `assets/health/images/Ellipse_185.png` | bindCDNPath
                "
                alt="達標"
                class="mx-slot-footer-img"
              />
              <label class="mx-slot-footer-txt">達標</label>
              <img
                :src="
                  `assets/health/images/Ellipse_199.png` | bindCDNPath
                "
                alt="未達標"
                class="mx-slot-footer-img"
              />
              <label class="mx-slot-footer-txt">未達標</label>
            </div>
            <div class="mx-slot-list" style="margin-bottom: 9px;" v-if="isAerobic">
              <img
                :src="
                  `assets/health/images/Ellipse_186.png` | bindCDNPath
                "
                alt="達標"
                class="mx-slot-footer-img"
              />
              <label class="mx-slot-footer-txt">達標</label>
              <img
                :src="
                  `assets/health/images/Ellipse_199.png` | bindCDNPath
                "
                alt="未達標"
                class="mx-slot-footer-img"
              />
              <label class="mx-slot-footer-txt">未達標</label>
            </div>
            <div class="mx-slot-list">
              <img
                :src="
                  `assets/health/images/Ellipse_199.png` | bindCDNPath
                "
                alt="未達標"
                class="mx-slot-footer-img"
                v-if="isWalk"
              />
              <label 
                class="mx-slot-footer-txt"
                v-if="isWalk"
              >
                未達標
              </label>
              <img
                :src="
                  `assets/health/images/Ellipse_199.png` | bindCDNPath
                "
                alt="未達標"
                class="mx-slot-footer-img"
                v-if="isHeart"
              />
              <label 
                class="mx-slot-footer-txt"
                v-if="isHeart"
              >
                未達標
              </label>
              <img
                :src="
                  `assets/health/images/Ellipse_198.png` | bindCDNPath
                "
                alt="無效"
                class="mx-slot-footer-img"
              />
              <label class="mx-slot-footer-txt">無效（逾期上傳/手動修改）</label>
            </div>
          </div>
          <button 
            class="mx-slot-footer-btn" 
            type="button" 
            :style="{color: options.content.mainColor, background: options.goalDescription.background}"
          >
            查看數據統計
          </button>
        </template>
      </date-picker>
  
      <!--狀態說明popup-->
      <div class="popupTop popupBg" v-show="goalMsgPopup">
        <div class="popupBox">
          <div class="popupContents">
            <template>
              <div>
                <div class="cl-mb-12">
                  <!-- 驚嘆號-提示插圖 icon 動畫 -->
                  <div
                    id="notice"
                  ></div>
                </div>
                <div class="mx-popup-box">
                  <h3>狀態說明</h3>
                  
                  <ul>
                    <!-- 步數 -->
                    <li class="cl-flex cl-align-center cl-mb-12"  v-if="isWalk">
                      <span class="mx-slot-footer-num mx-slot-footer-num-step">5,000</span>
                      <span class="mx-slot-footer-txt">達成階段目標(5,000步)</span>
                    </li>
                    <li class="cl-flex cl-align-center cl-mb-12"  v-if="isWalk">
                      <span class="mx-slot-footer-num mx-slot-footer-num-step">7,500</span>
                      <span class="mx-slot-footer-txt">達成最終目標(7,500步)</span>
                    </li>
                    <li class="cl-flex cl-align-center cl-mb-12"  v-if="isWalk">
                      <img
                        :src="
                          `assets/health/images/Ellipse_199.png` | bindCDNPath
                        "
                        alt="未達標"
                        class="mx-slot-footer-img"
                      />
                      <label class="mx-slot-footer-txt">未達階段目標(5,000步)</label>
                    </li>
                    <!-- end 步數 -->
                    <!-- 心率 -->
                    <li class="cl-flex cl-align-center cl-mb-12"  v-if="isHeart">
                      <span class="mx-slot-footer-num mx-slot-footer-num-heart-rate">20</span>
                      <span class="mx-slot-footer-txt">達成初級目標<br>(達運動心率維持20分鐘)</span>
                    </li>
                    <li class="cl-flex cl-align-center cl-mb-12"  v-if="isHeart">
                      <span class="mx-slot-footer-num mx-slot-footer-num-heart-rate">30</span>
                      <span class="mx-slot-footer-txt">達成進階目標<br>(達運動心率維持30分鐘)</span>
                    </li>
                    <li class="cl-flex cl-align-center cl-mb-12"  v-if="isHeart">
                      <img
                        :src="
                          `assets/health/images/Ellipse_199.png` | bindCDNPath
                        "
                        alt="未達標"
                        class="mx-slot-footer-img"
                      />
                      <label class="mx-slot-footer-txt">未達標：未達任一目標</label>
                    </li>
                    <!-- end 心率 -->
                    <!-- 睡眠 -->
                    <li class="cl-flex cl-align-center cl-mb-12"  v-if="isSleep">
                      <img
                        :src="
                          `assets/health/images/Ellipse_184.png` | bindCDNPath
                        "
                        alt="達標"
                        class="mx-slot-footer-img"
                      />
                      <span class="mx-slot-footer-txt">達標：達成目標<br>(睡眠達6小時(含)以上未超過9小時)</span>
                    </li>
                    <li class="cl-flex cl-align-center cl-mb-12"  v-if="isSleep">
                      <img
                        :src="
                          `assets/health/images/Ellipse_199.png` | bindCDNPath
                        "
                        alt="未達標"
                        class="mx-slot-footer-img"
                      />
                      <label class="mx-slot-footer-txt">未達標：未達目標</label>
                    </li>
                    <!-- end 睡眠 -->
                    <!-- 健身場館 -->
                    <li class="cl-flex cl-align-center cl-mb-12"  v-if="isGYM">
                      <img
                        :src="
                          `assets/health/images/Ellipse_185.png` | bindCDNPath
                        "
                        alt="達標"
                        class="mx-slot-footer-img"
                      />
                      <span class="mx-slot-footer-txt">達標：達平台設定標準</span>
                    </li>
                    <li class="cl-flex cl-align-center cl-mb-12"  v-if="isGYM">
                      <img
                        :src="
                          `assets/health/images/Ellipse_199.png` | bindCDNPath
                        "
                        alt="未達標"
                        class="mx-slot-footer-img"
                      />
                      <label class="mx-slot-footer-txt">未達標：未達目標</label>
                    </li>
                    <!-- end 健身場館 -->
                    <!-- 有氧運動 -->
                    <li class="cl-flex cl-align-center cl-mb-12"  v-if="isAerobic">
                      <img
                        :src="
                          `assets/health/images/Ellipse_186.png` | bindCDNPath
                        "
                        alt="達標"
                        class="mx-slot-footer-img"
                      />
                      <span class="mx-slot-footer-txt">達成目標<br>(在指定健身房使用有氧運動器材達20分鐘)</span>
                    </li>
                    <li class="cl-flex cl-align-center cl-mb-12"  v-if="isAerobic">
                      <img
                        :src="
                          `assets/health/images/Ellipse_199.png` | bindCDNPath
                        "
                        alt="未達標"
                        class="mx-slot-footer-img"
                      />
                      <label class="mx-slot-footer-txt">未達標：未達目標</label>
                    </li>
                    <!-- end 有氧運動 -->
                    <li>
                      <div class="cl-flex cl-align-center mx-mb-4">
                        <img
                          :src="
                            `assets/health/images/Ellipse_198.png` | bindCDNPath
                          "
                          alt="無效"
                          class="mx-slot-footer-img"
                        />
                        <label class="mx-slot-footer-txt">無效數據：</label>
                      </div>
                      <ol class="mx-popup-txt">
                        <li class="mx-slot-footer-txt mx-mb-4" style="margin-right: 0;">1. 逾期上傳：上傳健康數據時，7日內 (含當日)之資料達標時發放任務值，超過7日之達標資料則顯示逾期，且不發送任務值</li>
                        <li class="mx-slot-footer-txt mx-mb-4" style="margin-right: 0;">2. 手動修改：若任務數據有手動修改的軌跡，不列入成績計算，任務數據將不顯示</li>
                      </ol>
                    </li>
                  </ul>
                </div>
                <div>
                  <a
                    href="javascript:void(0);"
                    class="btn02 btnColorGreen01"
                    style="margin: 0;"
                    @click="goalMsgPopup = false"
                  >
                    知道了
                  </a>
                </div>
              </div>
            </template>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import lottie from "lottie-web";
import notice from "@/pages/health/assets/animation/notice.json";
// 禁止點擊非當月的日期
// handleCellClick()
// node_modules/vue2-datepicker/index.esm.js 需隱藏 1311 行, "click": _vm.handleCellClick
import DatePicker from 'vue2-datepicker'
import 'vue2-datepicker/locale/zh-tw'
import $ from 'jquery'

export default {
  name: "Calendar",
  components: {
    DatePicker
  },
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
    options: {
      type: [Object],
      default: true
    },
    changePageType: {
      type: [String],
      default: true
    },
  },
  data() {
    return {
      time: this.value,
      // lang: 覆蓋默認語言選項
      // 變更星期
      lang: {
        formatLocale: {
          firstDayOfWeek: 0,
        },
        yearFormat: 'YYYY',
        monthFormat: 'MM',
      },
      goalMsgPopup: false,
    }
  },
  mounted() {
    this.optionSettings();
    this.hideNotCurrentMonth();
    $('.mx-btn-icon-left').on('click', () => {
      this.hideNotCurrentMonth();
    });
    $('.mx-btn-icon-right').on('click', () => {
      this.hideNotCurrentMonth();
    });
  },
  updated() {
    this.optionSettings();
    this.optionYearAndMonth();
  },
  computed: {
    isWalk() {
      return this.changePageType === '4';
    },
    isHeart() {
      return this.changePageType === '5';
    },
    isSleep() {
      return this.changePageType === '6';
    },
    isGYM() {
      return this.changePageType === '1';
    },
    isAerobic() {
      return this.changePageType === '2';
    },
  },
  methods: {
    // 當日曆面板改變時
    openPanel() {
      this.optionYearAndMonth();
    },
    // 當改變年月時
    changePanel(date) {
      this.hideNotCurrentMonth();
      setTimeout(() => {
        // 清除所有樣式
        $('.mx-table-date').find('.cell').each(function(k,v) {
          $(v).html(`<div>${v.title.split('/')[2]}</div>`);
          if ($(this).text() === '逾期') {
            $(v).html(`<div>${v.title.split('/')[2]}</div>`);
          } 
          if ($(this).text() === '手動') {
            $(v).html(`<div>${v.title.split('/')[2]}</div>`);
          }
          if ($(this).find('div').hasClass('active')) {
            $(this).find('div').removeClass('active');
          } 
          if ($(this).find('div').hasClass('unactive')) {
            $(this).find('div').removeClass('unactive');
          } 
        })
        this.panelSettings(date);
      }, 0)
    },
    optionYearAndMonth() {
      setTimeout(() => {
        if (this.isWalk) {
          this.yearAndMonthChangeColor();
        } else if (this.isHeart) {
          this.yearAndMonthChangeColor();
        } else if (this.isSleep) {
          this.yearAndMonthChangeColor();
        } else if (this.isGYM) {
          this.yearAndMonthChangeColor();
        } else if (this.isAerobic) {
          this.yearAndMonthChangeColor();
        }
      }, 0);
    },
    // 年和月的顏色切換
    yearAndMonthChangeColor() {
      $('.mx-table-year').find('.cell.active').css('background', this.options.content.mainColor);
      $('.mx-table-month').find('.cell.active').css('background', this.options.content.mainColor);
    },
    optionSettings() {
      const date = this.time && this.time.length > 0 ? this.time : new Date()
      this.panelSettings(date);
    },
    panelSettings(date) {
      this.options.settings.overdue.forEach(item => {
        if (
          new Date(item).getMonth() === new Date(date).getMonth() &&
          new Date(item).getFullYear() === new Date(date).getFullYear()
        ) {
          $('.cell').not('.not-current-month').each((k,v) => {
            if (v.title === item) {
              $(v).html('<div class="invalid">逾期</div>');
            }
          })
        }
      })
      this.options.settings.manual.forEach(item => {
        if (
          new Date(item).getMonth() === new Date(date).getMonth() &&
          new Date(item).getFullYear() === new Date(date).getFullYear()
        ) {
          $('.cell').not('.not-current-month').each((k,v) => {
            if (v.title === item) {
              $(v).html('<div class="invalid">手動</div>');
            }
          })
        }
      })
      this.options.settings.active.forEach((item, idx) => {
        if (
          new Date(item).getMonth() === new Date(date).getMonth() &&
          new Date(item).getFullYear() === new Date(date).getFullYear()
        ) {
          $('.cell').not('.not-current-month').each((k,v) => {
            if (v.title === item) {
              if (this.isWalk) {
                // 步數
                $(v).find('div').addClass('active').css('background', this.options.content.mainColor);
                
                $(v).append(`<div class="mx-slot-score" style="color: #2196f3; border: 1px solid #2196f3;">${parseInt(this.options.settings.score[idx]).toLocaleString()}</div>`);
              } else if (this.isHeart) {
                // 心率
                $(v).find('div').addClass('active').css('background', this.options.content.mainColor);
                
                $(v).append(`<div class="mx-slot-score" style="color: #FF7043; border: 1px solid #FF7043;">${parseInt(this.options.settings.score[idx]).toLocaleString()}</div>`);
              } else if (this.isSleep) {
                // 睡眠
                $(v).find('div').addClass('active').css('background', this.options.content.mainColor);
              } else if (this.isGYM) {
                // 健身場館
                $(v).find('div').addClass('active').css('background', this.options.content.mainColor);
              } else if (this.isAerobic) {
                // 有氧運動
                $(v).find('div').addClass('active').css('background', this.options.content.mainColor);
              }
            }
          })
          // 移除分數
          $('.mx-slot-score.active').remove();
        }
      })
      this.options.settings.unactive.forEach(item => {
        if (
          new Date(item).getMonth() === new Date(date).getMonth() &&
          new Date(item).getFullYear() === new Date(date).getFullYear()
        ) {
          $('.cell').not('.not-current-month').each((k,v) => {
            if (v.title === item) {
              $(v).find('div').addClass('unactive');
            }
          })
        }
      })
    },
    checkBeacon() {
      this.goalMsgPopup = true;
      $('#notice').find('svg').remove();
      // 驚嘆號-提示插圖 icon 動畫
      this.renderNoticeAnimation();
    },
    // 驚嘆號-提示插圖 icon 動畫
    renderNoticeAnimation() {
      lottie.loadAnimation({
          // 動畫要渲染在哪個 DOM 元素
          container: document.getElementById("notice"),
          renderer: "svg",
          loop: false,
          autoplay: true,
          animationData: notice // 動畫 json 路徑
      });
    },
    // 清除非當前月份的日期
    hideNotCurrentMonth() {
      setTimeout(() => {
        if ($('.mx-table-date').find('tr').eq(1).find('.not-current-month').length === 7) {
          $('.mx-table-date').find('tr').eq(1).hide();
        } else {
          $('.mx-table-date').find('tr').eq(1).show();
        }
        if ($('.mx-table-date').find('tr').eq(6).find('.not-current-month').length === 7) {
          $('.mx-table-date').find('tr').eq(6).hide();
        } else {
          $('.mx-table-date').find('tr').eq(6).show();
        }
      }, 0);
    },
  }
}
</script>
<style lang="scss">
@import '~vue2-datepicker/scss/index.scss';

.mx-icon-calendar {
  display: none;
}
.formDatePicker {
  text-align: center;
}
.mx-datepicker-inline {
    width: 100%;
}
.mx-calendar {
  width: 100%;
  height: auto;
  padding: 0;
}
.mx-slot-header {
  position: relative;
  top: 51px;
  display: flex;
}
.mx-slot-header-list {
  width: calc(100% / 2);
  padding: 0 12px;
  border-left: 1px solid #eff0f2;
  &:first-child {
    border-left: 0;
  }
}
.mx-slot-header-title {
  font-weight: 400;
  font-size: 14px;
  line-height: 20px;
  letter-spacing: 0.3px;
  color: #878787; 
  display: block;
  text-align: left;
}
.mx-slot-header-num {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  font-weight: 400;
  font-size: 13px;
  line-height: 18px;
  letter-spacing: 0.3px;
  color: #878787;
  height: 34px; 
}
.mx-slot-header-num1 {
  margin-right: 4px;
  font-weight: 500;
  font-size: 32px;
  line-height: 38px;
}
.mx-slot-header-num2 {
  font-weight: 500;
  font-size: 24px;
  line-height: 34px;
  letter-spacing: 0.3px;
  color: #444d4d;
}
.mx-datepicker-header {
  padding: 0;
  height: 120px;
  border: 0;
}
.mx-btn-icon-left {
  width: 28px;
  height: 28px;
  background: #f8f8f8;
  border-radius: 8px;
  display: flex;
  align-items: center;
  .mx-icon-left {
    &::before {
      width: 14px;
      height: 14px;
      top: 0;
      left: 5px;
      color: #878787;
    }
  }
}
.mx-btn-icon-right {
  width: 28px;
  height: 28px;
  background: #f8f8f8;
  border-radius: 8px;
  display: flex;
  align-items: center;
  .mx-icon-right {
    &::before {
      width: 14px;
      height: 14px;
      top: 0;
      color: #878787;
    }
  }
}
.mx-calendar-header {
  position: relative;
  top: -120px;
}
.mx-calendar-header-label {
  .mx-btn-text, span {
    font-weight: 500;
    font-size: 20px;
    line-height: 28px;
    letter-spacing: 0.3px;
    color: #444b4d;
  }
}
.mx-btn-current-year {
  padding: 0;
  font-weight: 500;
  font-size: 20px;
  line-height: 38px;
  letter-spacing: 0.3px;
  color: #444d4d;
  &::after {
    content: '/';
  }
}
.mx-btn-current-month {
  padding: 0;
  font-weight: 500;
  font-size: 20px;
  line-height: 38px;
  letter-spacing: 0.3px;
  color: #444d4d;
}
.mx-calendar-content {
  top: -20px;
  height: auto;
}
.mx-table-year, .mx-table-month {
  tr {
    height: 36px;
  }
  .cell {
    &.active {
      border-radius: 99em;
    }
  }
}
.mx-table-date {
  td, th {
    text-align: center;
    font-weight: 400;
    font-size: 13px;
    letter-spacing: 0.3px;
    height: 44px;
    position: relative;
  }
  th {
    color: #444b4d;
    
  }
  td {
    color: #b1b1b1;
  }
  .cell {
    &.active {
      background: transparent;
      color: #b1b1b1;
    }
    // 分數
    .mx-slot-score {
      font-weight: 500;
      font-size: 10px;
      line-height: 14px;
      text-align: center;
      letter-spacing: 0.3px;
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 0px 4px;
      background: #FFFFFF;
      border-radius: 8px;
      transform: scale(0.83);
      max-width: 43px;
      height: 17px;
      position: absolute;
      left: 0;
      right: 0;
      bottom: -2px;
      margin: auto;
      z-index: 1;
      width: calc(36px / 0.83);
      height: calc(14px / 0.83);
    }
    // 達標
    .active {
      display: flex;
      align-items: center;
      justify-content: center;
      font-weight: 500;
      border-radius: 99em;
      width: 32px;
      height: 32px;
      margin: 0 auto;
      color: #ffffff;
    }
    // 無效
    .invalid {
      display: flex;
      align-items: center;
      justify-content: center;
      border-radius: 99em;
      width: calc(32px / 0.91);
      height: calc(32px / 0.91);
      margin: 0 auto;
      background: #eff0f2;
      color: #444b4d;
      font-size: 11px;
      font-weight: 400;
      letter-spacing: 0.3px;
      transform: scale(0.91);
      color: #444b4d;
    }
    // 未達標
    .unactive {
      display: flex;
      align-items: center;
      justify-content: center;
      border-radius: 99em;
      width: 32px;
      height: 32px;
      margin: 0 auto;
      border: 1px solid #eaeaea;
      color: #b1b1b1;
    }
  }
  .not-current-month > div {
    display: none;
  }
}
.mx-btn-icon-double-left {
  display: none;
}
.mx-btn-icon-double-right {
  display: none;
}
.mx-datepicker-main {
  border: 0;
}
.mx-datepicker-footer {
  border: 0;
  position: relative;
  top: -15px;
}
.mx-slot-footer {
  border: 1px solid #E3F2FD;
  padding: 8px 12px;
  border-radius: 8px;
  position: relative;
  text-align: left;
  margin-bottom: 24px;
  .mx-slot-footer-info {
    width: 16px;
    height: 16px;
    position: absolute;
    top: 8px;
    right: 12px;
  }
  .mx-slot-list {
    display: flex;
    align-items: center;
    &:last-child {
      margin-bottom: 0;
    }
  }
}
.mx-slot-footer-btn {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 12px 10px;
  width: 240px;
  height: 42px;
  border-radius: 8px;
  font-weight: 500;
  font-size: 18px;
  line-height: 18px;
  border: 0;
  margin: 0 auto;
}
.mx-slot-footer-img {
  width: 16px;
  height: 16px;
  display: inline-block;
  margin-right: 4px;
}
.mx-slot-footer-num {
  font-weight: 500;
  font-size: 10px;
  line-height: 14px;
  letter-spacing: 0.3px;
  padding: 0px 4px;
  display: inline-block;
  border-radius: 8px;
  margin-right: 4px;
  transform: scale(0.83);
  width: calc(36px / 0.83);
  height: calc(14px / 0.83);
  text-align: center;
  background: #FFFFFF;
}
.mx-slot-footer-num-step {
  color:#2196F3;
  border: 1px solid #2196F3;
}
.mx-slot-footer-num-heart-rate {
  color:#FF7043;
  border: 1px solid #FF7043;
}
.mx-slot-footer-txt {
  font-weight: 400;
  font-size: 13px;
  line-height: 18px;
  letter-spacing: 0.3px;
  color: #878787;
  margin-right: 12px;
}
.popupContents {
    padding: 24px 24px 20px;
    width: auto;
    text-align: left;
    h3 {
      margin: 0 0 12px;
    }
}
.mx-popup-box {
  padding: 16px 12px;
  background: #F8F8F8;
  border-radius: 12px;
  margin-bottom: 24px;
}
.mx-popup-txt {
  padding-left: 20px;
}
.mx-mb-4 {
  margin-bottom: 4px;
}
</style>
