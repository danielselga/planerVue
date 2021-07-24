<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current"/>
    <div v-if="projects.length">
      <div v-for="project in filteredProjects" :key="project.id">
        <SingleProject :project="project" @delete="handleDelete" @complete="handleComplete" />
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from '../components/SingleProject.vue'
import FilterNav from '../components/FilterNav.vue'

export default {
  name: 'Home',
  components: {SingleProject, FilterNav},
  data(){
    return {
      projects: [],
      current: 'all'
    }
  },
  mounted(){
    fetch('http://localhost:3000/projects')
    .then(res => res.json())
    .then(data => this.projects = data)
    .catch(err => console.log(err.message))
  },
  methods: {
    handleDelete(id){
      //Esse id é o nome do param recebido pelo evento, pode ser chamado do que for.
      this.projects = this.projects.filter((project)=> {
        return project.id !== id
      })
    },
    handleComplete(id){
      let p = this.projects.find(project => {
        return project.id === id
      }) //O find dispara uma função callback que retorna e armazena na variavel tudo o que for true e ingnora o que for false.
      p.complete = !p.complete
    }
  },
  computed: {
    filteredProjects(){
      if (this.current === 'completed'){
        return this.projects.filter(project => project.complete)
      }
      if (this.current === 'ongoing'){
        return this.projects.filter(project => !project.complete)
      }
      return this.projects
    }
  }
}
</script>

<style>

  h3 {
    cursor: pointer;
  }


</style>
