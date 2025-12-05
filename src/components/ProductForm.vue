<template>
  <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title">Product Form</h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <div class="container">
            <div class="mb-3">
              <label for="name" class="form-label">Name</label>
              <input type="text" class="form-control" id="name" placeholder="name" v-model="productItem['name']">
            </div>
            <div class="mb-3">
              <label for="price" class="form-label">Price</label>
              <input type="number" class="form-control" id="price" placeholder="price" v-model="productItem['price']">
            </div>
            <div class="mb-3">
              <label for="all" class="form-label">Num</label>
              <input type="number" class="form-control" id="all" placeholder="number of product" v-model="productItem['all']">
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
  name: 'ProductForm',
  created() {
    this.get()
  },
  computed:{
    token(){
      return sessionStorage.getItem('token')
    }
  },
  props:{
    id: Number
  },
  data(){
    return {
      productItem : {
        name: "",
        price: 0,
        all:0
      }
    }
  },
  watch : {
    id : function(newId, oldId) {
      if(newId > 0 && newId != oldId){
        this.get();
      }
    }
  },
  methods: {
    async get(){
      if(this.id){
        let res = await fetch(`http://localhost:8000/product/${this.id}`, {
          method: "GET",
          headers: {
            "Content-Type": "application/json",
            "Authorization": `Bearer ${this.token}`
          },
        })
        if(res.status == 401){
          sessionStorage.removeItem("token")
          location.reload()
        } else {
          this.productItem = await res.json()
        }
      }
    },
    async send(){
      if(this.id){
        let res = await fetch(`http://localhost:8000/product/edit/${this.id}`, {
          headers: {
            "Content-Type": "application/json",
            "Authorization": `Bearer ${this.token}`
          },
          method: "PUT",
          body: JSON.stringify(this.productItem)
        })
        if(res.status == 401){
          sessionStorage.removeItem("token")
          location.reload()
        } 
      } else {
        let res = await fetch("http://localhost:8000/product/add", {
          headers: {
            "Content-Type": "application/json",
            "Authorization": `Bearer ${this.token}`
          },
          method: "POST",
          body: JSON.stringify(this.productItem)
        })
        if(res.status == 401){
          sessionStorage.removeItem("token")
          location.reload()
        } 
      }
      this.$emit('update', true);
    }
  },
  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
