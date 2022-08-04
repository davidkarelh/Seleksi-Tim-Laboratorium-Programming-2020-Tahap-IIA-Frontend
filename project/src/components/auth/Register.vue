<template>
    <div class="card bg-dark mb-2">
        <div class="card-body mx-auto text-light">
            Register Form
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

    <div class="card mx-auto mt-5 bg-dark mb-5" style="width: 18rem;">
        <div class="card-body">
            <form @submit.prevent="handleRegister">
                <div id="emailHelp" class="form-text text-warning mb-2">Register if you don't have an account!</div>
                <div class="mb-3">
                    <label for="exampleInputEmail1" class="form-label text-light">Name</label>
                    <input type="text" v-model="name" class="form-control" id="name" aria-describedby="emailHelp" name="name">
                </div>
                <div class="mb-3">
                    <label for="exampleInputEmail1" class="form-label text-light">Username</label>
                    <input type="text" v-model="username" class="form-control" id="username" aria-describedby="emailHelp" name="username">
                </div>
                <div class="mb-3">
                    <label for="exampleInputPassword1" class="form-label text-light">Password</label>
                    <input type="password" v-model="password" class="form-control" id="password" name="password">
                </div>
                <button type="submit" class="btn btn-success" style="width: 6rem;">Register</button>
            </form>
        </div>
    </div>
</template>

<script>
export default {
  name: 'Register',
  data() {
    return {
        name: '',
        username: '',
        password: '',
        errors: []
    }
  },
  methods : {
    redirectHome() {
        this.$router.push('/about'); 
    },
    handleRegister() {
        fetch ('http://localhost:5000/api/auth/register', {
            method: "POST",
            headers: {
                'Content-Type': 'application/json'
            },
            body : JSON.stringify({
                'name': this.name,
                'username': this.username,
                'password': this.password
            })
        })
        .then((response) => response.json())
        .then((data) => {
          console.log('Success:', data);
          if (data.status) {

          } else {
            this.errors = data.errors
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
