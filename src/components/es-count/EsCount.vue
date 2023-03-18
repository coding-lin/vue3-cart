<template>
  <div class="counter-container">
    <!-- 数量 -1 按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="onSubClick">-</button>
    <!-- 输入框 -->
    <input type="number" class="form-control form-control-sm ipt-num" v-model.number.lazy="number" />
    <!-- 数量 +1 按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="onAddClick">+</button>
  </div>
</template>

<script>
export default {
  name: 'EsCount',
  emits: ['numChange'],
  props: {
    num: {
      type: Number,
      default: 0
    },
    // 最小值
    min: {
      type: Number,
      // 默认值为 NaN，表示不限制最小值
      default: NaN
    }
  },
  data() {
    return {
      number: this.num
    }
  },
  methods: {
    onSubClick() {
      // 如果 min 的值存在，且 number - 1 之后小于 min
      if (!isNaN(this.min) && this.number - 1 < this.min) return;
      this.number--;
    },
    onAddClick() {
      this.number++;
    }
  },
  watch: {
    number(newVal) {
      // 将输入的值转化为整数
      const paresResult = parseInt(newVal);
      // 如果转换的结果不是数字，或小于1，则强制 number 的值为1
      if (isNaN(paresResult) || paresResult < 1) {
        this.number = 1
        return
      }
      // 如果值为小数，则把转换的结果赋值给 number
      if (String(newVal).indexOf('.') !== -1) {
        this.number = paresResult;
        return;
      }
      this.$emit('numChange', this.number);
    }
  }
}
</script>

<style lang="less" scoped>
.counter-container {
  display: flex;
  .btn {
    width: 25px;
  }
  .ipt-num {
    width: 34px;
    text-align: center;
    margin: 0 4px;
  }
}
</style>