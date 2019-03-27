<style lang='scss'>

@import '@/style/px2rem.scss';
.hjsBankList{
    padding-left:px2rem(30);
    padding-right: px2rem(30);
    background: #fff;
    height: px2rem(58);
    padding-bottom: px2rem(23);
    border-bottom: 1px solid  rgba(240,240,240,1);
    img{
        width:px2rem(58);
        height: px2rem(58);
        float: left;
    }

    .bankname{
        font-size:px2rem(28);
        font-weight:400;
        color:rgba(51,51,51,1);
        float: left;
        height: px2rem(58);
        line-height: px2rem(58);
        margin-left: px2rem(30);
        
    }

    .more{
        float: right;
        height: px2rem(58);
        width: px2rem(58);
        background: url("../../assets/components/arrow.png") no-repeat center right;
        background-size: px2rem(13) px2rem(24);
    }

    .bank_list{
        background: #fff;

        li{
            height: px2rem(90);
            border-bottom: 1px solid rgba(229,229,229,1);
            line-height: px2rem(90);
            padding:px2rem(0) px2rem(30);
            filter:saturate(0.8);
            img{
                float: left;
                height: px2rem(40);
                width: px2rem(40);
                margin-top: px2rem(25);

            }
            span{
                float: left;
                font-size:px2rem(32);
                color:#333;
                line-height:px2rem(90);
                height: px2rem(90);
                margin-left: px2rem(34);
            }

            b{
                display: inline-block;
                height: px2rem(32);
                width: px2rem(32);
                float: right;
                background: url("../../assets/components/card_on.png") no-repeat center center;
                background-size: px2rem(32);
                margin-top: px2rem(29);
            }


        }
        li.disable{
            filter:saturate(0.8);
            span{
                color:#999;
            }
        }
        li.add{
            img{
                margin-top: px2rem(26);
            }
        }
    }
}
</style>
<template>
 <div class="hjsBankList" @click="showList()">
     <img :src="cur_usableBank&&cur_usableBank.pic"  alt="" @error="error_img($event)" >
     <span class="bankname" v-if="cur_usableBank && cur_usableBank.bankName && cur_usableBank.lastNo">{{cur_usableBank.bankName}} 尾号{{cur_usableBank.lastNo}}</span>
     <span class="bankname" v-else>请选择银行卡</span>
     <span class="more"></span>
    
    <hjs-action-sheet ref="sheet" title="选择银行卡">
        <div slot="con">
            <ul class="bank_list">
                <li v-for="(item,index) in  bankList" :class="item.support?'':'disable'" :key="index" 
                    @click.stop="change_card(index)">
                    <img :src="item.pic" alt="" @error="error_img($event)">
                    <span>{{item.bankName+'('+ item.lastNo+')'}}{{item.support?'&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;支持':'&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;不支持'}}</span>
                    <b v-if="index ==idx"></b>
                </li>
                <li class="add" @click="$router.push({path:'/common/bind_card',query:{...$route.query}})">
                    <img src="../../assets/components/add.png" alt="" @error="error_img($event)">
                    <span>添加银行卡</span>
                </li>
            </ul>
        </div>

    </hjs-action-sheet>

 </div>
</template>

<script>
import hjsActionSheet from "../hjsActionSheet"

 export default {
   components: {hjsActionSheet},
   props:{
    //    bankImg:{
    //        type:String,
    //        default:''
    //    },
    //    bankName:{
    //        type:String,
    //        default:'',
    //    },
    //    bankNo:{
    //         type:String,
    //         default:'',
    //    },
       bankList:{
           type:Array,
           // eslint-disable-next-line vue/require-valid-default-prop
           default:[],
       },
       usableBank:{
           type:Object,
       }
   },
   data () {
     return {
        idx:'-1',
        cur_usableBank:this.usableBank,
     }
   },
   methods:{
    error_img(obj){
      obj.target.src=require("../../assets/components/bank.png");
    },
    showList(){
        this.$refs.sheet.up();
    },
    change_card(index){
        if(!this.bankList[index].support){
            return ;
        }
        this.idx = index;
        this.cur_usableBank =  this.bankList[index]
        this.$refs.sheet.down();
        
    }
   },
   created(){
       this.cur_usableBank = this.usableBank
   },
   watch:{
       usableBank:{
           handler(newVal){
               this.idx
               this.cur_usableBank = newVal;
           },
           deep:true
       },
       cur_usableBank:{
           deep:true,
           handler(newVal){
               this.$emit("update:usableBank",newVal)
           }
       }
   }
 }
</script>

