<template>
<div>
  <h1>Edit Project</h1>
   <form @submit.prevent="handleSubmit">
      <label>Title:</label>
      <input type="text" required v-model="title">
      <label>Details:</label>
      <textarea required v-model="details"></textarea>
      <button>Update Project</button>
  </form>
</div>
</template>

<script>
export default {
  props: ['id'], //esse props serve para fazer requisiçõe referente ao id da url. lembrando precsa ser setado no vue router.
  data(){
    return {
      title: '',
      details: '',
      uri: 'http://localhost:3000/projects/' + this.id
    }
  },
  mounted(){
    fetch(this.uri)
    .then(res => res.json())
    .then(data => {
      this.title = data.title
      this.details = data.details
    })
  },
  methods: {
    handleSubmit(){
      fetch(this.uri,{
      method: 'PATCH',
      headers: {'Content-Type': 'application/json'},
      body: JSON.stringify({title: this.title, details: this.details})
      })
      .then(() => this.$router.push('/'))
      .catch(err => console.log(err))
    }
  }
}
</script>

<style>

</style>