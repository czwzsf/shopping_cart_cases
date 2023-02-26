<template>
  <div id="app">
    <!--headerCom-->
    <HeaderCom></HeaderCom>
    <p>
      {{ fullState }}
      {{ calAmount }}
      {{ calSelect }}
    </p>
    <!--goodsCom-->
    <GoodsCom v-for="item in list"
              :key="item.id"
              :id="item.id"
              :title="item.goods_name"
              :pic="item.goods_img"
              :price="item.goods_price"
              :state="item.goods_state"
              :count="item.goods_count"
              @state-change="getNewState"
    ></GoodsCom>
    <FooterCom
        :isfull="fullState"
        :amount="calAmount"
        :all="calSelect"
        @full-change="getFullCheck"
    ></FooterCom>
  </div>
</template>

<script>
import GoodsCom from "@/components/Goods/goodsCom.vue";
import HeaderCom from "@/components/Header/headerCom.vue";
import FooterCom from "@/components/Footer/footerCom.vue";
// import axios
import axios from "axios";

export default {
  name: 'App',
  components: {
    HeaderCom,
    GoodsCom,
    FooterCom,
  },
  computed: {
    // Judge whether to select all
    fullState() {
      return this.list.every(item => item.goods_state)
    },
    // Calculate the price
    calAmount() {
      let amount = 0
      let list_length = this.list.length
      for (let i = 0; i < list_length; i++) {
        if (this.list[i].goods_state === true) {
          amount = amount + this.list[i].goods_price
        }
      }
      return amount
    },
    // Record the selected quantity
    calSelect() {
      let num_select = 0
      let list_length = this.list.length
      for (let i = 0; i < list_length; i++) {
        if (this.list[i].goods_state === true) {
          num_select++
        }
      }
      return num_select
    }
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
    },
    // Accept data passed by sub-components
    getNewState(val) {
      // change value of id and goods_state
      this.list.some(item => {
        if (item.id === val.id) {
          item.goods_state = val.value
        }
      })
    },
    // Accept data passed by footerCom to change check
    getFullCheck(val) {
      console.log(val)
      if (val === true) {
        this.list.forEach((item) => {
          item.goods_state = true
        })
      } else {
        this.list.forEach((item) => {
          item.goods_state = false
        })
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
