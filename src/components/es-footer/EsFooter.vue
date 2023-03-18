<template>
  <div class="footer-container">
    <!-- 全选区域 -->
    <div class="custom-control custom-checkbox">
      <input type="checkbox" class="custom-control-input" id="fullCheck" :checked="isfull" @change="onCheckBoxChange" />
      <label class="custom-control-label" for="fullCheck">全选</label>
    </div>
    <!-- 合计区域 -->
    <div>
      <span>合计：</span>
      <span class="amount">￥{{amount.toFixed(2)}}</span>
    </div>
    <!-- 结算按钮 -->
    <button type="button" class="btn btn-primary btn-settle" :disabled="total === 0">结算({{total}})</button>
  </div>
</template>

<script>
export default {
  name: 'EsFooter',
  emits: ['fullChange'],
  props: {
    // 已勾选商品的总价格
    amount: {
      type: Number,
      default: 0
    },
    // 已勾选商品的总数量
    total: {
      type: Number,
      default: 0
    },
    // 全选按钮的选中状态
    isfull: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    // 监听复选框选中状态的变化
    onCheckBoxChange(e) {
      // 通过 $emit 触发自定义事件，把最新的选中状态传递给当前组件的使用者
      this.$emit('fullChange', e.target.checked);
    }
  }
}
</script>

<style lang="less" scoped>
.footer-container {
  width: 100%;
  height: 50px;
  background-color: #fff;
  border-top: 1px solid #efefef;
  position: fixed;
  bottom: 0;
  left: 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 10px;
} 
.amount {
  color: red;
  font-weight: bold;
}
.btn-settle {
  min-width: 90px;
  height: 38px;
  border-radius: 19px;
}
</style>