<template>
  <div class="container">

    <h2 class="text-center mt-5">Vue Todo list</h2>

    <!-- Input  -->
    <div class="d-flex text-center mt-5">
      <input v-model="task" type="text" placeholder="Enter task" class="w-100 form-control">
      <button @click="submitTask" class="btn btn-warning rounded-0">SUBMIT</button>
    </div>

    <!-- Task table  -->
    <table class="table table-bordered mt-5">
  <thead>
    <tr>
      <th style="width: 90vh" scope="col">Task</th>
      <th scope="col">Status</th>
      <th scope="col" class="tex-center">#</th>
      <th scope="col" class="tex-center">#</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(task, index) in tasks" :key="index">
      <td>
        <span :class="{'finished': task.status === 'finished'}">
          {{task.name}}
        </span>
      </td>
      <td style="width: 120px">
          <span @click="changeStatus(index)" class="pointer"
            :class="{'text-danger': task.status === 'to-do',
            'text-warning': task.status === 'in-progress',
            'text-success': task.status === 'finished'
            }"
          >
            {{firstCharUpper(task.status)}}
          </span>
        </td>
      <td style="width: 80px">
        <div class="text-center" @click="editTask(index)">
          <span class="fa fa-pen"></span>
        </div>
      </td>
      <td style="width: 80px">
        <div class="text-center" @click="deleteTask(index)">
          <span class="fa fa-trash"></span>
        </div>
      </td>
    </tr>
  </tbody>
</table>



  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },

  data(){
    return {
      task: '',
      editedTask: null,
      availableStatuses: ['to-do', 'in-progress', 'finished'],
      tasks: [
        {
          name: 'Steal abbabababa from the store',
          status: 'to-do'
        },
        {
          name: 'eat 1 kg abbabababa',
          status: 'in-progress'
        }
      ]
    }
  },

  methods: {
    submitTask() {
      if(this.task.length === 0) return;

      if(this.editedTask === null)  {
        this.tasks.push({
          name: this.task,
          status: 'to-do'
        });
      } else {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }
      
      this.task = '';
    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },

    changeStatus(index) {
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
    },

    firstCharUpper(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

.pointer {
  cursor: pointer;
}

.finished {
  text-decoration: line-through;
}



</style>
