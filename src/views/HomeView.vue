<template>
  <div class="bg-img">
    <img
      class="desk-img"
      src="@/assets/bg-desktop-dark.jpg"
      alt=""
      :style="[day ? { display: 'none' } : { display: 'block' }]"
    />
    <img class="mobile-img" src="@/assets/bg-mobile-dark.jpg" alt="" />
    <img
      src="@/assets/bg-desktop-light.jpg"
      alt=""
      class="desk-img-light"
      :style="[day ? { display: 'block' } : { display: 'none' }]"
    />
  </div>
  <div
    class="backdrop"
    :style="[
      day ? { 'background-color': '#FFFFFF' } : { 'background-color': 'black' },
    ]"
  >
    <p>Hello</p>
  </div>
  <div class="main">
    <div class="top">
      <h2>TODO</h2>
      <img
        src="@/assets/icon-sun.svg"
        alt=""
        class="icon-sun"
        :style="[day ? { display: 'none' } : { display: 'block' }]"
        @click="changeMode"
      />
      <img
        src="@/assets/icon-moon.svg"
        alt=""
        class="icon-moon"
        :style="[day ? { display: 'block' } : { display: 'none' }]"
        @click="changeMode"
      />
    </div>
    <div class="input">
      <form @submit.prevent="submit">
        <input
          type="text"
          placeholder="   Create a new todo..."
          v-model="task"
          :style="[day ? { background: '#FFFFFF' } : { background: '#25273d' }]"
        />
      </form>
    </div>
    <div
      class="tasks"
      :style="[day ? { background: '#FFFFFF' } : { background: '#25273d' }]"
    >
      <div class="task" v-for="task in filteredTasks">
        <img
          :src="[day && !task.completed ? dayImg : task.box]"
          alt=""
          @click="checked(task)"
        />
        <p
          :style="[
            task.completed
              ? { 'text-decoration': 'line-through' }
              : { 'text-decoration': 'none' },
            day ? { color: 'black' } : { color: '#c8cbe7' },
          ]"
        >
          {{ task.text }}
        </p>
      </div>
      <div class="bottom">
        <div class="bottom-left">
          <p>{{ activeTasks.length }} items left</p>
        </div>
        <div class="bottom-mid">
          <p
            @click="all"
            class="clear"
            :style="[
              state === 'all' ? { color: '#3A7CFD' } : { color: '#5b5e7e' },
            ]"
          >
            All
          </p>
          <p
            @click="active"
            class="clear"
            :style="[
              state === 'active' ? { color: '#3A7CFD' } : { color: '#5b5e7e' },
            ]"
          >
            Active
          </p>
          <p
            @click="completed"
            class="clear"
            :style="[
              state === 'completed'
                ? { color: '#3A7CFD' }
                : { color: '#5b5e7e' },
            ]"
          >
            Completed
          </p>
        </div>
        <div class="bottom-right">
          <p @click="clearCompleted">Clear Completed</p>
        </div>
      </div>
    </div>
    <div class="bottom-mobile">
      <p
        @click="all"
        class="show"
        :style="[state === 'all' ? { color: '#3A7CFD' } : { color: '#5b5e7e' }]"
      >
        All
      </p>
      <p
        @click="active"
        class="show"
        :style="[
          state === 'active' ? { color: '#3A7CFD' } : { color: '#5b5e7e' },
        ]"
      >
        Active
      </p>
      <p
        @click="completed"
        class="show"
        :style="[
          state === 'completed' ? { color: '#3A7CFD' } : { color: '#5b5e7e' },
        ]"
      >
        Completed
      </p>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: 'HomeView',
  components: {},
  data() {
    return {
      task: '',
      allTasks: [],
      activeTasks: [],
      completedTasks: [],
      state: 'all',
      day: false,
      dayImg: require('../assets/Oval-day.png'),
      dayChecked: require('../assets/Oval-checked-day.png'),
    };
  },
  methods: {
    active() {
      this.state = 'active';
    },

    all() {
      this.state = 'all';
    },

    completed() {
      this.state = 'completed';
    },

    changeMode() {
      this.day = !this.day;
    },

    submit() {
      let taskObj = {
        text: null,
        box: null,
        completed: false,
      };
      taskObj.text = this.task;
      taskObj.box = require('../assets/Oval.png');
      this.allTasks.push(taskObj);
      this.activeTasks.push(taskObj);

      this.task = '';
    },

    checked(obj) {
      obj.completed = !obj.completed;
      if (obj.completed) {
        if (!this.day) {
          obj.box = require('../assets/checked.png');
        } else {
          obj.box = this.dayChecked;
        }
        // obj.completed = true;
        this.completedTasks.push(obj);
        let index = this.activeTasks.indexOf(obj);
        if (index > -1) {
          this.activeTasks.splice(index, 1);
        }
      } else {
        obj.box = require('../assets/Oval.png');
        let index = this.completedTasks.indexOf(obj);
        if (index > -1) {
          this.completedTasks.splice(index, 1);
        }
        this.activeTasks.push(obj);
      }
    },

    clearCompleted() {
      for (let i = 0; i < this.allTasks.length; i++) {
        if (this.allTasks[i].completed) {
          let index = i;
          if (index > -1) {
            this.allTasks.splice(index);
          }
        }
      }
      this.completedTasks.splice(0, this.completedTasks.length);
    },
  },

  computed: {
    filteredTasks() {
      if (this.state === 'all') {
        return this.allTasks;
      } else if (this.state === 'completed') {
        return this.completedTasks;
      } else {
        return this.activeTasks;
      }
    },
  },
};
</script>

<style scoped>
.bg-img img {
  width: 100%;
}

.desk-img-light {
  display: none;
}

.icon-moon {
  display: none;
}
.mobile-img {
  display: none;
}
.main {
  position: absolute;
  width: 34%;
  left: 30%;
  top: 10%;
}

.backdrop {
  position: fixed;
  top: 0;
  bottom: 0;
  height: 100%;
  background-color: black;
  width: 100%;
  margin: 0;
  z-index: -1;
}

.icon-sun,
.icon-moon {
  cursor: pointer;
}
.top {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
h2 {
  color: white;
  font-weight: 700;
  font-size: 40px;
  line-height: 40px;
  /* identical to box height */
  letter-spacing: 15px;
}

input {
  margin-top: 48px;
  height: 64px;
  width: 100%;
  background: #25273d;
  box-shadow: 0px 35px 50px -15px rgba(0, 0, 0, 0.5);
  border-radius: 5px;
  border: none;
  font-weight: 400;
  font-size: 18px;
  line-height: 18px;
  /* identical to box height */
  letter-spacing: -0.25px;
  font-family: 'Josefin Sans', sans-serif;
  color: #767992;
  padding-left: 30px;
}

.tasks {
  box-shadow: 0px 35px 50px -15px rgba(0, 0, 0, 0.5);
  border-radius: 5px;
  background: #25273d;
  margin-top: 24px;
}

.task {
  height: 64px;
  width: 100%;
  border-bottom: 1px solid rgb(87, 87, 87);
  font-weight: 400;
  font-size: 18px;
  line-height: 18px;
  /* identical to box height */
  letter-spacing: -0.25px;
  align-items: center;
  color: #c8cbe7;
  display: flex;
  gap: 24px;
}

.task img {
  margin-left: 24px;
}

.bottom {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
}

.bottom-mid {
  display: flex;
  gap: 18px;
}

.bottom-mid p {
  cursor: pointer;
}
.bottom p {
  font-style: normal;
  font-weight: 400;
  font-size: 14px;
  line-height: 14px;
  /* identical to box height */
  letter-spacing: -0.194444px;

  color: #5b5e7e;
}

.bottom-mobile {
  display: none;
}
.bottom-right p {
  cursor: pointer;
}

@media only screen and (max-width: 420px) {
  .main {
    width: 90%;
    left: 5%;
  }

  .mobile-img {
    display: block;
  }

  .desk-img {
    display: none;
  }

  .clear {
    display: none;
  }

  .bottom-mobile {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 20px;
    margin-top: 16px;
    font-style: normal;
    font-weight: 400;
    font-size: 14px;
    line-height: 14px;
    /* identical to box height */
    letter-spacing: -0.194444px;
    box-shadow: 0px 35px 50px -15px rgba(0, 0, 0, 0.5);
    border-radius: 5px;
    background: #25273d;
    color: #5b5e7e;
  }

  .bottom-mobile p {
    cursor: pointer;
  }
}
</style>
