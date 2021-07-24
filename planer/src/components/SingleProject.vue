<template>
       <div class="project" :class="{complete: project.complete}">
       <div class="actions">
           <h3 @click="showDetails = !showDetails">{{project.title}}</h3>
           <div class="icon-container">
                <PencilIcon class="icon" />
                <TrashIcon @click="deleteProject" class="icon" />
                <CheckIcon @click="toggleComplete" class="icon" :class="{tick: project.complete}" />
           </div>
       </div>
       <div v-if="showDetails" class="details">
           <p>{{project.details}}</p>
       </div>
   </div>
</template>
 <script>
import { TrashIcon, CheckIcon, PencilIcon } from '@heroicons/vue/outline'

 export default {
     props: ['project'],
     //aqui ele está passando um array de objetos vua props, e estamos fazendo a chamada como se fosse um objeto.
     data(){
         return {
            showDetails: false,
            uri: 'http://localhost:3000/projects/' + this.project.id
        }
     },
     components: {
         TrashIcon,
         CheckIcon,
         PencilIcon
     },
     methods: {
         deleteProject(){
             fetch(this.uri, {method: 'DELETE'})
             .then(() => this.$emit('delete', this.project.id))
         },
         toggleComplete(){
            fetch(this.uri, {
            method: 'PATCH',
            headers: {'Content-Type': 'application/json'},
            body: JSON.stringify({complete: !this.project.complete})
            }) //O METODO PATCH ALTERA UMA PARTE ESPECIFICA DO OBJETO NO BACK END.
            .then(() => {
            this.$emit('complete', this.project.id)
            }).catch((err) => (console.log(err)))
         }
     }
 }
 </script>
 
 <style>
  .project {
    margin: 20px auto;
    background: white;
    padding: 10px 20px;
    border-radius: 4px;
    box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.05);
    border-left: 4px solid #e90074;
  }
     .project.complete {
     border-left: 4px solid #00ce89;
 }

   .project.complete .tick {
     color: #00ce89;
 }

 .actions {
     display: flex;
     justify-content: space-between;
 }

 icon-container{
     display: flex;
     justify-content: space-evenly;
     cursor: pointer;
 }

 .icon {
     height: 1.5rem;
     width: 1.5rem;
     margin-top: auto;
     margin-bottom: auto;
     cursor: pointer;
     margin-top: 1.1rem;
     margin-bottom: auto;
     margin-left: 0.3rem;
 }

 .icon:hover{
     color: gray;
 }
 </style>