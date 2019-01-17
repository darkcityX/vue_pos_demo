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
                                    <el-button type="text" size="small">删除</el-button>
                                    <el-button type="text" size="small">增加</el-button>
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

                <div class="div-btns">
                    <el-button type="warning">挂单</el-button>
                    <el-button type="danger">删除</el-button>
                    <el-button type="success">结账</el-button>
                </div>
            </el-col> 
            <el-col :span='17'>
                <div class="often-foods">
                    <div class="often-title">常用商品</div>
                    <div class="often-foods-list">
                        <ul>
                            <li v-for="item in oftenGoods" :key="item.goodsId">
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
                                    <li v-for="item in type0Goods" :key="item.goodsId">
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
                                    <li v-for="item in type1Goods" :key="item.goodsId">
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
                                    <li v-for="item in type2Goods" :key="item.goodsId">
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
                                    <li v-for="item in type3Goods" :key="item.goodsId">
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
                tableData: [
                    {
                        goodsName: '可口可乐',
                        price: 8,
                        count:1
                    }, 
                    {
                        goodsName: '香辣鸡腿堡',
                        price: 15,
                        count:1
                    }, 
                    {
                        goodsName: '爱心薯条',
                        price: 8,
                        count:1
                    }, 
                    {
                        goodsName: '甜筒',
                        price: 8,
                        count:1
                    }
                ],
                oftenGoods:[],
                type0Goods:[],
                type1Goods:[],
                type2Goods:[],
                type3Goods:[],
            }
        },
        created : function(){
            // 服务端拉取常用商品数据
            axios.get("https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods")
                .then(
                    res => {
                        // console.log( res );
                        this.oftenGoods = res.data;
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
        }
    }
</script>

<style scoped>
    .pos-order{
        min-height: 100vh;
        border-right: 1px solid #C0CCDA;
        background-color: #F9FAFC;

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
        height: auot;
        overflow: hidden;
        background-color:#fff;
        padding: 2px;
        float:left;
        margin: 2px;
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


