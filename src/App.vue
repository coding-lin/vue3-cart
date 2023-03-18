<template>
  <div class="app-container">
    <h1>App 根组件</h1>
    <hr/>
    <es-header title="购物车案例"></es-header>
  </div>
</template>

<script>
// 导入 header 组件
import EsHeader from './components/es-header/EsHeader.vue';

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
    }
  },
  components: {
    // 注册 header 组件
    EsHeader
  }
}
</script>

<style scoped>
.app-container {
  padding-top: 45px;
}
</style>
