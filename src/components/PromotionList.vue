<template>
  <div class="container">
    <h1>Promotion list</h1>
    <button type="button" @click="promotionSelectedId=0" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#PromotionModal">
      Create
    </button>
    <table class="table">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Name</th>
          <th scope="col">Product</th>
          <th scope="col">Active</th>
          <th scope="col"></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="promotionItem in promotions" :key="promotionItem.id">
          <th scope="row">{{ promotionItem['id'] }}</th>
          <td>{{ promotionItem['name'] }}</td>
          <td>{{ promotionItem['product']['name'] }}</td>
          <td>{{ promotionItem['active'] }}</td>
          <td>
            <button class="btn btn-primary" @click="promotionSelectedId=promotionItem['id']" data-bs-toggle="modal" data-bs-target="#PromotionModal">Edit</button>
            <button class="btn btn-danger" @click="to_delete(promotionItem.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
    <PromotionForm :id="promotionSelectedId" @update="get()"/>
  </div>
</template>

<script>
import PromotionForm from './PromotionForm.vue'
export default {
  name: 'PromotionList',
  components:{
    PromotionForm
  },
  data() {
    return  {
      promotions : [],
      promotionSelectedId: 0
    }
  },
  created() {
    this.get();
  },
  computed:{
    token(){
      return sessionStorage.getItem('token')
    }
  },
  methods: {
    async get(){
      let res = await fetch("http://localhost:8000/promotion", {
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
        this.promotions = await res.json()
      }
    },
    async to_delete(id){
      let yes = confirm(`do you want to delete ID: ${id}`)
      if(yes){
        let res = await fetch(`http://localhost:8000/promotion/delete/${id}` , {
          method: "DELETE",
          headers: {
            "Content-Type": "application/json",
            "Authorization": `Bearer ${this.token}`
          },
        })
        if(res.status == 401){
          sessionStorage.removeItem("token")
          // location.reload()
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
