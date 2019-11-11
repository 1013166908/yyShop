<template>
  <div>
    <div class="search">
      <div>
        <input type="text"><button>搜索</button>
      </div>
    </div>

    <swiper class="lunbo1" autoplay="true" interval="2000">
      <swiper-item v-for="(item,index) in 4" :key="index">
        <img :src="'http://localhost:3000/upload/type/lunbo'+(index+1)+'.jpg'" alt="">
      </swiper-item>
    </swiper>

    <swiper class="type-list-swiper"  indicator-dots="true">
      <swiper-item v-for="(item,index) in length" :key="index">
        <ul v-if="typeArr.length" class="type-list">
          <li v-for="(item1,index1) in typeArr" v-if="index1>=index*10&&index1<(index+1)*10" :key="index1" @click="toGoods(item1.id,item1.name)">
            <div class="img"><img :src="item1.image" alt=""></div>
            <h3>{{item1.name}}</h3>
          </li>
        </ul>
      </swiper-item>
    </swiper>

    <img src="http://localhost:3000/images/adv.jpg" alt="" style="display:block;width:100%;height:6rem">

    <swiper>
      <swiper-item v-for="(item,index) in goodsLength" :key="index">
        <ul class="goods-list">
          <li v-for="(item_g,index_g) in goodsList" v-if="index_g>=index*4&&index_g<(index+1)*4" :key="index_g" @click="toDetail(item_g.gid)">
            <div>
              <img :src="item_g.image" alt="">
              <span>￥{{item_g.price}}</span>
            </div>
          </li>
        </ul>
      </swiper-item>
    </swiper>

    <!--parseInt(typeArr.length/10)-->

  </div>
</template>

<script>
import card from '@/components/card'

export default {
  data () {
    return {
        typeArr:[],
        goodsList:[],
        length:0,
        goodsLength:0
      }
    },
  methods:{
    toGoods(tid,tname){
      wx.navigateTo({
        url:'/pages/goods-list/main?tid='+tid+'&tname='+tname
      })
    },
    toDetail(gid){
      wx.navigateTo({
        url:'/pages/detail/main?gid='+gid
      })
    }
  },
  mounted(){
    let that=this;
    //获取全部商品分类
    wx.request({
      url:"http://localhost:3001/getTypes",
      data:{},
      success(res){
        that.typeArr=res.data;
        that.length=Math.ceil(res.data.length/10);
      }
    })

    //获取商品列表
    wx.request({
      url:"http://localhost:3001/getGoods",
      data:{},
      success(res){
        //删除状态为下架的商品，避免影响长度
        for(let i=0;i<res.data.length;i++){
            if(!res.data[i].state){
              res.data.splice(i,1);
              i--;
            }
        }

        that.goodsList=res.data;
        that.goodsLength=Math.ceil(res.data.length/4);
      }
    })
  }
}
</script>

<style scoped lang="less">
  .search{
    display: flex;
    justify-content: center;
    align-items: center;
    height:0.6rem;
    background: #ff927b;
    div{
      display: flex;
      width:80%;
    }
    input{
      background: white;
      width:80%;
      height:0.3rem;
      line-height: 0.3rem;
      font-size: 0.24rem;
      border-radius: 0.05rem;
    }
    button{
      width:10%;
      height:0.3rem;
    }
  }
  .lunbo1{
    height:2rem;
    overflow: hidden;

    img{
      width:100%;
    }
  }
  .type-list-swiper{
    height:3.6rem;
  }
  .type-list{
    height:3.6rem;
    padding: 0.1rem 0.2rem 0;
    background: #fffbfb;
    overflow: hidden;
    li{
      float: left;
      width:20%;
      height:1.2rem;
      margin: 0.2rem 0;
      font-size: 0.24rem;
      .img{
        width:0.8rem;
        height:0.8rem;
        margin: 0 auto;
        border-radius: 50%;
        overflow: hidden;
        img{
          width:0.8rem;
          height:0.8rem;
        }
      }
      h3{
        text-align: center;
      }
    }
  }
  .goods-list{
    background: #fffbfb;
    overflow: hidden;
    padding: 0.2rem 0;
    li{
      float: left;
      width:25%;
      div{
        background: white;
        width:90%;
        margin:0 auto;
        padding: 0.2rem 0;
        border:0.05rem solid pink;
        border-radius: 0.1rem;
        text-align: center;
        image{
          width:1.5rem;
          height:1.8rem;
          margin: 0 auto;
        }
        span{
          color:red;
          font-weight: bold;
        }
      }
    }
  }
</style>
