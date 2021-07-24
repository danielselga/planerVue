<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current"/>
    <div v-if="projects.length">
      <div v-for="project in projects" :key="project.id">
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
  }
}
</script>

<style>
  .filter-nav button {
    background: none;
    border: none;
    color: #bbb;
    outline: none;
    font-size: 12px;
    text-transform: uppercase;
    margin-right: 10px;
    letter-spacing: 1px;
    font-weight: bold;
    cursor: pointer;
  }

  h3 {
    cursor: pointer;
  }


</style>
