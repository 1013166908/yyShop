<template>
  <div class="wrap">
    <div class="user">
      账号：<input type="text" v-model="usn" required><br>
      密码：<input type="password" v-model="psw" required><br>
    </div>
    <div  v-if="now==0">
      <input type="button" value="登录" @click="login"><span @click="changeState">没有账号，前往注册</span><br>
    </div>
    <div v-else>
      <input type="button" value="注册" @click="register"><span @click="changeState">已有账号，去登录</span><br>
    </div>
    <p class="msg">{{msg}}</p>
    <!--<button open-type="getUserInfo"  @getuserinfo="getuserinfo">一键登录</button>-->
  </div>
</template>

<script>

  export default {
    data(){
      return{
        now:0,
        usn:"",
        psw:"",
        msg:""
      }
    },
    onLoad(){

    },
    methods:{
      login(){
        let that=this;
        wx.request({
          url:"http://localhost:3001/login",
          data:{
            usn:that.usn,
            psw:that.psw
          },
          method:"POST",
          success(res){
//            console.log(res);
            if(res.data=="1000"){
              that.msg="";
              wx.showLoading({
                title:"登录成功",
              })
              wx.setStorageSync('userId',that.usn);
//              console.log(wx.getStorageSync('userId'));
              setTimeout(()=>{
                wx.navigateBack({
                  delta: 1
                })
              },1000)
            }else{
              that.msg="用户名不存在或密码错误";
            }
          }
        })
      },
      register(){
        let that=this;
        wx.request({
          url:"http://localhost:3001/register",
          data:{
            usn:that.usn,
            psw:that.psw
          },
          method:"POST",
          success(res){
//            console.log(res);
            if(res.data=="1000"){
              that.msg="";
              wx.showToast({
                title:"注册成功",
                duration:1000
              })
            }else{
              that.msg="该用户名已被注册";
            }
          }
        })
      },
      changeState(){
          this.now=!this.now;
      }
    }

  }
</script>

<style scoped>
  @import 'index.less';

</style>
