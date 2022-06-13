<template>
  <div>
    <div class="item__title">
      <h1>{{ todo.text }} </h1>
    </div>
    <div class="item__content">
      <button @click="modalWindow" class="button_description">Подробнее</button>
      <transition name="fade">
        <div class="modal" v-if="showModal"
             @click="modalWindow">
          <div class="modal__content"
               v-on:click="e => e.stopPropagation()">
            <h2> TEXT: {{ todo.text }}</h2>
            <h2>ID: {{ todo.id }}</h2>
            <h2>ACTIVE: {{ todo.active }}</h2>
          </div>
        </div>
      </transition>
     <div class="item__buttons">
       <transition name="fade">
         <button v-if="todo.active" v-on:click="completed(todo)">завершить</button>
       </transition>
       <button v-on:click="remove(todo)">удалить</button>
     </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "TaskItem",
  props: ['todo','remove','completed'],
  data() {
    return {
      showModal: false
    }
  },
  methods: {
      modalWindow() {
      this.showModal = !this.showModal
    }
  }
}
</script>

<style scoped>

</style>
