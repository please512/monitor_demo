<template>
  <div>
    <header>地铁人防门监控演示</header>
    <div class="btnbox">
      <el-button-group>
        <el-button
          @click="checkBtn(item)"
          :type="selectScreenBlock == item.block ? 'primary' : 'info'"
          v-for="(item, index) in btnList"
          :key="index"
          :class="'iconfont ' + item.icon"
        >
          <span style="margin-left: 5px">{{ item.name }}</span>
        </el-button>
      </el-button-group>
    </div>
    <section>
      <aside>
        <div class="left">
          <el-input
            style="bordder-radius: 0"
            placeholder="输入摄像头名称进行过滤"
            v-model="filterText"
          >
          </el-input>
          <el-tree
            class="filter-tree tree"
            :data="data"
            :props="defaultProps"
            default-expand-all
            :filter-node-method="filterNode"
            @node-click="handleNodeClick"
            ref="tree"
          >
            <span class="custom-tree-node" slot-scope="{ data }">
              <span>
                <i
                  :class="data.icon"
                  style="font-size: 18px; color: #409eff; margin-right: 10px"
                ></i
                >{{ data.deviceName }}
              </span>
            </span>
          </el-tree>
        </div>
      </aside>
      <aside>
        <!-- 1分屏 -->
        <div class="right" ref="box" v-if="selectScreenBlock == 1">
          <div style="display:flex;flex-wrap:wrap;justify-content:center;align-items:center;">
            <p v-if="empty" :style="'width:'+(width-36)+'px;height:'+(box_height+1)+'px;border:1px #fff solid;color:#fff;text-align:center;line-height:'+box_height+1+'px;background:#000;'">暂无信号</p>
            <Player
              :src="src"
              :playerId="playerId"
              :autoplay="true"
              :videoWidth="(width-36)"
              v-if="playerId"
              selectScreenBlock="1"
            />
          </div>
        </div>
        <!-- 4分屏 -->
        <FourScreen ref="fourScreen"  v-if="selectScreenBlock == 4"/>
        <!-- 9分屏 -->
        <NineScreen ref="nineScreen"  v-if="selectScreenBlock == 9"/>
      </aside>
    </section>
    <footer>
      <div class="footer_wrap">
        <el-table
          :data="tableData"
          empty-text="暂无异常"
          height="100%"
          border
          style="width: 100%"
        >
          <el-table-column prop="devicename" align="center" label="设备名称">
          </el-table-column>
          <el-table-column prop="position" align="center" label="设备位置">
          </el-table-column>
          <el-table-column prop="time" align="center" label="异常时间">
          </el-table-column>
          <el-table-column fixed="right" align="center" label="操作">
            <template slot-scope="scope">
              <el-button
                @click="handleClick(scope.row)"
                type="primary"
                size="small"
                class="iconfont icon-video"
                style="width: 90px"
              >
                <span style="margin-left: 5px">异常回放</span>
              </el-button>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </footer>
    <Dialog :dialogVisible="visible" @closeDialog="closeDialog"/>
  </div>
</template>
<script>
import eventBus from "@/eventBus.js";
import axios from "axios";
import Player from "@/components/player.vue";
import Dialog from './record.vue'
import FourScreen from './screen_4.vue'
import NineScreen from './screen_9.vue'
export default {
  components: {
    Player,
    Dialog,
    FourScreen,
    NineScreen,
  },
  data() {
    return {
      // 测试源地址
      // https://yunqivedio.alicdn.com/2017yq/v2/0x0/96d79d3f5400514a6883869399708e11/96d79d3f5400514a6883869399708e11.m3u8
      // http://42.81.125.31/liveplay-kk.rtxapp.com/live/program/live/hnwshd/4000000/mnf.m3u8
      // http://yztv.wshls.homecdn.com/live/1bd71.m3u8?t=1629685909&k=27487a3e7664e0fe407e481dec4ad96b&wsSession=60fbf0cc81f9fdfbf1a09cb8-162968827137820&wsIPSercert=948abae1806698ac331911346f0a4fb6&wsMonitor=0
      // https://tcdn.itouchtv.cn/live/gdws.m3u8?t_token=36ff7fe0001c4c486264ed9948e26a47-vkywSFqvs41vSOV%2B0AP%2FsZDTTX%2BEOydBtdQLNtclm12nNdw%2BKQrLDL02DM0mg85koXf1nHLBO0wQg%2FWD9AtK5K0DZeA%2BMDkcXMcHJrYal5AcjU2Fy0jjZnNxw6OfyEqOXXgvsDgeC3kDY09whuR6Xf8nKXN%2FXNqe2UOtG3J3yLlHqoenEQ1p%2BCT44H1u7Q%2B8
      // http://livealone302.iqilu.com/iqilu/sdtv.m3u8
      // http://stream.qhbtv.com/qhds/sd/live.m3u8?_upt=2fd6bbea1629696173
      filterText: "",
      data: [],
      defaultProps: {
        children: "children",
        label: "deviceName",
      },
      btnList: [
        {
          name: "单屏",
          icon: "icon-yipingshitu",
          block: 1,
        },
        {
          name: "四分屏",
          icon: "icon-sipingshitu",
          block: 4,
        },
        {
          name: "九分屏",
          icon: "icon-jiupingshitu",
          block: 9,
        },
      ],
      selectScreenBlock: "1",
      playerId: "",
      
      width: 0,
      box_height:0,
      empty: true,
      src: "",
      tableData: [
        {
          devicename: "实验室摄像头1",
          position: "实验室门口",
          time: "2021-08-31 09:23:34:234",
        },
      ],
      currentPlayers: [],
      visible:false,
    };
  },
  watch: {
    filterText(val) {
      this.$refs.tree.filter(val);
    },
  },
  mounted() {
    this.renderScreen();
    this.getCameraList();
    // this.src = "http://172.17.31.203:8080/live/livestream.m3u8";
    // this.src1 = "http://172.17.31.203:8080/live/livestream.m3u8";
    // this.src2 = "http://livealone302.iqilu.com/iqilu/sdtv.m3u8";
    // this.src3 = "http://172.17.31.203:8080/live/livestream.m3u8";
    let that = this;
    eventBus.$on("players", (player) => {
      if(player){
        that.currentPlayers.push(player);
        console.log(that.currentPlayers);
      }else{
        that.currentPlayers=[]
      }
    });
  },
  methods: {
    closeDialog(){
      this.visible=false
    },
    handleClick() {
      this.visible=true
    },
    renderScreen() {
      if (this.selectScreenBlock == 1) {
        this.width = 0;
        this.empty = true;
        this.playerId = "";
        this.src = "";
        this.oneScreen();
      }
      if (this.selectScreenBlock == 4) {
        this.$refs.fourScreen.initData()
      }
      if (this.selectScreenBlock == 9) {
        this.$refs.nineScreen.initData()
      }
    },
    checkBtn(item) {
      let that=this
      if(this.currentPlayers.length){
        this.currentPlayers.forEach(s=>{
          for(let id in s){
            console.log(s[id])
            s[id].dispose()
          }
        })
      }
      setTimeout(()=>{
        eventBus.$emit('haveCleanedPlayers')
        that.selectScreenBlock = item.block;
        that.$nextTick(()=>{
          that.renderScreen();
        })
      })
    },
    oneScreen() {
      let num = 1025 / 576;
      let height = this.$refs.box.offsetHeight;
      this.width = this.$refs.box.offsetWidth;
      this.box_height = parseInt(this.width-36) * 0.5625;
      if (this.width / height > num) {
        this.width = (1025 / 576) * height - 10;
      } else {
        this.width = this.$refs.box.offsetWidth;
      }
    },
    handleNodeClick(data) {
      console.log(data.m3u8_address);
      let that = this;
      if (this.selectScreenBlock == 1) {
        if (data.m3u8_address) {
          this.playerId =
            "video" + parseInt(Math.random() * 10000000000000000).toString();
          setTimeout(() => {
            that.src = data.m3u8_address;
            that.empty = false;
          }, 500);
        }
      }
      if (this.selectScreenBlock == 4) {
        if (data.m3u8_address) {
          this.$refs.fourScreen.startPlayer(data)
        }
      }
      if (this.selectScreenBlock == 9) {
        if (data.m3u8_address) {
          this.$refs.nineScreen.startPlayer(data)
        }
      }
    },
    filterNode(value, data) {
      if (!value) return true;
      return data.label.indexOf(value) !== -1;
    },
    getCameraList() {
      let children = [];
      children = children.concat([
          {
            id: 4,
            deviceName: "阿里-云频道",
            icon: "el-icon-video-camera-solid",
            m3u8_address:
              "https://yunqivedio.alicdn.com/2017yq/v2/0x0/96d79d3f5400514a6883869399708e11/96d79d3f5400514a6883869399708e11.m3u8",
            children: [],
          },
          {
            id: 5,
            deviceName: "湖南-卫视频道",
            icon: "el-icon-video-camera-solid",
            m3u8_address:
              "http://42.81.125.31/liveplay-kk.rtxapp.com/live/program/live/hnwshd/4000000/mnf.m3u8",
            children: [],
          },
          {
            id: 8,
            deviceName: "山东-卫视频道",
            icon: "el-icon-video-camera-solid",
            m3u8_address: "http://livealone302.iqilu.com/iqilu/sdtv.m3u8",
            children: [],
          },
          {
            id: 9,
            deviceName: "青海-都市频道",
            icon: "el-icon-video-camera-solid",
            m3u8_address:
              "http://stream.qhbtv.com/qhds/sd/live.m3u8?_upt=2fd6bbea1629696173",
            children: [],
          },
          {
            id: 11,
            deviceName: "CCTV6-电影频道",
            icon: "el-icon-video-camera-solid",
            m3u8_address:
              "https://hlslive.1905.com/live/LIVEQYR8M567KGOXO/index.m3u8?tm=1631635536&sign=3be119d0bae9481279e76e7259ebbba0",
            children: [],
          },
          {
            id: 13,
            deviceName: "山东-体育频道",
            icon: "el-icon-video-camera-solid",
            m3u8_address:
              "http://livealone302.iqilu.com/iqilu/typd.m3u8",
            children: [],
          },
          {
            id: 14,
            deviceName: "海南-卫视频道",
            icon: "el-icon-video-camera-solid",
            m3u8_address:
              "http://stream1.hnntv.cn/lywsgq/sd/live.m3u8?_upt=7f83d2021630467300",
            children: [],
          },
          {
            id: 15,
            deviceName: "浙江-新闻频道",
            icon: "el-icon-video-camera-solid",
            m3u8_address:
              "http://yd-vl.cztv.com/channels/lantian/channel001/360p.m3u8?a=1000&d=72628cf9087479cd58b88946c87249d1&k=1060f4d60596cb8fd6c8bb8afb3ceaf5&t=1630460277",
            children: [],
          },
          {
            id: 17,
            deviceName: "山东-齐鲁频道",
            icon: "el-icon-video-camera-solid",
            m3u8_address:
              "http://livealone302.iqilu.com/iqilu/qlpd.m3u8",
            children: [],
          },
          {
            id: 20,
            deviceName: "杭州-综合频道",
            icon: "el-icon-video-camera-solid",
            m3u8_address:
              "https://live.hoolo.tv/hztv1/sd/live.m3u8",
            children: [],
          },
          {
            id: 21,
            deviceName: "杭州-西湖频道",
            icon: "el-icon-video-camera-solid",
            m3u8_address:
              "https://live.hoolo.tv/hztv2/sd/live.m3u8",
            children: [],
          },
          {
            id: 28,
            deviceName: "山东-文旅频道",
            icon: "el-icon-video-camera-solid",
            m3u8_address:
              "http://livealone302.iqilu.com/iqilu/yspd.m3u8",
            children: [],
          },
        ]);
        this.data = [
          {
            id: 100,
            deviceName: "人防门监控测试设备",
            icon: "",
            children: children,
          },
        ];
        console.log(this.data);
    },
  },
};
</script>
<style scoped>
/deep/.el-dialog__header{
  background: #409EFF;
}
/deep/.el-dialog__title{
  font-size: 20px;
  color: #fff;
}
/deep/.el-dialog__close{
  color: #fff!important;
}
/deep/.el-dialog__body{
  padding: 0;
  background: #D9e2ff;
}
/deep/.el-dialog__footer{
  background: #D9e2ff!important;
}
/deep/.el-table__body-wrapper::-webkit-scrollbar {
  /*滚动条整体样式*/
  display: none;
}
/deep/.btnbox .el-button {
  width: 200px !important;
}
/deep/.el-input__inner {
  border-radius: 0;
  border: 1px solid #fff;
  border-bottom: 1px solid #c0c4cc;
  outline: none;
}
/deep/.el-input__inner:hover,
/deep/.el-input__inner:focus {
  border: 1px solid #fff;
  border-bottom: 1px solid #c0c4cc;
}
/deep/.el-tree-node__content {
  height: 35px;
  user-select: none;
}
header {
  height: 12vh;
  line-height: 12vh;
  text-align: center;
  font-size: 60px;
  font-weight: 600;
}
.btnbox {
  height: 10vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
section {
  width: 980PX;
  height: 49vh;
  display: flex;
  justify-content: center;
  margin: 0 auto;
}
section aside:first-child {
  width: 300px;
  min-width: 300px;
  margin-right: 15px;
}
.left {
  /* margin: 0 15px; */
  height: 100%;
  overflow: auto;
  background: #fff;
  display: flex;
  flex-direction: column;
}
.tree {
  flex: 1;
  overflow: auto;
  margin-bottom: 15px;
}
.tree::-webkit-scrollbar{
  display: none;
}
section aside:last-child {
  width: 1050px;
  /* min-width: 1050px; */
}
.right {
  height: 100%;
  background: rgb(51, 51, 51);
  display: flex;
  /* flex-wrap: wrap; */
  justify-content: center;
  align-items: center;
}
footer {
  height: 20vh;
  margin-top: 15px;
}
.footer_wrap {
  height: 100%;
  width: 980PX;
  margin: 0 auto;
  background: #fff;
}
</style>