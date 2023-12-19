<template>
  <div class="home">
    <div v-if="projects.length">
      <div v-for="project in projects" :key="project.id">
        <SingleProjectComponent :project="project" @delete="handleDelete" @complete="handleComplete">
        </SingleProjectComponent>
      </div>
    </div>
  </div>
</template>

<script>
import SingleProjectComponent from "@/components/SingleProjectComponent.vue"
export default {
  name: 'HomeView',
  components: {
    SingleProjectComponent
  },
  data() {
    return{
      projects : []
    }
  },
  mounted(){
    fetch("http://localhost:3000/projects")
    .then(res => res.json())
    .then(data => this.projects = data)
    .catch(error => console.log(error.message))
  },
  methods:{
    handleDelete(id){
        this.projects= this.projects.filter((project) => project.id != id)
    },
    handleComplete(id){
      let p = this.projects.find((project) => project.id === id);
      p.complete = !p.complete;
    }
  }
}
</script>
