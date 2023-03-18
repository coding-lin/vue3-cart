<template>
  <div class="goods-container">
    <!-- 左侧图片区域 -->
    <div class="left">
      <!-- 复选框 -->
      <div class="custom-control custom-checkbox">
        <input type="checkbox" class="custom-control-input" :id="id" :checked="checked" @change="onCheckBoxChange" />
        <!-- 将商品图片包裹于 label 中，点击图片可以切换选中状态 -->
        <label class="custom-control-label" :for="id">
          <img :src="thumb" alt="商品图片" class="thumb" />
        </label>
      </div>
    </div>
    <!-- 右侧商品图片 -->
    <div class="right">
      <!-- 商品名称 -->
      <div class="top">{{title}}</div>
      <div class="bottom">
        <!-- 商品价格 -->
        <div class="price">￥{{price.toFixed(2)}}</div>
        <!-- 商品数量 -->
        <div class="count">
          <es-count :num="count" :min="1" @numChange="getNumber"></es-count>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// 导入 count 组件
import EsCount from '../es-count/EsCount.vue';

export default {
  name: 'EsGoods',
  emits: ['stateChange', 'countChange'],
  props: {
    // 唯一的 key 值
    id: {
      type: [String, Number],
      required: true
    },
    // 商品缩略图
    thumb: {
      type: String,
      required: true
    },
    // 商品名称
    title: {
      type: String,
      required: true
    },
    // 单价
    price: {
      type: Number,
      required: true
    },
    // 数量
    count: {
      type: Number,
      required: true
    },
    // 商品的勾选状态
    checked: {
      type: Boolean,
      required: true
    }
  },
  methods: {
    // 监听复选框选中状态变化的事件
    onCheckBoxChange(e) {
      // 向外发送的数据是一个对象，包含了 id 和 value
      this.$emit('stateChange', {
        id: this.id,
        value: e.target.checked
      })
    },
    // 监听数量的变化
    getNumber(num) {
      this.$emit('countChange', {
        id: this.id,
        value: num
      })
    }
  },
  components: {
    // 注册 count 组件
    EsCount
  }
}
</script>

<style lang="less" scoped>
.goods-container {
  +.goods-container {
    border-top: 1px solid #efefef;
  }
  display: flex;
  padding: 10px;
  .left {
    margin-right: 10px;
    .thumb {
      display: block;
      width: 100px;
      height: 100px;
      background-color: #efefef;
    }
  }
  .right {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    flex: 1;
    .top {
      font-weight: bold;
    }
    .bottom {
      display: flex;
      justify-content: space-between;
      align-items: center;
      .price {
        color: red;
        font-weight: bold;
      }
    }
  }
}
.custom-control-label::before,
.custom-control-label::after {
  top: 3.4rem;
}
</style>