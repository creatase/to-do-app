<template>
  <div id="app">
    <h1>zkr.oom</h1>
    <h2>ツイキャス</h2>
    <ul>
      <li>動画1</li>
      <li>動画2</li>
      <li>動画3</li>
      <li>動画4</li>
    </ul>

    <h1>ToDoApp</h1>
    <input v-model="message" />
    <button v-on:click="add">追加</button>
    <br />
    <ul>
      <li v-for="todo in todos" :key="todo.id" class="task-card">
        {{ todo.text }}
      </li>
    </ul>
    {{ message }}
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data: () => ({
    todos: [{ id: 1, text: "vueのお勉強" }],
    message: "これはメッセージです！",
    isLoading: false,
    moveis: []
  }),
  created() {
    this.fetch();
  },
  methods: {
    add() {
      const newToDo = {
        id: this.todos.length + 1,
        text: this.message
      };
      this.todos.push(newToDo);
      this.message = "";
    },
    async fetch() {
      this.isLoading = true;
      const URL = "https://apiv2.twitcasting.tv/users/zakuro_96/moveis";
      const ClientId =
        "1127906652315734016.04edec17a6138f4d31d8ffb567454548d058276234ce84d5bcc571c66d6ed700";
      const ClientSecret =
        "7273e4d7469182f7db6883eb53d24365b2e3d7c75a2b55673588730c053c2e99";
      const Auth = "Basic " + btoa(ClientId + ":" + ClientSecret);
      const { data } = await axios.get(URL, {
        withCredentials: true,
        headers: {
          "Accept": "application/json",
          "X-Api-Version": "2.0",
          "Authorization": Auth
        },
        responseType: "json"
      });
      this.isLoading = false;
      this.moveis = data;
      console.log(this.moveis);
    }
  }
};
</script>

<style>
.task-card {
  border: solid 1px;
  margin: 5px;
  padding: 2px;
  width: 250px;
}
</style>
