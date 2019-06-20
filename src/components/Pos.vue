 <template>
  <div class="pos">
    <el-row>
      <el-col :span='7' class="pos-order" id="order-list">
        <el-tabs v-model="activeName"  type="border-card">
          <el-tab-pane label="点餐" name="first">

            <el-table :data="tableData" style="width: 100%" border>
              <el-table-column prop="goodsName" label="商品"></el-table-column>
              <el-table-column prop="count" label="数量" width="70"></el-table-column>
              <el-table-column prop="price" label="金额" width="70"></el-table-column>
              <el-table-column label="操作" width="100" fixed="right">
                <template slot-scope="scope">
                  <el-button type="text" size="mini" @click="delSingleGoods(scope.row)">删除</el-button>
                  <el-button type="text" size="mini" @click="addOrderList(scope.row)">增加</el-button>
                </template>
              </el-table-column>
            </el-table>

            <div class="totalDiv">
              <span><small>数量: </small>{{tCount}}</span>
              <span><small>金额: </small>{{tMoney}}元</span>
            </div>

            <div class="div-btn">
              <el-button type="warning">挂单</el-button>
              <el-button type="danger" @click="delAllGoods">删除</el-button>
              <el-button type="success" @click="checkout">结账</el-button>
            </div>

          </el-tab-pane>
          <el-tab-pane label="挂单" name="second">挂单</el-tab-pane>
          <el-tab-pane label="外卖" name="third">外卖</el-tab-pane>
        </el-tabs>
      </el-col>
      <el-col :span='17'>

        <div class="often-goods">
          <div class="og-title">常用商品</div>
          <div class="og-list">
            <ul>
              <li v-for="og in oftenGoods" @click="addOrderList(og)">
                <span>{{og.goodsName}}</span>
                <span id="o-price">${{og.price}}元</span>
              </li>
            </ul>
          </div>
        </div>

        <div class="good-type">
          <el-tabs   type="border-card">
            <el-tab-pane label="汉堡">
              <div>
                <ul class='cookList'>
                  <li v-for="t0g in type0Goods" @click="addOrderList(t0g)">
                    <span class="foodImg">
                      <!--<img :src="t0g.goodsImg" width="100%">-->
                      <img src="http://how2j.cn/example.gif" width="100%">
                    </span>
                    <span class="foodName">{{t0g.goodsName}}</span>
                    <span class="foodPrice">￥{{t0g.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="小食">
              <div>
                <ul class='cookList'>
                  <li v-for="t0g in type1Goods" @click="addOrderList(t0g)">
                    <span class="foodImg">
                      <!--<img :src="t0g.goodsImg" width="100%">-->
                      <img src="http://how2j.cn/example.gif" width="100%">
                    </span>
                    <span class="foodName">{{t0g.goodsName}}</span>
                    <span class="foodPrice">￥{{t0g.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              <div>
                <ul class='cookList'>
                  <li v-for="t0g in type2Goods" @click="addOrderList(t0g)">
                    <span class="foodImg">
                      <!--<img :src="t0g.goodsImg" width="100%">-->
                      <img src="http://how2j.cn/example.gif" width="100%">
                    </span>
                    <span class="foodName">{{t0g.goodsName}}</span>
                    <span class="foodPrice">￥{{t0g.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="套餐">
              <div>
                <ul class='cookList'>
                  <li v-for="t0g in type3Goods" @click="addOrderList(t0g)">
                    <span class="foodImg">
                      <!--<img :src="t0g.goodsImg" width="100%">-->
                      <img src="http://how2j.cn/example.gif" width="100%">
                    </span>
                    <span class="foodName">{{t0g.goodsName}}</span>
                    <span class="foodPrice">￥{{t0g.price}}元</span>
                  </li>
                </ul>
              </div>
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
    name: "Pos",
    data() {
      return {
        activeName: 'first',
        tableData: [],
        oftenGoods:[],
        type0Goods:[],
        type1Goods:[],
        type2Goods:[],
        type3Goods:[],
        totalMoney:0,
        totalCount:0,
      }
    },
    //获取数据
    created(){
      //读取常用商品列表
      axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods')
        .then(response=>{
          // console.log(response);
          this.oftenGoods=response.data;
        })
        .catch(error=>{
          console.log(error);
          alert('网络错误，不能访问');
        })

      //读取分类商品列表
      axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods')
        .then(response=>{
          // console.log(response);
          //this.oftenGoods=response.data;
          this.type0Goods=response.data[0];
          this.type1Goods=response.data[1];
          this.type2Goods=response.data[2];
          this.type3Goods=response.data[3];
        })
        .catch(error=>{
          console.log(error);
          alert('网络错误，不能访问');
        })

    },
    //编译,挂载,方法结束后,js调整元素高度
    mounted: function () {
      var orderHeiht = document.getElementById('main').clientHeight;
      // console.log(orderHeiht);
      document.getElementById('order-list').style.height = orderHeiht + 'px'
    },
    computed: {
      tMoney:function () {
        this.totalMoney = 0;
        this.tableData.forEach(el=>{
          this.totalMoney= this.totalMoney+(el.count*el.price);
        })
        return this.totalMoney;
      },
      tCount:function () {
        this.totalCount = 0;
        this.tableData.forEach(el=>{
          this.totalCount+=el.count;
        })
        return this.totalCount;
      }
    },
    methods: {
      addOrderList(goods){
        // this.totalMoney = 0;
        // this.totalCount = 0;

        //订单数组是否存在商品数组,没有的话,加添加到订单列表,有就添加数量
        let isHave = false;
        for (let i = 0; i < this.tableData.length; i++) {
          if (this.tableData[i].goodsId == goods.goodsId) {
            isHave = true;
            //存在一样的,添加,增加数量
          }
        }
        //根据判断的值,进行业务
        if (isHave) {
          //改变列商品数量
          let arr = this.tableData.filter(o=>o.goodsId == goods.goodsId);
          //找对应的订单数组的对象,找到就一个,然后就数量加1
          arr[0].count++;
        }else {
          //没有那个数据,加添加一个新的数据,而且数量,一开始就是1,并且添加到订单数组里面
          let newGoods = {
            goodsId:goods.goodsId,
            goodsName:goods.goodsName,
            price:goods.price,
            count:1
          }
          this.tableData.push(newGoods);
        }
        //计算汇总金额和数量
        // this.tableData.forEach(el=>{
        //   this.totalCount+=el.count;
        //   this.totalMoney= this.totalMoney+(el.count*el.price);
        // })
      },
      //删除单个商品
      delSingleGoods(goods){
        console.log(goods);
        this.tableData=this.tableData.filter(o => o.goodsId !=goods.goodsId);
      },
      //删除所有商品
      delAllGoods() {
        this.tableData = [];
        this.totalCount = 0;
        this.totalMoney = 0;
      },
      //模拟结账
      checkout() {
        if (this.totalCount!=0) {
          this.tableData = [];
          this.totalCount = 0;
          this.totalMoney = 0;
          this.$message({
            message: '结账成功，感谢你又为店里出了一份力!',
            type: 'success'
          });

        }else{
          this.$message.error('不能空结。老板了解你急切的心情！');
        }

      }
    }
  }
</script>

 <!--

第一个元素flost
第二个元素clear: both;

ul list-style: none 取消点
li list-style-type : none 取消点,保留位置

-->

<style scoped>
  .pos-order {
    background-color: #f9fafc;
    border-right: 1px solid #c0ccda;
  }
  .div-btn{
    margin-top: 10px;
  }
  .often-goods{

  }
  .og-title{
    height: 20px;
    border-bottom: 1px solid #d3dce6;
    background-color: #f9fafc;
    text-align: left;
    padding: 10px;
  }
  .og-list{

  }
  .og-list ul li{
    list-style: none;
    float: left;
    border: 1px solid #e5e9f2;
    padding: 10px;
    margin: 10px;
    background-color: #fff;
    cursor: pointer;
  }
  #o-price{
    color: #58b7ff;
  }
  .good-type{
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
    width: 40%;
  }
  .foodName{
    font-size: 18px;
    padding-left: 20px;
    color:brown;
  }
  .foodPrice{
    font-size: 16px;
    padding-left: 20px;
    padding-top:20px;
  }

  .totalDiv{
    background: #ffffff;
    padding: 10px;
    border-bottom: 1px solid #d3dce6;
  }
  .totalDiv span{
    margin: 10px;
  }



</style>
