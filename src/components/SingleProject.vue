<template>
  <div class="project" :class="{complete:project.complete}">
    <div class="flexing">
        <div>
            <h3 @click="showDetail=!showDetail">{{project.title}}</h3>
        </div>
        <div>
            <span class="material-icons" @click="deleteProject">delete</span>
            <span class="material-icons">edit</span>
            <span class="material-icons" @click="completeProject">done</span>
        </div>
    </div>
    <p v-if="showDetail">{{project.detail}}</p>
    {{project.complete}}
  </div>
</template>

<script>
export default {
    props:['project'],
    data(){
        return{
            showDetail:false,
            api:'http://localhost:3000/projects/'
        }
    },
    methods:{
        deleteProject(){
            let deleteRoute=this.api+this.project.id;
            fetch(deleteRoute,{method:'DELETE'})
            .then(()=>{
                this.$emit('delete',this.project.id)
            })
            .catch((err)=>{
                console.log(err);
            })
        },
        completeProject(){
            let updatecompleteRoute=this.api+this.project.id;
            console.log(updatecompleteRoute)
            fetch(updatecompleteRoute,{
                method:"PATCH",
                headers:{
                    "Content-Type":"application/json"
                },
                body:JSON.stringify({
                    complete:!this.project.complete
                })
            })
            .then(()=>{
                this.$emit("complete",this.project.id)
            })
            .catch((err)=>{
                console.log(err);
            })
        }
    }
}
</script>

<style>
.project{
    padding: 20px;
    margin: 10px;
    background-color: #f2f2f2;
    border-left: 6px solid crimson;
    border-radius: 8px;
}
h3{
    color:indigo;
     cursor: pointer;
}
.flexing{
    display: flex;
    justify-content: space-between;
    align-items: center;
}
span{
    margin-left: 10px;
     cursor: pointer;
}
span:hover{
    color:#777;
}
.complete{
    border-left-color: green;
}
</style>