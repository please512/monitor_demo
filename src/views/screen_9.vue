<template>
  <div class="right" ref="box">
    <div style="display:flex;flex-wrap:wrap;justify-content:center;align-items:center;">
      <div
        :style="
          'width:' +
          width_9 +
          'px;height:30%;display:flex;justify-content:center;align-items:center;margin:5px;'
        "
      >
        <p v-if="empty_9_1" :style="'width:'+width_9+'px;height:'+(box_height_9+1)+'px;border:1px #fff solid;color:#fff;text-align:center;line-height:'+box_height_9+1+'px;background:#000;'">暂无信号</p>
        <Player
          :src="src_9_1"
          :playerId="playerId_9_1"
          :autoplay="true"
          :videoWidth="width_9"
          v-if="playerId_9_1"
          selectScreenBlock="9"
        />
      </div>
      <div
        :style="
          'width:' +
          width_9 +
          'px;height:30%;display:flex;justify-content:center;align-items:center;margin:5px;'
        "
      >
        <p v-if="empty_9_2" :style="'width:'+width_9+'px;height:'+(box_height_9+1)+'px;border:1px #fff solid;color:#fff;text-align:center;line-height:'+box_height_9+1+'px;background:#000;'">暂无信号</p>
        <Player
          :src="src_9_2"
          :playerId="playerId_9_2"
          :autoplay="true"
          :videoWidth="width_9"
          v-if="playerId_9_2"
          selectScreenBlock="9"
        />
      </div>
      <div
        :style="
          'width:' +
          width_9 +
          'px;height:30%;display:flex;justify-content:center;align-items:center;margin:5px;'
        "
      >
        <p v-if="empty_9_3" :style="'width:'+width_9+'px;height:'+(box_height_9+1)+'px;border:1px #fff solid;color:#fff;text-align:center;line-height:'+box_height_9+1+'px;background:#000;'">暂无信号</p>
        <Player
          :src="src_9_3"
          :playerId="playerId_9_3"
          :autoplay="true"
          :videoWidth="width_9"
          v-if="playerId_9_3"
          selectScreenBlock="9"
        />
      </div>
      <div
        :style="
          'width:' +
          width_9 +
          'px;height:30%;display:flex;justify-content:center;align-items:center;margin:5px;'
        "
      >
        <p v-if="empty_9_4" :style="'width:'+width_9+'px;height:'+(box_height_9+1)+'px;border:1px #fff solid;color:#fff;text-align:center;line-height:'+box_height_9+1+'px;background:#000;'">暂无信号</p>
        <Player
          :src="src_9_4"
          :playerId="playerId_9_4"
          :autoplay="true"
          :videoWidth="width_9"
          v-if="playerId_9_4"
          selectScreenBlock="9"
        />
      </div>
      <div
        :style="
          'width:' +
          width_9 +
          'px;height:30%;display:flex;justify-content:center;align-items:center;margin:5px;'
        "
      >
        <p v-if="empty_9_5" :style="'width:'+width_9+'px;height:'+(box_height_9+1)+'px;border:1px #fff solid;color:#fff;text-align:center;line-height:'+box_height_9+1+'px;background:#000;'">暂无信号</p>
        <Player
          :src="src_9_5"
          :playerId="playerId_9_5"
          :autoplay="true"
          :videoWidth="width_9"
          v-if="playerId_9_5"
          selectScreenBlock="9"
        />
      </div>
      <div
        :style="
          'width:' +
          width_9 +
          'px;height:30%;display:flex;justify-content:center;align-items:center;margin:5px;'
        "
      >
        <p v-if="empty_9_6" :style="'width:'+width_9+'px;height:'+(box_height_9+1)+'px;border:1px #fff solid;color:#fff;text-align:center;line-height:'+box_height_9+1+'px;background:#000;'">暂无信号</p>
        <Player
          :src="src_9_6"
          :playerId="playerId_9_6"
          :autoplay="true"
          :videoWidth="width_9"
          v-if="playerId_9_6"
          selectScreenBlock="9"
        />
      </div>
      <div
        :style="
          'width:' +
          width_9 +
          'px;height:30%;display:flex;justify-content:center;align-items:center;margin:5px;'
        "
      >
        <p v-if="empty_9_7" :style="'width:'+width_9+'px;height:'+(box_height_9+1)+'px;border:1px #fff solid;color:#fff;text-align:center;line-height:'+box_height_9+1+'px;background:#000;'">暂无信号</p>
        <Player
          :src="src_9_7"
          :playerId="playerId_9_7"
          :autoplay="true"
          :videoWidth="width_9"
          v-if="playerId_9_7"
          selectScreenBlock="9"
        />
      </div>
      <div
        :style="
          'width:' +
          width_9 +
          'px;height:30%;display:flex;justify-content:center;align-items:center;margin:5px;'
        "
      >
        <p v-if="empty_9_8" :style="'width:'+width_9+'px;height:'+(box_height_9+1)+'px;border:1px #fff solid;color:#fff;text-align:center;line-height:'+box_height_9+1+'px;background:#000;'">暂无信号</p>
        <Player
          :src="src_9_8"
          :playerId="playerId_9_8"
          :autoplay="true"
          :videoWidth="width_9"
          v-if="playerId_9_8"
          selectScreenBlock="9"
        />
      </div>
      <div
        :style="
          'width:' +
          width_9 +
          'px;height:30%;display:flex;justify-content:center;align-items:center;margin:5px;'
        "
      >
        <p v-if="empty_9_9" :style="'width:'+width_9+'px;height:'+(box_height_9+1)+'px;border:1px #fff solid;color:#fff;text-align:center;line-height:'+box_height_9+1+'px;background:#000;'">暂无信号</p>
        <Player
          :src="src_9_9"
          :playerId="playerId_9_9"
          :autoplay="true"
          :videoWidth="width_9"
          v-if="playerId_9_9"
          selectScreenBlock="9"
        />
      </div>
    </div>
  </div>
</template>
<script>
import Player from "@/components/player.vue";
export default {
  components: {
    Player,
  },
  props: {
    dialogVisible: {
      type: Boolean,
      default: false,
    },
  },
  watch: {
    dialogVisible() {
      if (this.dialogVisible) {
        this.dialogHeight = this.dialogWidth * 0.5625;
      }
    },
  },
  data() {
    return {
      dialogWidth: 980,
      dialogHeight: 0,
      playerId_9_1: "",
      playerId_9_2: "",
      playerId_9_3: "",
      playerId_9_4: "",
      playerId_9_5: "",
      playerId_9_6: "",
      playerId_9_7: "",
      playerId_9_8: "",
      playerId_9_9: "",
      width_9: 0,
      box_height_9:0,
      empty_9_1: true,
      empty_9_2: true,
      empty_9_3: true,
      empty_9_4: true,
      empty_9_5: true,
      empty_9_6: true,
      empty_9_7: true,
      empty_9_8: true,
      empty_9_9: true,
      src_9_1: "",
      src_9_2: "",
      src_9_3: "",
      src_9_4: "",
      src_9_5: "",
      src_9_6: "",
      src_9_7: "",
      src_9_8: "",
      src_9_9: "",
    };
  },
  methods: {
    closeDialog() {
      this.$emit("closeDialog");
    },
    initData() {
      this.playerId_9_1 = "";
      this.playerId_9_2 = "";
      this.playerId_9_3 = "";
      this.playerId_9_4 = "";
      this.playerId_9_5 = "";
      this.playerId_9_6 = "";
      this.playerId_9_7 = "";
      this.playerId_9_8 = "";
      this.playerId_9_9 = "";
      this.empty_9_1 = true;
      this.empty_9_2 = true;
      this.empty_9_3 = true;
      this.empty_9_4 = true;
      this.empty_9_5 = true;
      this.empty_9_6 = true;
      this.empty_9_7 = true;
      this.empty_9_8 = true;
      this.empty_9_9 = true;
      this.width_9 = 0;
      this.src_9_1 = "";
      this.src_9_2 = "";
      this.src_9_3 = "";
      this.src_9_4 = "";
      this.src_9_5 = "";
      this.src_9_6 = "";
      this.src_9_7 = "";
      this.src_9_8 = "";
      this.src_9_9 = "";
      this.nineScreen();
    },
    nineScreen() {
      let num = 1025 / 576;
      let height = this.$refs.box.offsetHeight / 3.1;
      this.width_9 = this.$refs.box.offsetWidth / 3.2;
      this.box_height_9 = parseInt(this.width_9) * 0.5625;
      if (this.width_9 / height > num) {
        this.width_9 = (1025 / 576) * height - 10;
      } else {
        this.width_9 = this.$refs.box.offsetWidth / 3.2;
      }
    },
    startPlayer(data) {
      let that = this;
      if (!this.src_9_1) {
        this.playerId_9_1 =
          "video" + parseInt(Math.random() * 10000000000000000).toString();
        setTimeout(() => {
          that.src_9_1 = data.m3u8_address;
          that.empty_9_1 = false;
        }, 500);
      } else if (!this.src_9_2) {
        this.playerId_9_2 =
          "video" + parseInt(Math.random() * 10000000000000000).toString();
        setTimeout(() => {
          that.src_9_2 = data.m3u8_address;
          that.empty_9_2 = false;
        }, 500);
      } else if (!this.src_9_3) {
        this.playerId_9_3 =
          "video" + parseInt(Math.random() * 10000000000000000).toString();
        setTimeout(() => {
          that.src_9_3 = data.m3u8_address;
          that.empty_9_3 = false;
        }, 500);
      } else if (!this.src_9_4) {
        this.playerId_9_4 =
          "video" + parseInt(Math.random() * 10000000000000000).toString();
        setTimeout(() => {
          that.src_9_4 = data.m3u8_address;
          that.empty_9_4 = false;
        }, 500);
      } else if (!this.src_9_5) {
        this.playerId_9_5 =
          "video" + parseInt(Math.random() * 10000000000000000).toString();
        setTimeout(() => {
          that.src_9_5 = data.m3u8_address;
          that.empty_9_5 = false;
        }, 500);
      } else if (!this.src_9_6) {
        this.playerId_9_6 =
          "video" + parseInt(Math.random() * 10000000000000000).toString();
        setTimeout(() => {
          that.src_9_6 = data.m3u8_address;
          that.empty_9_6 = false;
        }, 500);
      } else if (!this.src_9_7) {
        this.playerId_9_7 =
          "video" + parseInt(Math.random() * 10000000000000000).toString();
        setTimeout(() => {
          that.src_9_7 = data.m3u8_address;
          that.empty_9_7 = false;
        }, 500);
      } else if (!this.src_9_8) {
        this.playerId_9_8 =
          "video" + parseInt(Math.random() * 10000000000000000).toString();
        setTimeout(() => {
          that.src_9_8 = data.m3u8_address;
          that.empty_9_8 = false;
        }, 500);
      } else {
        this.playerId_9_9 =
          "video" + parseInt(Math.random() * 10000000000000000).toString();
        setTimeout(() => {
          that.src_9_9 = data.m3u8_address;
          that.empty_9_9 = false;
        }, 500);
      }
    },
  },
};
</script>