<template>
  <div class="body-main">
    <router-view
      v-on:completedLogIn="completedLogIn"
      v-on:completedSignUp="completedSignUp"
      v-on:loadLogIn="loadLogIn"
    ></router-view>
  </div>
</template>

<script>
import Swal from "sweetalert2";
import gql from "graphql-tag";

export default {
  name: "App",

  data: function () {
    return {
      is_auth: false,
      userDetailById: {
        is_superuser: "",
        first_name: "",
      },
    };
  },
  computed: {
    is_auth: {
      get: function () {
        return this.$route.meta.requiresAuth;
      },
      set: function () {},
    },
  },
  methods: {
    completedLogIn: async function (data) {
      localStorage.setItem("isAuth", true);
      localStorage.setItem("userId", data.userId);
      localStorage.setItem("token_access", data.token_access);
      localStorage.setItem("token_refresh", data.token_refresh);

      this.$apollo.queries.userDetailById.skip = false;
      const result = await this.$apollo.queries.userDetailById.refetch();
      localStorage.setItem(
        "is_superuser",
        result.data.userDetailById.is_superuser
      );
      localStorage.setItem("first_name", result.data.userDetailById.first_name);

      Swal.fire({
        icon: "success",
        title: "Bienvenido " + localStorage.getItem("first_name"),
        text: "Autenticación Exitosas",
        confirmButtonColor: "#141e28",
      });
      this.loadHome();
    },
    loadHome: function () {
      this.$router.push({ name: "home" });
    },
    completedSignUp: async function (data) {
      await Swal.fire({
        icon: "success",
        title: "Felicidades",
        text: "Registro Exitoso",
        confirmButtonColor: "#141e28",
      });
      this.completedLogIn(data);
    },
    verifyAuth: function () {
      this.is_auth = localStorage.getItem("isAuth") || false;
      if (this.is_auth == false) this.$router.push({ name: "login" });
      else this.$router.push({ name: "home" });
    },
    loadLogIn: function () {
      this.$router.push({ name: "login" });
    },
  },
  apollo: {
    userDetailById: {
      query: gql`
        query ($userId: Int!) {
          userDetailById(userId: $userId) {
            is_superuser
            first_name
          }
        }
      `,
      variables() {
        return {
          userId: parseInt(localStorage.getItem("userId"), 10),
        };
      },
      skip() {
        return this.skipQuery;
      },
    },
  },
};
</script>

<style src="@/assets/css/normalize.css">
</style>

<style>
:root {
  --pink-color: #d1345b;
  --dark-blue-color: #141e28;
  --light-blue-color: #7c8f99;
  --white-color: #f3f8fa;
}

html {
  overflow-y: scroll;
  scroll-behavior: smooth;
  font-family: "Montserrat";
  font-size: 50px;
}

body {
  background-color: var(--white-color);
}

.swal2-popup {
  font-size: 40% !important;
}
</style>
