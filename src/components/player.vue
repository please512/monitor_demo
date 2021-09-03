<template>
  <div
    v-if="visible"
    class="videoBox"
    :style="'width:' + videoWidth + 'px;height:' + (videoHeight+1) + 'px;'"
  >
    <video
      :poster="waitImg"
      :id="playerId"
      :ref="playerId"
      :style="'width:100%;display:flex;'"
    ></video>
    <div class="layer" :style="'height:' + boxHeight + 'px;'" @click="play">
      <div style="width: 100%; height: 100%; position: relative">
        <p
          :class="hadPlayed ? 'muted afterPlay' : 'muted beforePlay'"
          :style="'height:' + 25 + 'px;line-height:' + 25 + 'px;'"
          @click.stop="muted"
        >
          <span
            v-if="isMuted"
            class="iconfont icon-silence"
            style="color: #fff;"
          ></span>
          <span
            v-else
            class="iconfont icon-volume"
            style="color: #fff;"
          ></span>
        </p>
        <p
          :class="hadPlayed ? 'screen afterPlay' : 'screen beforePlay'"
          :style="'height:' + 25 + 'px;line-height:' + 25 + 'px;'"
          @click.stop="togglescreen"
        >
          <span
            v-if="isFullScreen"
            class="iconfont icon-tuichuquanping"
            style="color: #fff;"
          ></span>
          <span
            v-else
            class="iconfont icon-fullscreen-expand"
            style="color: #fff;"
          ></span>
        </p>
      </div>
    </div>
  </div>
</template>
<script>
import eventBus from '@/eventBus.js'
import videojs from "video.js";
import waitImg from '@/assets/images/loading.png' 
export default {
  props: {
    autoplay: {
      type: Boolean,
      default: false,
    },
    src: {
      type: String,
      default: "",
    },
    playerId: {
      type: String,
      default: "",
    },
    videoWidth: {
      type: Number,
      default: 0,
    },
    selectScreenBlock:{
      type:String,
      default:'',
    }
  },
  watch: {
    src() {
      if (this.src) {
        if(this.selectScreenBlock=='1' && Object.keys(this.player).length==1){
          console.log('切换视频源！')
          this.player[Object.keys(this.player)[0]].src(this.src)
          return
        }
        this.visible = true;
        let that = this;
        that.isMuted = that.autoplay ? true : false;
        that.hadPlayed = that.autoplay ? true : false;
        this.videoHeight = parseInt(this.videoWidth) * 0.5625;
        setTimeout(() => {
          that.initPlayer();
        });
      }
    },
  },
  data() {
    return {
      visible: false,
      player: {},
      isMuted: false,
      isPlaying: false,
      videoHeight: "",
      boxHeight: 0,
      hadPlayed: false,
      isFullScreen: false,
      waitImg:waitImg,
    };
  },
  mounted() {
    console.log(this.player)
    eventBus.$on('haveCleanedPlayers',()=>{
      this.player={}
      eventBus.$emit('players',null)
    })
  },
  methods: {
    destoryAllPlayer(){
      console.log('销毁播放器')
      for(let id in this.player){
        if(this.player[id]){
          this.player[this.playerId].dispose()
        }
      }
    },
    togglescreen() {
      if(this.selectScreenBlock=='1' && Object.keys(this.player).length==1){
        console.log('单屏全屏后取消暂停功能！')
        if (this.player[Object.keys(this.player)[0]].paused()) {
          this.player[Object.keys(this.player)[0]].play();
          this.isPlaying = true;
          this.hadPlayed = true;
        }
        this.player[Object.keys(this.player)[0]].requestFullscreen();
      }else{
        if (this.player[this.playerId].paused()) {
          this.player[this.playerId].play();
          this.isPlaying = true;
          this.hadPlayed = true;
        }
        this.player[this.playerId].requestFullscreen();
      }
    },
    play() {
      this.hadPlayed = true;
      if(this.selectScreenBlock=='1' && Object.keys(this.player).length==1){
        console.log('单屏播放或暂停！')
        if (this.player[Object.keys(this.player)[0]].paused()) {
          this.player[Object.keys(this.player)[0]].play(false);
          this.isPlaying = false;
        } else {
          //静音操作
          this.player[Object.keys(this.player)[0]].pause(true);
          this.isPlaying = true;
        }
      }else{
        if (this.player[this.playerId].paused()) {
          this.player[this.playerId].play();
          this.isPlaying = true;
        } else {
          this.player[this.playerId].pause();
          this.isPlaying = false;
        }
      }
    },
    muted() {
      if(this.selectScreenBlock=='1' && Object.keys(this.player).length==1){
        console.log('单屏静音或取消静音！')
        if (this.player[Object.keys(this.player)[0]].muted()) {
          this.player[Object.keys(this.player)[0]].muted(false);
          this.isMuted = false;
        } else {
          //静音操作
          this.player[Object.keys(this.player)[0]].muted(true);
          this.isMuted = true;
        }
      }else{
        if (this.player[this.playerId].muted()) {
          this.player[this.playerId].muted(false);
          this.isMuted = false;
        } else {
          //静音操作
          this.player[this.playerId].muted(true);
          this.isMuted = true;
        }
      }
    },
    initPlayer() {
      let component = this;
      this.player[this.playerId] = videojs(
        this.playerId,
        {
          autoplay: component.autoplay ? true : false,
          muted: component.autoplay ? true : false,
          sources: [
            {
              src: this.src,
              type: "application/x-mpegURL",
            },
          ],
        },
        function () {
          component.createLayer();
          console.log(this)
          this.on("click", () => {
            console.log('退出全屏')
            this.exitFullscreen();
          });
          this.on("tap", () => {
            console.log('退出全屏')
            this.exitFullscreen();
          });
          // setTimeout(()=>{
          //   component.player[component.playerId].dispose()
          //   // console.log()
          // },3000)
        }
      );
      console.log(this.player)
      eventBus.$emit('players',this.player)
    },
    createLayer() {
      this.boxHeight = parseInt(this.videoHeight);
    },
  },
};
</script>
<style scoped>
/deep/.vjs-loading-spinner,
/deep/.vjs-big-play-button,
/deep/.vjs-control-bar,
/deep/.vjs-modal-dialog {
  display: none !important;
}
/deep/.vjs-playing,
/deep/.vjs-paused {
  height: auto !important;
}
.videoBox {
  position: relative;
  border: 1px #fff solid;
}
.layer {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  color: #fff;
}
.muted {
  width: 25px;
  position: absolute;
  right: 30PX;
  bottom: 0;
  text-align: center;
}
.screen {
  width: 25px;
  position: absolute;
  right: 5PX;
  bottom: 0;
  text-align: center;
}
.beforePlay {
  background: rgb(51, 51, 51);
}
.afterPlay {
  background: transparent;
}
.iconfont{
  
  font-size: 25px;
}
</style>