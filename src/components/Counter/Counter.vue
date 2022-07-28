<template>
  <div
    class="number-container d-flex justify-content-center align-items-center"
  >
    <!-- 减 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="sub">-</button>
    <!-- 购买的数量 -->
    <span class="number-box">{{ num }}</span>
    <!-- 加 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="add">+</button>
  </div>
</template>

<script>
// import bus from "@/components/eventBus.js";

export default {
  props: {
    // 接收商品的id值，将来使用EventBus方案，把数量传递到App.vue的时候，告知App组件更新哪个商品的数量
    // id: {
    //   type: Number,
    //   required: true,
    // },
    // slot方法：要展示的商品的数量
    num: {
      type: Number,
      default: 1,
    },
  },
  methods: {
    add() {
      // 要发送给App的数据格式为{id, value}
      // 其中，id是商品的id， vulue是商品最新的购买数量{id, this.num + 1}
      // 这里使用的是num + 1，而不是num = num + 1，因此没有修改num原值，是安全的。
      // const obj = { id: this.id, value: this.num + 1 };
      // 通过EventBus把obj对象发给App.vue组件
      // bus.$emit("share", obj);
      this.$emit("num-change", this.num + 1);
    },
    sub() {
      if (this.num - 1 === 0) return;
      // const obj = { id: this.id, value: this.num - 1 };
      // 通过EventBus把obj对象发给App.vue组件
      // bus.$emit("share", obj);
      this.$emit("num-change", this.num - 1);
    },
  },
};
</script>

<style lang="less" scoped>
.number-box {
  min-width: 30px;
  text-align: center;
  margin: 0 5px;
  font-size: 12px;
}

.btn-sm {
  width: 30px;
}
</style>
