<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
  <h1 class="title">Calculadora De Gastos</h1>

  <calc></calc>
</template>

<script>
import calc from './components/interfaceCalculator.vue'
export default {
  name: 'App',
  components: {
    calc
  }
}
</script>

<style>
body {
  background-color: black;
}

#app {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  scale: 1.5;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  font-family:  Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  /* height: vh; */
  background-color: black;
}

.title {
  font-size: 2.5em;
  text-align: center;
  color: #9b9b9b;
  margin-bottom: 20px;
  font-weight: bold;
  text-shadow: 2px 5px 4px rgba(255, 255, 255, 0.5);
  text-transform: uppercase;
  letter-spacing: 2px;
  user-select: none;
}

@media (max-width: 1300px) {
  #app {
    scale: 1;
  }
}
</style>
