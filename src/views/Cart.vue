<template>
  <div>
    <mu-sub-header>List</mu-sub-header>
    <mu-content-block>
      <mu-select-field v-model="product" label="Select product">
        <mu-menu-item v-for="(item,idx) in shelfList" :key="idx" :value="item" :title="item" />
      </mu-select-field>
      <mu-raised-button primary label="Add" @click="handleSelect('product')" />
      <mu-raised-button secondary label="Clear" @click="handleClear('products')" />
    </mu-content-block>
    <mu-content-block>
      <mu-select-field v-model="group" label="Select group">
        <mu-menu-item v-for="(item,idx) in groups" :key="idx" :value="item" :title="item" />
      </mu-select-field>
      <mu-raised-button primary label="Add" @click="handleSelect('group')" />
      <mu-raised-button secondary label="Clear" @click="handleClear('groups')" />
    </mu-content-block>
    <mu-sub-header>Your cart</mu-sub-header>
    <mu-content-block>
      {{selectList}}
    </mu-content-block>
    <mu-content-block>
      {{selectGroup}}
    </mu-content-block>
    <mu-content-block>
      <mu-raised-button secondary label="Get Recommend items" fullWidth @click="handleRecommend()" />
    </mu-content-block>
    <mu-sub-header>Recommend List</mu-sub-header>

    <div v-if="errors.length != 0" style="color: red;">
      <mu-content-block>
        {{ errors.toString() }}
      </mu-content-block>
    </div>
    <div v-else>
      <mu-content-block>
        {{recommendList}}
      </mu-content-block>
    </div>

    <mu-content-block>
      <mu-raised-button primary label="Checkout" fullWidth @click="handleCheckout()" />
    </mu-content-block>
  </div>
</template>
<script>
import { mapState } from "vuex";
import axios from "axios";

export default {
  computed: {
    ...mapState(["user"])
  },
  data() {
    return {
      product: "",
      group: "",
      selectGroup: "",
      shelfList: ["264", "339", "12820"],
      groups: ['M18-24', 'M25-34', 'M35-44', 'M44+', 'F18-24', 'F25-34', 'F35-44', 'F44+'],
      selectList: [],
      recommendList: [],
      errors: []
    };
  },
  methods: {
    handleSelect(type) {
      var val = "";

      switch(type) {
        
        case 'group':
          val = this.group;
          this.selectGroup = val;
          break;

        case 'product':
          val = this.product;
          this.selectList.push(val);
          break;
          
      }
      
    },
    handleClear(type) {

      switch(type) {
        case 'groups':
          this.groups = [];
          break;
        
        case 'products':
          this.selectList = [];
          break;
      }
    },
    handleRecommend() {
      // connect to webservice & get recommend list
      this.recommendList = [];
      var uniqueItems = [];

      uniqueItems = this.selectList.filter((elem, index, self) => {
        return index == self.indexOf(elem);
      });

      axios
        .post(`https://shopapi.nytu.net/api/v1.0/post_recommendation/`, {
          itemId: uniqueItems,
          group: this.selectGroup
        })
        .then(response => {

          // TODO: response.result
          if (response.data.result === "success") {
            this.recommendList = response.data.recommend_itemId;
          } else {
            // handle unsuccessful result
            console.log("====================response.result");
            console.log(response.result);
            this.errors.push("Sorry, Something went wrong. Please try again");
          }
        })
        .catch(e => {
          this.errors.push(e);
        });
    },
    handleCheckout() {
      // go to check out
    }
  }
};
</script>
<style scoped>

</style>
