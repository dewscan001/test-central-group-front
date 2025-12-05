<template>
  <div class="modal fade" id="PromotionModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title">Promotion Form</h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <div class="container">
            <div class="mb-3">
              <label for="name" class="form-label">Name</label>
              <input type="text" class="form-control" id="name" placeholder="name" v-model="promotionItem['name']">
            </div>
            <div class="mb-3">
              <label for="price" class="form-label">Description</label>
              <input type="text" class="form-control" id="description" placeholder="description" v-model="promotionItem['description']">
            </div>
            <div class="mb-3">
              <select class="form-select" aria-label="Default select example" v-model="promotionItem['product']">
                <option :selected="!id">Select Product</option>
                <option v-for="product in products" :key="product['id']" :selected="product['id'] == promotionItem['product']['id']" :value="product.id">{{ product.name }}</option>
              </select>
            </div>
            <div class="mb-3">
              <input class="form-check-input" v-model="promotionItem['active']" type="checkbox" value="" id="checkDefault">
              <label class="form-check-label" for="checkDefault">
                Active
              </label>
            </div>
          </div>
        </div>
        <div class="modal-footer">
            <button type="button" class="btn btn-primary" @click="send()" data-bs-dismiss="modal" aria-label="Close">Submit</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PromotionForm',
  props: {
    id : Number
  },
  created() {
    this.get()
  },
  data(){
    return {
      promotionItem : {
        name: "",
        description: "",
        product:0,
        active: false
      },
      products : []
    }
  },
  watch : {
    id : function(newId, oldId) {
      if(newId > 0 && newId != oldId){
        this.get();
      }
    }
  },
  computed:{
    token(){
      return sessionStorage.getItem('token')
    }
  },
  methods: {
    async get(){
      if(this.id){
        let res = await fetch(`http://localhost:8000/promotion/${this.id}`, {
          method: "GET",
          headers: {
            "Content-Type": "application/json",
            "Authorization": `Bearer ${this.token}`
          },
        })
        if(res.status == 401){
          sessionStorage.removeItem("token")
          // location.reload()
        } else {
          this.promotionItem = await res.json()
        }
      }

      let res = await fetch("http://localhost:8000/product", {
        method: "GET",
        headers: {
          "Content-Type": "application/json",
          "Authorization": `Bearer ${this.token}`
        },
      })
      if(res.status == 401){
        sessionStorage.removeItem("token")
        // location.reload()
      } else {
        this.products = await res.json()
      }
    },
    async send(){
      if(this.id){
        let res = await fetch(`http://localhost:8000/promotion/edit/${this.id}`, {
          headers: {
            "Content-Type": "application/json",
            "Authorization": `Bearer ${this.token}`
          },
          method: "PUT",
          body: JSON.stringify(this.promotionItem)
        })
        if(res.status == 401){
          sessionStorage.removeItem("token")
          // location.reload()
        }
      } else {
        let res = await fetch("http://localhost:8000/promotion/add", {
          headers: {
            "Content-Type": "application/json",
            "Authorization": `Bearer ${this.token}`
          },
          method: "POST",
          body: JSON.stringify(this.promotionItem)
        })
        if(res.status == 401){
          sessionStorage.removeItem("token")
          // location.reload()
        }
      }
      this.$emit('update', true);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
