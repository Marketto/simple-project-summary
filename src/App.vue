<template>
  <div id="app">
    <h1>Summary</h1>
    <h2>Projects</h2>
    <Projects :src="projects"/>
    <hr/>
    <h2>Websites</h2>
    <Websites :src="websites"/>
  </div>
</template>
<script>
import Projects from './components/Projects.vue';
import Websites from './components/Websites.vue';

import { projects, websites } from './assets/list.json';

export default {
  name: 'simple-project-summary',
  components: {
    Projects,
    Websites
  },
  data: ()=> ({
    projects,
    websites
  })
}

</script>
<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.table.b-table {
  width: 100%;
}
</style>
