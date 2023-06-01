<template>
  <div>
    <h1>RESULTS</h1>
    <button @click="launchCall">LAUNCH</button>
    <button @click="fetchCall">FETCH to DB</button>
    <button @click="companyLeads">GET out of DB</button>
    <ol>
      <li>
        <div class="text">

        </div>
      </li>
    </ol>
  </div>
</template>

<script>
export default {
  name: 'search-page',
  props: {
    msg: String
  },
  mounted(){
    fetch(process.env.VUE_APP_ROOT_API + 'company/leads', {
            method: "GET",
            headers: {
              'search_id': sessionStorage.getItem("search_id")
            }
          })
            .then(response => response.json())
            .then(response => console.log(response))
  },
  data() {
    return {
      link: sessionStorage.getItem('link'),
      user_id: sessionStorage.getItem('user_id')
    }
  },
  methods: {
    launchCall() {
      fetch(process.env.VUE_APP_ROOT_API + 'phantom/updateAndLaunch', {
        method: "POST",
        headers: {
          'address': this.link
        }
      })
        .then(response => console.log(response))
    },
    fetchCall() {
      fetch(process.env.VUE_APP_ROOT_API + 'phantom/fetcher', {
        method: "POST",
        headers: {
          'search_id': sessionStorage.getItem("search_id"),
          'type': "companysearch"
        }
      })
        .then(response => console.log(response))
    },
    companyLeads() {
          fetch(process.env.VUE_APP_ROOT_API + 'company/leads', {
            method: "GET",
            headers: {
              'search_id': sessionStorage.getItem("search_id")
            }
          })
            .then(response => response.json())
            .then(response => console.log(response))
    }
  },
  beforeRouteEnter(to, from, next) {
    const isLoggedIn = sessionStorage.getItem('LoggedInStatus')
    if (isLoggedIn) {
      next()
    } else {
      next('/login')
    }
  }
};
</script>
