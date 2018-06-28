<template>
  <div class="pos">
    <el-row>
      <el-col :span="8" class="pos-order" id="order-list">
        <el-tabs>
          <el-tab-pane label="点餐">
            <el-table :data="tableData" border style="width:100%">
              <el-table-column prop="goodsName" label="商品名称"></el-table-column>
              <el-table-column prop="count" label="数量" width="70"></el-table-column>
              <el-table-column prop="price" label="金额" width="70"></el-table-column>
              <el-table-column label="操作" width="100" fixed="right">
                <template slot-scope="scope">
                  <el-button type="text" size="small" @click="delThisData(scope.row)">删除</el-button>
                  <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="totalDiv" >
              <small>数量: </small>{{totalCount}} &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <small>总价: </small>{{totalPrice}}
            </div>
            <div class="div-btn">
              <el-button type="warning">挂单</el-button>
              <el-button type="danger" @click="delAllGoods">删除</el-button>
              <el-button type="success" @click="jiezhang">结账</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label="挂单">挂单</el-tab-pane>
          <el-tab-pane label="外卖">外卖</el-tab-pane>
        </el-tabs>
      </el-col>
      <el-col :span="16">
        <div class="often-goods">
          <div class="title">常用商品</div>
          <div class="often-goodsList">
            <ul>
              <li v-for="(items,index) in oftenGoods" :key="index" @click="addOrderList(items)">
                <span>{{items.goodsName}}</span>
                <span class="foods-price">¥{{items.price}}元</span>
              </li>
            </ul>
          </div>
        </div>

        <div class="goods-type">
          <el-tabs>
            <el-tab-pane label="汉堡">
              <div>
                <ul class="cookList">
                  <li v-for="(goods,index) in type0Goods" @click="addOrderList(goods)" :key="index">
                    <span class="foodImg"><img :src="goods.goodsImg" width="100" alt=""></span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">¥{{goods.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="小食">
              <ul class="cookList">
                  <li v-for="(goods,index) in type1Goods" @click="addOrderList(goods)" :key="index">
                    <span class="foodImg"><img :src="goods.goodsImg" width="100" alt=""></span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">¥{{goods.price}}元</span>
                  </li>
                </ul>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              <ul class="cookList">
                  <li v-for="(goods,index) in type2Goods" @click="addOrderList(goods)" :key="index">
                    <span class="foodImg"><img :src="goods.goodsImg" width="100" alt=""></span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">¥{{goods.price}}元</span>
                  </li>
                </ul>
            </el-tab-pane>
            <el-tab-pane label="套餐">
              <ul class="cookList">
                  <li v-for="(goods,index) in type3Goods" @click="addOrderList(goods)" :key="index">
                    <span class="foodImg"><img :src="goods.goodsImg" width="100" alt=""></span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">¥{{goods.price}}元</span>
                  </li>
                </ul>
            </el-tab-pane>
          </el-tabs>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Pos',
  data(){
    return {
      tableData: [],
      oftenGoods:[],
      type0Goods:[],
      type1Goods:[],
      type2Goods:[],
      type3Goods:[],
      totalCount:0,
      totalPrice:0
    }
  },
  methods:{
    //添加订单列表的方法
    addOrderList(goods){
      this.totalCount=0;
      this.totalPrice=0;
      // console.log(goods)
      //判断这个商品是否存在于订单列表
      let isExisted = false;
      for (let i=0; i<this.tableData.length;i++){
        // console.log(this.tableData[i].goodsId);
        if(this.tableData[i].goodsId==goods.goodsId){
            isExisted=true; //存在
        }
      }
      if(isExisted){
        let arr = this.tableData.filter(o => o.goodsId == goods.goodsId);
        // console.log(arr)
        arr[0].count++;
      }else{
        let newGoodsList = {goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
        this.tableData.push(newGoodsList)
      }
      this.getAllMone();
    },
    // 删除单个商品
    delThisData(goods){
      this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId);
      this.getAllMone();
    },
    // 删除全部商品
    delAllGoods(){
      this.tableData = [];
      this.totalCount=0;
      this.totalPrice=0;
    },
    // 模拟结账效果
    jiezhang(){
      if(this.tableData != 0){
        this.tableData = [];
        this.totalCount=0;
        this.totalPrice=0;
        this.$message({
          message:'恭喜你, 结账成功!',
          type:'success'
        })
      }else{
        this.$message.error({message:"请您添加商品,在结账!"})
      }
      
    },
    //汇总数量和金额
    getAllMone(){
      this.totalCount=0;
      this.totalPrice=0;
      if(this.tableData){
        this.tableData.forEach((element) => {
          // console.log(element)
          this.totalCount += element.count;
          this.totalPrice = this.totalPrice + (element.price * element.count);
        })
      }
    }
  },
  created:function(){
    axios.get("http://jspang.com/DemoApi/oftenGoods.php")
    .then(response => {
      // console.log(this)
      this.oftenGoods = response.data
    })
    .catch(error => {
      alert("网络错误")
    })

    axios.get("http://jspang.com/DemoApi/typeGoods.php")
    .then(response => {
      this.type0Goods = response.data[0]
      this.type1Goods = response.data[1]
      this.type2Goods = response.data[2]
      this.type3Goods = response.data[3]
    })
    .catch(error => {
      alert("网络错误")
    })
  },
  mounted:function(){
    var bodyHeight = document.body.clientHeight;
    // alert(bodyHeight);
    document.getElementById('order-list').style.height = bodyHeight + "px"
  }
}
</script>

<style lang="scss" scoped>
  .pos-order{
    background-color: #f9fafc;
    border-right:1px solid #c0ccda;
  }
  .div-btn{
    margin-top: 10px;
  }
  .often-goods{
    > .title{
      height: 21px;
      border-bottom: 1px solid #d3dce6;
      background-color: #f9fafc;
      padding: 10px;
      text-align: left;
    }
    > .often-goodsList ul li{
      list-style: none;
      float: left;
      border: 1px solid #d3dce6;
      padding: 10px;
      margin: 10px;
      background-color: #fff;
      cursor: pointer;
      > .foods-price{
        color: #20a0ff;
      }
    }
  }
  .goods-type{
    clear: both;
  }
  .cookList li{
       list-style: none;
       width:23%;
       border:1px solid #E5E9F2;
       height: auto;
       overflow: hidden;
       background-color:#fff;
       padding: 2px;
       float:left;
       margin: 2px;
       cursor: pointer;
   }
   .cookList li span{
        display: block;
        float:left;
   }
   .foodImg{
       width: 48%;
    }
   .foodName{
       font-size: 14px;
       padding-left: 10px;
       color:brown;
   }
   .foodPrice{
       font-size: 16px;
       padding-left: 10px;
       padding-top:10px;
   }
   .totalDiv{
     background-color: #fff;
     padding: 10px;
     border-bottom: 1px solid #ccc
   }
</style>
