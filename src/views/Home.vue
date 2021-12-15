<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current" />
    <div v-if="projects">
      <div 
      v-for="project in filteredProjects" 
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
import FilterNav from '../components/FilterNav.vue'

export default {
  name: 'Home',
  components: { SingleProject, FilterNav },
  data () {
    return {
      projects: [],
      current: "all"
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
  },
  computed: {
    filteredProjects() {
      if (this.current === "completed") {
        return this.projects.filter(project => project.complete)
      }
      if (this.current === "ongoing") {
        return this.projects.filter(project => !project.complete)
      }
      return this.projects
    }
  }
}
</script>
