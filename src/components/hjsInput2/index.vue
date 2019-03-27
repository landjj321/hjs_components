<style lang='scss'>
    @import '@/style/px2rem.scss';
    .hjsInput2{
        width: 100%;
        height: px2rem(90);
        background: #fff;
        box-sizing: border-box;
        display: flex;
        flex-direction: row;
        overflow: hidden;
        &.top_border{
            border-top: 1px  solid rgba(240,240,240,1);
        }
        &.bottom_border{
            border-bottom: 1px  solid rgba(240,240,240,1);
        }
        .label{
            position: relative;
            display: inline-block;
            width:px2rem(205);
            height: px2rem(90);
            line-height: px2rem(90);
            font-size:px2rem(30);
            font-weight:400;
            color:rgba(51,51,51,1);
            margin-left: px2rem(30);
            vertical-align: middle;

            .line2{
                width: px2rem(300);
                position: absolute;
                height:px2rem(22);
                font-size:px2rem(22);
                color:rgba(105,91,255,1);
                span{
                    float: left;
                    height: px2rem(22);
                    line-height: px2rem(22);
                    margin-right: px2rem(8);
                }
            }
        }
        .val{
            height: px2rem(90);
            line-height: px2rem(90);
            width:px2rem(400);
            padding: 0;
            border: none;
            vertical-align: middle;
            font-size:px2rem(30);
        }
        .val::placeholder{
            vertical-align: middle;
            width: px2rem(470);
            font-size:px2rem(30);
            line-height: px2rem(90);
            font-family:PingFang-SC-Regular;
            font-weight:400;
            color:rgba(153,153,153,1);

        }
        .close{
            display: inline-block;
            width:px2rem(85);
            height: px2rem(90);
            background: url("../../assets/components/searchcha.png") no-repeat  right center;
            background-size: px2rem(30) px2rem(30);
        }

        .choose{
            display: inline-block;
            width:px2rem(85);
            height: px2rem(90);
            background: url("../../assets/components/arrow.png") no-repeat  right center;
            background-size: px2rem(16) px2rem(30);
        }
        


        
    }
</style>
<template>
 <div class="hjsInput2" 
    :style="(cardType.length>0 && bankType.length>0)?{'height':'1.87rem'}:''" 
    :class="border=='top'?'top_border':border=='bottom'?'bottom_border':''">
     <div class="label">
         <span>{{label}}</span>
         <p class="line2" v-if="cardType.length>0 && bankType.length>0"><span>{{bankType}}</span><span>储蓄卡</span></p>
    </div>
     <input class="val" 
            type="text" 
            :placeholder="placeholder" 
            v-model="curVal" 
            :disabled="disable?'disabled':false" 
     >
     <b class="close" @click="clear()" v-if="curVal.length>0 &&  clearable && !disable"></b>
     <b class="choose" @click="choose()" v-if="type=='address'"></b>


    <van-popup v-model="show" position="bottom" :overlay="true">
      
        <van-picker 
            ref="picker"
            :columns="areaList" 
            @change="onChange" 
            :show-toolbar="true"
            @confirm="onConfirm"
            @cancel="onCancel"
            />
    </van-popup>
 </div>
</template>

<script>
import '@/style/px2rem.scss';
import { Popup,Area,Picker } from 'vant';

// import areaList  from  "../../utils/area.js"

 export default {
   components: {
       // eslint-disable-next-line vue/no-unused-components
       [Popup.name]:Popup,
       // eslint-disable-next-line vue/no-unused-components
       [Area.name]:Area,
       [Picker.name]:Picker,
   },
   props:{
        label:{ type:String, default:'' },
        placeholder:{ type:String, default:'' },
        val:{ type:String, default:'' },
        clearable:{ type:Boolean, default:true },
        border:{ type:String, default:'' },
        type:{type:String,default:''},
        cardType:{type:String,default:''},
        bankType:{type:String,default:''},
        provinceCode:{type:String,default:''},
        cityCode:{type:String,default:''},
        disable:{type:Boolean,default:false},
        areaList:{type:Array,default:null},
   },
   data () {
     return {
         curVal:this.val,
         show:false,
     }
   },
   methods:{
        clear(){
            this.curVal = '';
        },
        choose(){
            this.show = true;  
        },
        onConfirm(val){
            // eslint-disable-next-line no-console
            this.curVal =  val[0].name + "  " +val[1].name
            this.show=false;
            this.$emit("onConfirm",val)
        },
        onCancel(){
            this.show=false;
        },
        onChange(picker,values){
            this.$emit("onChange",values)
        },
   },
   created(){ 
    
      
   },
   updated(){
       
        if(!this.areaList || !this.provinceCode || !this.cityCode) return ;
        let province ;
        let city ;

        this.areaList[0].values.forEach(item=>{
            if(this.provinceCode ==  item.code){
                province =  item.name;
            }
        })  
        
        this.areaList[1].values.forEach(item=>{
            if(this.cityCode ==  item.code){
                city =  item.name;
            }
        })  
        this.curVal = (province?province:'') + "  " +(city?city:'')
   },
   watch:{
        curVal(newVal){
            this.$emit("update:val",newVal)
        },
        val(newVal){
            this.curVal = newVal
        },

   },

   
 }
</script>

