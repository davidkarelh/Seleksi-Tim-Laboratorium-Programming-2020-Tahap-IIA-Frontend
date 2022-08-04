<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-dark">
    <div class="container-fluid">
      <a class="navbar-brand text-light" href="/">Hi, {{name}}!</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <a class="nav-link active text-light" aria-current="page" href="/">Home</a>
          </li>
        </ul>
      </div>
    </div>
    <p class="navbar-brand text-light float-end" href="/">Home Page</p>
  </nav>

  <div v-if="role == 'ADMIN'">
    <HomeAdmin></HomeAdmin>
  </div>
  <div v-else-if="role == 'USER'">
    <HomeCustomer></HomeCustomer>
  </div>

</template>

<script>
// @ is an alias to /src
import HomeAdmin from '@/components/admin/HomeAdmin.vue'
import HomeCustomer from '@/components/customer/HomeCustomer.vue'

export default {
  name: 'HomeView',
  components: {
    HomeAdmin,
    HomeCustomer,
    HomeCustomer
},
  data() {
    return {
      name: '',
      role: ''
    }
  },
  created() {
    fetch('http://localhost:5000/api/profile', {
      method: "GET",
      headers: {
          'Content-Type': 'application/json',
          'Authorization': 'Bearer ' + (localStorage.getItem('token') != null ? localStorage.getItem('token') : "")
      },
    })
    .then(res => res.json())
    .then(response => {
      if (response.status) {
        this.name = response.data.name;
        this.role = response.data.role;
      } else {
        this.$router.push('/login')
      }
    }).catch((error) => {
      console.error('Error:', error);
    })
  }
}
</script>
