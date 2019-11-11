<template>
  <div>
    账号：<input type="text" v-model="usn" required><br>
    密码：<input type="password" v-model="psw" required><br>
    <input type="button" value="登录" @click="login"><span>没有账号，前往注册</span><br>
    <input type="button" value="注册"><span>已有账号，去登录</span><br>
    <p>{{msg}}</p>
    <!--<button open-type="getUserInfo"  @getuserinfo="getuserinfo">一键登录</button>-->
  </div>
</template>

<script>

  export default {
    data(){
      return{
        usn:"",
        psw:"",
        msg:""
      }
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
            console.log(res);
            if(res.data=="1000"){
              that.msg="";
              wx.showLoading({
                title:"登录成功",
              })
              wx.setStorageSync('userId',that.usn);
              console.log(wx.getStorageSync('userId'));
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
      }
//      getuserinfo(e){
//        // console.log(e)
//        if(e.mp.detail.userInfo){
//          //授权
//          wx.login({
//            success (res) {
//              if (res.code) {
//                //登录成功
//                wx.showLoading({
//                  title: '登录成功',
//                })
//                wx.setStorageSync('userinfo',e.mp.detail.userInfo)
//
//                //  调回商品详情页
//                wx.navigateBack({
//                  delta: 1
//                })
//
//              }else{
//                // 登录失败（等于没有授权）
//              }
//            }
//          })
//        }else{
//          //没授权
//
//        }
//      }
    }

  }
</script>

<style scoped>


</style>
