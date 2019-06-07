<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">vue toy</router-link> |
      <router-link to="/about">memory</router-link>
    </div>
    <transition name="router-transition" >
      <router-view/>
    </transition>
  </div>
</template>

<style>
@import "https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.css";

body {
background: #eee;
}

#app {
margin: 0 auto;
max-width: 1000px;
background: #fff;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
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
}

.page {
  position: fixed; 
  right: 0;
  left: 0;
  width: inherit;
  padding: 0 50px;
}

.router-transition-enter-active {
animation: fadeIn 1s;
animation-delay: .5s;
opacity: 0;
}

@keyframes fadeIn {
from {
transform: translateY(-20px);
opacity: 0;
}
to {
transform: translateY(0);
opacity: 1;
}
}

.router-transition-leave-active {
animation: fadeOut 1s;
}

@keyframes fadeOut {
from {
transform: translateY(0);
}
to {
transform: translateY(-20px);
opacity: 0;
}
}

</style>
