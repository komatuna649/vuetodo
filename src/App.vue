<template>
  <div id="app">
    <div class="main">
      <h1>ToDoリスト</h1>

      <ul class="work-title">
        <li v-for="work in works" :key="work.id">
          <input
            type="radio"
            name="show"
            :id="work.id"
            :value="work.value"
            v-model="show"
          />
          <label :for="work.id">{{ work.text }}</label>
        </li>
      </ul>
      <table>
        <tr>
          <th>ID</th>
          <th>コメント</th>
          <th>状態</th>
        </tr>
        <tr v-for="(todo, index) in computedTodos" v-bind:key="index">
          <td>{{ todo.id }}</td>
          <td>{{ todo.comment }}</td>
          <td v-if="todo.state">
            <button @click="changeWorkState(todo.id)">作業中</button>
          </td>
          <td v-else>
            <button @click="changeWorkState(todo.id)">完了</button>
          </td>
          <td><button @click="deleteTodo(todo.id)">削除</button></td>
        </tr>
      </table>
      <h2>新規タスクの追加</h2>
      <form @submit.prevent>
        <input type="text" v-model="comment" />
        <input type="submit" value="送信" @click="addTask" />
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      works: [
        { id: 'all', value: 1, text: 'すべて' },
        { id: 'doing', value: 2, text: '作業中' },
        { id: 'done', value: 3, text: '完了' },
      ],
      todos: [],
      comment: '',
      show: 1,
    }
  },
  methods: {
    addTask: function () {
      const todo = {
        comment: this.comment,
        state: true,
        id: this.todos.length,
      }
      this.todos.push(todo)
      this.comment = ''
    },
    deleteTodo(todoId) {
      this.todos.splice(todoId, 1)
      this.todos.forEach((todo, index) => {
        todo.id = index
      })
    },
    // todosの状態変更
    changeWorkState(todoId) {
      this.todos[todoId].state = !this.todos[todoId].state
    },
  },
  computed: {
    //todosの表示、非表示の切り替え
    computedTodos() {
      switch (this.show) {
        case 1:
          return this.todos
        case 2:
          return this.todos.filter((e) => e.state)
        case 3:
          return this.todos.filter((e) => !e.state)
        default:
          return []
      }
    },
  },
}
</script>

<style>
.work-title {
  display: flex;
  list-style: none;
}
.main {
  width: 80%;
  margin: auto;
  padding: 0 300px;
}
th,
td,
button {
  width: 80px;
}
th,
td {
  text-align: center;
}
h1,
h2,
form {
  padding-left: 15px;
}
</style>
