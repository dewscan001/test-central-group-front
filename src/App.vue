<template>
  <LoginForm v-if="!token"/>
  <template v-if="token">
    <nav class="navbar navbar-expand-lg bg-body-tertiary ">
      <div class="container-fluid">
        <ul class="nav nav-tabs">
          <li class="nav-item">
            <a class="nav-link"  :class="is_product? 'active' : ''" @click="is_product=true; is_promotion=false" href="#">Product</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" :class="is_promotion? 'active' : ''" @click="is_product=false; is_promotion=true" href="#">Promotion</a>
          </li>
          <li class="nav-item" style="float: right;">
            <a class="nav-link" @click="logout()" href="#">Logout</a>
          </li>
        </ul>
      </div>
    </nav>
    <ProductList v-if="is_product"/>
    <PromotionList v-if="is_promotion"/>
  </template>
</template>

<script>
import ProductList from './components/ProductList.vue'
import PromotionList from './components/PromotionList.vue'
import LoginForm from './components/Login.vue'
export default {
  name: 'App',
  components: {
    ProductList,
    PromotionList,
    LoginForm
  },
  watch : {
    token : function(newId) {
      if(newId != ""){
        this.get();
      }
    }
  },
  computed:{
    token(){
      return sessionStorage.getItem('token')
    }
  },
  data(){
    return {
      is_product : false,
      is_promotion : false,
    }
  },
  methods: {
    logout(){
      sessionStorage.removeItem('token')
      location.reload()
    }
  }
}
</script>

<style>
</style>
