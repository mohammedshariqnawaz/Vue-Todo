<template>
  <!-- <div>
    <div class="textarea-container">
      <textarea
        style="resize: none;"
        v-model="newTodo.title"
        placeholder="Add Task"
      ></textarea>
      <button class="btn" @click="addTodo">
        <b-icon font-scale="2" icon="plus-circle-fill"></b-icon>
      </button>
    </div>

    <b-list-group class="mt-4">
      <b-list-group-item v-for="task in todolist" :key="task.id" variant="light"
        >{{ task.title }}
      </b-list-group-item>
    </b-list-group>
  </div> -->
  <div class="container">
    <div class="jumbotron mt-5 mx-auto">
      <div class="textarea-container">
        <textarea
          style="resize: none;"
          v-model="newTodo.title"
          placeholder="Add Task"
          onfocus="this.placeholder=''"
          onblur="this.placeholder='Add Task'"
        ></textarea>
        <button class="btn" @click="addTodo">
          <b-icon icon="plus-circle-fill"></b-icon>
        </button>
      </div>
      <div class="listarea">
        <b-list-group class="mt-4">
          <b-list-group-item
            v-for="task in todolist"
            :key="task.id"
            style="font-size:20px; font-weight:500"
          >
            <input
              type="checkbox"
              :checked="task.completed"
              @click="finishTodo(task)"
            /><span
              style="margin-left:10px;"
              :style="[
                task.completed
                  ? { 'text-decoration': 'line-through' }
                  : { 'text-decoration': 'none' },
              ]"
            >
              {{ task.title }}</span
            ><button
              class="btn text-right"
              style="float: right; color:red;"
              @click="delTodo(task)"
            >
              <b-icon icon="trash-fill"></b-icon>
            </button>
          </b-list-group-item>
        </b-list-group>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      todolist:[],
      newTodo: {
        id: "",
        title: "",
        completed: false,
      },
    };
  },
  methods: {
    addTodo() {
      console.log(this.todolist)
      if (this.todolist.length > 1) {
        this.newTodo.id = this.todolist[this.todolist.length - 1].id + 1;
        this.todolist.push(this.newTodo);
        this.newTodo = { id: "", title: "", completed: false };
      } else {
        this.newTodo.id = "1";
        this.todolist.push(this.newTodo);
        this.newTodo = { id: "", title: "", completed: false };
      }
    },
    delTodo(task) {
      const index = this.todolist.indexOf(task);
      if (index > -1) {
        this.todolist.splice(index, 1);
      }
    },
    finishTodo(task) {
      task.completed = !task.completed;
    },
  },
  created() {
    // axios.get('https://jsonplaceholder.typicode.com/todos')
    //   .then(function(response){
    //     console.log("todolist",this.todolist)
    //     // this.todolist = response.data
    //     console.log(response.data)
    //   })
    //   .catch(function(error){
    //     console.log("Promise Error",error)
    //   })
    console.log(this.todolist)
    axios.get('https://jsonplaceholder.typicode.com/todos?_limit=8')
      .then(response => this.todolist = response.data)
      .catch(error => console.log("Promise Error", error))
  },
};
</script>

<style scoped>
.jumbotron {
  width: 500px;
  min-height: 400px;
}
.textarea-container {
  position: relative;
  border-radius: 15px;
  margin-top: -10px;
}
.textarea-container textarea {
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  vertical-align: middle;
}
.textarea-container button {
  position: absolute;
  top: 0;
  right: 0;
  margin-top: 5px;
  vertical-align: bottom;
  display: inline-block;
}

textarea {
  border-radius: 8px;
  white-space: normal;
  text-align: center;
  text-align-last: center;
  font-size: 18px;
  /* display: block; */
}
</style>
