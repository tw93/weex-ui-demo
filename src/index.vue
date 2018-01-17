<template>
    <div class="wx-page">
        <scroller class="scroller">
            <div class="text-content">
                <text class="text">行程小助手</text>
            </div>
            <wxc-searchbar></wxc-searchbar>
            <wxc-cell label="日期选择"
                      :title="currentDate"
                      :has-arrow="true"
                      @wxcCellClicked="dateChooseClicked"></wxc-cell>

            <wxc-cell label="更多选择"
                      :has-arrow="true"
                      @wxcCellClicked="moreChooseClicked"></wxc-cell>

            <wxc-cell label="是否往返" :has-arrow="false">
                <switch slot="value"></switch>
            </wxc-cell>

            <div class="item-button">
                <wxc-button type="yellow"
                            text="去查询"
                            @wxcButtonClicked="showLoading"></wxc-button>
            </div>

            <div class="item-button">
                <wxc-button type="red"
                            text="出错啦"
                            @wxcButtonClicked="showError"></wxc-button>
            </div>

        </scroller>
        <wxc-popup width="500"
                   pos="right"
                   :show="isShowPop"
                   @wxcPopupOverlayClicked="overlayClicked">
            <text class="more-text">加入更多选择</text>
        </wxc-popup>
        <wxc-page-calendar ref="wxcPageCalendar"
                           :date-range="dateRange"
                           :selected-date="selectedDate"
                           :selected-note="selectedNote"
                           :minibar-cfg="minibarCfg"
                           :desc-list="descList"
                           @wxcPageCalendarDateSelected="pageCalendarDateSelected"
        ></wxc-page-calendar>
        <wxc-mask height="800"
                  width="702"
                  :show="isShowMask"
                  @wxcMaskSetHidden="maskSetHidden">
            <scroller class="content">
                <text class="mask-title">你的行程安排如下</text>
                <text class="mask-content">京都:
                    上午游览伏见稻荷大社，走一走千本鸟居，下午游览三十三间堂，参观完后有时间可以逛一逛锦市场，不过部分商铺会在17:00左右打烊，这是一处专卖京都特产的地方，有“京都厨房”的美称，傍晚时分去到京都著名的花街——先斗町，这里紧邻鸭川，吃过饭后可以沿着鸭川逛一圈。</text>
                <text class="mask-content">大阪:
                    大阪市内的交通以地铁为主，单程票价14元以上，每日乘坐地铁多次建议购买大阪地铁一日卡，大阪周游卡也兼具交通功能，持该卡则无需购买地铁卡，周游卡还包含了28个市内景点的门票，每天游览2、3个景点便可值回票价。
                    除此之外，大阪市内交通可使用关西地区的ICOCA一卡通（余额可退），更适用于需要往返于多个城市游玩的蜂蜂。</text>
            </scroller>
        </wxc-mask>
        <wxc-result type="errorPage"
                    :show="isShowError"
                    @wxcResultButtonClicked="resultButtonClicked">
        </wxc-result>
        <wxc-loading :show="isShowLoading"
                     loading-text="正在加载中"></wxc-loading>
    </div>
</template>

<script>
  import {
    WxcSearchbar,
    WxcButton,
    WxcPopup,
    WxcCell,
    WxcResult,
    WxcPageCalendar,
    WxcIndexlist,
    WxcLoading,
    WxcMask
  } from 'weex-ui';

  const modal = weex.requireModule('modal');

  module.exports = {
    components: {
      WxcSearchbar,
      WxcButton,
      WxcPopup,
      WxcCell,
      WxcResult,
      WxcPageCalendar,
      WxcIndexlist,
      WxcLoading,
      WxcMask
    },
    data: () => ({
      isShowPop: false,
      isShowError: false,
      isShowLoading: false,
      isShowMask: false,
      currentDate: '2017-01-20',
      selectedDate: ['2017-01-20'],
      calendarTitle: '选择日期',
      dateRange: ['2018-01-01', '2018-04-30'],
      selectedNote: ['出发'],
      minibarCfg: {
        title: '日期选择',
        backgroundColor: "#EEEEEE"
      },
      descList: [
        { date: '2017-12-23', value: '￥200' },
        { date: '2017-12-24', value: '￥200' },
        { date: '2017-12-25', value: '￥200' },
        { date: '2017-12-26', value: '￥200' },
        { date: '2017-12-27', value: '￥222' },
        { date: '2017-12-28', value: '￥341' },
        { date: '2017-12-29', value: '￥230' },
        { date: '2017-12-06', value: '￥2000', emphasize: true }
      ]
    }),
    methods: {
      moreChooseClicked () {
        this.isShowPop = true;
      },
      overlayClicked () {
        this.isShowPop = false;
      },
      cellClicked () {
        modal.toast({
          'message': 'wxcCellClicked',
          'duration': 1
        })
      },
      showError () {
        this.isShowError = true;
      },
      showLoading () {
        this.isShowLoading = true;
        setTimeout(() => {
          this.isShowLoading = false;
          this.isShowMask = true;
        }, 1000);
      },
      resultButtonClicked () {
        this.isShowError = false;
      },
      dateChooseClicked (e) {
        setTimeout(() => {
          this.$refs['wxcPageCalendar'].show();
        }, 1);
      },
      pageCalendarDateSelected (e) {
        this.selectedDate = e.date;
        this.currentDate = e.date[0];
      },
      maskSetHidden () {
        this.isShowMask = false;
      }
    }
  };
</script>

<style scoped>
    .wx-page {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
    }

    .scroller {
        flex: 1;
    }

    .item-button {
        margin-top: 40px;
        align-items: center;
    }

    .content{
        height: 800px;
    }
    .text-content {
        height: 140px;
        margin-top: 40px;
        margin-bottom: 10px;
        align-items: center;
        justify-content: center;
    }

    .text {
        font-size: 40px;
        color: #333333;
    }

    .more-text {
        font-size: 30px;
        margin-top: 140px;
        margin-left: 80px;
    }

    .mask-title {
        font-size: 40px;
        margin-top: 40px;
        margin-bottom: 30px;
        align-items: center;
        margin-left: 40px;
    }

    .mask-content {
        padding-top: 10px;
        padding-left: 40px;
        padding-right: 40px;
        font-size: 30px;
    }
</style>
