<template>
    <div class="left-nav">
        <ul>
            <li>
                <i class="icon iconfont icon-goumaichongzhi"></i>
                <div>收银</div>
            </li>
            <li>
                <i class="icon iconfont icon-jinrudianpu"></i>
                <div>店铺</div>
            </li>
            <li>
                <i class="icon iconfont icon-hanbao"></i>
                <div>商品</div>
            </li>
            <li>
                <i class="icon iconfont icon-huiyuanqia"></i>
                <div>会员</div>
            </li>
            <li>
                <i class="icon iconfont icon-paixing"></i>
                <div>统计</div>
            </li>
            <li>
                <i class="icon iconfont icon-shezhi"></i>
                <div>设置</div>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    name:'leftNav'
}
</script>

<style lang="scss">
    .left-nav{
        color: #fff;
        font-size: 10px;
        height: 100%;
        background-color: #1d8ce0;
        float: left;
        width: 8%;
        .iconfont{
            font-size: 24px;
        }
        > ul{
            padding: 0px;
            margin: 0px;
            li{
                list-style: none;
                text-align: center;
                padding: 10px 20px;
                border-bottom: 1px solid #20a0ff;
            }
        }
    }
    

</style>
