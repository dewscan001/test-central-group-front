<template>
  <div class="container">
    <h1>Product list</h1>
    <button type="button" @click="productSelectedId=0" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#exampleModal">
      Create
    </button>
    <table class="table">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Name</th>
          <th scope="col">Price</th>
          <th scope="col">ALL</th>
          <th scope="col"></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="productItem in products" :key="productItem.id">
          <th scope="row">{{ productItem.id }}</th>
          <td>{{ productItem.name }}</td>
          <td>{{ productItem.price }}</td>
          <td>{{ productItem.all }}</td>
          <td>
            <button class="btn btn-primary" @click="productSelectedId=productItem.id" data-bs-toggle="modal" data-bs-target="#exampleModal">Edit</button>
            <button class="btn btn-danger" @click="to_delete(productItem.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
    <ProductForm :id="productSelectedId" @update="get()"/>
  </div>
</template>

<script>
import ProductForm from './ProductForm.vue'
export default {
  name: 'ProductList',
  components:{
    ProductForm
  },
  computed:{
    token(){
      return sessionStorage.getItem('token')
    }
  },
  data() {
    return  {
      products : [],
      productSelectedId: 0
    }
  },
  created() {
    this.get();
  },
  methods: {
    async get(){
      let res = await fetch("http://localhost:8000/product", {
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
        this.products = await res.json()
      }
    },
    async to_delete(id){
      let yes = confirm(`do you want to delete ID: ${id}`)
      if(yes){
        let res = await fetch(`http://localhost:8000/product/delete/${id}` , {
          method: "DELETE",
          headers: {
            "Content-Type": "application/json",
            "Authorization": `Bearer ${this.token}`
          },
        })
        if(res.status == 401){
          sessionStorage.removeItem("token")
          location.reload()
        } else {
          this.get()
        }
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
