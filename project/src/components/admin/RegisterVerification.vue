<template>
    <nav class="navbar navbar-expand-lg navbar-light bg-dark">
        <div class="container-fluid">
            <a class="navbar-brand text-light" href="/">Hi, user!</a>
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
        <p class="navbar-brand text-light float-end" href="/">Register Verifications</p>
    </nav>

    <div class="container mt-5">
        <div v-for="element in register_verifications">
            <div class="row">         
                <div class="card me-5 mb-5" style="width: 18rem;">
                    <img class="card-img-top" src="./../../assets/Robot.png" onerror="" alt="Card image cap"
                        style="height: 300px;">
                    <div class="card-body bg-primary mb-2">
                        <p class="card-text text-light">ID: {{element.ID}}</p>
                        <p class="card-text text-light">Name: {{element.name}}</p>
                        <p class="card-text text-light">Username: {{element.username}}</p>
                        <p class="card-text text-light">Password: <i class="text-muted">Hidden</i></p>
                        <button class="btn btn-success float-start" style="width: 6rem;">Approve</button>
                        <button class="btn btn-danger float-end" style="width: 6rem;">Reject</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>



<script>
// @ is an alias to /src

export default {
  name: 'RegisterVerification',
  components: {
  },
  data() {
    return {
        register_verifications : []
    }
  },
  methods : {
    registerVerificationHandler() {
        return this.$router.push('/register-verification')
    }
  },
  created() {
    fetch ('http://localhost:5000/api/register-verification', {
            method: "GET",
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + (localStorage.getItem('token') != null ? localStorage.getItem('token') : "")
            }
        })
        .then((res) => res.json())
        .then((response) => {
          console.log('Success:', response);
           if (response.status) {
            this.register_verifications = response.data
          } else {
            this.$router.push('/login')
          }
        })
        .catch((error) => {
          console.error('Error:', error);
        })
  }
}
</script>

<style>
    .admin-menu-button:hover {
        cursor: pointer;
        box-shadow: rgba(0, 0, 0, 0.16) 0px 1px 4px, rgb(51, 51, 51) 0px 0px 0px 3px;
    }
</style>