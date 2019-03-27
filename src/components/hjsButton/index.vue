<style lang='scss'>
@import "@/style/px2rem.scss";
.hjsButton {
  width: px2rem(580);
  height: px2rem(88);
  border-radius: px2rem(44);
  text-align: center;
  background-color: #6b71e7;
  color: #fff;
  line-height: px2rem(88);
  overflow: hidden;
  font-size: px2rem(32);
  letter-spacing: px2rem(6);
  margin: 0 auto;
}
.hjsButton.rect{
  width:px2rem(690);

  border-radius:px2rem(4);
}
.hjsButton.disable{
  background: #e0e0e0;
}
</style>
<template>
  <div class="hjsButton" :class="[disable?'disable':'',type=='rect'?'rect':'']" @click="disable?'':$emit('click')">
    <span>{{text}}</span>
  </div>
</template>

<script>
export default {
  props: {
      text:{
          type:String,
          default:''
      },
      disable:{
          type:Boolean,
          default:false
      },
      type:{
        type:String,
        default:''
      }

  },
  components: {},
  data() {
    return {};
  },
  methods: {
  },
  created() {}
};
</script>

