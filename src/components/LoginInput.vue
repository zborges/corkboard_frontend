<template>
  <div class="login">
    <form v-on:submit.prevent="submit()">
      <h1>Login</h1>
      <ul>
        <li class="text-danger" v-for="error in errors" v-bind:key="error">
          {{ error }}
        </li>
      </ul>
      <div class="form-group">
        <label>Username:</label>
        <input type="username" class="form-control" v-model="username" />
      </div>
      <div class="form-group">
        <label>Password:</label>
        <input type="password" class="form-control" v-model="password" />
      </div>
      <input type="submit" class="btn btn-primary" value="Submit" />
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      username: "",
      password: "",
      errors: [],
    };
  },
  methods: {
    submit: function () {
      var params = {
        username: this.username,
        password: this.password,
      };
      axios
        .post("/api/sessions", params)
        .then((response) => {
          axios.defaults.headers.common["Authorization"] =
            "Bearer " + response.data.jwt;
          localStorage.setItem("jwt", response.data.jwt);
          localStorage.setItem("user_id", response.data.user_id);
          // this.$router.push("/");
          this.$router.push("/connections");
        })
        .catch((error) => {
          console.log(error.response);
          this.errors = ["Invalid username or password."];
          this.username = "";
          this.password = "";
        });
    },
  },
};
</script>
