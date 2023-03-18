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
      @stateChange="onGoodStateChange"
      @countChange="onGoodCountChange"
    ></es-goods>
    <es-footer :total="total" :amount="amount" :isfull = "isFull" @fullChange="onFullStateChange"></es-footer>
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
      this.goodslist.forEach(x => x.goods_state = isfull)
    },
    onGoodStateChange(e) {
      // 根据 id 查找(e 是一个对象，包含了 id 和 value)
      const findResult = this.goodslist.find(x => x.id === e.id);
      // 对对应商品更新选中状态
      if (findResult) {
        findResult.goods_state = e.value;
      }
    },
    // 监听商品数量变化的事件
    onGoodCountChange(e) {
      const findResult = this.goodslist.find(x => x.id === e.id);
      if (findResult) {
        findResult.goods_count = e.value;
      }
    }
  },
  computed: {
    // 已勾选商品总价
    amount() {
      let totalPrice = 0;
      this.goodslist
        .filter(x => x.goods_state)
        .forEach(x => {
          totalPrice += x.goods_price * x.goods_count
        })
      return totalPrice;
    },
    // 已勾选商品的数量
    total() {
      let num = 0;
      this.goodslist
        .filter(x => x.goods_state)
        .forEach(x => {
          num += x.goods_count
        })
      return num;
    },
    // 是否全选
    isFull() {
      return this.goodslist.every(x => x.goods_state);
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
  padding-bottom: 50px;
}
</style>
