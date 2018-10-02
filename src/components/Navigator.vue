<template>
  <nav class="navbar navbar-expand-lg navbar-light">
    <a class="navbar-brand" href="#">PyFun</a>
    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbatNav" aria-expanded="false" aria-label="Toggle navgaton">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNav">
      <div class="navbar-nav">
        <router-link to="/" class="nav-item nav-link">Index</router-link>
        <li class="nav-item dropdown" v-for="(value, key) in stages" :key="key">
          <a class="nav-link dropdown-toggle" href="#" id="dropdownAbout" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">Stage {{ key }}</a>
          <div class="dropdown-menu" aria-labelledby="dropdownAbout">
              <router-link v-for="name in value" :key="name" :to="'/stage/' + key + '/' + convertURL(name)" class="dropdown-item">{{ name }}</router-link>
          </div>
        </li>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  name: 'navigator',
  data: function () {
    return {
      stages: {}
    }
  },
  mounted: function () {
    this.$ajax({
      'method': 'GET',
      'url': this.$backend + '/stage'
    }).then(response => {
      this.stages = response.data.data
    }).catch(error => {
      console.log(error)
    })
  },
  methods: {
    convertURL: function (name) {
      return name.replace(/ /g, '_')
    }
  }
}
</script>

<style scoped>
  nav {
    background-color: #fa561e;
  }
</style>
