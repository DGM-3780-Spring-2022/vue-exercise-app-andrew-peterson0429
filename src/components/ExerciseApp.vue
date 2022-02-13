<template>
  <div class="container">
    <h2 class="text-center mt-5 pb-2 vueBackground">Vue Exercise Tracking App</h2>
    
   <!--  Input -->
   <div class="mt-5">
      <input v-model="date" type="date" class="rounded-2">
    </div>
   
   <div class="d-flex mt-2">
      <input v-model="task" type="text" placeholder="Enter Exercise" class="form-control">
      <input v-model="rep" type="text" placeholder="Enter Reps" class="form-control">
      <input v-model="distance" type="text" placeholder="Enter Distance mi" class="form-control">
      <input v-model="time" type="text" placeholder="Enter Time" class="form-control">
      <button @click="submitTask()" class="btn btn-success rounded-2 vueBackground">Submit</button>
   </div>

      <!-- Table -->
        <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Workout</th>
          <th scope="col">Reps</th>
          <th scope="col">Distance</th>
          <th scope="col">Time</th>
          <th scope="col">Date</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center"></th>
          <th scope="col" class="text-center"></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <th>
            <span :class="{'finished': task.status === 'Finished'}">{{task.name}}</span>
            </th>
            <td>
              {{task.rep}}
            </td>
            <td>
              {{task.distance}} mi
            </td>
            <td>
              {{task.time}}
            </td>
            <td>
              {{task.date}}
            </td>
          <td style="width: 120px">
            <span @click="changeStatus(index)" class="pointer" 
            :class="{'text-danger': task.status === 'To-do',
            'text-warning': task.status === 'In-progress',
            'text-success': task.status === 'Finished'}">
            {{task.status}}
            </span>
            </td>
          <td>
            <div class="text-center pointer" @click="editTask(index)">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div class="text-center pointer" @click="deleteTask(index)">
              <span class="fa fa-trash"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
   
  <h3 class="mt-5">Workout Stats</h3>

    <table class="table mt-2">
  <thead>
    <tr>
      <th scope="col">Total Workouts</th>
      <th scope="col">Total Reps</th>
      <th scope="col">Total Distance</th>
      <th scope="col">Total Time</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">{{totalWorkouts}}</th>
      <td>{{totalReps}}</td>
      <td>{{totalDistance}} mi</td>
      <td>{{totalTime}}</td>
    </tr>
    
  </tbody>
</table>

  <footer class="vueBackgroundDark footer">
    <div>
      <p>Vue Exercise Tracking App for DGM 3780 by Andrew Peterson</p>
    </div>
  </footer>

  </div>
</template>

<script>
export default {
  data() {
    return {
      task: '',
      editedTask: null,
      rep: '',
      editedRep: null,
      distance: '',
      editedDistance: null,
      time: '',
      editedTime: null,
      date: '',
      availableStatuses: ['To-do', 'In-progress', 'Finished'],

      tasks: [
        /* {
          name: 'Push ups',
          status: 'To-do',
          rep: 1,
          distance: 2,
          time: 3
        },
        {
          name: 'Jog',
          status: 'In-progress',
          rep: 4,
          distance: 5,
          time: 6
        }, */
        
      ],

      totalWorkouts: 0,
      totalReps: 0,
      totalDistance: 0,
      totalTime: 0,
    }
  },

  methods: {
    submitTask() {
      if(this.task === 0) return;

      if(this.editedTask === null) {
          this.tasks.push({
          name: this.task,
          rep: this.rep,
          time: this.time,
          distance: this.distance,
          date: this.date,
          status: 'To-do'
        });
      } else {
        this.tasks[this.editedTask].name = this.task;
        this.tasks[this.editedTask].rep = this.rep;
        this.tasks[this.editedTask].distance = this.distance;
        this.tasks[this.editedTask].time = this.time;
        this.tasks[this.editedTask].date = this.date;
        this.editedTask = null;
      }

      this.task = '';
      this.rep = '';
      this.distance = '';
      this.time = '';
      this.date = '';
      this.totals();
    },

    deleteTask(index) {
      this.tasks.splice(index, 1)
    },

     editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
      this.rep = this.tasks[index].rep;
      this.distance = this.tasks[index].distance;
      this.time = this.tasks[index].time;
      
      
     },

     changeStatus(index) {
       let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
       if (++ newIndex > 2) newIndex = 0;
       this.tasks[index].status = this.availableStatuses[newIndex];
     },
    
    totals() {
      this.totalWorkouts = 0;
      this.totalReps = 0;
      this.totalDistance = 0;
      this.totalTime = 0;
    
      for (let i = 0; i < this.tasks.length; i++) {
        this.totalWorkouts += 1;
        this.totalReps += parseInt(this.tasks[i].rep);
        this.totalDistance += parseInt(this.tasks[i].distance);
        this.totalTime += parseInt(this.tasks[i].time);
        console.log(this.tasks);
      }
      
    }

  },

};

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .pointer {
    cursor: pointer;
  }

  .finished {
    text-decoration: line-through;
  }

 .vueDarkColor {
   color: #35495e;
 }
  
.vueBackground {
  background-color: #42b883;
}

.vueBackgroundDark {
  background-color: #35495e;
}

footer {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 5rem;
  height: 4rem;
  width: auto;
  color: #fff;
  text-align: center;
}

</style>
