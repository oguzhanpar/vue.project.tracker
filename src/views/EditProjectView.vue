<template>
  <h3>Edit Project</h3>
  <form @submit.prevent="handleEditSubmit">
    <label>Title:</label>
    <input type="text" v-model="title" required />
    <label>Details:</label>
    <textarea v-model="details" required></textarea>
    <button>Add Project</button>
  </form>
</template>

<script>
export default {
  props: ["id"],
  data() {
    return {
      title: "",
      details: "",
      uri: "http://localhost:3000/projects/" + this.id,
    };
  },
  mounted() {
    fetch(this.uri, {
      method: "GET",
    })
      .then((res) => res.json())
      .then((data) => {
        console.log(data)
        this.title = data.title;
        this.details = data.details;
        })
      .catch((err) => err.message);
  },
  methods: {
    handleEditSubmit() {
      let project = {
        title: this.title,
        details: this.details,
      };

      fetch(this.uri, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(project),
      })
        .then(() => this.$router.push("/"))
        .catch((err) => err.message);
    },
  },
};
</script>

<style>
form {
  background: #fff;
  padding: 20px;
  border-radius: 10px;
}
label {
  display: block;
  color: #bbb;
  font-size: 14px;
  font-weight: bold;
  letter-spacing: 1px;
  margin: 20px 0 10px 0;
}
input {
  padding: 10px;
  border: 0;
  border-bottom: 2px solid #ddd;
  width: 100%;
  box-sizing: border-box;
}
textarea {
  border: 2px solid #ddd;
  padding: 10px;
  width: 100%;
  box-sizing: border-box;
  height: 100px;
}
form button {
  display: block;
  margin: 20px 0auto 0;
  background: #76dd78;
  color: #fff;
  padding: 10px;
  border: 0;
  border-radius: 7px;
  font-size: 16px;
  font-weight: bold;
}
</style>