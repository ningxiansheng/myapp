<template>
  <div class="wrapper">
    <ul>
      <li v-for="(item,index) in products" wx:key="index">
        <div class="box">
          <div>
            <input type="checkbox" :checked="item.checked" @click="singleCheck(index)"/>
          </div>
          <div>
            <img :src="item.image" mode="aspectFit" />
          </div>
          <div class="pricebox">
            <div class="tt">
              你的善良必须有点锋芒
              <img src="/static/images/delet.png" class="delet" @click="deleteOne(index)"/>
            </div>
            <div class="price">
              <span class="it1">￥{{item.price}}</span>
              <span class="it2">￥{{item.old_price}}</span>
              <div class="item">
                <div class="sun" @click="reduce(index)">-</div>
                <div class="num">{{item.count}}</div>
                <div class="sun" @click="add(index)">+</div>
              </div>
            </div>
          </div>
        </div>
        <p class="de">
          <span class="ad">加价购</span>
          <span class="detail">购买一件，即可享受换购优惠</span>
          <span class="rt">去选择></span>
        </p>
      </li>
    </ul>
    <div class="footer">
      <div>
        <input type="checkbox" :checked="isCheckAll" @click="checkAll"/>
        <span>全选</span>
        <span class="heji">合计：</span>
        <span>￥{{total}}</span>
      </div>
      <div class="colum">结算({{math}})</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: [],
      total: "",
      isCheckAll:true,
      math:0
    };
  },
  methods: {
    // 点击增加
    add(index) {
      this.products[index].count += 1;
      wx.setStorageSync("cart", this.products);
      this.getTotal();
    },
    // 点击减少
    reduce(index) {
      this.products[index].count -= 1;
      if (this.products[index].count < 1) {
        this.products[index].count = 1;
      }
      wx.setStorageSync("cart", this.products);
      this.getTotal();
    },
    // 每个商品总价
    getTotal() {
      let products = wx.getStorageSync("cart") || [];
      let total = 0;
      let math = 0;
      products.forEach(item => {
        if (item.checked == true) {
          total += item.price * item.count;
          math += item.count
         }
      });
      // 渲染到页面里
      this.total = total;
      this.math = math
    },
    // 单选
    singleCheck(index) {
      // 状态取反
      this.products[index].checked = !this.products[index].checked;
      wx.setStorageSync("cart", this.products);
      // 判断全选
      this.isCheckAll = this.products.every(item=>{
        return item.checked==true;
      })
      // 计算总价
      this.getTotal();
    },
    checkAll(){
      this.isCheckAll=!this.isCheckAll;
      this.products.forEach(item=>{
        item.checked = this.isCheckAll
      })
      wx.setStorageSync("cart",this.products)
      this.getTotal()
    },
    deleteOne(index){
      this.products.splice(index,1);
      wx.setStorageSync("cart",this.products);
      this.getTotal()
    }
  },
  // created() {
  //   let products = wx.getStorageSync("cart");
  //   this.products = products;
  //   // 计算总价
  //   this.getTotal();
  // },
  onShow(){
    let products = wx.getStorageSync("cart");
    this.products = products;
    // 计算总价
    this.getTotal();
  }
};
</script>

<style>
html,
body,
.wrapper {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
}
.wrapper ul {
  flex: 1;
  overflow: auto;
}
.wrapper li {
  padding: 0 10px;
  box-sizing: border-box;
  margin-bottom: 10px;
  border-top: 1px solid #ccc;
}
.wrapper .box img {
  width: 100px;
}
.wrapper .box {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 200px;
}
.wrapper .box .pricebox {
  /* line-height: 80px; */
  width: 200px;
}
.wrapper .box .pricebox .delet {
  width: 20px;
  height: 20px;
  float: right;
}
.wrapper .box .pricebox .tt {
  margin-bottom: 80px;
}
.box .pricebox .price .it1 {
  color: red;
  font-size: 20px;
}
.box .pricebox .price .it2 {
  color: #ccc;
  font-size: 12px;
  text-decoration: line-through;
  margin: 0 10px;
}
.box .pricebox .price {
  display: flex;
}
.box .pricebox .price .item {
  display: flex;
  align-items: center;
}
.box .pricebox .price .sun {
  width: 20px;
  height: 20px;
  background: #f4f4f4;
  border-radius: 50%;
  text-align: center;
  font-size: 20px;
  line-height: 20px;
}
.box .pricebox .price .num {
  margin: 0 10px;
}
.wrapper li .de {
  margin: 0;
}
.wrapper li .de .ad {
  border: 1px solid red;
  margin: 0 5px;
  font-size: 12px;
  color: red;
}
.wrapper li .de .detail {
  color: #ccc;
  font-size: 14px;
}
.wrapper li .de .rt {
  font-size: 14px;
  float: right;
}
.footer {
  display: flex;
  justify-content: space-between;
  height: 50px;
  padding: 0 10px;
  box-sizing: border-box;
  line-height: 50px;
}
.footer .heji {
  margin: 0 20px;
}
.footer .colum {
  width: 100px;
  background: red;
  color: #fff;
  text-align: center;
  line-height: 40px;
  border-radius: 25px;
}
</style>