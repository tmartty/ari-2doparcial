<template>
  <div id="app">
    <!-- <div id="nav"> -->
      <!-- <router-link to="/">Examen ARI</router-link> -->
      <!-- <router-link to="/about">About</router-link> -->
    <!-- </div> -->
    <div class="aclaracion">
      <div style="text-align:center">
        <b>Administracion de Recursos Informaticos - Nestor Mazza. <br>
        FCE, UBA, Argentina.</b><br>
        Herramienta no oficial
      </div>
      <br>
      Las preguntas correctas suman 1. <br>
      Las incorrectas restan 1. <br>
      No contestadas: no suman ni restan.
    </div>
    <a href="#app" class="arriba"></a>
    <a href="#resultados" class="abajo"></a>
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
.arriba, .abajo {
  position: fixed;
  text-decoration: none;
  border: 1px solid #CACACA;
  padding: 10px;
  color: black;
  box-shadow: 2px 2px 2px #CACACA;
  font-size: 22px;
  border-radius: 100%;
  height: 20px;
  width: 20px;
  opacity: 0.5;
  transition: all 0.2s;
}
.arriba:hover, .abajo:hover {
  opacity: 1;
  transform: scale(1.05);
  box-shadow: 5px 5px 5px #CCC;
}
.arriba {
  top: 5%;
  right: 5%;
}
.arriba:before {
  content: " ";
  width: 0px;
  height: 0px;
  border-top: 10px solid transparent;
  border-right: 10px solid transparent;
  border-bottom: 10px solid #CCC;
  border-left: 10px solid transparent;
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
  top: 5px;
}
.abajo {
  bottom: 5%;
  right: 5%;
}
.abajo:before {
  content: " ";
  width: 0px;
  height: 0px;
  border-top: 10px solid #CCC;
  border-right: 10px solid transparent;
  border-bottom: 10px solid transparent;
  border-left: 10px solid transparent;
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
  bottom: 5px;
}
</style>
