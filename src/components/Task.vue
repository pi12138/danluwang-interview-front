<template>
  <div class="task">
      <form @submit.prevent>
          <div class="row">
            <div class="form-group col-md-6 offset-md-3">
                <input type="text" class="form-control" v-on:keyup.enter="addTask" v-model="title">
            </div>
          </div>
      </form>
    
      <ul class="list-group col-md-6 offset-md-3">
        <li class="list-group-item" v-for="(item, index) in taskList" :key="index" >
          <div class="row">
            <div v-text="item.title" class="col-md-7"></div>
            <button class="col-md-2 btn btn-primary btn-sm">完成</button>
            <button class="col-md-2 offset-md-1 btn btn-secondary btn-sm" @click="deleteTask(index)">删除</button>
          </div>
        </li>
      </ul>
      
      <br>
      <div class="btn-group btn-group-lg" role="group" v-if="taskList.length != 0">
        <button type="button" class="btn btn-ligth">All</button>
        <button type="button" class="btn btn-light">Active</button>
        <button type="button" class="btn btn-light">Completed </button>
      </div>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'Task',
  components: {
    // HelloWorld
    
  },
  data(){
    return {
        taskList: [{'title': "xxx"}],
        baseUrl: 'http://127.0.0.1:8000/api/todo_mvc/task/',
        title: ""
    }
  },
  methods: {
      getTaskList(){
          let url = this.baseUrl

          this.$axios.get(url)
          .then((result) => {
              this.taskList = result.data
          }).catch((err) => {
              console.log(err.response)
          });
      },
      
      addTask(){
          let url = this.baseUrl
          let data = {
              title: this.title
          }

          this.$axios.post(url, data)
          .then((result) => {
              this.taskList.push(result.data)
          }).catch((err) => {
              console.log(err.response)
          });
      },

      // completeTask(itemId){
        // let url = this.baseUrl + `{itemId}/`
        
      // },

      deleteTask(index){
        let itemId = this.taskList[index].id
        let url = this.baseUrl + `${itemId}/`

        this.$axios.delete(url)
        .then((result) => {
          if (result.data.data === "ok"){
            this.taskList.splice(index, 1)
          }

        }).catch((err) => {
          console.log(err.response)
        });

      }
  },

  beforeMount(){
      this.getTaskList()
  }
}
</script>
