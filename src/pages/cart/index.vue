<template>
  <div class="wrap">
    <div class="user">
      <span>欢迎您，{{usn}} </span> <a href="javascript:;" @click="toLogin">退出</a>
    </div>
    <ul class="goods-list">
      <li v-for="(item,index) in goodsList" :key="index">
        <input type="checkbox">
        <img :src="item.image" alt="">
        <div>
          <h2>{{item.name}}</h2>
          <p>{{item.describe}}</p>
          <div>
            <div class="price">￥{{item.price}}</div>
            <div class="num">
              <div>-</div>
              <span>{{item.num}}</span>
              <div>+</div>
            </div>
          </div>
        </div>

      </li>
    </ul>
  </div>
</template>

<script>
  export default {
    data () {
      return {
          usn:"",
          goodsList:[]
      }
    },
    onShow(options){
      this.ifLog();
      let usn=wx.getStorageSync('userId');
      let that=this;
      that.usn=usn;
      wx.request({
        url:"http://localhost:3001/getCart",
        data:{usn},
        success(res){
          console.log(res);
          that.goodsList=res.data;
          wx.setNavigationBarTitle({
            title: "购物车"
          })
        }
      })


    },
    methods:{
        //加入购物车
      //检测登录
      ifLog(){
        console.log(wx.getStorageSync('userId'));
        if(!wx.getStorageSync('userId')){ //未登录
          wx.navigateTo({
              url:'/pages/login/main'
            })
        }
      },

      toLogin(){
        wx.setStorageSync('userId',"");
        wx.navigateTo({
          url:'/pages/login/main'
        })
      }
    }
  }
</script>


<style scoped lang="less">
  @import 'index.less';
</style>
