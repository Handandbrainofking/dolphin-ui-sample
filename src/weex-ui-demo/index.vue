<template>
  <div class="container">
    <wxc-minibar
      title="标题"
      background-color="#009ff0"
      text-color="#FFFFFF"
      :left-button="leftButton"
      right-text="更多"
      @wxcMinibarLeftButtonClicked="minibarLeftButtonClick"
      @wxcMinibarRightButtonClicked="minibarRightButtonClick"
    ></wxc-minibar>
    <wxc-tab-page
      ref="wxc-tab-page"
      :tab-titles="tabTitles"
      :tab-styles="tabStyles"
      title-type="icon"
      :tab-page-height="tabPageHeight"
      @wxcTabPageCurrentTabSelected="wxcTabPageCurrentTabSelected"
    >
      <list
        v-for="(v,index) in tabList"
        :key="index"
        class="item-container"
        :style="{ height: (tabPageHeight - tabStyles.height) + 'px' }"
      >
        <cell class="border-cell"></cell>
        <cell v-for="(demo,key) in v" class="cell" :key="key">
          <wxc-pan-item
            :ext-id="'1-' + (v) + '-' + (key)"
            url="https://h5.m.taobao.com/trip/ticket/detail/index.html?scenicId=2675"
            @wxcPanItemPan="wxcPanItemPan"
          >
            <div class="content">
              <text>{{key}}</text>
            </div>
          </wxc-pan-item>
        </cell>
      </list>
    </wxc-tab-page>
  </div>
</template>

<script>
// import Vue from 'vue'
import { WxcTabPage, WxcPanItem, Utils, BindEnv, WxcMinibar } from "weex-ui";
const dom = weex.requireModule("dom");
const modal = weex.requireModule("modal");
import Config from "./config";
export default {
  components: { WxcMinibar, WxcTabPage, WxcPanItem },
  data() {
    return {
      height: "100px",
      leftButton:
        "https://gw.alicdn.com/tfs/TB1cAYsbv2H8KJjy0FcXXaDlFXa-30-53.png",
      tabTitles: Config.tabTitles,
      tabStyles: Config.tabStyles,
      tabList: [],
      demoList: [1, 2, 3, 4, 5, 6, 7, 8, 9],
      tabPageHeight: 1334
    };
  },
  created() {
    this.tabPageHeight = Utils.env.getPageHeight();
    this.tabList = [...Array(this.tabTitles.length).keys()].map(i => []);
    // Vue.set(this.tabList, 0, this.demoList);
  },
  methods: {
    wxcTabPageCurrentTabSelected(e) {
      const self = this;
      const index = e.page;
      /* Unloaded tab analog data request */
      if (!Utils.isNonEmptyArray(self.tabList[index])) {
        setTimeout(() => {
          Vue.set(self.tabList, index, self.demoList);
        }, 100);
      }
    },
    wxcPanItemPan(e) {
      if (BindEnv.supportsEBForAndroid()) {
        this.$refs["wxc-tab-page"].bindExp(e.element);
      }
    },
    minibarLeftButtonClick() {},
    minibarRightButtonClick() {
      modal.toast({ message: "click rightButton!", duration: 1 });
    }
  }
};
</script>
<style scoped>
.item-container {
  width: 750px;
  background-color: #f2f3f4;
}

.border-cell {
  background-color: #f2f3f4;
  width: 750px;
  height: 24px;
  align-items: center;
  justify-content: center;
  border-bottom-width: 1px;
  border-style: solid;
  border-color: #e0e0e0;
}

.cell {
  background-color: #ffffff;
}

.content {
  width: 750px;
  height: 300px;
  border-bottom-width: 1px;
  align-items: center;
  justify-content: center;
}
</style>
