<style lang='scss'>
    @import "@/style/px2rem.scss";
    .hjs_input{
        height:px2rem(70);
        padding-left: px2rem(26);
        border-bottom: px2rem(1) solid #e6e6e6;
        width: px2rem(580);
        margin: 0 auto;
        overflow: hidden;
        position:relative;

        .left{
            float: left;
           
            margin-right: px2rem(30);
            margin-top: px2rem(17);
        }
        .phone{
            width: px2rem(25);
            height: px2rem(38);
        }

        .auth{
            width: px2rem(28);
            height: px2rem(36);
        }
        .password{
            width: px2rem(31);
            height: px2rem(34);
        }
        .recommend{
            width: px2rem(32);
            height: px2rem(32);
        }


        input{
            float: left;
            height: px2rem(63);
            width: px2rem(470);
            font-size: px2rem(30);
            vertical-align: baseline;
        }
        /*! autoprefixer: off */ 
        ::-webkit-input-placeholder{
            font-weight: normal;
            font-size: px2rem(30);
            font-stretch: normal;
            letter-spacing: 0px;
            color: #999999;
        }
        .delete{
            width: px2rem(36);
            height: px2rem(32);
            float: right;
            margin-top: px2rem(20);
            margin-right: px2rem(12);
        }

        .closeEye{
            width: px2rem(36);
            height: px2rem(18);
            float: right;
            margin-top: px2rem(34);
            margin-right: px2rem(12);
        }

        .openEye{
            width: px2rem(36);
            height: px2rem(22);
            float: right;
            margin-top: px2rem(26);
            margin-right: px2rem(12);
        }
        .getAuth{
            font-size: px2rem(30);
            color: #6b71e7;
            position: absolute;
            right: px2rem(10);
            top: px2rem(14);
        }
        .disable{
            color: #999;
        }

    }
 
</style>
<template>
 <div class="hjs_input">
    <img v-if="icon=='phone'" src="../../assets/login/phone.png" alt="" class="left phone">
    <img v-if="icon=='password'" src="../../assets/login/password.png" alt="" class="left password">
    <img v-if="icon=='auth'" src="../../assets/login/auth.png" alt="" class="left auth">
    <img v-if="icon=='recommend'" src="../../assets/login/rec.png" alt="" class="left recommend">
    
    
    <input :value="value"  @input="$emit('input', $event.target.value)"  :type="type"  :maxlength="icon=='auth'?6:20" :placeholder="placeholder"  ref="input">
    
    
    <img v-if="value.length>0 && icon=='phone'" 
        src="../../assets/login/delete.png" 
        class="delete"
        @click="$emit('clear')"> 

    <img v-if="icon=='password'" 
        :src="isOpen?require('../../assets/login/open_eye.png'):require('../../assets/login/close_eye.png')" 
        :class="isOpen?'openEye':'closeEye'"
        ref="eye"
        @click="changePwd()"
    > 

    <span @click="sendCode()" v-if="icon=='auth'" :class="['getAuth',timer.isStart?'disable':'']">{{timer.display_content}}</span>


 </div>
</template>

<script>
 export default {
    model:{
        prop:"value",
        event:"input"
    },
    props:{
        type:{
            type:String,
            defatult:'text'
        },
        placeholder:{
            type:String,
            defatult:'手机号'
        },
        icon:{
            type:String,
            default:'text'
        },
        value:{
            type:String,
            default:'text'
        },
        isInitCode:{
            type:Boolean,
            default:true
        }
    },
    data () {
        return {
            input:'',
            isOpen:false,
            timer:{
                isStart:false,
                time:60,
                timer:null,
                display_content:'获取验证码'
            },
        }
    },
    methods:{
        changePwd(){
            this.isOpen =!this.isOpen
            if(this.isOpen){
                this.$refs.input.type ='text'
            }else{
                this.$refs.input.type= 'password'
            }
        },
        _stopTimer(){
             this.timer.time = 60;
            this.timer.isStart =  false;
            clearInterval(this.timer.timer);
            this.timer.timer =  null;
            this.timer.display_content = "请重新获取"
        },
        _sendCode(request){
            
            if(!this.timer.isStart){
                this.timer.isStart = true;
                //请求
                request &&  request()
            }else{
                return  ;
            }
            this.timer.timer = setInterval(()=>{

                this.timer.display_content = (this.timer.time--)+'s重新获取'
                if(this.timer.time == -1){
                    this._stopTimer();
                }
            },1000)

        },
        sendCode(){
            this.$emit("sendCode",this._sendCode)
        }
    },
    created(){
        if(this.isInitCode){
            this._sendCode();
        }
       
    },
 }
</script>


