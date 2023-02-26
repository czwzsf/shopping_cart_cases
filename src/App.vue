<template>
  <div id="app">
    <!--headerCom    -->
    <header-com></header-com>
    <h1>购物车实例</h1>
  </div>
</template>

<script>
import headerCom from "@/components/Header/headerCom.vue";
// import axios
import axios from "axios";

export default {
  name: 'App',
  components: {
    headerCom,
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
      if(res.status === 200){
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
