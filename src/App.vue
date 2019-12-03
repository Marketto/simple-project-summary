<template>
  <div id="app">
    <h1>Summary</h1>
    <section v-if="projects.length">
      <h2>Projects</h2>
      <Projects :src="projects"/>
    </section>
    <section v-if="websites.length">
      <h2>Websites</h2>
      <Websites :src="websites"/>
    </section>
  </div>
</template>
<script>
import Projects from './components/Projects.vue';
import Websites from './components/Websites.vue';
import axios from 'axios';

export default {
  name: 'simple-project-summary',
  components: {
    Projects,
    Websites
  },
  data: ()=> ({
    projects: [],
    websites: []
  }),
  mounted() {
    return axios
      .get('assets/list.json')
      .then(({ data: { projects, websites } }) => {
        this.projects = projects;
        this.websites = websites;
      });
  }
}

</script>
<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

  display: flex;
  flex-direction: column;
  justify-content: stretch;
  align-items: stretch;
  padding: 1em 0;
}
h1 {
  color: lightskyblue;
}
section {
  margin: 1em 0;
}
section:not(:first-of-type) {
  border-top: 2px solid lightskyblue;
  padding-top: 1em;
}
section > h2 {
  padding: 0.4em 0;
}

.table.b-table {
  width: 100%;
}
</style>
