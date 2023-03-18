<template>
  <div class="app-container">
    <es-header title="购物车案例"></es-header>
    <es-goods 
      v-for="item in goodslist" 
      :key="item.id" 
      :id="item.id"
      :thumb="item.goods_img"
      :title="item.goods_name"
      :price="item.goods_price"
      :count="item.goods_count"
      :checked="item.goods_state"
    ></es-goods>
    <es-footer @fullChange="onFullStateChange"></es-footer>
  </div>
</template>

<script>
// 导入 header 组件
import EsHeader from './components/es-header/EsHeader.vue';
// 导入 footer 组件
import EsFooter from './components/es-footer/EsFooter.vue';
// 导入 goods 组件
import EsGoods from './components/es-goods/EsGoods.vue';

export default {
  name:'MyApp',
  data() {
    return {
      // 商品列表的数据
      goodslist: [],
    }
  },
  // 组件实例创建完毕之后的生命周期函数
  created() {
    // 调用 methods 中的 getGoodsList 方法，请求商品列表的数据
    this.getGoodsList()
  },
  methods: {
    // 请求商品列表的数据
    async getGoodsList() {
      // 通过组件实例 this 访问到全局挂载的 $http 属性，并发起 Ajax 请求
      const {data: res} = await this.$http.get('/api/cart');
      // 判断是否请求成功
      if (res.status !== 200) return alert('请求商品列表数据失败');
      this.goodslist = res.list;
    },
    // 监听选中状态变化的事件
    onFullStateChange(isfull) {
      console.log(isfull);
    }
  },
  components: {
    // 注册 header 组件
    EsHeader,
    // 注册 footer 组件
    EsFooter,
    // 注册 goods 组件
    EsGoods
  }
}
</script>

<style scoped>
.app-container {
  padding-top: 45px;
}
</style>
