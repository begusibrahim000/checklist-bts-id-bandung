<template>
  <div id="app">
    
    <div class="container">
      <div class="row">
        <div class="col">

          <!-- <router-view></router-view> -->

          <div class="jumbotron">
            <h1 class="text-center">Checklist - <a href="http://bts.id" target="_blank">bts.id</a></h1>
          </div>

          <form @submit.prevent="save">
            <input type="hidden" v-model="form.id"> <br>
            <div class="input-group">
              <input type="text" v-model="form.name" class="form-control" placeholder="Name">
              <div class="input-group-append">
                <button class="btn btn-outline-success" type="submit">Post</button>
              </div>
            </div>
          </form>

          <table class="table table-striped">
            <thead>
              <tr>
                <th scope="col">No</th>
                <th scope="col">Name</th>
                <th scope="col">Action</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item,index) in checklist" :key="item.id">
                <td>{{index + 1}}</td>
                <td>{{item.name}}</td>
                <td>
                  <button type="submit" v-on:click="hapus(item)" class="btn btn-danger">Delete</button>
                </td>
              </tr>
            </tbody>
          </table>

        </div>
      </div>

      <p class="text-center">&copy; 2020 - Begus Ibrahim </p>
    </div>

  </div>
</template>

<script>
  import axios from 'axios'

  export default {
    data() {
      return {
        checklist: '',
        form: {
          id: '',
          name: ''
        },
        authorization: {
                        headers: {
                          'Authorization': `Bearer eyJhbGciOiJIUzUxMiJ9.eyJyb2xlcyI6W119.i2OVQdxr08dmIqwP7cWOJk5Ye4fySFUqofl-w6FKbm4EwXTStfm0u-sGhDvDVUqNG8Cc7STtUJlawVAP057Jlg`
                        }
                      }
      }
    },
    mounted() {
      this.getChecklist()
    },
    methods: {
      getChecklist() {
        axios.get('http://18.141.178.15:8080/checklist', this.authorization)
        .then(response => {
          console.log(response.data.data[0].name)
          this.checklist = response.data.data
        })
        .catch(error => {
          alert(`Error ${error}`)
          console.log(error)
        })
      },

      // menambah data | INSERT
      save() {
        axios.post('http://18.141.178.15:8080/checklist', this.form, this.authorization)
        .then(() => {
          this.form.name = ''
          alert('Data berhasil ditambahkan')
          this.getChecklist()
        })
        .catch(error => {
          alert(`Error ${error}`)
          console.log(error)
        })
      },

      // delete
      hapus(item) {
        axios.delete(`http://18.141.178.15:8080/checklist/${item.id}`, this.authorization)
        .then(() => {
          alert('Data berhasil di hapus')
          this.form.name = ''
          this.getChecklist()
        })
        .catch(error => {
          alert(`Error ${error}`)
          console.log(error)
        })
      },

    },
  }
</script>

<style>
#app {
  
}
</style>
