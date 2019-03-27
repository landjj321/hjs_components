<style lang="scss">
@import "../../style/px2rem.scss";

.dialog {
  position: fixed;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.7);
  color: white;
  top: 0;
  left: 0;
  z-index: 1000;

  .box {
    position: absolute;
    top: px2rem(474);
    left: 0;
    right: 0;
    margin-left: auto;
    margin-right: auto;
    z-index: 1001;
    display: block;
    width: px2rem(540);
    height: px2rem(386);
    background: #fff;
    border-radius: px2rem(10);

    .header {
      margin-top: px2rem(31);
      font-size: px2rem(30);
      position: relative;
      text-align: center;
      font-weight: 400;
      color: rgba(51, 51, 51, 1);

      .close {
        width: px2rem(24);
        height: px2rem(24);
        position: absolute;
        background: url("../../assets/components/close.png") no-repeat center
          center;
        background-size: px2rem(24) px2rem(24);
        right: px2rem(31);
        top: px2rem(10);
      }

      .remind {
        font-size: px2rem(22);
        color: #333333;
        text-align: left;
        margin-top: px2rem(20);
        padding-left: px2rem(31);
      }
    }

    .code {
      display: block;
      width: px2rem(480);
      height: px2rem(90);
      border: solid px2rem(1) #bbbbbb;
      margin: px2rem(28) auto px2rem(6);
      box-sizing: border-box;
      padding: px2rem(20) px2rem(10) px2rem(20) px2rem(40);
      font-size: px2rem(60);
      line-height: px2rem(60);
      letter-spacing: px2rem(34);
      color: #333333;
      font-weight: 600;
    }

    .btn_wrap {
      margin-top: px2rem(15);
      padding-right: px2rem(32);
      height: px2rem(40);
      overflow: hidden;
      span {
        float: right;
        display: inline-block;
        height: px2rem(32);
        font-size: px2rem(22);
        color: #6b71e7;
      }
    }

    .comfirm {
      text-align: center;
      height: px2rem(79);
      font-weight: 600;
      line-height: px2rem(66);
      margin-top: px2rem(12);
      font-size: px2rem(28);
      color: #6b71e7;
      border: px2rem(1) solid rgba(9, 20, 31, 0.13);
    }

    .disable{
      color: #666;
    }
  }
}
</style>
<template>
  <div class="dialog" v-if="isShow">
    <div class="box">
      <div class="header">
        <p class="dialog_title">
          请输入短信验证码
          <span class="close" @click="close"></span>
        </p>
        <p class="remind">接收手机号{{format_mobile(mobile)}}</p>
      </div>

      <input type="text" name="code" v-model="curCode" class="code" maxlength="6">

      <p class="btn_wrap">
        <span @click.stop="getCode()">{{code_txt}}</span>
      </p>

      <div class="comfirm" :class="curCode.length==6?'':'disable'" @click="comfirm()">确定</div>
    </div>
  </div>
</template>

<script>

import  {format_mobile} from  "../../utils/utils.js"
export default {
  name: "dialogcode",
  props: {
    dialogIsShow: {
      type: Boolean,
      default: false
    },
    mobile:{
      type:String,
      default:''
    },
    code:{
      type:String,
      default:''
    }
  },
  data() {
    return {
      format_mobile,
      curCode: this.code,
      isShow: this.dialogIsShow,
      code_txt: "获取验证码",
      timer: {
        isStart: false,
        time: 60,
        timer: null
      }
    };
  },
  methods: {
    close() {
      this.$emit("close")
    },
    comfirm() {
      if(this.curCode.length!=6)return;
      this.$emit("confirm");
    },
    getCode() {
      if (!this.timer.isStart) {
        this.timer.isStart = true;
        this.$emit("getCode")
      } else {
        return;
      }

      this.timer.timer = setInterval(() => {
        this.code_txt = "重新获取(" + this.timer.time-- + "s)";
        if (this.timer.time == 0) {
          this.timer.time = 60;
          this.timer.isStart = false;
          clearInterval(this.timer.timer);
          this.timer.timer = null;
          this.code_txt = "请重新获取";
        }
      }, 1000);
    }
  },
  created() {
  } ,
  destroyed(){
        this.code_txt="获取验证码";
        clearInterval(this.timer.timer)
        this.timer={
            isStart: false,
            time: 60,
            timer: null
        }
  },
  watch: {
    dialogIsShow(val) {
      this.isShow = val;
    },
    curCode(newVal){
      this.$emit("update:code",newVal)
    },
    code(newVal){
      this.curCode =  newVal;
    }
  },
  components: {}
};
</script>


