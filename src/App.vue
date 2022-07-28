<template>
  <div class="app-container">
    <!-- Header -->
    <Header></Header>
    <!-- 循环渲染每个商品的信息 -->
    <Goods
      v-for="item in list"
      :key="item.id"
      :id="item.id"
      :title="item.goods_name"
      :pic="item.goods_img"
      :price="item.goods_price"
      :state="item.goods_state"
      :count="item.goods_count"
      @state-change="getNewState"
    >
      <Counter
        :num="item.goods_count"
        @num-change="getNewNum(item, $event)"
      ></Counter>
    </Goods>

    <!-- Footer -->
    <Footer
      :isfull="fullState"
      :amount="amt"
      :allCount="total"
      @full-change="getFullState"
    ></Footer>
  </div>
</template>

<script>
// 导入axios请求库
import axios from "axios";
// 导入组件
import Header from "@/components/Header/Header.vue";
import Goods from "@/components/Goods/Goods.vue";
import Footer from "@/components/Footer/Footer.vue";
// import bus from "@/components/eventBus.js";
import Counter from "@/components/Counter/Counter.vue";

export default {
  // 注册
  components: {
    Header,
    Goods,
    Footer,
    Counter,
  },
  data() {
    return {
      // 用来存储购物车的列表数据，默认为空数组
      list: [],
    };
  },
  // 计算属性
  computed: {
    // 动态计算出全选状态是true还是false
    fullState() {
      return this.list.every((item) => item.goods_state === true);
    },
    // 已勾选商品的总价格
    amt() {
      // 1. 先filter过滤（选择已勾选的商品）
      // 2. 再reduce（把这些商品的价格加起来）
      return this.list
        .filter((item) => item.goods_state)
        .reduce(
          (total, item) => (total += item.goods_price * item.goods_count),
          0
        );
    },
    // 已勾选商品的总数量
    total() {
      return this.list
        .filter((item) => item.goods_state)
        .reduce((t, item) => (t += item.goods_count), 0);
    },
  },
  created() {
    // 调用请求数据的方法
    this.initCartList();
    // bus.$on("share", (val) => {
    //   this.list.some((item) => {
    //     if (item.id === val.id) {
    //       item.goods_count = val.value;
    //       return true;
    //     }
    //   });
    // });
  },
  methods: {
    // 封装请求列表数据的方法
    async initCartList() {
      // 调用axios的get方法，请求列表数据
      const { data: res } = await axios.get("https://www.escook.cn/api/cart");
      // 只要渲染中用到某个数据，这个数据必须得存到data中去。
      console.log(res);
      if (res.status === 200) {
        this.list = res.list;
      }
    },
    // 接收子组件传递过来的数据
    // val的格式为{id, value}
    getNewState(val) {
      this.list.some((item) => {
        if (item.id === val.id) {
          // 将数据应用到渲染中
          item.goods_state = val.value;
          // 终止后续的循环
          return true;
        }
      });
    },
    // 接收Footer子组件传递过来的全选按钮的状态
    getFullState(val) {
      this.list.forEach((item) => (item.goods_state = val));
    },
    //slot: 获取Counter组件发过来的最新的数量值
    getNewNum(item, e) {
      item.goods_count = e;
    },
  },
};
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
