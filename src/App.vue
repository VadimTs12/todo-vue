<template>
  <div id="app">
    <div class="wrapper">
      <div class="form__create">
        <input class="input" v-model="value" id="newToDo" placeholder="Введите задачу"/>
        <button class="button_submit" v-on:click="todo()">Добавить задачу</button>
      </div>
      <div class="form__create">
        <input class="input" v-model="valueSearch" id="search" placeholder="Найти задачу"/>
      </div>
      <div class="content">
        <div class="header">
          <span v-on:click="setFilter('all')"
                :class="(innerData.activeFilter === 'all') ? 'activeHeader'  :  'headerBlock'"
          >Все</span>
          <span v-on:click="setFilter('active')"
                :class="(innerData.activeFilter === 'active') ? 'activeHeader' :  'headerBlock'"
          >Активные</span>
          <span v-on:click="setFilter('completed')"
                :class="(innerData.activeFilter === 'completed') ? 'activeHeader' :  'headerBlock'"
          >Завершенные</span>
        </div>
        <!-- Блок будет отображаться если нет никаких данных -->
        <div v-if="this.innerData.zadachi.length==0" style="text-align: center; margin-top: 20px">
          Список пуст
        </div>
        <div else>
          <div class="block__list">
            <TaskItem
                v-for="todo in listTodo" :key="todo.id"
                :remove="remove"
                :completed="completed"
                :todo="todo"
                class="block__item"
                :class="{ activeToDo: todo.active}"
            />

          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import TaskItem from "@/components/ToDoItem";

export default {
  name: 'App',
  components: {TaskItem},
  data() {
    return {
      innerData: {
        zadachi: [],
        filtered: [],
        activeFilter: "all"
      },
      value: "",
      valueSearch: "",
    };
  },
  //получаю данные из удалённого хранилища
  async created() {
    const res = await fetch(
        "https://my-json-server.typicode.com/falk20/demo/todos"
    );
    this.innerData.zadachi = await res.json();

  },

  methods: {
    todo() {
      //создаю объект с новой задачей
      if (this.value !== "") {
        const newTodo = {
          id: Date.now(),
          active: true,
          text: this.value
        };
        // добавляю объект в список задач
        this.innerData.zadachi = [newTodo, ...this.innerData.zadachi];
        //очищаю поле инпут
        this.value = "";
      }
    },
    remove(item) {
      this.innerData.zadachi.splice(this.innerData.zadachi.indexOf(item), 1);
    },

    completed(item) {
      item.active = false;
    },

    setFilter(filter) {
      this.innerData.activeFilter = filter
    },

  },
  computed: {
    listTodo() {
      let regexp = new RegExp(this.valueSearch, 'i');

      return this.innerData.zadachi.filter((elem) => {
        if (this.innerData.activeFilter === "all" && regexp.test(elem.text)) {
          return elem;
        }
        if (this.innerData.activeFilter === "active" && regexp.test(elem.text)) {
          return elem.active === true;
        }
        if (this.innerData.activeFilter === "completed" && regexp.test(elem.text)) {
          return elem.active === false;
        }
      });
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
