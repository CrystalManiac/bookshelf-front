<template>
  <div class="container">
    <header class="jumbotron">
      <h4>
        <strong>Список прочитанного</strong>
      </h4>
    </header>
    <table v-if="!errored" class="table">
      <thead>
        <tr>
          <th scope="col">ID</th>
          <th scope="col">Произведение</th>
          <th scope="col">Автор</th>
          <th scope="col">Страниц прочитано</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="book in content" :key="book.id">
          <th scope="row">{{book.id}}</th>
          <td>{{book.name}}</td>
          <td>{{book.author}}</td>
          <td>{{book.page}}</td>
        </tr>
      </tbody>
    </table>
    <div v-else>
      <p>Server error</p>
    </div>
  </div>
</template>

<script>
import UserService from "../services/user.service";

export default {
  name: "Data",
  data() {
    return {
      content: null,
      errored: false
    };
  },
  computed: {
    currentUser() {
      return this.$store.state.auth.user;
    },
  },
  mounted() {
    if (!this.currentUser) {
      this.$router.push("/login");
    }
    UserService.getBooks().then(
      (response) => {
        this.content = response.data.bookList;
      },
      () => {
        this.errored = true;
      }
    );
  },
};
</script>