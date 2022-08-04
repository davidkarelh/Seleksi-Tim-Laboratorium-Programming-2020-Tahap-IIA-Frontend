<template>
    <!-- <div>Login</div>
    <button @click="login"> Login? </button> -->
    <div class="card mt-5 bg-dark">
      <div class="card-body mx-auto text-light">
        Welcome to the BNMO (Binomo) Website!
      </div>
    </div>

    <div v-if="errors.length > 0">
        <div v-for="error in errors">
            <div class="alert alert-danger" role="alert">
            <ul>
                <li style="list-style: none;">
                    {{ error }}
                </li>
            </ul>
            </div>
        </div>
    </div>

    <div class="card mx-auto mt-5 bg-dark" style="width: 18rem;">
        <div class="card-body">
            <form @submit.prevent="handleLogin">
                <div id="emailHelp" class="form-text text-warning mb-2">Register if you don't have a customer account!</div>
                <div class="mb-3">
                    <label for="exampleInputEmail1" class="form-label text-light">Username</label>
                    <input type="text" v-model="username" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp" name="username">
                </div>
                <div class="mb-3">
                    <label for="exampleInputPassword1" class="form-label text-light">Password</label>
                    <input type="password" v-model="password" class="form-control" id="exampleInputPassword1" name="password">
                </div>
                <button type="submit" class="btn btn-success float-start" style="width: 6rem;">Log In</button>
                <a href="/register" type="submit" class="btn btn-warning float-end" style="width: 6rem;">Register</a>
            </form>
        </div>
    </div>
</template>

<script>
export default {
  name: 'Login',
  data() {
    return {
      username: '',
      password: '',
      errors: []
    }
  },
  methods : {
    redirectHome() {
        this.$router.push('/about'); 
    },
    handleLogin() {
        fetch ('http://localhost:5000/api/auth/login', {
            method: "POST",
            headers: {
                'Content-Type': 'application/json'
            },
            body : JSON.stringify({
                'username': this.username,
                'password': this.password
            })
        })
        .then((res) => res.json())
        .then((response) => {
          console.log('Success:', response);
           if (response.status) {
            localStorage.setItem('token', response.data.token)
            this.$router.push('/')
          } else {
            this.errors = response.errors
          }
        })
        .catch((error) => {
          console.error('Error:', error);
        })

    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
