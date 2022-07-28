<template>
  <div class="goods-container">
    <!-- 左侧图片 -->
    <div class="thumb">
      <div class="custom-control custom-checkbox">
        <!-- 复选框 -->
        <input
          type="checkbox"
          class="custom-control-input"
          :id="'cb' + id"
          :checked="state"
          @change="stateChange"
        />
        <label class="custom-control-label" :for="'cb' + id">
          <!-- 商品的缩略图 -->
          <img :src="pic" alt="" />
        </label>
      </div>
    </div>
    <!-- 右侧信息区域 -->
    <div class="goods-info">
      <!-- 商品标题 -->
      <h6 class="goods-title">{{ title }}</h6>
      <div class="goods-info-bottom">
        <!-- 商品价格 -->
        <span class="goods-price">￥{{ price }}</span>
        <!-- 商品的数量 -->
        <!-- <Counter :num="count" :id="id"></Counter> -->
        <slot></slot>
      </div>
    </div>
  </div>
</template>

<script>
// import Counter from "@/components/Counter/Counter.vue";
export default {
  // components: {
  //   Counter,
  // },
  props: {
    id: {
      // 商品id
      // 为什么要在这里封装一个id属性呢？
      // 原因：将来，子组件中商品的勾选状态变化之后，需要子向父的形式，通知父组件根据id修改对应商品的状态。
      required: true,
      type: Number,
    },
    // 要渲染的商品标题
    title: {
      default: "",
      type: String,
    },
    // 要渲染的商品的图片
    pic: {
      default: "",
      type: String,
    },
    price: {
      default: 0,
      type: Number,
    },
    // 商品的勾选状态：默认选中
    state: {
      default: true,
      type: Boolean,
    },
    count: {
      type: Number,
      default: 1,
    },
  },
  methods: {
    // 只要复选框的选中状态发生了变化，就会调用这个处理函数
    stateChange(e) {
      const newState = e.target.checked;
      // 触发自定义事件
      // 在methods中使用props的value，只需要用this.指向value即可
      this.$emit("state-change", { id: this.id, value: newState });
    },
  },
};
</script>

<style lang="less" scoped>
.goods-container {
  + .goods-container {
    border-top: 1px solid #efefef;
  }
  padding: 10px;
  display: flex;
  .thumb {
    display: flex;
    align-items: center;
    img {
      width: 100px;
      height: 100px;
      margin: 0 10px;
    }
  }

  .goods-info {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    flex: 1;
    .goods-title {
      font-weight: bold;
      font-size: 12px;
    }
    .goods-info-bottom {
      display: flex;
      justify-content: space-between;
      .goods-price {
        font-weight: bold;
        color: red;
        font-size: 13px;
      }
    }
  }
}
</style>
