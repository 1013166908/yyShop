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
          <li v-for="(item1,index1) in typeArr" v-if="index1>=index*10&&index1<(index+1)*10" :key="index1">
            <div class="img"><img :src="item1.image" alt=""></div>
            <h3>{{item1.name}}</h3>
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
        length:0
      }
    },
  methods:{
      changeImageUrl(str){
//          let newStr=new String(str);

        let newStr = str.replace('u','v');
        console.log('http://localhost:3000/'+newStr);
        return 'http://localhost:3000/'+newStr;
      }
  },
  mounted(){
    let that=this;
    wx.request({
      url:"http://localhost:3001/getTypes",
      data:{},
      success(res){
        that.typeArr=res.data;
        that.length=Math.ceil(res.data.length/10);
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
    background: orange;
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
    padding: 0 0.2rem;
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
</style>
