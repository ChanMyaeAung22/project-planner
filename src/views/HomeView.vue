<template>
  <div class="home">
    <h1>Home</h1>
    <div v-for="project in projects" :key="project.id">
      <SingleProject :project="project" @delete="deleteProject" @complete="comepleteProject"></SingleProject>
    </div>
  </div>
</template>

<script>


import SingleProject from '../components/SingleProject'
export default {
  name: 'HomeView',
  components: {
    SingleProject,},
  data(){
    return{
      projects:[] //[{…}, {…}, {…}]
    }
  },
  methods:{
    deleteProject(id){
      this.projects=this.projects.filter((project)=>{
        return project.id!=id;
      })
    },
    comepleteProject(id){
      let findProject=this.projects.find((project)=>{
        return project.id == id
      })
      findProject.complete = !findProject.complete
    }
  },
  mounted(){
    fetch('http://localhost:3000/projects')
    .then((response)=>{
      return response.json();
    })
    .then((datas)=>{
      this.projects=datas;
    })
    .catch((err)=>{
      console.log(err.message());
    })
  }
}
</script>
