<template>
  <div>

    <div class="userinfo" >
      <!-- <div class="nav"> -->
        <img src="/static/images/danglogal.png"/>
        <div class="sousuo">
          <img src="/static/images/sousuo.png" alt="">
          <input type="text" placeholder="Lily超品日，3折封顶">
        </div>
        <img src="/static/images/dangdang.png" alt="">
      <!-- </div> -->
    </div>
    <!-- 轮播 -->
     <swiper 
    autoplay="true"
    indicator-dots="true"
    indicator-color="rgba(0, 0, 0, .3)"
    indicator-active-color="yellow"
    circular="true"
  >
    <swiper-item>
      <image src="/static/images/banner1.jpg"></image>
    </swiper-item>
    <swiper-item>
      <image src="/static/images/banner2.jpg"></image>
    </swiper-item>
    <swiper-item>
      <image src="/static/images/banner3.jpg"></image>
    </swiper-item>
    <swiper-item>
      <image src="/static/images/banner4.jpg"></image>
    </swiper-item>
    <swiper-item>
      <image src="/static/images/banner5.jpg"></image>
    </swiper-item>
  </swiper>

  <!-- 商品 -->
    <div class="shop">
      <ul >
        <li v-for="(item,index) in list" wx:key="index">
          <img :src="item.image" alt="">
        </li>
        
      </ul>
    </div>
  <!-- 广告 -->
  <div class="guanggao">
    <img src="/static/images/guanggao.png" mode="guanggao">
  </div>
  <!-- 书籍 -->
  <div class="bookbox">
    <ul>
      <li v-for="(item,index) in books" wx:key="index" @click="details(item.id)">
        <img :src="item.image" mode="widthFix">
        <p class="text">{{item.desc}}</p>
        <p class="it"><span class="textborder">当当自营</span><span class="pri">满减</span></p>
        <p class="it">￥{{item.price}}</p>
      </li>
      
    </ul>
  </div>
  </div>
</template>

<script>
import card from '@/components/card'

export default {
  data () {
    return {
      // motto: 'Hello miniprograme',
      // userInfo: {
      //   nickName: 'mpvue',
      //   avatarUrl: 'http://mpvue.com/assets/logo.png'
      // },
      list:[],
      books:[]
    }
  },

  components: {
    card
  },
  // 跳转detail页面
  methods: {
    details(id){
      wx.navigateTo({url:`/pages/detail/main?id=${id}`})
    }
  },
  created() {
    
  },
  mounted(){
    // let app = getApp()
    wx.request({
      url:"http://127.0.0.1:8080/type.json",
      success:(res)=>{
        this.list = res.data;
        console.log(res);
      }
    }),
      wx.request({
      url:"http://127.0.0.1:8080/book.json",
      success:(res)=>{
        this.books = res.data;
        console.log(res);
      }
    })
  }
}
</script>

<style scoped>
.userinfo {
  width: 100%;
  height: 50px;
  padding: 0 10px;
  box-sizing: border-box;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.userinfo img{
  width: 40px;
  height: 40px;
}
.userinfo input{
  border: 1px solid #ccc;
  width: 260px;
  display: inline-block;
  height: 30px;
  border-radius: 25px;
  text-indent: 2em;
}
.userinfo .sousuo {
  position: relative;
}
.userinfo .sousuo img{
  width: 20px;
  height: 20px;
  position: absolute;
  top: 6px;
  left: 5px;
}
/* 轮播 */
swiper swiper-item image{
  width: 100%;
  height: 100%;
}
/* 商品 */
.shop ul{
  display: flex;
  flex-wrap: wrap;
}
.shop img{
  width: 70px;
  height: 70px;
}
/* 广告 */
.guanggao{
  width: 100%;
  height: 100px;
  margin: 10px 0;
}
.guanggao img{
  width: 100%;
  height: 100px;
}
/* 书籍 */
.bookbox{
  width: 100%;
  /* padding: 0 10px; */
}
.bookbox ul{
  display: flex;
  flex-wrap: wrap;
  padding: 0 10px;
  box-sizing: border-box;
  /* align-items: center; */
  justify-content: space-between;
}
.bookbox ul li{
  width: 160px;
  margin-bottom: 20px;
}
.bookbox ul li img{
  width: 100%;
}
.bookbox ul li .it{
  color: red;
  padding-top: 5px;
}
.bookbox ul li .it span{
  margin-right: 4px;
}
.bookbox ul li .textborder{
  border: 1px solid red;
}
.bookbox ul li .pri{
  background: red;
  color: #fff;
}
.bookbox .text{
  overflow: hidden;
text-overflow:ellipsis;
white-space: nowrap;
}
</style>
