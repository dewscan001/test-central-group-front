<template>
    <div class="container">
        <div class="mb-3">
            <label for="username" class="form-label">User</label>
            <input type="text" class="form-control" id="username" placeholder="username" v-model="username">
        </div>
        <div class="mb-3">
            <label for="password" class="form-label">Password</label>
            <input type="password" class="form-control" id="password" placeholder="password" v-model="password">
        </div>
        <button class="btn btn-success" @click="login()">Submit</button>
    </div>
</template>

<script>
export default {
    name: 'LoginForm',
    data() {
        return {
            username:"",
            password:""
        }
    },
    methods:{
        async login(){
            let res = await fetch(`http://localhost:8000/api/token/`, {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify({"username": this.username, "password": this.password})
            });
            let result = await res.json()
            sessionStorage.setItem('token', result.access);

            location.reload()
        }
    }
}
</script>

<style scoped>

</style>