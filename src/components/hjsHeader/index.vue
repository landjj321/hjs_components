<style lang='scss'>
//@import url(); 引入公共css类
    .hjs_header{
        box-sizing: content-box;
        // border:  1px solid red;
    }

    .hjs_header.pt30 .van-nav-bar{ padding-top: 30px; }
    .hjs_header.pt20 .van-nav-bar{ padding-top: 20px; }
    .hjs_header.pt0 .van-nav-bar{ padding-top: 0px; }

    .hjs_header.white{
        // border-bottom: 2px solid  #f8f9ff;
        // border-bottom: 1px solid  #e5e5e5;
        color: #333333;
        .van-nav-bar{
            background: #fff;
        }
        .van-nav-bar--fixed{
            border-bottom: 1px solid #e5e5e5;
        }
    }

    .hjs_header.blue{
        color: #fff!important;
        .van-nav-bar__title{
            color: #fff;

        }

        .van-nav-bar{
            background: #695bff;
        }
    }

    .hjs_header.none{
        color: #fff;
        .van-nav-bar{
            background: rgba(0, 0, 0, 0);
        }
    }


</style>
<!-- hjs_header -->
<template>
  <div class="hjs_header" :class="[theme,osType=='ios'?isIphoneX()?'pt30':'pt20':'pt0']" 
       :style="osType=='ios'?isIphoneX()?'padding-top:76px;':'padding-top:66px;':'padding-top:46px;'">
      <van-nav-bar
        :title="hjsTitle"
        @click-left="onClickLeft"
        @click-right="onClickRight"
        :left-text="leftText"
        :right-text="rightText"
        :left-arrow="leftArrow"
        :fixed="fixed"

        >  
        <!--  :style="osType=='ios'? isIphoneX()?'padding-top:30px;':'padding-top:20px;':''" --> 
            <div slot="left">
                <slot name="left">
                    <van-icon size='18px' v-if="leftArrow" name="arrow-left" :color="theme=='white'?'#333333':'#fff'" />
                    <span v-if="leftText" v-text="leftText" />
                </slot>
            </div>
            
            <div slot="title">
                <slot name="title" ><span :style="{'font-size':'17px'}">{{ hjsTitle }}</span></slot>
            </div>


            <div slot="right">
                <slot name="right">
                    <span v-if="rightText" v-text="rightText" />
                </slot>
            </div>
            
           
      </van-nav-bar>
  </div>
</template>

<script>
//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
//例如：import 《组件名称》 from '《组件路径》';
import {isIphoneX,os_type} from  "@/utils/utils.js"
import {NavBar,Icon}  from "vant"
export default {
  //import引入的组件需要注入到对象中才能使用
  name:"hjsHeader",
  components: {
        [NavBar.name]:NavBar,
        [Icon.name]:Icon,
  },
  data() {
    //这里存放数据
    return {
        isIphoneX:isIphoneX,
        os_type,
    };
  },
  props:{
      theme:{
          type:String,
          default:'white'
      },
      hjsTitle:{
          type:String,
          default:''
      },
      hjsColor:{
          type:String,
          default:'#fff'
      },
      leftText:{
          type:String,
          default:''
      },
      rightText:{
          type:String,
          default:''
      },
      leftArrow:{
          type:Boolean,
          default:true
      },
      fixed:{
          type:Boolean,
          default:true
      },
      osType:{
          type:String,
          default:'h5'
      }
  },
  //方法集合
  methods: {
      onClickLeft(){
        this.$emit("click-left")
      },
      onClickRight(){
        this.$emit("click-right")
      }
  },
  
  //监听属性 类似于data概念
  computed: {},
  //监控data中的数据变化
  watch: {},
  //生命周期 - 创建完成（可以访问当前this实例）
  created() {
  },

};
</script>
