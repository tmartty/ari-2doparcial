<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Examen ARI</router-link>
      <!-- <router-link to="/about">About</router-link> -->
    </div>
    <div class="aclaracion">
      Las preguntas correctas suman 1. <br>
      Las incorrectas restan 1. <br>
      No contestadas: no suman ni restan.
    </div>
    <router-view/>
  </div>
</template>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding: 0px 20%;
}
#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
  text-decoration: none;
}
.aclaracion {
  text-align: left;
}
</style>
