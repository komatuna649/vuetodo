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
        <tr v-for="(list, index) in showTasks" v-bind:key="index">
          <td>{{ list.id }}</td>
          <td>{{ list.comment }}</td>
          <td v-if="list.state">
            <button @click="changeWorkState(list.id)">作業中</button>
          </td>
          <td v-else>
            <button @click="changeWorkState(list.id)">完了</button>
          </td>
          <td><button>削除</button></td>
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
        { id: 'all', value: 0, text: 'すべて' },
        { id: 'doing', value: 1, text: '作業中' },
        { id: 'done', value: 2, text: '完了' },
      ],
      lists: [],
      comment: '',
      show: 0,
    }
  },
  methods: {
    addTask: function () {
      const todo = {
        comment: this.comment,
        state: true,
        id: this.lists.length,
      }
      this.lists.push(todo)
      this.comment = ''
    },
  },
  computed: {
    showTasks() {
      switch (this.show) {
        case 0:
          return this.lists
        case 1:
          return this.lists.filter((e) => e.state)
        case 2:
          return this.lists.filter((e) => !e.state)
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
