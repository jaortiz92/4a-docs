<template>
  <div class="navbar">
    <img
      v-if="$route.name != 'home'"
      class="logo-navbar"
      src="../assets/images/logo_transparent.png"
      @click="loadHome"
    />
    <ul>
      <li><a v-if="is_admin" @click="loadAdmin">Administración</a></li>
      <li><a @click="loadHome">Inicio</a></li>
      <li><a @click="loadRentals">Mis reservas</a></li>
      <li><a @click="logout">Cerrar Sesión</a></li>
    </ul>
  </div>
</template>

<script>
import Swal from "sweetalert2";

export default {
  name: "Navbar",
  data: function () {
    return {
      is_admin: false,
    };
  },
  created() {
    if (localStorage.getItem("is_superuser") === "true") this.is_admin = true;
  },
  methods: {
    logout: async function () {
      localStorage.clear();
      await Swal.fire({
        icon: "info",
        title: "Hasta Pronto",
        text: "Sesión Cerrada",
        confirmButtonColor: "#141e28",
      });
      this.$router.push({ name: "login" });
    },
    loadHome: function () {
      this.$router.push({ name: "home" });
    },
    loadAdmin: function () {
      this.$router.push({ name: "carIndex" });
      this.$emit("initialAdmin");
    },
    loadRentals: function () {
      this.$router.push({ name: "rentals" });
    },
  },
};
</script>

<style scoped>
.navbar {
  width: 100%;
  height: 60px;
  position: absolute;
  top: 0;
  background-color: var(--dark-blue-color);
  text-align: center;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.navbar ul {
  display: flex;
  align-items: center;
  justify-content: right;
  height: 100%;
  margin: 0 5% 0 40%;
  width: 60%;
}

.navbar ul li {
  list-style: none;
  margin-left: 3%;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 50%;
}

.navbar ul li a {
  font-size: 35%;
  text-decoration: none;
  color: var(--white-color);
}

.navbar ul li:nth-child(-n + 4) {
  padding-right: 3%;
  border-right: 2px solid var(--light-blue-color);
}

.navbar ul li a:hover {
  color: var(--pink-color);
  cursor: pointer;
}
.logo-navbar {
  margin-top: 0.5%;
  width: 7%;
  margin-left: 4%;
  object-fit: cover;
}
</style>

