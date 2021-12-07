<template>
  <div class="home">
    <div v-if="projects">
      <div 
      v-for="project in projects" 
      :key="project.id">
      <SingleProject
      :project="project"
      @deleted = "handleDelete"
      @completed = "handleCompleted"
       />
      </div>
    </div>
  </div>
</template>


<script>
import SingleProject from '../components/SingleProject.vue'

export default {
  name: 'Home',
  components: { SingleProject },
  data () {
    return {
      projects: []
    }
  },
  mounted() {
    fetch('http://localhost:3000/projects')
    .then(res => res.json())
    .then(data => this.projects = data)
    .catch(err => console.log(err))
  },
  methods: {
    handleDelete(projectId) {
     this.projects = this.projects.filter(project => project.id !== projectId)
    },
    handleCompleted(projectId) {
      let p = this.projects.find(project => project.id === projectId)
      p.complete = !p.complete
    }
  }
}
</script>
