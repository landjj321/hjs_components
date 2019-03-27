<style lang="scss" scoped>
@import "@/style/px2rem.scss";
.actions_sheet {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  top: 0;
  background-color: rgba(0, 0, 0, 0.7);
  z-index: 2002;

  .wrap {
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    .target {
      padding-right: px2rem(30);
      padding-left: px2rem(30);
      height: px2rem(102);
      width: 100%;
      background: #fff;
      text-align: center;
      line-height: px2rem(102);
      font-size: px2rem(32);
      color: rgba(51, 51, 51, 1);
      position: relative;
      border-bottom: 1px solid rgba(229,229,229,1);
      .close {
        position: absolute;
        left: 0;
        top: 0;
        display: inline-block;
        width: px2rem(102);
        height: px2rem(102);
        background: url("../../assets/components/close2.png") no-repeat center center;
        background-size: px2rem(30) px2rem(30);
      }
    }
  }
}
</style>

<template>
  <div
    class="actions_sheet animated"
    style="animation-duration:0.3s;"
    :class="classname2"
    @click.stop="down()"
  >
    <div class="wrap animated fast" style="animation-duration:0.3s;" :class="classname">
      <div class="box">
        <p class="target">
          <span class="close"></span>
          {{title}}
        </p>
        <slot name="con"></slot>
      </div>
    </div>
  </div>
</template>

<script>
import "animate.css";

export default {
  name: "hjsActionSheet",
  props: {
    title: {
      type: String,
      default: ""
    }
  },
  data() {
    return {
      classname: "hide",
      classname2: "hide"
    };
  },
  components: {},
  created() {},
  methods: {
    up() {
      this.classname = "slideInUp";
      this.classname2 = "fadeIn";
    },
    down() {
      this.classname = "slideOutDown";
      this.classname2 = "fadeOut";
      setTimeout(() => {
        this.classname2 = "hide";
      }, 500);
    }
  }
};
</script>




