<template>
  <div class="todo-list">
    <header class="head">Tasks</header>

    <input class="todotask" type="text" @change="addlist()" v-model="text"/>
    <button @click="addlist()" class="add">+Add <i class="fa fa-plus-circle"></i> </button>
    <button class="undo" @click="undo">Undo</button>
    <button  @click="clearall" class="clearall">clear all</button>
    <button class="clearcompleted" @click="clearcompleted()">clear completed</button>
    <ul>
      <li v-for="(item, index) in list" :key="index">
        <div v-if="!item.editing">
          <span @click="enableEditing(index)"
          :class="{ completed: item.completed }">{{ item.label}}</span>
        </div>
        <div v-if="item.editing">
          <input type="text" v-model="tempValue" class="input"/>
          <button @click="disableEditing(index)"> Cancel </button>
          <button @click="saveEdit(index)"> Save </button>
        </div>
        <button @click="check(index)" class="done">Done</button>
        <button @click="deletelist(index)" class="delete">Delete</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      list: [],
      text: '',
      completed: Boolean,
      tempValue: '',
      editing: false,
    };
  },
  mounted() {
  document.title = "page title";
  },
  created() {
    this.list = JSON.parse(localStorage.getItem('list')) || [];
  },
  computed: {
    incomplete() {
      return this.list.filter(this.inProgress).length;
    },
  },
  methods: {
    addlist() {
      if (!this.text.trim()) return;
      if (this.list.length >= 20) return;
      this.list.unshift({ label: this.text, completed: false });
      this.memory();
      this.text = '';
    },
    deletelist(index) {
      this.list.splice(index, 1);
      this.memory();
    },
    memory() {
      localStorage.setItem('list', JSON.stringify(this.list));
    },
    check(index) {
      this.list[index].completed = !this.list[index].completed;
      this.memory();
    },
    clearall() {
      this.list = [];
    },
    clearcompleted() {
      this.list = this.list.filter(this.inprogress);
    },
    inprogress(item) {
      return !this.iscompleted(item);
    },
    iscompleted(item) {
      return item.completed;
    },
    enableEditing(index) {
      this.tempValue = this.list[index].label;
      this.list[index].editing = true;
    },
    disableEditing(index) {
      this.tempValue = '';
      this.list[index].editing = false;
    },
    saveEdit(index) {
      this.list[index].label = this.tempValue;
      this.disableEditing(index);
    },
    undo() {
      this.list.pop();
    },

  },

};

</script>

<style>

.todo-list {
  max-width: 800px;
  margin: auto;
}

header {
  font-family: 'Nunito', sans-serif;
  text-align: left;
  font-size: xx-large;
}

.todotask {
  width: 70%;
  height: 50px;
  font-family: 'Nunito', sans-serif;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  justify-content: space-between;
  padding-bottom: 20px;
}

.completed {
  text-decoration: line-through;
}
.add{
  height: 56px;
  width: 69px;
  font-family: 'Nunito', sans-serif;
  background-color: rgb(99, 167, 227);
  border-color: rgb(99, 167, 227);
}
.completed{
  text-decoration: line-through;
}
.undo{
  height: 56px;
  width: 69px;
  font-family: 'Nunito', sans-serif;
  background-color: rgb(55, 17, 222);
  border-color: rgb(17, 35, 113);
}
.clearcompleted{
  height: 56px;
  width: 150px;
  font-family: 'Nunito', sans-serif;
  background-color: rgb(234, 146, 73);
  border-color: rgb(245, 167, 102);
}
.clearall{
  height: 56px;
  width: 150px;
  font-family: 'Nunito', sans-serif;
  background-color: rgb(236, 226, 114);
  border-color: rgb(240, 240, 142);

}
.edit{
  margin-left: 300px;
  height: 56px;
  width: 69px;
  font-family: 'Nunito', sans-serif;
  background-color: rgb(86, 228, 238);
  border-color: rgb(134, 199, 218);
}
.delete{
  height: 56px;
  width: 69px;
  font-family: 'Nunito', sans-serif;
  background-color: rgb(247, 3, 3);
  border-color: rgb(242, 21, 21);
}
.done{
  margin-left: 400px;
  height: 56px;
  width: 69px;
  font-family: 'Nunito', sans-serif;
  background-color: rgb(95, 235, 179);
  border-color: rgb(133, 227, 213);
  margin-left: 500px;
}
</style>
