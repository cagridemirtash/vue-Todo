<template>
  <div class="container">
    <div class="row d-flex justify-content-center mt-2">
      <div class="col-md-6 col-sm-12">
        <div class="d-grid gap-2 input-group-lg">
        <input type="text" v-model = "name" class="form-control" placeholder="Todo Ekle..">
        </div>
      </div>
      <div class="col-md-6 col-sm-12">
        <div class="d-grid gap-2">
          <button @click="postTodo" class="btn btn-success btn-lg">Todo Gonder</button>
        </div>
      </div>
      
    </div>
    <hr>
    <div class="row">
      <h1>Toplam todo : {{todos.length}}</h1>
      <ul v-if="todos.length > 0">
        <li v-for="todo in todos" :key="todo.id" class="mt-2 border border-dark rounded-pill m-3 p-3 ">
          <div class="row">
             <div class="col-md-3 border border-info rounded-pill">
               <span class="fs-2">{{todo.name}}</span>
              </div>
              <div class="col-md-3">
                <div class="d-grid gap-2 input-group-lg">
                  <input type="text" v-model= "updateName" class="form-control" placeholder="Todo Duzenle..">
                </div>
              </div>
              <div class="col-md-3">
                <div class="d-grid gap-2">
                  <button @click="updateTodo(todo.id)" class="btn btn-primary btn-lg">Duzenle</button>
                </div>
              </div>
              <div class="col-md-3">
                <div class="d-grid gap-2">
                  <button @click="deleteTodo(todo.id)" class="btn btn-danger btn-lg">Sil</button>
                </div>
              </div>
          </div>
        </li>
      </ul>
      <p v-if="todos.length == 0">Elimizde hic Todo kalmamis.</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Todo',
  data() {
    return {
      token : null,
      name : '',
      updateName: '',
      todos : []
    }
  },
  methods : {
    async postTodo(){
      console.log(new Date())
      let dates= new Date()
      let month = dates.getUTCMonth() + 1
      let day = dates.getUTCDate()
      let year = dates.getUTCFullYear()
      let currentDate = year + "-" + month + "-" + day
      await axios({
        method: 'post',
        url: 'https://aodapi.eralpsoftware.net/todo',
        headers : {
          token: this.token
        },
        data: {
          name : this.name,
          date : currentDate
        },
      }).catch(err => console.log(err))
      .then( response =>
      console.log(response))
      
    },
    deleteTodo(todoID){
        axios({
          method : 'delete',
          url: `https://aodapi.eralpsoftware.net/todo/${todoID}`,
          headers : {
          token: this.token
          }
        }).then(res =>
          console.log(res))
    },
    updateTodo(todoID) {
      axios({
        method : 'put',
        url: `https://aodapi.eralpsoftware.net/todo/${todoID}`,
        headers : {
          token: this.token
        },
        data : {
          name : this.updateName
        }
      }).then(res => console.log(res))
    }
  },
  async created(){
      await axios({
        method : 'post',
        url : 'https://aodapi.eralpsoftware.net/login/apply',
        data:{
          username : "cagridemirtash@gmail.com",
          password : "123456"
        }
      })
      .then( response =>{
        this.token = response.data.token
      }
      ),
      axios({
        method : 'get',
        url : 'https://aodapi.eralpsoftware.net/todo',
        headers :{
          token: this.token
        }
      }
      ).then( response =>
        {console.log(response)
          response.data.body.map((item) => {
            this.todos.push(item) 
          })
          
        }
      ).catch(err => console.log(err))
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
li {
  list-style: none;
}
</style>
