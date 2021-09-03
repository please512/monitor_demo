<template>
  <div class="right" ref="box">
    <div style="display:flex;flex-wrap:wrap;justify-content:center;align-items:center;">
      <div
        :style="
          'width:' +
          width_4 +
          'px;height:50%;display:flex;justify-content:center;align-items:center;margin:18px;'
        "
      >
        <p v-if="empty_4_1" :style="'width:'+width_4+'px;height:'+(box_height_4+1)+'px;border:1px #fff solid;color:#fff;text-align:center;line-height:'+box_height_4+1+'px;background:#000;'">暂无信号</p>
        <Player
          :src="src_4_1"
          :playerId="playerId_4_1"
          :autoplay="true"
          :videoWidth="width_4"
          v-if="playerId_4_1"
          selectScreenBlock="4"
        />
      </div>
      <div
        :style="
          'width:' +
          width_4 +
          'px;height:50%;display:flex;justify-content:center;align-items:center;margin:18px;'
        "
      >
        <p v-if="empty_4_2" :style="'width:'+width_4+'px;height:'+(box_height_4+1)+'px;border:1px #fff solid;color:#fff;text-align:center;line-height:'+box_height_4+1+'px;background:#000;'">暂无信号</p>
        <Player
          :src="src_4_2"
          :playerId="playerId_4_2"
          :autoplay="true"
          :videoWidth="width_4"
          v-if="playerId_4_2"
          selectScreenBlock="4"
        />
      </div>
      <div
        :style="
          'width:' +
          width_4 +
          'px;height:50%;display:flex;justify-content:center;align-items:center;margin:18px;'
        "
      >
        <p v-if="empty_4_3" :style="'width:'+width_4+'px;height:'+(box_height_4+1)+'px;border:1px #fff solid;color:#fff;text-align:center;line-height:'+box_height_4+1+'px;background:#000;'">暂无信号</p>
        <Player
          :src="src_4_3"
          :playerId="playerId_4_3"
          :autoplay="true"
          :videoWidth="width_4"
          v-if="playerId_4_3"
          selectScreenBlock="4"
        />
      </div>
      <div
        :style="
          'width:' +
          width_4 +
          'px;height:50%;display:flex;justify-content:center;align-items:center;margin:18px;'
        "
      >
        <p v-if="empty_4_4" :style="'width:'+width_4+'px;height:'+(box_height_4+1)+'px;border:1px #fff solid;color:#fff;text-align:center;line-height:'+box_height_4+1+'px;background:#000;'">暂无信号</p>
        <Player
          :src="src_4_4"
          :playerId="playerId_4_4"
          :autoplay="true"
          :videoWidth="width_4"
          v-if="playerId_4_4"
          selectScreenBlock="4"
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
      playerId_4_1: "",
      playerId_4_2: "",
      playerId_4_3: "",
      playerId_4_4: "",
      width_4: 0,
      box_height_4:0,
      empty_4_1: true,
      empty_4_2: true,
      empty_4_3: true,
      empty_4_4: true,
      src_4_1: "",
      src_4_2: "",
      src_4_3: "",
      src_4_4: "",
    };
  },
  methods: {
    closeDialog() {
      this.$emit("closeDialog");
    },
    initData() {
      this.playerId_4_1 = "";
      this.playerId_4_2 = "";
      this.playerId_4_3 = "";
      this.playerId_4_4 = "";
      this.empty_4_1 = true;
      this.empty_4_2 = true;
      this.empty_4_3 = true;
      this.empty_4_4 = true;
      this.width_4 = 0;
      this.src_4_1 = "";
      this.src_4_2 = "";
      this.src_4_3 = "";
      this.src_4_4 = "";
      this.fourScreen();
    },
    fourScreen() {
      let num = 1025 / 576;
      let height = this.$refs.box.offsetHeight / 2.5;
      this.width_4 = this.$refs.box.offsetWidth / 2.5;
      this.box_height_4 = parseInt(this.width_4) * 0.5625;
      if (this.width_4 / height > num) {
        this.width_4 = (1025 / 576) * height - 10;
      } else {
        this.width_4 = this.$refs.box.offsetWidth / 2.5;
      }
    },
    startPlayer(data) {
      let that = this;
      if (!this.src_4_1) {
        this.playerId_4_1 =
          "video" + parseInt(Math.random() * 10000000000000000).toString();
        setTimeout(() => {
          that.src_4_1 = data.m3u8_address;
          that.empty_4_1 = false;
        }, 500);
      } else if (!this.src_4_2) {
        this.playerId_4_2 =
          "video" + parseInt(Math.random() * 10000000000000000).toString();
        setTimeout(() => {
          that.src_4_2 = data.m3u8_address;
          that.empty_4_2 = false;
        }, 500);
      } else if (!this.src_4_3) {
        this.playerId_4_3 =
          "video" + parseInt(Math.random() * 10000000000000000).toString();
        setTimeout(() => {
          that.src_4_3 = data.m3u8_address;
          that.empty_4_3 = false;
        }, 500);
      } else {
        this.playerId_4_4 =
          "video" + parseInt(Math.random() * 10000000000000000).toString();
        setTimeout(() => {
          that.src_4_4 = data.m3u8_address;
          that.empty_4_4 = false;
        }, 500);
      }
    },
  },
};
</script>