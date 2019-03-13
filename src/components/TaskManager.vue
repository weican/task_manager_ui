<template>
  <div class="hello">
    <h1>{{ msg }}</h1>


    <b-container fluid>
        <div>
          <div><h3>Please create a task and it will be updated to server.</h3></div> 
          <h2>{{status}}</h2>
          <b-input-group prepend="Task Name" class="mt-3" >
            <b-form-input v-model="input_name"/>
            <b-input-group-append>
              <b-button variant="info" v-on:click="createTask" :disabled="isDisable()" >Create</b-button>
            </b-input-group-append>
          </b-input-group>
        </div>
        <div>
          <p></p>
        </div>
        <b-list-group>

          <b-list-group-item class="d-flex align-items-center" v-for="task in taskList" :key="task.id">
     
            <div style="margin-left:4%"> {{task.task_name}} </div>
         
             <b-badge style="margin-left:25px" variant="primary" pill v-if="task.task_status">Complete</b-badge>
             <b-badge style="margin-left:15px" variant="primary" pill v-else v-on:click="updatedComplete(task.id)">Incomplete</b-badge> 
              <b-button style="margin-left:20px" variant="outline-primary" v-if="!task.task_status" v-on:click="updatedComplete(task.id)">Complete</b-button> 
             
             <b-button style="margin-left:auto" variant="outline-danger"  v-on:click="deleteTask(task.id)">Delete</b-button>

        </b-list-group-item>
     

      </b-list-group>
    </b-container>
  </div>
 
</template>

<script>
import axios from 'axios'
export default {
  name: 'HelloWorld',
  data() {
    return {
      status : "",
      serverLive: false,
      URL: "http://localhost:8080/",
      input_name:"",
      taskList: []
    }
  },
  props: {
    msg: String
  },
  mounted() {
    this.getAllTasks();
  },
  watch: {
    taskList: function(val) {
    }
  },
  methods: {
    isDisable() {
      if(this.serverLive && this.input_name != "") 
        return false;
      else return true;
    },
    updatedComplete(val) {
      axios.put(this.URL + 'updateTask?id=' + val) 
      .then(response => {
        if(response.status == 200 && response.data.Message == "Success") {
          this.getAllTasks();
        }
        else alert("The task can't be updated. status:" + response.status );
      })
    },
    deleteTask(val) {
      if(!confirm("Do you want to delete this task?")) return;

      axios.delete(this.URL + 'deleteTask?id=' + val) 
      .then(response => {
        if(response.status == 200 && response.data.Message == "Success") {
          this.getAllTasks();
        }
        else alert("The task can't be deleted. status:" + response.status );
      })
    },    
    createTask(val) {

      axios.post(this.URL + "insertTask", {
        "task_name": this.input_name,
        "task_status": 0,  //incomplete
        "task_description": "description "
      })
      .then(response => {
        if(response.status == 200 && response.data.Message == "Success") {
          this.input_name = "";
          this.getAllTasks();
        }
        else alert("The task can't be created. status:" + response.status );
      })
      .catch(function (error) {
        console.log(error);
      })
    },
    getAllTasks() {
      axios.get(this.URL + "getTasks")
      .then(response => {
          this.serverLive = true;
          this.taskList = response.data;
      })
      .catch(function (error) {
        alert("Network Error")
      })
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
