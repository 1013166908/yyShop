<template>
  <div :class="wrap">
    <div class="goods-ifo">
      <div class="top">
        <img :src="goodsIfo.image" alt="">
      </div>
      <div class="bottom">
        <div>￥{{goodsIfo.price}}</div>
        <h2>{{goodsIfo.name}}</h2>
        <p>{{goodsIfo.describe}}</p>
      </div>
    </div>
    <div class="buy">
      <div class="car" @click="addToCart">加入购物车</div>
      <div class="buy-now">立即购买</div>
    </div>
    <div class="buy-ifo" :style="'bottom:'+bottom+'rem'">
      <header>
        <div class="img">
          <img :src="goodsIfo.image" alt="">
        </div>
        <div class="right">
          <div>
            <p class="price">￥{{goodsIfo.price}}</p>
            <p>库存{{goodsIfo.total}}件</p>
          </div>
        </div>
      </header>
      <div>
        <span>购买数量</span>
        <div>
          <button @click="reduceNum">-</button>
          <input type="text" :value="num" v-model="num">
          <button @click="addNum">+</button>
        </div>
      </div>
      <button @click="addOk">确定</button>
    </div>
    <div class="mask" v-show="mask" @click="cancelAddToCart">
    </div>
  </div>
</template>

<script>
  //  import card from '@/components/card'

  export default {
    data () {
      return {
        goodsIfo:{},
        mask:false,
        wrap:"wrap",
        bottom:-22,
        num:1
      }
    },
    watch:{
      num(value){
        if(value<1) this.num=1;
        if(value>this.goodsIfo.total)
            this.num=this.goodsIfo.total;
      }
    },
    onLoad(options){
//      console.log(wx.getStorageSync('userId'));
      let that=this;
      let {gid}=options;

      wx.request({
        url:"http://localhost:3001/getGoods",
        data:{gid},
        success(res){
//          console.log(res);
          that.goodsIfo=res.data[0];

          wx.setNavigationBarTitle({
            title: that.goodsIfo.name
          })
        }
      })

      that.bottom=-22;
      that.warp="wrap";
      that.mask=false;
    },
    methods:{
        //加入购物车
      addToCart(){
        this.ifLog();
        this.num=1;
        this.mask=true;
        this.wrap="wrap-mask";
        let timer=setInterval(()=>{
          this.bottom+=1;
          if(this.bottom>=0) clearInterval(timer);
        },5);
      },
      //取消加入购物车
      cancelAddToCart(){
        this.num=0;
        this.mask=false;
        this.wrap="wrap";
        let timer=setInterval(()=>{
          this.bottom-=1;
          if(this.bottom<=-22) clearInterval(timer);
        },5);
      },

      //数量减少
      reduceNum(){
        if(this.num>1)
          this.num--;
        else{
          wx.showToast({
            title:"至少添加一件",
            icon:'none'
          })
        }
//        console.log(this.num);
      },

      //数量增加
      addNum(){
        if(this.num<this.goodsIfo.total)
          this.num++;
        else{
          wx.showToast({
            title:"不得超过库存数",
            icon:'none'
          })
        }
      },

      //添加完成
      addOk(){
        let that=this;
        wx.request({
          url:"http://localhost:3001/addToCart",
          data:{
            usn:wx.getStorageSync('userId'),
            gid:that.goodsIfo.gid,
            num:that.num
          },
          method:'POST',
          success(res){
            console.log(res);
            wx.showToast({
              title:"添加成功！"
            })
            that.num=1; //恢复数量
            that.cancelAddToCart();
          }
        })
      },
      //检测登录
      ifLog(){
//        console.log(wx.getStorageSync('userId'));
        if(!wx.getStorageSync('userId')){ //未登录
          wx.navigateTo({
              url:'/pages/login/main'
            })
        }
//        wx.checkSession({
//          success () {
//            //判断是否授权
//            wx.getSetting({
//              success (res) {
//                if(res.authSetting['scope.userInfo']){
//                  //登录并且授权[在这里执行插入购物车]
//                   console.log("插入购物车");
//                  console.log(wx.getStorageSync('userinfo'));
////                  let nickName =  wx.getStorageSync('userinfo').nickName;
////                  wx.request({
////                    url:"http://127.0.0.1:3001/wxapiAddCart",
////                    data:{
////                      goodsid,
////                      nickName
////                    },
////                    success(res){
////                       console.log(res,123);
////                      if(res.data.ok){
////                        //添加购物车成功
////                        wx.showToast({
////                          title: '添加成功',
////                          icon: 'success',
////                          duration: 2000
////                        })
////
////
////                      }else{
////                        //添加失败
////                        wx.showToast({
////                          title: '添加失败',
////                          icon: 'success',
////                          duration: 2000
////                        })
////                      }
////                    }
////                  })
//
//                }else{
//                  //没有授权 去登录授权
//                  wx.navigateTo({
//                    url:"/pages/login/main"
//                  })
//                }
//              }
//            })
//
//          },
//          fail () {
//            // session_key 已经失效，需要重新执行登录流程
//            //  wx.login() //重新登录
//            //  没有登录 --  去登录
//            wx.navigateTo({
//              url:"/pages/login/main"
//            })
//          }
//        })
//        return false;
      }
    }
  }
</script>


<style scoped lang="less">
  @import 'index.less';
</style>
