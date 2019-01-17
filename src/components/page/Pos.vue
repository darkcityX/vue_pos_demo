<template>
    <div class="pos"> 
        <el-row>
            <el-col :span='7' id="order-list" class="pos-order">
                <el-tabs type="card">
                    <el-tab-pane label="点餐">
                        <el-table :data="tableData" border style="width: 100%;">
                            <el-table-column prop="goodsName" label="商品名称"></el-table-column>
                            <el-table-column prop="count" label="数量"></el-table-column>
                            <el-table-column prop="price" label="金额"></el-table-column>
                            <el-table-column label="操作" fixed="right">
                                <template slot-scope="scope">
                                    <el-button type="text" size="small" @click="removeOrderList(scope.row)">删除</el-button>
                                    <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                                </template>
                            </el-table-column>
                        </el-table>
                    </el-tab-pane>
                    <el-tab-pane label="挂单">
                        挂单
                    </el-tab-pane>
                    <el-tab-pane label="外卖">
                        外卖
                    </el-tab-pane>
                </el-tabs>

				<div class="totalDiv">
					<span>数量：</span><span>{{totalNum}}</span>
					<span>金额：</span><span>{{totalSalary}}</span>
				</div>

                <div class="div-btns">
                    <el-button type="warning">挂单</el-button>
                    <el-button type="danger" @click="removeAllOrderList()">删除</el-button>
                    <el-button type="success" @click="checkOut()">结账</el-button>
                </div>
            </el-col> 
            <el-col :span='17'>
                <div class="often-foods">
                    <div class="often-title">常用商品</div>
                    <div class="often-foods-list">
                        <ul>
                            <li v-for="item in oftenGoods" :key="item.goodsId" @click="addOrderList(item)">
                                <span>{{item.goodsName}}</span>
                                <span class="o-price">￥{{item.price}}元</span>
                            </li>
                        </ul>
                    </div>
                </div>

                <div class="foods-type">
                    <el-tabs type="border-card">
                        <el-tab-pane label="汉堡">
                            <div>
                                <ul class="cookList">
                                    <li v-for="item in type0Goods" :key="item.goodsId" @click="addOrderList(item)">
                                        <span class="foodImg">
                                            <img :src="item.goodsImg" width="100%">
                                        </span>
                                        <span class="foodName">{{item.goodsName}}</span>
                                        <span class="foodPrice">￥{{item.price}}元</span>
                                    </li>
                                </ul>
                            </div>
                        </el-tab-pane>
                        <el-tab-pane label="小食">
                            <div>
                                <ul class="cookList">
                                    <li v-for="item in type1Goods" :key="item.goodsId" @click="addOrderList(item)">
                                        <span class="foodImg">
                                            <img :src="item.goodsImg" width="100%">
                                        </span>
                                        <span class="foodName">{{item.goodsName}}</span>
                                        <span class="foodPrice">￥{{item.price}}元</span>
                                    </li>
                                </ul>
                            </div>
                        </el-tab-pane>
                        <el-tab-pane label="饮料">
                            <div>
                                <ul class="cookList">
                                    <li v-for="item in type2Goods" :key="item.goodsId" @click="addOrderList(item)">
                                        <span class="foodImg">
                                            <img :src="item.goodsImg" width="100%">
                                        </span>
                                        <span class="foodName">{{item.goodsName}}</span>
                                        <span class="foodPrice">￥{{item.price}}元</span>
                                    </li>
                                </ul>
                            </div>
                        </el-tab-pane>
                        <el-tab-pane label="套餐">
                            <div>
                                <ul class="cookList">
                                    <li v-for="item in type3Goods" :key="item.goodsId" @click="addOrderList(item)">
                                        <span class="foodImg">
                                            <img :src="item.goodsImg" width="100%">
                                        </span>
                                        <span class="foodName">{{item.goodsName}}</span>
                                        <span class="foodPrice">￥{{item.price}}元</span>
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
    import axios from 'axios';
    export default {
        name : 'pos',
        data () {
            return {
                tableData: [],
                oftenGoods:[],
                type0Goods:[],
                type1Goods:[],
                type2Goods:[],
				type3Goods:[],
				totalNum : 0,
				totalSalary: 0
            }
        },
        created : function(){
            // 服务端拉取常用商品数据
            axios.get("https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods")
                .then(
                    res => {
                        // console.log( res );
						this.oftenGoods = res.data;
						console.log( this.oftenGoods );
                    }
                )
                .catch(
                    err => {
                        alert("网络错误,请稍后再试！");
                    }
                );

            //拉取分类商品数据
             axios.get("https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods")
                .then(
                    res => {
                        // console.log( res );
                        this.type0Goods = res.data[0];
                        this.type1Goods = res.data[1];
                        this.type2Goods = res.data[2];
                        this.type3Goods = res.data[3];
                    }
                )
                .catch(
                    err => {
                        alert("网络错误,请稍后再试！");
                    }
                );
        },
		methods : {
			// 增加订单商品
			addOrderList (goods){

				// 判断是否该商品已经存在于订单列表【tableData】中
				let isHave = false , _i;
				for(let i = 0 ; i < this.tableData.length ; i++){
					if( this.tableData[i].goodsId == goods.goodsId ){
						_i = i;
						isHave = true;
					}
				}

				// 根据判断的值编写业务逻辑
				if( isHave ){ 
					// 如果订单列表【tableData】中当前点击的商品存在，数量++
					this.tableData[_i].count++;

				}else{
					// 否则就将该对象添加进入
					this.tableData.push({
						goodsId : goods.goodsId,
						goodsName : goods.goodsName,
						price : goods.price,
						count : 1
					});

				}

				this.getTotal();

			},

			// 计算总数量和总金额
			getTotal() {
				// 先清零总数量和总金额
				this.totalNum = 0;
				this.totalSalary = 0;

				// 计算总金额和总数量
				this.tableData.forEach((el)=>{
					this.totalNum += el.count;
					this.totalSalary = this.totalSalary+(el.price*el.count);
				});
			},

			// 删除单个商品
			removeOrderList(goods){
				/* 代码冗余
				console.log( goods );
				let newTableData = [];
				this.tableData.forEach((el)=>{
					if(el.goodsId != goods.goodsId){
						newTableData.push( el );
					}
				});
				this.tableData = newTableData;
				*/

				// this.tableData = this.tableData.filter(el=>{
				// 	el.goodsId != goods.goodsId;
				// })
				this.tableData = this.tableData.filter(el=>el.goodsId != goods.goodsId);

				this.getTotal();
			},

			// 删除全部商品
			removeAllOrderList(){
				if( this.totalNum ){
					this.tableData = [];
					this.totalNum = 0;
					this.totalSalary = 0;
					this.$message({
						message : "清空成功，老板请重新下单哦~",
						type : "success",
						duration: 1000
					});
				}else{
					this.$message({
						message:"老板，购物车已清空啦，是时候剁一波手了~",
						type: "error",
						duration: 1000
					});
				}
			},

			// 模拟结账
			checkOut(){
				if( this.totalNum ){
					this.tableData = [],
					this.totalNum = 0,
					this.totalSalary = 0,
					this.$message({
						message: "恭喜老板，贺喜老板，结账成功啦!,欢迎下次光临~",
						type: "success",
						duration: 3000					
					});

				}else{
					this.$message({
						message: "哼！没点吃的就想结帐，想的美~",
						type: "error",
						duration: 2000
					})
				}
			}


		}
    }
</script>

<style scoped>
    .pos-order{
        min-height: 100vh;
        border-right: 1px solid #C0CCDA;
        background-color: #F9FAFC;

    }
	.totalDiv{
		padding: 20px 0;
		border-bottom: 1px solid #C0CCDA;
		background: #fff;
	}
    .div-btns{
        margin-top: 20px;
    }
    .often-title{
        padding: 20px;
        height: 20px;
        text-align: left;
        border-bottom: 1px solid #D3dce6;
        background-color: #F9FAFC;
    }
    .often-foods-list ul{
        margin: 0;
    }
    .often-foods-list ul li{
        margin: 10px;
        padding: 10px;
        list-style-type: none;
        float: left;
        border: 1px solid #E5E9F2;
		cursor: pointer;
        background-color: #fff;
    }
    .o-price{
        color: #58B7FF;
    }
    .foods-type{
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
        min-width: 100px;
        min-height: 60px;
    }
    .foodName{
        font-size: 18px;
        padding-left: 10px;
        color:brown;
    }
    .foodPrice{
        font-size: 16px;
        padding-left: 10px;
        padding-top:10px;
    }
</style>


