<template>
  <div class="wrap">
    <ul class="goods-list">
      <li v-for="(item,index) in goodsList" :key="index" v-if="item.state" @click="toDetail(item.gid)">
        <div>
          <div class="top">
            <div class="img">
              <img :src="item.image" alt="">
            </div>
            <h3>{{item.name}}</h3>
            <p>{{item.describe}}</p>
          </div>
          <div class="bottom">
            <span class="price">￥{{item.price}}</span><span>剩余{{item.total}}件</span>
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
        goodsList:[]
      }
    },
    onLoad(options){
      let that=this;
      let {tid,tname} = options;
      wx.setNavigationBarTitle({
        title: tname
      })

      wx.request({
        url:"http://localhost:3001/getGoods",
        data:{tid},
        success(res){
//          console.log(res);
          that.goodsList=res.data;
        }
      })
    },
    methods:{
      toDetail(gid){
        wx.navigateTo({
          url:'/pages/detail/main?gid='+gid
        })
      }

    }
  }
</script>


<style scoped lang="less">
  .wrap{
    background: #fff6f8;
    height:100%;
    background: #fff6f8;
  }
  .goods-list {
    overflow: hidden;
    margin: 0 auto;
    li {
      float: left;
      box-sizing: border-box;
      width: 45%;
      margin: 0.25rem 0 0 3%;
      padding-top: 0.2rem;
      border: 0.02rem solid #e4e4e4;
      border-radius: 0.1rem;
      background: white;
      font-size: 0.24rem;
      line-height: 0.28rem;
      > div {
        box-sizing: border-box;
        width: 100%;
        .top {
          width: 90%;
          margin: 0 auto;
          .img{
            width:100%;
            text-align: center;
            img {
              width: 2.5rem;
              height: 3rem;
              margin: 0 auto;
            }
          }
          h3 {
            font-size: 0.3rem;
            line-height: 0.34rem;
            height:0.34rem;
            overflow: hidden;
            text-overflow: ellipsis;
          }
          p {
            height: 0.56rem;
            overflow: hidden;
            color: #777;
            text-overflow: ellipsis;
          }
        }
        .bottom {
          display: flex;
          justify-content: space-between;
          background: #e4e4e4;
          padding: 0 0.2rem;
          line-height: 0.5rem;
          .price {
            color: red;
          }
        }
      }
    }
  }
</style>
