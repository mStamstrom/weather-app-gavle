<template>
  <div id="app">
    <Weather/>
    <div id="preload-01" />
    <div id="preload-02" />
    <div id="preload-03" />
    <div id="preload-04" />
    <div id="preload-05" />
  </div>
</template>

<script>
import Weather from './components/Weather.vue'

export default {
  name: 'app',
  components: {
    Weather
  }
}
</script>

<style>
html, body, #app {
  height: 100%;
  margin: 0;
  padding: 0;
  font-family: sans-serif;
  text-align: center;
  color: white;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
  
  --sun: #5bd3ff;
  --clouds: #b3b3b3;
  --rain: #575757;
  --snow: #7e7e7e;
  --night: rgb(4, 11, 49);
}

#preload-01 { background: url('./assets/sunny.jpg') no-repeat -9999px -9999px; }
#preload-02 { background: url('./assets/rain.jpg') no-repeat -9999px -9999px; }
#preload-03 { background: url('./assets/snow.jpg') no-repeat -9999px -9999px; }
#preload-04 { background: url('./assets/clouds.jpg') no-repeat -9999px -9999px; }
#preload-05 { background: url('./assets/night.jpg') no-repeat -9999px -9999px; }

button {
  border: 0;
  outline: none !important;
  background-color: inherit;
  align-items: center;
  cursor: pointer;
  border: 2px solid white;
  border-radius: 8px;
}

a {
  color: inherit;
  text-decoration: none;
}

</style>
