<template>
  <div class="wrap">
    <div class="addr">
      <p>地址：{{addr}}</p>
      <a href="javascript:;" @click="toAddr">
        <img src="../../../static/images/edit.png" alt="">
      </a>
    </div>
    <ul class="goods-list">
      <li v-for="(item,index) in goodsList" :key="index">
        <img :src="item.image" alt="">
        <div>
          <div class="top">
            <h2>{{item.name}}</h2>
            <p>{{item.describe}}</p>
          </div>
          <div class="bottom">
            <div class="price">￥{{item.price}}</div>
            <div class="num" @click.stop="">
              <span>x{{item.num}}</span>
            </div>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
//  import card from '@/components/card'

  export default {
    data () {
      return {
        goodsList:[],
        usn:"",
        addr:"我的地址"
      }
    },
    onLoad(options){
      this.goodsList=JSON.parse(options.list);
      this.usn=wx.getStorageSync('userId');
      let that=this;
      wx.request({
        url:"http://localhost:3001/getAddress",
        method:'POST',
        data:{usn:this.usn},
        success(res){
//          console.log(res.data.addr);
//          console.log(this.addr);
          that.addr=res.data.addr;
        }
      })
    },


    method:{
        //前往地址选择
      toAddr(){
          wx.navigateTo({
            url:'/pages/address/main'
          })
      }
    }
  }
</script>


<style scoped lang="less">
  @import 'index.less';
</style>
