<template>
  <el-dialog
    title="异常回放"
    :visible.sync="dialogVisible"
    :width="dialogWidth + 'px'"
    :close-on-click-modal="false"
    :destroy-on-close="true"
    @close="closeDialog"
  >
    <div>
      <video
        :poster="waitImg"
        :style="'width:' + dialogWidth + 'px;height:' + dialogHeight + 'px'"
        muted
        :autoplay="true"
        loop
        src="https://www.runoob.com/try/demo_source/movie.mp4"
      ></video>
    </div>
    <span slot="footer" class="dialog-footer">
      <el-button type="primary" @click="closeDialog">确 定</el-button>
    </span>
  </el-dialog>
</template>
<script>
import waitImg from "@/assets/images/wait.jpg";
export default {
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
      waitImg: waitImg,
    };
  },
  methods: {
    closeDialog() {
        this.$emit("closeDialog")
    },
  },
};
</script>