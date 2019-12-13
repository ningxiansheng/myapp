<template>
  <div class="wrapper">
    <div class="main">
      <div class="imgbox">
        <img :src="list.image" />
      </div>
      <div class="detail">
        <p class="title">
          <span class="ziying">当当自营</span>
          <span>{{list.name}}</span>
        </p>
        <p class="conent">{{list.desc}}</p>
        <p class="pricebox">
          <span class="pr">￥{{list.price}}</span>
          <span class="oldpr">(9.48折)</span>
          <span class="primg">
            <img src="/static/images/price.png" />
            降价通知
          </span>
        </p>
        <p class="dingjia">
          定价：
          <span>￥{{list.old_price}}</span>
        </p>
      </div>
    </div>
    <div class="footer">
      <div>
        <img src="/static/images/dianpu.png" />
        <p>店铺</p>
      </div>
      <div @click="cart">
        <img src="/static/images/cart.png" />
        <p>购物车</p>
      </div>
      <div class="cartbox">
        <div class="item1 box">立即购买</div>
        <div class="item2 box" @click="addCart(list.id)">加入购物车</div>
      </div>
    </div>
  </div>
</template>)

<script>
export default {
  data() {
    return {
      books: [],
      list: ""
      // detail:{}
    };
  },
  methods: {
    addCart(id) {
      // console.log(id)
      // 读取缓存的信息
      let products = wx.getStorageSync("cart") || [];
      console.log(products);
      // 根据id判断在购物车（缓存）中是否存在
      let index = products.findIndex(item => {
        return item.id == id;
      });
      console.log(index);
      if (index != -1) {
        // 存在，对当前商品的count进行累加
        products[index].count += 1;
      } else {
        // 不存在，把商品信息push到数组里面
        this.list.count = 1;
        this.list.checked = true;
        products.push(this.list);
      }
      // console.log(products)
      // 存入缓存
      wx.setStorageSync("cart", products);

      wx.showToast({
        title: "加入购物车成功",
        icon: "success",
        duration: 1000,
        success: res => {
          wx.setStorageSync("cart", products);
        }
      });
    },
    cart() {
      wx.switchTab({
        url: "/pages/cart/main"
      });
    }
  },
  created() {
    wx.request({
      url: "http://127.0.0.1:8080/book.json",
      success: res => {
        this.books = res.data;
        // console.log(res);
      }
    });
  },
  onLoad: function(e) {
    let id = e.id;
    let book = this.books.find(item => {
      return item.id == id;
    });
    // console.log(book,"-------------")
    this.list = book;
    // console.log(book, "11111111111");
    // console.log(this.list, "22222222222");
  }
};
</script>

<style >
html,
body,
.wrapper {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
}
.wrapper .main {
  flex: 1;
}
.imgbox {
  border-bottom: 1px solid #ccc;
  height: 280px;
}
.detail {
  padding: 10px;
}
.detail .title .ziying {
  background: red;
  color: #fff;
  border-radius: 25px;
  font-size: 12px;
  width: 64px;
  display: inline-block;
  text-align: center;
}
.detail .conent {
  color: #666;
  line-height: 30px;
  /* overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap; */
}
.detail .pricebox {
  margin: 20px 0;
}
.detail .pricebox .pr {
  color: red;
  font-size: 20px;
}
.detail .pricebox .oldpr {
  margin: 0 5px;
  color: #666;
}
.detail .pricebox .primg {
  display: inline-block;
  border: 1px solid #666;
  border-radius: 25px;
  font-size: 12px;
  width: 80px;
}
.detail .pricebox .primg img {
  width: 20px;
  height: 20px;
}
.detail .dingjia {
  color: #666;
}
.detail .dingjia span {
  text-decoration: line-through;
}
/* 底部 */
.footer {
  display: flex;
  justify-content: space-between;
  height: 50px;
  padding-left: 10px;
  box-sizing: border-box;
}
.footer img {
  width: 30px;
  height: 30px;
}
.footer div {
  text-align: center;
  align-items: center;
}
.cartbox {
  display: flex;
}
.cartbox .box {
  width: 100px;
}
.footer .item1 {
  background: orange;
  line-height: 50px;
  color: #fff;
}
.footer .item2 {
  background: red;
  line-height: 50px;
  color: #fff;
}
.footer p {
  font-size: 12px;
}
</style>