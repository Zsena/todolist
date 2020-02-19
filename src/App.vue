<template>
  <div id="app">
    <section class="hero">
      <div class="hero-body">
        <div class="container">
          <h1 class="title">Amazing ToDo list</h1>
          <h2 class="subtitle">Amazing ToDo list</h2>

          <div class="card">
            <header class="card-header">
              <div class="field">
                <div class="control">
                  <input
                    class="input is-primary"
                    v-model="toDoInput"
                    type="text"
                    placeholder="What needs to be done?"
                    v-on:keyup.enter="submit"
                  />
                </div>
              </div>
              <a
                href="#"
                @click.prevent="allDone"
                class="card-header-icon"
                aria-label="more options"
              >
                <span class="icon">
                  <i class="fas fa-angle-down" aria-hidden="true"></i>
                </span>
              </a>
            </header>
            <div class="card-content">
              <div class="content">
                <div v-for="(toDo, index) in filterList(status)" :key="index">
                  <label v-bind:class="{'strike-through': toDo.completed, 'checkbox': true}">
                    <input type="checkbox" v-model="toDo.completed" />
                    {{toDo.text}}
                    <span @click.prevent="del(index)">[x]</span> 
                  </label>
                </div>
                <br />
                <small>by Zsena</small>
              </div>
            </div>
            <footer class="card-footer">
              <p class="card-footer-item">{{filterList('active').length}} items left</p>
              <a href="#" @click.prevent="status = 'all'" class="card-footer-item">all</a>
              <a href="#" @click.prevent="status = 'active'" class="card-footer-item">active</a>
              <a href="#" @click.prevent="status = 'completed'" class="card-footer-item">completed</a>
              <hr />
            </footer>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>


<script>
export default {
  name: "App",
  data: function() {
    return {
      toDoInput: "",
      toDoList: [
        {text: "elso", completed: false},
        {text: "masodik", completed: false},
        {text: "harmadik", completed: false},
        {text: "negyedik", completed: false},
        {text: "otodik", completed: false},
      ],
      status: "all",
      allDoneList: []
    };
  },
  components: {},

  methods: {
    submit() {
      this.toDoList.push({ text: this.toDoInput, completed: false });
      this.toDoInput = "";
    },

    filterList(status) {
      if (status == "all") {
        return this.toDoList;
      }

      if (status == "active") {
        return this.toDoList.filter(toDo => !toDo.completed);
      }

      if (status == "completed") {
        return this.toDoList.filter(toDo => toDo.completed);
      }
    },

    del(i) {
      this.toDoList.splice(i, 1)
    },

    allDone() {
      if (this.allDoneList.length > 0) {
        for (let i = 0; i < this.allDoneList.length; i++) {
          this.toDoList[this.allDoneList[i]].completed = false;
        }
        this.allDoneList = [];
      } else {
        for (let i = 0; i < this.toDoList.length; i++) {
          if (!this.toDoList[i].completed) {
            this.allDoneList.push(i);
            this.toDoList[i].completed = true;
          }
        }
      }
    }
  }
};
</script>

<style>
.strike-through {
  text-decoration: line-through;
}
</style>
