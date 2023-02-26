<template>
  <div id="app">
    <!--headerCom    -->
    <HeaderCom></HeaderCom>
    <GoodsCom v-for="item in list" :key="item.id" :id="item.id" :title="item.goods_name" :pic="item.goods_img"
              :price="item.goods_price" :state="item.goods_state" :count="item.goods_count"></GoodsCom>
  </div>
</template>

<script>
import GoodsCom from "@/components/Goods/goodsCom.vue";
import HeaderCom from "@/components/Header/headerCom.vue";
// import axios
import axios from "axios";

export default {
  name: 'App',
  components: {
    HeaderCom,
    GoodsCom,

  },
  data() {
    return {
      // Used to store the data returned from the shopping cart interface
      list: []
    }
  },
  methods: {
    /*
    1.This function is used to request data using axios
    2.Initialize data
     */
    async initCartList() {
      // Call Axios get method to request list data
      const {data: res} = await axios.get('https://www.escook.cn/api/cart')
      if (res.status === 200) {
        // store date
        this.list = res.list
      }
    }
  },
  created() {
    // call initCartList()
    this.initCartList()
  }
}
</script>

<style>
#app {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
