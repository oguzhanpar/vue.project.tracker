<template>
  <div class="project" :class="{ complete: project.complete}">
    <div class="actions">
        <h3 @click="showDetails = !showDetails">{{project.title}}</h3>
        <div class="icons">
            <router-link :to="{ name: 'editproject', params: { id: project.id } }">
                <span class="material-symbols-outlined">edit</span>
            </router-link>
            <span @click="deleteProject" class="material-symbols-outlined">delete</span>
            <span @click="toggleComplete" class="material-symbols-outlined">done</span>
        </div>
    </div>
    <div v-if="showDetails" class="details">
            <h5>Detail:</h5>
            <p>{{project.details}}</p>
        </div>
  </div>
</template>

<script>
export default {
    props: ["project"],
    data(){
        return {
            showDetails: false,
            uri: "http://localhost:3000/projects/" + this.project.id
        }
    },
    methods: {
        deleteProject() {
            fetch(this.uri, { method: "DELETE" })
            .then(() => this.$emit("delete", this.project.id))
        },
        toggleComplete(){
            fetch(this.uri, { 
                method: "PATCH",
                headers: {"Content-Type": "application/json"},
                body:JSON.stringify({ complete: !this.project.complete })
                })
            .then(() => this.$emit("complete", this.project.id))
            .catch((err) => err.message)
        }
    }
}
</script>

<style>
.project{
    margin: 20px auto;
    background: #fff;
    padding: 10px 20px;
    border-radius: 5px;
    box-shadow: 1px 2px 3px rgba(0,0,0,0.05);
    border-left: 4px solid #e90074;
    cursor:pointer;
    }
.actions{
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.material-symbols-outlined{
    font-size: 24px;
    margin-left: 10px;
    color: #bbb;
    cursor: pointer;
}
.material-symbols-outlined:hover{
    color: #777;
}
.project.complete{
    border-left: 4px solid #76dd78;
}
</style>